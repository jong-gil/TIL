`@SpringBootApplication`의 내부구조
![[springBootApp_structure.png]]

- 하단의 어노테이션부터 순차적으로 적용됨
	- `@ComponentScan`: 사용자가 등록한 `@Component`를 우선적으로 탐색
	- `@EnableAutoConfiguration` 
		- `org.springframework.boot:spring-boot-autoconfigure` 라는 외부 라이브러리를 참조함
		- 이 라이브러리 내부에 `spring.factories`파일은 컨테이너 설정을 위한 `ApplicationConetxt` 실행부터 다양한 역할을 수행
		- 내부의 spring 패키지의 `AutoConfiguration.imports` 파일에서 자동으로 구성할 구성파일들을 읽어서 설정함