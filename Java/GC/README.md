# 개념

안쓰는 메모리 관리



# 필요성

프로그램 코드로 명시적으로 메모리 해제하면 시스템 성능에 악영향 -> GC가 쓰레기 객체를 찾아 메모리 해제하도록 해야됨



# Heap GC 종류

![image-20220329211634696](C:\Users\YG\AppData\Roaming\Typora\typora-user-images\image-20220329211634696.png) 

## 1. Minor GC

### 과정 

* Eden -> Survivor -> Old 에서 참조하지 않는 객체 삭제  

​		Eden GC 에서 살아남음-> Survivor1로 이동 -> survivor1 꽉참 -> survivor1 GC -> survivor2로 이동 -> 반복 -> Old 영역으로 이동

​		주의사항 :  survivor1, survivor2 둘중 하나는 비어있어야됨

### GC 대상 식별 

* by 카드 테이블

​		Old 영역이 Young 영역 참조할 때 표시됨 -> 이걸 보고 판별

​		카드테이블 관리는 write barrier가 함 -> 약간의 오버헤드 발생 but GC 시간 줄어듦

## 2. Major GC

### 특징

* Old 영역에 있는 것들에서 참조하지 않는 객체를 한번에 삭제
* 시간 오래걸림
* GC를 제외한 스레드 모두 정지(stop-the-world)

### GC 대상 판별

* Heap 내의 객체 중에서 Root set이 참조하지 않은 객체(Unreachable Objects) 처리

* Root set 구성

  1. Java Stack, 즉 Java 메서드 실행 시에 사용하는 지역변수와 파라미터들에 의한 참조

  ​	2. 네이티브 스택(JNI, Java Native Interface)에 의해 생성된 객체에 대한 참조

  ​	3. 메서드 영역의 정적 변수에 의한 참조

![image-20220329212410557](C:\Users\YG\AppData\Roaming\Typora\typora-user-images\image-20220329212410557.png)



### 종류

* mark-sweep-compact

​				Old 영역의 GC는 mark-sweep-compact이라는 알고리즘을 사용한다. 

​				첫 단계는 Old 영역에 살아 있는 객체를 식별(Mark)하는 것이다. 

​				두번째 단계는 힙(heap)의 앞 부분부터 확인하여 살아 있는 것만 남긴다(Sweep). 

​				마지막 단계에서는 각 객체들이 연속되게 쌓이도록 힙의 가장 앞 부분부터 채워서 객체가 존재하는 부분과 객체가 없는 부분으로 나눈다(Compaction).

#### Serial GC

* mark-sweep-compact 단계를 거친다.
* 적은 메모리와 CPU 코어 개수가 적을 때 적합한 방식

![image-20220329215850459](C:\Users\YG\AppData\Roaming\Typora\typora-user-images\image-20220329215850459.png)

#### Parallel GC

* mark-sweep-compact 단계를 거친다.
* GC를 처리하는 쓰레드가 여러 개이다.
* 메모리가 충분하고 코어의 개수가 많을 때 유리

![image-20220329215902975](C:\Users\YG\AppData\Roaming\Typora\typora-user-images\image-20220329215902975.png)

#### Parallel Old GC(Parallel Compacting GC)

* JDK 5 update 6부터 제공한 GC방식이다. 
* Mark-Summary-Compaction 단계를 거친다.
  * Summary 단계는 앞서 GC를 수행한 영역에 대해서 별도로 살아 있는 객체를 식별한다.



#### CMS GC

* stop-the-world 시간이 매우 짧다. 
* 모든 애플리케이션의 응답 속도가 매우 중요할 때 사용
* 메모리와 CPU를 더 많이 사용
* Compaction 단계가 기본적으로 제공되지 않는다.

![image-20220329215313779](C:\Users\YG\AppData\Roaming\Typora\typora-user-images\image-20220329215313779.png)

#### G1 GC

* 데이터가 Old 영역으로 이동하는 단계가 사라진 GC 방식
* 빠르다
* 불안정적이다.

![image-20220329220300657](C:\Users\YG\AppData\Roaming\Typora\typora-user-images\image-20220329220300657.png)