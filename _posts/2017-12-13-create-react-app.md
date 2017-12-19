---
layout: post
title: create-react-app으로 프로젝트 생성하기
category: react
tag: [react, redux] 
---

create-react-app로 프로젝트를 생성하고 eslint 설정이나 추가적으로 필요한 패키지 설치를 정리해본다.
그냥 내가 정리해놓고 갖다 쓰려고 작성하는 포스트이다.

## create-react-app 패키지 설치
```jsx
$ npm install -g create-react-app
```

## create-react-app으로 프로젝트 생성
```jsx
$ create-react-app hello-world
$ cd hollow-world
```

## eslint 설정
react-app 전용 코딩 컨벤션을 사용한다.

### .eslintrc.js 추가
```js
module.exports = {
  "extends": "react-app"
};
```

## typescript react패키지 설치
웹스톰에서 소스 참조가 되지 않을 경우 @types/react, @types/react-dom 를 설치하면 소스 참조가 잘 동작한다. 물론 typescript 소스일테지만 소스를 참조할 수 없다는 경고 메시지가 안 뜨게 하려면 설치하는 게 좋다.
```
$ npm install --save-dev @types/react @types/react-dom
```

## redux
redux, react-redux를 설치한다.
```
$ npm install --save redux react-redux
```

## prop-types
React패키지 안에 있던 PropTypes는 별도의 패키지로 분리됐다. 따로 설치해줘야한다.
```
$ npm install --save prop-types
```

## 프로젝트 폴더 안에서 설치하는 패키지(복붙용)
``` 
$ npm install --save-dev @types/react @types/react-dom
$ npm install --save redux react-redux prop-types
```
