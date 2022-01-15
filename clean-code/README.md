## 2장 - 의미있는 이름

- 의도를 명확하게 밝혀라, 함축하지 마라

- 약어를 사용하지 마라
- 여러 계정을 그룹으로 묶을 때, List를 이름 뒤에 명명하지 마라
  - accoutList -> accoutGroup, bunchOfAccounts, accounts
  - 실제 컨테이너가 List인 경우라도 컨테이너 유형을 이름에 넣지 않는 편이 바람직 함
- 차이를 알기 어렵게 구분하지 마라
  - customerInfo vs customer, accountData vs account, theMessage vs message  ( X )

- 인코딩을 피하라

  - 타입이나 멤버 변수를 나타내는 접두어를 붙이지 마라
  - 인터페이스 이름은 접두어를 붙이지 않는 편이 좋다
    - IShapeFactory -> ShapeFactoryImp

- 클래스 이름과 객체 이름은 명사나 명사구가 적합하다

  - 좋은 예 : Customer, WikiPage, Account, AddressParser
  - Manager, Proccessor, Data, Info 등과 같은 단어는 피해라
  - 동사는 사용하지 않는다

- 메서드 이름은 동사나 동사구가 적합하다

  - 좋은 예 : postPayment, deletePage, save
  - 접근자, 변경자, 조건자는 javabean 표준에 따른다.
    - get, set, is

- 생성자를 overload 할 때는 정적 팩토리 메서드를 사용한다.

  ```java
  // 변경 전
  Complex fulcrumPoint = new Complex(23.0);
  
  // 정적 팩토리 메서드 사용
  Complex fulcrumPoint = Complex.FromRealNumber(23.0);
  ```

- 한 개념에 한 단어를 사용하라
  - (fetch, retrieve, get) , (controller, manager, driver) 처럼 섞어 쓰지 마라

## 3장 - 함수

- 작게 만들어라
  - 들여쓰기 수준을 2단을 넘어서지 마라
- 한 가지만 해라
  - 의미 있는 이름으로 다른 함수를 추출할 수 있다면 그 함수는 여러 작업을 하는 셈이다.
- 서술적인 이름을 사용하라
  - 길어도 괜찮다
  - 일관성이 있어야한다
- 오류 코드보다 예외를 사용하라
- 반복하지 마라

## 5장 - 형식 맞추기

코드 형식은 의사소통의 일환이다. 오늘 구현한 코드의 가독성은 앞으로 바뀔 코드의 품질에 지대한 영향을 미친다. 오랜 시간이 지나 많은 부분 코드가 바뀌어도 맨 처음 잡아놓은 구현 스타일과 가독성 수준은 유지보수 용이성과 확장성에 계속 영향을 미친다. 따라서 원활한 소통을 유지하는 코드 형식이 필요하다.

- 적절한 행 길이를 유지하라
- 개념은 빈 행으로 분리하라
  - 패키지 선언부, import, 각 함수 사이
- 밀접한 코드 행은 가까이 놓아라
- 호출하는 함수를 호출되는 함수보다 먼저 배치하라
- 변수는 사용하는 위치에 최대한 가까이 선언하라
- 인스턴스 변수는 클래스 맨 처음에 선언하라