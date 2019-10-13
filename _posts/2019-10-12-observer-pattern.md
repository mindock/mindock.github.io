---
published: true
layout: single
title: "Observer Pattern"
category: book
tags: [Head First, design pattern, observer pattern]
comments: false
---

> **옵저버 패턴(Observer Pattern)**  
> 한 객체의 상태가 바뀌면 그 객체에 의존하는 다른 객체들한테 연락이 가고 자동으로 내용이 갱신되는 방식  
> 일대다(one-to-many) 의존성을 정의한다.

### 옵저버 패턴의 정의

1. Subject(주제)
   - 상태를 저장하고 있는 객체
   - 데이터가 바뀌면 새로운 데이터 값이 어떤 방법으로든 옵저버들에게 전달된다.
2. Observer(옵저버)
   - 주제에 의존한다(주제 객체를 구독하고 있다).
   - 반드시 상태를 가지고 있어야 하는 것은 아니다.

![옵저버 패턴 클래스 다이어그램](/assets/images/observer_pattern_1.png)

### 느슨한 결합(Loose Coupling)의 위력

1. 주제가 옵저버에 대해서 아는 것은 옵저버가 특정 인터페이스(Observer 인터페이스)를 구현한다는 것 뿐이다.
   - 옵저버가 무엇을 하는지/구상 클래스가 무엇인지 등 알 필요가 없다.
2. 옵저버는 언제든지 새로 추가/제거할 수 있다.
3. 새로운 형식의 옵저버를 추가하려고 할 때도 주제를 전혀 변경할 필요가 없다.
   - 새로운 클래스에서 Observer 인터페이스를 구현하고 옵저버로 등록하면 된다.
4. 주제와 옵저버는 서로 독립적으로 재사용할 수 있다.
5. 주제나 옵저버가 바뀌더라도 서로한테 영향을 미치지는 않는다.
   - 주제 혹은 옵저버 인터페이스를 구현한다는 조건만 만족된다면, 어떻게 바꿔도 문제가 되지 않는다.

> **디자인 원칙**  
> 서로 상호작용을 하는 객체 사이에서는 가능하면 느슨하게 결합하는 디자인을 사용해야 한다.

:bulb: 객체 사이의 상호의존성을 최소화해 변경 사항이 생겨도 무난히 처리할 수 있는 유연한 객체지향 시스템을 구축할 수 있다.

### 옵저버에게 상태 정보를 전달하는 방법

1. PUSH 방식
   - 주제가 옵저버에게 정보를 보내는 방식
   - 옵저버들이 필요한 것을 모두 한번에 전달할 수 있다.
   - 옵저버 입장에서는 필요없는 상태의 변화에 대해서도 연락이 온다.
2. PULL 방식
   - 옵저버가 주제의 상태를 가져오는 방식
   - public getter를 이용해 필요한 상태를 가져올 수 있다.
     - 조금만 알아도 되는 옵저버가 있다면, 일부 상태만 가져가도 된다.
   - 필요한 상태를 모두 가져가기 위해 메소드를 여러번 호출해야한다.

### 🤷‍ 잘 이해되지 않은 부분?

```java
public class CurrentConditionsDisplay implements Observers, DisplayElement {
    private float temperature;
    private float humidity;
    private Subject weatherData;

    public CurrentConditionsDisplay(Subject weatherData) {
        this.weatherData = weatherData;
        weatherData.registerObserver(this);
    }

    public void update(float temperature, float humidity, float pressure) {
        this.temperature = temperature;
        this.humidity = humidity;
        display();
    }

    public void display() {
        System.out.println("Current conditions: " + temperature + "F degrees and " + humidity + "% humidity");
    }
}
```

Q1. 생성자에서 Subject를 넘겨 저장하는 이유?

- 나중에 옵저버 목록에서 탈퇴해야 한다면, 주제 객체에 대한 레퍼런스를 저장해 두면 유용하게 쓸 수 있을 것이다.
