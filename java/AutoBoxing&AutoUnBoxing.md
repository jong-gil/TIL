## Auto Boxing & Auto Unboxing

### 기본 타입과 Wrapper 클래스
- 기본 타입: `int, long, float, double, boolean`등
- Wrapper 클래스: `Integer, Long, Float, Double, Boolean` 등

### 박싱과 언박싱
- 박싱: 기본 타입 데이터 -> Wrapper 클래스
- 언박싱: Wrapper 클래스 -> 기본 타입

![Alt text](asset/boxing-unBoxing.png)

```java
// Boxing
int i = 10;
Integer num = new Integer(i);

// Unboxing
Integer num = new Integer(10);
int i = num.intValue();

// Auto Boxing
int i = 10;
Integer num = i;

// Auto Unboxing
Integer num = new Integer(10);
int i = num;
```

### 성능
오토 박싱과 언박싱은 내부적으로 추가 연산 작업 필요 <br/>
-> 오토 박싱&언박싱이 일어나지 않게 동일한 타입 연산이 이루어지도록 하는 것이 필요