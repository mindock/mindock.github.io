---
published: true
layout: single
title: "Factory Pattern"
category: book
tags: [Head First, design pattern, factory pattern]
comments: false
---

> **팩토리 패턴(Factory Pattern)**  
> 팩토리 메소드 패턴과 추상 팩토리 패턴이 존재한다.

```java
Pizza orderPizza(String type) {
    Pizza pizza;

    if (type.equals("cheese")) {
        pizza = new CheesePizza();
    } else if (type.equals("greek")) {
        pizza = new GreekPizza();
    } else if (type.equals("pepperoni")) {
        pizza = new PepperoniPizza();
    }

    pizza.prepare();
    pizza.bake();
    pizza.cut();
    pizza.box();
    return pizza;
}
```

만약, 야채피자(Veggie Pizza)가 추가되고 그리스식 피자(Greek Pizza)가 제외된다면?

```java
Pizza orderPizza(String type) {
    Pizza pizza;

    if (type.equals("cheese")) {
        pizza = new CheesePizza();
    } else if (type.equals("pepperoni")) {
        pizza = new PepperoniPizza();
    } else if (type.equals("veggie")) {
        pizza = new VeggiePizza();
    }

    pizza.prepare();
    pizza.bake();
    pizza.cut();
    pizza.box();
    return pizza;
}
```

위 코드에서 바뀌는 부분과 바뀌지 않는 부분을 분리할 수 있다.

바뀌는 부분은 피자 종류가 바뀔 때마다 코드를 고쳐야한다. 코드 변경에 대해 닫혀있지 않다.

```java
if (type.equals("cheese")) {
    pizza = new CheesePizza();
} else if (type.equals("pepperoni")) {
    pizza = new PepperoniPizza();
} else if (type.equals("veggie")) {
    pizza = new VeggiePizza();
}
```

해당 부분을 orderPizza() 메소드에서 뽑아내 객체 생성 코드만 만드는 일을 전담하는 객체 **Factory**를 생성한다.
이와 같이 분리하면, orderPizza() 메소드에서는 어떤 피자를 만들어야 하는지 고민하지 않아도 된다.  
또한, SimplePizzaFactory는 구상 Pizza 클래스를 직접 참조하는 부분이다.

```java
public class SimplePizzaFactory {
    public Pizza createPizza(String type) {
        Pizza pizza = null;

         if (type.equals("cheese")) {
            pizza = new CheesePizza();
        } else if (type.equals("pepperoni")) {
            pizza = new PepperoniPizza();
        } else if (type.equals("veggie")) {
            pizza = new VeggiePizza();
        }
        return pizza;
    }
}
```

```java
public class PizzaStore {
    SimplePizzaFactory factory;

    public PizzaStore(SimplePizzaFactory factory) {
        this.factory = factory;
    }

    public Pizza orderPizza(String type) {
        Pizza pizza;

        pizza = factory.createPizza(type);

        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();
        return pizza;
    }
}
```

### 🤷‍ orderPizza에서 발생하던 문제를 SimplePizzaFactory에 넘겨 버린거 아닌가요?

- SimplePizzaFactory를 사용하는 클라이언트가 매우 많을 수 있다는 점을 생각해야한다.
- 피자를 생성하는 작업을 한 클래스에 캡슐화시켜 놓으면 구현을 변경해야하는 경우 팩토리 클래스 하나만 고치면 된다.

### 💁‍♀ 정적 팩토리의 장점과 단점?

- 여기서 정적 팩토리는 정적(static) 메소드로 정의하는 기법을 의미한다.
- 객체를 생성하기 위한 메소드를 실행시키기 위해서 객체의 인스턴스를 만들지 않아도 된다.
- 서브클래스를 만들어 객체 생성 메소드의 행동을 변경시킬 수 없다.

### :bulb: 디자인 패턴에서 "인터페이스를 구현한다" 라는 표현은?

- 항상 클래스 선언 부분에서 `implements` 키워드를 써서 인터페이스를 구현하는 클래스를 만드는 것이 아니다.
- 어떤 상위 형식(클래스/인터페이스)에 있는 메소드를 구현하는 구상 클래스

### 🤷‍♀️ 만약 프랜차이즈가 생기고 지점마다 조금씩 다른 차이점을 가진다면?

```java
NYPizzaFactory nyFactory = new NYPizzaFactory();
PizzaStore nyStore = new PizzaStore(nyFactory);
nyStore.order("Veggie");

ChicagoPizzaFactory chicagoFactory = new ChicagoPizzaFactory();
PizzaStore chicagoStore = new PizzaStore(chicagoFactory);
chicagoFactory.order("Veggie");
```

