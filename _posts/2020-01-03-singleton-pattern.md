---
published: true
layout: single
title: "Singleton Pattern"
category: book
tags: [Head First, design pattern, singleton pattern]
comments: false
---

> **싱글톤 패턴(Singleton Pattern)**  
> 해당 클래스의 인스턴스가 하나만 만들어지고, 어디서든지 그 인스턴스에 접근할 수 있도록 하기 위한 패턴

### 고전적인 싱글턴 패턴 구현법

```java
public class Singleton {
    private static Singleton uniqueInstance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (uniqueInstance === null) {
            uniqueInstance = new Singleton();
        }
        return uniqueInstance;
    }
}
```

- private으로 생성자를 선언했기 때문에 해당 클래스에서만 인스턴스를 만들 수 있다.
- public 정적 메소드를 통해, 다른 객체에서도 Singleton 인스턴스를 접근할 수 있다.
- getInstance 메소드에서 아직 인스턴스가 만들어지지 않은 경우, private으로 선언된 생성자를 이용해 객체를 만든 다음 uniqueInstance에 그 객체를 대입한다.  
  즉, 인스턴스가 필요한 상황이 발생하기 전에는 아예 인스턴스를 생성하지 않는다. 이를 `게으른 인스턴스 생성(lazy instantiation)`라고 부른다.

### 싱글턴 패턴 적용 방법

1. 클래스에서 자신의 단 하나뿐인 인스턴스를 관리하도록 만든다.
   - 다른 클래스에서 자신의 인스턴스를 추가로 만들지 못하도록 해야 한다.
   - 인스턴스가 필요하면, 반드시 클래스 자신을 거치도록 해야 한다.
2. 어디서든 그 인스턴스에 접근할 수 있도록 만들어야 한다.
   - 다른 객체에서 이 인스턴스가 필요하면 언제든지 요청할 수 있게 만들어야 한다.
   - 요청이 들어오면, 하나뿐인 그 인스턴스를 건네주도록 만들어야 한다.

### 🤦‍♀️ 고전적인 싱글턴 패턴 문제점

![2개 객체가 만들어지는 문제](/assets/images/singleton_pattern_problem.PNG)

- 서로 다른 두 객체가 리턴되었다.

### :bulb: 멀티스레딩 문제 해결 방법

#### getInstance() 메소드를 동기화시킨다.

```java
public class Singleton {
    private static Singleton uniqueInstance;

    private Singleton() {}

    public static synchronized Singleton getInstance() {
        if (uniqueInstance === null) {
            uniqueInstance = new Singleton();
        }
        return uniqueInstance;
    }
}
```

- `synchronized` 키워드를 추가하면, 스레드가 메소드 사용을 끝내기 전까지는 다른 스레드는 기다려야 한다.  
  즉, getInstance 메소드를 동시에 실행시키는 일이 일어나지 않는다.
- 사실 동기화가 꼭 필요한 시점은 getInstance 메소드가 시작되는 때 뿐이다. 그렇기 때문에 첫 번째 과정을 제외하면 동기화는 불필요한 오버헤드만 증가시킬 뿐이다.

### 🙋‍♀️ 효율적인 문제 해결 방법

1. getInstance()의 속도가 중요하지 않다면, 그냥 둔다.
   - 애플리케이션에 큰 부담을 주지 않는다면 그냥 놔둬도 된다.
   - 메소드를 동기화하면 성능이 100배 정도 저하된다는 것은 기억해야 한다.
   - 만약 애플리케이션에서 getInstance()가 병목으로 작용한다면, 다른 방법을 생각해봐야 한다.
2. 인스턴스를 필요할 때 생성하지 말고, 처음부터 만들어 버린다.

   - 애플리케이션에서 반드시 Singleton의 인스턴스를 생성하고, 항상 그 인스턴스를 사용하는 경우, 또는 인스턴스를 실행중 수시로 만들고 관리하기가 성가신 경우 괜찮은 방법이다.

   ```java
   public class Singleton {
       private static Singleton uniqueInstance = new Singleton();

       private Singleton() {}

       public static Singleton getInstance() {
           return uniqueInstance;
       }
   }
   ```

