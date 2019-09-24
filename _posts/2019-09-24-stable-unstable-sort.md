---
published: true
layout: single
title: "Stable Sort, Unstable Sort"
category: algorithm
tags: [stable, unstable, sort, stable sort, unstable sort]
comments: false
---

## Stable Sort(안정 정렬)

- 동일한 정렬기준을 가진 것은 **정렬하기 전의 순서와 정렬한 후의 순서가 동일하다.**
- ex) 아래의 숫자들을 오름차순으로 정렬해보자. 이때, 1'과 1은 같은 숫자이다.
  - 5 3 1' 9 2 1
- 여기서 동일한 정렬기준을 가진 1'과 1이 존재한다. 이런 경우에 정렬 전/후의 순서가 동일하기 때문에 아래와 같은 결과가 나온다.
  - 1' 1 2 3 5 9

### 종류

1. Bubble Sort(버블 정렬)
2. Insertion Sort(삽입 정렬)
3. Merge Sort(합병 정렬)

## Unstable Sort(불안정 정렬)

- 동일한 정렬기준을 가진 것의 **정렬 전/후 순서가 다를 수 있다.**
- ex) 아래의 숫자들을 오름차순으로 정렬해보자. 이때, 1'과 1은 같은 숫자이다.
  - 5 3 1' 9 2 1
- 여기서 동일한 정렬기준을 가진 1'과 1이 존재한다. 이런 경우에 정렬 전의 순서가 유지된다는 보장을 할 수 없다. 그렇기 때문에 아래의 2가지 결과가 모두 나올 수 있다.
  - 1' 1 2 3 5 9
  - 1 1' 2 3 5 9

### 종류

1. Selection Sort(선택 정렬)
2. Quick Sort(퀵 정렬)
3. Heap Sort(힙 정렬)

## Reference

- <http://egloos.zum.com/entireboy/v/3516993>
- <http://blog.naver.com/zephyehu/150013176075>
- <https://ko.khanacademy.org/computing/computer-science/algorithms/>
