---
layout: post
title: "[TypesScript] 타입스크립트 기초 공부"
date: 2021-05-12 17:10:00 +0900
categories: [Study, TypeScript]
tags: [typescript]
comments: true
---

### 타입스크립트란?

타입스크립트는 자바스크립트에 타입을 부여한 언어이다. 자바스크립트의 확장된 언어라고 볼 수 있다. 타입스크립트는 자바스크립트와 달리 브라우저에서 실행하려면 변환을 해줘야 하고, 이 과정을 **컴파일(compile)** 라고 부른다.

### 타입스크립트를 사용하는 이유?

#### 에러의 사전방지

동적인 자바스크립트에 정적 타입을 지원하여 에러를 사전에 미리 방지할 수 있다.

```javascript
// javascript
const sum = (a, b) => a + b;

sum(10, 20); // 30
sum("10", "20"); // 1020
```

```typescript
//typescript
const sum = (a: number, b: number) => a + b;

sum(10, 20); // 30
sum("10", "20"); // Error: "10"은 number에 할당될 수 없습니다.
```

#### 예측 가능한 코드

명시적 타입 지정을 통해 개발자의 의도를 명확하게 기술할 수 있다. 또한, 코드의 가독성을 높이고 예측할 수 있게 하여 디버깅이 쉬워진다.

### 기본 타입

타입스크립트로 함수나 변수에 타입을 정의할 수 있으며 기본 타입은 크게 다음 12가지가 있다.

- Boolean
- Number
- String
- Object
- Array
- Tuple
- Emum
- Any
- Void
- Null
- Undefined
- Never

#### String

자바스크립트 변수의 타입이 문자열인 경우 아래와 같이 선언해서 사용한다.

```typescript
const str: string = "hi";
```

> **TIP**
> 위와같이 `:`를 이용해서 자바스크립트 코드에 타입을 정의하는 방식을 타입 표기라고 한다.

#### Number

자바스크립트 변수 타입이 숫자인 경우 아래와 같이 선언해서 사용한다.

```typescript
const num: number = 10;
```

#### Boolean

자바스크립트 변수의 타입이 Bool인 경우 아래와 같이 선언해서 사용한다.

```typescript
const isTrue: boolean = true;
```

#### Array

자바스크립트 변수의 타입이 배열인 경우 아래와 같이 선언해서 사용한다.

```typescript
const arr: number[] = [1, 2, 3];
const arr: Array<number> = [1, 2, 3];
```

#### Tuple

튜플은 배열의 길이가 고정되어 있고 각 요소의 타입이 지정되어 있는 배열의 형식이다. 만약 정의하지 않은 타입, 인덱스로 접근할 경우 에러가 발생한다.

```typescript
const arr: [string, number] = ["hi", 2];
arr[1] = "!"; // 에러, '!'은 number에 할당될 수 없다.
arr[3] = "hello"; // 에러, property 3은 존재하지 않는다.
```

#### Enum

이넘은 C, Java와 같은 다른 언어에서 흔히 사용되는 타입으로 특정 값들의 집합을 의미한다.

```typescript
enum Country {
  Korea,
  China,
  Japan,
}
const myCountry: Country = Country.Korea;
// or
const myCountry: Country = Country[0]; // 인덱스로 접근이 가능하다.
```

#### Any

모든 타입에 대해 허용한다는 의미를 가지고 있으며, any를 사용하는 순간 타입 추론을 할 수없기 때문에 최대한 지양하는 것이 좋다.

```typescript
let str: any = "hi";
str = 1;
```

#### Void

변수에 `undefined` 와 `null`만 할당하고, 함수에는 반환 값을 설정할 수 없는 타입이다.

```typescript
const notuse: void = null;
const notfound = (): void => {
  console.log("Not Found!");
};
```

#### Never

함수의 끝에 절대 도달하지 않는다는 의미

```typescript
const neverEnd = (): never => {
  while (true) {}
};
```

### Interface

인터페이스는 일반적으로 타입을 체크하기 위해 사용되며 typealias와 유사한 기능을 한다. 여러가지 타입을 갖는 프로퍼티로 이루어진 새로운 타입을 정의한다.

```typescript
interface Person {
  name: string;
  age: number;
  address: string;
}
```

### typealias

인터페이스와 비슷한 동작을 하지만 default값을 지정할 수 있다는 점이 다르다.

```typescript
type Person {
  name: string;
  age: number;
  address: string;
}
```

### type vs interface

typealias와 interface의 가장 큰 차이점은 타입의 확장 가능 / 불가능 여부이다. 인터페이스는 확장이 가능한 반면 typealias는 확장이 불가능하다. 따라서 interface로 선언해서 사용하는 것이 좋다.

#### generics

제네릭은 타입스크립트에서 함수, 클래스, interface, type을 정의하는 시점에 매개변수나 반환값의 타입을 선언하기 어려운 경우 사용한다.

```typescript
const MyParams<T>(param:T) => {param}

const params = MyParams(10)
```

### Union

유니온 타입은 자바스크립트의 OR연산자와 같은 의미로 사용되는 타입이다.

```typescript
const numOrStr = (param: string | number) => {
  // ...
};
```

위 함수의 파라미터인 param에는 문자열이나 숫자 타입이 모두 올 수 있다. `|`연산자를 사용해서 여러 타입을 연결하는 방식을 유니온 방식이라고 부른다.
