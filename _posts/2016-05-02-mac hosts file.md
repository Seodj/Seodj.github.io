---
layout: post
title: Mac에서 hosts 파일 변경하기.
categories: [tip]
tags: [mac, tip]
description: Mac에서 localhost를 이용하지 않고, hosts 파일 변경하는 방법.
---

# Mac hosts 파일 변경하는 방법.
1. terminal을 실행한다.
2. $sudo nano /private/etc/hosts 파일에 들어간다.
3. 패스워드를 입력 후, 다음과 같은 구조로 변경한다.

```
[ip 주소] tab url
202.179.177.22:80	hello.naver.com
```

4. ctrl + o 누른 뒤, 엔터
5. ctrl + x 로 종료.

![hosts_terminal]({{ site.BASE_PATH }}/assets/post/2016-05-02/terminal.png)

위에 예시와 같이, host파일을 변경하여 네이버를 hello.naver.com 도메인으로 접속해보자.

6. $dscacheutil -flushcache 로 적용.

7. 사용한 도메인으로 정상적으로 작동하는지 확인.

![helloNaver]({{ site.BASE_PATH }}/assets/post/2016-05-02/helloNaver.png)

hello.naver.com으로 네이버에 접속한 것을 확인할 수 있다.

끝!
