---
published: true
layout: single
title: "Expression Statement 차이"
category:
tags: [expression, statement]
comments: false
---

## Expression(표현식)

- 값, 변수, 연산자 조합
- 하나의 값으로 수렴하는 코드 조각 (하나의 단일값으로 평가될 수 있다.)
- statement의 부분집합
- 함수 호출은 이에 해당한다.  
  (자바스크립트에서는 함수가 아무것도 반환하지 않아도 undefined가 반환하기 때문에 가능)

```
'expression'
1+2
true && false
sample()
```

## Statement(문장)

- 실행 가능한 최소의 독립적인 코드 조각
- 컴파일러가 이해하고 실행할 수 있는 모든 구문
- 프로그래밍에서 결과를 얻기 위해 수행하는 각각의 action을 말한다.
- 함수 선언, 제어문(if, switch, 반복문, ...)이 이에 해당한다.

```
const one = 1;
if (true) { // do something }
function sample() { // do something }
```

<br>

**[참고]**  
<https://2ality.com/2012/09/expressions-vs-statements.html>  
<https://maksimivanov.com/posts/statements-expressions-js>
