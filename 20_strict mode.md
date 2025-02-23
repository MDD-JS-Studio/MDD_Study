# 20장 strict mode

## 1️⃣ 20.1 strict mode란?

```
function foo() {
    x = 10;
}

foo();

console.log(x);
```

1. 자바스크립트 엔진은 먼저 foo 함수의 스코프에서 x 변수의 선언을 검색
2. foo 함수의 스코프에는 x 변수의 선언이 없으므로 검색에 실패할 것이고, 자바스크립트 엔진은 x 변수를 검색하기 위해 foo 함수 컨텍스트의 상위 스코프에서 x 변수의 선언을 검색
3. 전역 스코프에도 x 변수의 선언이 존재하지 않기 때문에 `ReferenceError`를 발생시킬 것 같지만 자바스크립트 엔진은 **암묵적으로 전역 객체에 x 프로퍼티를 동적 생성**
   - 이때 전역 객체의 x 프로퍼티는 마치 전역 변수처럼 사용 가능
   - 이러한 현상을 `암묵적 전역`이라고 함

- 개발자의 의도와는 상관없이 발생한 암묵적 전역은 오류를 발생시키는 원인이 될 가능성이 큼.
  - 따라서 반드시 var, let, const 키워드를 사용하여 변수를 선언한 다음 사용해야 함
- 이라힌 오류를 해결하기 위해 ES5부터 `strict mode(엄격 모드)`가 추가됨

  ***

- `strict mode`는 자바스크립트 언어의 문법을 좀 더 엄격히 적용하여 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작접에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킴
  - `ESLint` 같은 린트 도구를 사용해도 `strict mode`와 유사한 효과를 얻을 수 있음
  - 린트 도구는 **정적 분석** 기능을 통해 소스코드를 실행하기 전에 소스코드를 스캔하여 문법적 오류만이 아니라 잠재적 오류까지 찾아내고 오류의 원인을 리포팅해주는 도구
  - 린트 도구는 `strict mode`가 제한하는 오류는 물론 코딩 컨벤션을 설정 파일 형태로 정의하고 강제할 수 있기 때문에 더욱 강력한 효과를 얻을 수 있음

<br/>

## 2️⃣ 20.2 strict mode의 적용

- 전역의 선두 또는 함수 몸체의 선두에 `use strict` 추가

  ```
  'use strict';

  function foo() {
      x = 10;  // ReferenceError: x is not defined
  }
  foo();
  ```

<br/>

## 3️⃣ 20.3 전역에 strict mode를 적용하는 것은 피하자

- 전역에 적용한 `strict mode`는 스크립트 단위로 적용됨

```
<!DOCTYPE html>
<html>
<body>
  <script>
    'use strict';
  </script>
  <script>
    x = 1; // 에러가 발생하지 않는다.
    console.log(x); // 1
  </script>
  <script>
    'use strict';

    y = 1; // ReferenceError: y is not defined
    console.log(y);
  </script>
</body>
</html>
```

- 위 예제와 같이 스크립트 단위로 적용된 `strict mode`는 다른 스크립트에 영향을 주지 않고 해당 스크립트에 한정되어 적용됨

<br/>

## 4️⃣ 20.4 함수 단위로 strict mode를 적용하는 것도 피하자

- 어떤 함수는 `strict mode`를 적용하고 어떤 함수는 `strict mode`를 적용하지 않는 것은 바람직하지 않음
- 또한 모든 함수에 일일이 `strict mode`를 적용하는 것은 번거로운 일
- 또한 `strict mode`가 적용된 함수가 참조할 함수 외부의 컨텍스트에 `strict mode`를 적용하지 않는다면 문제 발생 가능
- 따라서 `strict mode`는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직

<br/>

## 5️⃣ 20.5 strict mode가 발생시키는 에러

### 📍 20.5.1 암묵적 전역

- 선언하지 않은 변수를 참조하면 `ReferenceError` 발생

### 📍 20.5.2 변수, 함수, 매개변수의 삭제

- `delete` 연산자로 변수, 함수, 매개변수를 삭제하면 `SyntaxError` 발생

### 📍 20.5.3 매개변수 이름의 중복

- 중복된 매개변수 이름을 사용하면 `SyntaxError` 발생

### 📍 20.5.4 with 문의 사용

- with 문을 사용하면 `SyntaxError` 발생

<br/>

## 6️⃣ 20.6 strict mode 적용에 의한 변화

### 📍 20.6.1 일반 함수의 this

- strict mode에서 함수를 일반 함수로 호출하면 this에 `undefined`가 바인딩
- 생성자 함수가 아닌 일반 함수 내부에서는 this를 사용할 필요가 없기 때문

### 📍 20.6.2 arguments 객체

- 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않음
