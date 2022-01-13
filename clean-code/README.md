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