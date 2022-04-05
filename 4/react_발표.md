# 0. 들어가기 ...

강의 절반 정도를 2주 전에 듣고 이번에 남은거를 다시 들었는데 다 까먹어서 시간이 오래 걸렸다.

근데 또 라우팅이랑 이것저것 찾아보다 보니까 할게 많은 것 같아서 진도 따라가려면 내가 직접 만들기보다는 잘 정리된걸 따라하면서 공부하는게 낫다고 생각했다.

그래서 사용한 것들 정리를 해보았다 !!



# 1. react-router-dom

라우터 적용은 index.js에서`BrowserRouter` 라는 컴포넌트를 사용하여 구현한다. `<App />`을 `<BrowserRouter>`로 감싸준다.

```
// index.js

import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import { BrowserRouter } from 'react-router-dom';


ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
  document.getElementById('root')
);
```

예전 블로그를 봤을 때에는 `<Route path="/" component={Home} />` 이렇게 한다고 나와있었는데, 문법이 틀렸는지 적용이 안 되었다. v6으로 바뀌면서 `<Route path="/" element={<Home />} />`로 바뀌었다고 한다.

```
// App.js

import logo from './logo.svg';
import './App.css';
import { Route, Routes } from 'react-router-dom';
import Header from './Header';
import Home from './Home';
import About from './About';

function App() {
  return (
    <div className="App">
      <Header/>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </div>
  );
}

export default App;

```

다음으로 `Link`를 눌렀을 때, 이동하는 방법이다. `<Link to="/주소"></Link>` 이렇게 사용한다.

```
import { Link } from 'react-router-dom';

const Home = () => {
  return (
    <div>
      <h1>홈</h1>
      <p>가장 먼저 보여지는 페이지입니다.</p>
      <Link to="/about">소개</Link>
    </div>
  );
};

export default Home;
```

파라미터를 전달하는 방법도 있다고 하는데 그거까지는 아직 사용하지 않아서 대충 읽어보기만 했다.

# 2. useReducer

### reducer란?

reducer란 현재 상태와 액션 객체를 파라미터로 받아와서 새로운 상태로 반환해주는 함수이다.

```
function reducer(state, action) {
  // 새로운 상태를 만드는 로직
  // const nextState = ...
  return nextState;
}
```

여기서 action은 주로 `type` 값을 가진 객체 형태로 사용한다. 아래와 같이 action에 따라 다른 동작을 할 수 있다.

```
function todoReducer(state, action) {
  switch (action.type) {
    case 'CREATE':
      return state.concat(action.todo);
    case 'TOGGLE':
      return state.map(todo =>
        todo.id === action.id ? { ...todo, done: !todo.done } : todo
      );
    case 'REMOVE':
      return state.filter(todo => todo.id !== action.id);
    default:
      throw new Error(`Unhandled action type: ${action.type}`);
  }
}
```

### useReducer

useReducer의 사용법은 다음과 같다.

```
const [state, dispatch] = useReducer(reducer, initialState);
```

여기서 `state`는 앞으로 컴포넌트에서 사용할 상태를 나타내고, `dispatch`는 액션을 발생시키는 함수이다.

이 함수는 `dispatch({ type: 'INCREMENT' })`와 같은 형태로 사용한다.

```
// +, - 버튼을 클릭하면 값이 +1, -1 되는 코드

import React, { useReducer } from 'react';

function reducer(state, action) {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1;
    case 'DECREMENT':
      return state - 1;
    default:
      return state;
  }
}

function Counter() {
  const [number, dispatch] = useReducer(reducer, 0);

  const onIncrease = () => {
    dispatch({ type: 'INCREMENT' });
  };

  const onDecrease = () => {
    dispatch({ type: 'DECREMENT' });
  };

  return (
    <div>
      <h1>{number}</h1>
      <button onClick={onIncrease}>+1</button>
      <button onClick={onDecrease}>-1</button>
    </div>
  );
}

export default Counter;
```

# 3. Context API

