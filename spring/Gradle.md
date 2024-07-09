> 안드로이드, 자바, 코틀린, 그루비, 스칼라, 자바스크립트, C/C++ 지원
> 빌드 자동화 도구

## 빌드
- 빌드
	- 소스 코드 -> 컴파일 + 의존성 추가 -> 실행 가능한 파일로 패키징 (.jar .war)
- 빌드 도구
	- 코드를 실행 가능한 파일로 만들어 줌
	- 라이브러리 관리, 테스팅 등을 **자동화**
- 역사
	- Apache Ant
		- xml 기반 스크립트 사용
		- 정해진 규칙 x -> 유연성
		- 라이브러리 의존 관계를 정의하는 구조가 없음
		- 유지 보수가 어려움
	- Maven
		- 정해진 라이프 사이클에 따라 빌드
		- pom.xml 파일 사용
		- 라이브러리 의존성 자동 관리
		- xml 기반 -> 가독성이 떨어짐
		- 상속구조를 이용한 멀티 모듈 -> 의존관계가 복잡한 프로젝트에 부적절
## Gradle
#### Groovy 기반의 스트립트 언어
- 스크립트 언어
	- 동적으로 실행 가능
	- 스크립트 로직을 직접 작성 -> 추가적인 로직 구현
	- Gradle이 지원하는 Plugin 호출 가능
- Groovy 기반의 DSL
	- DSL (Domain Specific Language): 특정 도메인을 대상으로 만들어진 특수 프로그래밍 언어
	- 자바와 유사한 문법 구조 && 자바 호환
	- JVM에서 실행되는 스크립트 언어
- 빌드 스크립트 (`build.gradle`)
	- Plugins
		- 특정 작업을 위해 모아놓은 task들의 묶음
	- Dependencies
		- dependencies configuration: 라이브러리 추가 시점 설정
			- Implementation: 런타임 + 컴파일 시점
			- compileOnly: 컴파일할 때만 사용
			- runtimeOnly: 런타임일 때만 사용
			- testImplementation: 테스트에만 사용
	- Repositories
		- 라이브러리(모듈)가 저장된 위치를 정의
		- mavenCentral(), jCenter(), googleAndroid() 등이 있음
		- 라이브러리 저장소를 명시하면 Gradle이 해당 저장소에서 필요한 라이브러리를 가지고 옴
#### 특징
- Build Cache
	- 빌드 결과물을 캐싱하여 재사용
	- 라이브러리 의존성을 캐시로 저장 후 이전에 다운로드한 라이브러리 재사용
- 점진적 빌드
	- 마지막 빌드 호출 후 변경된 부분만 빌드
	- 변경되지 않은 부분은 캐시 결과를 검색해 재사용
	- 태스크의 입력/출력 혹은 변경되지 않은 부분은 빌드x
		- 변경 사항이 없을 경우 up-to-date 표시
- 데몬 프로세스 사용
	- 다음 빌드 작업을 위해 백그라운드에서 대기하는 프로세스
	- 초기 빌드 이후 빌드 실행 시 초기화 작업x
	- 한 번 빌드된 프로젝트는 다음 빌드에서 소요되는 시간이 매우 적음
#### 멀티프로젝트 빌드 지원
- 폴더 구조
```
root-project
	ㄴa-project
	ㄴb-project
	ㄴcommon-module
```
- root-project > settings.gradle
```
rootProject.name = 'root-project'
include `a-project`
include `b-project`
include `common-module`
```
- root-project > build.gradle
```
project(':a-project') {
	dependencies {
		compileOnly project(':common-module')
	}
}
project(':a-project') {
	dependencies {
		compileOnly project(':common-module')
	}
}
```
- jar 생성 비활성화
	- root > build.gradle
	- common-module > build.gradle
```
bootJar.enabled = false
```
	- root 프로젝트와 common-project는 main 없이 라이브러리 역할을 하는 모듈이기 때문에 실행하지 않는 다는 것을 명시
---
참고
- [우테코 Youtube](https://www.youtube.com/watch?v=V4knLFDG-ZM&list=PLgXGHBqgT2TvpJ_p9L_yZKPifgdBOzdVH&index=145)