위와 같이 서로 다른 팩토리를 만들어서 사용하면 지점마다 다르게 만들 수 있다. 하지만, 독자적인 방법들을 사용하기 시작한다면?

### 🙋‍♀️ 피자 가게와 피자 제작 과정 전체를 하나로 묶어주는 프레임워크를 만들자

피자를 만드는 활동 자체는 전부 PizzaStore 클래스에 국한시키면서도 분점마다 고유의 스타일을 살릴 수 있도록 하는 방법이 있다.

```java
public abstract class PizzaStore {
    public Pizza orderPizza(String type) {
        Pizza pizza;

        pizza = createPizza(type); // PizzaStore에 있는 createPizza를 호출한다.

        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();

        return pizza;
    }

    abstract Pizza createPizza(String type);
}
```

타입에 따라 피자를 만드는 메소드를 **추상 메소드**로 선언하고, 각 지역마다 고유의 스타일에 맞게 PizzaStore의 서브클래스를 만들도록 하는 방법이다.

여기서 모든 분점에서 orderPizza() 메소드를 통해 주문은 똑같이 진행한다.  
만약, orderPizza() 메소드를 서브 클래스에서 고쳐서 쓸 수 없게 하고 싶다면, `final`로 선언할 수도 있다.

PizzaStore의 orderPizza() 메소드에서 여러 가지 작업을 하지만, Pizza는 추상 클래스이기 때문에 실제로 어떤 구상 클래스에서 작업이 처리되고 있는지 전혀 알 수 없다. 즉, PizzaStore와 Pizza는 서로 완전히 분리되어 있다.

우리가 선택하는 PizzaStore의 서브클래스 종류에 따라 결정되는 것이지만, 만들어지는 피자의 종류를 해당 서브클래스에서 결정한다고 할 수도 있다.

```java
public abstract class Pizza {
    String name;
    String dough;
    String sauce;
    ArrayList toppings = new ArrayList();

    void prepare() {
        System.out.println("Preparing " + name);
        System.out.println("Tossing dough...");
        System.out.println("Adding sauce...");
        System.out.println("Adding toppings: ");
        for (int i = 0; i < toppings.size(); i++) {
            System.out.println(" " + toppings.get(i));
        }
    }

    void bake() {
        System.out.println("Bake for 25 minutes at 350");
    }

    void cut() {
        System.out.println("Cutting the pizza into diagonal slices");
    }

    void box() {
        System.out.println("Place pizza in official PizzaStore box");
    }

    public String getName() {
        return name;
    }
}

public class ChicagoStyleCheesePizza extends Pizza {
    public ChicagoStyleCheesePizza() {
        name = "Chicago Style Deep Dish Cheese Pizza";
        dough = "Extra Thick Crust Dough";
        sauce = "Plum Tomato Sauce";

        toppings.add("Shredded Mozzarella Cheese");
    }

    void cut() {
        System.out.println("Cutting the pizza into square slices");
    }
}
```

### 팩토리 메소드 선언

```java
abstract Product factoryMethod(String type)
```

- 팩토리 메소드를 이용하면, 객체를 생성하는 작업을 서브클래스에 캡슐화시킬 수 있다.
- 수퍼클래스에 있는 클라이언트 코드와 서브클래스에 있는 객체 생성 코드를 분리시킬 수 있다.
- 추상 메소드로 선언하여, 서브클래스에서 객체 생성을 책임지도록 한다.
- 특정 제품을 리턴하며, 그 객체는 보통 수퍼클래스에서 정의한 메소드 내에서 쓰이게 된다.
- 클라이언트(ex. orderPizza 메소드)에서 실제로 생성되는 구상객체가 무엇인지 알 수 없게 만드는 역할도 한다.

### 팩토리 메소드 패턴 정의

#### 생산자(Creator) 클래스

![생산자 클래스](/assets/images/factory_pattern_1.JPG)

- 추상 생산자 클래스는 나중에 서브클래스에서 제품을 생산하기 위해 구현할 팩토리 메소드를 정의한다.
- 제품을 가지고 원하는 일을 하기 위한 모든 메소드들이 구현되어 있다.

#### 제품(Product) 클래스

![제품 클래스](/assets/images/factory_pattern_2.JPG)

- 팩토리에서는 제품을 생산한다.
- 모두 똑같은 인터페이스를 구현해야 한다.
  - 제품을 사용하는 클래스에서 구상 클래스가 아닌 인터페이스에 대한 레퍼런스를 써서 객체를 참조하기 때문이다.

