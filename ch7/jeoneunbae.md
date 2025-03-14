# 오류 처리

- 깨끗한 코드와 오류 처리는 확실히 연관성이 있다.
    
    상당수 코드 기반은 전적으로 오류 처리 코드에 좌우된다.
    
    여기서 좌우된다는 표현은 코드 기반이 오류만 처리한다는 의미가 아니며,
    여기저기 흩어진 오류 처리 코드 때문에 실제 코드가 하는 일을 파악하기가 거의 불가능하다는 의미다.
    

## 오류 코드보다 예외를 사용하라

- 오류가 발생하면 예외를 던지는 편이 낫다.
논리가 오류 처리 코드와 뒤섞이지 않으니 작성자 코드가 더 깔끔해진다.
    
    ```java
    public class DeviceController {
    	...
    	
    	public void sendShutDown() {
    		try {
    			tryToShutDown();
    		} catch (DeviceShutDownError e) {
    			logger.log(e);
    		}
    	}
    	
    	private void tryToShutDown() throws DeviceShutDownError {
    		DeviceHandle handle = getHandle(DEV1);
    		DeviceRecord record = retrieveDeviceRecord(handle);
    		
    		...
    	}
    }
    ```
    
- 코드가 확실히 깨끗하며 코드 품질 또한 나아졌다.
- 앞서 뒤섞였던 개념, 즉 디바이스를 종료하는 알고리즘과 오류를 처리하는 알고리즘을 분리했기 때문이다.

## Try-Catch-Finally 문부터 작성하라

- 예외에서 프로그램 안에다 범위를 정의한다는 사실은 매우 흥미롭다.
- `try-catch-finally` 문에서 try 블록에 들어가는 코드를 실행하면 어느 시점에서든 실행이 중단된 후 catch 블록으로 넘어갈 수 있다.
- 어떤 면에서 try 블록은 트랜잭션과 비슷하다.
try 블록에서 무슨 일이 생기든지 catch 블록은 프로그램 상태를 일관성 있게 유지해야 한다.
그러므로 예외가 발생할 코드를 짤 때는 `try-catch-finally` 문으로 시작하는 편이 낫다.
그러면 try 블록에서 무슨 일이 생기든지 호출자가 기대하는 상태를 정의하기 쉬워진다.

- 파일이 없으면 예외를 던지는지 알아보는 단위 테스트 예시 코드
    
    ```java
    @Test(expected = StorageException.class)
    public void retrieveSectionShouldThrowOnInvalidFileName() {
    	sectionStore.retrieveSection("invalid - file");
    }
    ```
    
    ```java
    public List<RecordedGrip> retrieveSection(String sectionName) {
    	try {
    		FileInputStream stream = new FileInputStream(sectionName);
    		stream.close();
    	} catch (FileNotFoundException e) {
    		throw new StorageException("retrieval error", e);
    	}
    	return new ArrayList<RecordedGrip>();
    }
    ```
    
- 강제로 예외를 일으키는 테스트 케이스를 작성한 후 테스트를 통과하게 코드를 작성하는 방법을 권장한다.
그러면 자연스럽게 try 블록의 트랜잭션 범위부터 구현하게 되므로 범위 내에서 트랜잭션 본질을 유지하기
쉬워진다.

## 미확인 예외를 사용하라

- 확인된 예외는 OCP(Open Closed Principle)를 위반한다.
- 메서드에서 확인된 예외를 던졌는데 catch 블록이 세 단계 위에 있다면 그 사이 메서드 모두가 선언부에 해당
예외를 정의해야 한다.
즉, 하위 단계에서 코드를 변경하면 상위 단계 메서드 선언부를 전부 고쳐야 한다는 말이다.
- 모듈과 관련된 코드가 전혀 바뀌지 않았더라도 (선언부가 바뀌었으므로) 모듈을 다시 빌드한 다음 배포해야
한다는 말이다.

## 예외에 의미를 제공하라

