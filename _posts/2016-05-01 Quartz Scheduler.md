---
layout: post
title: Quartz 스케줄러
categories: [scheduler]
tags: [Quartz, scheduler]
description: Quartz 스케줄러 + 스프링
---

# 스케줄러란?
* 어플리케이션 서버 내에서 주기적으로 발생하거나 반복적으로 발생하는 작업을 지원하는 기능.
* 유닉스에서는 CronTab을 예시로 들 수 있다.

## Quartz 스케줄러
1. Quartz는 자바 기반 오픈 소스 스케줄링 프레임워크이다.
2. 유닉스의 cron 표현식으로 스케줄을 설정할 수 있다.

**cron 표현식**
* 초, 분, 시, 일, 월, 요일, 연도
* * : 모든 값
* ? : 특정 값 없음
* - : 범위 지정
* , : 여러 값 지정 구분에 사용
* / : 초기값과 증가치 설정에 사용
* L : 지정할 수 있는 범위의 마지막 값
* W : 월~금요일 또는 가장 가까운 월/금요일
* # : 몇 번째 무슨 요일 2#1 => 첫 번째 월요일

```
0 0 12 ? * WEB  -  "매주 수요일 12시"
0 0/10 * ?  -  "매 10분마다 실행"
```

## 구성
maven dependency

``` xml
<dependency>
	<groupId>org.quartz-scheduler</groupId>
	<artifactId>quartz</artifactId>
	<version>2.2.2</version>
</dependency>
```

Quartz를 사용하기 위한 필수 구성은 다음가 같다.
* Scheduler
* Job
* JobDetail
* Trigger



아래와 같이 실행시킬 job 메소드를 정의해놓고,

``` java
public class QuartzJobExecute extends QuartzJobBean {
	protected void executeInternal(JobExecutionContext context) throws JobExecutionException {
    	// executeJob();
    }
}
```

Job -> JobDetail -> Trigger -> Scheduler 순으로 set해준다.
SchedulerFactoryBean 클래스에 afterPropertiesSet() 메소드는 모든 properties가 set된 뒤, BeanFactory에 의해 실행된다.


``` java
public class SchedulerFactoryBean extends SchedulerFactoryBean {

	public CronTrigger[] getCronTriggerArray() {
    	JobDataMap jobDataMap = new JobDataMap();
        jobDataMap.put("job",job);
        jobDataMap.put("jobName",jobName);
        JobDetailImpl jdi = new JobDetailImpl();
        jdi.setJobDataMap(jobDataMap);
        CronTriggerFactoryBean trigger = new CronTriggerFactoryBean();
        trigger.setJobDetail(jdi);
        trigger.setCronExpression("0 0/10 * ?");
        trigger.afterPropertiesSet();

        return asdasd;
    }
```



출처
http://fmd1225.tistory.com/60
http://docs.spring.io/spring/docs/3.2.6.RELEASE/javadoc-api/org/springframework/scheduling/quartz/SchedulerFactoryBean.html
