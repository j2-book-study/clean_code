# 의미 있는 이름

## 의도를 분명히 밝혀라

- 의도가 분명하게 이름을 지어라
- 따로 주석이 필요하다면 의도를 분명히 드러내지 못했다는 말이다.

```java
int d; // 경과 시간 (단위 : 날짜)

int elapsedTimeInDays;
int daysSinceCreation;
int daysSinceModification;
int fileAgeInDays;
```

- 이름이 확실하지 않으면 어떤 행동을 하는지 명확하지 않다.

```java
public List<Cell> getFlaggedCells() {
  List<Cell> flaggedCells = new ArrayList<Cell>();
  for (Cell cell : gameBoard) {
    if (cell.isFlagged()) {
      flaggedCells.add(cell);
    }
  }
  return flaggedCells;
}
```

- 위 코드처럼 명확한 이름을 사용한다면 어떤 일을 하는 함수인지 이해하기 쉬워진다.

## 그릇된 정보를 피하라

- List라는 이름은 컬렉션을 연상케 할 수 있다.
    - 실제로 List 컬렉션이라도 사용하지 않는 편이 바람직하다.
- 유사한 개념은 유사한 표기법을 사용한다.
    - 일관성이 떨어지는 표기법은 그릇된 정보다.

## 의미 있게 구분하라

- 연속된 숫자를 붙이거나
    - source와 destinatino과 같은 방식을 활용
- 불용어를 추가하는 방식은 적절치 못하다.
    - Product에 Info 혹은 Data를 붙이면?
    - a, an, the와 마찬가지로 의미가 불분명하다
    - 완전 사용하지 말라는 의미가 아니라..
    - 그 불용어가 의미를 전달 할 수 있어야 한다.
    - customerInfo와 customer
    - accountData와 account
    - 이들의 차이를 알기 힘들다.

## 발음하기 쉬운 이름을 사용하라

## 검색하기 쉬운 이름을 사용하라

- 이름 길이는 범위 크기에 비례해야 한다.
    - 여러 곳에서 사용된다면 검색하기 쉬운 이름이 바람직

```java
int realDaysPerIdealDay = 4;
const int WORK_DAYS_PER_WEEK = 5;
int sum = 0;
for (int j=0; j < NUMBER_OF_TASKS; j++) {
  int realTaskDays = taskEstimate[j] * realDaysPerIdealDay ;
  int realTaskWeeks = (realTaskDays / WORK_DAYS_PER_WEEK );
  sum += realTaskWeeks;
}
```

## 인코딩을 피하라

```java
PhoneNumber phoneString;
// 타입이 바뀌어도 이름은 바뀌지 않는다.
```

## 멤버 변수 접두어

```java
public class Part {
  String description;
}
```

### 인터페이스 클래스와 구현 클래스

- ShapeFactory ⇒ ShpaeFactoryImpl
- IShapeFactory는 XX

## 자신의 기억력을 자랑하지 마라

- for 문에서 i, j, k와 같은 전통적인 것은 괜찮다.
- 하지만 a와 b를 사용했으니 c를 사용한다는 논리는 최악

## 클래스 이름

- 명사/명사구
- Manager, Processor, Data, Info는 적절치 않다

## 메서드 이름

- 동사/동사구
- 생성자는 정적 팩토리 메서드를 사용한다.
    - 생성자는 private로 선언

```java
Complex fulcrumPoint = Complex.FromRealNumber(23.0);
```

## 기발한 이름은 피하라

- 농담 X

## 한 개념에 한 단어를 사용하라

- 일관성 있는 어휘를 사용해야 한다.
    - 그렇지 않다면 독자는 당연히 클래스도 다르고 타입도 다르다고 생각할 것이다.

## 말장난 하지 마라

- 맥락이 같았을 때 일관성을 고려하는 것이다.
- add가 많다고 맥락 상 insert인 메서드를 add로 해야할까?
    - 아니다. 무조건 이해하기 쉬운 코드가 좋다.

## 해법 영역에서 가져온 이름을 사용하라

- 모든 이름을 도메인에서 가져오는 정책은 현명하지 못하다.
- 기술 개념에는 기술 이름이 가장 적합하다.

## 문제 영역에서 가져온 이름을 사용하라.

- 적절한 ‘프로그래머 용어’가 없다면 문제 영역에서 가져온다.
- 코드를 보수할 때 분야 전문가에게 의미를 물어 파악할 수 있다.

## 의미 있는 맥락을 추가하라

- state 만을 보면 무슨 state인지 모른다.
- 이 때 맥락을 추가해서 addrState 로 해주면 된다.
- 가장 좋은 방버은 Address 객체에 state를 추가하는 것이다.

## 불필요한 맥락을 없애라

- 특정 도메인의 애플리케이션이라 해서 그 도메인의 접두사를 붙이는 것은 바람직하지 못하다.