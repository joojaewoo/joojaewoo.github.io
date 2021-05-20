---
layout: post
title: "[JavaScript] var, let, const 차이"
date: 2021-05-20 13:10:00 +09:00
categories: [Study, JavaScript]
tags: [javascript]
comments: true
---

JavaScript에는 `var`, `let`, `const` 세 가지로 변수 선언을 할 수있다. 각각의 차이점에 대해 알아보자.

### 1. 변수 선언 방식

`var` 변수는 재선언이 가능하다.

```javascript
var name = "joojaewoo";
console.log(name); // joojaewoo

var name = "joo";
console.log(name); // joo
```

변수를 한 번 더 선언했음에도 불구하고, 서로 다른값이 잘 출력된다.
유연하게 사용될 수도 있지만, 코드량이 많아진다면 어디서 선언되었고 어떻게 사용되는지 파악하기 힘들 뿐 아니라 값이 바뀔수도 있다.

`let`와 `const`는 ES6이후 `var`을 보완하기 위해 추가된 변수 선언방식이다.

```javascript
let name = "joojaewoo";
console.log(name); // joojaewoo

let name = "joo";
console.log(name); // Uncaught SyntaxError: Identifier 'name' has already been declared
```

`name`이 이미 선언되었다는 에러가 발생한다. `let`와 `const`는 변수 재 선언이 불가능한 공통점을 가지고 있지만 둘의 차이점은 `immutable` 여부이다.
`let`는 아래와 같이 변수에 재 할당이 가능하다.

```javascript
let name = "joojaewoo";
console.log(name); //joojaewoo

name = "joo";
console.log(name); //joo
```

그러나 `const`는 재선언, 재할당이 모두 불가능하다.

```javascript
const name = "joojaewoo";
console.log(name); // joojaewoo

name = "joo"; // Uncaught TypeError: Assignment to constant variable.
console.log(name);
```

### 2. 변수의 유효범위 (스코프)

> 스코프 ( Scope ) : 변수에 접근할 수 있는 범위
> 전역 스코프 : 말 그대로 전역에 선언되어있어 어느 곳에서든지 해당 변수에 접근할 수 있다는 의미
> 지역 스코프 : 해당 지역에서만 접근할 수 있어 지역을 벗어난 곳에선 접근할 수 없다는 의미

`var` 변수의 유효범위는 전역 스코프를 제외하면 오직 함수 스코프를 따른다.

```javascript
var a = 1;
function print() {
  var a = 111;
  console.log(a); //111
}

print();
console.log(a); //1
```

`print`함수에서의 `console.log(a);`는 a를 출력하기위해 자신의 함수 스코프내의 변수 a를 찾아본다. `var a =111;`을 찾은 다음 이 값을 출력하면서 '111'이라는 값이 출력된다.

그러나 함수 단위의 스코프를 따르기 때문에 문제점도 발생한다.

```javascript
for (var i = 0; i < 10; i++) console.log(i);
// 1 2 3 ... 9 한줄에 하나씩 순선대로 출력

console.log(i); // 9
```

반복문이 종료되었지만 `var`변수인 `i`는 함수 단위의 스코프를 사용하기 떄문에 `i`의 값이 유지되는 문제가 발생한다. 이처럼 `var`변수는 전역변수를 남발할 가능성을 높인다.

이를 해결하기 위해 `let`와 `const`는 블록 레벨 스코프를 따른다.

```javascript
let a = 1;
{
  let a = 2;
  let b = 2;
}
console.log(a); //  1
console.log(b); //  ReferenceError: b is not defined
```

블록 내에서 선언된 변수 a와 b는 지역변수이다. 전역에서 선언된 a 와 다른 별개의 변수이다. 따라서 전역 범위에서 b는 참조할 수 없고, a는 1을 가르킨다.

대부분의 문제는 전역변수로 인해 발생하는데, 유효범위가 넓어서 어디서 어떻게 사용될 것인지 파악하기 힘들며, 비 순수함수에 의해 의도하지 않게 변경될 가능성이 있어 복잡성을 증가시킨다. 따라서 변수의 스코프는 좁을수록 좋다.

Scope와 Scope Chain에 대해서는 다음에 자세히 포스팅 할 예정이다.

### 3. 호이스팅

> 호이스팅 ( Hoisting ) : 변수나 함수 선언문 등을 해당 스코프의 가장 최 상단으로 끌어올리는 것

자바스크립트는 모든 선언에 대해서 호이스팅을 한다.

하지만 `var`로 선언된 변수와 같이 `let`로 선언된 변수를 선언문 이전에 참조하면 ReferenceError이 발생한다

```javascript
console.log(foo); // undedinfed
var foo;

console.log(bar); // Error: Uncaught ReferenceError: bar is not defined
let bar;
```

이는 `let`로 선언된 변수는 스코프의 시작에서 변수의 선언까지 TDZ에 빠지기 때문이다.
변수는 `선언단계 => 초기화단계 => 할당단계`에 걸쳐 생성되는데 `var`으로 선언된 변수는 선언과 초기화 단계가 한번에 이루어진다. 그러나 `let`로 선언된 변수는 선언단계와 초기화 단계가 분리되어 진행된다.

```javascript
//스코프의 상단에서 선언단계가 실행된다.
// 아직 변수가 초기화(메모리 공간 확보) 되지않았다.

console.log(foo); // ReferenceError: foo is not defined

let foo; // 변수 선언문에서 초기화 단계가 실행된다.
console.log(foo); // undefined

foo = 1; // 변수 할당문에서 할당 단계가 실행된다.
console.log(foo); //1
```

### 정리

변수 선언에는 많은 문제를 야기할 수 있는 `var`은 지양하고 기본적으로 `const`를 사용하고, 재 할당이 필요한 경우 `let`를 사용하는 것이 좋다. `const`는 의도치 않은 재할당을 방지해주기 때문에 의도치 않은 결과를 방지할 수있다.
