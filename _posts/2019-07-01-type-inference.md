---
published: true
layout: single
title: "Type Inference"
category: typescript
tags: [typescript]
comments: false
---

## Type Inference(타입 추론)?

```typescript
const bool = true
const num = 1
```

위 코드를 보면, bool, num 변수에 타입을 선언하지 않았다.  
하지만, 초기화한 값을 통해 bool변수는 boolean형, num변수는 number형인 것을 추론할 수 있다.

```typescript
const numberArr: number[] = [1, 2, 3, 4, 5]
const stringArr: string[] = numberArr.map(num => num.toString())
```

위 코드에서는 numberArr 변수는 number 배열 타입으로 선언했다.  
여기서 map 안의 num은 따로 타입을 지정하지는 않았다. 하지만 numberArr가 number 배열 타입인 것을 통해 number 타입인 것을 추론할 수 있다.  
이처럼 변수를 선언할 때, 타입을 정의하면 map함수에서 type inference가 가능하다.

**:bulb: 변수 선언할 때, 타입 정의하는 습관을 들이자!**

<br>

**[참고]**  
<https://www.typescriptlang.org/docs/handbook/type-inference.html>
<https://www.tutorialsteacher.com/typescript/type-inference>
