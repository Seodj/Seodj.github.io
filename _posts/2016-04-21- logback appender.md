---
layout: post
title: LogBack appender 분리하기.
categories: [log]
tags: [log, logback]
description: LogBack 서로 다른 appender로 분리하기.
---

# LogBack appender 분리하기.

* 목적
한 클래스 파일에서 서로 다른 appender을 쓰고 싶을 경우가 있다. 이때, 적용할 수 있는 방법을 소개한다.

1. 로그를 찍는 Class

``` java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

private static final Logger doorayLogger = LoggerFactory.getLogger(A클래스.class);
```

2. logback 설정

``` xml
<appender name="BODYFILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
  <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
    <!-- daily rollover -->
    <file>/home1/irteam/logs/api-body-%d{yyyy-MM-dd}.log</file>
    <fileNamePattern>/home1/irteam/logs/api-body.log.%d{yyyy-MM-dd}.%i.log.gz</fileNamePattern>
    <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
    <maxFileSize>100MB</maxFileSize>
    </timeBasedFileNamingAndTriggeringPolicy>
    <!-- keep 180 days' worth of history -->
    <maxHistory>180</maxHistory>
  </rollingPolicy>
<encoder>
  <charset>UTF-8</charset>
  <pattern>%d{HH:mm:ss.SSS} - %msg%n</pattern>
  </encoder>
</appender>

<logger name="com.nhnent.asdf.asd.as.A클래스" level="info" additivity="false">
  <appender-ref ref="BODYFILE" />
</logger>
```

위에와 같이 logger의 name을 <span style="color:#fd4304">패키지.클래스명</span>으로 적고, class에서 <span style="color:#fd5004">LoggerFactory.getLogger("클래스명");</span> 을 적게 되면 해당 logger를 불러와 그 해당 logger에만 찍을 수 있다.


additivity="false" 설정을 하게 되면 해당 클래스의 상위 logger에는 찍히지 않게 된다.

예를 들어, 한 클래스 안에 log, bodyLogger 2개의 인스턴스가 있다고 해보자.
log는 logger의 위치가 해당 패키지이기 때문에 어떤 그 패키지 안에 로그를 직게 되면 모두 찍히게 된다.

bodyLogger.error("bodyLogger 에러입니다."); 라고 호출 했을 경우에도, log에 찍히게 된다.
하지만 additivity="false"를 bodyLogger 설정에 주게 되면, bodyLogger에서 찍은 로그는 bodyLogger의 appender에만 찍히게 된다.
additivity는 default 값이 true임으로 참고하기 바란다.
