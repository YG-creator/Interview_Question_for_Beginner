# Field member

## 개념

클래스에 변수를 정의하는 공간



## 주의점

* 여러 스레드가 접근하는 싱글톤 객체라면 field에서 상태값을 가지면 안된다.
* 모든 변수를 parameter로 넘겨받고 return 방식으로 구성해야됨



# Synchronized()

## 개념

필드에 Collection을 사용하고 싶을 때 synchronized 키워드 기반으로 Collection 구현



## ex

List -> Vector

Map -> HashTable



## 단점

1. 제공하는 API가 적음
2. 성능이 좋지 않음



## 단점 해결

1. Collection util 클래스에서 제공되는 static 메소드Collections.synchronizedList()`, `Collections.synchronizedSet()`, `Collections.synchronizedMap()

2. concurrent package 

   ConcurentHashMap이라는 구현체 제공

   Collections util 을 사용하는 것보다 `synchronized` 키워드가 적용된 범위가 좁아서 보다 좋은 성능을 낼 수 있는 자료구조

 



# ThreadLocal

## 개념

스레드 사이에 간섭이 없어야 하는 데이터에 사용한다.



## 사용법

1. ThreadLocal 객체를 생성한다.
2. ThreadLocal.set() : 현재 스레드의 로컬 변수에 값을 저장한다.
3. ThreadLocal.get() : 현재 스레드의 로컬 변수 값을 읽어온다.
4. ThreadLocal.remove() : 현재 스레드의 로컬 변수 값을 삭제한다.

