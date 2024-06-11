## 정의
- [[스프링컨테이너]]의 종류 (`BeanFactory` , `ApplicationContext` 등)
- `BeanFactory`의 하위 인터페이스 (BeanFactory에 부가기능이 추가된 것)
	- `BeanFactory`는 설정 프레임워크와 기본적 기능 제공
	- `ApplicationContext`는Spring의 AOP특징을 더 쉽게 통합할 수 있게 함

## 기능
- 어플리케이션 요소에 접근하는 Bean factory method 제공 (`ListableBeanFactory` 상속)
- 일반적인 방법으로 파일 리소스를 불러오기 (`ResourceLoader` 상속)
- 등록된 리스너에 이벤트 publish (`ApplicationEventPublisher` 상속)
- 국제화를 지원하는 메시지 resolve (`MessageSource`상속)
- 부모 context로부터 상속. 자손에 있는 정의가 항상 우선권을 가짐
	- 하나의 부모 context가 전반적인 웹 어플리케이션에 사용되면서,
	- 각각의 servlet이 고유한 자식 (다른 servlet과 독립된) context를 가질 수 있음

## 특징
- Spring IoC container를 나타냄
- Bean의 인스턴스화, 구성, 조합 등을 담당
- XML, Java 어노테이션, Java 코드 등으로 나타낼 수 있음
- 컨테이너는 configuration metadata를 읽어서 컴포넌트의 지시를 받음
	- configuration metadata는 
		- 어노테이션이 달린 컴포넌트 클래스
		- factory method로 표현된 configuration 클래스
		- 외부 XML 파일 혹은 Groovy 스크립트로 나타낼 수 있음

## Spring 동작 방식
- 어플리케이션 클래스들은 **configuration metadata**와 결합되어있음
- `ApplicationContext`가 생성되고 실행될 때, 완벽히 구성되고 실행 가능한 시스템이나 어플리케이션을 실행할 수 있음

![[springContainer_structure.png]]

### Configuration Metadata
- 개발자가 어플리케이션 내에서 스프링 컨테이너에게 구성요소(component)들을 인스턴스화하고, 구성(configure)하고 조합하도록 명령하는 것
- Spring IoC container는 configuration metadata가 어떻게 쓰여저있는 지와 완전히 독립되어있음
- 최근 많은 개발자들은 자바 기반의 구성([[Java-based Configuration]])을 사용하고 있음
	- 어플리케이션 클래스 외부에서 bean을 성의하는 방법
	- `@Configuration`, `@Bean`, `@Import`, `@DependsOn` 등의 어노테이션을 활용


출처
- [스프링공식문서 - ApplicationContext](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/ApplicationContext.html)
- [스프링공식문서 - SpringContainer](https://docs.spring.io/spring-framework/reference/core/beans/basics.html)
