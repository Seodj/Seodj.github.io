---
layout: post
title: OAuth에 관하여
categories: [OAuth]
tags: [NHN ent]
description: OAuth에 관하여 정리합니다.
---

##1. OAuth

1. Authentication
- 인증

2. Authorization
- 권한

OAuth는 2번인 권한 부여 의미 조금 더 맞다.


##2. 용어 정리

1. Resource Server
- 자원을 제공하는 서버. Client는 Access token을 이용하여 자원에 접근할 수 있다. (ex.페이스북,사진,비디오 등)

2. Resource Owner
- 사용자.

3. Client
- Resource Server의 자원을 사용하는 애플리케이션(ex. 페이스북의 뉴스를 모아서 보여주는 앱) => 

4. Authorization Server
- 사용자의 동의를 받아서 권한을 부여하는 서버. 일반적으로 Resource Server 와 같은 URL 하위에 있는 경우가 많음.
- 인증을 수행하는 서버로 로그인 및 접근허가 여부를 검사해서 Authorization code와 Access token을 발행.




{% highlight yaml %}
OAuth 작성 중
{% endhighlight %}


