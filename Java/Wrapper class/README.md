# 개념

* 기본 자료형(Primitive data type)에 대한 클래스 표현을 Wrapper class 라고 한다.

​		ex) Integer`, `Float`, `Boolean



# 사용하는 곳

* 컬렉션에서 제네릭을 사용하기 위해서는 Wrapper class 를 사용해줘야 한다.
* `null` 값을 반환해야만 하는 경우에는 return type 을 Wrapper class 로 지정하여 `null`을 반환하도록 할 수 있다.
* 객체지향적 프로그래밍



# 값 비교

* Primitive data type(==) 와는 다르게 메소드(.intvalue())로 비교



# AutoBoxing

```java
List<Integer> lists = new ArrayList<>();
lists.add(1);
```

add 할때 Integer 객체로 감싸서 넣지 않아도 자동으로 감싸줌