## Dispatcher Servlet
- 요청 처리를 위한 알고리즘 제공
- Spring configuration을 이용하여 요청 매핑, 뷰 보여주기, 예외 처리 등에 필요한 대표 component를 찾음
```java
public class MyWebApplicationInitializer implements WebApplicationInitializer {

	@Override
	public void onStartup(ServletContext servletContext) {

		// Load Spring web application configuration
		AnnotationConfigWebApplicationContext context = new AnnotationConfigWebApplicationContext();
		context.register(AppConfig.class);

		// Create and register the DispatcherServlet
		DispatcherServlet servlet = new DispatcherServlet(context);
		ServletRegistration.Dynamic registration = servletContext.addServlet("app", servlet);
		registration.setLoadOnStartup(1);
		registration.addMapping("/app/*");
	}
}
```

## Annotated Controllers
### `@RequestMapping`
- URL, HTTP method, 요청 파라미터, 해더, 미디어 타입 등의 요소를 가지고 있음
- HTTP method에 접근하는 shortcut이 존재
	- `@GetMapping`, `@PostMapping, @PutMapping, @DeleteMapping, @PatchMapping`
- `@RequestMapping`은 주로 공통된 표현을 나타낼 때 사용