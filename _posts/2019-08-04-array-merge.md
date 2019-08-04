---
published: true
layout: single
title: "Array Merge"
category: typescript
tags: [typescript, javascript, concat, push, spread]
comments: false
---

> concat, spread, push의 차이점

```
const arr: string[] = ['a', 'b', 'c'];
const tempArr: string[] = ['d', 'e'];

const concatArr = arr.concat(tempArr);
const spreadArr = [...arr, ...tempArr];
arr.push(...tempArr);
```

두 개의 배열을 합치거나 하나의 원소를 추가해야하는 경우, `concat`, `spread`, `push` 중 어느 것을 사용해야할지 판단하기 어려운 경우가 있었다. 다 똑같은 작업을 하는 것 같은데 어떤 차이점이 있는지 각각 정리해보고자 한다.

## concat vs push vs spread

### 1. concat

```
const a: number[] = [1, 2, 3];
const b: number[] = [4, 5, 6];
const c: number[] = [7, 8, 9];

a.concat(b);
const d = a.concat(c);
```

이 코드에서 d의 결과는? `[1, 2, 3, 7, 8, 9]` 이다.
'왜 `[1, 2, 3, 4, 5, 6, 7, 8, 9]`가 아니지? `a.concat(b)`를 실행했는데?' 라는 생각이 들 수 있다.

`a.concat(b)`를 실행하면 `[1, 2, 3, 4, 5, 6]` 배열을 반환한다. 하지만, a 배열의 값을 바꾸는 것이 아니라 새로운 배열을 반환한다. 그렇기 때문에 a는 여전히 `[1, 2, 3]` 값을 가진 상태로 `a.concat(c)`를 실행해 d 값은 a와 c가 합쳐진 새로운 배열인 `[1, 2, 3, 7, 8, 9]`이 된다.

```
const e: number[] = a.concat(b, c, 10, [11, 12, 13]); // e = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13]
```

또한, 위와 같이 여러 배열을 이어 붙일 수 있다.

<https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/concat>

### 2. push

```
const zero: number[] = [0];
const a: number[] = [1, 2, 3];
const b: number[] = [4, 5, 6];
const c: number[] = [7, 8, 9];

zero.push(a);
a.push(...b);
const d = a.push(...c);
```

이 코드에서 d의 결과는? `9` 이다.
'배열도 아니라? 숫자?'

d가 왜 9인지 알아보기 전에 먼저 `zero.push(a)`에 대해 알아보자.  
`concat`과 같다면, zero는 그대로 `[0]`이어야 한다. 하지만 틀렸다. zero의 값은 바꼈다. 그렇다면... `[0, 1, 2, 3]`이겠지? 라는 생각이 제일 먼저 들 것이다. 하지만 그것도 틀렸다.  
zero의 값은 `[0, [1, 2, 3]]`이 된다. 하지만, typescript에서는 number[]형식에 맞지 않기 때문에 해당 구문에서 에러를 던질 것이다. 주의하자!

`[0, 1, 2, 3]`과 같이 만들기 위해서는 **spread syntax(...)**를 사용해야한다.

`a.push(...b)`가 바로 spread operator를 적용한 예이다. 해당 코드를 실행하면, push의 경우는 기존 배열의 값을 바꾸기 때문에 a는 `[1, 2, 3, 4, 5, 6]` 값을 가진다.

그렇다면, 마지막 문장 `a.push(...c)`를 실행한 후 a는 `[1, 2, 3, 4, 5, 6, 7, 8, 9]` 값을 가진다. 그런데 왜 d는 `9`라는 값을 가지게 된걸까? push는 배열 끝에 원소들을 추가하고 새로운 길이를 반환하기 때문에 a의 길이인 9를 반환한다.

```
zero.push(a);
a.push(...b, ...c);
```

혼자 막 코드를 써보다가 이상한 점을 발견했다.  
나는 당연히 위 코드의 결과는 zero는 `[0, [1, 2, 3]]`, a는 `[1, 2, 3, 4, 5, 6, 7, 8, 9]` 이라고 생각했다. 값만 복사해서 배열 끝에 추가할 것이라 예상했기 때문이다.

![zero의 다른 결과](/assets/images/array_push_simple.jpeg)

하지만, 내 예상과는 다르게 zero에 push한 a 변수를 복사해서 사용한 것이 아니다. 같은 주소를 참조하고 있기 때문에 `zero.push(a)`만 실행했을 경우의 zero의 값과 `a.push(...b, ...c)`를 실행한 후의 zero의 값은 다르게 된다.

```
zero.push(a);
a.push(...b, ...c, ...zero);
```

![혼란의 카오스](/assets/images/array_push.jpeg)

a에 zero까지 push하면... a가 계속 나오는 매직을 볼 수 있다.

<https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/push>

### 3. spread

```
const a: number[] = [1, 2, 3];
const b: number[] = [4, 5, 6];
const c: number[] = [7, 8, 9];

[...a, ...b];
const d = [...a, ...c];
```

이 코드에서 d의 결과는? `[1, 2, 3, 7, 8, 9]`이다.

spread는 concat과 같이 원본 배열을 변경하는 것이 아니라 새로운 배열을 반환한다. ...을 통해 원본 배열을 풀어서 새로운 배열을 만든다. a를 spread를 통해 풀어쓰면, `1, 2, 3`, c를 spread를 통해 풀어쓰면, `7, 8, 9`이다. 그래서 d는 a, c를 풀어쓴 것을 `[]`로 감싸 `[1, 2, 3, 7, 8, 9]` 값을 가지게 된다.

<https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_syntax>

## 결론?

### concat

- 2개보다 많은 배열을 합칠 때 유리하다.
- 기존 배열을 변경하지 않는다.  
  새로운 배열을 리턴한다.

### push

- 1개보다 많은 원소를 배열 끝에 추가할 때 유리하다.
- 기존 배열을 변경한다.  
  새로운 배열의 길이를 리턴한다.

### spread

- 1차원 배열의 복사할 때 효과적으로 동작한다.  
  다차원 배열을 복사하는 것에는 적합하지 않을 수 있다.
- 기존 배열을 변경하지 않는다.  
  새로운 배열을 리턴한다.

<https://codeburst.io/jsnoob-push-vs-concat-basics-and-performance-comparison-7a4b55242fa9>
