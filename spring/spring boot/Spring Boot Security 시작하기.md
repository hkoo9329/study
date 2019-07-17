# 출처: [Spring Boot Security 시작하기](https://cusonar.tistory.com/8)

~~(혹시 해당 내용이 없어질까봐 백업용)~~

# 목차

1. [Spring Boot Security 시작하기](#Spring-Boot-Security-시작하기)
2. [Custom Login](#Custom-Login)

# Spring Boot Security 시작하기

Spring Security는 보안에서 중요한 요소인 인증과 권한 처리를 쉽게 하도록 도와준다. 인증은 일반적으로 로그인을 말하고, 권한은 admin과 일반 user 사이에 권한을 달리하여 특정 페이지는 일반 user에게 접속하지 못하도록 막는것을 말한다.



1. build.gradle 파일의 dependencies에 아래와 같은 dependency를 추가하고 dependency refresh를 통해 security lib를 내려받습니다.

```groovy
compile('org.springframework.boot:spring-boot-starter-security')
```

2. 서버를 실행 후 http://localhost:8080으로 접속해보면, 평소와 다르게 인증창이 나타나는 것을 볼 수 있다. default ID는 user이며, Password는 콘솔창에서 확인할 수 있다.

   인증을 거치게 되면 해당 페이지로 이동할 수 있고, 아니면 401(권한 없음) 에러가 발생한다. 이것만 을 가지고 실제 시스템에 사용하기에는 무리가 있어서 Customizing이 필요하다.

3. 우선 User를 정의해야 한다.보통은 DB 조회를 통해서 가져오는데 이 User가 시스템마다 다르기 때문에 시스템에 맞게 custonmizing 해야 한다. DB에 user와 authority 테이블을 만들고, domain 패키지를 만들고 아래에 User 클래슬를 생성한다

   기본 적으로 Spring Security를 사용하기 위해서는 UserDetails 인터페이스에 정의된 메소드를 구현해 줘야 한다.

   ```sql
   create table user (
        username varchar(20),
        password varchar(500),
        name varchar(20),
        isAccountNonExpired boolean,
        isAccountNonLocked boolean,
        isCredentialsNonExpired boolean,
        isEnabled boolean
   );
   
   ```

   - 기본적으로 제공하는 기능에 충실하게 만들되 추가 필드를 참고하기 위해 name을 추가했다.
   - 만약 사용하지 않을 기능이라면 필드를 만들지 않아도 된다.
   - 각 필드는 영어를 그대로 해석하면 된다. 
     - 계정이 만료되었는지, 
     - 계정이 잠겼는지(패스워드를 틀려서), 
     - 패스워드가 만료되었는지(몇 개월 주기로 변경), 
     - 계정 활성화 여부이다.



```sql
create table authority (
    username varchar(20),
    authority_name varchar(20)
);
```

- 권한은 한 사람이 여러개 권한을 가질 수 있으므로 별도 테이블로 만들었다. username을 통해 user 테이블과 조인할 수 있다.
- authority_name의 경우 number 타입으로 해서 enum으로 처리할 수도 있다. 직관적으로 하기 위해 varchar로 했다.



```java

package com.cusonar.example.user.domain;

import java.util.Collection;

import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;

public class User implements UserDetails {
     private String username;
     private String password;
     private String name;
     private boolean isAccountNonExpired;
     private boolean isAccountNonLocked;
     private boolean isCredentialsNonExpired;
     private boolean isEnabled;
     private Collection<? extends GrantedAuthority> authorities;
    
     @Override
     public Collection<? extends GrantedAuthority> getAuthorities() {
          return authorities;
     }

     @Override
     public String getPassword() {
          return password;
     }

     @Override
     public String getUsername() {
          return username;
     }

     @Override
     public boolean isAccountNonExpired() {
          return isAccountNonExpired;
     }

     @Override
     public boolean isAccountNonLocked() {
          return isAccountNonLocked;
     }

     @Override
     public boolean isCredentialsNonExpired() {
          return isCredentialsNonExpired;
     }

     @Override
     public boolean isEnabled() {
          return isEnabled;
     }

     public String getName() {
          return name;
     }

     public void setName(String name) {
          this.name = name;
     }

     public void setUsername(String username) {
          this.username = username;
     }

     public void setPassword(String password) {
          this.password = password;
     }

     public void setAccountNonExpired(boolean isAccountNonExpired) {
          this.isAccountNonExpired = isAccountNonExpired;
     }

     public void setAccountNonLocked(boolean isAccountNonLocked) {
          this.isAccountNonLocked = isAccountNonLocked;
     }

     public void setCredentialsNonExpired(boolean isCredentialsNonExpired) {
          this.isCredentialsNonExpired = isCredentialsNonExpired;
     }

     public void setEnabled(boolean isEnabled) {
          this.isEnabled = isEnabled;
     }

     public void setAuthorities(Collection<? extends GrantedAuthority> authorities) {
          this.authorities = authorities;
     }
}
```

- UserDetails 인터페이스를 구현해준다. implements UserDetails만 붙여주고 Alt + Enter(intellij 기준) 해주면 필요 메소드가 자동 생성 된다.
- 원래 블로그에서는 Getter / Setter를 만들었지만 롬복을 사용하기로 한다. @Data 어노테이션을 사용하면 Getter / Setter를 자동(?)으로 생성해준다.







4. 3에서 만든 User를 Spring Security에서 활용하기 위해서는 UserDetailsService 인터페이스를 구현한 Service Layer가 필요하다. UserService에 활용될 Persistent Layer인 UserMapper도 추가해보겠다.



<span style="color :#088A">아래의 mapping을 활용한 방식은 Mybatis 방식이고, 지금 사용하고 있는 방식은 JPA 방식으로 Mapper가 필요 없다 그렇기 때문에 참고 정도만 하자</span> 

### Mapper

```java
package com.cusonar.example.user.mapper;

import java.util.List;
import org.apache.ibatis.annotations.Mapper;

import com.cusonar.example.user.domain.User;

@Mapper
public interface UserMapper {
     public User readUser(String username);
     public List<String> readAuthority(String username);
}
```

지난번에 설명한 MyBatis를 이용해서 User를 가져 오는 Mapper 인터페이스입니다. Authority를 가져오는 인터페이스도 있어야 합니다.     .  readAuthority의 경우 쿼리에서 String을 리턴하기 때문에 List<String> 타입으로 받아야 합니다.(만약 GrantedAuthority로 받으시려면 번외글 참조 : http://cusonar.tistory.com/12)



resource/mybatis/mapper/UserMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.cusonar.example.user.mapper.UserMapper">
     <select id="readUser" parameterType="String" resultType="User">
          SELECT * FROM user WHERE username = #{username}
     </select>
    
     <select id="readAuthority" parameterType="String" resultType="String">
          SELECT authority_name FROM authority WHERE username = #{username}
     </select>
</mapper>
```

​     . 앞서 만든 테이블에서 username을 통해 User를 가져오는 쿼리입니다. Authority을 가져오는 쿼리도 있습니다.

5. 그런데 이렇게 코드를 추가하다보면 이게 제대로 동작하는지 궁금할때가 있습니다. 그렇다고 매번 서버를 올려서 Controller까지 다 만들어서 확인을 해볼수는 없는 노릇입니다. 그래서 여기서 테스트 코드를 한번 작성해보겠습니다. test 폴더 밑에다가 com.cusonar.example.user 패키지를 만들고, UserMapperTest 클래스를 만듭니다. 그 전에 테스트를 해보려면 DB에 데이터가 있어야 하므로 추가도 시켜줍니다.

```sql
insert into user values ('cusonar', '1234', 'YCU', true, true, true, true);
insert into user values ('abc', 'abcd', 'ABC', true, true, true, true);
insert into authority values ('cusonar', 'ADMIN');
insert into authority values ('cusonar', 'USER');
insert into authority values ('abc', 'USER');
```



src/test/java/com.cusonar.example.user/UserMapperTest.java

```java
package com.cusonar.example.user;
 
import static org.hamcrest.CoreMatchers.hasItem;
import static org.hamcrest.CoreMatchers.hasItems;
import static org.hamcrest.CoreMatchers.is;
import static org.junit.Assert.assertThat;

import java.util.List;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.SpringApplicationConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
import org.springframework.test.context.web.WebAppConfiguration;

import com.cusonar.example.ExampleApplication;
import com.cusonar.example.user.domain.User;
import com.cusonar.example.user.mapper.UserMapper;
 
@RunWith(SpringJUnit4ClassRunner.class)
@SpringApplicationConfiguration(classes = ExampleApplication.class)
@WebAppConfiguration
public class UserMapperTest {
     
     @Autowired UserMapper userMapper;
     
     @Test
     public void readUserTest() {
          User user = userMapper.readUser("cusonar");
          assertThat("cusonar", is(user.getUsername()));
          assertThat("YCU", is(user.getName()));
          assertThat("1234", is(user.getPassword()));
     }
     
     @Test
     public void readAuthorityTest() {
          List<String> authorities = userMapper.readAuthority("cusonar");
          assertThat(authorities, hasItems("ADMIN", "USER"));       
           
          authorities= userMapper.readAuthority("abc");
          assertThat(authorities, hasItem("USER"));     
     }
}
```







# Custom Login

1. 우선 UserService 인터페이스를 만든다. UserService 인터페이스는 UserDetailsService 인터페이스를 상속 받아야한다.

   이유는 UserDetailsService를 구현해야만 Spring Security에서 정상적으로 조회를 할 수 있기 때문이다.

```java
package com.cusonar.example.user.service;

import java.util.Collection;

import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.userdetails.UserDetailsService;

public interface UserService extends UserDetailsService {
     Collection<GrantedAuthority> getAuthorities(String username);
}
```

- 권한을 받아오는 메소드를 추가했다.
- 향후 유저 등록이라던지, 수정, 삭제 Service 등을 추가하면 된다.



2. UserService를 구현한 UserServiceImpl을 생성한다.

```java
package com.cusonar.example.user.service;

import java.util.ArrayList;
import java.util.Collection;
import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

import com.cusonar.example.user.domain.User;
import com.cusonar.example.user.mapper.UserMapper;

@Service
public class UserServiceImpl implements UserService {
    
     @Autowired UserMapper userMapper;

     @Override
     public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
          User user = userMapper.readUser(username);
          user.setAuthorities(getAuthorities(username));
         
          return user;
     }
    
     public Collection<GrantedAuthority> getAuthorities(String username) {
          List<String> string_authorities = userMapper.readAuthority(username);
          List<GrantedAuthority> authorities = new ArrayList<GrantedAuthority>();
          for (String authority : string_authorities) {
               authorities.add(new SimpleGrantedAuthority(authority));
          }
          return authorities;
     }
}
```

- UserService를 implements 하게 되면 loadUserByUsername 메소드를 반드시 구현해야한다. 이 메소드는 UserDetailsService에 정의된 메소드로 실제 Spring Security에서 User 정보를 읽을 때 사용된다.
- User를 읽어왔으면 권한을 부여해준다. getAuthorities 메소드를 이용해서 권한을 부여한 뒤 리턴 해준다. 권한은 MyBatis를 통해 Spring을 가져왔으므로 GrantedAuthority 인터페이스에 맞게 SimpleGrantedAuthority로 변환해서 리스트를 만든 후 리턴해 준다.





3. cofig 패키지를 만들고 SecurityConfig 클래스를 만든다.

```java
package com.cusonar.example.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

import com.cusonar.example.user.service.UserService;

@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
     @Autowired UserService userService;
     
     @Override
     protected void configure(HttpSecurity http) throws Exception {
          http
               .csrf().disable()
               .authorizeRequests()
                    .anyRequest().authenticated()
                    .and()
               .formLogin();
     }


     @Override
     protected void configure(AuthenticationManagerBuilder auth) throws Exception {
          auth.userDetailsService(userService);
     }
}
```

- WebSecurityConfigureAdapter는 기본적인 웹 인증에 대한 부분을 구현해 놓은 클래스이다. 해당 클래스를 상속 받은 뒤에 수정이 필요한 것만 override 해준다.
- 위에서 @Service로 정의한 userService를 Autowired 해주고,  AuthenticationManagerBuilder에 해당 Bean을 주힙해준다. (Customizing 한 UserService 적용)
- csrf().disable() : 이건 csrf (Cross Site Request Forgery)를 기본적으로 요청하는데, 이것을 하기 위해서는 따로 처리하는게 필요하므로 일단은 disable 처리한다.
- authorizeRequests() : 요청에 대해서 권한 처리를 한다.
- anyRequest().authenticated() : 어떠한 요청에도 인증을 요구한다. (영어 그대로 읽으면 그 의미)
- and().formLogin() : 그리고 Form을 이용한 로그인을 사용한다.



4. 매번 /cusonar로 접속하기 귀찮으실테니 HomeController에 경로 하나를 추가합니다.

```java
package com.cusonar.example.home.controller;
 
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import com.cusonar.example.home.domain.Home;
import com.cusonar.example.home.mapper.HomeMapper;

@RestController
public class HomeController {
	@Autowired HomeMapper homeMapper;
	
	@RequestMapping("/")
	public String home() {
		return "Hello World!";
	}
	
	@RequestMapping("/{name}")
	public Home home(@PathVariable String name) {
		Home home = homeMapper.readHome(name);          
		return home;
	}
}
```



5. 그리고 http://localhost:8080/ 으로 접속한다. 로그인 창이 나타나고 거기에 DB상에 입력한 ID/PW를 입력하면 정상적으로 Hello World!를 보실수 있습니다.





# 패스워드 암호화

만약 해커가 DB를 해킹했다면 사용자의 암호가 그대로 넘어가는 경우가 생길 수 있다. 그렇기 때문에 비밀번호를 암호화하는 방법에 대해 진행볼까한다.



1. UserService 인터페이스에 유저 등록 메소드를 추가합니다. 유저 등록만 계속 하면 테스트 하기 어려워지므로 삭제 메소드와 읽기 메소드도 같이 추가해줍니다. 그리고 사용할 PasswordEncoder를 리턴해줄 메소드도 추가해줍니다. (why? singleton으로 사용하기 위해서)
   * 왜 insert를 안쓰고 create를 써요? 저는 통일된 CRUD 메소드는 CRUD 단어를 사용합니다. 경험상 insert는 개발자마다 ins로 줄여버리기도 하더라구요. 그래서 전 CRUD 전체 단어를 사용하는걸 좋아합니다.

```java
package com.cusonar.example.user.service;

import java.util.Collection;

import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.userdetails.UserDetailsService;

import com.cusonar.example.user.domain.User;

public interface UserService extends UserDetailsService {
     Collection<GrantedAuthority> getAuthorities(String username);
     public User readUser(String username);
     public void createUser(User user);
     public void deleteUser(String username);
     public PasswordEncoder passwordEncoder();
}
```

2. UserServiceImpl 클래스에도 메소드를 추가 구현해줍니다. (왜 굳이 Service와 ServiceImpl로 나눌까요? 이건 나중에 Tips에서 다뤄봅시다. Tip이라기엔 아주 중요한거죠.)

```java
package com.cusonar.example.user.service;

import java.util.Collection;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.stereotype.Service;

import com.cusonar.example.user.domain.User;
import com.cusonar.example.user.mapper.UserMapper;

@Service
public class UserServiceImpl implements UserService {
    
     @Autowired UserMapper userMapper;
     private PasswordEncoder passwordEncoder = new BCryptPasswordEncoder();

     @Override
     public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
          User user = userMapper.readUser(username);
          user.setAuthorities(getAuthorities(username));
         
          return user;
     }
    
     public Collection<GrantedAuthority> getAuthorities(String username) {
          Collection<GrantedAuthority> authorities = userMapper.readAuthority(username);
         
          return authorities;
     }
    
     @Override
     public User readUser(String username) {
          User user = userMapper.readUser(username);
          user.setAuthorities(userMapper.readAuthority(username));
          return user;
     }

     @Override
     public void createUser(User user) {
          String rawPassword = user.getPassword();
          String encodedPassword = new BCryptPasswordEncoder().encode(rawPassword);
          user.setPassword(encodedPassword);
          userMapper.createUser(user);
          userMapper.createAuthority(user);
     }

     @Override
     public void deleteUser(String username) {
          userMapper.deleteUser(username);
          userMapper.deleteAuthority(username);
     }


     @Override
     public PasswordEncoder passwordEncoder() {
          return this.passwordEncoder;
     }
}
```

   . createUser 메소드는 userMapper의 createUser를 호출하기 전에 패스워드를 암호화해줍니다. 암호화에 사용된 인코더는 BCryptPasswordEncoder 입니다.

​     . Spring Security에 사용하는 인코더는 PasswordEncoder 인터페이스를 구현한 것이어야 합니다. 이것만 구현해준다면 본인만의 암호화 기법을 구현하시면 됩니다.

​     . PasswordEncoder는 인터페이스가 두개입니다. org.springframework.security.authentication.encoding.PasswordEncoder과 org.springframework.security.crypto.password.PasswordEncoder 두가지입니다. authentication 안에 있는 것은 deprecated(사용되지 않는) 된다고 적혀 있으니 사용하지 맙시다.



3. 이렇게 하려니 UserMapper에서 createUser, createAuthority, deleteUser, deleteAuthority를 추가해줘야합니다.

```xml
package com.cusonar.example.user.mapper;

