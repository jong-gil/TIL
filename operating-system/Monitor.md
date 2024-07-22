> 병행 컴퓨팅 (concurrent computing)에서 여러 스레드가 공유하는 자원에 동시에 접근하는 것을 방지하고 상태를 변화시킬 때까지 기다리게 하는 **동기화** 구조 

#### Mutex
- mutex lock을 취득하지 못한 스레드는 큐에 들어간 후 waiting (대기)상태로 전환
- mutex lock을 가진 스레드가 lock을 반환 -> 큐에 있던 대기상태인 스레드 중 하나가 실행

#### Condition Variable
- waiting queue: 조건이 충족되길 기다리는 스레드들이 대기 상태로 머무는 곳
- wait: thread가 자기 자신을 condition variable의 waiting queue에 넣고 대기 상태로 전환
- broadcast: waiting queue에 대기 중인 스레드 전부를 깨움
##### 두 개의 큐
- entry queue: critical section에 진입을 기다리는 큐
- waiting queue: 조건이 충족되기를 기다리는 큐
```cpp
acquire(m); // 모니터 락 취득
while(!p) {  // 조건 확인
	wait(m, cv); // 조건 충족x -> waiting
}
// critical section 코드

signal(cv2); --OR-- broadcast(cv2);
release(m); // 모니터 락 반환
```
### Bounded producer / consumer problem
- signal-and-wait
- signal-and-continue
```cpp
global volatile Buffer q;
global Lock lock;
global CV fullCV;
global CV emptyCV;
```
```cpp
public method producer() {
	while(true) {
		task myTask = ...;

		lock.acquire();
		
		while(q.isFull()) {
			wait(lock, fullCV);
		}
		q.enqueue(myTask);
		
		signal(emptyCV); --or-- broadcast(emptyCV);
		
		lock.release();
	}
}

public method consumer() {
	while(true) {
		lock.acquire();
		
		while(q.isEmpty()) {
			wait(lock, emptyCV);
		}
		myTask = q.dequeue();
		
		signal(fullCV); --or-- broadcast(fullCV);
		
		lock.release();
		
		doStuff(myTask);
	}

}
```
### Monitor in Java
- 자바의 모든 객체는 내부적으로 모니터를 가진다
- 모니터의 mutual exclusion -> **synchronized** keyword 사용
- 자바는 condition variable 하나만 가진다
- 자바 모니터의 동작
	- wait
	- notify (signal)
	- notifyAll (broadcast)
- bounded producer/consumer problem
```java
class BoundedBuffer {
	private final int[] buffer = new int[5];
	private int count = 0;

	public synchronized void produce(int item) {
		while(count == 5) {
			wait();
		}
		buffer[count++] = item;
		notifyAll();
	}

	public void consume() {
		int item = 0;
		synchronized(this) {
			while(count == 0) {
				wait();
			}
			item = buffer[--count];
			notifyAll();
		}
		System.out.println("Consume:" + item)
	}
}


public class Main {
	public static void main(String[] args) throws {
		BoundedBuffer buffer = new BoundedBuffer();
		
		Thread consumer = new Thread(() -> {
			buffer.consume();
		});
		
		Thread producer = new Thread(() -> {
			buffer.produce(100);
		});
		
		consumer.start();
		producer.start();
		
		consumer.join();
		producer.join();
		System.out.println("완료");
	}
}
```
- synchronized 
	- method 앞에 사용
	- block으로 사용
		- parameter (lock)를 전달해줘야 함
		- 자기 자신(객체)의 lock을 가지고 들어가라는 의미
---
참고
- [Monitor - Wikipedia](https://en.wikipedia.org/wiki/Monitor_(synchronization))
- [쉬운코드 - Youtube](https://www.youtube.com/watch?v=Dms1oBmRAlo&t=10s)
- 