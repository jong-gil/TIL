> Test Driven Development

![[tdd_process.png]]
1. The Red Phase
- 사용자는 실행될 행위에 대한 테스트를 생성
- 클래스나 함수를 정의하는 코드를 작성하지 않음 -> 본질적으로 실패할 수 밖에 없음 -> Red Stage
2. The Green Phase
- 최적의 실행 방법을 찾기 보다 문제점 해결을 우선 목표로 설정
- 첫 번째 테스트를 통과할 목적으로 코드 작성 (최대한 간단한 코드 작성)
- 첫 번째 테스트 통과 후 다음 테스트 통과 목적 코드 작성
3. The Refactoring Phase
- 모든 테스트를 통과하면서 코드 리팩토링 진행

## Test Double
- 영화 스턴트 더블 개념에서 비롯
- 실제 객체를 활용하기 어렵거나 비용이 많이 들 때 진행
#### Dummy
- 인스턴스화 된 객체가 필요하지만 기능은 필요하지 않을 때
- Dummy 객체 메스드의 정상 동작은 보장하지 않음
- 전달만 되고 사용되지 않는 객체
```java
public interface PrintWarning {
	void print()
}

public class PrintWarningDummy implements PrintWarning {
	@Override
	public void print(){
		// doesn't operate
	}
}
```
#### Fake
- 복잡한 로직 or 객체 내부에서 필요로 하는 다른 외부 객체의 동작을 단순화하여 구현
- 실제 프로덕션에 적합하지 않은 객체
```java
public interface UserRepository {
	void save(User user);
	User findById(Long userId);
}

public FakeUserRepository implements UserRepository {
	private Collection<User> users = new ArraryList<>();

	@Override
	public void save(User user) {
		if(findById(user.getId() == null)) {
			user.add(user);
		}
	}

	@Override
	public User findById(Long id) {
		for(User user: users) {
			if(user.getId() == id) {
			return user;
			}
			return null;
		}
	}
}
```
- 테스트 대상이 되는 객체가 DB와 연관이 있을 때
- 가짜 DB 역할을 하는 FakeUserRepository를 만들어 테스트 객체에 주입
- 테스트 객체는 DB에 의존하지 않으면서 동일한 동작을 하는 가짜 DB를 가지게 됨
#### Stub
- Dummy 객체가 실제로 동작하는 것처럼 보이게 만들어 놓은 객체
- 인터페이스 또는 기본 클래스가 최소한으로 구현된 상태
- 테스트에서 호출된 요청에 대해 미리 준비해둔 결과를 제공
```java
public class StubUserRepository implements UserRepository {
	@Override
	public User findbyId(Long id) {
		return new User(id, "Hello");
	}
}
```
- 테스트 환경에서 User 인스턴스의 name을 Test User 만 받기를 원할 때
- 테스트가 수정될 경우 Stub 객체도 함께 수정 해야한는 단점 존재
```java
@Test
public mockito_stub_example() {
	given(stubUserRepository.findById(anyInt())).willReturn(new User(1L, "Hello"))
	// ...

}
```
- 테스트를 위해 의도된 값만 반환하도록 하는 객체 -> **Stub**
#### Spy
- Stub의 역할과 함께 호출된 내용에 약간의 정보 기록
- 테스트 더블로 구현된 객체에 자기 자신이 호출되었을 때 확인이 필요한 부분을 기록
- 실제 객체처럼 동작 or 필요한 부분은 Stub으로 만들어 동작을 지정
```java
public class MailingService {
	private int sendMailCount = 0;
    private Collection<Mail> mails = new ArrayList<>();

	public void sendMail(Mail mail) {
		System.out.println("Real sendMail Called");
		sendMailCount++;
		mails.add(mail);
	}

	public int getMailCount() {
		System.out.println("Real getSendMailCount Called");
		return sendMailCount;
	}
}
```
테스트코드
```java
@Spy
MailingService mailingService;
@Test
void spy_example() throws Exception {
	// given
	Mail param = new Mail();
	// getSendMailCount()를 호출할 때 항상 5를 리턴 (실제 implementation과 상관없이)
	doReturn(5).when(mailingService).getSendMailCount();

	// when
	mailingService.sendMail(param);
	// 앞선 stubbing의 적용을 받아 5를 출력
	System.out.println(mailingService.getSendMailCount());

	// then
	verify(mailingService).sendMail(param);
	verity(mailingService).getSendMailCount();
}
```
- 

#### Mock




## Unit Test

## Integration Test

## Classicist

## Mockist

참고
- [what is tdd - Spcieworks](https://www.spiceworks.com/tech/devops/articles/what-is-tdd/)
- [알고풀자 Blog](https://algopoolja.tistory.com/119)
