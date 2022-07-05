스프링과 DB를 공부하다가 SQL에 막혀부렀다.
하다보니 나 DB에서 해결해야하는게 있었다.
까먹고있었다.

## SQL이란??
Structured Query Language로 관계형 데이터베이스 관리 시스템(RDBMS)의 데이터를 관리하기 위해 설계된 특수 목적의 프로그래밍 언어이다.
자료의 검색과 관리, 데이터베이스 스키마 생성과 수정, 데이터베이스 객체 접근 조정 관리를 위해 사용된다.

## SQL의 종류
1. 데이터 정의 언어(DDL : Data Definition Language)
2. 데이터 조작 언어(DML : Data Manipulation Language)
3. 데이터 제어 언어(DCL : Data Control Language)

### 데이터 정의 언어(DDL)
- 스키마, 도메인, 테이블, 뷰, 인덱스를 정의하거나 변경 또는 삭제할 때 사용하는 언어
- 데이터 구조를 정의
- 주로 DB 관리자 및 설계자가 사용
- 대표적으로 CREATE, ALTER, DROP등이 있음
	1) CREATE : 생성 (CREATE 생성대상 이름 (요소);)
    2) ALTER : 변경 (ALTER 변경대상 이름 ADD 변경요소;)
    3) DROP : 삭제 (DROP 삭제대상 이름;)
    4) TRUNCATE : 내부 데이터만 삭제(TRUNCATE 삭제대상 이름;)
    

### 데이터 조작 언어(DML)
- 데이터베이스 사용자가 응용 프로그램이나 질의어를 통해 저장된 데이터를 처리할 때 사용
- DBMS와 User간의 인터페이스 제공
	1) SELECT : 선택
    	- SELECT 컬러명 FROM 테이블명
    2) INSERT : 삽입
    	- INSERT INTO 테이블명(칼럼명들) VALUES(값들)
    3) DELETE : 삭제
    	- DELETE FROM 테이블명 WHERE 조건
    4) UPDATE : 갱신, 변경
    	- UPDATE 테이블명 SET 컬럼명 = 변경할 값
- 각 구간에서 WhERE등의 명령어를 통해 조건을 넣을 수 있다.

### 데이터 제어 언어(DCL)
- 데이터의 보안, 무결성, 회복, 병행 수행 제어 등을 정의할 때 사용되는 언어.
- 관리자가 관리를 목적으로 사용
	1) COMMIT : 작업완료 알림
    2) ROLLBACK : 백업
	3) GRANT : 권한부여 
    4) REVOKE : 권한취소
- 이 외에도 LOCK, SAVEPOINT, SET TRANSACTION등 많다...

----------------------------
## 그 외

### 자료형
1. 숫자형
	1) TINYINT : 가장 작은 숫자 자료형(1byte)
    2) SMALLINT : 2bytes 크기의 숫자 자료형
    3) MEDIUMINT : 3bytes 크기의 숫자 자료형
    4) INT : 주로 사용하는 숫자 자료형(4bytes). INTEGER로도 쓰임
    5) BIGINT : 8bytes 크기의 숫자 자료형
    6) FLOAT : 위 내용들이 정수만 취급한다면 이것은 소수까지 가능(4bytes)
    7) DOUBLE : FLOAT 상위버전 (8bytes)
    8) DECIMAL : 소수를 저장하는 용도로 사용. 내부적으로는 문자 형태로 저장되는 타입. 예를 들어, 3.14면 숫자하나하나를 나눠서 char로 저장
    

2. 문자형
	1) CHAR : 무제한 길이의 문자형
    2) VARCHAR : 길이를 제한 ex) VARCHAR(10)하면 10자리
    3) TINYBLOB + 1byte
    4) TINYTEXT
    5) BLOB : 65535개의 문자 저장 + 2bytes
    6) TEXT : BLOB과 동일
    7) MEDIUMBLOB + 3bytes
    8) MEDIUMTEXT
    9) LONGBLOB + 4bytes
    10) LONGTEXT
    11) ENUM : 입력한 문자형태의 값을 숫자로 저장
    

3. 날짜형
	1) DATE : YYYY-MM-DD
    2) DATETIME : YYYY-MM-DD HH:MM:SS
    3) TIMESTAMP : 1970-01-01 00:00:00이후 부터 초를 숫자로 저장	
    4) TIME : HH:MM:SS
    5) YEAR : 연도만 저장
    
    ---------------------
    SQL 정리하다가 생각 낫지만 내가 에러난건 외래키 제약조건이었을탠데
   ...
   다음 주제는 요고탓