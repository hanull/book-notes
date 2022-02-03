## CHAPTER 3 : 람다 표현식

### 3.1 람다란 무엇인가?

람다 표현식은 메서드로 전달할 수 있는 익명 함수를 단순화한 것이다. 람다 표현식에는 이름은 없지만, 파라미터 리스트, 바디, 반환 형식, 발생할 수 있는 예외 리스트를 가질 수 있다.

### 특징

- 익명 - 보통의 메서드와 달리 이름이 없으므로 익명이라 표현한다.
- 함수 - 람다는 메서드처럼 특정 클래스에 종속되지 않으므로 함수라고 부른다.
- 전달 - 람다 표현식을 메서드 인수로 전달하거나 변수로 저장할 수 있다.
- 간결성 - 익명 클래스처럼 많은 자질구레한 코드를 구현할 필요가 없다.

### 구성

```java
// 람다 파라미터     |   화살표     |   람다 바디
(Apple a1, Apple a2) -> a.getWeight().compreTo(a2.getWeight);
```

- 파라미터 리스트 - Comprator의 compare 메서드 파라미터(사과 두개)
- 화살표 - 화살표(->)는 람다의 파라미터 리스트와 바디를 구분한다.
- 람다 바디 - 두 사과의 무게를 비교한다. 람다의 반환값에 해당하는 표현식이다.



### 3.2 어디에, 어떻게 람다를 사용할까?

### 함수형 인터페이스

함수형 인터페이스는 정확히 하나의 추상 메서드를 지정하는 인터페이스다. 인터페이스는 디폴트 메서드를 포함할 수 있다. 많은 디폴트 메서드가 있더라도 추상 메서드가 오직 하나면 함수형 인터페이스다. 람다 표현식으로 함수형 인터페이스의 추상 메서드 구현을 직접 전달할 수 있으므로 전체 표현식을 함수형 인터페이스의 인스턴스로 취급할 수 있다.

@FunctionallInterface는 함수형 인터페이스임을 가리키는 어노테이션이다. 해당 어노테이션으로 인터페이스를 선언했지만 실제로 함수형 인터페이스가 아니면 컴파일러가 에러를 발생시킨다.

### 함수 디스크립터

함수형 인터페이스의 추상 메서드 시그니처는 람다 표현식의 시그니처를 가리킨다. 람다 표현식의 시그니처를 서술하는 메서드를 함수 디스크립터라고 부른다. 예를 들어 함수형 인터페이스 Comparator의 compare 메서드의 함수 디스크립터는 (T, T) -> int 이다.



### 3.4 함수형 인터페이스 사용

자바8 라이브러리 설계자들은 java.util.function 패키지로 여러 가지 새로운 함수형 인터페이스를 제공한다.

### 3.4.1 Predicate

`test` 라는 추상 메서드를 정의하며 `test` 라는 제네릭 형식 T 객체를 인수로 받아 `boolean` 을 반환한다.

```java
public interface Predicate<T> {
  boolean test(T t);
}
```

```java
private static <T> List<T> filter(List<T> list, Predicate<T> predicate) {
  List<T> results = new ArrayList<>();
  for (T t : list) {
    if (predicate.test(t)) {
      results.add(t);
    }
  }
  return results;
}

List<String> listOfStrings = Arrays.asList(
  "abc",
  ""
);
Predicate<String> nonEmptyStringPredicate = (String str) -> !str.isEmpty();
List<String> nonEmpty = filter(listOfStrings, nonEmptyStringPredicate);
for (String str : nonEmpty) {
  System.out.println(str);
}
```

```
abc
```

### 3.4.2 Consumer

제네릭 형식 T 객체를 받아서 `void` 를 반환하는 `accept` 라는 추상 메서드를 정의한다.

```java
@FuncationalInterface
public interface Consumer<T> {
	void accept(T t);
}
```

```java
private static <T> void forEach(List<T> list, Consumer<T> consumer) {
  for (T t : list) {
    consumer.accept(t);
  }
}

forEach(
  Arrays.asList(1, 2, 3, 4, 5),
  (Integer i) -> System.out.println(i)
);
```

```
1
2
3
4
5
```

### 3.4.3 Function

제네릭 형식 T를 인수로 받아서 제네릭 형식 R 객체를 반화하는 추상 메서드 `apply`를 정의한다.

```java
@FuncionalInterface
public interface Function<T, R> {
	R apply(T t);
}
```

```java
public static <T, R> List<R> map(List<T> list, Function<T, R> f) {
    List<R> result = new ArrayList<>();
    for (T t : list) {
        result.add(f.apply(t));
    }
    return result;
}

List<Integer> list = map(
  Arrays.asList("lambdas", "in", "action"),
  (String s) -> s.length()
);
for (Integer i : list) {
  System.out.println(i);
}
```

```
7
2
6
```

### 기본형 특화

제네릭의 내부 구현 때문에 제네릭 파라미터에는 참조형 형식만 사용할 수 있다. 

