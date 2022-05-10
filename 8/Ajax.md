# Ajax

Asynchronous Javascript And Xml

- 비동기식 자바스크립트와 XML

- 자바스크립트를 이용해 서버와 브라우저가 비동기 방식으로 데이터를 교환할 수 있는 통신 기능

- 브라우저가 가지고있는 XMLHttpRequest 객체를 이용해서 전체 페이지를 새로 고치지않고도 페이지의 일부만을 위한 데이터를 로드하는 기법

> JS로 서버에 데이터를 비동기 방식으로 요청

## AJAX를 사용 가능하게 만드는 것들

- HTML

- DOM

- JS

- XMLHttpRequest

- 기타...

## AJAX를 사용하는 이유

단순하게 WEB화면에서 무언가 부르거나 데이터를 조회하고 싶을 때, **페이지 전체를 새로고침하면 많은 자원과 시간이 든다. 그럴 때 새로고침하지 않기 위해서 AJAX를 사용한다**. AJAX는 XMLHttpRequest객체를 통해서 서버에 request하고 JSON이나 XML형태로 필요한 데이터만 받기 때문에 그만큼의 자원과 시간을 아낄 수 있다.

## AJAX의 장단점

#### 장점

- 속도향상
  
  - 서버의 처리를 기다릴 필요가없다.

- 서버에서 Data만 전송하면 되므로 코딩의 양이 줄어든다.

- 기존 웹에서 불가능했던 다양한 UI를 가능하게 해준다. 
  
  > 사진의 제목이나 태그를 페이지의 리로드 없이 수정가능 ex) Flicker

#### 단점

- 히스토리 관리가 안된다.

- 페이지 이동없는 통신으로 인한 보안상의 문제가 있다.

- 연속으로 데이터를 요청하면 서버 부하 증가

- XMPHttpRequest 를 통해 통신하는 경우, 사용제에 아무런 진행 정보가 주어지지 않는다.

- Cross-Domain 문제

## Fetch API

- fetch

- callback

- response

#### fetch()

```js
fetch(url, [options])
// url 접근하고자 하는 url
// options - 선택 매개변수, method나 header 등을 지정할 수 있음

const options = {
    method: "GET",
    headers: *,// 
    body: *,//
}
```

#### fetch method

- POST
  
  - 리소스 생성

- GET (default)
  
  - 리소스 조회

- PUT
  
  - 리소스 수정(전체)

- PATCH
  
  - 리소스 수정(부분)

- DELETE
  
  - 리소스 삭제

> body를 json형태로 요청을 보내면 header에다가 명시를 해줘야한다.

### Callback

특정 이벤트 이후에 호출 되는 함수.

```js
fetch(...).then(callback)

callback(response){
...
}
```



### Response

fetch를 통해 요청하고 서버로 부터 응답을 받으면 .then을 통해 함수의 인자에 담기게 된다. 이 값은 Response객체로서 여러가지 정보들을 담고 있다.



- response.status - HTTP 상태 코드 (200)

- response.ok - HTTP 상태 코드가 200~299 일 경우 true

- response.body - 내용

- response.text() - 응답을 읽고 텍스트로 반환한다.

- response.json() - 응답을 JSON 형태로 파싱한다.

- response.formData() - 응답을 FormData 객체 형태로 변환한다.



> 응답 자료 형태 반환 메서드는 한번의 요청에 한번만 사용가능하다.
> 
> 만약 text()를 사용해서 응답을 얻었다면 본문의 콘텐츠가 모두 처리 된 상태여서 json() 써줘도 동작하지 않는다.





#### Async / Await 를 이용하기

```js
async function 함수명() {
    await 비동기_처리_메서드_명()
}

async function post(...){
    const options = {
        method: "POST"
        headers: {
            "Content-Type": "application/json",
            ...headers,
        },
        body: JSON.stringify(body),
    }
    const res = await fetch(url,options)
    ...
    if(res.ok){
    ...
    }
    else(){
    ...
    }
}
```
