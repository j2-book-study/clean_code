### 의도를 분명히 밝혀라
---
- 좋은 이름을 지으려면 시간이 걸리지만 좋은 이름으로 절약하는 시간이 훨씬 많다.
    - 코드를 읽는 사람이 좀 더 행복해지는 효과도 있다.

```java
public List<int[]> getThem() {
	List<int[]> list1 = new ArrayList<int[]>();
	
	for (int[] x : theList)
		if (x[0] == 4)
			list1.add(x)
	return list1;
```

- 위 문제는 코드의 단순성이 아니라 코드의 함축성
    - 코드 맥락이 코드 자체에 명시적으로 드러나있지 않는다.
        - theList 에 무엇이 들었는가?
        - theList 에서 0번째 값이 어째서 중요한가?
        - 값 4는 무슨 의미인가?
        - 함수가 반환하는 리스트 list1 을 어떻게 사용하는가?

- 위 코드는 지뢰찾기 게임 코드이다.

```java
public List<Cell> getFlaggedCells() {
	List<Cell> flaggedCells = new ArrayList<Cell>();
	
	for (Cell cell : gameBoard)
		if (cell.isFlagged())
			flaggedCells.add(cell)
	return flaggedCells;
```

- 단순히 이름만 고쳤는데 함수가 하는 일을 이해하기 쉬워졌다.
바로 이것이 좋은 이름이 주는 위력

### 그릇된 정보를 피하라
---
- 일관성이 떨어지는 표기법은 그릇된 정보이다.

### 의미 있게 구분하라
---
- 읽는 사람이 차이를 알도록 이름을 지어라
    - moneyAmount 는 money 와 구분이 안된다.
    - customerInfo 는 customer 와, accountData 는 account 와, theMessage 는 message 와 구분이 안된다.

### 발음하기 쉬운 이름을 사용하라
---
- 발음하기 쉬운 이름은 중요하다.

### 검색하기 쉬운 이름을 사용하라
---
- 이름을 의미있게 지으면 함수가 길어진다.
하지만 찾기가 얼마나 쉬운지 생각해보라.
ex) WORK_DAYS_PER_WEEK
    
    → 항상 고민했던 문제.. 이름이 길어져도 뜻을 명확히 할 것
    

### 인코딩을  피하라
---
- 유형이나 범위 정보까지 인코딩에 넣으면 그만큼 이름을 해독하기 어려워진다.

### 자신의 기억력을 자랑하지 마라
---
- 루프에서 반복 횟수 변수는 전통적으로 한 글자를 사용하기 때문에 괜찮지만, 그 외에는 대부분 적절하지 못하다.
    
    
- 클래스 이름
    - 클래스 이름과 객체 이름은 명사나 명사구가 적합하다.
        - Customer
        - WikiPage
        - Account
        - AddressParser

- 메소드 이름
    - 메소드 이름은 동사나 동사구가 적합하다.
        - postPayment
        - deletePage
        - save
    - 생성자를 중복정의할 때는 정적 팩토리 메소드를 사용한다.
    메소드는 인수를 설명하는 이름을 사용한다.
        - `Complex fulcrrumPoint = Complex.FromRealNumber(23.0);`
        → `Complex fulccrrumPoint = new Complex(23.0);`

### 기발한 이름은 피하라
---
- 이름이 너무 기발하면 저자와 유머 감각이 비슷한 사람만, 그리고 농담을 기억하는 동안만 이름을 기억한다.
- 의도를 분명하고 솔직하게 표현하라

### 한 개념에 한 단어를 사용하라
---
- 일관성 있는 어휘는 코드를 사용할 프로그래머가 반갑게 여길 선물이다.

### 말장난을 하지 마라
---
- 한 단어를 2가지 목적으로 사용하지 마라
- 프로그래머는 코드를 최대한 이해하기 쉽게 짜야 한다.
- 집중적인 탐구가 필요한 코드가 아니라 대충 훑어봐도 이해할 코드 작성이 목표다.

### 의미 있는 맥락을 추가해라
---
- `firstName`, `lastName`, `state` 라 사용하면 애매하지만,
`addrFirstName`, `addrlastName`, `addrState` 라 쓰면 맥락이 분명해진다.
