---
title: emacs yasnippet 사용하기
layout: post
categories: [tool]
tags: [emacs, snippet]
---

## yasnippet 설치
emacs elpa를 이용해 yasnippet을 설치한다.
```
M-x package-install yasnippet
```

## 새 snippet 만들기
`M-x yas-new-snippet`나 단축키 `C-c & C-n`로 new-snippet 화면으로 이동한다.
![등록 화면](/assets/emacs-yasnippet-001.png)

예제로 jekyll의 포스트 헤더를 작성해 보았다.
![jeyll 포스트 헤더](/assets/emacs-yasnippet-002.png)

key로 지정된 부분이 나중에 snippet을 호출할 때 사용하는 키가 된다.

snippet을 호출하면 $1 -> $2 -> ... -> $0 순서로 입력 가능하다. $1를 입력하고 tab을 누르면 $2영역으로 넘어가는 식이다.

## 테스트 및 저장
작성한 snippet을 테스트하려면 C-c C-t를 눌러서 테스트한다.
테스트가 완료되면 다시 snippet등록 화면으로 넘어가서 C-c C-c로 snippet을 저장한다.
snippet이 사용될 파일의 메이저 모드(jekyll 포스트의 경우 markdown-mode)와 snippet저장 경로를 지정하면 저장된다.

## 실행
지정한 모드에 해당하는 파일에서 지정 키를 입력한 뒤 tab을 누르면 snippet이 나타난다.

![실행화면](/assets/emacs-yasnippet-003.gif)


## 참고
  - [Emacs Tutorial: Yasnippet](https://www.youtube.com/watch?v=-4O-ZYjQxks)
  - [Emacs Wiki Yasnippet](https://www.emacswiki.org/emacs/Yasnippet)
