### @Bean
#### Bean 선언
- [[ApplicationContext]] 내에서 선언되어 동작함
- `@Configuration` 혹은 `@Component` 어노테이션이 달린 클래스에서 사용 가능
- Bean 선언 코드
```java
@Configuration
public class AppConfig {

	@Bean
	public TransferServiceImpl transferService() {
		return new TransferServiceImpl();
	}
}
```