자바에서는 오토박싱 기능을 제공하는 데, 이러한 과정에는 비용이 소모된다.

자바8에서는 기본형을 입출력으로 사용하는 상황에서 오토박싱 동작을 피할 수 있도록 특별한 버전의 함수형 인터페이스를 제공한다. 예를 들어 IntPredicate는 1000이라는 값을 박싱하지 않지만, Predicate<Integer> 는 1000이라는 값을 Integer 객체로 박싱한다.

일반적으로 특정 형식을 입력으로 받는 함수형 인터페이스의 이름 앞에는 DoublePredicate, IntConsumer, LongBinaryOperator, IntFunction 처럼 형식명이 붙는다.

```java
public interface IntPredicate {
  boolean test(int t);
}

IntPredicate evenNumbers = (int i) -> i % 2 == 0;
evenNumbers.test(1000);

Predicate<Integer> oddNumbers = (Integer i) -> i % 2 != 0;
oddNumbers.test(1000);	// 박싱
```



### 3.5 형식 검사, 형식 추론, 제약

람다 표현식 자체에는 어떤 함수형 인터페이스를 구현하는지의 정보가 포함되어 있지 않다. 따라서 람다 표현식을 더 제대로 이해햐라면 람다의 실제 형식을 파악해야 한다.

### 3.5.1 형식 검사

람다가 사용되는 콘텍스트(context)를 이용해서 람다의 형식(type)을 추론할 수 있다. 어떤 콘텍스트에서 기대되는 람다 표현식의 형식을 `대상형식(target tpye)` 이라고 부른다. 형식 황식 과정은 다음과 같은 순서로 진행된다.

1. 메서드의 선언을 확인한다.
2. 메서드는 두 번째 파라미터로 대상 형식을 기대한다.
3. 인터페이스의 추상 메서드는 무엇인지 확인한다.
4. 함수 디스크립터를 묘사한다.
5. 메서드로 전달된 인수의 요구사항을 만족하는지 확인한다.

### 3.5.2 같은 람다, 다른 함수형 인터페이스

대상 형식(target typing) 이라는 특징 때문에 같은 람다 표현식이라더라도 호환되는 추상 메서드를 가진 다른 함수형 인터페이스로 사용될 수 있다.

```java
Callable<Integer> c = () -> 42;
PrivilegedAction<Integer> p () -> 42;

Comparator<Apple> c1 = (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());
ToIntBiFunction<Apple> c2 = (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());
BiFunction<Apple> c3 = (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());
```



### 3.5.3 형식 추론

자바 컴파일러는 람다 표현식이 사용된 콘텍스트(대상 형식)를 이용해서 람다 표현식과 관련된 함수형 인터페이스를 추론한다. 즉, 대상 형식을 이용해서 함수 디스크립터를 알 수 있으므로 컴파일러는 람다의 시그니처도 추론할 수 있다. 결과적으로 컴파일러는 람다 표현식의 파라미터 형식에 접근할 수 있으므로 람다 문법에서 이를 생략할 수 있다.

```java
// 형식 추론을 하지 않음
Comparator<Apple> c =	(Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());

// 형식을 추론함
Comparator<Apple> c = (a1, a2) -> a1.getWeight().compareTo(a2.getWeight());
```

### 3.5.4 지역 변수 사용과 제약

람다 표현식에서는 익명 함수가 하는 것처럼 자유 변수(파라미터로 넘겨진 변수가 아닌 외부에서 정의된 변수)를 활용할 수 있다. 이와 같은 동작을 `람다 캡처링` 이라고 한다.

자유 변수는 명시적으로 `final` 로 선언되어 있어야 하거나 실질적으로 final로 선언된 변수와 똑같이 사용되어야 한다. 즉, 람다 표현식은 한 번만 할당할 수 있는 지역 변수만 캡처할 수 있다.

```java
int portNumber = 1337;
Runnable r = () -> System.out.println(portNumber);
portNumber = 3333; // 컴파일 에러 발생
```

### 왜 이런 제약이 생겼는가?

1. 인스턴스 변수 : 힙에 저장
2. 지역 변수 : 스택에 저장

람다에서 지역 변수에 바로 접근할 수 있다는 가정하에 람다가 스레드에서 실행된다면 변수를 할당한 스레드가 사라져서 `변수 할당이 해제` 되었는데도 람다를 실행하는 스레드에서는 해당 변수에 `접근`하려 할 수 있다.

자바 구현에서는 원래 변수에 접근을 허용하는 것이 아니라 자유 지역 변수의 `복사본` 을 제공한다. 따라서 복사본의 값이 바뀌지 않아야 하므로 지역 변수에는 한 번만 값을 할당해야 한다는 제약이 생긴 것이다.

### 3.6 메서드 참조

명시적으로 메서드 명을 참조함으로써 `가독성을 높일 수 있다.` 메서드 참조는 메서드명 앞에 `구분자(::)`를 붙이는 방식으로 사용할 수 있다.

### 메서드 참조를 만드는 방법

