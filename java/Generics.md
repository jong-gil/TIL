> 컴파일 시 타입 에러를 발견하는 기능

- 문법
	- angle bracket으로 표현됨 (e.g. `List<String>`)
	- 타입 매개변수: 보통 T, E, K 와 같은 하나의 대문자 문자로 표현됨
- 자주 쓰이는 경우
	- Collections: `List<T>`, `Map<K, V>`
	- 클래스: `class Box<T> {...}`
	- 메소드: `<T> void printArray(T[] arrary) {...}`
- 와일드 카드
	- `List<?>` 혹은 `List<? extends Number>`
#### Generic Class
```java
public class Box<T> {
    private T content;
    
    public void set(T content) {
        this.content = content;
    }
    
    public T get() {
        return content;
    }
}

Box<Integer> intBox = new Box<>();
intBox.set(10);
Integer content = intBox.get();
```

- 참조변수와 생성자에 대입된 타입이 일치해야 함
```java
Box<Apple> appleBox = new Box<Apple>();
/*
Apple이 Fruit을 상속해도 불가능함 (error)
**/
Box<Fruit> fruitBox = new Box<Apple>();
```
- 제네릭 클래스가 상속 관계인 것은 가능함
```java
Box<Fruit> fruitBox = new FruitBox<Fruit>();
List<Fruit> fruits = new ArrayList<Fruit>();

/*
대입되는 값이 같아야 함 (error)
**/
Box<Fruit> grapeBox = new FruitBox<Grape>();
```
#### Generic Method
```java
public <T> T foo(List<T> list) {}

public <T extends Fruit> T foo(List<T> list) {}
/*
매개변수에 직접 표시하지 않음 (error)
**/
public <T> T foo(List<T extends Fruit> list) {}
```

#### Wildcard
- 오류 코드
```java
/*
아무 타입의 list를 출력하는 메소드 -> error
'Object' 타입에 해당하는 타입만 들어올 수 있음
Object가 아닌 Fruit을 받고 있음
**/
public static void printList(List<Object> list) {
	for (Object elem : list) {
		System.out.println(elem + "")
	}
}
public static void main(String[] args) {
	List<Fruit> fruits = new ArrayList<>();
	MyClass.printList(fruits);
}

```
- 정상 코드
```java
public static void printList(List<? extends Object> list) {
	for (Object elem : list) {
		System.out.println(elem + "")
	}
}
public static void main(String[] args) {
	List<Fruit> fruits = new ArrayList<>();
	MyClass.printList(fruits);
}
```
	- Object를 상속받는 모든 타입이 들어올 수 있음

참고
[우아한테크코스 Generics - YouTube]()
