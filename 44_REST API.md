# 44장 REST API
- REpresentational State Transfer
- HTTP의 장점을 최대한 활용할 수 있는 아키텍처
- HTTP 프로토콜을 의도에 맞게 디자인하도록 유도함
- REST의 기본 원칙을 성실히 지킨 서비스 디자인을 "RESTful" 이라고 표현함
- 즉, REST는 **HTTP를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처**
- REST API는 REST를 기반으로 서비스 API를 구현한 것

## 1️⃣ 44.1 REST API의 구성
- 자원, 행위, 표현의 3가지 요소로 구성됨
- 자체 표현 구조로 구성되어 REST API 만으로 HTTP 요청의 내용을 이해할 수 있음

| 구성 요소 | 내용 | 표현 방법 |
| ------- | ----- | ------- |
| 자원 | 자원 | URI(엔드포인트) | 
| 행위 | 자원에 대한 행위 | 	HTTP 요청 메서드 |
| 표현 | 	자원에 대한 행위의 구체적 내용 |	페이로드 |

## 2️⃣ 44.2 REST API 설계 원칙
1. URI는 리소스를 표현해야 한다
  - 리소스를 식별할 수 있는 이름은 동사보다 명사를 사용한다
  - 이름에 get 같은 행위에 대한 표현이 들어가서는 안됨
2. 리소스에 대한 행위는 HTTP 요청 메서드로 표현한다
  - HTTP 요청 메서드는 클라이언트가 서버에게 요청의 종류와 목적을 알리는 방법
  - 주로 5가지 요청 메서드를 사용해 CRUD를 구현한다

```
#bad
GET /getTodos/1
GET /todos/show/1

#good
GET /todos/21
```

## 3️⃣ 44.3 JSON Server를 이용한 REST API 실습
| HTTP 요청 메서드 |	종류 | 	목적 |	페이로드 |
| ------- | ----- | ------- | -------- | 
| GET | 	index/retrieve | 	모든/특정 리소스 취득 | 	X |
| POST | 	create | 	리소스 생성 | O |
| PUT |	replace	| 리소스의 전체 교체 |	O |
| PATCH |	modify | 리소스의 일부 수정 |	O |
| DELETE | 	delete |	모든/특정 리소스 삭제 |	X |
