> request를 보내고 response를 받아오는데 사용됨
> immutable

## HttpClient
- *builder*를 사용하여 `HttpClient`가 생성됨
- *newBuilder* 메서드는 default HttpClient 인스턴스를 생성
- *builder*는 각 클라이언트 별 상태를 설정할 수 있음
	- 선호하는 프로토콜 버전 (HTTP/1.1 or HTTP/2)  ([[HTTP Connection Management]])
	- redirect, 프록시, 혹은 authenticator를 사용할 지

## Connection Pool
- 동일한 클라이언트가 같은 요청을 보낼 때, HttpConnection은 한번 만 생성됨
	- 다른 클라이언트가 보내면 HttpConnection이 새로 생성됨

참고
- [HttpClient - Java Docs](https://docs.oracle.com/en/java/javase/17/docs/api/java.net.http/java/net/http/HttpClient.html)
- [Baeldung Blog](https://baeldung.com/java-httpclient-connection-management)