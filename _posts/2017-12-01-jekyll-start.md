---
layout: post
title: 지킬 시작하기
categories: jekyll
tags: jekyll
---
어떤 경험이든 정리해 놓지 않으면 완벽히 내것이 되지 않는 것 같다는 생각이 들었다.
블로그를 하면서 경험하는 모든 것을 정리해 놓기로 결정했다.
블로그를 만드는 방법은 여러가지가 있겠지만 Markdown으로 문서를 쉽게 작성하면서 커스터마이징이 자유로우며 github page를 통해 무료로 호스팅할 수 있는 jekyll을 선택하게 되었다.

  > 이 포스트를 작성할 당시 이미 hoisharka.github.io에 블로그가 생성되어 있었지만 블로그 생성과정을 다시 정리해보기 위해 다른 아이디로 다시 생성해보았다. 

## 지킬이란
- 깃허브에서 제공하는 정적 사이트 생성기이다.
- Markdown으로 작성한 문서를 템플릿 상에서 보여준다. 
- Liquid라는 템플릿 언어를 사용해서 탬플릿을 자유롭게 커스터마이징 할 수 있다.

## 설치
지킬은 Windows 환경에서도 사용 가능하지만 Mac OS나 리눅스 환경을 권장하고 있는 것으로 보인다. 실제로 예전에 windows 환경에서 설치해본 경험에 의하면 설치도 다른 OS에 비해 번거롭다.

  > Windows는 공식 지원하지 않는다.
  ![Windows는 공식 지원하지 않는다.]({{ site.url }}/assets/jekyll-start-001.png)


우분투 환경에서는 apt 패키지 관리자를 통해 필요한 패키지를 설치하는 것이 간단하기 때문에 우분투(v17.10) 환경에서 작업했다. 

## ruby(gem포함), jekyll, git 설치 
{% highlight bash %}
$ sudo apt-get install ruby-full
$ sudo gem install jekyll
$ sudo gem install git
{% endhighlight %}

## jekll 테마 선택 및 Fork

### 테마 고르기 
[지킬 테마 사이트](http://jekyllthemes.org/)에 접속해서 원하는 테마를 선택한다. 

*나는 [hydeout](http://jekyllthemes.org/themes/hydeout/)이라는 테마를 선택했다.* 
![ubuntu_base_setting_002.png]({{ site.url }}/assets/jekyll-start-002.png)

### 테마 저장소 Fork하기
Homepage버튼을 누르면 깃허브 저장소로 이동한다.
Fork 버튼을 눌러 프로젝트를 Fork한다. 
![hydeout 깃허브 저장소]({{ site.url }}/assets/jekyll-start-003.png)

### 저장소명 변경 
Fork가 완료되면 Settings로 이동해서 저장소 이름을 바꿔준다.
![Fork]({{ site.url }}/assets/jekyll-start-004.png)


저장소 이름은 `[username].github.io`로 지정하고 Rename버튼을 누르면 저장소 명이 변경된다.
![저장소 명 변경]({{ site.url }}/assets/jekyll-start-005.png)

### git page 확인 
변경한 주소로 접속하면 git page가 뜨는 것을 확인할 수 있다.
![git page 접속]({{ site.url }}/assets/jekyll-start-006.png)
 
## 로컬 작업 환경 만들기 

### 저장소 소스 clone 
저장소 소스를 로컬로 clone해 온다.
나는 ~/blog 디렉토리를 만들어 저장소를 clone했다. 
{% highlight bash %}
$ cd ~
$ mkdir blog
$ cd blog
$ git config --global user.name syowow
$ git config --global user.email syowow@gmail.com
$ git clone https://github.com/syowow/syowow.github.io.git
{% endhighlight %}

### 로컬에서 지킬 실행
clone한 디렉토리에서 바로 jekyll serve를 할 경우 다음과 같은 에러가 발생할 수 있다.
![jekyll server error]({{ site.url }}/assets/jekyll-start-007.png)

아마도 테마 소스가 이전 버전의 지킬을 사용하기 때문에 버전이 안 맞아서 나타나는 에러인 것 같다. 

  > jekyll new 명령어를 통해 처음 시작할 경우엔 `jekyll serve`만으로도 동작한다.*
  ![jekyll server error]({{ site.url }}/assets/jekyll-start-008.png)

{% highlight bash %}
$ cd syowow.github.io
$ sudo gem install bundler
$ bundler install
$ jekyll serve --watch
{% endhighlight %}

실행 결과는 localhost:4000으로 접속해서 확인할 수 있다.
![jekyll server error]({{ site.url }}/assets/jekyll-start-009.png)
