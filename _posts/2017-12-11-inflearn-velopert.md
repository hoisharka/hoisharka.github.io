---
layout: post
title: velopert님의 react 강좌를 듣고
category: react
tag: [react, redux] 
---

[velopert님의 react 강좌](https://www.inflearn.com/course/react-%EA%B0%95%EC%A2%8C-velopert/) 를 듣고 느낀점과 메모해두고 싶은 내용을 적어봤다.

## 느낀점
- 초반에 react 부분은 이해도 잘 되고 특별히 어려운 부분이 없었다. redux 부분을 들어가면서 왜 react가 러닝커브가 처음에는 낮지만 점점 높아진다고 하는지 알겠다. 
- flux 개념까진 [Flux로의 카툰안내서](http://bestalign.github.io/2015/10/06/cartoon-guide-to-flux/)를 보고 어느정도 이해했다. 그런데 [Redux로의 카툰 안내서](http://bestalign.github.io/2015/10/26/cartoon-intro-to-redux/)는 머리속에 내용이 안들어온다.
- react의 기본적인 내용을 이해하는데 도움이 많이 됐고, redux는 다른 설명으로 이해하기 보다는 예제를 한번더 분석하는 포스팅을 하면서 정리해야겠다.
- 이 강좌에 express부분은 학습하지 않았는데, 이유는 angular를 공부하면서 express에 대해서는 대략 맛보기로 봤었고, 지금 집중할 부분은 프론트 부분이라서 패스하기로 했다.

## redux 예제 코드 분석
![redux example]({{ site.url }}/assets/inflearn-velopert-001.png)
예제는 굉장히 간단하다.
- 기능
  - [+]버튼을 누르면 숫자가 증가한다.
  - [-]버튼을 누르면 숫자가 감소한다.
  - [Randomize Color]버튼을 누르면 배경색이 랜덤으로 변한다.

- 소스 구조
![source]({{ site.url }}/assets/inflearn-velopert-002.png)

  - acitons
    - ActionTypes.js
    
      ```jsx
      export const INCREMENT = "INCREMENT";
      export const DECREMENT = "DECREMENT";
      export const SET_COLOR = "SET_COLOR";
      ```
      - 액션 타입을 정의한다.
      - 액션 타입은 INCREMENT, DECREMENT, SET_COLOR 세가지이다.
    - index.js
    
      ```jsx
      import * as types from './ActionTypes';
      
      export const increment = () => ({
        type: types.INCREMENT
      });
      
      export const decrement = () => ({
        type: types.DECREMENT
      });
      
      export const setColor = color => ({
        type: types.SET_COLOR,
        color
      });
      ```
      - {type: 액션 타입 ...} 형태로 데이터를 반환하는 함수를 정의해놨다. 
      - increment, decrement, setColor 함수 3가지가 있고 setColor 함수의 경우 type말고도 color값을 파라미터로 받아 그대로 반환한다. 
  - components
    - App.js
      
      ```jsx
      import React from 'react';
      import Counter from './Counter';
      
      const App = () => <Counter />;
      
      export default App;
  
      ```
      - Counter 컴포넌트를 랜더링한다.
    - Control.js
    
      ```jsx
      import React from 'react';
      import PropTypes from 'prop-types';
      
      const propTypes = {
        onPlus: PropTypes.func,
        onSubtract: PropTypes.func,
        onRandomizeColor: PropTypes.func
      };
    
      function createWarning(funcName) {
        return () => console.warn(`${funcName} is not defined`);
      }
      
      const defaultProps = {
        onPlus: createWarning('onPlus'),
        onSubtract: createWarning('onSubtract'),
        onRandomizeColor: createWarning('onRandomizeColor')
      };
      
      const Control = props => (
        <div>
          <button onClick={props.onPlus}>+</button>
          <button onClick={props.onSubtract}>-</button>
          <button onClick={props.onRandomizeColor}>Randomize Color</button>
        </div>
      );
    
      Control.propTypes = propTypes;
      Control.defaultProps = defaultProps;
      
      export default Control;
      ```
      - 세개의 버튼으로 구성된 Control 컴포넌트이다.
      - 각 버튼의 이벤트는 props로 전달 받는다.
      - state가 없고 전달받은 props로 컴포넌트를 구성하고 있다.
    - Counter.js
      ```jsx
      import React, { Component } from 'react';
      import PropTypes from 'prop-types';
      import { connect } from 'react-redux';
      
      import Value from './Value';
      import Control from './Control';
      import * as actions from '../actions';
      
      const propTypes = {
        number: PropTypes.number,
        color: PropTypes.shape(PropTypes.number),
        handleIncrement: PropTypes.func,
        handleDecrement: PropTypes.func,
        handleSetColor: PropTypes.func
      };
      
      function createWarning(funcName) {
        return () => console.warn(`${funcName} is not defined`);
      }
      
      const defaultProps = {
        number: 0,
        color: 255,
        handleIncrement: createWarning('handleIncrement'),
        handleDecrement: createWarning('handleDecrement'),
        handleSetColor: createWarning('handleSetColor')
      };
      
      class Counter extends Component {
        constructor(props) {
          super(props);
          this.setRandomColor = this.setRandomColor.bind(this);
        }
      
        setRandomColor() {
          const color = [
            Math.floor((Math.random() * 55) + 200),
            Math.floor((Math.random() * 55) + 200),
            Math.floor((Math.random() * 55) + 200)
          ];
      
          this.props.handleSetColor(color);
        }
      
        render() {
          const style = {
            background: `rgb(${this.props.color[0]}, ${this.props.color[1]}, ${this.props.color[2]})`
          };
      
          return (
            <div style={style}>
              <Value number={this.props.number} />
              <Control
                onPlus={this.props.handleIncrement}
                onSubtract={this.props.handleDecrement}
                onRandomizeColor={this.setRandomColor}
              />
            </div>
          );
        }
      }
      
      Counter.propTypes = propTypes;
      Counter.defaultProps = defaultProps;
      
      const mapStateToProps = state => ({
        number: state.counter.number,
        color: state.ui.color
      });
      
      const mapDispatchToProps = dispatch =>
        // return bindActionCreators(actions, dispatch);
        ({
          handleIncrement: () => { dispatch(actions.increment()); },
          handleDecrement: () => { dispatch(actions.decrement()); },
          handleSetColor: (color) => { dispatch(actions.setColor(color)); }
        });
      export default connect(mapStateToProps, mapDispatchToProps)(Counter);
      ```
      - Counter 컴포넌트는 state가 있는 컴포넌트이다.
      - 핵심 적인 코드 부분은 mapStateToProps와 mapDispatchToProps이다.
      - mapStateToProps
        - state의 내용을 받아 props로 변환하고 있다.
      - mapDispatchToProps
        - actions에 있는 함수를 각각 handle함수로 재구성하고 있다.
        - dispatch() 함수는 파라미터로 액션을 보내 해당 액션에 대한 값을 받아온다.
    - Value.js
    
      ```jsx
      import React from 'react';
      import PropTypes from 'prop-types';
      
      const propTypes = {
        number: PropTypes.number
      };
      
      const defaultProps = {
        number: -1
      };
      
      const Value = props => (
        <div>
          <h1>{props.number}</h1>
        </div>
      );
      
      
      Value.propTypes = propTypes;
      Value.defaultProps = defaultProps;
      
      export default Value;
      ```
      - 단순히 props.number를 화면에 뿌리는 컴포넌트이다.
  - reducers
    - counter.js
    
      ```jsx
      import * as types from '../actions/ActionTypes';
      
      const initialState = {
        number: 0
      };
      
      export default function counter(state = initialState, action) {
        switch (action.type) {
          case types.INCREMENT:
            return {
              ...state,
              number: state.number + 1
            };
          case types.DECREMENT:
            return {
              ...state,
              number: state.number - 1
            };
          default:
            return state;
        }
      }
      ```
      - state를 구성하는 데이터 중 number에 해당하는 정보만 다루고있다.
      - action의 type에 따라 분기되어 증가, 감소 시킨 값을 반환하고 있다.
    - ui.js
    
      ```jsx
      import * as types from '../actions/ActionTypes';
      
      const initialState = {
        color: [255, 255, 255]
      };
      
      export default function ui(state = initialState, action) {
        if (action.type === types.SET_COLOR) {
          return {
            color: action.color
          };
        }
        return state;
      }
      ```
      - state를 구성하는 데이터 중 color에 대한 데이터만 다루고 있다.
      - action의 type이 SET_COLOR일 때 action.color값을 state에 적용시켜 반환한다.
    - index.js
    
      ```jsx
      import { combineReducers } from 'redux';
      import counter from './counter';
      import ui from './ui';
      
      const reducers = combineReducers({
        counter, ui
      });
      
      export default reducers;
      ```
      - combineReducers 함수로 reducer 2개를 합친다.
  - index.js
  
    ```jsx
    import React from 'react';
    import ReactDOM from 'react-dom';
    
    import { createStore } from 'redux';
    import { Provider } from 'react-redux';
    import reducers from './reducers';
    
    import App from './components/App';
    
    const store = createStore(reducers);
    
    ReactDOM.render(
      <Provider store={store}>
        <App />
      </Provider>,
      document.getElementById('root')
    );
    ```
    - reducers를 파라미터로해서 store를 생성한다.
    - 생성한 store를 prop로 하는 Provider를 랜더링한다.
    - Provider 안에 App 컴포넌트가 포함되어 있다.


### store를 Provider 컴포넌트의 props에 넣어준 것은 무엇을 의미하는가?
  - Provider 컴포넌트는 connect 함수를 사용하여 다른 컴포넌트와 연결할 수 있도록 store를 제공한다.
  - Counter.js에서 `connect(mapStateToProps, mapDispatchToProps)(Counter);` 구문의 사용으로 Counter는 state값을 props로 전달 받을 수 있었다.
  - 이 방식은 react-redux를 사용할 때의 방식이라고 한다. react-redux를 사용하지 않고 작업할 수동 있지만 번거로운 코드가 많아지는 모양이다.

### 그림으로 이해한 내용을 그려보았다.
  ![redux diagram]({{ site.url }}/assets/inflearn-velopert-003.png)
  
- 참고
  - [https://www.vobour.com/book/view/6vas6uCQF8GXDJDHt](https://www.vobour.com/book/view/6vas6uCQF8GXDJDHt)
  - [https://www.inflearn.com/course/react-%EA%B0%95%EC%A2%8C-velopert/](https://www.inflearn.com/course/react-%EA%B0%95%EC%A2%8C-velopert/)
