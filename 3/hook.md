# HOOK
## Hook이란
- React 16.8부터 추가된 기능
- 기존 Class바탕의 코드를 작성할 필요 없이 상태 값과 여러 React의 기능을 사용할 수 있다.
- 즉, 클래스 형식의 리액트 컴포넌트에서만 할 수 있었던 state관리나 라이프사이클등을 함수 형태 컴포넌트에서도 사용할 수 있게 만들어주는 기능

## Hook을 만들게 된 이유
1. 컴포넌트 사이에서 상태 로직을 재사용하기 어렵다.
    - HOC(High order component)를 사용하지 않아도 된다.
2. 복잡한 컴포넌트들은 이해하기 어렵다.
3. Class는 사람과 기계를 혼동시킨다.
    - this의 존재가 원인

## HooK 예시
- 기존 class를 이용하여 작성한 경우
```
class ListAppender extends Component{
    constructor(props){
        super(props);
        this.state={
            value : ""            
        }
    }

    ChangeValue = value => {
        this.setState({
            value
        })
    }

    render(){
        return(
            <Box display="flex" justifyContent="center">
                <Box m={1} pt={3}><TextField label="할 일" variant="filled" value={this.state.value} onChange={(e)=> this.ChangeValue(e.target.value)}/></Box>
                <Box m={2} pt={3}><Button variant="contained" color="primary" onClick={function(e){
                    e.preventDefault();
                    this.props.onAddList(this.state.value);
                    var value = '';
                    this.setState({
                        value
                    });
                }.bind(this)}>추가</Button></Box>
            </Box>
        )
    }
}
```

- hook을 이용하여 함수형 컴포넌트로 변경한 경우
```
function ListAppender() {
    const [value, setValue] = useState("");
    
    return(
        <Box display="flex" justifyContent="center">
            <Box m={1} pt={3}><TextField label="할 일" variant="filled" value={this.state.value} onChange={(e)=> setValue(e.target.value)}/></Box>
            <Box m={2} pt={3}><Button variant="contained" color="primary" onClick={function(e){
                e.preventDefault();
                this.props.onAddList(this.state.value);
                var value = '';
                this.setState({
                    value
                });
            }.bind(this)}>추가</Button></Box>
        </Box>
    )
}
```

## useSate
- 현재의 state값과 이 값을 업데이트하는 함수를 쌍으로 제공
- class의 setSate와 유사하지만 이전 state와 새로운 state를 합치지않음
- 인자 값으로 state의 초기 값을 받음
- 하나의 컴포넌트 안에 여러 state넣기 가능

## useEffect
- 함수 컴포넌트 내에서 side effects를 수행할 수 있게 해줌
- side effects란 컴포넌트 안에서 데이터를 가져오거나 구독하고, DOM을 직접 조작하는 작업 또한 동작, 다른 컴포넌트에 영향을 줄 수 있고 렌더링 과정에서는 구현할 수 없는 작업
- 작동 조건
    1. 페이지가 처음 렌더링 되고 난 후 useEffect가 무조건 한번 실행
    2. useEffect에 배열로 지정된 useState의 값이 변경되면 실행
- 즉, 렌더링 혹은 변수의 값 혹은 오브젝트가 달라지게 되면, 그것을 인지하고 업데이트 해주는 함수

- useEffect를 사용하는 3가지 방법
    1. Dependency가 없는 방법
    ```
    useEffect(()=>{}); // 작동 조건이 발생될 때 마다 실행
    ```
    
    2. 대괄호 이용
    ```
    useEffect(()=>{},[]); // 첫 렌더링에만 실행을 원할 때
    ```
    
    3. 대괄호 속 특정 변수 지정시
    ```
    useEffect(()=>{},{특정 변수}); // 첫 렌더링 후 특정 변수(state)가 변경될 때 마다 실행
    ``` 

- return 활용시 언마운트시 함수 실행 가능

## 그외..
- useContext나 useReducer와 같은 다른 내장 Hook도 존재
- 경우에 따라서는 직접 Hook을 제작할 수 있음