## Error and Exception
#### Error
- 한 번 발생하면 복구하기 어려운 수준의 심각한 문제
- e.g) OutOfMemorryError, StackOverFlow
#### Exception
- 개발자의 잘못된 사용으로 인해 발생
- 코드 수정으로 해결 가능한 문제

## Hierarchy of Exception Class
![[exception_class_hierarhcy.png]]
### Exception (Checked Exception)
- 컴파일러가 코드 실행 전 예외 처리 코드 여부를 검사 
- e.g) ClassNotFoundException, DataFormatException
### Runtime Exception (Unchecked Exception)
- 컴파일러가 예외 처리 코드 여부를 검사하지 않음 
- e.g) ClassCastException, ArrayIndexOutOfBoundsException, NullPointerException
## Try-Catch
```java
try {
	// code to run
} catch (Exception e){
	// code to run when exception occurs
} finally {
	// code always excecutes regradless of exception
}
```
- finally 블록은 꼭 작성해야 하는 것은 아님

### throws & throw