> **팩토리 메소드 패턴**  
> 객체를 생성하기 위한 인터페이스를 정의하는데, 어떤 클래스의 인스턴스를 만들지는 서브클래스에서 결정하게 만든다.

#### 장점

1. 객체 생성 코드를 전부 한 객체 또는 메소드에 집어넣는다.
   - 중복된 내용을 제거할 수 있다.
   - 나중에 관리할 때도 한 군데에만 신경을 쓰면 된다.
2. 클라이언트 입장에서는 객체 인스턴스를 만들 때 인터페이스만 필요로 하게 된다.
   - 유연성과 확장성이 뛰어난 코드를 만들 수 있다.

### 💁‍♀ 간단한 팩토리와 팩토리 메소드 패턴의 차이점?

#### 간단한 팩토리

- 일회용 처방에 불과하다.
- 객체 생성을 캡슐화하는 방법을 사용하긴 한다.
- 강력한 유연성을 제공하진 못한다.

#### 팩토리 메소드 패턴

- 어떤 구현을 사용할지를 서브클래스에서 결정하는 프레임워크를 만들 수 있다.
- 강력한 유연성을 제공한다.

### 의존성 뒤집기 원칙(Dependency Inversion Principle)

구상 클래스에 대한 의존성을 줄이는게 좋다.

> **디자인 원칙**  
> 추상화된 것에 의존하도록 만들어라.  
> 구상 클래스에 의존하도록 만들지 않도록 한다.

고수준 구성요소가 저수준 구성요소에 의존하면 안된다는 것이 내포되어 있다. 이때, 고수준 구성요소는 다른 저수준 구성요소에 의해 정의되는 행동이 들어있는 구성요소를 뜻한다.

![의존성이 심한 PizzaStore](/assets/images/factory_pattern_3.JPG)

- 피자 종류를 새로 추가하면, 더 많은 피자 객체가 PizzaStore에 의존하게 된다.
- 모든 피자 객체들에게 직접적으로 의존하게 된다.
- 피자 클래스들의 구현이 변경되면, PizzaStore까지 고쳐야 할 수도 있다.

![의존성 뒤집기 원칙 적용](/assets/images/factory_pattern_4.JPG)

- 고수준 구성요소인 PizzaStore과 저수준 구성요소인 피자 객체들이 모두 추상 클래스인 Pizza에 의존하게 된다.

#### 원칙을 지키기 위한 가이드라인

- 어떤 변수에도 구상 클래스에 대한 레퍼런스를 저장하지 않는다.
- 구상 클래스에서 유도된 클래스를 만들지 않는다.
  - 구상 클래스에서 유도된 클래스를 만들면 특정 구상 클래스에 의존하게 된다.
- 베이스 클래스에 이미 구현되어 있던 메소드를 오버라이드하지 않는다.
  - 오버라이드한다는 것은 애초부터 베이스 클래스가 제대로 추상화된 것이 아니었다고 볼 수 있다.

### 🤷‍ 분점에서 좋은 재료를 사용하도록 관리할 수 있는 방법은?

원재료를 생산하는 공장을 만들고 분점까지 재료를 배달하도록 한다.

```java
public interface PizzaIngredientFactory {
    public Dough createDough();
    public Sauce createSauce();
    public Cheese createCheese();
    public Veggies[] createVeggies();
    public Pepperoni createPepperoni();
    public Clams createClam();
}

public class NYPizzaIngredientFactory implements PizzaIngredientFactory {
    public Dough createDough() {
        return new ThinCrustDough();
    }

    public Sauce createSauce() {
        return new MarinaraSauce();
    }

    public Cheese createCheese() {
        return new ReggianoCheese();
    }

    public Veggies[] createVeggies() {
        Veggies veggies[] = [new Gralic(), new Onion(), new Mushroom(), new RedPepper()];
        return veggies;
    }

    public Pepperoni createPepperoni() {
        return new SlicedPepperoni();
    }

    public Clams createClam() {
        return new FreshClams();
    }
}

public abstract class Pizza {
    String name;
    Dough dough;
    Sauce sauce;
    Veggies veggies[];
    Cheese cheese;
    Pepperoni pepperoni;
    Clams clam;

    abstract void prepare();

    void bake() {
        System.out.println("Bake for 25 minutes at 350");
    }

    void cut() {
        System.out.println("Cutting the pizza into diagonal slices");
    }

    void box() {
        System.out.println("Place pizza in official PizzaStore box");
    }

    void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}

public class CheesePizza extends Pizza {
    PizzaIngredientFactory ingredientFactory;

    public CheesePizza(PizzaIngredientFactory ingredientFactory) {
        this.ingredientFactory = ingredientFactory;
    }

    void prepare() {
        System.out.println("Preparing " + name);
        dough = ingredientFactory.createDough();
        sauce = ingredientFactory.createSauce();
        cheese = ingredientFactory.createCheese();
    }
}
```

