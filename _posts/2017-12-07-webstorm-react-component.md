---
layout: post
title: webstorm에서 eslint 사용 시 react.component unresolved 문제
category: react
tag: [react, webstorm, eslint] 
---

Webstorm에서 eslint를 사용하면서 react를 개발할 때 아래와 같은 경고 메시지가 신경쓰였다. 왜 React.Component 선언을 못 읽을까?

![unresolved component]({{ site.url }}/assets/webstorm-react-component-001.png)

구글링을 하다가 다음과 같은 글을 발견했다.
![unresolved component]({{ site.url }}/assets/webstorm-react-component-002.png)
내용을 요약하면 Component와 PropTypes는 import한 React.js 쪽에 없고 ReactIsomorphic.js에 있다고 한다. 그래서 webstorm이 소스를 참조할 수 없다.

경고 문구가 거슬린다면 아래 명령어로 `@types/react`를 설치해서 webstorm이 해당 코드를 참조할 수 있게 해주면 된다.
```
npm --save install @types/react
```

* 참고
  - [https://intellij-support.jetbrains.com/hc/en-us/community/posts/207725245-React-import-are-not-resolved-in-WebStrom-and-Intellij-2016-2](https://intellij-support.jetbrains.com/hc/en-us/community/posts/207725245-React-import-are-not-resolved-in-WebStrom-and-Intellij-2016-2)
