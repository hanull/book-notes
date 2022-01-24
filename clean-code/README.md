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

## 6장 - 객체와 자료구조

- 객체에서 자료를 세세하게 공개하기보다 추상화를 통해 표현하는 것이 더 좋다
- 객체는 동작을 공개하고 자료를 숨긴다
- 자료구조는 자료를 그대로 공개하며 별다른 함수는 제공하지 않는다
- 시스템을 구현할 때, 새로운 자료 타입을 추가하는 유연성이 필요하다면 객체가 더 적합하고, 새로운 동작을 추가하는 유연성이 필요하면 절차지향 코드와 자료구조 형태가 더 적합하다

## 7장 - 오류 처리

- 오류를 일일이 확인하는 것보다 예외를 던지는게 더 깔끔하다
- 미확인 예외(Unchecked Exception) 를 사용하라
  - 왜 ? 하위 메소드에서 확인된 예외(Checked Exception) 를 던진다면, 모든 상위 메소드에서 이 예외에 대한 throws 절을 추가해야 하는 상황이 발생한다. 따라서 OCP 규칙을 위반한다.
- 호출하는 메소드가 Exception을 활용해서  의미 있는 작업을 할 수 있다면, 확인 예외(Checked Exception을 사용하라.
- 만약 호출하는 메소드가 Exception을 catch해서 예외 상황을 해결하거나 문제를 해결할 수 없다면, 미확인 예외(Unchecked Exception) 을 사용하라. 명확하지 않다면 미확인 예외(Unchecked Exception) 을 사용하라.

- 예외에 의미있는 내용을 담아 오류가 발생한 원인, 위치를 파악하는 데 도움이 되게 하라

- 호출자를 고려해 예외 클래스를 정의하라

  ```java
   ACMEPort port = new ACMEPort(12);
  
   try {
       port.open();
   } catch (DeviceResponseException e) {
       reportPortError(e);
       logger.log("Device response exception", e);
   } catch (ATM1212UnlockedException e) {
       reportPortError(e);
       logger.log("Unlock exception", e);
   } catch (GMXError e) {
       reportPortError(e);
       logger.log("Device response exception");
   } finally {
       ...
   }
  ```

  - 위의 코드를 보면 매번 open()을 호출하는 곳에서 예외 처리를 해줘야 한다. 즉, 의존성이 높고 오류가 하나만 늘어도 해당 메소드를 호출하는 모든 곳을 찾아 추가해줘야 하는 문제가 있다.
  - 해당 문제를 별도의 클래스로 감싸서 예외를 처리하는 방법으로 해결할 수 있다. (아래 코드 확인)
    - 에러 처리가 간결해짐
    - 외부 라이브러리와 프로그램 사이의 의존성이 줄어듬
    - 프로그램 테스트가 쉬워짐

  ```java
  public class LocalPort {
  
    private ACMEPort innerPort;
  
    public void open() {
  		try { 
  			innerPort.open();
  		} catch (DeviceResponseException e) {
  			// 내부에서 약속된 하나의 에러 패턴을 사용.
  			throw new PortDeviceFailuer(e);
  	  } catch (NetworkErrorException e) { 
  			throw new PortDeviceFailuer(e);
  	  } catch (BindingException e) {
  			throw new PortDeviceFailuer(e);
  	  } catch ....
  	  }
  	}
  }
  ```

- null 을 반환하지 마라

  - 빈 리스트(`Collections.emptyList()`)와 같은 특수한 사례 객체를 만들어 반환한다.

  ```java
  List<Employee> emploees = getEmployees();
  if (employees != null) {
    for(Employee e : employees) {
      totalPay += e.getPay();
    }
  }
  
  // 수정 후
  List<Employee> emploees = getEmployees();
  for(Employee e : employees) {
    totalPay += e.getPay();
  }
  ```

- null 을 넘기지마라

## 8장 - 경계

Map과 같은 공개된 인터페이스를 사용할 때 문제점이 있다.

1. map 인스턴스를 전달받은 쪽에서 마음대로 추가하거나 삭제할 수 있다.
2. map 인스턴스를 여기저기로 넘긴다면, Map 인터페이스가 변할 경우, 수정할 코드가 상당히 많아진다.

### 해결 방법

1. `캡슐화` 한다.

```java
public class Sensors {
  private Map sensors = new HashMap();
  
  public Sensor getById(String id) {
    return (Sensor) sensors.get(id);
  }
}
```

- Map 인터페이스가 변하더라도 나머지 프로그램에는 영향을 미치지 않는다.
- 원하는 기능만 공개해 코드를 보호할 수 있다.
- Map 클래스를 사용할 때마다 캡슐화하라는 것은 아니다. Map 인스턴스를 공개 API의 인수로 넘기거나 반환값으로 사용하지 말라는 말이다.

2. `Adapter pattern` 을 사용한다. 

## **10장 - 클래스**

### **클래스 체계**

- Java Convention에 따르면 가장 먼저 변수 목록이 나온다. static public -> static private -> private 인스턴스 -> (public 은 필요한 경우가 거의 없다)
- 변수 목록 다음에는 공개 함수가 나온다. 비공개 함수는 자신을 호출하는 공개 함수 직후에 나온다. 즉, 추상화 단계가 순차적으로 내려간다.

### **클래스는 작아야 한다**

- 큰 클래스 몇개가 아니라 작은 클래스 여럿으로 이루어진 시스템이 더 바람직하다. 작은 클래스는 각자 맡은 책임이 하나며, 변경할 이뮤가 하나이고, 다른 작은 클래스와 협력해서 시스템에 필요한 동작을 수행한다.
- 클래스가 응집력을 잃는다면 쪼개라

## 11장 - 시스템

### 시스템 제작과 시스템 사용을 분리하라

모든 애플리케이션에서 관심사 분리는 중요한 부분이다.

제작은 사용과 아주 다른 개념이다. 소프트웨어 시스템은 준비 과정과 (준비 과정 이후에 이어지는) 런타임 로직을 분리해야 한다.

1. Main 분리
    
    생성과 관련한 코드는 모두 main 이나 main이 호출하는 모듈로 옮기고, 나머지 시스템은 모든 객체가 생성되었고 모든 의존성이 연결되었다고 가정한다.
    
2. 팩토리
    
    객체가 `생성되는 시점`을 애플리케이션이 결정할 필요가 있는 때, `ABSTRACT FACTORY` 패턴을 사용한다. 객체를 생성하는 시점은 애플리케이션이 결정하지만, 객체를 생성하는 코드는 main 이나 main이 호출하는 모듈에서 맡아 애플리케이션이 모르도록 한다.
    
3. 의존성 주입
    
    DI 컨테이너가 필요한 객체의 인스턴스를 만든 후 생성자 인수나 설정자 메서드를 사용해 의존성을 설정한다. 
    

### 확장

처음부터 올바르게 시스템을 맞출 수는 없다. 관심사를 적절하게 분리하고 관리해야 소프트웨어 아키텍처를 점진적으로 발전할 수 있다.

### 도메인 특화 언어를 사용하라

좋은 DSL(Domain-Specific-Language) 는 도메인 개념과 그 개념을 구현한 코드 사이에 존재하는 ‘의사소통 간극’을 줄여준다. DSL을 사용하면 도메인을 잘못 구현할 가능성이 줄어들고, 모든 추상화 수준과 모든 도메인을 POJO로 표현할 수 있다.

### 결론

- 시스템은 깨끗해아하고, 깨끗하지 못한 아키텍처는 도메인 논리를 흐리며 기민성을 떨어뜨린다. 도메인 논리가 흐려지면 제춤 품질이 떨어지고 버그가 숨어들기 쉬워진다.
- 모든 추상화 단계에서 의도는 명확히 표현해야 한다. 그러려면 POJO를 작성하고, 각 구현 관심사를 분리해야 한다.
- 시스템을 설계하든 개별 모듈을 설계하든, 실제로 돌아가는 가장 단순한 수단을 사용해야 한다.
