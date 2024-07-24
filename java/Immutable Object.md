> 객체 생성 이후 상태가 바뀌지 않는 객체
#### 장점
- 이해하기 쉽고 안정적 서비스 개발에 도움
- map, set, cache에 쓰기 적절
- 일반적으로 thread-safe
- 불변 객체를 필드로 쓰면 방어적 복사를 할 필요없음
-> 가능하다면 불변 객체를 적극적으로 쓰는 것이 필요함

#### Java의 String
```java
// 두 개의 변수는 같은 메모리 주소를 가리킴
String name = "hello";
String sameRef = name;

System.out.println(name == sameRef); // true

name = "hola"; // 새롭게 만들어진 문자열을 가리킴 (다른 메모리 주소)
System.out.println(name == sameRef); // false
```

#### Java 예시
```java
public final class Person {
	private final String name;  
	private final RGB rgb; // mutable
	  
	public Person(String name, RGB rgb) {  
		this.name = name;  
		this.rgb = new RGB(rgb.r, rgb.g, rgb.b);  
	}  
	  
	public String getName() {  
		return this.name;  
	}  
	  
	public RGB getRgb() {  
		return new RGB(rgb.r, rgb.g, rgb.b);  
	}
}
```
- 상태 변경 메소드 제거 (setter 등)
- 모든 필드 private final 지정
- 클래스 상속 금지 (class 앞에 `final` keyword)
- mutable 객체의 레퍼런스 공유 금지 (새로운 객체 반환, 방어적 복사)

```java
public final class Person {  
	private final String name;  
	private final List<RGB> rgbs; // mutable  
  
	public Person(String name, List<RGB> rgbs) {  
		this.name = name;  
		this.rgbs = copy(rgbs);  
	}  
	  
	public String getName() {  
		return this.name;  
	}  
	  
	public List<RGB> getRgbs() {  
		return copy(rgbs);  
	}  

	// deep copy
	private List<RGB> copy(List<RGB> rgbs) {  
		List<RGB> cps = new ArrayList<>();  
		rgbs.forEach(o -> cps.add(new RGB(o.r, o.g, o.b)));  
		return cps;  
	}  
}
```