---
published: true
layout: single
title: "Strategy Pattern"
category: book
tags: [Head First, design pattern, strategy pattern]
comments: false
---

> **스트래티지 패턴(Strategy Pattern)**  
> 알고리즘군을 정의하고 각각을 캡슐화하여 교환해서 사용할수 있도록 만든다.  
> 스트래티지 패턴을 활용하면 알고리즘을 사용하는 클라이언트와는 독립적으로 알고리즘을 변경할 수 있다.

- 오리 연못 시뮬레이션 게임을 만든다고 가정하자.

### 표준적인 객체지향 기법을 사용

- Duck이라는 수퍼클래스를 만들고 그 클래스를 확장해 다른 모든 종류의 오리를 만든다.  
  ![표준적인 객체지향 기법 사용](/assets/images/strategy_pattern_1.png)

### 🤷‍♀️ 오리들이 날아다닐 수 있도록 변경해주세요!

- Duck에 fly() 메소드를 추가하고 모든 서브클래스에서 fly()를 상속받는다.  
  ![fly() 메소드 추가](/assets/images/strategy_pattern_2.png)

### 🤦‍♀️ RubberDuck(고무로 된 오리 인형)이 날라다닌다?

- 수퍼클래스 Duck에 fly 메소드가 추가되면서 일부 서브클래스에는 적합하지 않은 행동이 추가되었다.

```java
public abstract class Duck {
    public abstract void display();

    public void quack() {
        System.out.println("꽥꽥");
    }

    public void swim() {
        System.out.println("어푸어푸");
    }

    public void fly() {
        System.out.println("훨훨");
    }
}

public class MallardDuck extends Duck {
    public void display() {
        System.out.println("나는 MarrardDuck이다");
    }
}

public class RedheadDuck extends Duck {
    public void display() {
        System.out.println("나는 RedheadDuck이다");
    }
}

public class RubberDuck extends Duck {
    public void display() {
        System.out.println("나는 RubberDuck이다");
    }

    public void quack() {
        System.out.println("삑삑");
    }

    public void fly() {
    }
}
```

- 위의 RubberDuck와 같이 fly() 메소드를 오버라이드한 후, 아무것도 하지 않게 함으로써 해결할 수 있다.
- 하지만, 계속해서 규격이 변한다면, 전에 추가했던 서브클래스의 fly(), quack() 메소드를 살펴봐야 하고, 상황에 따라 오버라이드해야 할 수도 있다.
- 상속을 사용하는 것이 올바른 해결책이 아니다!

### 🙋‍♀️ fly(), quack()를 인터페이스로 만들자

- Duck 수퍼클래스에서 fly(), quack()을 빼고 fly() 메소드가 있는 Flyable, quack() 메소드가 있는 Quackable 인터페이스를 만들 수 있다.  
  ![Flyable, Quackable 인터페이스](/assets/images/strategy_pattern_3.png)
- 고무 오리가 날라다니는 것과 같은 문제점은 해결할 수 있다.
- 인터페이스에는 구현된 코드가 전혀 들어가지 않는다.
  - 서브클래스에서 fly(), quack()를 구현하도록 함으로써 코드 재사용을 전혀 기대할 수 없다.
    - 날아가는 동작을 조금 바꾸기 위해 관련된 서브클래스를 찾아 모두 고쳐야 한다.
    - 고치는 과정에서 새로운 버그가 생길 가능성도 있다.

> **디자인 원칙**  
> 애플리케이션에서 달라지는 부분을 찾아내고, 달라지지 않는 부분으로부터 분리시킨다.

### :bulb: 바뀌는 부분과 그렇지 않은 부분 분리하기

- 코드에 새로운 요구사항이 있을 때마다 바뀌는 부분이 있다면, 바뀌지 않는 다른 부분으로부터 골라내서 분리해야 한다.

  - 바뀌는 부분: fly(), quack()
  - 바뀌지 않는 부분: fly(), quack()를 제외한 Duck 클래스

- 바뀌는 행동을 갈라내기 위해 각 행동을 나타낼 클래스 집합을 새로 만든다.

  - 각 클래스 집합에는 각각의 행동을 구현한 것을 전부 집어넣는다.

- 행동(behavior) 인터페이스는 행동 클래스에서 구현한다.

  - Duck 클래스에서는 그 행동을 구체적으로 구현하는 방법에 대해서는 더 이상 알고 있을 필요가 없다.

  - 🤷‍♀️ 행동만을 나타내는 클래스를 만드는 게 좀 이상해요.
    - 클래스는 일반적으로 상태와 메소드를 둘 다 가지고 있다.
    - 행동에도 상태와 메소드가 들어있을 수 있다.
      - ex) 나는 행동에 행동의 속성(1분당 날개를 펄럭이는 횟수, 최고 높이, 속도 등)을 나타내는 인스턴스 변수를 집어넣을 수도 있다.

> **디자인 원칙**  
> 구현이 아닌 인터페이스에 맞춰서 프로그래밍한다.

- 실제 실행시에 쓰이는 객체가 코드에 의해 고정되지 않도록, 어떤 상위 형식에 맞춰서 프로그래밍함으로써 다형성을 활용해야 한다는 것이다.
- 변수를 선언할 때는 보통 추상 클래스나 인터페이스 같은 상위 형식으로 선언해야 한다.

  - 상위 클래스를 구현한 형식이면 어떤 객체든 집어넣을 수 있다.
  - 해당 변수를 선언하는 클래스에서는 실제 객체의 형식을 몰라도 된다.

