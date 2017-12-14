---
layout: post
title: npm scoped package
category: tool
tag: [npm] 
---

npm 패키지 중 @types/react처럼 앞에 @가 붙고 /로 구분되어있는 패키지가 있다. 이런 패키지를 `scoped package`라고 한다. 


@scope/project-name 구조이고 @scorp는 namespace역할을 한다.

각 npm 사용자는 각자의 scope를 username으로 가진다고 한다.(아마도 패키지를 제작해서 배포할 때 필요한 내용인 것 같다.)

패키지 명이 겹치는 것을 방지하기 위해 namespace으로 사용된다고 생각하면 될 것 같다.

- 참고
  - [https://docs.npmjs.com/getting-started/scoped-packages](https://docs.npmjs.com/getting-started/scoped-packages)
 
