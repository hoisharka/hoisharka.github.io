---
title: 윈도우에서 타이틀바 보이기/숨기기
layout: post
categories: [tool]
tags: [emacs, windows]
---

실수로 눌렀는지 모르겠지만 갑자기 윈도우의 타이틀바가 없어지는
현상이 발생했다. 크기 조절이나 창 위치 이동을 마우스로 할 수 없게
되자 불편해서 다시 타이틀바가 나오도록 하는 설정을 찾아봤다.

스택오버플로우에서 [How can I customize the Windows GNU Emacs title
bar?](https://stackoverflow.com/questions/21264185/how-can-i-customize-the-windows-gnu-emacs-title-bar?lq=1) 라는 글을 찾았고 내용중에 \`toggle-frame-fullscreen\` 이란 명령어를
쓰라는 내용을 발견했다.

    M-x toggle-frame-fullscreen

이 기능을 실행하자 바로 타이틀바가 나타났다. 단축키는 [F11]이라고
한다. 아마도 [F12]를 누르다가 잘못해서 [F11]을 눌렀었나보다.
