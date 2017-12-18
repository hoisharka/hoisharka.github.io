---
layout: post
title: eslint - 명시적 전역 변수
category: eslint
tag: [eslint, airbnb] 
---
react 예제를 분석하는 중 clone받은 소스에서는 에러가 나지 않았던 부분인데, 새로 만든 프로젝트에서는 ESLint상에서 에러가 발생하는 걸 발견했다.

![no-restricted-globals]({{ site.url }}/assets/specific-global-variables.png)

location이나 history 같은 전역 변수를 ESLint가 참조할 수 있게 주석으로 명시해 주는 것이다. 

같은 eslint-config-airbnb-base 패키지를 사용하지만 예제 프로젝트는 v15.1.0이고 내가 설치한 최신 버전은 v16.1.0 이었다. v15.1.0로 설치하고 나니 에러가 사라졌다. 만약 v16.1.0을 사용한다고 하면 eslintrc 설정에 다음 rule을 추가하면 된다.
```
"no-restricted-globals": ["off"]
```

- 참고
  - [eslint.org > no-restricted-globals](https://eslint.org/docs/rules/no-restricted-globals)
