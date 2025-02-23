# 1. Set

> Set 객체는 중복되지 않는 유일한 값들의 집합이며, 배열과 유사하나 아래와 같은 차이점이 있다.

| 구분                                   | 배열 | Set 객체 |
| -------------------------------------- | :--: | :------: |
| 동일한 값을 중복하여 포함 할 수 있는가 |  O   |    X     |
| 요소 순서에 의미가 있는가              |  O   |    X     |
| Index로 요소에 접근이 가능한가         |  O   |    X     |

<hr/>

## 1-1. Set객체의 생성

Set 생성자 함수에 인수를 전달하지 않으면 빈 Set 객체가 생성되며,

** Set 생성자 함수는 이터러블을 인수로 전달받아 Set 객체를 생성한다.
이때, 이터러블의 중복된 값은 Set 객체에 요소로 저장되지 않는다.**

```js
const set = new Set();
console.log(set); // Set(0) {}

const set1 = new Set([1, 2, 3, 3]);
console.log(set1); // Set(3) {1, 2, 3}

const set2 = new Set('good');
console.log(set2); // Set(3) {"g", "o", "d"}
```

<hr/>

## 1-2. 요소 개수 확인

> Set의 요소 갯수를 확인할 때는 `Set.prototype.size` 프로퍼티를 활용한다.

size 프로퍼티는 setter 함수 없이 getter 함수만 존재하는 접근자 프로퍼티이기에, Set 객체의 요소 갯수를 변경할 순 없다.

```js
const set = new Set([1, 2, 3]);

set.size = 10; // 무시됨.
console.log(set.size); // 3
```

<hr/>

## 1-3. 요소 추가

> Set 객체에 요소를 추가할땐 `Set.prototype.add`메서드를 사용함.

```js
const set = new Set();
console.log(set); // Set(0) {}

set.add(1);
console.log(set); // Set(1) {1}

set.add(2).add(3); // 연속 호출도 가능.
console.log(set); // Set(3) {1, 2, 3}
```

<hr/>

## 1-4. 요소 존재여부 확인

> Set 객체에 특정 요소가 존재하는지 확인하려면 `Set.prototype.has` 메서드를 사용한다.
> has 메서드는 불리언 값을 반환한다.

```js
const set = new Set([1, 2, 3]);
console.log(set.has(2)); // true
console.log(set.has(4)); // false
```

<hr/>

## 1-5. 요소 삭제

> Set 객체에 특정 요소를 삭제하려면 `Set.prototype.delete` 메서드를 사용한다.
> delete 메서드는 삭제 성공 여부를 불리언 값으로 반환한다.

`delete 메서드`에는 인덱스가 아니라 삭제하려는 요소값을 인수로 전달해야 하며, `인덱스를 가지지 않는다`.

```js
const set = new Set([1, 2, 3]);

console.log(set.delete(2)); // true
console.log(set); // Set(2) {1, 3}

// 존재하지 않는 요소를 삭제하면 에러없이 그냥 무시됨.
set.delete(0);
console.log(set); // Set(2) {1, 3}
```

<hr/>

## 1-6. 요소 일괄삭제

> 모든 요소를 일괄 삭제하려면 `Set.prototype.clear`메서드를 사용한다.
> clear 메서드는 언제나 `undefined`를 반환한다.

```js
const set = new Set([1, 2, 3]);

set.clear();
console.log(set); // Set(0) {}
```

<hr/>

## 1-7. 요소 순회

> Set 객체의 요소를 순회하려면 `Set.prototype.forEach`메서드를 사용한다.
> `Array.prototype.forEach`메서드와 유사하게 콜백 함수와 forEach 메서드의 콜백 함수 내부에서 this로 사용될 객체를 인수로 전달함.

이때 콜백 함수는 다음과 같이 3가지의 인수를 전달받는다.

- 첫 번째 인수: 현재 순회 중인 요소값
- 두 번째 인수: 현재 순회 중인 요소값
- 세 번째 인수: 현재 순회 중인 Set 객체 자체

<hr/>

## 1-8. 집합 연산

Set 객체는 수학적 집합을 구현하기 위한 자료구조다.

따라서, Set 객체를 통해 `교집합`, `합집합`, `차집합` 등을 구현할 수 있다.

<hr/>
 
 
<br/>

# 2. Map

> Map 객체는 키와 값의 쌍으로 이루어진 컬렉션으로, 일반 객체와 유사하지만 아래와 같은 차이점이 있다.

| 구분                   |           객체            |       Map 객체        |
| ---------------------- | :-----------------------: | :-------------------: |
| 키로 사용할 수 있는 값 |    문자열 또는 심벌 값    | 객체를 포함한 모든 값 |
| 이터러블               |             X             |           O           |
| 요소 갯수 확인         | `Object.keys(obj).length` |      `map.size`       |

<hr/>

## 2-1. Map 객체의 생성

