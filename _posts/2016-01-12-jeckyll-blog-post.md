---
layout: post
title: jekyll로 블로그 만들기!
categories: [general]
tags: [jekyll]
description: jekyll 만드는 방법에 대해서 처음부터 상세하게 소개합니다.
---


## 1. 루비 설치!

http://rubyinstaller.org/downloads/

![ruby setup]({{ site.BASE_PATH }}/assets/post/2016-01-12/ruby_setup.jpg)

위에 사진 처럼 Path 자동 설정하면 편합니다!



## 2. development kit 설치

http://rubyinstaller.org/downloads/  
편한 위치에 설치 후, 층로 디렉토리 이동

Devkit을 사용하게 초기화 해야합니다. 윈도우 CMD 창에서 아래 명령으로 초기화와 Ruby 와 Binding 을 해줍니다.

{% highlight yaml %}
$cd C:\RubyDevkit  
$ruby dk.rb init  
$ruby dk.rb install
{% endhighlight %}


![development error]({{ site.BASE_PATH }}/assets/post/2016-01-12/development_error.jpg)


위에 문제 발생 시 루비 경로를 `config.yml` 파일에 추가해야 한다.(아래 사진 참조.)
 

 ![development_config]({{ site.BASE_PATH }}/assets/post/2016-01-12/development_config.jpg)

 ![development_success]({{ site.BASE_PATH }}/assets/post/2016-01-12/development_success.jpg)
 

이러면 성공!



## 3. 지킬 설치

{% highlight yaml %}
$gem install jekyll
{% endhighlight %}

지킬 설치 완료!

{% highlight yaml %}
$gem install rouge
{% endhighlight %}

로우지 설치 완료!


## 4. 파이썬 설치
https://www.python.org/downloads/  
파이썬은 2.x 버전을 추천합니다. 3.x는 뒤에서 에러발생..

환경변수 path에 C:\Python27 경로 추가   
추가로 C:\Python27\Scripts 도 추가!  
즉 path에 C:\Python27;C:\Python27\Scripts  
이렇게 추가!  

{% highlight yaml %}
$python 
{% endhighlight %}


명령어 실행되면 설치 성공!  
Pip는 파이썬 설치할 때 자동으로 설치됨  


## 5. Pygments 설치  
{% highlight yaml %}
$pip install Pygments
{% endhighlight %}
윈도우 계정이 한글로 되어있으면 오류 발생할 수 있습니다.  


## 6. Jekyll 실행
원하는 디렉토리 하나 생성 후

{% highlight yaml %}
$jekyll serve 
{% endhighlight %}

![jekyll_serve]({{ site.BASE_PATH }}/assets/post/2016-01-12/jekyll_serve.jpg)
 
Jekyll가 실행되지만 wdm 설치하라고 나옵니다.

{% highlight yaml %}
$gem install wdm 
{% endhighlight %}

으로 wdm 설치!

다시 jekyll을 실행하면 정상 실행!  
실행시키고 브라우져에 127.0.0.1:4000 입력.

만약 Jekyll 서버 실행 시 다음과 같은 에러 발생한다면,

{% highlight yaml %}
C:/Ruby22/lib/ruby/2.2.0/rubygems/coreext/kernelrequire.rb:54:in require': ca
nnot load such file -- hitimes/hitimes (LoadError)
        from C:/Ruby22/lib/ruby/2.2.0/rubygems/core_ext/kernel_require.rb:54:in
require'
        from C:/Ruby22/lib/ruby/gems/2.2.0/gems/hitimes-1.2.2-x86-mingw32/lib/hi
times.rb:37:in rescue in <top (required)>'
…
{% endhighlight %}


{% highlight yaml %}
$gem uni hitimes
$Remove ALL versions
$gem install hitimes -v 1.2.1 --platform ruby
{% endhighlight %}

Hitimes 삭제 후 다시 설치

127.0.0.1:4000 접속이 잘 됬다면 성공!



## 7. jekyll 블로그 생성하기
$jekyll new github아이디.github.io
Blog 디렉토리로 이동 후, _config.yml 파일에 밑에 설정 추가

{% highlight yaml %}
encoding: utf-8
highlighter: rouge
highlighter: pygments
{% endhighlight %}

다시 jekyll 실행
 
![jekyll_error]({{ site.BASE_PATH }}/assets/post/2016-01-12/jekyll_error.jpg)


위에 오류 발생 시, 아래 명령어로 설치

{% highlight yaml %}
$gem install pygments.rb
{% endhighlight %}

![jekyll_error2]({{ site.BASE_PATH }}/assets/post/2016-01-12/jekyll_error2.jpg)

 
위 에러 시 pygments를 재설치 해본다.
{% highlight yaml %}
$gem uninstall pygments.rb
$gem install pygments.rb --version "=0.5.0"
{% endhighlight %}

Pygments.rb 재설치 해보고 안된다면, 파이썬이 3.x 버전이어서 안되는 가능성이 높음.
http://127.0.0.1:4000/
다음과 같이 나오면 성공

![jekyll_success]({{ site.BASE_PATH }}/assets/post/2016-01-12/jekyll_success.jpg)


## 8. git 설치
http://git-scm.com/download/win
github에서 레퍼지토리 새로 생성

![git_setup]({{ site.BASE_PATH }}/assets/post/2016-01-12/git_setup.jpg)

git 설치하고 본인이 만든 C:\jekyll\github아이디.github.io 디렉토리 이동 후,

{% highlight yaml %}
$git init
$git remote add origin https://github.com/본인아이디/본인아이디.github.io.git
$git config –global user.email “본인이메일”
$git config –global user.name “본인이름”
$git add .
$git commit –m “my blog start”
$git push –u origin master
{% endhighlight %}

이후 본인 블로그 주소로 접속!
https://본인아이디.github.io 입력해서 정상적으로 나오면 성공!


## 9. 블로그 꾸미기

http://jekyllthemes.org/
에서 무료 themes를 배포하니 여기서 다운받아서 적용하면 됩니다!