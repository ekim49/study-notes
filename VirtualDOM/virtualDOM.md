# DOM 과 Virtual DOM

[React 공식문서 Virtual DOM 정의](https://reactjs.org/docs/faq-internals.html)

가상의 DOM 객체로, 실제 DOM(Real DOM)의 복사본 같은 개념이다.

React가 Real DOM 객체에 접근해서 조작하는 대신에 Virtual DOM 객체에 접근해서 조작한다.

<br/>

## DOM

[MDN에서 DOM의 정의](https://developer.mozilla.org/ko/docs/Web/API/Document_Object_Model/Introduction)

> 문서 객체 모델(The Document Object Model, 이하 DOM) 은 HTML, XML 문서의 프로그래밍 interface 이다. DOM은 문서의 구조화된 표현(structured representation)을 제공하며 프로그래밍 언어가 DOM 구조에 접근할 수 있는 방법을 제공하여 그들이 문서 구조, 스타일, 내용 등을 변경할 수 있게 돕는다.

<br/>

- Document Object Model, 문서 객체 모델
  - 여기서 Document 는 HTML 문서를 말한다.
- JavaScript 와 같은 스크립트 언어가 `<html>`, `<head>` 와 같은 태그에 접근하고, 속성, 디자인, 배치 등을 조작할 수 있도록 문서를 객체화한 상태
  - 하지만 JavaScript 라는 특정 언어에 종속되지는 않는다. 다만 JavaScript를 사용해서 제어할 수 있다. 라이브러리만 갖춰지면 어떤 언어든지 DOM을 조작할 수 있다. (e.g. Python + BeautifulSoup)
  - 이것이 가능한 이유는 DOM은 API를 가지고 있기 때문이다.
- 브라우저가 **트리 구조**로 만든 객체 모델
- DOM의 구성 요소는 node 이다.
  - HTML element 들은 모두 node 이다. (모두 node로부터 상속받는다.) 따라서 모두 node의 속성, 기능을 가지고 있다.
  - node 의 속성: `textContent`, `childNodes`, `firstChild`, `lastChild`, `parentNode`, `appendChild`, `removeChild` 등..
  - 이 node는 EventTarget으로 부터 상속 받는다.
    - node는 EventTarget 이다.

<br/>

### 문제점

DOM을 조작하면 변경 사항이 생길 때 마다 업데이트가 된다.

여기서 발생하는 문제점은, 변경 사항이 생기면 변경된 사항 외에도 모든 DOM 요소가 리렌더링이 된다. 이것은 애플리케이션의 성능과 속도의 측면에 있어서 비효율적이다. 이런 비효율적인 리렌더링이 지속된다면 최악의 상황에서는 frame drop 과 같은 문제가 발생한다.

React는 이런 문제의 해결방안으로 변경된 부분만 렌더링을 하기 위해 Virtual DOM을 만들었다.

<br/>

## Virtual DOM

[React 공식문서의 Virtual DOM 정의](https://ko.reactjs.org/docs/faq-internals.html#gatsby-focus-wrapper)

> Virtual DOM (VDOM)은 UI의 이상적인 또는 “가상”적인 표현을 메모리에 저장하고 ReactDOM과 같은 라이브러리에 의해 “실제” DOM과 동기화하는 프로그래밍 개념입니다.

- Virtual DOM은 DOM의 구조만 흉내내는 JavaScript의 객체로, 최소한의 변경으로 DOM을 조작하는 방법을 찾을 때 쓰인다.
- Real DOM(DOM)과 비교했을 때 동일한 속성을 가지고 있음에도 가볍다.
- Virtual DOM 을 조작하는 것은 실제 DOM을 조작하는 것보다 훨씬 빠르다.
- 가상의 UI 요소를 **ReactDOM** 과 같은 라이브러리를 통해 실제 DOM과 동기화 시킨다.
- Virtual DOM은 **Runtime** 아래에서 사용된다.

JavaScript에 의해 DOM이 조작되는 변경 사항이 일어날 때, 실제 DOM을 조작하면 무겁고 "비싼 연산"을 해야 하므로 속도도 느려지게 된다.

Virtual DOM에서는 실제 DOM을 간략한 JavaScript Object로 바꾼 다음에 변경 사항을 토대로 시뮬레이션을 해보고 나서 가장 효율적인 방식을 찾는다.

합리적인 알고리즘을 통해 차이가 생기는 부분만 파악을 한다(Diffing Algorithm). 그리고 나서 차이가 생기는 부분만 실제 DOM에 반영을 한다. 즉, 전체가 아닌 변경 사항이 필요한 부분만 조작을 하는 것이다.

<br/>
<br/>

### References

- https://developer.mozilla.org/ko/docs/Web/API/Document_Object_Model/Introduction
- https://reactjs.org/docs/faq-internals.html
