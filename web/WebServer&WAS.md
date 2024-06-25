## Web Server
- 하드웨어 측면
	- 웹 서버의 소프트웨어와 웹 사이트의 **컴포넌트 파일**을 저장하는 컴퓨터
		- 컴포넌트 파일 (static 파일)
			- HTML 문서
			- 이미지
			- CSS Stylesheets
			- JavaScript 파일
- 소프트웨어 측면
	- 웹 사용자가 어떤 방식으로 호스트 파일에 접근하는지 관리
	- URL, HTTP, HTML
- 작동
	- 브라우저가 웹 서버에 호스트된 파일을 필요로 할 때 HTTP를 통해 요청을 보냄
	- HTTP 서버가 요청을 받고 요구된 문서를 찾고 브라우저로 다시 보낼 때도 HTTP를 사용
![[web-server.svg]]
## WAS
> Web Application Server

- 동적 컨텐츠 생성, 어플리케이션 로직과 다양한 리소스와의 통합을 통해 웹 서버의 기능을 확장함
	- 동적 컨텐츠: 사용자 지정된 데이터 표현, 개인화된 UI, DB 결과 및 처리된 HTML
- 어플리케이션 코드 실행
- **다른 소프트웨어** 구성 요소와 상호 작용할 수 있는 런타임 환경 제공
	- 메시징 시스템
	- 데이터베이스
- 작동
	- 브라우저가 웹 서버에 요청을 보냄
	- 웹 서버는 요청을 어플리케이션 서버로 전송
	- 어플리케이션 서버는 비지니스 로직 적용 & 다른 서버 및 서드 파티 시스템과 통신하여 요청 수행하여 웹 서버로 전달
	- 웹 서버가 브라우저에 응답 반환

### 웹 컨테이너
> [[Servlet]], JSP를 실행할 수 있는 소프트웨어

### Servlet
- 요청에 대한 응답을 제공하는 자바 클래스
- 

### 참고
[MDN docs-Web Server](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_web_server)
[AWS](https://aws.amazon.com/ko/compare/the-difference-between-web-server-and-application-server/)

