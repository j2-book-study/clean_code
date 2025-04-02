# 클래스

## 클래스 체계

클래스를 정의하는 표준 자바 관례에 따르면, 가장 먼저 변수 목록이 나온다.

정적(Static) 공개(Public) 상수가 있다면 맨 처음에 나온다.

다음으로 정적 비공개(Private) 변수가 나오며, 이어서 비공개 인스턴스 변수가 나온다.

## 캡슐화

변수와 유틸리티 함수는 가능한 공개하지 않는 편이 낫지만 반드시 숨겨야한다는 법칙도 없다.

## 클래스는 작아야한다.

클래스를 만들 때 첫 번째 규칙은 크기다! 클래스는 작아야 한다.

두 번째 규칙도 크기다! 더 작아야한다!

자바 내 SuperDashboard 클래스는 메서드 수가 작지만 불구하고 책임이 너무 많다.

클래스 이름은 해당 클래스 책임을 기술해야 한다.

실제로 작명은 클래스 크기를 줄이는 첫 번째 관문이다.

간결한 이름이 떠오르지 않는다면 필경 클래스 크기가 너무 커서 그렇다.

클래스 이름이 모호하다면 필경 클래스 책임이 너무 많아서이다.

### Ex

- `Processor`, `Manager`, `Super` 등과 같이 모호한 단어가 있다면 클래스에 여러 책임을 안겼다는 증거이다
- 또한 클래스 설명은 만일(”`if`”), 그리고(”`and`”), 또는(”`or`”), 하지만(”`but`”) 을 사용하지 않고서 25단어 내외로 가능해야 한다.

## 단일 책임 원칙

단일 책임 원칙(`Single Responsibility Principle`) 은 클래스나 모듈을 변경할 이유가 하나, 단 하나뿐이어야 한다는 원칙이다.

SRP 는 ‘책임’ 이라는 개념을 정의하며 적절한 클래스 크기를 제시한다.

클래스는 책임, 즉 변경할 이유가 하나여야 한다는 의미다.

그리고 책임, 즉 변경할 이유를 파악하려 애쓰다 보면 코드를 추상화하기도 쉬워진다.

더 좋은 추상화가 더 쉽게 떠오른다.

큰 클래스 몇 개가 아니라 작은 클래스 여럿으로 이뤄진 시스템이 더 바람직하다.

작은 클래스는 각자 맡은 책임이 하나며, 변경할 이유가 하나이며, 다른 작은 클래스와 협력해 시스템에 필요한 동작을 수행한다.

## 응집도

클래스는 인스턴스 변수 수가 작아야 한다.

각 클래스 메서드는 클래스 인스턴스 변수를 하나 이상 사용해야 한다.

일반적으로 메서드가 변수를 더 많이 사용할수록 메서드와 클래스는 응집도가 더 높다.

모든 인스턴스 변수를 메서드마다 사용하는 클래스는 응집도가 가장 높다.

응집도가 높다는 말은 클래스에 속한 메서드와 변수가 서로 의존하며 논리적인 단위로 묶인다는 의미기 때문이다.

### Ex

응집도가 높은 클래스

```java
public class Stack {
	private int topOfStack = 0;
	List<Integer> elements = new LinkedList<Integer>();
		
	public int size() {
			return topOfStack;
	}
		
	public void push(int element) {
			topOfStack++;
			elements.add(element);
	}
		
	public int pop() throws PoppedWhenEmpty {
		if (topOfStack == 0)
			throw new PoppedWhenEmpty();
			
		int element = elements.get(--topOfStack);
		elements.remove(topOfStack);
		return element
	}
}
```

‘함수를 작게, 매개변수 목록을 짧게’라는 전략을 따르다 보면 때때로 몇몇 메서드만이 사용하는 인스턴스 변수가 아주 많아진다.

이는 십중팔구 새로운 클래스로 쪼개야 한다는 신호다.

응집도가 높아지도록 변수와 메서드를 적절히 분리해 새로운 클래스 두세 개로 쪼개준다.

## 응집도를 유지하면 작은 클래스 여럿이 나온다.

큰 함수를 작은 함수 여럿으로 나누기만 해도 클래스 수가 많아진다.

한 쪽을 조금 넘겼던 코드 뭉텅이 프로그램이 정리 후에는 세 쪽으로 늘어났다.

길이가 늘어난 이유는 여러 가지다.

첫째, 리팩터링한 프로그램은 좀 더 길고 서술적인 변수 이름을 사용한다.

둘째, 리팩터링한 프로그램은 코드에 주석을 추가하는 수단으로 함수 선언과 클래스 선언을 활용한다.

셋째, 가독성을 높이고자 공백을 추가하고 형식을 맞추었다.

## 변경하기 쉬운 클래스

대다수 시스템은 지속적인 변경이 가해진다.

그리고 뭔가 변경할 때마다 시스템이 의도대로 동작하지 않을 위험이 따른다.

### Ex

변경해야 하는 클래스

```java
public class Sql {
	public sql(String table, Column[] columns)
	public String create()
	public String insert(Object[] fields)
	public String selectAll()
	public String findByKey(String keyColumn, String keyValue)
	public String select(Column column, String pattern)
	...
}
```

변경 클래스

```java
abstract public class Sql {
	public sql(String table, Column[] columns)
		abstract public String generate();
}

public class CreateSql extends Sql {
	public CreateSql(String table, Column[] columns)
	@Override public String generate()
}

public class InsertSql extends Sql {
	public InsertSql(String table, Column[] columns, Object[] fields)
	@Override public String generate()
	private String valuesList(Object[]] fields, final Column[] columns)
}

...
```

각 클래스는 극도로 단순하다.

코드는 순식간에 이해된다.

함수 하나를 수정해서 다른 함수가 망가질 위험도 사실상 사라졌다.

테스트 관점에서 모든 논리를 구석구석 증명하기도 쉬워졌다.

클래스가 서로 분리되었기 때문이다.

변경 클래스에는 객체 지향 설계에서 OCP 도 지원한다.

OCP 는 클래스는 확장에 개방적이고 수정에 폐쇄적이어야 한다는 원칙이다.

우리가 재구성한 Sql 클래스는 파생 클래스를 생성하는 방식으로 새 기능에 개방적인 동시에 다른 클래스를 닫아놓는 방식으로 수정에 폐쇄적이다.

## 변경으로부터 격리

결합도가 낮다는 소리는 각 시스템 요소가 다른 요소로부터 그리고 변경으로부터 잘 격리되어 있다는 의미이다.

시스템 요소가 서로 잘 격리되어 있으면 각 요소를 이해하기도 더 쉬워진다.

이렇게 결합도를 최소로 줄이면 자연스럽게 또 다른 클래스 설계 원칙인 DIP 를 따르는 클래스가 나온다.

본질적으로 DIP 는 클래스가 상세한 구현이 아니라 추상화에 의존해야 한다는 원칙이다.
