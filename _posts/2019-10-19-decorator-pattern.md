---
published: true
layout: single
title: "Decorator Pattern"
category: book
tags: [Head First, design pattern, decorator pattern]
comments: false
---

> **데코레이터 패턴(Decorator Pattern)**  
> 객체에 추가적인 요건을 동적으로 첨가한다.  
> 데코레이터는 서브클래스를 만드는 것을 통해서 기능을 유연하게 확장할 수 있는 방법을 제공한다.

### OCP(Open-Closed Principle)

#### 상속과 구성

1. 상속
   - 서브클래스를 만드는 방식으로 행동을 상속받으면, 그 행동은 컴파일시에 완전히 결정된다.
   - 모든 서브클래스에서 똑같은 행동을 상속 받아야 한다.
   - 새로운 기능을 추가하기 위해 기존 코드를 건드리게 된다.
     - 기존 코드에서 버그가 생기거나 의도하지 않은 부작용이 발생한다.
2. 구성과 위임
   - 실행중에 동적으로 행동을 설정할 수 있다.
   - 객체에 여러 임무를 새로 더할 수 있다.
     - 수퍼클래스를 디자인했던 사람이 전혀 생각하지 못했던 것을 추가할 수 있다.
   - 기존 코드를 건드리지 않고 새로운 코드를 만들어서 새로운 기능을 추가할 수 있다.

> **디자인 원칙**  
> 클래스는 확장에 대해서는 열려 있어야 하지만 코드 변경에 대해서는 닫혀 있어야 한다.

:bulb: 기존 코드를 건드리지 않은 채로 확장을 통해서 새로운 행동을 간단하게 추가할 수 있도록 한다. 이는 새로운 기능을 추가하는 데 있어서 매우 유연해서 급변하는 주변 환경에 잘 적응할 수 있으면서도 강하고 튼튼한 디자인을 만들 수 있다.

### 데코레이터 패턴

- 데코레이터는 데코레이터가 감싸고 있는 객체에 행동을 추가하기 위한 용도로 만들어진 것이다.
- 데코레이터의 수퍼클래스는 자신이 장식하고 있는 객체의 수퍼클래스와 같다.
- 한 객체를 여러 개의 데코레이터로 감쌀 수 있다.
- 데코레이터는 자신이 감싸고 있는 객체와 같은 수퍼클래스를 가지고 있기 때문에 원래 객체(싸여져 있는 객체)가 들어갈 자리에 데코레이터 객체를 집어넣어도 상관 없다.
- **데코레이터는 자신이 장식하고 있는 객체에게 어떤 행동을 위임하는 것 외에 원하는 추가적인 작업을 수행할 수 있다.**
- 겍체는 언제든지 감쌀 수 있기 때문에 실행중에 필요한 데코레이터를 마음대로 적용할 수 있다.
- 추상 구성요소 형식을 바탕으로 돌아가는 코드에 대해서 데코레이터 패턴을 적용해야만 제대로 된 결과를 얻을 수 있다.

![decorator pattern 구조](/assets/images/decorator_pattern_1.JPG)

#### [예시] 스타벅스 커피

![스타벅스 커피](/assets/images/decorator_pattern_2.JPG)

```java
public abstract class Beverage {
    String description = "제목 없음";

    public String getDescription() {
        return description;
    }

    public abstract double cost();
}

public abstract class CondimentDecorator extends Beverage {
    public abstract String getDescription();
}

public class HouseBlend extends Beverage {
    public HouseBlend() {
        description = "하우스 블랜드 커피";
    }

    public double cost() {
        return .89;
    }
}

public class Mocha extends CondimentDecorator {
    Beverage beverage;

    public Mocha(Beverage beverage) {
        this.beverage = beverage;
    }

    public String getDescription() {
        return beverage.getDescription() + ", 모카";
    }

    public double cost() {
        return .20 + beverage.cost();
    }
}

public class StarbucksCoffee {
    public static void main(String args[]) {
        Beverage beverage = new HouseBlend();
        beverage = new Soy(beverage);
        beverage = new Mocha(beverage);
        beverage = new Whip(beverage);
        System.out.println(beverage.getDescription() + " $" + beverage.cost());
    }
}
```

### 🤷‍ 데코레이터 패턴은 상속을 이용한 것 아닌가?

- 상속을 이용해서 형식을 맞추는 용도로 사용한다.
  - 데코레이터의 형식이 데코레이터로 감싸는 형식과 같다는 점이 포인트!
- 새로운 행동을 얻는 것은 수퍼클래스가 아니라 객체들을 구성하는 방법을 통해서 얻게 되는 것이다.

### 데코레이터 패턴의 단점

1. 클래스들이 많아진다.
   - 남들이 봤을 때 이해하기 힘든 디자인이 만들어지곤 한다.
2. 특정 형식에 의존하는 클라이언트 코드에 데코레이터 패턴을 적용하면 안된다.
   - 데코레이터를 끼워넣어도 클라이언트 쪽에서는 데코레이터를 사용하고 있다는 것을 전혀 알 수 없다.
3. 구성 요소를 초기화하는데 필요한 코드가 훨씬 복잡해진다.
   - 꽤 많은 데코레이터로 감싸야 하는 경우가 종종 있기 때문이다.
