---
layout: post
title: emacs configuration
category: tool
tag: [emacs]
---

이맥스를 처음 설치하면 UI가 멋있지도 않고 무슨 유용한 설정이 있는지도 잘 파악하기 힘들다.[Emacs configuration in 24 minutes](https://www.youtube.com/watch?v=FRu8SRWuUko)라는 강좌를 보고 emacs의 초기 설정 방법을 정리한다.

## 주의 사항
- terminal 버전과 gui버전
  - 두가지는 다르게 동작한다. 예를들어 키 바인딩 설정 시 C->는 terminal에서 제대로 동작하지 않는다. >을 누르려면 Shift키를 함께 눌러야하는데 그것이 내부에 전달되지 않는 모양이다.
  - 강좌에서는 gui 버전을 사용했고 나도 gui버전을 계속 사용하기로 했다.
- 강좌에선 ~/.emacs파일에 설정하는 걸로 나오는데 나는 ~/.emacs.d/init.el에서 작업을 했다.
- 모든 설정마다 함수 단위로 즉시 실행시켜보려면 C-x C-e 키를 사용하면 된다.

## 테마 설정
load-theme 함수로 wombat라는 테마를 설정한다.
```lisp
(load-theme 'wombat)
```

## 기본 설정 

```lisp
(setq frame-title-format "emacs") ;;해더 타이틀을 emacs로 변경
(menu-bar-mode -1) ;; 메뉴바 숨김
(tool-bar-mode -1) ;; 툴바 숨김
(scroll-bar-mode -1) ;; 스크롤 바 숨김
(set-default 'cursor-type 'hbar) ;; 커서는 언더바로 표시 
(column-number-mode) ;; 컬럼 번호 표시
(show-paren-mode) ;; 매칭되는 괄화를 표시
(global-hl-line-mode) ;; 현재 라인에 하일라이트 효과
```
## ido-mode
이맥스 하단의 미니버퍼에서 파일명을 입력할 때 자동완성 기능을 제공한다. 기능이 다양하겠지만 일단 자동완성, 최근 파일명 자동 입력 기능만 사용해도 편리하다. 자세한 기능은 나중에 시간날 때 자세히 알아보기로 한다.

ido-mode를 다룬 블로그가 있어 링크를 걸어둔다.
[[Emacs] ido - Interactive Do Things](http://seorenn.blogspot.kr/2011/03/emacs-ido-interactive-do-things.html)
ido-mode
```lisp
(ido-mode) ;; ido-mode를 활성화
```
## window 작업 관련(기본 패키지로 설정)
```lisp
(winner-mode) ;; winner-mode 활성화
(windmove-default-keybindings) ;; shift + 방향키로 창 이동
```
- winner모드가 활성화되면 분할장 구조 변경 시 이전 상태로 돌아올 수 있도록 한다.
- windmove-default-keybindings가 활성화되면 Shift + 방향키로 분활창 간의 커서 이동이 가능하게 한다.

- 참고
  - [Winner Mode](https://www.emacswiki.org/emacs/WinnerMode)
  - [Wind Move](https://www.emacswiki.org/emacs/WindMove)
  
## 패키지 아카이브 설정
패키지 아카이브를 설정하면 손쉽게 패키지를 설치할 수 있다.

- 다음 설정을 하면 melpa와 marmalade라는 패키지 아카이브에서 패키지 정보를 받아올 수 있다.
```lisp
(require 'package)
(add-to-list 'package-archives
	         '("melpa" . "http://melpa.milkbox.net/packages/")
			 t)
(add-to-list 'package-archives
	         '("marmalade" . "http://marmalade-repo.org/packages/)
			 t)
(package-initialize)			 
```

- 패키지 아카이브 설정후 `M-x package-list-packages [RET]` 를 입력해서 패키지 목록을 불러온다.
- C-s를 눌러 검색 기능을 작동시킨 후 패키지 명을 검색한다. 
- 설치할 패키지명에 커서를 두고 i키로 설치할 패키지로 지정한다.
- x를 누르면 패키지가 설치된다.

## 자동완성 설정
- auto-complete, smex 패키지를 찾아서 설치한다.
- auto-complete
  - 코드 작성 시 자동 완성을 지원한다.
- smex
  - M-x로 실행하는 커맨드 입력 시 자동완성을 지원한다.
```lisp
  (global-set-key (kbd "M-x") 'smex)
  (global-set-key (kbd "C-c C-c M-x") 'execute-extended-command)
  (ac-config-default)
```
  
## 테마 변경
새 테마를 설치해서 기존 테마를 교체한다.
- M-x package-install [RET]
- monokai-theme [RET]
- M-: 로 미니버퍼에 Eval을 띄운 후 `(disable-theme 'wombat')`을 입력하면 위에서 설정했던 wombat 테마가 해제된다. 
- 설치한 monokai 테마로 교체한다.
```lisp
(load-theme 'monokai t)
```

## nlinum
라인 넘버를 표시한다.
- nlinum 패키지 설치
```lisp
(nlinum-mode) ;; nlium-mode 활성화
```

## autopair
여는 괄호를 입력하면 자동으로 닫는 괄호를 입력해준다.
- autopair 패키지 설치
```lisp
(autopair-global-mode) ;; autopair 활성화
```

## undo-tree
```lisp
(global-undo-tree-mode) ;; undo-tree-mode 활성화
(global-set-key (kbd "M-/") 'undo-tree-visualize) ;; undo-tree 보기
```
## switch-window
화면이 여러개일 때 번호를 매겨서 한번에 이동할 수 있도록 한다.
```lisp
(global-set-key (kbd "C-M-z") 'switch-window) ;; switch-window 키바인딩
```

## ace-jump-mode
```lisp
(global-set-key (kbd "C->") 'ace-jump-mode) ;; ace-jump-mode 키 바인딩 
```
- 실행하면 글자 하나를 입력받는다.
- 해당 글자 위치 각각을 알파벳 순서대로 변환하여 하이라이트해서 보여준다. -- 원하는 위치의 알파벳을 입력하면 커서가 이동된다.


## multiple-cursor
- multiple-cursor 패키지를 설치한다
- 키 바인딩
```lisp
(global-set-key (kbd "C-}") 'mc/mark-next-like-this)
(global-set-key (kbd "C-{") 'mc/mark-previous-like-this)
```

## powerline
mode-line을 VIM의 powerline처럼 보이게 한다. 이게 훨씬 이쁘다.
- powerline 패키지 설치
```lisp
(powerline-center-theme) ;; 테마 설정
(setq powerline-default-separator 'wave) ;; separator 모양 설정
```