import java.util.List;

import org.apache.ibatis.annotations.Mapper;
import org.springframework.security.core.GrantedAuthority;

import com.cusonar.example.user.domain.User;

@Mapper
public interface UserMapper {
     public User readUser(String username);
     public List<GrantedAuthority> readAuthority(String username);
     public void createUser(User user);
     public void createAuthority(User user);
     public void deleteUser(String username);
     public void deleteAuthority(String username);
}

 
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.cusonar.example.user.mapper.UserMapper">
     <select id="readUser" parameterType="String" resultType="User">
          SELECT * FROM user WHERE username = #{username}
     </select>
    
     <select id="readAuthority" parameterType="String" resultType="org.springframework.security.core.authority.SimpleGrantedAuthority">
          SELECT authority_name FROM authority WHERE username = #{username}
     </select>
    
     <insert id="createUser" parameterType="User">
          INSERT INTO user (username, password, name, isAccountNonExpired, isAccountNonLocked, isCredentialsNonExpired, isEnabled)
                    VALUES (#{username}, #{password}, #{name}, #{isAccountNonExpired}, #{isAccountNonLocked}, #{isCredentialsNonExpired}, #{isEnabled})
     </insert>
    
     <insert id="createAuthority" parameterType="org.springframework.security.core.GrantedAuthority">
          INSERT INTO authority (username, authority_name)
                    VALUES
                         <foreach item="authority" index="index" collection="authorities" separator=",">
                              (#{username}, #{authority})
                         </foreach>
     </insert>
    
     <delete id="deleteUser" parameterType="String">
          DELETE FROM user WHERE username = #{username}
     </delete>
    
     <delete id="deleteAuthority" parameterType="String">
          DELETE FROM authority WHERE username = #{username}
     </delete>
</mapper>
```

. createRole을 보시면 foreach 구문을 사용합니다. 간단히 설명드리면 동적 SQL을 작성하는 구문으로 반복에 사용됩니다. collection에 사용될 변수를 입력하고, item은 반복되는 데이터를 표현합니다. seperator는 반복시에 반복되는 구문의 사이에 추가하는 값입니다

​     . 위와 같이 했을 때, username=cusonar, authorities={USER, ADMIN}이 들어온다면 INSERT INTO role (username, rolename) VALUES (cusonar, ADMIN), (cusonar USER) 가 됩니다.



4. 여기까지 되었으면 정상적으로 들어가는지 확인해야 합니다. 간단하게 테스트 코드를 이용해서 등록 작업을 해봅시다.

```java
package com.cusonar.example.user;

import static org.hamcrest.CoreMatchers.hasItem;
import static org.hamcrest.CoreMatchers.is;
import static org.junit.Assert.assertThat;

import java.util.Collection;
import java.util.Iterator;

import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.SpringApplicationConfiguration;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.AuthorityUtils;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
import org.springframework.test.context.web.WebAppConfiguration;

import com.cusonar.example.ExampleApplication;
import com.cusonar.example.user.domain.User;
import com.cusonar.example.user.service.UserService;

@RunWith(SpringJUnit4ClassRunner.class)
@SpringApplicationConfiguration(classes = ExampleApplication.class)
@WebAppConfiguration
public class UserServiceTest {
     @Autowired private UserService userService;
    
     private User user1;
    
     @Before
     public void setup() {
          user1 = new User();
          user1.setUsername("user1");
          user1.setPassword("pass1");
          user1.setAccountNonExpired(true);
          user1.setAccountNonLocked(true);
          user1.setName("USER1");
          user1.setCredentialsNonExpired(true);
          user1.setEnabled(true);
          user1.setAuthorities(AuthorityUtils.createAuthorityList("USER"));
     }
    
     @Test
     public void createUserTest() {
          userService.deleteUser(user1.getUsername());
          userService.createUser(user1);
          User user = userService.readUser(user1.getUsername());
          assertThat(user.getUsername(), is(user1.getUsername()));
         
          PasswordEncoder passwordEncoder = userService.passwordEncoder();
          assertThat(passwordEncoder.matches("pass1", user.getPassword()), is(true));

          Collection<? extends GrantedAuthority> authorities1 = user1.getAuthorities();
          Iterator<? extends GrantedAuthority> it = authorities1.iterator();
          Collection<GrantedAuthority> authorities = (Collection<GrantedAuthority>) user.getAuthorities();
          while (it.hasNext()) {
               GrantedAuthority authority = it.next();
               assertThat(authorities, hasItem(new SimpleGrantedAuthority(authority.getAuthority())));
          }
     }
}
```

5. 마지막으로 SecurityConfig에도 PasswordEncoder를 적용시켜줍시다.

```java
package com.cusonar.example.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;

import com.cusonar.example.user.service.UserService;

@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
     @Autowired UserService userService;
    
     @Override
     protected void configure(HttpSecurity http) throws Exception {
          http
               .csrf().disable()
               .authorizeRequests()
                    .anyRequest().authenticated()
                    .and()
               .formLogin();
     }
    
     @Override
     protected void configure(AuthenticationManagerBuilder auth) throws Exception {
          auth.userDetailsService(userService)
               .passwordEncoder(userService.passwordEncoder())
               ;
     }
}
```



6. 테스트 코드에서 user1 계정이 등록되었으므로 user1/pass1 으로 로그인이 정상적으로 되는 것을 확인할 수 있습니다. 또한 DB 상에는 패스워드가 암호화 되서 들어가 있는것도 확인됩니다. 물론 기존에 등록된 cusonar와 abc 계정은 접속되지 않습니다. 두 계정도 똑같이 인코딩해서 넣어주면 정상적으로 로그인이 될겁니다.



여기까지 패스워드를 암호화해서 넣는 방법에 대해서 알아봤습니다. 물론 BCrypt를 사용할 필요는 없습니다. 다른 PasswordEncoder 구현체가 많으니 시스템에 맞는 것을 사용하시면 됩니다. 또한 솔루션을 사용하거나 기존에 암호화된 데이터들이 있는 경우에는 PasswordEncoder를 구현해서 사용하시면 됩니다. 이로써 DB 안의 패스워드가 그대로 노출되지 않게되었습니다.