Map 객체는 Map 생성자 함수로 생성하며, 인수를 전달하지 않으면 빈 Map 객체가 생성된다.

Map 생성자 함수는 이터러블을 인수로 전달받아 Map 객체를 생성한다.
이때 인수로 전달되는 이터러블은 `키와 값의 쌍으로 이루어진 요소`로 구성되어야 한다.

```js
const map = new Map();
console.log(map); // Map(0) {}

const map1 = new Map([
  ['k1', 'v1'],
  ['k2', 'v2'],
]);
console.log(map1); // Map(2) {"k1" => "v1", "k2" => "v2"}
```

Map 생성자 함수의 인수로 전달한 이터러블에 중복된 키를 갖는 요소가 존재하면 값이 덮어써지기 때문에
중복된 키를 갖는 요소가 존재할 수 없다.

<hr/>

## 2-2. 요소 갯수 확인

> Map 객체의 요소 갯수 확인에는 `Map.prototype.size` 프로퍼티를 사용한다.

size 프로퍼티는 setter 함수 없이 getter 함수만 존재하는 접근자 프로퍼티이기에,
Map 객체의 요소 갯수를 변경할 순 없다.

```js
const map = new Map([
  ['k1', 'v1'],
  ['k2', 'v2'],
]);

map.size = 10; // 무시된다.
console.log(map.size); // 2
```

<hr/>

## 2-3. 요소 추가

> Map 객체의 요소를 추가할때는 `Map.prototype.set`메서드를 사용한다.

```js
const map = new Map();
console.log(map); // Map(0) {}

map.set('k1', 'v1');
console.log(map); // Map(1) {"k1" => "v1"}

map.set('k2', 'v2').set('k3', 'v3'); // 연속 호출도 가능.
console.log(map); // Map(1) {"k1" => "v1", "k2" => "v2", "k3" => "v3"}
```

<hr/>

## 2-4. 요소 취득

> Map 객체에서 특정 요소를 취득하려면 `Map.prototype.get` 메서드를 사용한다.
> Map 객체에서 인수로 전달한 키를 갖는 요소가 존재하지 않으면, undefined를 반환한다.

```js
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

map.set(lee, 'developer').set(kim, 'designer');

console.log(map.get(lee)); // developer
console.log(map.get('key')); // undefined
```

<hr/>

## 2-5. 요소 존재여부 확인

> Map 객체에서 특정 요소의 존재여부를 확인하려면 `Map.prototype.has` 메서드를 사용한다.
> has 메서드는 존재여부를 불리언값으로 반환한다.

```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([
  [lee, 'developer'],
  [kim, 'designer'],
]);

console.log(map.has(lee)); // true
console.log(map.has('key')); // false
```

<hr/>

## 2-6. 요소 삭제

> Map 객체의 요소를 삭제하려면 `Map.prototype.delete` 메서드를 사용한다.
> delete 메서드는 성공여부를 불리언값으로 반환한다.

```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([
  [lee, 'developer'],
  [kim, 'designer'],
]);

console.log(map.delete(kim)); // true
console.log(map); // Map(1) {{ name: "Lee"} => "developer"}

// 존재하지 않는 요소를 삭제하면 에러없이 그냥 무시됨.
map.delete(park);
console.log(map); // Map(1) {{ name: "Lee"} => "developer"}
```

<hr/>

## 2-7. 요소 일괄 삭제

> Map 객체의 모든 요소를 일괄 삭제하려면 `Map.prototype.clear`메서드를 사용한다.
> clear 메서드는 언제나 `undefined`를 반환한다.

```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([
  [lee, 'developer'],
  [kim, 'designer'],
]);

map.clear();
console.log(map); // Map(0) { }
```

<hr/>

## 2-8. 요소 순회

> Map 객체의 요소를 순회하려면 Map.prototype.forEach메서드를 사용한다.
> Array.prototype.forEach메서드와 유사하게 콜백 함수와 forEach 메서드의 콜백 함수 내부에서 this로 사용될 객체를 인수로 전달함.

이때 콜백 함수는 다음과 같이 3가지의 인수를 전달받는다.

- 첫 번째 인수: 현재 순회 중인 요소값
- 두 번째 인수: 현재 순회 중인 요소값
- 세 번째 인수: 현재 순회 중인 Map 객체 자체

**Map 객체는 이터러블**이기 때문에 `for...of`문으로 수노히할 수 있으며, 스프레드 문법과 배열 디스트럭처링 할당의 대상이 될 수도 있다.

Map 객체는 이터러블이면서 동시에 이터레이터인 객체를 반환하는 메서드를 제공한다.

| Map 메서드            |                                              설명                                              |
| --------------------- | :--------------------------------------------------------------------------------------------: |
| Map.prototype.keys    |     Map 객체에서 요소키를 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환한다.      |
| Map.prototype.values  |     Map 객체에서 요소값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환한다.      |
| Map.prototype.entries | Map 객체에서 요소키와 요소값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환한다. |
