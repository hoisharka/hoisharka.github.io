---
layout: post
title: React 개발 환경, Webstorm + eslint(airbnb)
category: react
tag: react, webstorm, eslint 
---

코딩 컨벤션은 한번도 써 본 적이 없어서 한번 써보기로 했다. 
프론트엔드에서는 airbnb를 많이 쓴다고 하여 webstorm에 설치해봤다.
 
## npm으로 eslint 설치

```bash
$ npm install --save-dev eslint
```

## webstorm setting
Languages & Frameworks > Javascript 항목을 눌러 사용할 Javascript 버전을 React JSX로 변경한다.  
![webstorm js setting]({{ site.url }}/assets/react-eslint-001.png)

세팅 메뉴에 들어가서 eslint를 검색하면 아래 화면이 뜬다. <br>
ESLint package 항목에 현재 프로젝트의 node_modules안에 eslint를 선택한다.
![webstorm js setting]({{ site.url }}/assets/react-eslint-001.png)

### airbnb-eslint관련 플러그인 설치
```bash
npm install --save-dev eslint-plugin-import eslint-plugin-react eslint-plugin-jsx-a11y eslint-config-airbnb
```
### 프로젝트 디렉토리 루트에 .eslintrc.js 추가
아래 설정은 react개발 시 eslintrc.js를 설정하는 방법을 검색해서 복붙해 놓은 것이다.<br>
env는 실행 환경을 말하는 것 같고 airbnb 스타일을 사용한다고 extends에 명시해 놓는 것 같다.<br>
plugins에 react를 명시해 주므로써 위에서 설치한 eslint-plugin-react 플러그인을 사용하게 하는 것 같다.<br>

rules의 react/jsx-filename-extension 는 js파일에서 jsx문법을 사용했을 때 오류가 발생하는 문제가 있어서 구글링해서 추가시켰다.
```js
module.exports = {
  "env": {
    "browser": true,
    "node": true,
    "es6": true
  },
  "extends": "airbnb",
  "plugins": [
    "react"
  ],
  "rules": {
    "react/jsx-filename-extension": [1, {"extensions": [".js", ".jsx"]}]
  }
};
```
- 참고
  - [http://jojoldu.tistory.com/230](http://jojoldu.tistory.com/230)
