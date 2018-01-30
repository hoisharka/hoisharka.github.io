---
title: 이전 커서 위치로 이동하기
layout: post
categories: [tool]
tags: [emacs]
---

## 현재 버퍼에서 이전 커서 위치로 이동
  - mark-ring에 커서 위치가 저장되며 mark-ring은 하나의 버퍼에 하나씩 존재한다.
  - C-u C-SPC -> 현재 버퍼에서 이전 커서 위치로 이동한다.
## 모든 버퍼 범위에서 이전 커서 위치로 이동
  - global-mark-ring에 커서 위치가 저장되며 전역적으로 하나만 존재한다..
  - C-x C-SPC -> 모든 버퍼에 범위에서 이전 커서 위치로 이동한다.

## 참고
  - [ergoemacs.org > Emacs: How to Bind Super Hyper Keys](http://ergoemacs.org/emacs/emacs_hyper_super_keys.html)
