---
title: mysql workbench로 trigger 생성해보기
author: 서동진
profile_picture: http://www.famousbirthdays.com/faces/laurel-stan-image.jpg

---


##1. trigger란?

trigger 단어는 방아쇠라는 뜻을 가진다.

그럼 데이터베이스에서 trigger는 무슨 뜻일까?
{% highlight ruby %}
데이터베이스 트리거(Database Trigger)는 테이블에 대한 이벤트에 반응해 자동으로 실행되는 작업을 의미한다. 트리거는 데이터 조작 언어(DML)의 데이터 상태의 관리를 자동화하는 데 사용된다. 트리거를 사용하여 데이터 작업 제한, 작업 기록, 변경 작업 감사 등을 할 수 있다.
{% endhighlight %}

테이블에 대한 이벤트에 따라 원하는 작업을 추가할 수 있다!


##2. workbench에서 trigger 추가하기

![trigger image]({{ site.url }}/assets/trigger.jpg)

위에 사진처럼 원하는 트리거를 추가하고 싶은 테이블을 선택하여 alter table을 선택한다.
항목에서 trigger을 선택하여

![trigger image2]({{ site.url }}/assets/trigger2.jpg)

다음과 같이 어떤 이벤트에 트리거를 추가할지 선택한다.
before과 after은 이벤트가 일어나기 전과 후 차이이다.

{% highlight ruby %}
CREATE DEFINER=`root`@`localhost` TRIGGER `db_oneq`.`tb_question_BEFORE_UPDATE` BEFORE UPDATE ON `tb_question` FOR EACH ROW
BEGIN
    IF OLD.vote_limit <= NEW.vote_num
    THEN SET NEW.finished_date = NOW();
    END IF;
END
{% endhighlight %}

나는 다음과 같은 문법으로 추가하였다.
tb_question에서 참여자 제한 수 vote_limit과 참여자 수 vote_num 2가지 컬럼이 있는데, 참여자 수가 참여자 제한수와 같아질 때,
finished_date에 종료 시간을 저장해주는 목적으로 trigger을 만들었다.

apply 한 후, 실제 vote_num을 vote_limit과 같은 수로 업데이트 시켜주면 finished_date에 현재 시간이 들어간다.
접근이 많은 테이블에 trigger을 사용하면, 서비스에 부담을 줄 수 있다는 점!