---
layout: post
title: ESLint no-path-concat
category: react
<<<<<<< HEAD
tag: [eslint, react, webstorm]
---

Udemy에서 react강좌를 구매했었다. 영어라서 아무 사전 지식 없이 들으면 시간이 너무 오래 걸릴 것 같았는데, [무료 한국어 강좌](https://www.inflearn.com/course/react-%EA%B0%95%EC%A2%8C-velopert/)가 있어서 먼저 빠르게 훑어보기로 했다. 

예제에 webpack.config.js 코드를 내 프로젝트에 붙여놨는데 ESLint에서 에러가 발생했다.  
![no-path-concat]({{ site.url }}/assets/eslint-no-path-concat-001.png)

에러를 수정하기 위해 구글링에 들어갔다. [no-path-concat](https://eslint.org/docs/rules/no-path-concat)에 대한 ESLint 문서에 해결 방법이 있었다. 내용을 요약해보면 다음과 같다.

- nodejs에서 __dirname, __filename 을 전역변수로 사용한다.
- __dirname은 현재 파일의 디렉토리이고 __filename은 현재 파일의 파일명이다
- 종종 다른 파일 경로를 표현하기 위해 다음과 같이 쓰려고 시도한다.
```js
var fullPath = __dirname + "/foo.js";
```
- 위처럼 사용할 때 문제점은 다음과 같다
  - 시스템에 따라 경로를 표현하는 구분자가 다른다. (유닉스 기반은 `/`, 윈도우는 `\` 이다)
  - 구분자를 겹쳐쓰거나 유효하지 않은 경로로 끝날 수 있다.
- 해결 방법은 아래와 같이 `path.join`이나 `path.resolve`함수를 사용하는 것이다. 
```js
var fullPath = path.join(__dirname, "foo.js");
var fullPath = path.resolve(__dirname, "foo.js");
```
  
`path.resolve`함수는 `fully-qualified path`를 반환 한다고 하는데 `fully-qualified path`가 무엇인지 몰라서 추가로 찾아봤다. 

 `fully-qualified path`는 `canonical-path`와 같은 개념이고 이것은 `absolute-path`와 다른 것이라고 한다. <br>
 아래 예제만 봐도 차이를 알 수 있을 것 같다. 

```
Abs path : /home/originfile
Can path : /home/originfile
Abs path : /home/symlink
Can path : /home/originfile
Abs path : /home/./././originfile
Can path : /home/originfile
```
`absolute-path`는 여러가지로 존재할 수 있는 것에 반해 `canonical-path`는 한가지로 정의된다.

아래는 path.join을 사용해서 webpack.config.js를 수정한 내용이다. path 패키지를 사용하기 위해 상단에 require 문도 추가했다. 

```js
const webpack = require('webpack');
const path = require('path');

module.exports = {
  entry: ['./src/index.js'],

  output: {
    path: path.join(__dirname, 'public/'),
    filename: 'bundle.js'
  },

  devServer: {
    hot: true,
    inline: true,
    host: '0.0.0.0',
    port: 4000,
    contentBase: path.join(__dirname, 'public/')
  },

  module: {
    loaders: [
      {
        test: /\.js$/,
        loader: 'babel-loader',
        exclude: /node_modules/,
        query: {
          cacheDirectory: true,
          presets: ['es2015', 'react']
        }
      }
    ]
  },

  plugins: [
    new webpack.HotModuleReplacementPlugin()
  ]
};

```
* 참고
  - [https://eslint.org/docs/rules/no-path-concat](https://eslint.org/docs/rules/no-path-concat)
  - [http://www.benjaminlog.com/entry/absolute-path-vs-canonical-path](http://www.benjaminlog.com/entry/absolute-path-vs-canonical-path)
