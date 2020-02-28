---
published: true
layout: single
title: "객체지향의 사실과 오해 7장"
category: book
tags: [The Essence of Object-Orientation]
comments: false
---

# 7장 함께 모으기

#### 개념 관점(Conceptual Perspective)

- 설계는 도메인 안에 존재하는 개념과 개념들 사이의 관계를 표현한다.
- 사용자가 도메인을 바라보는 관점을 반영한다.
- 실제 도메인의 규칙과 제약을 최대한 유사하게 반영하는 것이 핵심이다.

#### 명세 관점(Specification PErspective)

- 객체의 인터페이스를 바라보게 된다. = 실제로 소프트웨어 안에서 살아 숨쉬는 객체들의 책임에 초점을 맞추게 된다.
- 프로그래머는 객체가 협력을 위해 '무엇'을 할 수 있는가에 초점을 맞춘다.
- "구현이 아니라 인터페이스에 대해 프로그래밍하라"를 따르는 것은 명세 관점과 구현 관점을 명확하게 분리하는 것에서부터 시작된다.

#### 구현 관점(Implementation Perspective)

- 실제 작업을 수행하는 코드와 연관되어 있다.
- 객체들이 책임을 수행하는 데 필요한 동작하는 코드를 작성하는 것에 초점을 맞춘다.
- 프로그래머는 객체의 책임을 '어떻게' 수행할 것인가에 초점을 맞춘다.

클래스가 은유하는 개념은 도메인 관점을 반영한다. 클래스의 공용 인터페이스는 명세 관점을 반영한다. 클래스의 속성과 메서드는 구현 관점을 반영한다.

:bulb: 클래스는 세가지 관점을 모두 수용할 수 있도록 개념, 인터페이스, 구현을 함께 드러내야 한다. 동시에 코드 안에서 세 가지 관점을 쉽게 식별할 수 있도록 깔끔하게 분리해야 한다.

## 커피 전문점 도메인

### 커피 전문점이라는 세상

![객체로 구성된 커피 전문점 세상](/assets/images/coffee_store_object.PNG)

상태와 무관하게 동일하게 행동할 수 있는 객체들을 동일한 타입으로 분류해 복잡성을 낮추자.

손님 객체는 '손님 타입'의 인스턴스로 볼 수 있다. 바리스타 객체는 '바리스타 타입' 인스턴스로 볼 수 있다. 아메리카노 커피, 에스프레소 커피, 카푸치노 커피는 모두 '커피 타입'의 인스턴스로 볼 수 있다. 메뉴판 객체는 '메뉴판 타입'의 인스턴스다. 메뉴판 객체는 아메리카노, 에스프레소, 카푸치노라는 메뉴 항목 객체를 포함할 수 있으며, 이때 메뉴 항목 객체는 '메뉴 항목 타입'의 인스턴스다.

![커피 전문점의 도메인 모델](/assets/images/coffee_store_domain_model.PNG)

커피 제조와 관련된 객체들을 타입과 관계를 이용해 추상화한 일종의 모델로, 소프트웨어가 대상으로 하는 영역인 도메인을 단순화해서 표현한 모델 즉, **도메인 모델**이다.

메뉴판 타입에서 메뉴 항목 쪽으로 향하는 선에 그려진 속이 찬 마름모는 **포함(containment) 관계** 또는 **합성(composition) 관계**를 나타낸다. 즉, 메뉴 항목이 메뉴판에 포함된다는 사실을 표현한다. 메뉴 항목 좌측 아래 숫자는 메뉴판에 포함되는 메뉴 항목의 갯수를 의미한다.

손님 타입은 메뉴판 타입을 알고 있어야 원하는 커피를 선택할 수 있다. 하지만, 메뉴판 타입은 손님의 일부가 아니므로 합성 관계가 아니다. 이처럼 한 타입의 인스턴스가 다른 타입의 인스턴스를 포함하지는 않지만 서로 알고 있어야 할 경우 **연관(association) 관계**라고 한다. 바리스타 타입은 커피를 제조해야하므로 커피 타입을 알고 있어야하지만, 메뉴판 타입과 커피 타입 중 어떤 것도 바리스타의 일부가 아니므로 연관 관계이다.

## 설계하고 구현하기

### 커피를 주문하기 위한 협력 찾기

1. 메시지를 먼저 선택한다.
2. 메시지를 수신하기에 적절한 객체를 선택해야 한다.
   - 이때, 도메인 모델 안에 책임을 수행하기에 적절한 타입이 존재하는지 살펴봐야한다.
3. 해당 객체가 스스로 할 수 없는 일이 있는지 찾고 다른 객체에게 이를 요청한다.

![커피 주문을 위한 객체 협력](/assets/images/coffee_order_collaboartion.PNG)

협력에 필요한 객체의 종류와 책임, 주고받아야 하는 메시지에 대한 대략적인 윤곽이 잡혔다.

### 인터페이스 정리하기

객체가 수신한 메시지가 객체의 인터페이스를 결정한다. 객체가 어떤 메시지를 수신할 수 있다는 것은 그 객체의 인터페이스 안에 메시지에 해당하는 오퍼레이션이 존재한다는 것을 의미한다.

객체의 타입을 구현하는 일반적인 방법은 클래스를 이용하는 것이다. 인터페이스에 포함된 오퍼레이션은 외부에서 접근 가능하도록 공용(public)으로 선언되어 있어야 한다.

