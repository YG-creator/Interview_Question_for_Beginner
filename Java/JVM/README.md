# 1. JVM이란?

자바 가상 머신(프로그램의 실행을 실행하기 위해 필요한 물리적 머신을 소프트웨어로 구현)



# 2. 역할 및 특징

* JAVA application을 클래스 로더로 읽어 들여 자바 API와 함께 실행한다.
* OS상관없이 JAVA를 재사용 가능하게 한다.
* 메모리 관리, Garbage Collection
* 스택기반



# 3. 자바프로그램 실행과정

1. 프로그램이 실행되면 JVM은 OS로부터 이 프로그램이 필요로 하는 메모리를 할당받는다.

​	  JVM은 이 메모리를 용도에 따라 여러 영역으로 나누어 관리한다.

2. 자바 컴파일러(javac)가 자바 소스코드(.java)를 읽어들여 자바 바이트코드(.class)로 변환시킨다.

3. Class Loader를 통해 class파일들을 JVM으로 로딩한다.

4. 로딩된 class파일들은 Execution engine을 통해 해석된다.

5. 해석된 바이트코드는 Runtime Data Areas 에 배치되어 실질적인 수행이 이루어지게 된다.

​	이러한 실행과정 속에서 JVM은 필요에 따라 Thread Synchronization과 GC같은 관리작업을 수행한다.

 

*JVM>>*

![img](https://t1.daumcdn.net/cfile/tistory/25616D45576B854C3F)

# 4. JVM 구성

## **Class Loader(클래스 로더)**

* Loading : JVM내로 클래스(.class파일)를 로드
* Linking : 배치하는 작업을 수행
* Initialization : Runtime 시에 동적으로 클래스를 로드하고 링크한다. 
* 사용하지 않는 클래스들은 메모리에서 삭제한다. 

## **Execution Engine(실행 엔진)**

* 바이트코드 -> 기계어 -> 실행

### **Interpreter(인터프리터)**

* 바이트 코드 명령어 마다(한 줄) -> 기계어 -> 실행   :  느림

* 한번만 사용할때 유리

### **JIT(Just - In - Time)**

* 바이트 코드 전체 -> 네이티브 코드 -> 실행      :  변환이 느림
* 네이티브 코드는 캐시에 보관되서 한번 컴파일된 코드는 빠르게 수행 가능
* 여러번 사용 할 때 유리

### **Garbage collector**

* GC(Garbage Collection)를 수행하는 모듈 (쓰레드)이 있다.

  

## **Runtime Data Area**

* 프로그램을 수행하기 위해 OS에서 할당받은 메모리 공간

![img](https://t1.daumcdn.net/cfile/tistory/275A103F576B85550D)

### **1) PC Register**

* 공간으로 스레드마다 하나씩 존재한다. 

* 현재 수행 중인 JVM 명령의 주소를 갖는다.

### **2) JVM 스택 영역**

* 메소드 안에서 사용되는 값들(local variable)을 저장한다. 
* 호출된 메소드의 매개변수, 지역변수, 리턴 값 및 연산 시 일어나는 값들을 임시로 저장한다.

### **3) Native method stack**

* 기계어로 작성된 프로그램을 실행시키는 영역이다. 

### **4) Method Area (= Class area = Static area)**

* 클래스 정보를 처음 메모리 공간에 올릴 때 **초기화되는 대상을 저장하기 위한 메모리 공간**. 
* **Runtime Constant Pool**(상수 자료형을 저장하여 참조하고 중복을 막는 역할)
* 올라가는 정보의 종류

#### **1) Field Information**

멤버변수의 이름, 데이터 타입, 접근 제어자에 대한 정보

#### **2) Method Information**

메소드의 이름, 리턴타입, 매개변수, 접근제어자에 대한 정보

#### **3) Type Information**

class인지 interface인지의 여부 저장 /Type의 속성, 전체 이름, super class의 전체 이름(interface 이거나 object인 경우 제외)

*Method Area는 클래스 데이터를 위한 공간이라면 Heap영역이 객체를 위한 공간이다.*

* *Heap과 마찬가지로 GC의 관리 대상에 포함된다.* 



### **5) Heap( 힙 영역 )**

* new연산자로 생성된 객체와 배열을 저장한다. 
* 물론 class area영역에 올라온 클래스들만 객체로 생성할 수 있다. 

![img](https://t1.daumcdn.net/cfile/tistory/266E283B576B8E060B)



##### **New/Young 영역**

\- **Eden** : 객체들이 최초로 생성되는 공간

\- **Survivor 0 / 1** : Eden에서 참조되는 객체들이 저장되는 공간



##### **Old 영역**

New area에서 일정 시간 참조되고 있는, 살아남은 객체들이 저장되는 공간 Eden영역에 객체가 가득차게 되면 첫번째 GC(minor GC)가 발생한다. Eden영역에 있는 값들을 Survivor 1 영역에 복사하고 이 영역을 제외한 나머지 영역의 객체를 삭제한다.

인스턴스는 소멸 방법과 소멸 시점이 지역 변수와는 다르기에 힙이라는 별도의 영역에 할당된다. 자바 가상 머신은 매우 합리적으로 인스턴스를 소멸시킨다. 더이상 인스턴스의 존재 이유가 없을 때 소멸시킨다.



##### **Permanent Generation**

생성된 객체들의 정보의 주소값이 저장된 공간이다. Class loader에 의해 load되는 Class, Method 등에 대한 Meta 정보가 저장되는 영역이고 JVM에 의해 사용된다. Reflection을 사용하여 동적으로 클래스가 로딩되는 경우에 사용된다. 내부적으로 Reflection 기능을 자주 사용하는 Spring Framework를 이용할 경우 이영역에 대한 고려가 필요하다.
