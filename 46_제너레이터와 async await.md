# 1. 제너레이터란?
> ES6d에서 도입되었으며, 코드 블록의 실행을 일시 중기했다가 필요한 시점에 재개할 수 있는 특수 함수.

제너레이터와 일반함수에는 몇가지 차이점이 존재한다.

>## ① 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도 할 수 있음.

일반 함수의 경우 함수 호출 시 제어권이 함수에게 넘어기 때문에 함수 호출자는 함수 호출 이후 함수 실행을 제어할 수 없음.
  
>## ② 제너레이터 함수는 함수 호출자와 함수의 상태를 주고받을수 있다.

일반 함수는 호출하면 배개변수를 통해 함수 외부에서 값을 주입받고, 함수 코드를 일괄 실행하여 결과값을 함수 외부로 반환하기에 함수 실행 동안에는 외부에서 함수 내부로 값을 전달하여 함수 상태를 변경할 수 없다.

>## ③ 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.

일반 함수를 호출하면 함수 코드를 일괄 실행하고 값을 반환하지만, 제너레이터 함수는 함수 코드를 실행하는 것이 아닌 이터러블이면서 동시에 이터레이터인 제너레이터 객체를 반환한다.

<br/>

# 2. 제너레이터 함수의 정의
> 제너레이터 함수는 `function*` 키워드로 선언하며, 하나 이상의 yield 표현식을 포함한다. 


```js
// 제너레이터 함수 선언문
function* genDecFunc() {
	yield 1;
}

// 제너레이터 함수 표현식
const genExpfunc = function* () {
	yield 1;
}

// 제너레이터 메서드
const obj = {
	* genObjMethod() {
    	yield 1;
    }
}

// 제너레이터 클래스 메서드
class Myclass {
	* genClsMethod() {
    	yield 1;
    }
}
```

애스터리스크`*`의 위치는 function 키워드와 함수 사이 어떤 위치던 상관없으며,

`화살표함수`로 <u>정의할 수 없고</u>, 

`new 연산자`와 함께 <u>생성자 함수로 호출할 수 없다</u>.

```js
const genArrowFunc = * () => {
	yield 1;
}; // 에러 발생. 
// SyntaxError: Unexpected token '*' 
   


function* genFunc() {
	yield 1;
}
new genFunc(); // 에러 발생. 
// TypeError: genFunc is not a constructor

```

<br/>

# 3. 제너레이터 객체
> 제너레이터 함수를 호출하면 일반 함수처럼 함수 코드 블록을 실행하는 것이 아니라, 제너레이터 객체를 생성해 반환하게 된다.
제너레이터 함수가 반환한 제너레이터 객체는 `이터러블`이면서 동시에 `이터레이터`이다.

```js
// 제너레이터 함수
function* genFunc() {
	yield 1;
  	yield 2;
  	yield 3;
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환함.
const generator = getFunc();
// 제너레이터 객체는 이터러블이면서 동시에 이터레이터이다.
// 이터러블은 Symbol.itrator 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체이다.
console.log(Symbol.iterator in generator) // true
// 이터레이러는 nexts 메서드를 갖는다.
console.log('next' in generator); // true

```

제너레이터 객체는 next 메서드를 갖는 이터레이터이지만, 이터레이터에는 없는 `return`, `throw` 메서드를 갖는다. 제너레이터 객체의 세 개의 메서드를 호출하면 다음과같이 동작한다.

- `next 메서드를 호출`하면 제너레이터 함수의 yield 표현식까지 코드 블록을 실행하고, yield된 값을 value 프로퍼티 값으로, false를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다. 

- `return 메서드를 호출`하면 인수로 전달받은 값을 value프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.

- `throw 메서드를 호출`하면 인수로 전달받은 에러를 발생시키고 undefined를 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.


<br/>

# 4. 제너레이터의 일시 중지와 재개
> 제너레이터는 yield 키워드와 next 메서드를 통해 실행을 일시 중지했다가 필요한 시점에 다시 재개할 수 있다.

