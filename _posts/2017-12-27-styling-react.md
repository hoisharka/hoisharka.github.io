---
layout: post
title: 리액트 컴포넌트 스타일링
category: react
tag: [react, css]
---
verlopert 님의 [리액트 컴포넌트 스타일링](https://velopert.com/3447) 을 통해 공부한 것을 기록해 놓는 포스터다.

## webpack 설정에서 사용하는 3가지 로더
webpack 설정 파일을 살펴보면 3가지 로더가 사용되는 것을 볼 수 있다.

- style-loader
  - 스타일들을 불러와서 페이지에서 활성화 해주는 역할
- css-loader
  - css파일에서 `import`와 `url(...)` 문들을 webpack의 require 기능을 통하여 처리해주는 역할
- postcss-loader
  - css구문이 모든 브라우저에서 제대로 동작할 수 있도록 자동으로 -webkit, -mos, -ms 등의 접두사를 붙여줌

## CSS Module 사용 설정
  - css-loader 속성 변경
	  - modules: true
	  : CSS Module 활성화
    - localIdentName: '[path][name]__[local]--[hash:base64:5]'
    : 고유 클래스네임 생성 형식

## classnames
[classnames](https://www.npmjs.com/package/classnames)는 엘리먼트의 클래스 속성을 간편하게 조합해서 나타내기 위한 라이브러리이다.

classnames의 다양한 사용 예
```jsx
classNames('foo', 'bar'); // => 'foo bar'
classNames('foo', { bar: true }); // => 'foo bar'
classNames({ 'foo-bar': true }); // => 'foo-bar'
classNames({ 'foo-bar': false }); // => ''
classNames({ foo: true }, { bar: true }); // => 'foo bar'
classNames({ foo: true, bar: true }); // => 'foo bar'
classNames(['foo', 'bar']); // => 'foo bar'

// 동시에 여러개의 타입으로 받아올 수 도 있습니다.
classNames('foo', { bar: true, duck: false }, 'baz', { quux: true }); // => 'foo bar baz quux'

// false, null, 0, undefined 는 무시됩니다.
classNames(null, false, 'bar', undefined, 0, 1, { baz: null }, ''); // => 'bar 1'
```

## CSSModule 사용 예제
- CSSModule 기본 사용
- classnames 사용
- [CSSModule 사용 예제 소스](https://github.com/hoisharka/styling-react/tree/CSSModule)

## Sass 기본 예제

- 필요한 패키지
  - sass-loader
    - webpack에서 sass 파일을 읽어오는 역할
  - node-sass
    - sass 코드를 css로 변환
- sass 사용을 위한 webpack 설정
- 간단한 변수, 믹스인 사용
- 전역적으로 사용하기
- [Sass 기본 예제 소스](https://github.com/hoisharka/styling-react/tree/sass)

## styled-components 예제
- styled-components 패키지 사용
- es6의 Tagged Template Literal 문법 사용
  - backquote 사이에 `${자바스크립트표현}`을 사용하면 자바스크립트 표현식에 해당하는 부분을 별도의 인자로 함수에 전달한다.
- [styled-components 예제 소스](https://github.com/hoisharka/styling-react/tree/styled-components)

## Sass 라이브러리를 사용한 반응형 버튼 예제
- include-media, open-color 라이브러리를 사용
- [Sass 라이브러리 사용 예제](https://github.com/hoisharka/styling-react/tree/sass-button)

## 마무리
  - 가장 낯선 방법이 styled-components였다.
  - 태그드 템플릿 리터럴도 너무 낯선 문법이다. 예제를 좀 더 찾아봐야겠다.
  - 개인 프로젝트 작업을 할 땐 scss를 사용할 것 같다.

- 참고
  - [velopert.com > 리액트 컴포넌트 스타일링](https://velopert.com/3447)
