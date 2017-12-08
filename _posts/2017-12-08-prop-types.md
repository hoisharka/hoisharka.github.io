---
layout: post
title: prop types
category: react
tag: [react] 
---

다음과 같은 구문에서 에러가 발생했다.
```js
ContactCreate.propsTypes = {
  onCreate: React.PropTypes.func
};
```

에러 내용은 다음과 같다
```
Uncaught TypeError: Cannot read property 'func' of undefined
```

원인을 알아보니 React v15.5 부터는 React.PropTypes가 별도의 패키지로 분리되었다고 한다. 

PropTypes를 사용하려면 다음과 같은 작업이 필요하다.
 
- 다음 명령어로 설치한다. 
```bash
$ npm install --save prop-types
```

- 소스 상단에 설치한 패키지를 import 해준다.
```jsx
import PropTypes from 'prop-types';
```

React.PropTypes를 PropTypes로 변경한다.
```js
ContactCreate.propsTypes = {
  onCreate: PropTypes.func
};
```

* 참고 
  - [https://reactjs.org/docs/typechecking-with-proptypes.html](https://reactjs.org/docs/typechecking-with-proptypes.html)
  - [https://www.npmjs.com/package/prop-types](https://www.npmjs.com/package/prop-types)
