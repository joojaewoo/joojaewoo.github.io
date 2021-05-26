---
layout: post
title: "[Interview] JavaScript/React 면접 예상 질문"
date: 2021-05-26 21:20:00 +09:00
categories: [Interview]
tags: [interview, javascript, react]
comments: true
---

질문에 대한 답은 주관적으로 저의 생각을 적은 것이기 떄문에 정답이 아닐 수 도 있습니다. 만약 틀린경우 알려주시면 감사하겠습니다.
질문은 실제 면접에서 받은 질문과 인터넷 검색 등을 통해 제 나름대로 만들어본 질문입니다. 참고 용도로 보시면 좋을 것 같습니다.

## JavaScript

### this

- this 란?

  this는 일반적으로 메소드를 호출한 객체가 저장된 속성.

  자신이 속한 객체 또는 자신이 생성할 인스턴트를 가르키는 자기 참조 변수

- this는 어떻게 결정될까?

  this는 함수 호출 방식에 따라 동적으로 결정된다.

  1. 일반 함수 : 전역 객체와 바인딩
  2. 메서드 : 메서드를 호출한 객체와 바인딩
  3. 생성자 함수 : 함수가 생성할 인스턴스와 바인딩
  4. call,apply,bind : 첫 번째 인수로 전달하는 객체에 바인딩

- bind / apply / call 의 차이점

  bind : 함수가 가리키는 this만 바꾸고 호출하지 않음

  apply : 첫번째 인자로 this값, 두번째 인자로 배열로 함수 호출에 필요한 인자들을 입력하며 함수를 호출

  call : 첫번째 인자로 this값, 두번째 인자부터 함수 호출에 필요한 인자들을 입력하며 함수를 호출

### Scope

- let와 var차이는?

  var : 함수레벨 스코프를 따르고 재선언, 재할당이 모두 가능하다, 또한 변수 호이스팅이 발생한다

  let : 블록레벨 스코프를따르고 재할당은 가능하지만 재 선언은 불가능하다.

- 렉시컬 스코프란?

  함수가 선언되는 위치에 따라서 상위 스코프가 결정되는 스코프

### 클로저

- 클로저란?

  내부 함수가 선언되었을 때 외부 스코프를 기억하여 외부함수 밖에서 호출될 때 외부함수의 지역 변수에 접근할 수 있는 함수

- 클로저는 함수를 왜 반환해야할까?

  함수를 반환해야 스코프 체인을 통해 외부함수의 지역변수에 접근할 수 있다.

- 클로저를 사용하는 이유는?

  전역 변수의 남용을 막고 변수값을 은닉하는 용도로 사용할 수 있다.

  프로젝트에서 싱글톤 패턴을 구현하기할 때 클로저를 사용해서 전역변수의 사용을 막을 수 있었다.

### 이벤트루프

- 자바스크립트는 싱글스레드인데 어떻게 비동기 처리를 할까?

  이벤트 루프를 통해 비동기 처리를 할 수 있다.

- 이벤트루프에 대해 설명해보세요

  자바스크립트엔진은 메모리힙과 콜 스택으로 구성되어있다.

  비동기 처리가 들어오면 브라우저에게 이벤트를 요청한 후 스택에서 제거된다. 이후 비동기 처리가 완료되면 테스크 큐에 담기게 된다. 이벤트 루프가 지속적으로 콜 스택을 관찰하며, 콜스택이 비었을 때 테스크 큐의 첫번째 값을 콜 스택에 추가해준다.

  마이크로 테스크큐 ( promise ) ⇒ 애니메이션 프레임 ( request AnimationFrame API ) ⇒ 테스크큐

### 이벤트 처리

- 이벤트 버블링과 캡처링에 대해 설명해주세요

  이벤트 버블링은 이벤트가 document 객체를 만날때 까지 상위 요소로 전달되는 것

  이벤트 캡처링은 이벤트가 document 객체부터 클릭한 객체까지 하위요소로 전달되는 것

