---
layout: post
title: window 환경에서 npm update하기 
category: npm
tag: [npm] 
---

윈도우 환경에서 npm을 업데이트 하는 방법은 리눅스나 맥OS와 좀 다르다. 

순서대로 적어보겠다.

## 설치 순서
1. 관리자 권한으로 cmd를 실행 한다.
2. `npm-windows-upgrade`라는 npm 패키지를 설치한다.
  ```bash
  $ npm install npm-windows-upgrade
  ```
3. `npm-windows-upgrade`를 실행한다.
  ```bash
  $ npm-windows-upgrade
  ```
4. 버전 목록이 나오면 원하는 버전을 선택해서 설치한다.

- 참고
  - [stackoverflow > How do I update Node.js and npm on Windows?](https://stackoverflow.com/questions/18412129/how-do-i-update-node-js-and-npm-on-windows)