![img](https://i.imgur.com/hX8jjXG.png)

원래라면 이런 식으로 props를 사용해서 자식 컴포넌트에게 값을 전달해주는데, 지금처럼 작은 규모로 만든다면 불편하지 않지만 프로그램이 커질수록 App에서 모든 상태 관리를 해야 한다. 그러면 props를 전달할 때 계속 컴포넌트를 거쳐서 전달해주어야 하기 때문에 코드가 복잡해질 수 있다.



![img](https://i.imgur.com/lYiiIZF.png)

Context API를 사용하면 프로젝트 안에서 값을 전역적으로 사용할 수 있다.



### 리액트 공식 문서에서의 Context

```
context를 이용하면 단계마다 일일이 props를 넘겨주지 않고도 컴포넌트 트리 전체에 데이터를 제공할 수 있습니다.

일반적인 리액트 애플리케이션에서 데이터는 위에서 아래로 (즉, 부모로부터 자식에게) props를 통해 전달되지만, 애플리케이션 안의 여러 컴포넌트들에 전해줘야 하는 props의 경우 (예를 들면 선호 로케일, UI 테마) 이 과정이 번거로울 수 있습니다. context를 이용하면, 트리 단계마다 명시적으로 props를 넘겨주지 않아도 많은 컴포넌트가 이러한 값을 공유하도록 할 수 있습니다.
```

값을 관리하는 것이 아니라 오로지 전달하고 공유한다. 즉, **Prop Drilling**을 피하는 것이 목적이라고 한다.

실질적인 상태 관리는 useState, useReducer를 통해서 가능하다.

* **Prop Drilling**이란, props를 오직 하위 컴포넌트로 전달하는 용도로만 쓰이는 컴포넌트를 거치며 컴포넌트에서 다른 컴포넌트로 데이터를 전달하는 과정이다.

### Context 생성하기

Context는 다음과 같이 생성할 수 있다. 함수 인자에는 상태의 초기값이 들어간다.

```
const TodoStateContext = createContext();
```

Context를 생성하면 Context 안에 Provider라는 컴포넌트가 들어있다. 이 컴포넌트를 통해서 Context의 값을 정할 수 있다. 사용하려면 `value`라는 값을 설정해주면 된다.

```
<TodoStateContext.Provider value={state}>...</TodoStateContext.Provider>
```

### 다른 컴포넌트에서 state 사용하기

먼저, 사용하기 위해서는 `App.js`와 같은 루트 컴포넌트에서 Provider를 정의해야 한다.

```
import { TodoProvider } from './components/TodoContext';

function App() {
  return (
    <TodoProvider>
      <GlobalStyle />
      <TodoTemplate>
        <TodoHead />
        <Routes>
          <Route path="/" element={<TodoList />} />
          <Route path="/about" element={<About />} />
        </Routes>
        <TodoCreate />
      </TodoTemplate>
    </TodoProvider>
  );
}
```

#### 1) useContext

이 Context를 내보내서 다른 곳에서 사용하려면 다음과 같이 하면 된다.

```
import { TodoStateContext } from '../TodoContext';

const state = useContext(TodoStateContext);
```

#### 2) 커스텀 Hook 만들기

`useContext`를 직접 사용하는 대신 이런 방법도 존재한다.

```
export function useTodoState() {
    const context = useContext(TodoStateContext);
    if (!context) {
      throw new Error('Cannot find TodoProvider');
    }
    return context;
 }
```

사용하고자 하는 곳에서 다음과 같이 import 후 가져올 수 있다.

```
import { useTodoState } from './TodoContext';

const todos = useTodoState();
```

# 4. Redux

Context API와 Redux를 비교하는 글이 많았다. 둘다 전역 상태 관리를 한다는 점에서 공통점을 가지고 있다.

Redux는 Context API로 만든 라이브러리이고, 제공하는 기능이 좀 더 많다고 한다. React 뿐만 아니라 Vanilla JS, Vue 등에서 사용이 가능하다.

1. 로컬스토리지에 상태를 영속적으로 저장하고, 시작할 때 다시 불러오는 것에 뛰어나다.

2. 상태를 Server에서 미리 채워서, HTML에 담아 Client로 보내고, 앱을 시작할 때 다시 불러오는 것에 뛰어나다.

3. 사용자의 액선을 직렬화(Serialize)해서, 상태와 함께 자동으로 버그 리포트에 첨부할 수 있고, 개발자들은 이를 통해 에러를 재현할 수 있다. 

4. 액션 객체를 네트워크를 통해 보내면, 코드를 크게 바꾸지 않고도 협업 환경을 구현할 수 있다.

5. 코드를 크게 바꾸지 않고도 실행 취소 내역의 관리나 Optimistic Mutations(낙관적인 변경)을 구현할 수 있다.

6. 개발할 때, 상태 내역 사이를 오가고 액션 내역에서 현재 상태를 다시 계산하는 일을 TDD 스타일로 할 수 있다.

7. 개발자 도구가 완전한 조사, 제어를 할 수 있게 함으로써 개발자들이 자신의 앱을 위한 도구를 직접 만들 수 있게 해준다.

8. 비즈니스 로직을 대부분 재사용하면서 UI를 변경할 수 있게 한다. 

   

#### Store

Store는 전역상태를 저장하는 공간이다. JSON 형태로 저장된다.



#### Action

Action은 Reducer에게 보내는 Store에 대한 행동을 정의한다. Action을 Reducer에게 전달하기 위해서는 dispatch 메소드를 사용한다.

![img](https://miro.medium.com/max/1400/0*7j_lofjdmPM96Pb3.)



#### Reducer

이전 상태와 Action을 합쳐 새로운 state를 만드는 조작이다.

![img](https://miro.medium.com/max/1400/0*CwvI4QU26E-Ww8mb.)

#### 컴포넌트 연결

1. Store가 가진 `state`를 어떻게 `props`에 엮을 지 정한다**(이 동작을 정의하는 함수는** `mapStateToProps`**라고 불린다.).**
2. Reducer에 action을 알리는 함수 `dispatch`를 어떻게 `props`에 엮을 지 정한다**(이 동작을 정의하는 함수는** `mapDispatchToProps`**라고 불린다.)**.
3. 위에 두가지가 적용된 `props`를 받을 Component를 정한다.
4. Store와 Reducer를 연결시킬 수 있도록 만들어진 Component가 반환값이 된다.

![img](https://miro.medium.com/max/1400/0*fdVQeHMgpJbCn7xC.)



### 전체적인 흐름

![img](https://miro.medium.com/max/1400/0*Z18iLsM7Bf1xoNth.)

[React+Redux 플로우의 이해]: https://medium.com/@ca3rot/%EC%95%84%EB%A7%88-%EC%9D%B4%EA%B2%8C-%EC%A0%9C%EC%9D%BC-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-%EC%89%AC%EC%9A%B8%EA%B1%B8%EC%9A%94-react-redux-%ED%94%8C%EB%A1%9C%EC%9A%B0%EC%9D%98-%EC%9D%B4%ED%95%B4-1585e911a0a6





#### 