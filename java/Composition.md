## Inheritance
```java
class Animal {
    void eat() {
        System.out.println("Animal is eating");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("Dog is barking");
    }
}

Dog myDog = new Dog();
myDog.eat(); // Inherited from Animal
myDog.bark(); // Defined in Dog
```
- `is - a`관계 : Dog는 Animal이다
## Composition
- `has - a`관계: Car는 Engine을 가지고 있다
```java
class Engine {
    void start() {
        System.out.println("Engine started");
    }
}

class Car {
    private Engine engine;
    
    Car() {
        this.engine = new Engine();
    }
    
    void startCar() {
        engine.start();
    }
}

Car myCar = new Car();
myCar.startCar(); // Delegates to Engine's start() method
```
- 다중 상속 지원
```java
class Flying {
    void fly() {
        System.out.println("Flying");
    }
}

class Swimming {
    void swim() {
        System.out.println("Swimming");
    }
}

class Duck {
    private Flying flyingBehavior = new Flying();
    private Swimming swimmingBehavior = new Swimming();
    
    void performFly() {
        flyingBehavior.fly();
    }
    
    void performSwim() {
        swimmingBehavior.swim();
    }
}
```