yield 키워드는 제너레이터 함수의 실행을 일시 중지시키거나 yield 키워드 뒤에 오는 표현식의 평가 결과를 제너레이터 함수 호출자에게 반환한다.
```js
function* genFunc() {
	yield 1;
  	yield 2;
  	yield 3;
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환.
// 이터러블이면서 동시에 이터레이터인 제너레이터 객체는 next 메서드를 갖는다.
const generator = genFunc();

// 처음 next 메서드를 호출하면 첫 번째 yield 표현식까지 실행되고 일시 중지된다.
// next 메서드는 이터레이터 리절트 객체({value, done})를 반환한다.
// value 프로퍼티에는 첫 번째 yield 표현식에서 yield된 값 1이 할당된다.
// done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 false가 할당된다.
console.log(generator.next()); // {value: 1, done: false}

// 다시 next 메서드를 호출하면 두 번째 yield 표현식까지 실행되고 일시 중지된다.
// next 메서드는 이터레이터 리절트 객체({value, done})를 반환한다.
// value 프로퍼티에는 두 번째 yield 표현식에서 yield된 값 2가 할당된다.
// done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 false가 할당된다.
console.log(generator.next()); // {value: 2, done: false}

// 다시 next 메서드를 호출하면 세 번째 yield 표현식까지 실행되고 일시 중지된다.
// next 메서드는 이터레이터 리절트 객체({value, done})를 반환한다.
// value 프로퍼티에는 세 번째 yield 표현식에서 yield된 값 3이 할당된다.
// done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 false가 할당된다.
console.log(generator.next()); // {value: 3, done: false}

// 다시 next 메서드를 호출하면 남은 yield 표현식이 없으므로 제너레이터 함수의 마지막까지 실행한다.
// next 메서드는 이터레이터 리절트 객체({value, done})를 반환한다.
// value 프로퍼티에는 제너레이터 함수의 반환값 undefined가 할당된다.
// done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었음을 나타내는 true가 할당된다.
console.log(generator.next()); // {value: undefined, done: true}
```

제너레이터 객체의 next 메서드를 호출하면 yield 표현식까지 실행되고 일시중지되며, 이때 함수의 제어권이 호출자로 양도되게 된다.

제너레이터 객체의 next 메서드는 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환한다. next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에는 yield 표현식에서 yield된 값이 할당되고, done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 불리언 값이 할당된다.

<br/>

# 5. async/await
> ES8에서는 제너레이터보다 간단하고 가독성이 좋게 비동기 처리를 동기 처리처럼 동작하도록구현할 수 있는 `asyn/await` 가 도입되었다.

asyn/await는 프로미스를 기반으로 동작하며, 프로미스의 `thn/catch/finally`후속 처리 메서드 없이 동기 처리처럼 프로미스가 처리 결과를 반환하도록 구현할 수 있다.

### 1) async 함수 

await 키워드는 반드시 async 함수 내부에서 사용해야 하며, async 함수는 async 키워드를 사용해 정의하며 언제나 프로미스를 반환한다. async 함수가 명시적으로 프로미스를 반환하지 않더라도 async 함수는 암묵적으로 반환값을 resolve하는 프로미스를 반환한다.
```js
// async 함수 선언문
async function foo(n) { return n; }
foo(1).then (v =. console.log(v)); // 1

// async 함수 표현문
const bar = async function foo(n) { return n; }
bar(2).then (v =. console.log(v)); // 2

const baz = async n => n;
baz(3).then(v => console.log(v)); // 3

// async 메서드
const obj = {
	async foo(n) { return n; }
};
obj.foo(4).then(v= > console.log(v)); // 4

// async 클래스 메서드
class MyClass {
	async bar(n) { return n; }
}
const myClass = new MyClass();
myClass.bar(5).then(v => console.log(v)); // 5
```
클래스의 consturctor 메서드는 async 메서드가 될 수 없다. 클래스의 constructor 메서드는 인스턴스를 반환해야하지만 async 언제나 프로미스를 반환해야 한다.

<br/>

### 2) await 키워드

await 키워드는 프로미스가 settled 상태가 될 때까지 대기하다가 settled 상태가 되면 프로미스가 resolve한 결과를 반환한다. await 키워드는 반드시 프로미스 앞에서 사용해야 한다.

```js
const fetch = require('node-fetch');

const getGithubUserName = async id => {
	const res = await fetch(`https:api.github.com/users/${id}`);
  	const { name } = await res.json();
  	console.log(name);
};

getGithubUserName('ungmo2');

```
프로미스가 settled 상태가 되면 프로미스가 resolve한 처리 결과가 res 변수에 할당된다.

이처럼 await 키워드는 다음 실행을 일시 중지시켰다가 프로미스가 settled 상태가 되면 다시 재개한다.

<br/>

### 3) 에러처리

비동기 처리 콜백 패턴의 단점중 제일 큰 문제는 에러 처리가 곤란하다는 것이며,비동기 함수의 콜백 함수를 호출한 것은 비동기 함수가 아니기 때문에 try...catch문을 사용해 에러를 캐치할 수 없다.

하지만, `async/await`에서는 에러 처리를 `try...catch문`을 통해 처리할 수 있는데, 프로미스를 반환하는 비동기 함수는 명시적으로 호출할 수 있기 때문에 호출자가 명확하다는 특징이 있기 때문이다.

async 함수 내에서 catch 문을 사용해서 에러를 처리하지 않으면 async 함수는 발생한 에러를 reject하는 프로미스를 반환한다.

