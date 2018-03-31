---
title: emacs에서 clojure 개발 환경 세팅(1)
layout: post
categories: [clojure]
tags: [emacs, clojure]
---
emacs에서 clojure 개발 환경을 세팅해봤다. windows에서 하려니 좀 까다로웠던 것 같다.
결국 [braveclojure.com](https://www.braveclojure.com/basic-emacs/)의 글을 보고 따라하면 세팅된다.
emacs설정 파일도 제공해주기 때문에 간단하게 설치된다.

아래 글은 그냥 삽질했던 것 몇가지 적어놓은 것이다.

나는 기존에 사용하던, 여태껏 emacs를 배우면서 삽질했던 결과를 보존하기 위해 기존 세팅을 기반으로 세팅하고 싶었고 삽질을 시작했다.

## leiningen-win-installer 설치
우선 [leiningen](https://github.com/technomancy/leiningen)을 설치해야한다. leiningen은 clojure 프로젝트에 대한 빌드 자동화와 의존성을 관리하는 툴이라고한다.

windows에서는 leiningen-win-installer로 설치할 수 있다.

![leiningen-win-installer](/assets/emacs-clojure-setting-001.png)

[https://djpowell.github.io/leiningen-win-installer/](https://djpowell.github.io/leiningen-win-installer/) 에서 다운받아 설치한다. 내 경우 jdk는 깔려있어서 패스하고 leiningen-win-installer만 설치했다.

설치 경로는 C:\Users\[사용자명]이 기본값이다. 나도 기본값으로 설치했다.

## leiningen-2.8.1-standalone.jar
내가 참고했던 youtube강좌에선 leiningen-win-installer를 설치하고 `lein --version`을 입력하면 lein버전이 출력된다. 그런데 내 경우엔 `leiningen-2.8.1-standalone.jar` 파일을 찾을 수 없다고 떠서 당황했다.

문제를 해결해보려고 이것저것 해봤는데 결국 해결한 방법은 이것이다.

1. [lein.sh](https://raw.githubusercontent.com/technomancy/leiningen/stable/bin/lein) 파일을 받는다.
2. HOME 디렉토리에 받은 파일을 위치 시킨다.
3. 쉘 스크립트이기 때문에 cmd가 아닌 git bash를 연다.
4. HOME 디렉토리로 이동해서 `./lein.sh` 명령어로 다운 받은 파일을 실행한다.
5. $HOME/.lein/self-install/경로에 leiningen-2.8.1-standalone.jar가 다운받아진 것을 확인할 수 있다.

아래는 lein.sh를 실행했을 때 나타나는 화면이다.
![lein.sh 실행 화면](/assets/emacs-clojure-setting-002.png)

여기까지 하면 `lein repl`명령어로 clojure예제를 실행할 수 있는 repl을 띄울 수 있다.
![repl](/assets/emacs-clojure-setting-003.png)

여기까지는 그냥 leiningen을 cmd에서 실행하는데 성공한 내용이다.

## git-bash에서 leiningen 실행
lein이 cmd에서는 실행됐지만 git-bash상에서는 실행되지 않았다. 나는 emacs에서도 git-bash를 기본 shell로 사용하고 있었기 때문에 git-bash에서 lein명령어가 되지 않으면 emacs에서도 사용할 수 없을 것 같았다.

그래서 검색한 결과 몇가지 세팅을 해주고 git-bash에서 lein을 실행할 수 있었다.

1. 홈디렉토리(C:/Users/[사용자명])에 `bin`디렉토리를 만들고 leiningen에서 제공하는 lein.sh파일을 넣어주었다.
2. git-bash를 열어 홈디렉토리 아래 .bashrc파일을 생성하고 vim으로 열어 내용을 아래처럼 적어준다.

``` bash
alias=lein.bat
```
3. git-bash를 재시작하면 git-bash에서 lein이 동작한다.

## clojure-mode & cider 패키지 설치
clojure-mode는 emacs에서 clojure 코딩을 하기 위해 필요한 패키지이다.
CIDER는 "Clojure(Script) Interactive Development Environment that Rocks!"의 약자로, emacs를 확장하여 clojure프로그래밍을 지원한다.

이맥스를 열고 clojure-mode와 cider를 설치해준다.

``` lisp
M-x package-install [RET] clojure-mode [RET]
M-x package-install [RET] cider [RET]
```

이렇게 설치하고 `M-x cider-jack-in [RET]`를 쳐주면 lein repl이 나와야 하는데... 에러가 발생했다.
그리고 검색하고 또 검색했지만 해결 방법을 찾을 수 없었다.


## 결론
앞에서 말했듯이 [braveclojure.com](https://www.braveclojure.com/basic-emacs/)의 글을 보고 따라하면 emacs에서 lein repl이 정말 쉽게 뜬다.
다음 포스팅에서 무슨 차이로 인해 내가 세팅했던 것이 정상동작하지 않았는지 알아볼거다.


## 참고
  - [clojure: Install lein( leiningen) on windows](https://www.youtube.com/watch?v=axOfYQg1VQw)
  - [https://github.com/clojure-emacs/cider](https://github.com/clojure-emacs/cider)
