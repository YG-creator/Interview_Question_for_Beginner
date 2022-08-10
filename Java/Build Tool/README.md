# 1. Apache Ant

## 1. 특징

1. 자바 기반의 Build Tool(여러 플랫폼에서 독립적으로 실행 -> 동인한 빌드 환경)

2. 파일 형식은 XML(build.xml)

3. target에 작업들을 배치해 커스터마이징 함

4. 빌드 시 target의 우선순위를 변경할 수 있고, 배제할 수도 있음
5. 형식적인 규칙이 없음
6. 명확한 빌드 절차가 없음
7. 생명주기가 없음
8. 외부저장보 불러올 수 가 없음

## 2. 기본 구조

```xml
<project name="" default="" basedir="">
  <description></description>
  <proppery></property>
  <proppery></property>
  <proppery></property>
  <target name="A">
        <Task />
         <Task />
  </target>
  <target name="B" depends="" description="">
        <Task />
         <Task />
  </target>
</project>
```



## 3. 설치 시 주의사항

1. Java가 깔려 있어야 한다.
2. JDK_HOME이 환경변수에 잡혀 있어야 한다.
3. 사용할 ant의 버전이 1.2~1.5라면 jdk 1.1이상
                      1.6.* 이라면 1.2 이상
                      1.7.* 이라면 1.3 이상
                      1.8.* 이라면 1.4 이상 
                      1.9. ~ 1.10. 이라면 jdk 1.4 이상

​	4. jdk 1.7이상 추천



## 4. 설치

http://ant.apache.org/bindownload.cgi

1. 위 사이트에서 zip 버전을 다운받는다(Window 기준, Linux 에서는 Make를 사용해 볼 생각).
2. 압축을 푼다.
3. 환경변수를 잡아준다(압축푼 디렉토리\apache-ant-1.10.1[바꾸지 않았을 경우]\bin).
4. 정상적으로 잡힌 것을 확인한다. => CommandLine에서 ant -version 이라고 치면 설치 버전 나온다.





# 2. Maven

## 1. 특징

1. 표준화된 포멧 제공
2. 외부저장소 불러 올 수 있다.
3. 프로젝트의 생명주기 관리
4. 상속구조로 멀티 모듈 구현



# 3. Gradle

## 1. 특징

1. groovy 라는 스크립트 언어 사용
2. XML에 비해 가독성이 좋음
3. Maven에 비해 빠름
4. Configuration Inject(의존성 주입)으로 상속 안해도됨
5. 단위 테스트 시 의존성 관리
6. 일괄된 디렉토리 구조 가짐