1. 정적 메서드 참조

​	Integer의 parseInt 메서드는 `Integer::parseInt` 로 표현할 수 있다.

2. 다양한 형식의 인스턴스 메서드 참조

​	String의 length 메서드는 `String::length` 로 표현할 수 있다.

3. 기존 객체의 인스턴스 메서드 참조

​	Transaction 객체를 할당받은 expensiveTransaction 지역 변수가 있고, Transaction 객체에는 getValue 메서드가 있다면, `expensiveTransaction::getValue` 라고 표현할 수 있다.

### 생성자 참조

`ClassName::new` 처럼 클래스명과 new 키워드를 이용해서 기존 생성자의 참조를 만들 수 있다.

```java
Supplier<Apple> c1 = Apple::new;
Apple a1 = c1.get();

Supplier<Apple> c1 = () -> new Apple();
Apple a1 = c1.get();
```

```java
Function<Integer, Apple> c2 = Apple::new;
Apple a2 = c2.apply(100);

Function<Integer, Apple> c2 = (weight) -> new Apple(weight);
Apple a2 = c2.apply(100);
```

```java
List<Integer> weights = Arrays.asList(7, 3, 4, 10);
List<Apple> apples = map(weights, Apple::new);

public static <T, R> List<R> map(List<T> list, Function<T, R> f) {
    List<R> result = new ArrayList<>();
    for (T t : list) {
        result.add(f.apply(t));
    }
    return result;
}
```



### 3.7 람다, 메서드 참조 활용하기

1. 1단계 : 코드 전달

   ```java
   void sort(Comparator<? super E> c)
   ```

   ```java
   public class AppleComparator implements Comparator<Apple> {
     
     @Override
     public int compare(Apple a1, Apple a2) {
       return a1.getWeight() - a2.getWeight();
     }
   }
   ```

2. 2단계 : 익명 클래스 사용

   ```java
   inventory.sort(new Comparator<>() {
     @Override
     public int compare(Apple a1, Apple a2) {
       return a1.getWeight() - a2.getWeight();
     }
   });
   ```

3. 3단계 : 람다 표현식 사용

   ```java
   inventory.sort((a1, a2) -> a1.getWeight() - a2.getWeight());
   ```

4. 4단계 : 메서드 참조 사용

   ```java
   inventory.sort(Comparator.comparingInt(Apple:getWeight));
   ```



### 3.8 람다 표현식을 조합할 수 있는 유용한 메서드

자바8 API의 몇몇 함수형 인터페이스는 다양한 유틸리티 메서드를 포함한다. 이것은 `디폴트 메서드(default method)` 로 제공된다.

### 3.8.1 Comparator

```java
Comparator<Apple> c = Comparator.comparing(Apple::getWeight);
```

역정렬

```java
apples.sort(Comparator.comparingInt(Apple::getWeight).reversed());
```

Comperator 연결 사용

```java
apples.sort(Comparator.comparingInt(Apple::getWeight)
			.reversed()
			.thenComparing(Apple::getColor));
```

### 3.8.2 Predicate

```java
List<Apple> apples = Arrays.asList(
  new Apple(Color.RED, 100),
  new Apple(Color.RED, 200),
  new Apple(Color.GREEN, 100),
  new Apple(Color.RED, 150),
  new Apple(Color.GREEN, 500)
);
```



negate(not 연산)

```java
Predicate<Apple> notRedApple = redApple.negate();

List<Apple> notRedApples = filter(apples, notRedApple);

for (Apple apple : notRedApples) {
  System.out.println(apple);
}
```

```
Apple{color=GREEN, weight=500}
Apple{color=GREEN, weight=100}
```

and, or

```java
Predicate<Apple> redAndHeavyAppleOrGreen = redApple.and(apple -> apple.getWeight() > 150)
				.or(apple -> Color.GREEN.equals(apple.getColor()));

List<Apple> redAndHeavyOrGreenApples = filter(apples, redAndHeavyAppleOrGreen);

for (Apple apple : redAndHeavyOrGreenApples) {
  System.out.println(apple);
}
```

```
Apple{color=GREEN, weight=500}
Apple{color=RED, weight=200}
Apple{color=GREEN, weight=100}
```

### 3.8.3 Function

andThen - 주어진 함수를 먼저 적용한 결과를 다른 함수의 입력으로 전달하여 결과값 계산

```java
Function<Integer, Integer> f = x -> x + 1;
Function<Integer, Integer> g = x -> x * 2;
Function<Integer, Integer> h = f.andThen(g);
int result = h.apply(1);
System.out.println(result);
```

```
4
```

compose - 인수로 주어진 함수를 먼저 실행한 다음에 그 결과를 외부 함수의 인수로 제공하여 계산

```java
Function<Integer, Integer> f = x -> x + 1;
Function<Integer, Integer> g = x -> x * 2;
Function<Integer, Integer> h = f.compose(g);
int result = h.apply(1);
System.out.println(result);
```

```
3
```

