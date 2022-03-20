## 설계 원칙: SOLID

## 1. 단일 책임 원칙(Single responsibility principle)
> "어떤 클래스를 변경해야하는 이유는 오직 하나뿐이어야 한다" -로버트 C.마틴

객체 지향의 기본은 책임을 객체에게 할당하는 데 있다. 단일 책임 원칙은 `클래스는 단 한개의 책임을 가져야 한다`는 규칙을 가지고 있다. 

### 하나의 클래스가 다양한 책임을 가지면 어떤 문제점이 있을까?
만약 클래스가 여러 책임을 갖는 경우 `변경`이라는 관점에서 문제가 발생한다. 
- 프로그램의 요구사항은 계속해서 변하는데 이러한 요구사항의 변화는 각 책임을 갖는 클래스의 코드를 변경시켜야 한다는 뜻이다. 즉, 클래스가 다양한 책임을 가진다면 요구사항의 변화에 더 민감하게 대응해야 한다.
- 클래스 내부에서 서로 다른 역할을 수행하는 코드끼리 강하게 결합되게 되고, 작은 변화에도 많은 변경사항을 연쇄적으로 발생시킬 수 있다. 

책임의 수가 많아질수록 한 책임의 기능 변화가 다른 책임에 주는 영향은 비례해서 증가하게 되는데, 이는 결국 코드를 절차 지향적으로 만들어 변경을 어렵게 만든다.


## 2. 개방 폐쇄 원칙(Open-closed principle)
> "소프트웨어 엔티티(클래스, 모듈, 함수 등)는 확장에 대해서는 열려 있어야 하지만 변경에 대해서는 닫혀 있어야 한다." -로버트 C.마틴

위의 말을 구체적으로 풀어 보면 다음과 같다.
- 기능을 변경하거나 확장할 수 있으면서
- 그 기능을 사용하는 코드는 수정하지 않는다.

 - 요구사항이 변경되었을 때, 코드에서 변경되어야 하는 부분과 변경되지 않아야하는 부분을 명확하게 구분하여, 변경되어야 하는 부분을 유연하게 작성하는 것을 의미한다. 또한 확장에는 유연하게 반응하며 변경은 최소화하는 것을 의미한다.

아래의 코드는 랜덤 카드 생성기로 52장의 카드를 생성하는 코드이다. RandomCardDeckGenerator 는 생성하는 역할을 하고, 이를 CardDeck 에서 사용한다.

```java
public class RandomCardDeckGenerator {

    private static final List<Card> CARDS = initializeCardDeckByCardNumber();

    private static LinkedList<Card> initializeCardDeckByCardNumber() {
        LinkedList<Card> cards = new LinkedList<>();
        for (CardPattern cardPattern : CardPattern.values()) {
            addCardByPattern(cards, cardPattern);
        }
        return cards;
    }

    private static void addCardByPattern(LinkedList<Card> cards, CardPattern cardPattern) {
        for (CardNumber cardNumber : CardNumber.values()) {
            cards.add(new Card(cardPattern, cardNumber));
        }
    }

    public List<Card> generate() {
        List<Card> cards = new LinkedList<>(CARDS);
        Collections.shuffle(cards);
        return cards;
    }
}
```

```java
public class CardDeck {

    private final Queue<Card> cardDeck;

    public CardDeck(RandomCardDeckGenerator randomCardDeckGenerator) {
        this.cardDeck = new LinkedList<>(randomCardDeckGenerator.generate());
    }
}
```

그런데 만약 더 이상 랜덤으로 생성하는 RandomCardDeckGenerator 를 사용하지 않고, 1번부터 정렬된 카드를 생성하는 SortCardDeckGenerator 를 사용한다는 변경사항이 생긴다면 어떤일이 발생할까?

위에서 RandomCardDeckGenerator를 생성하는 코드를 SortCardDeckGenerator 생성으로 변경해야 한다. 이 과정에서 Production Code 를 변경하는 일이 발생하게 된다. 

이러한 문제를 해결하기 위해서 어떻게 해야할까?

