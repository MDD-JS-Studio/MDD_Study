# 22장 this 키워드 

# 1. this 키워드란?

> #### 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기참조 변수.

- `this`를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있음.

- js 엔진에 의해 암묵적으로 생성되며, 코드 어디서나 참조 가능

- this 바인딩은 함수 호출 방식에 의해 동적으로 결정.

`(* this 바인딩 : this(키워드이지만, 식별자 역할을 함)와 this가 가리킬 객체를 바인딩 하는 것. 여기서 바인딩이란, 식별자와 값을 연결하는 과정)`

<br/>
 

java나 c++같은 `클래스 기반 언어`에서 this는 언제나 클래스가 생성하는 인스턴스를 가리키지만, 

자바스크립트의 this는 
`함수가 호출되는 방식`에 따라 `this 바인딩이 동적으로 결정`되며, strict mode도 영향을 준다.

`
strict mode가 적용된 일반 함수 내부 this에는 undefined 가 할당되는데, 이는 일반함수 내부에서는  this 사용할 필요가 없기 때문이다.
`

<br/><hr/>

# 2. this를 사용하는 이유
> `특수한 식별자`의 역할을 수행하기 위함인데, 자신이 속한 객체나 자신이 생성할 인스턴스를 가리키는 역할을 이 this가 수행함.

`객체`는 상태를 나타내는 프로퍼티와 동작을 나타내는 메서드를 하나로 묶은 자료구조로, 동작을 나타내는 `메서드`는 `프로퍼티를 참조하고 변경` 할 수 있어야 하지만

그러기 위해서는 먼저 `자신이 속한 객체를 가리키는 식별자를 참조`할 수 있어야 함.

허나 생성자 함수 정의 시점에는 인스턴스를 생성하기 이전이기 때문에 
생성할 인스턴스를 가리키는 식별자를 미리 알 수가 없음.

이러한 이유로 인해
<br/>

`this 키워드를 사용하는 것이며, 자신이 속한 객체나 생성할 인스턴스를 이 this 키워드를 통해서 가리킬 수 있음.`

<br/><hr/>
 

 

# 3. 함수 호출방식들
> 함수 호출 방식에는 아래와 같은 방법들이 존재한다.

- 일반 함수 호출
- 메서드 호출
- 생성자 함수 호출
- `prototype`에 속한 `apply`, `call`, `bind` 메서드들에 의한 간접 호출
 
<br/><br/>
 

## 3-1. 일반 함수 호출 방식

> 일반 함수로 호출된 함수들 내부의 this에는 `전역 객체`가 바인딩됨.

- 중첩 함수 또한 일반 함수로 호출하면 함수 내부의 `this`에는 전역 객체가 바인딩됨.
- 또한 메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면 함수 내부의 `this`에 전역 객체가 바인딩됨.

문제점

`중첩 함수` or `콜백 함수`는 외부 함수를 돕는 함수의 역할을 수행하므로 this가 일치하지 않는다면 동작의 어려움이 생기게 되며,

이를 위해 
> 1)`this 바인딩`을 `변수에 할당`하거나
	2) 명시적 메서드를 통해 this에 바인딩하거나
	3) 내부 함수를 `화살표 함수`로 변경하여 this를 일치시킬 수 있다.

<br/><br/>


## 3-2. 메서드 호출 방식

메서드 내부의 `this`에는 메서드를 호출한 객체, 
즉 메서드를 호출할 때 메서드 이름 앞의 점 표기법으로 기술한 객체가 바인딩되며,

prototype 메서드 내부에서 사용된 this에도 해당 메서드를 호출한 객체가 바인딩됨.
 

<br/><br/>

## 3-3. 생성자 함수 호출 방식

생성자 함수 내부의 `this`에는 생성자 함수가 미래에 생성할 인스턴스가 바인딩된다.


<br/><br/>

## 3-4. `apply`, `call`, `bind` 메서드에 의한 간접호출

> ### `apply`,`call` 메서드 

this로 사용할 객체와 인수 리스트를 모두 인수로 전달받아 함수를 호출하며, 함수를 호출하여 첫 번째 인수로 전달한 특정 객체를 호출한 함수의 `this`에 바인딩한다.

<br/>

> ### bind 메서드

 함수를 호출하지 않지만, 첫 인수로 전달한 값으로 
 `this` 바인딩이 교체된 함수를 새롭게 생성해 반환함.

<br/><br/>