- 이벤트 위임이란?

  이벤트 위임은 버블링을 통해 상위 객체에 이벤트를 등록하여 하위 객체를 제어하는 방식

  - 장점
    - 많은 핸들러를 할당하지 않아도 되어 메모리 절약
    - 객체를 추가, 제거시 이벤트 핸들러를 추가, 제거할 필요가 없다
  - 단점
    - 이벤트가 반드시 버블링 되어야한다.

- preventDefault와 stopPropagation의 차이

  preventDefault : 기본적으로 정의된 이벤트를 작동하지 못하게 막는 용도 ex) form의 submit

  stopPropagation : 버블링과 캡처링을 막는 메서드

### 실행 컨텍스트

- 실행컨텍스트란?

  자바스크립트의 scope, hosting, clousre 등의 동작 원리를 담고있는 핵심원리

- 실행 컨텍스트는 어떻게 구성되어있나요?

  1. Variable Object ( 변수 객체 )

     실행에 필요한 여러 정보를 담고있는 객체

     함수, 변수 등 ( 함수컨텍스트일 경우 매개변수 )

  2. 스코프 체인

     일종의 리스트로 중첩된 함수의 스코프 레퍼런스를 차례로 저장한다. 현재 실행컨텍스트의 활성 객체부터 순차적으로 상위 컨텍스트의 활성객체를 가리키며 마지막 리스트는 전역 객체를 가리킨다.

     현재 스코프, 즉 활성 객체에서 검색해보고 실패하면 스코프 체인에 담긴 순서대로 검색을 이어간다.

  3. this 값

### 호이스팅

- 호이스팅이란?

  호이스팅은 유효스코프의 최 상단으로 선언문을 끌어올리는 것이다.

- 호이스팅이 발생하는 이유는?

  var 변수에서는 선언과 초기화가 한번에 일어나기 때문에 발생한다.

  let 변수도 호이스팅이 발생하지만 초기화가 되지않아 reference Error 이 발생한다.

### 자료형

- 원시형 데이터와 참조형 데이터의 차이

  원시형 데이터는 값을 저장하는 타입으로 불변성을 가진다. 원시타입 비교시 값을 비교하지만, 참조 타입은 주소를 참조하고 있으므로, 주소값을 비교대상으로 한다.

- undefined와 null의 차이

  둘 다 값이 없음을 의미

  undefined : 값이 할당되어 있지 않은 변수, 즉 데이터 타입이자 값

  null : 명시적으로 값이 비어있음을 나타낸다

### Callback vs Promise vs async&await

- callback

  비동기 처리 방식 중 하나로 비동기 로직을 처리하기 위해 콜백 함수를 연속적으로 사용할때 콜백 지옥이라는 문제가 발생한다. 콜백안에 콜백함수가 있는 형식으로 가독성이 떨어지고 로직을 변경하기 힘들다. 이를 해결하기 위해 Promise나 Async&Await를 사용한다.

- Promise

  자바스크립트 비동기 처리에 사용되는 객체

  - Pending : 비동기 처리 로직이 완료되지 않은 상태
  - Fullfilled : 비동기 처리가 완료되어 Promise가 결과 값을 반환해준 상태
  - Rejected : 비동기가 실패하거나 오류를 발생한 상태

- Async&Await

  기존 처리방식인 Callback & Promise의 단점을 보완하고 가독성 높은 코드로 작성 가능

  함수앞에 async 예약어를 붙이고 비동기 처리 코드앞에 await를 붙인다. 비동기 처리가 Promise 객체를 반환할 때 의도대로 동작한다.

  예외처리는 try catch 문을 사용

### webpack과 babel

- webpack이란

  자바스크립트의 코드가 많아지면 하나의 파일로 관리하는데 한계가 있다. 그렇다고 여러개의 파일을 두면, 브라우저에서 여러개의 파일을 받아와야해서 네트워크 비용이 증가하게된다.

  이를 해결하기위해 webpack를 사용한다.

  Static Module Bundler로 Dependencies Graph를 통해 필요한 형태의 하나, 여러개의 번들로 생성한다.

  다른 모듈 번들러도 많지만 성능상 우수하다.

