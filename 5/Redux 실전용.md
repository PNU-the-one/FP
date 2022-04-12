# Redux 실전용...?

Redux는 자바스크립트 앱을 위한 예측 가능한 상태 컨테이너입니다.  

> 예측 가능한 = 쉽게
> 
> 상태 컨테이너 = 객체
> 
> 객체를 전역적으로 관리하면서 상태관리를 쉽게 해주나보다.

![](C:\Users\audtj\AppData\Roaming\marktext\images\2022-04-11-00-25-33-image.png)

### state(in store)

상태를 저장해주는 곳, 객체의 형태로 저장되어 있다.

store에 reducer를 구해줘야 한다.

### reducer

State에 접근 할 수있는 녀석(함수), 이녀석이 return 하는 객체가 State가 됨

> 상태를 변경할 때는 reducer의 허락을 받아야한다.

### getState

State를 받아오는 메쏘드

> 그냥 상태를 읽어오는건 허락 받을필요가 없다.

### dispatch

State의 값을 가져와 달라고 reducer에게 요청한다.

이때, action 이라는 종이(?, 사실은 객체)를 제출한다.

> reducer 에게 요청하는 메쏘드.

### subscribe(구독)

함수가 store를 구독하면 dispatch가 일어날 때 마다 함수가 재실행 된다.

> 우리가 아는 그 구독
> 
> 구독을 하면 알람이 오듯이 함수를 재실행함.

### action

하나의 객체, reducer에게 제출할 종이(?)

### render

그냥 적용(함수실행)된다는 의미.



## React-Redux

먼저 React-Redux는 Redux를 React에서 더 편하게 사용하기 위해서 필요한 라이브러리다. Redux를 이용해서 만든 것들을 react만의 방식으로 사용할 수 있게 해준다.

##### 사용법

1. 일단 action, reducer를 정의한다.

2. 정의한 reducer를 이용해서 store를 생성한다.(createStore(reduer))

3. Provider 라는 컴포넌트(?)와 store를 연결한다.

4. Provider 하위 컴포넌트 에서는 store를 공유한다. 즉, 전역적으로 사용가능 하다.

###### 예시

```js
// index.js
import {createStore} from 'redux';
import {Provider} from 'react-redux';

const store = createStore(reducer);

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```

### Hook

우리는 store에 접근하기 위해서 Hook을 사용해야한다. 마치, 컴포넌트 상태에 접근하기 위해서 useState를 사용하는 것과 같이.

##### useSelector

getState와 같은 결과를 인자로 받게된다.

```js
const value = useSelector(state => (state.key))


// state라는 인자가 store의 state를 의미한다.
```

##### useDispatch

useDispatch는 바로 dispatch를 실행하지 않는다. redux의 dispatch함수를 리턴한다. 보통 아래와 같은 방법으로 사용된다.

```js
const dispatch = useDispatch()

const onAcntion = () => dispatch(action);
// action 부분은 보통 action생성자 라는것을 사용해서 action()으로 action객체를
// 리턴시킨다.

function Actioncreator() = ({
    type: "GO",
    value : ""
})


```

### 구독(subscribe)는 어디로 갔나?

리액트에서는 Redux를 더욱 쉽게 사용하게 해주기 위해서 이 과정을 생략할 수 있다. 리액트에서 자동으로 구독을 해주기때문에 state와 연관된 컴포넌트들이 자동으로 rerender 되는 것을 볼 수 있다.
