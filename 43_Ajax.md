# 43장 Ajax

## 1️⃣ 43.1 Ajax란?
- 자바스크립트를 사용하여 브라우저가 서버에게 **비동기 방식**으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여 웹페이지를 **동적**으로 갱신하는 프로그래밍 방식
- 브라우저에서 제공하는 Web API인 XMLHttpRequest 객체 ( HTTP 통신을 위한 메서드와 프로퍼티 제공 ) 를 기반으로 동작함
- 서버로부터 웹페이지의 변경에 필요한 데이터만 비동기 방식으로 전송받아 웹페이지를 변경할 필요가 없는 부분은 다시 렌더링하지 않고, 변경이 필요가 있는 부분만 한정적으로 렌더링하는 방식이 가능해짐
- 브라우저에서도 데스크톱 애플리케이션과 유사한 빠른 퍼포먼스와 부드러운 화면 전환이 가능해짐

### 📍 전통적인 웹페이지 생명 주기
- 이전의 웹페이지는 html태그로 시작해서 html 태그로 끝나는 완전한 HTML을 서버로부터 전송받아 웹페이지 전체를 처음부터 다시 렌더링하는 방식으로 동작함
- 이전 웹페이지와 차이가 없어서 변경할 필요가 없는 부분까지도 포함된 완전한 HTML을 서버로부터 매번 다시 전송받기 때문에 불필요한 데이터 통신이 발생함
- 변경할 필요가 없는 부분까지 처음부터 다시 렌더링함. 이로 인해 화면 전환이 일어나면 화면이 순간적으로 깜박이는 현상이 발생함
- 클라이언트와 서버와의 통신이 동기 방식으로 동작하기 때문에 서버로부터 응답이 있을 때까지 다음 처리는 블로킹됨

### 📍 Ajax의 장점
- 변경할 부분을 갱신하는 데 필요한 데이터만 서버로부터 전송받기 때문에 불필요한 데이터 통신이 발생하지 않음
- 변경할 필요가 없는 부분은 다시 렌더링 하지 않음. 따라서 화면이 순간적으로 깜박이는 현상이 발생하지 않음
- 클라이언트와 서버와의 통신이 비동기 방식으로 동작하기 때문에 서버에게 요청을 보낸 이후 블로킹이 발생하지 않음
  
## 2️⃣ 43.2 JSON
- 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷
- **키와 값**으로 구성된 순수한 텍스트
### 43.2.1 JSON 표기 방식
### 43.2.2 JSON.stringify
- 객체나 배열을 JSON 포맷의 문자열로 변환가능
- 직렬화 : 클라이언트가 서버로 객체를 전송할 때객체를 문자열화하는 과정
### 43.2.3 JSON.parse
- JSON 포맷의 문자열을 객체로 변환함
- 반직렬화

## 3️⃣ 43.3 XMLHttpRequest
- 자바스크립트를 사용하여 HTTP 요청을 전송할 때 사용하는 객체
- HTTP 요청 전송과 HTTP 응답 수신을 위한 다양한 메서드와 프로퍼티를 제공함
- XMLHttpRequest 객체를 XMLHttpRequest 생성자 함수를 호출하여 생성함
  
### 43.3.1 XMLHttpRequest 객체 생성
### 43.3.2 XMLHttpRequest 객체의 프로퍼티와 메서드
- 프로퍼티
  - readyState
  - status
  - stausTest
  - responseType
  - response
  - responseText
- 이벤트 핸들러 프로퍼티
  - onreadystatechange
  - onloadstart
  - onprogress
  - onabort
  - onerror
  - onload
  - ontimeout
  - onloadend
- 메서드
  - open
  - send
  - abort
  - setRequestHeader
  - getResponseHeader
- 정적 프로퍼티
  - UNSENT
  - OPENED
  - HEADERS_RECEVIED
  - LOADING
  - DONE
 

### 43.3.3 HTTP 요청 전송
1. XMLHttpRequest.prototype.open 메서드로 HTTP 요청을 초기화함
2. 필요에 따라 XMLHttpRequest.prototype.setRequestHeader 메서드로 특정 HTTP 요청의 헤더 값을 설정함
3. XMLHttpRequest.prototype.send 메서드로 HTTP 요청을 전송함

```
const xhr = new XMLHttpRequest();

xhr.open('GET','/users');

xhr.setRequestHeader('content-type', 'application/json');

xhr.send();
```
### 43.3.4 HTTP 응답 처리
- 언제 응답이 클라이언트에 도달할지 알 수 없어 readystatechange 이벤트를 통해 HTTP 요청의 현재 상태를 확인해야 함
- readystatechange 이벤트는 HTTP 요청의 현재 상태를 나타내는 readystate 프로퍼티가 변경될 때마다 발생함
- onreadystatechange 이벤트는 HTTP 요청의 현재 상태를 나타내는 readState가 XMLHttpRequest.DONE인지 확인하여 서버의 응답이 완료되었는지 확인함
- 서버의 응답이 완료되면 statue가 200인지 확인하여 정상 처리와 에러 처리를 구분함