- 예외를 던질 때는 전후 상황을 충분히 덧붙인다.
그러면 오류가 발생한 원인과 위치를 찾기가 쉬워진다.
- 자바는 모든 예외에 호출 스택을 제공한다.
하지만 실패한 코드의 의도를 파악하려면 호출 스택만으로 부족하다.
- 오류 메시지에 정보를 담아 예외와 함께 던진다.
실패한 연산 이름과 실패 유형도 언급한다.
애플리케이션이 로깅 기능을 사용한다면 catch 블록에서 오류를 기록하도록 충분한 정보를 넘겨준다.

## 호출자를 고려해 예외 클래스를 정의하라

- 애플리케이션에서 오류를 정의할 때 프로그래머에게 가장 중요한 관심사는 오류를 잡아내는 방법이다.
- 다음은 오류를 형편없이 분류한 사례다.
외부 라이브러리를 호출하는 `try-catch-finally` 문을 포함한 코드로, 외부 라이브러리가 던질 예외를
모두 잡아낸다.
    
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
    
    - 위 코드는 중복이 심하지만 놀랍지 않다.
    - 대다수 상황에서 우리가 오류를 처리하는 방식은 (오류를 일으킨 원인과 무관하게) 비교적 일정하다.
        1. 오류를 기록한다.
        2. 프로그램을 계속 수행해도 좋은지 확인한다.
    - 위 경우는 예외에 대응하는 방식이 예외 유형과 무관하게 거의 동일하다.
    그래서 코드를 간결하게 고치기가 아주 쉽다.
    - 호출하는 라이브러리 API 를 감싸면서 예외 유형 하나를 반환하면 된다.
    
    ```java
    LocalPort port = new LocalPort(12);
    
    try {
    	port.open();
    } catch (PortDeviceFailure e) {
    	reportError(e);
    	logger.log(e.getMessage(), e);
    } finally {
    	...
    }
    ```
    

## 정상 흐름을 정의하라

- 외부 API 를 감싸 독자적인 예외를 던지고, 코드 위에 처리기를 정의해 중단된 계산을 처리한다.
대개는 멋진 처리 방식이지만, 때로는 중단이 적합하지 않은 때도 있다.
- 예제를 살펴보자.
다음은 비용 청구 애플리케이션에서 총계를 계산하는 허술한 코드이다.
    
    ```java
    try {
    	MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
    	m_total += expenses.getTotal()
    } catch (MealExpensesNotFound e) {
    	m_total += getMealPerDiem();
    }
    ```
    
    - 위에서 식비를 비용으로 청구했다면 직원이 청구한 식비를 총계에 더한다.
    식비를 비용으로 청구하지 않았다면 일일 기본 식비를 총계에 더한다.
    그런데 예외가 논리를 따라가기 어렵게 만든다.
    - 특수 상황을 처리할  필요가 없다면 더 좋지 않을까?
    그러면 코드가 훨씬 더 간결해진다.
    
    ```java
    MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
    m_total += expenses.getTotal();
    ```
    
    - 청구한 식비가 없다면 일일 기본 식비를 반환하는 `MealExpense` 객체를 반환한다.
    
    ```java
    public class PerDiemExpenses implements MealExpenses {
    	public int getTotal() {
    		// 기본 값으로 일일 기본 식비를 반환한다.
    	}
    }
    ```
    
- 이를 **특수 사례 패턴**이라 부른다.
    - 클래스를 만들거나 객체를 조작해 특수 사례를 처리하는 방식이다.
    - 그러면 클라이언트 코드가 예외적인 상황을 처리할 필요가 없어진다.
    - 클래스나 객체가 예외적인 상황을 캡슐화해서 처리하므로…

### NULL을 반환하지 마라

