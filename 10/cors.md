## CORS
CORS(Cross Origin Resource Sharing)는 다른 출처를 가진 곳에 리소스 공유를 할 수 있게 도와주는 정책으로 한 도메인 또는 Origin의 웹 페이지가 다른 도메인을 가진 리소스에 접근할 수 있게 해주는 보안 메커니즘이다. 

## SOP(Same-Origin Policy)
 웹 생태계에는 리소스 요청을 제한하는 것과 관련된 두가지 정책이 존재하는데 위의 CORS와 SOP이다.
 CORS가 다른 출처로 자료 요청을 하는 정책이라면 SOP는 말 그대로 같은 출처에서만 자료를 공유할 수 있다는 규칙을 가진다.
 SOP에서 예외 조항을 두고 이에 해당하는 경우에만 다른 출처에 요청하는 행위를 허용했는데 그중 하나가 CORS이다.

### 출처(Origin)가 뭘까?
![](https://velog.velcdn.com/images/rudgns9334/post/b2f27381-8ee8-459f-87d2-c6f6b6ef18d6/image.png)
https://evan-moon.github.io/2020/05/21/about-cors/ 에서 빌려온 사진
url들은 위 사진과 같이 구성되어 있다.
여기서 출처(origin)은 Protocol과 host 그리고 포트 번호까지 모두 합친 것을 의미한다. (포트 번호가 생략되어 있는 것은 HTTP 정의 문서 참고하자면 기본 포트 번호가 함께 정의되어 있다고..)
![](https://velog.velcdn.com/images/rudgns9334/post/4125212b-966c-455b-9cde-2641a41a5115/image.png)

결론적으로 같은 출처라고 한다면 Protocol, host, port까지 모두 동일해야 같은 출처라고 할 수 있다.

 ## CORS로 돌아와서...
 - CORS는 서버에 구현된 로직이 아닌 브라우저에 구현된 로직
 - 고로 서버간의 통신에서는 CORS는 적용되지 않는다.
 - 서버쪽에는 로그도 남지 않는다.
 
 ## CORS의 동작
 1. 클라이언트에서 요청을 보낼때 요청 헤더의 Origin이라는 필드에 요청을 보내는 출처를 함께 담아 보낸다.
 2. 서버는 이 요청에 대한 응답을 할 때 응답 헤더의 Access-Control-Allow-Origin이라는 값에 이 자료를 접근하는 것이 허용된 출처를 같이 보내준다.
 3. 이후 응답을 받은 브라우저에서 Origin과 Access-Control-Allow-Origin을 비교하여 응답이 유효한지를 판단한다.
 
 ### CORS의 시나리오
 
 #### Preflight Request
 - 브라우저가 요청을 보낼때 한번에 보내지 않고 예비 요청과 본 요청으로 나누는 방식
 - 예비 요청에는 HTTP 메소드중 OPTIONS 메소드가 사용
 - 예비 요청은 본 요청을 보내기전에 브라우저 스스로 이 요청을 보내는 것이 안전한지 확인 하는 역할
 ![](https://velog.velcdn.com/images/rudgns9334/post/e426d2fd-4741-44db-99f2-8bb5c7ad48ca/image.png)
- 자바스크립트의 fetch

#### Simple Request
- 예비 요청을 보내지 않고 바로 서버에 본 요청을 때려박고 서버의 응답에 따라 브라우저가 검사하는 방식
- 전체적인 로직은 Preflight Request와 같지만 예비 요청의 존재 유무만 다름
![](https://velog.velcdn.com/images/rudgns9334/post/c8e56c36-f554-4061-93cb-c2099ba01bdd/image.png)
- 하지만 예비 요청을 생략하기 위해선 특정 조건을 만족 해야한다.
	1. 요청의 메소드는 GET, HEAD, POST중 하나이여야 한다.
    2. Accept, Accept-Language, Content-Language, Content-Type, DPR, DOwnlink, Save-Data, Viewport-Width, Width를 제외한 헤더를 사용하면 안된다.
    3. 만약 Content-Type를 사용하는 경우에는 application/x-www-form-urlencoded, multiport/form-data, text/plain만 허용된다.
    
#### Credentialed Request
- 인증된 요청을 사용하는 방법
- 다른 출처간 통신에서 좀 더 보안을 강화하고 싶을 때 사용

## CORS 해결 방법
### 서버에서의 해결법
- 정석대로 서버에서 Access-Control-Allow-Origin 헤더에 알맞은 값 세팅하기
- *로 퉁쳐서 모든 출처를 받아먹어도 되지만... 보안상 위험
- 정확한 url명시해주는 것을 추천
- Spring Boot의 경우 @CrossOrigin("url") 이노테이션 사용
```
@CrossOrigin("http://localhost:8080")
@RestController
public class GreetingController {

  @GetMapping("/hello")
  public String hello() {
    return "안녕하세요?";
  }
}
```
- 아니면 WebConfig에 CORS 설정
```
@Configuration
public class WebConfig implements WebMvcConfigurer {

  @Override
  public void addCorsMappings(CorsRegistry registry) {
    registry.addMapping("/**")
        .allowedOrigins("http://localhost:8080");
  }
}
```

### 클라이언트에서의 해결법
- proxy설정하기
```
module.exports = {
    mode: "development",
    devServer: {
        static : "./dist",
        proxy: {
            '/todo' : {
                target: 'http://localhost:3000',
                changeOrigin: true
            },
            '/list' : {
                target: 'http://localhost:3000',
                changeOrigin: true
            }
        }
    }
```


# 끝
서버 공부하려고 혼자 프론트쪽 만지다가 만난 문제.
서버 문제인줄 알고 공부를 시작하였으나 하다보니 브라우저 문제인걸 깨달았지만 결국 서버에서도 세팅해야하는 값이 존재하니 좋은 공부였다.
알아서 안좋을 것 없는 공부!