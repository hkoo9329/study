## 서비스 등록
* 서비스 등록
```
sudo vi /etc/init.d/myApp
```

~~~
#!/bin/sh
SERVICE_NAME=myApp
PATH_TO_JAR=/usr/local/myApp.jar
PID_PATH_NAME=/tmp/myApp-pid
case $1 in
    start)
        echo "Starting $SERVICE_NAME ..."
        if [ ! -f $PID_PATH_NAME ]; then
            nohup java -jar $PATH_TO_JAR /tmp 2>> /dev/null >> /dev/null &
                        echo $! > $PID_PATH_NAME
            echo "$SERVICE_NAME started ..."
        else
            echo "$SERVICE_NAME is already running ..."
        fi
    ;;
    stop)
        if [ -f $PID_PATH_NAME ]; then
            PID=$(cat $PID_PATH_NAME);
            echo "$SERVICE_NAME stoping ..."
            kill $PID;
            echo "$SERVICE_NAME stopped ..."
            rm $PID_PATH_NAME
        else
            echo "$SERVICE_NAME is not running ..."
        fi
    ;;
    restart)
        if [ -f $PID_PATH_NAME ]; then
            PID=$(cat $PID_PATH_NAME);
            echo "$SERVICE_NAME stopping ...";
            kill $PID;
            echo "$SERVICE_NAME stopped ...";
            rm $PID_PATH_NAME
            echo "$SERVICE_NAME starting ..."
            nohup java $JAVA_OPTS -jar $PATH_TO_JAR /tmp 2>> /dev/null >> /dev/null &
                        echo $! > $PID_PATH_NAME
            echo "$SERVICE_NAME started ..."
        else
            echo "$SERVICE_NAME is not running ..."
        fi
    ;;
esac
~~~

* 서비스 실행
~~~
service myapp start | stop | restart
~~~

* Maven
```aidl
 <plugin>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-maven-plugin</artifactId>
	<configuration>
		<executable>true</executable>
	</configuration>
</plugin>
```

* 서비스 등록
```
sudo ln -s /var/myapp/myapp.jar /etc/init.d/myapp
```

* 서비스 실행
```aidl
service myapp start
```


참조 : https://heowc.tistory.com/38 , https://100milliongold.github.io/2018/12/27/Spring-Boot-jar%EB%A1%9C-%EC%84%9C%EB%B9%84%EC%8A%A4-%EB%93%B1%EB%A1%9D%ED%95%98%EA%B8%B0/
