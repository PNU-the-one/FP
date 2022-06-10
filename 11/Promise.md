# Promise

Promise는 자바스크립트 비동기 처리에 사용되는 객체.

## 비동기 처리란 무엇인가.

자바스크립트의 비동기 처리란 특정 코드의 연산이 끝날 때까지 코드의 실행을 멈추지 않고 다음 코드를 먼저 실행하는 자바스크립트의 특성

## 비동기 처리 예시

- XMLHttpRequest

```js
let xhr = new XMLHttpRequest();

// METHOD, 주소, 비동기여부 설정
xhr.open('GET', '주소', true); 

// 요청전송
xhr.send()

// 통신후 작업
xhr.onload = () => {}
```

- 제이쿼리
  
  ```js
  $.get('주소', function(){})
  ```

  function getData() {
   var tableData;
   $.get('https://domain.com/products/1', function(response) {
       tableData = response;
   });
   return tableData;
  }

  console.log(getData()); // undefined

```
- setTimeout

```js
setTimeout(function(){}, 3000);
```

## 

## 콜백함수

```js
function getData(callbackFunc) {
    $.get('https://domain.com/products/1', function(response) {
        callbackFunc(response); // 서버에서 받은 데이터 response를 callbackFunc() 함수에 넘겨줌
    });
}

getData(function(tableData) {
    console.log(tableData); // $.get()의 response 값이 tableData에 전달됨
});
```

콜백함수를 이용하면 위의 문제를 해결할 수 있다.

## 콜백지옥

콜백 안에 콜백을 계속 무는 형식의 코딩

```js
$.get('url', function(response) {
    parseValue(response, function(id) {
        auth(id, function(result) {
            display(result, function(text) {
                console.log(text);
            });
        });
    });
});
```

### 해결방법

```js
function parseValueDone(id) {
    auth(id, authDone);
}
function authDone(result) {
    display(result, displayDone);
}
function displayDone(text) {
    console.log(text);
}
$.get('url', function(response) {
    parseValue(response, parseValueDone);
});
```

각각의 콜백 함수를 함수로 분리해주면 된다.

### 여기까진 빌드업...

위의 해결방법은 코딩패턴으로만 콜백지옥을 해결하는 것이다. 하지만 더 쉬운(?) 일반적인 방법들이 있다.
그게 지금부터 얘기할 **Promise**이다.

## Promise

**“A promise is an object that may produce a single value some time in the future”**

### 이게 왜 필요한가?

콜백지옥 해결.

### 기초 코드

- 일반적인 ajax 코드
  
  ```js
  function getData(callbackFunc) {
  $.get('url 주소/products/1', function(response) {
    callbackFunc(response); // 서버에서 받은 데이터 response를 callbackFunc() 함수에 넘겨줌
  });
  }
  
  getData(function(tableData) {
   console.log(tableData); // $.get()의 response 값이 tableData에 전달됨
  });
  ```

- Promise 사용한 코드

```js
function getData(callback) {
  // new Promise() 추가
  return new Promise(function(resolve, reject) {
    $.get('url 주소/products/1', function(response) {
      // 데이터를 받으면 resolve() 호출
      resolve(response);
    });
  });
}

// getData()의 실행이 끝나면 호출되는 then()
getData().then(function(tableData) {
  // resolve()의 결과 값이 여기로 전달됨
  console.log(tableData); // $.get()의 reponse 값이 tableData에 전달됨
});
```

위와 아래의 차이는

1. get()를 프로미스 객체와 연결(?) 시켰다.
2. resolve, reject의 등장
3. getData()의 then의 등장

#### 위의 3가지로 유추해볼 수 있는 점

- promise를 쓰면 then을 쓸 수 있다.
- resolve가 콜백 함수를 실행 시키는 건가?
- reject는 거부..?

### Promise의 3가지 상태

- Pending(대기) : 비동기 처리 로직이 아직 완료되지 않은 상태
- Fulfilled(이행) : 비동기 처리가 완료되어 프로미스가 결과 값을 반환해준 상태
- Rejected(실패) : 비동기 처리가 실패하거나 오류가 발생한 상태

```js
new Promise() // Pending

new Promise(function(resolve, reject) {
  resolve(); // Fulfilled
});


new Promise(function(resolve, reject) {
  reject(); // Rejected
});
```

#### 실제 예시

```js
function getData() {
  return new Promise(function(resolve, reject) {
    $.get('url 주소/products/1', function(response) {
      if (response) {
        resolve(response); // response -> data
      }
      reject(new Error("Request is failed"));
    });
  });
}

// 위 $.get() 호출 결과에 따라 'response' 또는 'Error' 출력
getData().then(function(data) {
  console.log(data); // response 값 출력
}).catch(function(err) {
  console.error(err); // Error 출력
});
```

### 여러개의 프로미스 사용(Promise chaining)

```js
new Promise(function(resolve, reject){
  setTimeout(function() {
    resolve(1);
  }, 2000);
})
.then(function(result) {
  console.log(result); // 1
  return result + 10;
})
.then(function(result) {
  console.log(result); // 11
  return result + 20;
})
.then(function(result) {
  console.log(result); // 31
});
```

Promise 객체는 then을 호출할 수 있고 then을 호출하면 또 Promise 객체를 리턴하니까 계속해서 then을 사용할 수 있고 계속해서 인자를 넘겨줄 수 있다.

#### 실무 예시

```js
getData(userInfo)
  .then(parseValue)
  .then(auth)
  .then(diaplay);

var userInfo = {
  id: 'test@abc.com',
  pw: '****'
};

function parseValue() {
  return new Promise({
    // ...
  });
}
function auth() {
  return new Promise({
    // ...
  });
}
function display() {
  return new Promise({
    // ...
  });
}
```

### 프로미스 에러 처리 방법

에러처리 방법엔 2가지가 있다

- then()의 두번째 인자 사용하기
  
  ```js
  getData().then(
  handleSuccess,
  handleError
  );
  // then()의 두 번째 인자로는 감지하지 못하는 오류
  function getData() {
   return new Promise(function(resolve, reject) {
   resolve('hi');
   });
  }
  
  getData().then(function(result) {
   console.log(result);
   throw new Error("Error in then()"); // Uncaught (in promise) Error: Error in then()
  }, function(err) {
   console.log('then error : ', err);
  });
  ```

```

```

![](https://joshua1988.github.io/images/posts/web/javascript/then-not-handling-error.png)

> then의 콜백함수에서의 오류를 똑바로 잡아내지 못함.

- catch() 사용하기

```js
getData().then().catch(); 
// catch()로 오류를 감지하는 코드
function getData() {
 return new Promise(function(resolve, reject) {
 resolve('hi');
 });
} 
getData().then(function(result) {
 console.log(result); // hi
 throw new Error("Error in then()");
}).catch(function(err) {
 console.log('then error : ', err); // then error : Error: Error in then()
});
```

![](https://joshua1988.github.io/images/posts/web/javascript/catch-handling-error.png)

> catch를 사용하면 에러를 잘 잡아내는 모습.

결론은 두 가지 방법이 있는데, catch를 이용하는 게 더 많은 예외처리 상황을 처리하는데 도움이 될 것이다.

### 출처

https://joshua1988.github.io/web-development/javascript/promise-for-beginners/ (캡틴판교)
