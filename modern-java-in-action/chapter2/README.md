## CHAPTER2 : 동작 파라미터화 코드 전달하기

변화하는 요구사항은 소프트웨어 엔지니어링에서 피할 수 없는 문제다. 

`동작 파라미터화` 는 자주 바뀌는 요구사항에 효과적으로 대응할 수 있는 코드를 구현할 수 있고 나중에 엔지니어링 비용을 줄일 수 있다. 

동작 파라미터화 방법은 다양하다. 코드 전달 기법을 이용하면 동작을 메서드의 인수로 전달할 수 있다. 하지만 자바 8 이전에는 코드를 지저분하게 구현해야 했다. 익명 클래스로도 어느 정도 코드를 깔끔하게 만들 수 있지만 자바 8에서는 인터페이스를 상속받아 여러 클래스를 구현해야 하는 수고를 없앨 수 있는 방법을 제공한다.

### 1. 코드/동작 전달하기

Predicate를 만들어서 메소드로 원하는 필터링 조건을 전달할 수 있다. 

아래 코드는 사과색이 빨간색 그리고 사과 무게가 50이 넘는다는 동작 조건을 ApplePredicate 객체로 감싸서 전달한다. 해당 조건 외에도 다양한 동작 조건의 구현체를 구현해서 전달할 수 있다. 즉, 추상적 조건으로 필터링 가능하다.

```java
public class AppleRedAndHeavyPredicate implements ApplePredicate{
	@Override
	public boolean test(Apple apple) {
		return Color.RED.equals(apple.getColor()) &&
			apple.getWeight() > 50;
	}
}
```

```java
private List<Apple> filterApples(List<Apple> apples, ApplePredicate predicate) {
  List<Apple> result = new ArrayList<>();
  for (Apple apple : apples) {
    if (predicate.test(apple)) {
      result.add(apple);
    }
  }
  return result;
}
```

```java
List<Apple> redAndHeavyApples = filterApples(Main.apples, new AppleRedAndHeavyPredicate());
```

위의 방법은 다양한 조건을 구현하여 사용할 수 있다는 장점이 있지만, 구현체를 모두 구현해야 한다는 번거로움이 있다. 이는 상당히 번거로운 작업이고 시간 낭비이다.

### 2. 익명 클래스 사용

 `익명 클래스` 를 사용하면 이러한 문제를 줄일 수 있다.

익명 클래스는 클래스 선언과 인스턴스화를 동시에 할 수 있다. 즉, 즉석에서 필요한 구현을 만들어서 사용할 수 있다.

```java
List<Apple> redApples = filterApples(Main.apples, new ApplePredicate() {
  public boolean test(Apple apple) {
    return Color.RED.equals(apple.getColor());
  }
});
```

익명 클래스를 사용하면 인터페이스를 구혀ㅛㄴ하는 여러 클래스를 선언하는 과정을 줄일 수 있지만, 여전히 역시 반복되는 코드가 많고 구현 코드가 장황해질 수 있다는 단점이 존재한다. 장황한 코드는 구현하고 유지보수하는 데 시간이 오래 걸리고 가독성도 좋지 않다. 

### 3. 람다 표현식 사용

람다 표현식을 사용하면 위 코드를 다음처럼 간단하게 재구현할 수 있다.

```java
List<Apple> redApples = filterApples(Main.apples, (Apple apple) -> Color.RED.equals(apple.getColor()));
```



### 예제

자바 API의 많은 메서드를 다양한 동작으로 파라미터화할 수 있다. 또한 이들 메서드를 익명 클래스와 자주 사용한다.

1. Comparator로 정렬하기

   ```java
   public interface Comparator<T> {
     int compare(T o1, T o2);
   }
   ```

   ```java
   // 익명 클래스
   apples.sort(new Comparator<Apple>() {
     @Override
     public int compare(Apple o1, Apple o2) {
       return o1.getWeight() - o2.getWeight();
     }
   });
   
   // 람다
   apples.sort((o1, o2) -> o1.getWeight() - o2.getWeight());
   ```

2. Runnable로 코드 블록 실행하기

   ```java
   public interface Runnable {
     void run();
   }
   ```

   ```java
   // 익명 클래스
   Thread t = new Thread(new Runnable() {
     @Override
     public void run() {
       System.out.println("hello world");
     }
   });
   
   // 람다
   Thread t = new Thread(() -> System.out.println("hello world"));
   ```



### 정리

- 동작 파라미터화에서는 메서드 내부적으로 다양한 동작을 수행할 수 있도록 코드를 메서드 인수로 전달한다.
- 동작 파라미터화를 이용하면 변화하는 요구사항에 더 잘 대응할 수 있는 코드를 구현할 수 있고, 나중에 엔지니어링 비용을 줄일 수 있다.
- 코드 전달 기법을 이용하면 동작을 메서드의 인수로 전달할 수 있다. 하지만 자바 8 이전에는 코드를 지저분하게 구현해야 했다. 익명 클래스로도 코드를 어느 정도 깔끔하게 만들 수 있지만, 인터페이스를 상속받아 여러 클래스를 구현해야 하는 수고로움이 있었다. 이러한 수고를 없앨 수 있는 방법을 자바8에서 제공한다.
- 자바 API의 많은 메서드는 정렬, 스레드, GUI 처리 등을 포함한 다양한 동작으로 파라미터화할 수 있다.