## HTTP 1.1
- persistent-connection
	- 연속적인 요청에 대해 connection을 유지
	- 새로운 handshake에 대한 부담을 줄일 수 있음
	- idle connection은 특정 시간이 지난 후에 닫힘
	- idle 상태 일 때도 서버 자원을 소모함 -> DoS 공격에 취약함
- ~~Pipelining~~
	- 응답을 기다리지 않고 연속적인 요청을 보낼 수 있음
	- 제대로 수행 되기 어려움. HOL과 같은 문제에 취약함
	- Multiplexing으로 대체됨
![[http1_x_connections.png]]
## HTTP 2.0
- Multiplexed Streams
	- multiplexing은 여러 개의 신호를 하나의 공유된 매개로 결합하는 것
	- 하나의 connection에 여러 개의 메시지를 주고받을 수 있음
	- 응답은 순서에 상관없이 stream으로 주고 받음
	- stream이 뒤섞인 경우 stream number를 통해 받는 쪽에서 재조합
![[multiplexing.png]]

참고
- [HTTP - MDN docs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Connection_management_in_HTTP_1.x)
- [hoo00nn Velog](https://velog.io/@hoo00nn/HTTP1.1-vs-HTTP2.0)
- [Multiplexing - Wikipedia](https://en.wikipedia.org/wiki/Multiplexing)
