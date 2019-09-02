
## github와 maven 설치
<pre><code>
sudo su

yum install -y git

sudo wget http://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo -O /etc/yum.repos.d/epel-apache-maven.repo


sudo sed -i s/\$releasever/6/g /etc/yum.repos.d/epel-apache-maven.repo


sudo yum install -y apache-maven


mvn --version

</code></pre>
출처 : https://gist.github.com/sebsto/19b99f1fa1f32cae5d00

## java 설치 및 maven JAVA_HOME 환경설정
* 자바설치
```
sudo yum install -y java-1.8.0-openjdk-devel.x86_64
```
* 버전 확인
```aidl
sudo /usr/sbin/alternatives --config java
```
* 사용하지 않는 버전 삭제
```aidl
sudo yum remove java-1.7.0-openjdk
```

* 최종 버전 확인
```aidl
java -version
```

1. 확인
```
echo $JAVA_HOME
java -version
```
2. java 위치 확인
```
which java
 /usr/bin/java
readlink -f/usr/bin/java
/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.201.b09-0.43.amzn1.x86_64/bin/java
```
3. $JAVA_HOME 설정
```
vi /etc/profile
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.201.b09-0.43.amzn1.x86_64
```
4. 확인 2
* SSH 재접속 후 echo로 확인
```
echo $JAVA_HOME
```

출처 : https://zetawiki.com/wiki/%EB%A6%AC%EB%88%85%EC%8A%A4_$JAVA_HOME_%ED%99%98%EA%B2%BD%EB%B3%80%EC%88%98_%EC%84%A4%EC%A0%95