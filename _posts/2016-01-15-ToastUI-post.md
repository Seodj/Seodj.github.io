---
layout: post
title: window에서 Toast UI 컴포넌트 설치하기
categories: [TUI]
tags: [NHN ent]
description: window에서 Toast UI 컴포넌트 설치하고 기본 예제 해보기
---


##1. node.js 설치!

https://nodejs.org/ 

{% highlight yaml %}
$node -v
{% endhighlight %}

cmd창에 $node 명령어가 실행되면 설치 완료.


##2. bower 설치

{% highlight yaml %}
$npm i --global bower
{% endhighlight %}

npm 명령어로 간단하게 bower 설치 완료.

{% highlight yaml %}
$bower -v
{% endhighlight %}

cmd창에 $bower 명령어가 실행되면 설치 완료.


##3. Toast UI 컴포넌트 설치

https://github.com/nhnent/tui.code-snippet

첫 번째 방법으로 github에서 다운받아 사용 가능하다.


{% highlight yaml %}
$bower install tui-code-snippet
{% endhighlight %}

두 번째 방법으로 bower을 이용하여 위에 명령어로 설치한다.
원하는 디렉토리로 이동하여 위에서 cmd 명령어 실행한다.


##4. 적용 테스트

{% highlight yaml %}
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8"/>
        <script type="text/javascript" src="./js/jquery.min.js"></script>
        <script type="text/javascript" src="./js/code-snippet.js"></script>
        <script type="text/javascript" src="./js/calendar.min.js"></script>
    </head>
    <body>
    	<div id="layer"></div>
    </body>
    <script>
   	 	var calendar = new tui.component.Calendar("#layer");
	</script>
</html>
{% endhighlight %}


위에 코드와 같이 적용해보면 다음 사진과 같은 결과를 얻을 수 있다.

![calendar image]({{ site.BASE_PATH }}/assets/post/2016-01-15/calendar.jpg)

### ※주의할 점은 github 설명에 CodeSnippet은 기본적으로 ne.util을 네임스페이스로 가진다고 했는데, ne.util이 아니라 tui.util로 변경된 것이 문서에는 적용이 되지 않았습니다.
아래 예제 처럼 ne.component.Calendar("#layer"); 가 아니라 tui.component로 작성해주세요!

{% highlight yaml %}
var calendar = new tui.component.Calendar("#layer");
{% endhighlight %}
