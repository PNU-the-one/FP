시작하며.. https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/dashboard
김영한 선생님의 강의를 보고 끄적입니다.. 최고에오

# Spring Boot란?
- Spring framework 기반의 프로젝트를 복잡한 설명없이 쉽고 빠르게 만들어주는 라이브러리

## 그러면 Spring은 뭔가
- Java언어 기반의 웹 프레임워크로 다양한 어플케이션을 만들기 위한 틀인데 Java를 활용한 다양한 기술(JSP, JPA등등)을 더 편하게 사용하기 위해 만들어진 틀이다. 
- 주로 중복 코드의 사용률을 줄여주고 비지니스 로직을 더 간단하게 해준다.
- 오픈 소스를 좀더 효율적으로 가져다 쓰기 좋은 구조

## Spring의 주요 특징
1. IoC(Inversion of Control) - 객체 생명주기 관리를 해줌
2. DI(Dependency Injection) - 의존성 주입
3. AOP(Aspect Object Programming) - 기능을 분리하여 관리 가능


IDE : IntelliJ or Eclipse

스프링 부트 스타터 사이트에서 간단하게 프로젝트 생성 가능
https://start.spring.io/

## 주요 라이브러리
### 스프링 부트 라이브러리
spring-boot-starter-web
spring-boot-starter-tomcat: 톰캣 (웹서버)
spring-webmvc: 스프링 웹 MVC
spring-boot-starter-thymeleaf: 타임리프 템플릿 엔진(View)
spring-boot-starter(공통): 스프링 부트 + 스프링 코어 + 로깅
spring-boot
spring-core
spring-boot-starter-logging
logback, slf4j
### 테스트 라이브러리
spring-boot-starter-test
junit: 테스트 프레임워크
mockito: 목 라이브러리
assertj: 테스트 코드를 좀 더 편하게 작성하게 도와주는 라이브러리
spring-test: 스프링 통합 테스트 지원

## View 경로
- 기본적으로 spring boot는 static/index.html에 기본 view 정보를 입력해 둘 수 있다.

