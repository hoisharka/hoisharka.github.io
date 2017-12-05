---
layout: post
title: Git CRLF와 LF 이슈
category: git
tag: git
---

윈도우즈에서는 줄바꿈 문자가 CRLF이고 리눅스나 맥에서는 LF이다. 서로 다른 OS를 사용하는 사람끼리 협업할 때 줄바꿈 문자가 통일되지 않아 문제가 되는 경우가 있다. 

나는 vmware상의 리눅스에서 블로그 포스트를 작성하다가 vmware가 너무 느려서 windows로 다시 넘어왔을 때 이런 문제가 생겼다. 언제 또 내가 리눅스 환경에서 작업하고 싶어서 다른 우분투 노트북을 사용할지 모르기 때문에 이 문제를 해결해 놓고 싶어서 구글링해봤다.

# core.autocrlf 변경
Git에 자동으로 CRLF를 LF로 변환해주고 반대로 Checkout할 때 LF를 CRLF로 변환하는 기능이 있다고 한다.
로컬 저장소에서 다음 명령어를 실행하면 된다.

```bash
$ git config --global core.autocrlf input
```


- 참고
  - [http://handam.tistory.com/127](http://handam.tistory.com/127)
