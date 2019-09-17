---
published: true
layout: single
title: "Scope, Closure"
category: typescript
tags: [typescript, javascript, scope, closure]
comments: false
---

## Scope

- 변수를 사용할 수 있는 범위

### global scope(전역 스코프)

- 전체에서(코드 어디에서든지) 사용할 수 있다.

### local scope(지역 스코프)

- 해당 함수/블록에서만 사용할 수 있다.

```typescript
let a = 123 // global scope

{
  let a = 456 // local scope;
  let b = 789 // local scope;

  console.log(a) // print 456
}

console.log(a) //print 123;
console.log(b) // error
```

1. function scope(함수 스코프)

   - 함수 내부에서 변수를 선언하면, 해당 함수 내에서만 접근 가능하다.

2. block scope(블록 스코프)

   - 중괄호({}) 내부에서 변수를 선언하면, 그 중괄호 블록 내부에서만 접근 가능하다.
   - es5까지는 javascript에서 block scope을 사용할 수 없었다. var 키워드만 사용하기 때문에...
   - es6에서 **const, let** 도입되면서 block scope이 가능해졌다.

```typescript
function example() {
  const a = "hello" // function scope

  if (true) {
    const b = true // block scope
    console.log(b) // print true
  }

  console.log(a) // print 'hello'
  console.log(b) // error
}

console.log(a) // error
```

## Closure

- 자신의 scope 외부에 있는 변수에 접근할 수 있는 함수

```typescript
function first() {
  const pre = "내 이름은 "
  return function second(name) {
    const post = "입니다."
    return pre + name + post
  }
}

const myNameIs = first()
const mindock = myNameIs("mindock") // 내 이름은 mindock입니다.
const btob = myNameIs("BTOB") // 내 이름은 BTOB입니다.
```

위 예시를 보자!  
`const myNameIs = first();` 를 실행하고 myNameIs를 사용하는 부분에서 first 함수는 실행이 끝났기 때문에 지역변수인 pre는 소멸되는 것이 맞다. 하지만, second 함수에서 first 함수의 pre 변수를 계속 참조하고 있기 때문에, second 함수가 소멸될 때까지 pre 변수는 소멸되지 않는다.

:bulb: **내부함수가 외부함수의 지역 변수에 접근할 수 있다.**  
 **외부함수의 지역변수를 사용하는 경우, 내부함수가 소멸될 때까지 외부함수는 소멸되지 않는 특성을 가진다.**

<br>

**[참고]**  
<https://nigayo.github.io/page/lecture/advanced/3.html>  
<https://opentutorials.org/course/743/6544>  
<https://poiemaweb.com/es6-block-scope>
