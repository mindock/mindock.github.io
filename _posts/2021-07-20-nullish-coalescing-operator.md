---
published: true
layout: single
title: "Nullish coalescing operator(??)"
category: typescript
tags: [typescript, javascript]
comments: false
---

## || (논리 연산자 OR)
왼쪽 표현식이 falsy한 값(0, '', NaN, null, undefined)인 경우, 오른쪽 표현식 결과를 반환한다.

```javascript
null || true        // true
undefined || 0.1    // 0.1

0 || 1              // 1
'' || 'hello'       // 'hello'
NaN || 100          // 100
false || true       // true

null || undefined   // undefined
```

:bulb: 만약 0, '', NaN 과 같이 falsy한 값을 유효한 값으로 생각한 경우, 예기치 않은 결과를 초래할 수 있다.

## ?? (Nullish coalescing operator)
왼쪽 표현식이 null 또는 undefined인 경우, 오른쪽 표현식 결과를 반환한다.

```javascript
null ?? true        // true
undefined ?? 0.1    // 0.1

0 ?? 1              // 0
'' ?? 'hello'       // ''
NaN ?? 100          // NaN
false ?? true       // false

null ?? undefined   // undefined
```

### Short-circuit
만약 왼쪽이 null 또는 undefined가 아님이 판명되면, 오른쪽 표현식은 평가되지 않는다. 즉, 바로 왼쪽 표현식 결과 반환한다.

## || 와 ?? 차이 
- `||`는 첫번째 참인(truthy) 값을 반환한다.
- `??`는 첫번째 정의된(defined) 값을 반환한다.

**[참고]**  
<https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator>
<https://ko.javascript.info/nullish-coalescing-operator>