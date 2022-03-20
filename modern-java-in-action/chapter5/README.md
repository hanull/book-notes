# CHAPTER 5 : 스트림 활용

## 5.1 필터링

### 5.1.1 프레디케이트로 필터링

- filter

  프레디케이트(불리언을 반환하는 함수)를 인수로 받아서 프레디케이트와 일치하는 모든 요소를 포함하는 스트림을 반환한다.

  ```java
  List<Dish> vegetarianMenu = menu.stream()
  			.filter(Dish::isVegetarian)
  			.collect(Collectors.toList());
  ```

### 5.1.2 고유 요소 필터링

- distinct

  메서드는 고유 요소로 이루어진 스트림을 반환한다. 고유 여부는 스트림에서 만든 개게의 `hashCode`, `equeals` 로 결정된다.

  ```java
  List<Integer> numbers = Arrays.asList(1, 2, 1, 3, 3, 2, 4);
  		numbers.stream()
  			.distinct()
  			.forEach(System.out::println);
  ```

## 5.2 스트림 슬라이싱

### 5.2.1 프레디케이트를 이용한 슬라이싱

- TAKEWHILE

  해당 조건이 만족할 때까지 반복 작업을 수행하고, 만족하지 않을 시 반복을 종료한다.

  ```java
  private static List<Dish> menu = Arrays.asList(
    new Dish("계란", true, 100, Type.MEAT),
    new Dish("샐러드", true, 300, Type.OTHER),
    new Dish("피자", false, 6000, Type.OTHER),
    new Dish("치킨", false, 10000, Type.MEAT)
  );
  ```

  ```java
  List<Dish> slicedMenu = menu.stream()
    .takeWhile(dish -> dish.getCalorie() < 350)
    .collect(Collectors.toList());
  
  for (Dish dish : slicedMenu) {
    System.out.println(dish);
  }
  ```

  ```java
  Dish{name='계란', vegetarian=true, calorie=100, type=MEAT}
  Dish{name='샐러드', vegetarian=true, calorie=300, type=OTHER}
  ```

- DROPWHILE

  프레디케이트가 처음으로 거짓이 되면 그 지점에서 작업을 중단하고, 발견된 요소를 버린 후 남은 모든 요소를 반환한다.

  ```java
  List<Dish> slicedMenu = menu.stream()
    .dropWhile(dish -> dish.getCalorie() < 350)
    .collect(Collectors.toList());
  
  for (Dish dish : slicedMenu) {
    System.out.println(dish);
  }
  ```

  ```java
  Dish{name='피자', vegetarian=false, calorie=6000, type=OTHER}
  Dish{name='치킨', vegetarian=false, calorie=10000, type=MEAT}
  ```

### 5.2.2 스트림 축소

- limit

  프레디케이트와 일치하는 처음 n개 요소를 선택하여 반환한다. n개보다 적을 때 존재하는 수많큼 반환

  ```java
  List<Dish> dishes = menu.stream()
  			.filter(dish -> dish.getCalorie() < 400)
  			.limit(3)
  			.collect(Collectors.toList());
  ```

- skip

  처음 n개 요소를 제외한 스트림을 반환한다.

  ```java
  List<Dish> dishes = menu.stream()
  			.filter(dish -> dish.getCalorie() < 400)
  			.skip(1)
  			.collect(Collectors.toList());
  ```

## 5.3 매핑

### 5.3.1 스트림의 각 요소에 함수 적용하기

- map

  인수로 제공된 함수는 각 요소에 적용되며, 함수를 적용한 결과가 새로운 요소로 매핑된다. 이 과정은 '새로운 버전을 만든다'라는 개념에 가까우므로 변환에 가까운 `매핑` 이라는 단어를 사용한다.

  ```java
  // 단어 리스트가 주어졌을 때 각 단어가 포함하는 글자수 리스트 반환
  List<String> words = Arrays.asList("Modern", "java", "in", "action");
  List<Integer> wordLengths = words.stream()
    .map(String::length)
    .collect(Collectors.toList());
  ```

  ```java
  // 요리 리스트가 주어졌을 때 각 요리명의 길이 리스트 반환
  List<Integer> menuLengths = menu.stream()
    .map(Dish::getName)
    .map(String::length)
    .collect(Collectors.toList());
  ```

### 5.3.2 스트림 평면화