3. DCL(Double-Checking Locking)을 써서 getInstance()에서 동기화되는 부분을 줄인다.

   - DCL을 사용하면, 일단 인스턴스가 생성되어 있는지 확인한 다음, 생성되어 있지 않았을 때만 동기화를 할 수 있다.
   - `volatile` 키워드를 사용하면, 멀티스레딩을 쓰더라도 uniqueInstance 변수가 Singleton 인스턴스로 초기화되는 과정이 올바르게 진행되도록 할 수 있다.
   - DCL은 자바 1.4 이전 버전에서는 쓸 수 없다.

   ```java
   public class Singleton {
       private volatile static Singleton uniqueInstance;

       private Singleton() {}

       public static Singleton getInstance() {
           if (uniqueInstance == null) {
               synchronized (Singleton.class) {
                   if (uniqueInstance == null) {
                       uniqueInstance = new Singleton();
                   }
               }
           }
           return uniqueInstance;
       }
   }
   ```

### 🤷‍♀️ 모든 메소드와 변수가 static으로 선언된 클래스를 만들어도 되지 않나요?

- 결과적으로는 싱글턴 패턴을 사용한 것과 똑같다.
- 하지만 필요한 내용이 클래스에 다 들어있고, 복잡한 초기화가 필요없는 경우에만 이 방법을 쓸 수 있다.
- 자바에서 정적 초기화를 처리하는 방법 때문에 일이 꽤 복잡해질 수도 있다.
  - 여러 클래스가 얽혀 있는 경우에는 꽤 지저분해진다.

### 🤷‍♀️ 싱글턴 코드의 서브클래스를 만들어도 되는 건가요?

- 생성자가 private로 되어있기 때문에 확장할 수 없다.
  - 생성자를 public, protected로 변경한다면 확장은 가능하지만, 다른 클래스에서 인스턴스를 만들 수 있기 때문에 더 이상 싱글턴이라고 할 수 없다.
- 생성자를 고쳐도 정적 변수를 바탕으로 구현하기 때문에, 모든 서브클래스들이 똑같은 인스턴스 변수를 공유하게 된다.
  - 서브클래스를 만들려면 베이스 클래스에서 레지스트리 같은 걸 구현해놓아야 한다.
- 무엇보다도 "싱글턴을 확장해서 무엇을 할 것인가?"라는 질문에 먼저 답을 할 수 있어야 할 것이다.
  - 싱글턴은 제한된 용도로 특수한 상황에서 사용하기 위해 만들어진 것이기 때문이다.

### 🤷‍♀️ 클래스 로더 두 개가 각기 다른 싱글턴의 인스턴스를 가지게 될 수도 있는 문제

- 클래스 로더마다 서로 다른 네임스페이스를 정의하기 때문에 그렇게 될 수도 있다.
- 클래스 로더가 두 개 이상이라면 같은 클래스를 여러 번(각 클래스 로더마다 한번씩) 로딩할 수도 있다.
- 클래스 로더를 여러 개 사용하면서 싱글턴을 사용한다면 조심해야 한다.
- 클래스 로더를 직접 지정해서 이 문제를 피할 수도 있다.

### 🤷‍♀️ 전역변수와 싱글턴 패턴의 차이

- 전역변수는 기본적으로 객체에 대한 정적 레퍼런스이다.
- 전역변수는 `lazy instantiation(게으른 인스턴스 생성)`을 할 수 없다.
  - 처음부터 끝까지 인스턴스를 가지고 있어야 한다.
- 전역변수는 "전역적인 접근을 제공"할 수 있지만, "클래스의 인스턴스가 하나만 존재"를 달성할 수는 없다.
- 간단한 객체에 대한 전역 레퍼런스를 자꾸 만들게 되면서 네임스페이스를 지저분하게 만드는 경향이 있다.
