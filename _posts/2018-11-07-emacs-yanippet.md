---
title: emacs yanippet 다시 설정하기 
layout: post
categories: [tool]
tags: [emacs, yasnippet]
---
지킬 블로그 포스팅을 작성할 때 적어야하는 메타 데이터를 yasnippet으로
작성해 둔 적이 있었다.

최근에 emacs 최신 버전으로 설치한 후 init 파일을 다시 정리하고
있어서 이전에 설정해 놓은 포스트 메타 데이터를 자동 생성하는
yasnippet이 날아가버렸다.

어떻게 설정했는지 기억이 안나서 내가 올려놓은 포스팅을 보고
따라해봤다. [https://hoisharka.github.io/tool/2018/03/13/emacs-yasnippet/][emacs yasnippet 사용하기] yasnippet 등록까지만 되고 설정해놓은 key를 입력하고 탭을
눌러도 반응이 없었다.

다시한번 yanippet을 설치하는 설정부터 알아봤다. init.el에 아래 설정을
추가하면 된다.  yanippet 패키지에 yasnippet-snippets 패키지까지
설치한다.  yasnippet-snippets 패키지는 이미 만들어놓은 여러 snippet들을
모아놓은 패키지이다.

yanippet 설정. 
```lisp
(use-package yasnippet
  :ensure t
  :init
    (yas-global-mode 1)
  :config
  (use-package yasnippet-snippets
    :ensure t)
  (yas-reload-all))
```

설정 추가 후 `C-x Ce`로 코드를 실행시킨다. 패키지들이 깔려있지 않다면
설치가 진행될 것이다.  만약 설치 시 에러가 난다면 `M-x
package-refresh-contents`를 실행하여 패키지목록을 갱신해준다. 그래도
에러가 나면 package-archives 설정을 다시 확인해봐야될 것이다.

다음은 markdown 설정이다.

markdown 설정
```lisp
(use-package markdown-mode
  :ensure t
  :commands (markdown-mode gfm-mode)
  :mode (("README\\.md\\'" . gfm-mode)
         ("\\.md\\'" . markdown-mode)
         ("\\.markdown\\'" . markdown-mode))
  :init (setq markdown-command "multimarkdown"))
```
설정 추가 후 `C-x Ce`로 코드를 실행 시킨 후 [https://hoisharka.github.io/tool/2018/03/13/emacs-yasnippet/][emacs yasnippet 사용하기] 에서 등록했던 jph(jekyll post header를 약자로 해서 지음)를 .md 확장자 파일에서 실행했다.

정상적으로 snippet이 자동완성 되었다. 

추가로 yasnippet-snippets 패키지에 포함된 이미 만들어져있는 snippet을
사용하는 방법을 적어본다.

`M-x yas-describe-tables` 으로 snippet 목록을 확인할 수 있다.

![emacs-yasnippet-snippets.png]({{ site.url }}/assets/emacs-yasnippet-snippets.png)

내가 작성한 jph를 포함해서 다양한 snippet 목록을 확인할 수
있다. 사용할 snippet의 key를 타이핑하고 탭을 누르면 해당 snippet의
내용이 자동 완성된다. 

유용한 snippet이 있는지 살펴보고 익숙해지면 좋을 것 같다.

## 참고 
  - [Emacs Tutorial 20 - Yasnippet!](https://www.youtube.com/watch?v=39u8K12rXHE) 
  - [Markdown Mode for Emacs](https://jblevins.org/projects/markdown-mode/) 


