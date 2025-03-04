## 39장 DOM
> **DOM(Document Object Model)은 HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조임**
<br/>

## 1️⃣ 39.1 노드
### 39.1.1 HTML 요소와 노드 객체
![image](https://github.com/user-attachments/assets/ad07a45a-656c-40b6-b688-f7ee83bf541c)
- HTML 요소는 HTML 문서를 구성하는 개별적인 요소
- HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 변환됨
- HTML 요소의 어트리뷰트는 어트리뷰트 노드로, 요소의 텍스트 콘텐츠는 텍스트 콘텐츠로 변환됨
- HTML 문서는 HTML 요소들의 집합
- HTML 요소는 중첩 관계를 갖음 => 계층적인 부자 관계 형성
- HTML 요소 간의 부자 관계를 반영하여 HTML 문서의 구성 요소인 HTML 요소를 객체화한 모든 노드 객체들을 트리 자료 구조로 구성함

 <br/>
 
#### 📍 트리 자료구조
- 부모 노드와 자식 노드로 구성되어 노드 간의 계층적 구조를 표현하는 비선형 자료구조
- 하나의 최상위 노드에서 시작함
- 최상위 노드는 부모 노드가 없으며 루트 노드라고 함
- 루트 노드는 0개 이상의 자식 노드를 갖음
- 자식 노드가 없는 노드를 리프 노드라 함

=> **노드 객체들로 구성된 트리 자료구조를 DOM이라고 함**
<br/>

### 39.1.2 노드 객체의 타입
- **문서 노드**
  - DOM 트리의 최상위에 존재하는 루트 노드
  - document 객체를 가리킴
  - document 객체는 브라우저가 렌더링한 HTML 문서 전체를 가리키는 객체로서 전역 객체 window의 document 프로퍼티에 바인딩되어 있음
  - 문서 노드는 window.document 혹은 document 로 참조 가능
  - HTML 문서당 document 객체는 유일함
  - 문서 노드는 DOM 트리의 노드들에 접근하기 위한 진입점 역할
- **요소 노드**
  - HTML 요소를 가리키는 객체
  - HTML 요소 간의 중첩에 의해 부자 관계를 가지며 부자 관계를 통해 정보를 구조화함
  - 문서의 구조를 표현
- **어트리뷰트 노드**
  - HTML 요소의 어트리뷰트를 가리키는 객체
  - 어트리뷰트가 지정된 HTML 요소의 요소 노드와 연결되어 있음
  - 어트리뷰트 노드는 요소 노드와 연결되어 있지만 부모 노드는 없음. 즉, 요소 노드의 형제 노드는 아니지만 요소 노드를 통해 접근 가능함
- **텍스트 노드**
  - HTML 요소의 텍스트를 가리키는 객체
  - 문서의 정보를 표현
  - 요소 노드의 자식 노드이며 리프 노드임
  - DOM 트리의 최종단
- + Comment 노드, DocumentType 노드, DocumentFragment 노드 등 총 12개의 노드 타입이 있음
 

### 39.1.3 노드 객체의 상속 구조
- DOM은 HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조
- DOM API를 통해 노드 객체는 자신의 부모, 형제, 자식을 탐색하고 자신의 어트리뷰트와 텍스트 조작 가능함
- DOM을 구성하는 노드 객체는 표준 빌트인 객체가 아닌 **브라우저 환경에서 추가적으로 제공하는 호스트 객체**임
- 하지만 노드 객체도 자바스크립트 객체이므로 **프로토타입에 의한 상속 구조**를 갖음
- 모든 노드 객체는 Object, EventTarget, Node 인터페이스를 상속 받음
- 문서 노드는 Document, HTMLDocument 인터페이스를 상속 받음
- 어트리뷰트 노드는 Attr 상속 받음
- 텍스트 노드는 CharacterData 인터페이스를 각각 상속받음
- 요소 노드는 Element, HTMLHtmlElement, HTMLHeadElement, HTMLBodyElement, HTMLUListElement  인터페이스를 상속받음

## 2️⃣ 39.2 요소 노드 취득
- HTML의 구조나 내용 또는 스타일 등을 동적으로 조작하려면 먼저 요소 노드를 취득해야 함
- 이를 위해 DOM은 요소 노드를 취득할 수 있는 다양한 메서드를 제공함

### 39.2.1 id를 이용한 요소 노드 취득
- **Document.prototype.getElementById** 메서드는 인수로 전달한 id 어트리뷰트 값을 갖는 하나의 요소 노드를 탐색하여 반환함, 없다면 null을 반환
- id 값은 HTML 문서 내에서 유일한 값이어야 함, 중복되더라도 id 값을 갖는 첫 번째 요소 노드만 반환함


### 39.2.2 태그 이름를 이용한 요소 노드 취득
- **Document.prototype/Element.prototype.getElementsByTagName** 메서드는 인수로 전달한 태그 이름을 갖는 모든 요소 노드를 탐색하여 반환함. 즉, 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 HTMLCollection 객체를 반환함, 없다면 빈 HTMLCollection 객체를 반환함
- HTMLCollection 객체는 유사 배열 객체이면서 이터러블임


### 39.2.3 class를 이용한 요소 노드 취득
- **Document.prototype/Element.prototype.getElementsByClassName** 메서드는 인수로 전달한 class 어트리뷰트 값을 갖는 모든 요소 노드들을 탐색하여 반환함. 
- 인수로 전달할 class 값은 공백으로 여러 개의 class를 지정할 수 있음
- HTMLCollection 객체를 반환함 , 없다면 빈 HTMLCollection 객체를 반환함

### 39.2.4 CSS 선택자를 이용한 요소 노드 취득
- 스타일을 적용하고자 하는 HTML 요소를 특정할 때 사용하는 문법
- **Document.prototype/Element.prototype.querySelector** 메서드는 인수로 전달한 CSS 선택자를 만족시키는 하나의 요소 노드를 탐색하여 반환함
  - 만족시키는 요소가 여러 개인 경우 첫 번째 요소만 반환, 없다면 null 반환
  - 인수로 전달한 CSS 선택자가 문법에 맞지 않는 경우 DOMException 에러가 발생함
- **Document.prototype/Element.prototype.querySelectorAll** 메서드는 인수로 전달한 CSS 선택자를 만족시키는 모든 요소 노드를 탐색하여 반환함
  - 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 NodeList 객체를 반환, 없다면 빈 NodeList 객체를 반환
  - 인수로 전달한 CSS 선택자가 문법에 맞지 않는 경우 DOMException 에러가 발생함
  - NodeList 객체는 유사 배열 객체이면서 이터러블
  - getElementBy*** 메서드보다 다소 느리지만 CSS 선택자 문법을 사용해 좀 더 구체적인 조건으로 요소 노드를 취득할 수 있고 일관된 방식으로 요소 노드를 취득할 수 있다는 장점이 있음

### 39.2.5 특정 요소 노드를 취득할 수 있는지 확인
- **Element.prototype.matches** 메서드는 인수로 전달한 CSS 선택자를 통해 특정 요소 노드를 취득할 수 있는지 확인함
- 이벤트 위임을 사용할 때 유용함
```
<ul id="fruits">
	<li class="apple">apple</li>
    <li class="banana">banana</li>
    <li class="orange">orange</li>
</ul>

...

<script>
	const $apple = document.querySelector('.apple');
    // $apple 노드는 '#fruits > li.apple'로 취득할 수 있다
    console.log($apple.matches('#fruits > li.apple')); // true;
    // $apple 노드는 '#fruits > li.apple'로 취득할 수 없다
    console.log($apple.matches('#fruits > li.banana')); // false;
</script>
```

### 39.2.6 HTMLCollection과 NodeList
- DOM 컬렉션 객체인 HTMLCollection 과 NodeList는 DOM API가 여러 개의 결과값을 반환하기 위한 객체
- 유사 배열 객체이면서 이터러블
- 노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는 객체임
- HTMLCollection은 언제나 live 객체로 동작
- NodeList는 대부분의 경우 노드 객체의 상태 변화를 실시간으로 반영하지 않고 과거의 정적 상태를 유지하는 non-live 객체로 동작하지만 경우에 따라 live 객체로 동작할 때가 있음
- 스프레드 문법이나 Array.from 메서드를 사용해 간단히 배열로 변환 가능
 <br/>
 
#### 📍 HTMLCollection
- getElementsByTagName, getElementsByClassName 메서드가 반환하는 객체
- 노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는 객체
- 실시간으로 객체가 변화하기 때문에 for 문을 순회하면서 노드 객체의 상태를 변경할 때 부작용이 발생할 수 있음
  - for 문을 역뱡향으로 순회하여 회피
  - while 무한 반복문을 사용해 회피
  - HTMLCollection 객체를 사용하지 않기
 <br/>
 
#### 📍 NodeList
- querySelectorAll 메서드가 반환하는 객체
- 노드 객체의 상태 변경을 실시간으로 반영하지 않고 과거의 정적 상태를 유지하는 non-live 객체로 동작 => HTMLCollection 객체의 부작용을 해결할 수 있음
- 하지만 childNode 프로퍼티가 반환하는 NodeList 객체는 HTMLCollection 객체와 같이 실시간으로 노드 객체의 변경을 반영하는 live 객체로 동작하므로 주의가 필요함

## 3️⃣ 39.3 노드 탐색
- 요소 노드를 취득한 다음, 취득한 요소 노드를 기점으로 DOM 트리의 노드를 옮겨 다니며 부모, 형제, 자식 노드 등을 탐색해야 할 때가 있음
- Node, Element 인터페이스는 트리 탐색 프로퍼티를 제공함
- 노드 프로퍼티는 모두 접근자 프로퍼티임
- 단, setter 없이 getter 만 존재하여 참조 가능한 읽기 전용 접근자 프로퍼티임
-** Node**
  - parentNode
  - previousSibiling
  - firstChild
  - childNodes
- **Element**
  - previousElementSibiling
  - nextElementSibiling
  - children
 
### 39.3.1 공백 텍스트 노드
- HTML 요소 사이의 스페이스, 탭, 줄바꿈(개행) 등의 공백 문자는 텍스트 노드를 생성하고 이를 공백 텍스트 노드라고 함

### 39.3.2 자식 노드 탐색
| 프로퍼티 | 설명 |
| ------- | ---- |
| Node.prototype.childNodes | 자식 노드를 모두 탐색하여 DOM 컬렉션 객체인 NodeList에 담아 반환, 요소 노드뿐만 아니라 텍스트 노드도 포함될 수 있음 |
| Element.prototype.children | 자식 노드 중에서 요소 노드만 모두 탐색하여 DOM 컬렉션 객체인 HTMLCollection에 담아 반환 |
| Node.prototype.firstChild | 첫 번째 자식 노드를 반환. 텍스트 노드이거나 요소 노드임 |
| Node.prototype.lastChild | 마지막 자식 노드를 반환. 텍스트 노드이거나 요소 노드임 |
| Element.prototype.firstElementChild | 첫 번째 자식 노드를 반환. 요소 노드만 반환 |
| Element.prototype.lastElementChild | 마지막 자식 노드를 반환. 요소 노드만 반환 |


### 39.3.3 자식 노드 존재 확인
- Node.prototype.hasChildNodes 메서드를 사용해 자식 노드가 존재하는지 확인함
- 요소 노드와 텍스트 노드 모두 포함하여 자식 노드의 존재를 확인함
- 요소 노드만 확인하려면 children.length 나 Element 의 childElementCount 를 사용하면 됨


### 39.3.4 요소 노드의 텍스트 노드 탐색
- 요소 노드의 텍스트 노드는 요소 노드의 자식 노드
- firstChild 프로퍼티로 텍스트 노드에 접근 가능

### 39.3.5 부모 노드 탐색
- Node.prototype.parentNode 프로퍼티를 사용해 부모 노드 탐색 가능
- 텍스트 노드는 리프 노드이므로 부모 노드가 텍스트 노드인 경우는 없음

### 39.3.6 형제 노드 탐색
| 프로퍼티 | 설명 |
| ------- | ---- |
| Node.prototype.previousSibiling | 부모 노드가 같은 형제 노드 중에서 자신의 이전 형제 노드를 탐색하여 반환. 요소 노드나 텍스트 노드임 |
| Node.prototype.nextSibiling | 부모 노드가 같은 형제 노드 중에서 자신의 다음 형제 노드를 탐색하여 반환. 요소 노드나 텍스트 노드임 |
| Element.prototype.previousElementSibiling | 부모 노드가 같은 형제 노드 중에서 자신의 이전 형제 노드를 탐색하여 반환. 요소 노드만 반환 |
| Element.prototype.nextElementSibiling | 부모 노드가 같은 형제 노드 중에서 자신의 다음 형제 노드를 탐색하여 반환. 요소 노드만 반환 |


## 4️⃣ 39.4 노드 정보 취득
| 프로퍼티 | 설명 |
| ------- | ---- |
| Node.prototype.nodeType|노드 객체의 종류, 즉 노드 타입을 나타내는 상수를 반환함<br/> Node.ELEMENT_NODE : 요소 노드 타입을 나타내는 상수 1을 반환<br/> Node.TEXT_NODE : 텍스트 노드 타입을 나타내는 상수 3을 반환<br/> Node.DOCUMENT_NODE : 문서 노드 타입을 나타내는 상수 9를 반환 |
| Node.prototype.nodeName | 노드의 이름을 문자열로 반환함 <br/> 요소 노드 : 대문자 문자열로 태그 이름을 반환함 <br/> 텍스트 노드 : 문자열 "#text" 를 반환 <br/> 문서 노드 : 문자열 "#document"를 반환|


## 5️⃣ 39.5 요소 노드의 텍스트 조작
### 39.5.1 nodeValue
- setter 와 getter 모두 존재하는 접근자 프로퍼티
- nodeValue를 참조하면 노드 객체의 값을 반환함
- 노드 객체의 값이란 텍스트 노드의 텍스트
- 텍스트 노드가 아닌 노드를 참조하게 되면 null을 반환함

### 39.5.2 textContent
- setter 와 getter 모두 존재하는 접근자 프로퍼티
- 요소 노드의 텍스트와 모든 자손 노드의 텍스트를 모두 취득하거나 변경함
- 요소 노드의 콘텐츠 영역(시작 태그와 종료 태그 사이) 내의 텍스트를 모두 반환함
- 이 때 HTML 마크업은 무시하고 텍스트만 반환함

## 6️⃣ 39.6 DOM 조작
- 새로운 노드를 생성하여 DOM에 추가하거나 기존 노드를 삭제 또는 교체하는 것
- DOM 조작하면 리플로우와 리페인트가 발생하는 원인이 되므로 성능에 영향을 주니 주의해야 함

### 39.6.1 innerHTML
- setter 와 getter 모두 존재하는 접근자 프로퍼티
- 요소 노드의 HTML 마크업을 취득하거나 변경함
- 요소 노드의 콘텐츠 영역 내에 포함된 모든 HTML 마크업을 문자열로 반환함
- HTML 마크업 문자열로 간단히 DOM 조작이 가능함, 직관적인 장점
- 사용자로부터 입력받은 데이터를 그대로 innerHTML 프로퍼티에 할당하면 크로스 사이트 스크립팅 공격에 취약하므로 위험함
- 요소 노드의 innerHTML 프로퍼티에 HTML 마크업 문자열을 할당하는 경우 요소 노드의 모든 자식 노드를 제거하고 할당한 HTML 마크업 문자열을 파싱하여 DOM을 변경한다는 것으로 단점이 있음 => 비효율적임
- 새로운 요소를 삽입할 때 삽입될 위치를 지정할 수 없음

#### 📍 HTML 새니티제이션
- 사용자로부터 입력받은 데이터에 의해 발생할 수 있는 크로스 사이트 스크립팅 공격을 예방하기 위해 잠제적 위험을 제거하는 기능. DOMPurify 라이브러리를 사용하는 것을 권장함


### 39.6.2 insertAdjacentHTML
- 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입
- 두번째 인수로 전달한 HTML 마크업 문자열을 파싱하고 그 결과로 생성된 노드를 첫 번째 인수로 전달한 위치에 삽입하여 DOM에 반영함
- 첫 번째 매개변수 값은 beforebegin, afterbegin, beforeend, afterend 4가지가 있음
- XSS 공격에 취약하다는 단점 존재

### 39.6.3 노드 생성과 추가
- DOM을 노드를 직접 생성, 삽입, 삭제, 치환하는 메서드 제공함
- **요소 노드 생성**
  - Document.prototype.createElement(tagName)
  - 요소 노드를 생성하여 반환
  - 기존 DOM에 추가되지 않고 홀로 존재하는 상태
- **텍스트 노드 생성**
  - Document.prototype.createTextNode(text)
  - 텍스트 노드를 생성하여 반환
  - 텍스트 노드는 요소 노드의 자식 노드지만 현재 홀로 존재하는 상태
- **텍스트 노드를 요소 노드의 자식 노드로 추가**
  - Node.prototype.appendChild(childNode)
  - 매개변수 childNode에게 인수로 전달한 노드를 appendChild 메서드로 호출한 노드의 마지막 자식 노드로 추가함
  - 요소 노드 텍스트 노드 따로 만들지 말고 textContent 프로퍼티를 사용함이 더욱 간편함
- **요소 노드를 DOM에 추가**
  - Node.prototype.appendChild
  - 텍스트 노드와 부자 관계로 연결한 요소 노드를 호출한 노드의 마지막 자식 요소로 추가함
  - 새롭게 생성한 요소 노드가 DOM에 추가됨
 
### 39.6.4 복수의 노드 생성과 추가
``
<body>
	<ul id="fruits"></ul>
</body>
<script>
	const $fruits = document.getElementById('fruits);
    
    const $fragment = document.createDocumentFragment();
    
    ['Apple', 'Banana', 'Orange'].forEach(text => {
    	const $li = document.createElement('li');
        
        const textNode = document.createTextNode(text);
        
        $li.appendChild(textNode);
        
        $fragment.appendChild($li);
    });
    
    $fruits.appendChild($fragment);
</script>
``
- DocumentFragment 노들르 생성하고 DOM에 추가할 요소 노드를 생성하여 DocumentFragment  노드에 자식 노드를 추가한 다음, DocumentFragment  노드를 기존 DOM에 추가함
- 실제로 DOM 변경이 발생하는 것은 한 번이며 리플로우와 리페인트도 한 번만 실행되 렌더링 최적화를 이룰 수 있음

### 39.6.5 노드 삽입

- **마지막 노드로 추가**
  - Node.prototype.appendChild
  - 인수로 전달받은 노드를 자신을 호출한 노드의 마지막 자식 노드로 DOM에 추가함
  - 위치를 지정할 수 없고 언제나 마지막 자식 노드로 추가됨
- **지정한 위치에 노드 삽입**
  - Node.prototype.insertBefor(newNode, childNode)
  - 첫 버째 인수로 전달받은 노드를 두 번째 인수로 전달받은 노드 앞에 삽입함
 

### 39.6.6 노드 이동
- DOM에 이미 존재하는 노드를 appendChild나 insertBefor 메서드를 사용하여 DOM에 다시 추가하면 현재 위치에서 노드를 제거하고 새로운 위치에 노드를 추가함, 즉 노드가 이동함

### 39.6.7 노드 복사
- Node.prototype.cloneNode([deep : true | false])
- 노드의 사본을 생성하여 반환함
- deep에 true를 전달하면 깊은 복사하여 모든 자손 노드를 포함된 사본을 생성하고, false를 전달하거나 생략하면 얕은 복사로 노드 자신만의 사본을 생성함
- 얕은 복사로 생성된 요소 노드는 자손 노드를 복사하지 않으므로 텍스트 노드도 없음

### 39.6.8 노드 교체
- Node.prototype.replaceChild(newChild, oldChild)
- 자신을 호출한 노드의 자식 노드를 다른 노드로 교체함
- 두번 째 매개변수는 replaceChild 메서드를 호출한 노드의 자식 노드이어야 함

### 39.6.8 노드 삭제
- Node.prototype.removeChild(child)
- child 매개변수를 DOM에서 삭제함


## 7️⃣ 39.7 어트리뷰트
### 39.7.1 어트리뷰트 노드와 attributes 프로퍼티
- HTML 요소는 여러 개의 어트리뷰트(속성)을 가질 수 있음
- HTML 어트리뷰트는 어트리뷰트 이름 = "어트리뷰트 값" 형식으로 지정함
- 어트리뷰트 하나당 어트리뷰트 노드가 생성됨
- 모든 어트리뷰트 노드의 참조는 유사 배열 객체이자 이터러블인 NamedNodeMap 객체에 담겨서 요소 노드의 attributes 프로퍼티에 저장됨
- attributes 프로퍼티는 getter 만 가진 읽기 전용 접근자 프로퍼티

### 39.7.2 HTML 어트리뷰트 조작
- Element.prototype.getAttributes/setAttributes 메서드를 사용하면 attributes 프로퍼티를 통하지 않고 요소 노드에서 직접 HTML 어트리뷰트 값을 취득하거나 변경할 수 있음
- Element.prototype.hasAttribute(attributeName) : 특정 어트리뷰트 존재 확인
- Element.prototype.removeAttribute(attributeName) : 특정 어트리뷰트 삭제
 

### 39.7.3 HTML 어트리뷰트 vs DOM 프로퍼티
- 요소 노드 객체에는 HTML 어트리뷰트에 대응하는 DOM 프로퍼티가 존재함
- 요소 노드는 상태를 가지고 있음 => 초기 상태와 최신 상태를 관리해야 함
- **DOM 프로퍼티**
  - setter와 getter가 모두 존재하는 접근자 프로퍼티
  - 요소 노드의 최신 상태 관리
  - 사용자의 입력에 의한 상태 변화에 반응하여 언제나 최신 상태를 유지함
- **HTML 어트리뷰트**
  - HTML 요소의 초기 상태를 지정하는 것
  - 변하지 않음
  - 요소 노드의 초기 상태 관리
   

#### 📍 HTML 어트리뷰트와 DOM 프로퍼티의 대응 관계
- id 어트리뷰트와 id 프로퍼티는 1:1 대응하며, 동일한 값으로 연동함
- input 요소의 value 어트리뷰트는 value 프로퍼티아 1:1 대응함. 하지만, value 어트리뷰트는 초기 상태를, value 프로퍼티는 최신 상태를 갖음
- class 어트리뷰트는 className, classList 프로퍼티와 대응함
- for 어트리뷰트는 htmlFor 프로퍼티와 1:1 대응함
- td 요소의 colspan 어트리뷰트는 대응하는 프로퍼티가 존재하지 않음
- textConent 프로퍼티는 대응하는 어트리뷰트가 존재하지 않음
- 어트리뷰트 이름은 대소문자를 구별하지 않지만 대응하는 프로퍼티 키는 카멜 케이스를 따름
 

 

### 39.7.4 data 어트리뷰트와 dataset 프로퍼티
- data 어트리뷰트와 dataset 프로퍼티를 사용하면 HTML 요소에 정의한 사용자 정의 어트리뷰트와 자바스크립트 간에 데이터를 교환할 수 있음
- data 어트리뷰트 값은 HTMLElement.dataset 프로퍼티로 취득 가능
- dataset 프로퍼티는 HTML 요소의 모든 data 어트리뷰트의 정보를 제공하는 DOMStringMap 객체를 반환함
 

## 8️⃣ 39.8 스타일
### 39.8.1 인라인 스타일 조작
- HTMLElement.prototype.style
- setter 와 getter 모두 존재하는 접근자 프로퍼티
- 요소 노드의 인라인 스타일을 취득하거나 추가 또는 변경

### 39.8.2 클래스 조작
- Element.prototype.className : 참조 시 class 어트리뷰트 값을 분자열로 반환, 할당 시 class 어트리뷰트 값을  할당한 문자열로 변경
- Element.prototype.classList : class 어트리뷰트의 정보를 담은 DOMTokenList 객체를 반환
- DOMTokenList 객체는 컬렉션 객체로서 유사 배열 객체이면서 이터러블
  - add(...className) : 인수로 전달한 1개 이상의 문자열을 class 어트리뷰트 값으로 추가
  - remove(...className) : 인수로 전달한 1개 이상의 문자열과 일치하는 클래스를 삭제
  - item(index) : 인수로 전달한 index에 해당하는 클래스를 반환
  - contains(className) : 인수로 전달한 문자열과 일치하는 클래스가 포함되어 있는지 확인
  - replace(oldClassName, newClassName) : 첫 번째 인수로 전달한 문자열을 두 번째 인수로 전달한 문자열로 변경
  - toggle(className[,force]) : 인수로 전달한 문자열과 일치하는 클래스가 존재하면 제거하고, 존재하지 않으면 추가함
 

### 39.8.3 요소에 적용되어 있는 CSS 스타일 참조
- style 프로퍼티는 인라인 스타일만 반환하므로 클래스를 적용한 스타일이나 상속을 통해 암묵적으로 적용된 스타일을 참조할 수 없음
- window.getComputedStyle(element[,pseudo]) : 첫 번째 인수로 전달한 요소 노드에 적용되어 있는 평가된 스타일을 CSSStyleDeclaration 객체에 담아 반한함
- 두 번째 인수로 :after,:before 와 같은 의사 요소를 지정할 수 있음
- 평가된 스타일이란 요소 노드에 적용되어 있는 모든 스타일, 즉 링크 스타일, 임베딩 스타일, 인라인 스타일, 자바스크립트에서 적용한 스타일, 상속된 스타일, 기본 스타일 등 최종적으로 적용된 스타일
 

## 9️⃣ 39.9 DOM 표준
- HTML과 DOM 표준은 W3C, WHATWG이라는 두단체가 협력하며 공통된 표준을 만들어옴
- DOM 단계
  - DOM Level 1
  - DOM Level 2
  - DOM Level 3
  - DOM Level 4


