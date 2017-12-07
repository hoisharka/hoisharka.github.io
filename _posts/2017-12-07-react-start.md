---
layout: post
title: React 초기 프로젝트 세팅
category: react
tag: react
---

react 프로젝트 초기 설정했던 내용을 간단하게 요약해서 적어놓는다. 그냥 복붙하기 좋게.

## global dependency 패키지 설치

```bash
$ npm install -g webpack webpack-dev-server
```

## npm init
프로젝트 디렉토리 생성 후 디렉토리로 들어가 npm init 명령어를 실행하고 콘솔로 나오는 물음들에 계속 엔터 쳐주면 기본값으로 package.json이 생성된다.

```bash
$ mkdir project_name
$ cd project_name
$ npm init
```

## 의존성 패키지 설치
```bash
npm install --save react react-dom
```

## 개발버전 의존성 패키지 설치
```bash
npm install --save-dev react-hot-loader webpack webpack-dev-server
npm install --save-dev babel-core babel-loader babel-preset-es2015 babel-preset-react 
```
## webpack.config.js
프로젝트 루트 디렉토리에 webpack.config.js 파일 생성 후 아래 코드 추가

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

## src/components/App.js
src/components/App.js 생성 후 아래 코드 추가

```es6
import React from 'react';

const App = () => <h1>Hello!!!</h1>;

export default App;
```

## src/index.js
src/index.js 생성 후 아래 코드 추가

```es6
import React from 'react';
import ReactDOM from 'react-dom';
import App from './components/App';

const rootElement = document.getElementById('root');
ReactDOM.render(<App />, rootElement);
```

## package.json 수정
scripts 안에 dev-server 추가
```
  ...
    "dev-server": "webpack-dev-server"
  ...
  
```

## 실행
`npm dev-server`를 실행. 

실행 결과
![react-hello]({{ site.url }}/assets/react-start-001.png)

* 참고
  - [https://www.inflearn.com/course/react-%EA%B0%95%EC%A2%8C-velopert/](https://www.inflearn.com/course/react-%EA%B0%95%EC%A2%8C-velopert/)
  - [http://slides.com/minjunkim-1/deck#/13/6](http://slides.com/minjunkim-1/deck#/13/6)
  - [https://velopert.com/reactjs-tutorials](https://velopert.com/reactjs-tutorials)