- [1] 구현에 맞춰서 프로그래밍

```java
Dog d = new Dog();
d.bark();
```

- [2] 인터페이스/상위 형식에 맞춰서 프로그래밍

```java
Animal animal = new Dog();
animal.makeSound();
```

- [3] 구체적으로 구현된 객체를 실행시에 대입

```java
a = getAnimal();
a.makeSound();
```

<br>
![행동 인터페이스, 클래스](/assets/images/strategy_pattern_4.png)

#### 바뀌는 부분과 그렇지 않은 부분 분리했을 때, 장점

- 다른 형식의 객체에서도 나는 행동과 꽥꽥거리는 행동을 재사용할 수 있다.
- 새로운 행동을 쉽게 추가할 수 있다.

  - 기존의 행동 클래스를 수정하지 않음
  - Duck 클래스를 전혀 건드리지 않음

### Duck 행동 통합하기

- Duck 클래스에 인터페이스 형식의 인스턴스 변수를 추가한다.

  - 이때, Duck 클래스 내에서 정의한 fly(), quack() 메소드는 삭제한다.  
     ![행동 인터페이스가 추가된 Duck 클래스](/assets/images/strategy_pattern_5.png)

- performQuack()을 구현하자.
  - 행동 인터페이스 QuackBehavior에 의해 참조되는 객체에서 꽥꽥거리는 행동을 하도록 한다.
  - 객체의 종류에는 신경 쓸 필요가 없다.

```java
public class Duck {
    FlyBehavior flyBehavior;
    QuackBehavior quackBehavior;

    public void performFly() {
        flyBehavior.fly();
    }
    public void performQuack() {
        quackBehavior.quack(); // 행동 클래스에 위임한다.
    }
}
```

- Duck의 서브클래스의 인스턴스가 생성될 때, 생성자에서는 행동 클래스의 인스턴스를 인스턴스 변수에 대입한다.
  - ex) flyBehavior 인스턴스 변수에 FlyWithWings 형식의 인스턴스를 만들어서 대입한다.

```java
public class MallardDuck extends Duck {
    public MallardDuck() {
        flyBehavior = new FlyWithWings();
        quackBehavior = new Quack();
    }

    public void display() {
        System.out.println('나는 MallardDuck입니다.');
    }
}
```

- setter 메소드를 통해 동적으로 행동을 지정한다.
  - 실행시 행동을 바꾸고 싶다면, 원하는 행동의 setter 메소드를 호출하면 된다.

```java
public class ModelDuck extends Duck {
    public ModelDuck() {
        flyBehavior = new FlyWithWings();
        quackBehavior = new Quack();
    }

    public void display() {
        System.out.println('나는 ModelDuck입니다.');
    }

    public void setFlyBehavior(FlyBehavior fb) {
        flyBehavior = fb;
    }

    public void setQuackBehavior(QuackBehavior qb) {
        quackBehavior = qb;
    }
}

public class MiniDuckSimulator {
    public static void main(String[] args) {
        Duck model = new ModelDuck();
        model.performFly(); // FlyWithWings 실행
        model.setFlyBehavior(new FlyNoWay());
        model.performFly(); // FlyNoWay 실행
    }
}
```

### 💁‍♀ A에는 B가 있다.

- 두 클래스를합치는 것 = **구성(Composition)**
  - Duck 클래스에서는 행동을 상속받는 대신, 올바른 행동 객체로 구성됨으로써 행동을 부여받게 된다.

> **디자인 원칙**  
> 상속보다는 구성을 활용한다.

- 구성을 이용해 시스템을 만들면 유연성을 크게 향상시킬 수 있다.
  - 알고리즘군을 별도의 클래스의 집합으로 캡슐화할 수 있도록 만들어준다.
  - 실행시에 행동을 바꿀 수 있게 해준다.

### 💪 조금 더 알아보기

1. 상속(Inheritance)

   - **is a 관계**
   - '~는 ~이다.' 성립
   - 수퍼클래스의 변수와 메소드를 서브클래스에서 호출할 수 있다.
   - 강한 결합을 가진다.
   - 수퍼클래스를 수정하면, 서브클래스에 영향을 준다.
   - 서브클래스를 수정하면, 수퍼클래스엔 영향을 주지 않는다.
   - 클래스를 확장해야 코드를 재사용할 수 있다.
   - Java에서 다중 상속을 지원하지 않는다.
   - 컴파일 타임에 정적으로 관계가 결정된다.
   - ex) 학생은 사람이다.

2. 구성(Composition)

   - **has a 관계**
   - '~는 ~를 가진다.' 성립
   - 여러 클래스를 하나로 합쳐서 만들기 때문에 조각 프로그래밍이 편해진다.
   - 수퍼클래스와 서브클래스는 서로 독립적이다.
   - 런타임에 쉽게 변경 가능하다. = 유연성을 가진다.
   - 코드를 확장하지 않고 코드를 재사용할 수 있다.
   - 캡슐화 향상된다.
   - ex) 학생은 책을 가지고 있다.

## Reference

- <https://m.blog.naver.com/PostView.nhn?blogId=lunatic918&logNo=156290730&proxyReferer=https%3A%2F%2Fwww.google.com%2F>
- <https://eskeptor.tistory.com/51>
- <https://code-examples.net/ko/q/249d38>
- <https://javacan.tistory.com/entry/OO-Basic-4-Composition-Over-Inheritance>
