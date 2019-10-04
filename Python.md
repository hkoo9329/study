# Python

# 목차

- 문자열 연산자
  - [문자열 연결 연산자 : +](#문자열-연결-연산자-:-+)
  - [문자열 반복 연산자 : *](#문자열-반복-연산자-:-*)
  - [문자 선택 연산자(인덱싱) : []](#문자-선택-연산자(인덱싱)-:-[])

## 문자열 연산자



### 문자열 연결 연산자 : +

```python
print("안녕" + "하세요")
#>> 안녕하세요

print("안녕" + 1)
#>> TypeError : can only concatenate str (not "int") to str

```



### 문자열 반복 연산자 : *

```python
print("안녕하세요" * 3)
#안녕하세요안녕하세요안녕하세요

```



### 문자 선택 연산자(인덱싱) : []

문자 선택 연산자는 문자열 내부의 문자 하나를 선택하는 연산자이다. 다른 언어들과 마찬가지로 0부터 세는 제로 인덱스이다.

```python
print("문자 선택 연산자")
print("안녕하세요"[0])
#안
print("안녕하세요"[1])
#녕
print("안녕하세요"[2])
#하
print("안녕하세요"[3])
#세
print("안녕하세요"[4])
#요
```

대괄호 안에 숫자를 음수로 입력하면 뒤에서부터 선택이 가능하다.  이것은 다른 언어들과 차별화 되는 방식이다. 

```python
print("문자 선택 연산자")
print("안녕하세요"[-1])
#요
print("안녕하세요"[-2])
#세
print("안녕하세요"[-3])
#하
print("안녕하세요"[-4])
#녕
print("안녕하세요"[-5])
#안

```



### 문자열 범위 선택 연산자(슬라이싱) : [:]

문자열의 특정 범위를 선택할 때 사용하는 연산자이다. 

```python
print("안녕하세요"[1:4]) #범위에서 마지막 숫자는 포함하지 않는다. 
#녕하세


#앞 혹은 뒤 글자 생략의 경우 가장 뒤 혹은 앞으 인덱스를 의미한다.
print("안녕하세요"[1:])
#녕하세요

print("안녕하세요"[:3])
#안녕하

```

[] 기호를 이용해 문자열의 특정 위치에 있는 문자를 참조하는 것을 인덱싱이라고 하고,

[:] 기호를 이용해 문자열의 일부를 추출하는 것을 슬라이싱이라 한다.

그리고 슬라이싱을 하더라도 원본은 변하지 않는다.



### 문자열 길이 구하기

```python
# len()은 개수(길이)를 알려주는 함수
print(len("hello"))
#5
```



### 정수 나누기 연산자: //

```python
print("3 / 2 = ",3/2)
#3 / 2 = 1.5
print("3 // 2 = ",3//2)
#3 // 2 = 1

# // 연산자를 사용한 수식의 결과는 소수점 아래를 버린 값이 출력된다. 즉 int형으로 출력해준다는 말이다.

```



### 나머지 연산자: %

다른 언어와 사용법은 같음



### 제곱 연산자 : **

```python
print("2 ** 3 = ", 2 ** 3)
#2 ** 3 = 8
# 즉 2^3과 같은 결과를 출력해줌
```



### 사용자 입력 : input()

```python
string = input("인사말을 입력하세요 > ")
#인사말을 입력하세요 > 안녕하세요
print(string)
#안녕하세요

# input() 함수는 사용자가 무엇을 입력해도 결과는 무조건 문자열 자료형이다.

```



### 문자열을 숫자로 바꾸기

이러한 형변환을 캐스트라고 부른다.

- int() 함수 : 문자열을 int형 자료형으로 변환한다.
- float() 함수 : 문자열을 float형으로 변환한다.





## 숫자와 문자열의기능들

### 문자열의 format() 함수

```python
# 문자열의 {} 기호가 format() 함수 괄호 안에 있는 매개변수로 차례로 대치된다.
string = "{}".format(10)

print(string)
#10
print(type(string))
#<class 'str'>

format_a = "{}만원".format(5000)
format_b = "{} {} {}".format(3000,4000,5000)

#출력
#format_a => 5000만원
#format_b => 3000 4000 5000


# 정수
output_a = "{:d}".format(52)

# 특정 칸에 출력하기
output_b = "{:5d}".format(52)	# 5칸	=>     52
output_c = "{:10d}".format(52)	# 10칸	=>          52

# 빈칸을 0으로 채우기
output_d = "{:05d}".format(52)	#양수	=>	00052
output_d = "{:05d}".format(-52)	#음수	=>	-0052


# 기호와 함께 출력하기
output_f = "{:+d}".format(52) 	# +52	
output_g = "{:+d}".format(-52)	# -52
output_h = "{: d}".format(52)	# 52	# 양수 : 기호 부분 공백
output_i = "{: d}".format(-52)	# -52	# 음수 : 기호 부분 공백

# 소수점 아래 자릿수 지정
a = "{:15.3f}".format(52.273)	#               52.273
b = "{:15.2f}".format(52.273)	#               52.27
c = "{:15.1f}".format(52.273)	#               52.3

# 이렇게 입력하면 15칸을 잡고 소수점을 각각 3자리 2자리 1자리로 출력한다. 이때 자동으로 반올림도 일어난다.

# 의미 없는 소수점 제거
output_a = 52.0
output_b = "{:g}".format(output_a)	#52

```



### 대소문자 바꾸기 : upper() 와 lower()

```python
a = "Hello Python Programming"
a.upper()
#HELLO PYTHON PROGRAMMING
a.lower()
#hello python programming
# 원본 a의 변경은 이루어지지 않는다.
```



### 문자열 공백 제거하기 : strip()

- strip() : 문자열 양옆의 공백을 제거한다.
- lstrip() : 문자열 왼쪽의 공백을 제거한다.
- rstrip() : 문자열 오른쪽의 공백을 제거한다.



### 문자열의 구성 파악하기:  isXX()

- isalnum() : 문자열이 알파벳 또는 숫자로만 구성되어 있는지 확인한다.
- isalpha() : 문자열이 알파벳으로만 구성되어 있는지 확인한다.
- isidentifier() : 문자열이 식별자로 사용할 수 있는 것인지 확인한다.
- isdecimal() : 문자열이 정수 형태인지 확인한다.
- isdigit() : 문자열이 숫자로 인식될 수 있는 것인지 확인한다.
- isspace() : 문자열이 공백으로만 구성되어 있는지 확인한다.
- islower() : 문자열이 소문자로만 구성되어 있는지 확인한다.
- isupper() : 문자열이 대문자로만 구성되어 있는지 확인한다.

출력은 boolean 형식으로 출력된다.





### 문자열 찾기 : find() 와 rfind()

- find() : 왼쪽부터 찾아서 처음 등장하는 위치를 찾는다.
- rfind() : 오른쪽부터 찾아서 처음 등장하는 위치를 찾는다.



```python
output_a = "안녕안녕하세요".find("안녕")
print(output_a)
# 0
output_b = "안녕안녕하세요".rfind("안녕")
print(output_b)
# 2
```



### 문자열과 in 연산자

문자열 내부에 어떤 문자열이 있는지 확인하려면 <span style="color:red;">in 연산자</span>를 사용한다.출력은 boolean 형식으로 출력된다.

```python
print("안녕" in "안녕하세요") # True
print("잘자" in "안녕하세요") # False
```



### 문자열 자르기 : split()

```python
a = "10 20 30 40 50".split(" ")
print(a)
# ['10','20','30','40','50']

```



## 조건문

먼저 파이썬이 다른 언어와 다름점으로 문자열에도 비교 연산자가 가능하다는 점이다. 아스키코드 값을 비교하는 것이 아닌 사전 순서로 비교한다는 것이다. 

이것은 솔직히 좀 놀란 점 중 하나이다.

```python
print("가방"=="가방")
#True
print("가방"!="하마")
#True
print("가방"<"하마")
#True
print("가방">"하마")
#False
```



### not 연산자

not 연산자는 단항 연산자로, 참과 거짓을 반대로 바꿀 때 사용한다.

```python
print(not True)
#False
print(not False)
#True
```

### and 연산자와 or 연산자

써놓고 보니 다른 언어들과 같은 내용임으로 설명은 패스

java나 c언어에서의 &&나 ||와 같다고 생각하면 된다.





### if 조건문

```python
number = input("insert number :")
number = int(number)

if number > 0:
    print("Positive number")

if number < 0:
    print("negative number")
```



### 날짜 시간 활용

```python
# 날짜 시간과 관련된 기능을 가져온다.
import datetime

# 현재 날짜 시간을 구한다.
now = datetime.datetime.now()

print(now.year, "년")
print(now.month, "월")
print(now.day,"일")
print(now.hour,"시")
print(now.minute,"분")
print(now.second,"초")
#출력
# 2019 년
# 9 월
# 17일
# 17시
# 45분
# 30초

print("{}년 {}월 {}일 {}시 {}분 {}초".format(
	now.year,
   	now.month,
    now.day,
    now.hour,
    now.minute,
    now.second
))
# 출력
# 2019년 9월 17일 17시 45분 30초

# 아래와 같은 시간 비교 가능
if now.hour < 12:
    print("am")

if now.month <=5 and 3<=now.month:
    print("spring")
    
```



### 짝수 홀수 구분

```python
number = input("정수 입력 : ")

# 마지막 자리 숫자를 추출
last_chracter = number[-1]

# 숫자로 변환하기
last_number = int(last_character)

# 짝수 확인
if last_number == 0\
	or last_number == 2\
    or last_number == 4\
    or last_number == 6\
    or last_number == 8:
        print("짝수입니다.")
     
# 여기서 \는 엔터시 계속 같은 라인이다라는 것을 표시해주는 것이다.

```



### if, elif,else

크게 다른것은 없고 else if가 파이썬에서는 `elif`라는 키워드로 사용한다.

```python
if 조건A:
    ~
elif 조건B:
    ~
else:
    ~
```



### pass 키워드

```python
if number >0:
    #양수일때
else:
    #음수일때
```

다른 프로그래밍 언어에서는 위에서 처럼 조건문 뒤에 아무것도 작성하지 않아도 정상적으로 실행되지만, 파이썬의 경우에는 if 조건문 사이에는 무조건 들여쓰기 4칸을 넣고 코드를 작성해야만 구문이 성립되기 때문에 위와 같이 작성한 경우에는 `IndentationError`를 발생한다.

`IndentationError`는 들여쓰기가 잘못되어 있다라는 의미이다. 그렇기 때문에 if 구문 사이에는 어떤 내용이라도 넣어 줘야 한다. 다음과 같이 0을 넣어도 일단 실행은 정상적으로 된다.

그때 사용하는 것이 pass라는 것이다. pass 키워드를 만나면 "아무것도 안함", "곧 개발하겠음" 정도의 의미로 해석하면 된다.



```python
# 입력을 받는다.
number = input("정수 입력> ")
number = int(number)

# 조건문 사용
if number > 0:
    # 양수 일 때
    pass
else:
    # 음수 일 때
    pass

```



### raise NotImplementedError

raise 키워드와 NotImplementedError를 조합해 raise NotImplementedError를 사용하면 아직 구현하지 않은 부분이다 라는 오류를 강제로 발생시킬 수도 있다.

```python
# 입력을 받는다.
number = input("정수 입력 : ")
number = int(number)

# 조건문 사용
if number > 0:
    # 양수 일 때
    raise NotImplementedError
else:
    # 음수 일 때
    raise NotImplementedError
```

 코드를 실행하면 코드의 실행은 정상적으로 진행된다. 대신 구현되지 않은 부분에 들어서는 순간 NotImplementedError라는 오류를 발생시킨다. 따라서 "이 부분을 구현을 안했구나"라고 인지 할 수 있다.





## 리스트와 반복문

파이썬에서 리스트의 의미는 여러가지 자료를 저장할 수 있는 자료이다.

파이썬에서 리스트를 생성하는 방법은 다음과 같이 대괄호 [] 에 자료를 쉼표로 구분해서 입력한다. 대괄호[] 내부에 넣은 자료를 `요소`라고 하고 영어로는 `element`라고 부른다.

```python
[1,2,3,4,5]
# 각각 다른 형이지만 같은 리스트에 요소로 입력가능
list_a = [235,32,True,"문자열"]

list_a[0]
#235


# 리스트도 뒤에서부터 요소를 선택할 수 있다.
list_a[-1]
#문자열

# 이런식으로 사용도 가능 
list_a[-1][0] 
#문
```

```python

```

