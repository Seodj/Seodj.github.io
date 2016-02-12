---
layout: post
title: linux 톰캣, jdk설치 및 포트 설정
categories: [apache,server]
tags: [linux, server, tomcat]
description: linux 톰캣, jdk설치 및 포트 설정.
---

# 1. 톰캣 설치(tomcat8.0 설치)

사진처럼 받고 싶은 톰캣 버전의 주소를 복사 한다.
![tomcat_setup image]({{ site.BASE_PATH }}/assets/post/2016-02-12/tomcat_setup1.jpg)

자신이 원하는 위치에 download 디렉토리를 만들고 다음 명령을 실행한다.

{% highlight yaml %}
$ wget http://mirror.apache-kr.org/tomcat/tomcat-8/v8.0.32/bin/apache-tomcat-8.0.32.tar.gz
{% endhighlight %}

위에 명령어가 잘 안 될시, 아래 명령어로 설치해 준다.

{% highlight yaml %}
$ wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u72-b15/jdk-8u72-linux-x64.tar.gz"
{% endhighlight %}

<a class="btn btn-default" href="http://tecadmin.net/install-java-8-on-centos-rhel-and-fedora/#">아파치 톰캣 설치 참조 사이트 바로 가기</a>

아파치 톰캣 압축 파일을 모두 다운 받고 난 뒤 다음과 같이

{% highlight yaml %}
$ tar xzvf apache-maven-3.3.9-bin.tar.gz
{% endhighlight %}

압축을 풀린 톰캣 파일을 원하는 디렉토리에 이동 시킨다.

{% highlight yaml %}
$ ln -s apache-tomcat-8.0.32 tomcat
{% endhighlight %}

위에 명령어를 시키면, 톰캣 폴더를 가르키는 폴더가 생성된다. 바로가기와 유사하다.

{% highlight yaml %}
$ vi ~/.bashrc
{% endhighlight %}

<span style="color:#ff0000">bashrc 파일을 수정하였으면 반드시, 아래 명령어를 실행시켜야 적용된다.</span>

{% highlight yaml %}
$ source ~/.bashrc
{% endhighlight %}

위에 명령어를 실행 시켜, 다음과 같이 TOMCAT_HOME을 설정해준다.

{% highlight yaml %}
# .bashrc

# Source global definitions
if [ -f /etc/bashrc ]; then
        . /etc/bashrc
fi

        export APP_HOME=/home1/irteam
        export JAVA_HOME=${APP_HOME}/apps/jdk
        export APACHE_HTTP_HOME=${APP_HOME}/apps/apache
        export TOMCAT_HOME=${APP_HOME}/apps/tomcat
        export MVN_HOME=${APP_HOME}/apps/maven
        export PATH=${JAVA_HOME}/bin:$PATH
        export PATH=${APACHE_HTTP_HOME}/bin:$PATH
        export PATH=${MVN_HOME}/bin:$PATH

{% endhighlight %}

이제 톰캣의 bin 폴더로 이동하여, startup.sh 을 실행하고, 브라우져에 http://서버 ip:8080 를 입력하면 톰캣 첫 화면이 나온다.


# 2. jdk8 설치

사진처럼 받고 싶은 자바 버전의 주소를 복사 한다.
![jdk_setup image]({{ site.BASE_PATH }}/assets/post/2016-02-12/jdk_setup.jpg)

1번 톰캣을 설치한 것과 마찬가지로, 압축 풀고 PATH까지 잡아주면 완료.

{% highlight yaml %}
$ java -version
{% endhighlight %}

위에 명령어를 통해 jdk8이 제대로 설치됬는지 확인한다.

# 3. apache 포트 설정

<a class="btn btn-default" href="http://nhnent.dooray.com/task/projects/%EC%84%9C%EB%8F%99%EC%A7%84-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8/3?filterStoreMode=true">아파치 톰캣 연결 설정(서경원 동기 글)</a>

apache/conf 디렉토리에서 아래 파일로 이동한다.

{% highlight yaml %}
$ vi httpd.conf
{% endhighlight %}

파일을 쭉 내리다 보면, 다음과 같은 IfModule 태그를 볼 수 있다.
JKMount /* tomcat 라고 적은 것은 어떤 경로든 모두 tomcat에서 처리하겠다는 의미이다.

{% highlight yaml %}
<IfModule mod_jk.c>
JKMount /* tomcat
JkMount /*.jsp tomcat
JkMount /*.nhn tomcat
....
    <Location /jkmanager/>
    ...
    </Location>
...
</IfModule>
{% endhighlight %}

현재는 :8080 포트에서만 톰캣에서 처리하는데, 80포트에서 톰캣을 처리하게 바꾸어주어야 한다.

톰캣에 있는 server.xml 포트와 apache/conf/workers.properties 파일에 worker.tomcat.port 를 같게 해주면 80포트로 request를 날려도 톰캣으로 처리한다.
http://서버 ip 를 입력하여 직접 테스트 해보자.

# 4. maven 설치하기.

사진처럼 받고 싶은 maven 주소를 복사 한다.
![maven_setup image]({{ site.BASE_PATH }}/assets/post/2016-02-12/maven_setup.jpg)

1번 톰캣을 설치한 것과 마찬가지로, 압축 풀고 PATH까지 잡아주면 완료.

{% highlight yaml %}
$ mvn -help
{% endhighlight %}

위에 명령어를 통해 maven이 제대로 설치됬는지 확인한다.


# 5. git 프로젝트 톰캣에 올리기.

자신이 원하는 위치에 project 디렉토리를 만들고 다음 명령을 실행한다.

{% highlight yaml %}
$ git clone "자신의 github 복사한 주소"
{% endhighlight %}

git을 복사한 디렉토리로 이동하여, 아래 명령어를 실행하여 build한다.

{% highlight yaml %}
$ mvn package
{% endhighlight %}

target 디렉토리에 이동하면, war 파일이 생성되어 있다.

그 파일을 톰캣/webapps 에 이동시켜, ROOT.war 파일에 덮어 쓴다.
이후 정상적으로 올라갔는지 http://서버 ip 를 입력하여 직접 테스트 해보자.