팩토리 메소드 패턴을 이용한 코드를 만들었을 때는 NYCheesePizza와 ChicagoCheesePizza 클래스를 만들었다. 하지만, 두 개의 차이점은 지역별로 다른 재료를 사용한다는 것이다.  
그래서 피자마다 클래스를 지역별로 만들 필요가 없다. 이는 원재료 공장을 이용해 해결할 수 있다.

```java
public class NYPizzaStore extends PizzaStore {
    protected Pizza createPizza(String item) {
        Pizza pizza = null;
        PizzaIngredientFactory ingredientFactory = new NYPizzaIngredientFactory();

        if (item.equals("cheese")) {
            pizza = new CheesePizza(ingredientFactory);
            pizza.setNmae("New York Style Cheese Pizza");
        } else if (item.equals("veggie")) {
            pizza = new VeggiePizza(ingredientFactory);
            pizza.setNmae("New York Style Veggie Pizza");
        } else if (item.equals("clam")) {
            pizza = new ClamPizza(ingredientFactory);
            pizza.setNmae("New York Style Clam Pizza");
        } else if (item.equals("pepperoni")) {
            pizza = new PepperoniPizza(ingredientFactory);
            pizza.setNmae("New York Style Pepperoni Pizza");
        }
        return pizza;
    }
}
```

#### 추상 팩토리(Abstract Factory) 패턴 정의

- 제품군을 생성하기 위한 인터페이스를 제공할 수 있다.
- 이 인터페이스를 이용하는 코드를 만들면 코드를 제품을 생산하는 실제 팩토리와 분리시킬 수 있다.
- 서로 다른 상황별로 적당한 제품을 생산할 수 있다.
- 클라이언트 코드는 전혀 바뀔 필요가 없다.
- 클라이언트는 어떤 제품이 생산되는지 알 필요가 없다.
  - 클라이언트와 팩토리에서 생산되는 제품을 분리시킬 수 있다.

> **추상 팩토리 패턴**  
> 인터페이스를 이용하여 서로 연관된, 또는 의존하는 객체를 구상 클래스를 지정하지 않고도 생성할 수 있다.

![추상 팩토리 패턴](/assets/images/factory_pattern_5.JPG)

### 🤷‍추상 팩토리 패턴 뒤에는 팩토리 메소드 패턴이 숨어있는 건가요?

- 추상 팩토리 패턴에서 메소드가 팩토리 메소드로 구현되는 경우도 종종 있따.
- 추상 팩토리가 일련의 제품들을 생성하는데 쓰일 인터페이스를 정의하기 위해 만들어진 것이다.
- 그 인터페이스에 있는 각 메소드는 구상 제품을 생산하는 일을 맡고 있다.
- 추상 팩토리의 서브클래스를 만들어 각 메소드의 구현을 제공한다.

### 💁‍ 팩토리 메소드 패턴과 추상 팩토리 패턴 비교

- **팩토리 메소드 패턴**

  - 상속을 통해 객체를 만든다.
    - 클래스를 확장하고 팩토리 메소드를 오버라이드해야한다.
  - 클라이언트와 구상 형식을 분리시켜주는 역할을 한다.
    - 클라이언트에서는 자신이 사용할 추상 형식만 알면 된다.
    - 구상 형식은 서브클래스에서 처리한다.
  - 한 가지 제품만 생산한다.
  - 어떤 구상 클래스를 필요로 하게 될지 미리 알 수 없는 경우에 유용하다.

- **추상 팩토리 패턴**

  - 구성을 통해 객체를 만든다.
  - 클라이언트와 구상 형식을 분리시켜주는 역할을 한다.
    - 제품군을 만들기 위한 추상 형식을 제공한다.
    - 제품이 생산되는 방법은 추상 형식의 서브클래스에서 정의된다.
  - 일련의 연관된 제품을 하나로 묶을 수 있다.
  - 제품군에 제품을 추가하는 등 제품들을 확대하는 경우에는 인터페이스를 바꿔야한다.
  - 구상 팩토리를 구현할 때 팩토리 메소드를 사용해 제품을 생산하는 경우가 종종 있다.
