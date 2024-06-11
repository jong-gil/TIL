## `@Bean`과 `@Configuration`

- Spring의 자바 configuration 지원에서 주요 요소들은 `@Configuration` 어노테이션이 달린 클래스와 `@Bean` 어노테이션이 달린 메서드
- `@Bean` 어노테이션은 해당 매서드가 Spring IoC container에 의해 새로운 object가 인스턴스화하고, 구성하고, 시작하는 것을 의미하는데 쓰임 
- `@Bean` 이 사용된 메서드는 다른 모든 Spring의 `@Component`bean과 함께 쓰일 수 있음
- 하지만, 주로 `@Configuration` bean과 함께 쓰임
- 클래스를 `@Configuration`로 어노테이션을 쓰는 것은 클래스의 주목적이 bean을 정의하는 것에 쓰임을 나타냄
- `@Configuration` 클래스는 하나의 클래스에 다른 `@Bean` 매소드를을 호출함으로써 bean간의 의존성을 정의할 수 있음

>` @Bean` 메서드 간에 로컬 호출이 있는 또는 없는 `@Configuration` 클래스
> - `@Bean` 메서드가 `@Configuration` 클래스 안에서 선언되는 경우, 컨테이너의 생명주기 관리로 redirect됨
> 	- 이는 같은 `@Bean` 메서드가 발동되는 것을 막아줌
> 	- 주어진 bean에 존재하는 singleton(혹은 scoped) 인스턴스를 재사용함
> - `@Bean` 메서드가 `@Configuration`이 없는 클래스에 선언되는 경우 / `@Configuration(proxyBeanMethods=false)`가 선언되어 있는 경우
> 	- "lite" 모드에서 처리된다는 것을 의미함
> 	- 특별한 runtime processing 없이 일반적인 목적의 factory 메서드가 됨
> 	- container에 의해 intercept되지 않기 때문에 일반적인 메서드 호출과 동일하게 동작
> 	- 새로운 instance를 매번 생성하여 사용하게 됨
> 	- [[CGLIB]] 서브클래스를 생성하지 않음

