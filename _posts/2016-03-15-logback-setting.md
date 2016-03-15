---
layout: post
title: LOGBack 설정
categories: [log]
tags: [log, logback]
description: LOGBack 설정
---

## LOGBack
LOGBack은 Ceki Gülcü 개발자가 SLF4J 아키텍쳐 기반으로 만들었다고 합니다.
LOGBack이 좋은 점은 <span style="color:#ff0000">성능 10배 향상, 메모리 점유율 낮아짐, 자동 삭제</span> 등등 아래 링크에 자세하게 설명되어 있습니다.

{% highlight yaml %}
https://beyondj2ee.wordpress.com/2012/11/09/logback-%EC%82%AC%EC%9A%A9%ED%95%B4%EC%95%BC-%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0-reasons-to-prefer-logback-over-log4j/
{% endhighlight %}

간단하게 LOGBack을 적용하는 방법만 소개하겠습니다.

### 1. pom.xml에 dependency 추가!
sl4j는 이미 dependency 설정이 되어있기 때문에 패스.

``` xml
        <!-- logback -->
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>1.1.2</version>
        </dependency>
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-core</artifactId>
            <version>1.1.2</version>
         </dependency>
```

### 2. logback.xml 설정
``` xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <!-- file로 log 남기기 -->
    <timestamp key="byDay" datePattern="yyyyMMdd"/>
    <appender name="file" class="ch.qos.logback.core.FileAppender">
        <file>/logs/oneq-${byDay}.log</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>/logs/oneq-${byDay}.log.zip</fileNamePattern>
            <maxHistory>3</maxHistory>
        </rollingPolicy>
        <encoder>
            <pattern>
               ▶ %-5level %d{HH:mm:ss.SSS} [%thread] %logger[%method:%line] - %msg%n
            </pattern>
        </encoder>
    </appender>

    <logger name="org.springframework" level="info" />
    <root level="info">
        <appender-ref ref="console" />
        <appender-ref ref="file" />
    </root>
        
</configuration>
```
저는 기존에 있던 log4j.xml 파일을 삭제하고 새롭게 생성하였습니다.
위에 코드에서 필요한 것만 간단하게 설명하자면,

``` xml
<timestamp key="byDay" datePattern="yyyyMMdd"/>
```

timestamp 태그 년월일 데이터를 받아 파일 이름에 추가하기 위함입니다.

``` xml
<appender name="file" class="ch.qos.logback.core.FileAppender">
        <file>/logs/oneq-${byDay}.log</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>/logs/oneq-${byDay}.log.zip</fileNamePattern>
            <maxHistory>3</maxHistory>
        </rollingPolicy>
        <encoder>
            <pattern>
               ▶ %-5level %d{HH:mm:ss.SSS} [%thread] %logger[%method:%line] - %msg%n
            </pattern>
        </encoder>
    </appender>
```

appender name이 "file"을 설정하면, 파일 생성, "console"을 입력하면 콘솔에 찍는것입니다.


### 3. 로그 파일 디렉토리
file 태그 안에 폴더와 파일이름을 적습니다.
여기서 디렉토리 위치는 원하시는대로 적으면 되는데 위와 같이 아무것도 적지 않을 경우 C드라이브에 logs 폴더가 생성되었고,
${user.home}을 적으면, C:\Users\NHNENT.AD0 에 폴더가 생성되었습니다.

![logback_directory]({{ site.BASE_PATH }}/assets/post/2016-03-15/logback_directory.png)

file 생성 위치에 ${USER.HOME}을 대문자로 입력했을때는 위치를 찾지 못해, STS 설치 폴더에 위에 사진처럼 USER_HOME_IS_UNDEFINED 이름으로 폴더가 생성되었고 그 안에 로그가 저장되었습니다.

파일 이름에 위에서 설정한 timestamp 태그의 ${byDay}를 넣었기 때문에 일별로 다음과 같이 파일 이름에 년월일이 들어가게 됩니다.
![logback_filename]({{ site.BASE_PATH }}/assets/post/2016-03-15/logback_filename.png)
<br>

``` xml
<maxHistory>3</maxHistory>
```

### 4. maxHistory

maxHistory 태그는 로그 파일을 삭제하는 기준을 설정합니다.
<span style="color:#ff0000">문제는 이 숫자가 요일인지 월인지 기준이 확실하지 않습니다. </span>

LOGBack 홈페이지 메뉴얼에 다음과 같은 정보가 있었습니다.
![logback_maxhistory]({{ site.BASE_PATH }}/assets/post/2016-03-15/logback_maxhistory.png)

<br>

6이란 숫자로 set하면 6month라고 적혀있어서 월 기준인가 싶었는데, 조금 밑으로 내려가니
![logback_maxhistory_example]({{ site.BASE_PATH }}/assets/post/2016-03-15/logback_maxhistory_example.png)


제 눈을 의심했지만, 예제에는 30이 set되어있었고 주석은 일 기준으로 적혀있었습니다.

그래서 결론은 몇일 뒤에 삭제되나 보고 알려드리겠습니다.
<br>
마지막으로 encoder 태그는 원하는 형식으로 로그를 찍으면 됩니다.

``` xml
    <logger name="org.springframework" level="info" />
    <root level="info">
        <appender-ref ref="console" />
        <appender-ref ref="file" />
    </root>
```
이건 로그 레벨을 설정하는 것인데, LOGBack은 5가지 레벨로 이루어져있습니다.
TRACE, DEBUG , INFO, WARN, ERROR
위에 설정은 INFO로 설정해두었기 때문에, TRACE, BEBUG 레벨을 로그에 찍히지 않습니다.

### 3. 적용하기
이제 설정은 모두 끝났고 아래 처럼 로그를 선언하고 사용하면 됩니다.
``` java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
private static final Logger logger = LoggerFactory.getLogger(ApiController.class);
logger.info("hello");
```

![logback_example]({{ site.BASE_PATH }}/assets/post/2016-03-15/logback_example.png)

컨트롤러에 코드를 적고 테스트 했을 때, 콘솔과 파일에 잘 찍히는 것을 확인할 수 있습니다. 부족한 점은 추가해주세요!
감사합니다.

### 추가로 서버에 올릴때, 파일 path 잡는 법입니다. 파일태그에 ${HOME}을 적어주시면, irteam 위치입니다. 
아래 예제 참고하세요!

``` xml
<file>${HOME}/apps/logs/oneq-${byDay}.log</file>
```