- bundle란?

  작동하는데 필요한 모든 것을 포함하는 Package

- babel이란

  한마디로 트렌스파일러

  크로스 브라우징 이슈를 해결하기 위해 만들어짐, EX6+, 타입스크립트, jsx등 다른 언어로 분류되는 언어들에 대해서 모든 브라우저에서 동작할 수 있도록 호환성을 지켜준다

### ES6+ 추가된 문법

1. 변수

   let, const 추가

2. Destructuring ( 구조분해할당 )

   ```jsx
   const arr = [1, 2, 3];
   const [one, two, three] = arr;
   const person = { name: "kim", age: 20 };
   const { name, age } = person;
   ```

3. 템플릿 스트링

   ```jsx
   const name = "Kim";
   console.log(name + "님"); // 일반
   console.log(`${name}님`); // 템플릿 리터럴
   ```

4. Arrow Function

   순수하게 함수의 역할을 하며 익명함수로 사용된다. this역시 생성될 당시의 상위 스코프를 따른다

   ```jsx
   const f = (x) => x * x;
   ```

5. 함수 매개변수

   - default 설정 : 기본 값을 설정할 수 있다.

     ```jsx
     const f = (x, y = 10) => x + y;
     ```

   - Rest : 가변인자를 사용하며 배열로 치환

     ```jsx
     const f = (x, ...y) => x * y.length;
     ```

   - Spread : 인자를 rest로 전달할 수 있다

     ```jsx
     const f = (x, y, z) => x + y + z;
     f(...[1, 2, 3]);
     ```

6. Class
7. for of

   of로 배열의 값을 가져올 수 있다.

8. 고차함수

   새로운 배열를 반환하는 함수

   - map
   - filter
   - reduce

9. Object 할당 문법

   ```jsx
   const a = 1;
   const b = 2;
   const obj = { a, b };
   ```

10. Promise, async&await

### Garbage Collection

- Mark-and-sweep 알고리즘

  root부터 시작하여 root가 참조하는 Obj, 참조된 Obj가 참조하는 Obj 들을 닿을수 있는 Obj로 표시한다. 표시가 되어있지 않은 Obj에 대해 가비지 콜랙션을 수행한다.

  순환참조문제를 해결, 대부분에 브라우저에서 사용.

- reference-counting 알고리즘

  변수를 선언하고 참조 값이 할당되면 참조 카운트가 1이 되고 다른 변수가 같은 값을 참조하면 참조 카운트가 늘어난다. 마찬가지로 해당 값을 참조하는 변수에 다른 값을 할당하면 원래 값의 참조 카운터가 줄어든다. 참조 카운트가 0이되면 가비지컬렉터가 실행될때 메모리를 회수한다

  순환참조문제 : 서로 참조하고있을 경우 참조카운트가 0이되지 못해서 메모리낭비가 발생한다

## React

- 리액트란?

  프론트엔드 라이브러리로 컴포넌트 기반으로 되어있어 컴포넌트에 데이터를 내려주면 개발자가 설계한대로 UI가 만들어져 사용자에게 보여진다.

  핵심원리로는 Virtual DOM을 사용한다

  - Virtual DOM

    DOM조작이 속도상 느리기 떄문에 VDOM을 사용한다

    DOM차원에서 더블 버퍼링이라고 생각한다.

    변화가 발생하면 오프라인 DOM 트리에 적용 ( 실제 렌더링이 되지 않아 연산 비용이 작다) 후 최종적인 변화를 실제 DOM에게 전달한다 ( 모든 변화를 하나로 묶어서 진행, 레이아웃 계산과 렌더링의 규모는 커지지만 한번의 계산으로 연산 횟수를 줄일 수 있다 )

  - 비교과정

    휴리스틱 알고리즘

    - 서로다른 타입을 가진 두 엘리먼트는 다른 트리를 만들어낸다.
    - 개발자가 key prop를 통해 자식 엘리먼트의 변경 여부를 표시할 수 있다.

