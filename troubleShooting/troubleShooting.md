## Spring Cloud
### Eureka Gateway Service
#### 에러
```log
com.netflix.discovery.shared.transport.TransportException: Cannot execute request on any known server
```
#### 해결책
Discovery Service 먼저 등록
```yml
eureka:
    client:
        fetch-registry: false
# ...
```
- register-with-eureka 
  - 자기 자신을 서비스로 등록하는 것 -> true 설정 시 자기 자신을 계속 Health_check 하게 됨
  - 다른 Eureka 클라이언트가 보내는 요청을 자기 자신에게 보내게 됨
- fetch-registry: 마이크로 서비스 인스턴스 목록을 로컬에 캐시할 지 여부

### Spring Cloud Version 맞추기
build.gradle 파일에
```gradle
...
ext {
  set("springCloudVersion", "2021.0.7")
}
...
dependencyManagement {
  imports {
    mavenBom "org.springframework.cloud:spring-cloud-dependencies:${springCloudVersion}"
  }
}
```
등록하게 되면 spring-cloud-starter 관련 dependency 설정 시 버전 명시하지 않아도 됨