- flatMap

  제공된 함수를 각 요소에 적용하여 새로운 스트림의 콘텐츠로 매핑한다. 즉, 하나의 평면화된 스트림을 반환한다.

  ```java
  // 리스트에서 고유 문자로 이루어진 리스트 반환하기
  List<String> words = Arrays.asList("Modern", "java", "in", "action");
  		
  List<String> characters = words.stream()
    .map(word -> word.split(""))
    .flatMap(Arrays::stream)
    .distinct()
    .collect(Collectors.toList());
  ```

  ```java
  // 두 개의 숫자 리스트를 이용해서 숫자 쌍 만들기
  // 단, 숫자 쌍의 합이 3으로 나누어 떨어져야 한다.
  List<Integer> numbers1 = Arrays.asList(1, 2, 3);
  List<Integer> numbers2 = Arrays.asList(3, 4);
  
  List<int[]> pairs = numbers1.stream()
    .flatMap(i ->
        numbers2.stream()
           .filter(j -> (i + j) % 3 == 0)
           .map(j -> new int[] {i, j})
    )
    .collect(Collectors.toList());
  ```

## 5.4. 검색과 매칭

- anyMatch

  프레디케이트가 주어진 스트림에서 적어도 한 요소와 일치하는지 확인한다. anyMatch는 불리언을 반환하므로 최종 연산이다.

  ```java
  if (menu.stream().anyMatch(Dish::isVegetarian)) {
    System.out.println("The menu is vegetarian friendly!");
  }
  ```

- allMatch

  모든 요소가 주어진 프레디케이트와 일치하는지 검사한다.

  ```java
  boolean isHealthy = menu.stream().allMatch(dish -> dish.getCalorie() < 100000);
  ```

- noneMatch

  allMatch와 반대 연산을 수행한다. 즉, 주어진 프레디케이트와 일치하는 요소가 없는지 확인한다.

  ```java
  boolean isHealthy = menu.stream().noneMatch(dish -> dish.getCalorie() >= 100000);
  ```

- findFirst, findAny

  첫 번째 요소를 찾아 반환한다. 
  두 메서드는 병렬로 처리할 때 차이점이 존재한다. findFirst는 병렬로 처리할 때도 무조건 스트림의 첫 요소를 반환하지만, findAny는 반환 순서를 보장하지 않고 발견한 첫 번째 요소를 반환한다. 

  ```java
  List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
  Optional<Integer> first = numbers.stream()
    .map(n -> n * n)
    .filter(n -> n % 3 == 0)
    .findFirst();
  ```

## 5.5 리듀싱

모든 스트림 요소를 처리해서 값으로 도출하는 것을 `리듀싱 연산` 이라고 한다.

```java
int sum = numbers.stream().reduce(0, (a, b) -> a + b);
```

초기값을 받지 않는 reduce는 `Optional` 객체를 반환한다.

```java
Optional<Integer> sum = numbers.stream().reduce((a, b) -> a + b);
```

최대값, 최소값을 구할 수 있다.

```java
Optional<Integer> max = numbers.stream().reduce(Integer::max);
Optional<Integer> min = numbers.stream().reduce(Integer::min);
```



### reduce 메서드의 장점과 병렬화

reduce 를 이용하면 내부 반복이 추상화되면서 내부 구현에서 병렬로 reduce를 실행할 수 있게 된다. 반복적인 합계에서는 sum 변수를 공유해야 하므로 쉽게 병렬화하기 어렵다. 병렬 스트림은 내부적으로 포크/조인 프레임워크를 통해 이를 처리한다.



### 스트림 연산 : 상태 없음, 상태 있음

각각의 스트림 연산은 상태를 갖는 연산과 상태를 갖지 않는 연산으로 나뉘어져 있다.

map, filter 등은 입력 스트림에서 각 요소를 받아 0 또는 결과를 출력 스트림으로 보낸다. 따라서 이들은 보통 상태가 없는, `내부 상태를 갖지 않는 연산(stateless operation)` 이다.

하지만 reduce, sum, max 같은 연산은 결과를 누적할 내부 상태가 필요하다. 스트림에서 처리하는 요소 수와 관계없이 내부 상태의 크기는 `한정(bounded)` 되어 있다.

반면 sorted, distinct 같은 연산은 스트림의 요소를 정렬하거나 중복을 제거하려면 과거의 이력을 알고 있어야 한다. 예를 들어 어떤 요소를 출력 스트림으로 추가하려면 `모든 요소가 버퍼에 추가되어 있어야 한다.` 따라서 데이터 스트림의 크기가 무한이라면 문제가 발생할 수 있다. 이러한 연산을 `내부 상태를 갖는 연산(stateful operation)` 이라 한다.



## 5.6 숫자형 스트림



## 5.7 스트림 만들기

