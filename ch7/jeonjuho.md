# 오류 처리

## 오류 코드보다 예외를 사용하라

- 기능과 오류 처리를 분리한다.

## Try-Catch-Finally 문부터 작성하라

- 예외가 발생할 코드의 시작은 try-catch-finally 부터 시작하라.
    - try 블록에서 무슨 일이 생길지 호출자가 기대하는 상태를 정의하기 쉬워진다.

## Unchecked 예외를 사용하라

- 확인된 예외는 OCP를 위반한다.
    - 예외가 발생된 하위부터 상위까지 모든 함수에서 수정이 일어날 수 있다.
- 어떤 예외를 던지는지 알아야 하므로 캡슐화가 깨진다.

## 예외에 의미를 제공하라

- 오류가 발생한 원인과 위치를 찾기가 쉬워진다.
- catch 블록에서 오류를 기록하도록 충분한 정보를 넘겨준다.

## 호출자를 고려해 예외 클래스를 정의하라

- 오류를 정의할 때 오류를 잡아내는 방법이 가장 중요한 관심사이다.
- 대상 클래스가 던지는 예외를 잡아 변환하는 래퍼 클래스를 사용한다.
- 장점
    - 테스트에 용이하다.
    - 내부적으로 다른 기능을 사용하더라도 변경 비용이 저렴하다.
    - 특정 기능의 설계에 발목잡히지 않는다.

## 정상 흐름을 정의하라

```java
try {
  MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
  m_total += expenses.getTotaK);
} catch(MealExpensesNotFound e) {
  m_total += getMealPerDiem();
}
```

- 예외가 논리를 따라가기 어렵게 만든다.

```java
MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
m_total += expenses.getTotaK);

---
public class PerDiemMealExpenses implements MealExpenses {
  public int getTotaK) {
    // 기본값으로 일일 기본 식비를 반환한다.
  }
}
```

- 특수 사례 패턴이라 불린다.
- 클래스나 객체가 예외적인 상황을 캡슐화해서 처리하므로, 클라이언트 코드가 예외적인 상황을 처리할 필요가 없어진다.

## null을 반환하지 마라

- null을 반환하는 것은 호출자에게 문제를 떠넘기는 것이다.
- 누구 하나가 null 확인을 빼먹으면 애플리케이션이 통제에서 벗어난다.

```java
List<Employee> employees = getEmployees();
for(Employee e : employees) {
  totalPay += e.getPay();
}

---
public List<Employee> getEmployees() {
  if ( .. 직원이 없다면 ・ ・ )
    return Collections.emptyList();
}
```

## null을 전달하지 마라

- 문서화가 잘 되어 코드가 읽기 편하더라도 누군가 null을 전달하면 여전히 실행 오류가 발생한다.

## 결론

- 깨끗한 코드는 읽기도 좋아야 하지만 안정성도 높아야 한다.
- 오류 처리를 프로그램 논리와 분리하면 독립적인 추론이 가능해지며 코드 유지보수성도 크게 높아진다.