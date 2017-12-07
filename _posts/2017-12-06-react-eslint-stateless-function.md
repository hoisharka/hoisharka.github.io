---
layout: post
title: stateless function
category: react
tag: [react, webstorm, eslint] 
---

eslint(airbnb) 환경에서 아래와 같은 에러가 표시됐다.
![stateless funciton]({{ site.url }}/assets/react-eslint-stateless-function-001.png)
 
`stateless function` 을 쓰는 걸 권장하는 듯 한다.
`state`와 `stateless function`에 대해 알아보았다.

### state란? 
  - 컨포넌트에서 유동적인 데이터를 다룰 때 사용한다.
  - `state`를 사용하는 컨포넌트의 갯수는 최소화하는 것이 좋다. `state`를 다루는 컴포넌트가 많아지면 데이터를 관리하기 어려워진다. 
  - 여러개의 컴포넌트 각각에 `state`를 두기보다는 컨테이너 단위로 묶어서 사용하는 것이 효율적이다.
  - 컨테이너는 다른 컴포넌트를 묶어주는 역할을 하는 컴포넌트다.
    
  정리해보면 `state`는 유동적인 데이터를 담는 곳인데, 이게 있는 컴포넌트가 있고 없는 컴포넌트가 있는 듯 하다. `state`가 없는 컴포넌트의 경우 `stateless function` 을 사용하는 것이 좋다는 것이다.
  
## stateless function 이란?
- React .14부터 도입되었다.
- 컴포넌트를 정의하는 간단한 방법이다.
- plain javascript 함수를 사용한다

## stateless function으로 수정

- 기존 소스

```jsx
class SearchBar extends React.Component {
  render() {
    return <input />;
  }
}
```

- stateless function으로 수정한 소스

```jsx
const SearchBar = () => <input />;
```

바뀐 점을 살펴보면 다음과 같다
- `class`로 생성하지 않고 변수 선언 처럼 `const`를 사용했다
- `arrow function`을 사용했고 `render`함수도 필요 없다.
- `return` 구문 없이도 input 엘리먼트를 반환한다.

굉장히 간결해진 느낌이다. 앞으로는 간단한 구성의 컨포넌트는 이런식으로 작성해야겠다.


* 참고
  - [https://hackernoon.com/react-stateless-functional-components-nine-wins-you-might-have-overlooked-997b0d933dbc](https://hackernoon.com/react-stateless-functional-components-nine-wins-you-might-have-overlooked-997b0d933dbc)
  
