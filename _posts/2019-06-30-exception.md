---
published: true
layout: single
title: "Exception(예외)"
category: knowledge
tags: [exception]
comments: false
---

## Exception?

> 프로그램 실행 중에 정상적인 프로그램의 흐름에 어긋나는 이벤트

- 발생할 상황을 미리 예측하여 처리할 수 있다.

## Exception 종류

### 1. Checked Exception

- 예외 상황을 예측할 수 있다.
- 컴파일단계에서 확인 가능하다.
- 예외 처리를 해야한다(강제).
  - 처리하지 않으면, 컴파일 에러가 발생한다.

### 2. Unchecked Exception

- 예측하지 못하는 상황에서 발생한다.
- 실행단계(런타임)에서 확인 가능하다.
- try-catch문이나 throws로 처리를 강제하지 않는다.
- 주로 프로그래머의 실수에 의해 발생된다.

## 예외처리

### 1. try-catch-finally

- try 블럭
  - 예외가 발생할 가능성이 있는 범위를 지정
- catch 블럭
  - try 블럭에서 예외가 발생하면 실행되는 블럭
- finally 블럭
  - try-catch문을 사용할 때, 선택사항
  - 예외가 발생한 것과 상관없이 **무조건 마지막**에 실행되는 블럭

### 2. thorws

- 예외 처리 회피
- 발생한 예외를 호출한 메소드에 넘겨주는 방식