- 오류 처리를 논하는 장이라면 우리가 흔히 저지르는 바람에 오류를 유발하는 행위도 언급해야 한다고 생각한다
- 그중 첫째가 NULL 을 반환하는 습관이다.
- 한줄 건너 하나씩 NULL 을 확인하는 코드로 가득한 애플리케이션을 지금까지 수도 없이 봤다.
    
    ```java
    public void registerItem(Item item) {
    	if (item != null) {
    		ItemRegistry registry = peristntStore.getItemRegistry();
    		
    		if (registry != null) {
    			Item existing = registry.getItem(item.getID());
    			
    			if (existing.getBillingPeriod().hasRetailOwner()) {
    				existing.register(item);
    			}
    		}
    	}
    }
    ```
    
    - 위 코드는 나쁜 코드다!
        - NULL 을 반환하는 코드는 일거리를 늘릴 뿐만 아니라 호출자에게 문제를 떠넘긴다.
        - 누구 하나라도 NULL 확인을 빼먹는다면 애플리케이션이 통제 불능에 빠질지도 모른다.
        - 위 코드에서 둘째 행에 NULL 확인이 빠졌다는 사실을 눈치챘는가?
        - 만약 persistentStore 가 NULL 이라면 실행 시 어떤 일이 벌어질까?
            - NullPointerException 이 발생한다.
    - 메서드에서 NULL 을 반환하고픈 유혹이 든다면 그 대신 예외를 던지거나 특수 사례 객체를
    반환한다.
    - 사용하려는 외부 API 가 NULL 을 반환한다면 감싸기 메서드를 구현해 예외를 던지거나 특수 사례 객체를
    반환하는 방식을 고려한다.
    
    ```java
    List<Employee> employess = getEmployees();
    
    if (employees != null) {
    	for (Employee e : employees) {
    		totalPay += e.getPay();
    	}
    }
    ```
    
    - 변경 코드
    
    ```java
    public List<Employee> getEmployees() {
    	if (.. 직원이 없다면 ..)
    		return Collections.emptyList();
    }
    ```
    

### NULL 을 전달하지 마라

- 메서드에서 NULL 을 반환하는 방식도 나쁘지만 메서드로 NULL 을 전달하는 방식은 더 나쁘다.
- 정상적인 인수로 NULL 을 기대하는 API 가 아니라면 메서드로 NULL 을 전달하는 코드는 최대한 피한다.
    
    ```java
    public class MetricsCalculator {
    	public double xProjection(Point p1, Point p2) {
    		return (p2.x - p1.x) * 1.5;
    	}
    	...
    }
    ```
    
    - 누군가 인수로 NULL 을 전달하면 어떤 일이 벌어질까?
    
    ```java
    calculator.xProjection(null, new Point(12, 13));
    ```
    
    - 당연히 NullPointerException 이 발생한다.
    
    ```java
    public class MetricsCalculator {
    	public double xProjection(Point p1, Point p2) {
    		if (p1 == null || p2 == null) {
    			throw InvalidArgumentException(
    				"Invalid argument for MetricsCalculator.xProjection"
    			);
    		}
    		return (p2.x - p1.x) * 1.5;
    	}
    }
    ```
    
    - assert 문을 사용할 수 있다.
    
    ```java
    public class MetricsCalculator {
    	public double xProjection(Point p1, Point p2) {
    		assert p1 != null : "p1 should not be null";
    		assert p2 != null : "p2 should not be null";
    		return (p2.x - p1.x) * 1.5;
    	}
    }
    ```
    

- 문서화가 잘 되어 코드 읽기는 편하지만 문제를 해결하지는 못한다.
누군가 NULL 을 전달하면 여전히 실행 오류가 발생한다.
- 대다수 프로그래밍 언어는 호출자가 실수로 넘기는 NULL 을 적절히 처리하는 방법이 없다.
그렇다면 애초에 NULL 을 넘기지 못하도록 금지하는 정책이 합리적이다.
- 즉, 인수로 NULL 이 넘어오면 코드에 문제가 있다는 말이다.
이런 정책을 따르면 그만큼 부주의한 실수를 저지를 확률도 작아진다.

## 결론

- 깨끗한 코드는 읽기도 좋아야 하지만 안정성도 높아야 한다.
- 이 둘은 상충하는 목표가 아니다.
오류 처리를 프로그램 논리와 분리해 독자적인 사안으로 고려하면 튼튼하고 깨끗한 코드를 작성할 수 있다.
- 오류 처리를 프로그램 논리와 분리하면 독립적인 추론이 가능해지며 코드 유지보수성도 크게 높아진다.
