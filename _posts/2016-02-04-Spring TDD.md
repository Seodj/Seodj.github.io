---
layout: post
title: Spring 프로젝트 테스트 코드.
categories: [TDD]
tags: [spring, tdd]
description: Spring 프로젝트 테스트 코드.
---

# 단위 테스트란?
* 소스 코드의 특정 모듈이 의도대로 정확히 작동하는지 검증하는 절차.
* 즉, 모든 함수와 메소드에 대해 테스트 케이스를 작성하는 절차!

# 테스트 더블?
* 테스트 작성 시 테스트 대상 코드와 상호작용하는 객체
* 테스트 대상 코드와 협력 객체를 분리

# 스프링에서 테스트?

스프링에서 테스트 케이스를 짜려면 어떻게 해야 할까?
스프링MVC는 크게 Controller, Service, Dao의 레이어로 볼 수 있습니다. 각 레이어 별 테스트를 어떻게 진행해야 할지 간단하게 소개하고 진행한 예제 코드를 첨부합니다.

테스트 전 Maven pom.xml에서 junit, mokito, spring-test 라이브러리를 추가해줍니다.

{% highlight yaml %}
<!-- Test -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.7</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.mockito</groupId>
            <artifactId>mockito-all</artifactId>
            <version>1.9.5</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>3.2.3.RELEASE</version>
            <scope>test</scope>
        </dependency>
{% endhighlight %}


# 1. Controller
스트링 MVC에서 컨트롤러의 역할은 무엇일까? 브라우저에서 받은 Request를 URL을 통해 각 컨트롤러에서 필요한 Service를 호출하고 그 결과를 반환해 주는 역할이 주역할이다.
여기서 중요한 점은 Controller는 웹 세상이다. 때문에, request, response, session 등 웹에 관련된 것을 Controller에서 벗어나면 안된다. Controller에서 사용자 요청에 의해 분기된 메소드에서 해당 비지니스 로직과 그 로직을 수행하기 위한 데이터만 넘겨주는 역할에 그쳐야합니다.

따라서, Controller에서의 테스트는 url로 post, get 등 method가 requestMapping이 잘 되고, 그 반환값이 정상적으로 들어오는지 확인하면 됩니다. 별도로 여러 예외상황에 대해서도 테스트 하면 됩니다.


### 테스트 예제는 추후에 추가 예정


# 2. Service
Service는 Controller에 의해 비지니스 로직을 수행합니다. 여러 Dao를 Autowired 하여 비지니스 로직을 수행합니다. Controller가 웹 세상이라면 Service는 POJO(plain old java object )세상입니다. 즉, Controller가 없이 Main을 생성하여 Service에 바로 붙일 수 있게 분리되어야 합니다. Service에서는 원하는 비지니스 로직이 잘 수행되는지 확인하면 됩니다.

비지니스 로직을 수행할 때, Dao를 쓰게 된다면, 테스트용 내장 데이터베이스를 쓰거나 when문으로 Dao에서 호출하는 함수 값을 임의로 지정하고 진행 할 수 있습니다.
테스트를 실행시킬 때마다 DB를 바꿔야 한다면, pom.xml 파일에서 build태그 안에 다음을 추가시켜 주면 된다.

{% highlight yaml %}
<testResources>
    <testResource>
        <directory>src/test/resources</directory>
    </testResource>
</testResources>
{% endhighlight %}

그리고 테스트 하는 클래스 위에 

{% highlight yaml %}
@RunWith(MockitoJUnitRunner.class)
@ContextHierarchy({
    @ContextConfiguration("/spring/application-context.xml"),
    @ContextConfiguration("/spring/servlet-context.xml")
})
public class QuestionServiceTest {
    ...
}
{% endhighlight %}

다음을 추가 시켜주면, src/test/resources 위치에서 /spring/application.context.xml을 로딩하게 된다. 저 디렉토리 위치에 테스트용 디비 설정을 적어두면 매번 디비 설정을 바꿀 필요 없다. 디비를 분리하는 이유는 오늘 성공한 테스트가 내일 성공할 거라는 보장이 없습니다. 데이터가 바뀌게 되면 테스트가 실패할 수도 있기 때문에 테스트용 디비를 따로 구축하는 것이 좋습니다.

{% highlight yaml %}
@RunWith(MockitoJUnitRunner.class)
public class QuestionServiceTest {
    @Mock
    private QuestionDao questionDao;

    @Before
    public void setUp(){
        questionList = new ArrayList<QuestionVo>();
        question = new QuestionVo().setQuestionId(testQuestionId);
        questionList.add(question);
    }
    
    @Test
    public void getQuestionsTest(){
        when(questionDao.getQuestions(testStartIndex, testCount)).thenReturn(questionList);
        when(itemDao.getItemList(testQuestionId)).thenReturn(itemList);
        when(hashDao.getHashList(testQuestionId)).thenReturn(hashList);
        
        assertThat(questionService.getQuestions(testStartIndex, testCount, testUserId).get(0),equalTo(question));
    }
{% endhighlight %}

다음은 Question을 가져오는 Service에 대한 테스트 코드 입니다.
@Mock 어노테이션을 통해 사용할 dao를 정의해주고, when(questionDao.getQuestions(~~) ).thenReturn(questionList) 문을 통해 questionDao.getQuestion가 실행 시, questionList를 리턴하게 설정해 주었습니다. when문을 사용하면, Service를 테스트 할때, dao를 사용하는 함수의 리턴 값을 미리 정의해주었기 때문에 디비에 접근하지 않아도 테스트가 가능하다.

# 3. Dao

{% highlight yaml %}
@RunWith(SpringJUnit4ClassRunner.class)
@ContextHierarchy({
    @ContextConfigurat
    ion("/spring/application-context.xml"),
    @ContextConfiguration("/spring/servlet-context.xml")
})
@Transactional
@TransactionConfiguration(defaultRollback=true)
public class QuestionDaoTest {
    
    @Autowired
    QuestionDao questionDao;
    
    @Test
    public void updateVoteNumUpTest(){
        assertThat(questionDao.updateVoteNumUp(1), equalTo(1));
        assertThat(questionDao.updateVoteNumUp(10), equalTo(1));
        assertThat(questionDao.updateVoteNumUp(100), equalTo(0));
    }
    
    @Test
    public void updateVoteNumDownTest(){
        assertThat(questionDao.updateVoteNumDown(1), equalTo(1));
        assertThat(questionDao.updateVoteNumDown(10), equalTo(1));
        assertThat(questionDao.updateVoteNumDown(100), equalTo(0));
    }
{% endhighlight %}

Dao 테스트는 디비에서 select, update, insert 등 데이터가 바뀌게 되므로, 항상 테스트 이후에 rollback 설정을 걸어주는 것이 좋다.
위에 코드와 같이 

{% highlight yaml %}
@Transactional
@TransactionConfiguration(defaultRollback=true)
public class QuestionDaoTest {
    ...
}
{% endhighlight %}

설정으로 테스트 후에 데이터가 변하지 않고 일관되게 유지해야 테스트 코드가 바뀌지 않는다.
