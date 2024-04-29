## EC2 기본 환경 설정
### 자바 설치
```shell
sudo apt update
sudo apt-cache search jdk | grep openjdk-11
sudo apt install openjdk-11-jdk -y
java --version
```
### git 설정
```shell
# git 설치
sudo apt install git
git --version

# git repository 설정
git clone <repository-address>
```

### 빌드 및 실행
```shell
# 빌드
./gradlew build

# jar 파일 실행
cd build/libs
java -jar <SNAPSHOT.jar>
```
> `gradlew 권한 오류` 발생시 
> ```
> sudo chmod 777 ./gradlew   
> ```




---
#### 참고 자료
- [[Spring 배포 #1] AWS EC2 서버에 스프링 프로젝트 수동으로 배포하기](https://loosie.tistory.com/408)
- [aws 배포 1단계: EC2 수동 배포](https://velog.io/@wisdom08/aws-배포-1단계-버전)