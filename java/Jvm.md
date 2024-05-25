## 자바 가상 머신 (Java Virtual Machine)
시스템 메모리를 관리하면서 자바 기반 어플리케이션을 위해 이식 가능한 실행 환경을 제공<br/>
-> 기기 혹은 운영체제 종류에 영향 받지 않고 실행 가능하게 함<br/>
-> 프로그램 메모르를 관리하고 최적화

```
JVM에서 메모리 관리: 힙과 스택의 메모리 사용 확인
```
### JVM 실행 과정
1. 프로그램 실행시 JVM은 OS로부터 필요한 메모리 할당받음<br/>
    -> JVM은 메모리를 용도에 따라 여러 영역으로 나누어 관리
2. JAVAC (자바 컴파일러)가 자바 소스코드 읽고 .class (자바 바이트코드)로 변환시킴
3. 변경된 class 파일을 클래스 로더를 통해 JVM 메모리 영역으로 로딩
4. 로딩된 class 파일은 execution engine을 통해 해석
5. 해석된 byte code는 메모리 영역에 배치, 실질적으로 수행됨<br/>
   -> 이 과정에서 JVM은 스레드 동기화, 가비지 컬랙션 같은 멤모리 작업 수행

![Alt text](asset/jvm-diagram.png)


> ### Runtime Data Areas<br/>
>  JVM이 운영체제 위에서 실행되면서 할당받는 메모리 영역
> 1. PC Register: 스레드가 어떤 명령어로 실행되어야 할지 기록하는 부분(JVM 명령의 주소를 가짐)
> 2. stack area: 지역변수, 매개변수, 메서드 정보, 임시 데이터 저장
> 3. native method stack: 실제 실행할 수 있는 기계어로 작성된 프로그램을 실행시키는 영역
> 4. heap: 런타임에 동적으로 할당되는 데이터가 저장되는 영역. 객체나 배열 생성 등
> 5. method area: JVM이 시작될 때 생성. JVM이 읽은 클래스와 인터페이스에 대한 런타임 상수 풀, 필드 및 메서드 코드, 정적 변수, 메서드의 바이트 코드 등을 보관

### Garbage Collection
자바 프로그램에서 사용되지 않는 메모리를 지속적으로 찾아내서 제거하는 역할

> #### 실행순서<br/> 
> 참조되지 않은 객체 탐색 후 삭제 -> 삭제된 객체의 메모리 반환 -> 힙 메모리 재사용

[Garbage Collection 정리](./garbage-collection.md)