```java
class Customer {
    public void order(String menuName) {}
}

class MenuItem {
}

class Menu {
    public MenuItem choose(String name) {}
}

class Barista {
    public Coffee makeCoffee(MenuItem menuItem) {}
}

class Coffee {
    public Coffee(MenuItem menuItem) {}
}
```

### 구현하기

오퍼레이션을 수행하는 방법을 메서드로 구현한다.

```java
class Customer {
    public void order(String menuName, Menu menu, Barista barista) {
        MenuItem menuItem = menu.choose(menuName);
        Coffee coffee = barista.makeCoffee(menuItem);
        ...
    }
}
```

Customer 객체는 협력하기 위해 Meu 객체와 Barista 객체에 대한 참조를 알고 있어야 한다. 객체 참조를 얻는 다양한 방법이 있지만 여기서는 인자로 Menu와 Barista 객체를 전달받는 방법으로 해결한다.

여기서 구현 도중에 객체의 인터페이스가 변경될 수 있다. 설계 작업은 구현을 위한 스케치를 작성하는 단계지 구현 그 자체일 수는 없다. 협력을 구상하는 단계에 너무 오랜 시간을 쏟지 말고 최대한 빨리 코드를 구현해서 설계에 이상이 없는지, 설계가 구현 가능한지를 판단해야 한다.

```java
class Menu {
    private List<MenuItem> items;

    public Menu(List<MenuItem> items) {
        this.items = items;
    }

    public MenuItem choose(String name) {
        for(MenuItem each : items) {
            if (each.getName().equals(name)) {
                return each;
            }
        }
    }
}
```

인터페이스를 결정할 때는 가급적 객체 내부의 구현에 대한 어떤 가정도 하지 말아야 한다. 즉, 객체가 어떤 속성을 가지는지, 그 속성이 어떤 자료 구조로 구현되었는지를 고려하지 않는다. 객체가 어떤 책임을 수행해야 하는지를 결정한 후에야 책임을 수행하는데 필요한 객체의 속성을 결정해야 한다. 이는 구현 세부 사항을 객체의 공용 인터페이스에 노출시키지 않고 인터페이스와 구현을 깔끔하게 분리할 수 있는 기본적인 방법이다.

```java
class Barista {
    public Coffee makeCoffee(MenuItem menuItem) {
        Coffee coffee = new Coffee(menuItem);
        return coffee;
    }
}

class Coffee {
    private String name;
    private int price;

    public Coffee(MenuItem menuItem) {
        this.name = menuItem.getName();
        this.price = menuItem.cost();
    }
}

class MenuItem {
    private String name;
    private int price;

    public MenuItem(String name, int price) {
        this.name = name;
        this.price = price;
    }

    public int cost() {
        return price;
    }

    public String getName() {
        return name;
    }
}
```

![커피 전문점을 구현한 최종 클래스 구조](/assets/images/coffee_store_final_class_structure.PNG)

## 코드와 세 가지 관점

### 코드는 세 가지 관점을 모두 제공해야 한다

1. 개념 관점
   - 도메인을 구성하는 중요한 개념과 관계를 반영한다.
   - 클래스가 도메인 개념의 특성을 최대한 수용하면 변경을 관리하기 쉽고 유지보수성을 향상시킬 수 있다.
     - 변경을 위해 찾아야하는 코드의 양도 줄어든다.
2. 명세 관점
   - 클래스의 인터페이스를 바라본다.
   - 클래스의 public 메서드는 다른 클래스가 협력할 수 있는 공용 인터페이스를 드러낸다.
     - 공용 인터페이스는 외부의 객체가 해당 객체에 접근할 수 있는 유일한 부분이다.
     - 최대한 변화에 안정적인 인터페이스를 만들기 위해서는 인터페이스를 통해 구현과 관련된 세부사항이 드러나지 않게 해야 한다.
3. 구현 관점
   - 클래스의 내부 구현을 바라본다.
   - 메서드와 속성은 철저하게 클래스 내부로 캡슐화되어야 한다.
     - 메서드의 구현과 속성의 변경은 외부의 객체에게 영향을 미쳐서는 안 된다.

세 가지 관점을 모두 포함하면서도 각 관점에 대응되는 요소를 명확하고 깔끔하기 드러내라. 이는 변경에 유연하게 대응할 수 있는 객체지향 코드를 작성하는 가장 빠른 길이다.

### 도메인 개념을 참조하는 이유

어떤 메시지가 있을 때 그 메시지를 수신할 객체를 선택하는 방법 중 하나는 도메인 개념 중에서 가장 적절한 것을 선택하는 것이다. 이는 도메인에 대한 지식을 기반으로 코드의 구조와 의미를쉽게 유추할 수 있기 때문에 유지보수성을 향상시킨다. 즉, 변화에 쉽게 대응할 수 있다.

### 인터페이스와 구현을 분리하라

명세 관점은 클래스의 안정적인 측면을 드러내야 한다. 구현 관점은 클래스의 불안정한 측면을 드러내야 한다. 이 두 가지 관점을 분리하는것은 매우 중요하다.
캡슐화를 위반해서 구현을 인터페이스 밖으로 노출해서도 안 되고, 인터페이스와 구현을 명확하게 분리하지 않고 흐릿하게 섞어놓아서도 안된다.