- 프로젝트 진행하면서 리액트 몇 버전을 사용하셨나요?

  17.0.1버전을 사용했다.

- 최신버전이 몇 버전인줄 알고 계신가요?

  17.0.2

- 최신버전에서 새롭게 추가된 문법에 대해 말해주세요

  [React version - React Blog](<[https://ko.reactjs.org/versions/](https://ko.reactjs.org/versions/)>)

  clean up 함수를 비동기로 처리

  document 수준에서 이벤트 핸들러를 연결하지 않고 렌더링 되는 루트노드 컨테이너에 첨부한다.

- react 라이프 사이클에대해 말해주세요

  ![image](https://blog.kakaocdn.net/dn/w3ych/btqAQOjLtoN/R0EdkXLZBJ2qr2As5TKTt0/img.png)

- useEffect에 대해 설명해주세요

  컴포넌트가 렌더링 될 때마다 특정 작업을 실행할 수 있도록 해주는 Hooks

  의존 배열을 입력받아 해당 값이 변경되었을 때 실행, 의존배열이 없다면 리렌더링시 항상 실행

- useEffect는 어떤 라이프 사이클에 해당하나요?
  - componentDidMount, componentDidUpdate, componentWillUnmount
- cleanup 함수에 대해 말해주세요

  useEffect에서 return하는 함수

  컴포넌트의 unmount 이전, update 직전에 어떤 작업을 수행하고 싶을 때 cleanup를 반환해준다.

  정리가 필요한 side-effect를 해결하기 위해 사용한다.

  ex) 구독을 해제할때 사용한다.

- 클래스형과 함수형 컴포넌트의 차이점에 대해 말해주세요
  - 클래스형
    - state, lifeCycle 관련 기능사용 가능하다.
    - 메모리 자원을 함수형 컴포넌트보다 조금 더 사용한다.
    - 임의 메서드를 정의할 수 있다.
  - 함수형
    - state, lifeCycle 관련 기능사용 불가능[ hook를 통해 해결 ]
    - 메모리 자원을 클래스형 컴포넌트보다 덜 사용한다.
    - 컴포넌트 선언이 편하다.
- state와 props에 대해 말해주세요
  - state : 컴포넌트의 상태로 변할 수 있는 값, 컴포넌트 내부에서선언되고 관리된다.
  - props : 부모 컴포넌트로 받은 값으로 변경할 수 없는 읽기 전용의 값.
- 고차 컴포넌트에 대해 말해주세요

  컴포넌트 로직을 재사용하기 위한 react의 기술로 컴포넌트를 취하여 새로운 컴포넌트를 반환하는 함수

- 프로젝트에서 어떻게 상태관리를 했는지 말해주세요

  저는 contextAPI를 통해 상태관리를 하고, graphQL을 사용했을 때는 apollo-client, swr의 캐시를 통해서 상태관리를 했습니다.

- 리액트 키에 대해 설명해주세요

  키값을 통해 가상돔은 리스트의 값을 구분한다

  인덱스를 키값으로 설정했을 때, 배열 뒤쪽에 순차적으로 데이터가 추가된다면 문제가 발생하지 않지만, 앞쪽에 추가될 경우 키값이 모두 변경되어 리스트 전체가 다시 렌더링된다

  키값에 유니크한 값을 부여해서 불필요한 렌더링을 막을 수 있다

  키값은 하나의 리스트 내에서만 유일하면 된다

- SPA에 대해 설명해주세요

  단일 페이지 애플리케이션으로 모던 웹의 패러다임이다. 기본적으로 단일 페이지로 구성된다. 기본적으로 필요한 모든 정적 리소스를 최초에 한번 다운로드하고, 이후에는 패이제 갱신에 필요한 데이터만 받아 페이지를 갱신하므로 전체적인 트래픽을 감소시킬 수 있다. 또한 전체 페이지를 다시 렌더링 하지않고 변경되는 부분만 갱신하므로 좋은 사용자 경험을 제공할 수 있다.

  그러나 초기에 모든 리소스를 다운받기 위해 초기 구동속도가 느리고 SEO 문제가 발생한다