## Spring Boot 기본 동작 방식
![](https://velog.velcdn.com/images/rudgns9334/post/bc645da8-1385-439e-a6a8-fe3f68eb4105/image.png)


## 스프링 웹 개발 기초
1. 정적 컨텐츠
2. MVC와 템플릿 엔진
3. API

### 정적 컨텐츠
- 말그대로 그대로 가져다가 쓰는 형식
![](https://velog.velcdn.com/images/rudgns9334/post/d46103b8-3be6-4601-bc4b-cf951e211b5d/image.png)

### MVC와 템플릿 엔진
- M: model V: view C: controller에 의해  동작하는 동적 동작 방식
- 그리고 그것을 템플릿 엔진이 html로 변환해주는 방식
![](https://velog.velcdn.com/images/rudgns9334/post/08a24513-734b-4d59-8a27-cb3bcc7d4bfd/image.png)

### API
- 지금까지 거쳐온 ViewResolver를 거치지 않고 바로 HTTP의 BODY에 내용을 직접 반환한다.
- 기본적으로 JSON으로 반환되며 @ResponseBody 라는 애노테이션사용
![](https://velog.velcdn.com/images/rudgns9334/post/a7390e2b-80a1-49c5-b761-1c43b62381c2/image.png)

## 스프링 빈과 의존 관계
1. 컴포넌트 스캔과 자동 의존관계 설정
2. 자바 코드로 직접 스프링 빈 등록
- 의존관계를 형성하는 것을 DI(Dependency Injection)라고 하는데 MVC 방식에서 스프링에서 각자 필요한 객체를 찾아가기 위해 다리를 놓아주는 작업이다.
<br>
- 설명에 참고하기 위한 일반적인 웹 애플리케이션 계층 구조를 보자.
![](https://velog.velcdn.com/images/rudgns9334/post/e222386d-1a2c-49a9-9835-920fea238358/image.png)
- 컨트롤러 : 웹 MVC의 컨트롤러 역할
- 서비스 : 핵심 비즈니스 로직 구현
- 레포지토리 : 데이터베이스에 접근, 도메인 객체를 DB에 저장하고 관리
- 도메인 : 비즈니스 도메인 격체, 예) 회원, 주문, 쿠폰 등등 주로 데이터베이스에 저장하고 괸리되는 대상

### 컴포넌트 스캔
- 스프링에서는 Controller를 만들게 되면 처음 Spring이 실행될 때 Container라는 통이 생성되는데 그 안에 Controller를 객체 형태(Bean)로 넣어둔다.
- 컨트롤러 뿐만 아니라 여러 필요 서비스나 레포지토리 등등도 객체로 넣어둠
- 이때 여러 컨트롤러에서 사용되는 공용 객체(서비스)를 Container안에 한번만 선언해두고 공동으로 사용하는 것이 좋다.(이를 연결해주는 다리가 @Autowired)
- 원하는 서비스나 레포지토리에 애노테이션을 추가하여 wire를 연결할 수 있다.
- 컴포넌트 스캔은 같은 패키지 내부에서만 검색을 진행한다.

### 자바 코드로 직접 빈 등록
 - 굳이 서비스나 레포지토리를 등록하지 않고 따로 Config를 만들어서 Bean으로 직접 등록하는 방법
 - 직접 등록해두는 경우 서비스나 레포지토리가 변경될 때 쉽게 바꿔 줄 수 있다.
 
---------------------
- 정형화된 실무에서는 컨트롤러, 서비스, 레포지토리같은 코드를 컴포넌트 스캔을 이용하영 등록하지만 상황에 따라 구현 클래스를 변경해야 하면 설정을 통해 스프링 빈으로 등록하여 사용하기도 한다.

## DB의 성장과정
1. 순수 JDBC - 마치 바닐라 자바스크립트...
2. 스프링 JdbcTemplate - 위의 JDBC에서 반복적으로 사용되는 코드를 제거해줌
3. JPA - 반복 코드는 물록 기본적인 SQL도 직접 만들어서 실행해줌
4. 스프링 데이터 JPA - 마법임

### JPA
- 기존의 반복 코드는 물론이고 기본적인 SQL도 JPA가 직접 만들어서 실행
- SQL과 데이터 중심의 설계에서 객체 중심의 설계로 패러다임을 전환
- JPA를 사용하면 개발 생산성을 크게 높힘
- 실무에서 많이 사용되고 있으며 공부하기 시작하면 끝이 없대요...

### 스프링 데이터 JPA
- JPA만 사용해도 개발 생산성이 많이 증가하고 코드도 확연히 줄어들지만 스프링 데이터 JPA를 사용하면 레포지토리에 구현 클래스 없이 인터페이스만으로 개발을 완료할 수 있음
- CRUD기능은 물론 웬만한 모든 기능이 다 구현되어 있다.
- 개발자는 핵심 비즈니스 로직 개발에만 집중할 수 있다.
- 하지만 스프링 데이터 JPA는 JPA를 편리하게 사용할 수 있도록 도와주는 기술일 뿐 JPA를 모르는 상태에서 사용하면 무용지물
-----------

## AOP
- 만약 1000개의 메소드의 호출 시간을 측정하고 싶다면?
기존 : 1000개의 메소드를 모두 찾아가 해당 로직 적용
	- 문제점
    	1. 시간을 측정하는 기능은 핵심 관심 사항이 아님
        2. 시간을 측정하는 로직은 공통 관심 사항
        3. 시간을 측정하는 로직과 핵심 비즈니스의 로직이 섞여서 유지보수가 어려움
        4. 시간을 측정하는 로직을 별도의 공통 로직으로 만들기 매우 어렵다.
        5. 시간을 측정하는 로직을 변경할 때 모든 로직을 찾아가면서 변경해야함
<br>
- 이러한 문제점을 해결하기 위해 AOP(Aspect Oriented Programming)존재
![](https://velog.velcdn.com/images/rudgns9334/post/dd2cc6d8-2d6a-49ed-8d59-f358108bba38/image.png)

- 공통 관심 사항(cross-cutting concern)과 핵심 관심 사항(core concern)을 분리
- 공통 관심 사항을 별도의 로직으로 구현하여 뿌려주기
- 핵심 관심 사항만 깔끔하게 유지 가능
- 원하는 적용 대상을 선택할 수 있고 변경이 필요할 때 해당 로직만 변경하면 된다.
![](https://velog.velcdn.com/images/rudgns9334/post/1b5225fc-3fb8-40cd-b2b1-f798ec66a098/image.png)

# 마치며
- 이제 처음 시작이긴하지만 강의들으면서 공부를 해야할 것이 너무나도 많다고 느꼈다. 
- 차근차근 정리해가며 기초부터 (여차하면 Java부터..) 쌓아가야겠다...
- JPA나 DB, SQL너무 어지러울듯 허허
- 난 ㅁㅏㅇㅎㅐㅆㅇㅓ