- 변경되지 않아야하는 부분과 변경되는 부분을 명확하게 구분한다.
- 변경되어야 하는 부분을 추상화하여 분리한다.

여기서 변경되는 부분은 카드 생성 방법이 된다. generate 부분을 인터페이스로 추상화하여 분리하고, 유연하게 변경에 대응하면 된다.

```java
public interface CardDeckGenerator {
    List<Card> generate();
}
```

```java
public class SortCardDeckGenerator implements CardDeckGenerator {

    @Override
    public List<Card> generate() {}
    
}
```

```java
public class CardDeck {

    private final Queue<Card> cardDeck;

    public CardDeck(CardDeckGenerator cardDeckGenerator) {
        this.cardDeck = new LinkedList<>(candomCardDeckGenerator.generate());
    }
}
```

```java
private BlackjackGame setUpBlackjackGame() {
        final BlackjackGame blackjackGame = BlackjackGame.create(setUpPlayers(), new CardDeck(new SortCardDeckGenerator()));
}
```

## 3. 리스코프 치환 원칙(Liskov substitution principle)
> "서브 타입은 언제나 자신의 기반 타입으로 교체할 수 있어야 한다." -로버트 C.마틴"

리스코프 치환 원칙은 `다형성`에 관한 원칙을 제공한다.
- 상위 타입의 객체를 하위 타입의 객체로 치환해도 상위 타입을 사용하는 프로그램은 정상적으로 동작해야 한다.


### 리스코프 치환 원칙은 계약과 확장에 대한 것
- 하위 타입은 상위 타입에서 정의한 명세를 벗어나지 않는 범위에서 구현해야 한다.
- 리스코프 치환 원칙을 어기면 개방 폐쇄 원칙을 어길 가능성이 높아진다.


## 4. 인터페이스 분리 원칙(Interface segregation principle)
> "클라이언트는 자신이 사용하지 않는 메서드에 의존 관계를 맺으면 안 된다." -로버트 C.마틴

- 인터페이스는 그 인터페이스를 사용하는 클라이언트를 기준으로 분리해야 한다.

### 클라이언트 기준으로 분리되지 않았을 경우 문제점
만약, 인터페이스가 다양한 기능을 제공할 때, 인터페이스의 기능 중 몇개만 사용하기 위해 구현을 한 클라이언트가 있다면, 해당 클라이언트는 불필요한 기능까지 구현해야 한다. 그리고 인터페이스의 기능에 변경이 발생하면, 해당 클라이언트 역시 변경해야한다. 즉, 사용하지 않는 메서드를 위해 불필요한 비용이 발생하게 된다.

### 인터페이스 분리 원칙은 클라이언트에 대한 것
위와 반대로 클라이언트를 기준으로 인터페이스가 분리되어 있다면, 각 클라이언트에 맞게 기능을 구현할 수 있고 변경의 여파가 미치는 영향을 최소화할 수 있게 된다.


## 5. 의존 역전 원칙(Dependency inversion principle)
> "고차원 모듈은 저차원 모듈에 의존하면 안 된다. 이 두 모듈 모두 다른 추상화된 것에 의존해야 한다."
> "추상화된 것은 구체적인 것에 의존하면 안 된다. 구체적인 것이 추상화된 것에 의존해야 한다."
> "자주 변경되는 구체(Concrete)클래스에 의존하지 마라" -로버트 C.마틴

- 고수준 모듈은 저수준 모듈의 구현에 의존해서는 안 된다. 저수준 모듈이 고수준 모듈에서 정의한 추상 타입에 의존해야 한다.

- 고수준 모듈 : 어떤 의미 있는 단일 기능을 제공하는 모듈
- 저수준 모듈 : 고수준 모듈의 기능을 구현하기 위해 필요한 하위 기능의 실제 구현

추상화를 통해서 고수준 모듈을 만들고, 저수준 모듈을 상세 구현하여 사용한다. 추상화 해놓은 고수준 모듈을 기반으로 저수준 모듈을 유연하게 변경할 수 있다.


## 참고
- [SOLID 1편 SRP와 OCP](https://tecoble.techcourse.co.kr/post/2020-07-31-solid-1/)