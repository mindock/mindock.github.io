---
published: true
layout: single
title: "Object Shorthand Notation"
category: typescript
tags: [typescript, javascript, es6(es2015)]
comments: false
---

## 1. 정의

- shorthand: 약칭(긴 것을 줄인 것)
- notation: 표기법

**:bulb: object를 줄여 표기하는 것을 말한다.**

## 2. 사용법(예시)

```
interface A {
    b: string;
    c: number;
}
```

A라는 인터페이스가 있다.

```
const a: A = { b: 'hi', c: 0 };
```

A타입의 a 변수를 만들어보았다. 해당 변수는 b에는 'hi', c에는 0을 가진다.

만약 hi와 0이 변수에 담겨있다면?

```
const b = 'hi';
const c = 0;

const a: A = { b, c };
```

이때, `const a: A = { b: b, c: c };` 와 같은 형태로 써야하는거 아닌가? 라는 생각이 들 수도 있다.
하지만, 이름이 같은 경우에는 `const a: A = { b, c };` 처럼 줄여서 표현한다.
이게 바로 **object shorthand notation** 이다!

```
const bb = 'hi';
const cc = 0;

const a: A = { b: bb, c: cc };
```

이 경우에는 변수명이 다르기 때문에 단축해서 표현할 수 없다.
