뚜껑을 열었을 때 코드가 깔끔하고, 일관적이며, 꼼꼼하다가 감탄하면 좋겠다.

질서 정연하다고 탄복하면 좋겠다.

프로그래머라면 형식을 깔끔하게 맞춰 코드를 짜야한다.

코드 형식을 맞추기 위한 간단한 규칙을 정하고 그 규칙을 착실히 따라야 한다.

팀으로 일한다면 팀이 합의해 규칙을 정하고 모두가 그 규칙을 따라야 한다.

필요하다면 규칙을 자동으로 적용하는 도구를 활용한다.


## 형식을 맞추는 목적

- 코드 형식은 중요하다.
    - 의사소통의 일환이기 때문이다.
    - 의사소통은 전문 개발자의 1차적인 의무이다.

## 적절한 행 길이를 유지하라

- 대부분의 200줄 코드로 커다란 시스템을 구축할 수 있다는 사실이다.
- 반드시 지킬 엄격한 규칙은 아니지만 바람직한 규칙으로 삼으면 좋겠다.
    - 큰 파일보다 작은 파일이 이해하기 쉽다.

## 신문 기사처럼 작성하라

- 소스 파일도 신문 기사와 비슷하게 작성한다.
- 이름은 간단하면서도 설명이 가능하게 짓는다.
- 이름만 보고도 올바름 모듈을 살펴보고 있는지 아닌지를 판단할 정도로 신경써서 짓는다.
- 소스 파일 첫 부분은 고차원 개념과 알고리즘을 설명한다.
- 아래로 내려갈수록 의도를 세세하게 묘사한다.
- 마지막에는 가장 저차원 함수와 세부 내역이 나온다.

### 개념은 빈 행으로 분리하라

- 거의 모든 코드는 왼쪽에서 오른쪽 그리고 위에서 아래로 읽힌다.
- 빈 행은 새로운 개념을 시작한다는 시각적 단서다.
    
    ```java
    public class BoldWidget extends ParentWidget {
    	public static final String REGEXP = "'''.+?'''";
    	private static final Pattern pattern = Pattern.complie("'''(.+?)'''",
    		Pattern.MULTILINE + Pattern.DOTALL
    	);
    	
    	public BoldWidget(ParentWidget parent, String text) throws Exception {
    		super(parent);
    		Matcher match = pattern.matcher(text);
    		match.find();
    		addChildWidgets(match.group(1));
    	}
    }
    ```
    
    - 아래 코드는 위 코드에서 빈 행을 빼버린  코드이다.
    - 코드 가독성이 현저하게 떨어져 암호처럼 보인다.
    
    ```java
    public class BoldWidget extends ParentWidget {
    	public static final String REGEXP = "'''.+?'''";
    	private static final Pattern pattern = Pattern.complie("'''(.+?)'''",
    		Pattern.MULTILINE + Pattern.DOTALL
    	);
    	public BoldWidget(ParentWidget parent, String text) throws Exception {
    		super(parent);
    		Matcher match = pattern.matcher(text);
    		match.find();
    		addChildWidgets(match.group(1));
    	}
    }
    ```
    

### 세로 밀집도

- 줄바꿈이 개념을 분리한다면 세로 밀집도는 연관성을 의미한다.
- 즉, 서로 밀접한 코드 행은 세로로 가까이 놓여야 한다는 뜻이다.
    
    ```java
    public class ReporterConfig {
    	/**
    	* 리포터 리스너의 클래스 이름
    	*/
    	private String m_className;
    	
    	/**
    	* 리포터 리스너의 속성
    	*/
    	private List<Property> m_properties = new ArrayList<Property>();
    	public void addProperty(Property property) {
    		m_properties.add(property);
    	}
    }
    ```
    
    ```java
    public class ReporterConfig {
    	private String m_className;
    	private List<Property> m_properties = new ArrayList<Property>();
    	
    	public void addProperty(Property property) {
    		m_properties.add(property);
    	}
    }
    ```
    
- 척 보면 변수 2개에 메서드가 1개인 클래스라는 사실이 드러난다.
- 머리나 눈을 움직일 필요가 거의 없다.

### 수직 거리

- 서로 밀접한 개념은 세로로 가까이 둬야한다.
- 하지만 타당한 근거가 없다면 서로 밀접한 개념은 한 파일에 속해야 마땅하다.
- 이게 바로 `protected` 변수를 피해야 하는 이유 중 하나다.
- 같은 파일에 속할 정도로 밀접한 두 개념은 세로 거리로 연관성을 표현한다.
    - 여기서 연관성이란 한 개념을 이해하는 데 다른 개념이 중요한 정도다.
- 연관성이 깊은 두 개념이 멀리 떨어져 있으면 코드를 읽는 사람이 소스 파일과 클래스를 여기저기
뒤지게 된다.

### 변수 선언

- 변수는 사용하는 위치에 최대한 가까이 선언한다.
- 우리가 만든 함수는 매우 짧으므로 지역 변수는 각 함수 맨 처음에 선언한다.
    
    ```java
    public int countTestCases() {
    	int count = 0;
    	for (Test each: tests)
    		count += each.countTestCases();
    	return count;
    }
    ```
    

### 인스턴스 변수

- 인스턴스 변수는 클래스 맨 처음에 선언한다.
- 잘 설계한 클래스는 많은 (혹은 대다수) 클래스 메서드가 인스턴스 변수를 사용하기 때문이다.
- 자바에서 보통 클래스 맨 처음에 인스턴스 변수를 선언한다.
- 잘 알려진 위치에 인스턴스 변수를 모은다는 사실이 중요하다.
- 변수 선언을 어디서 찾을지 모두가 알고 있어야 한다.

```java
public class TestSuie implements Test {
	...
	
	private String fName;
	
	private Vector<Test> fTests = new Vector<Test>(10);
	
	...
}
```

### 종속 함수

- 한 함수가 다른 함수를 호출한다면 두 함수는 세로로 가까이 배치한다.
- 또한 가능하다면 호출하는 함수를 호출되는 함수보다 먼저 배치한다.
- 규칙을 일관적으로 적용한다면 독자는 방금 호출한 함수가 잠시 후에 정의된다라는 사실을 예측한다.

```java
public class WikiPageResponder implements SecureResponder {
	protected WikiPage page;
	protected PageData pageData;
	protected String pageTitle;
	protected Request request;
	protected PageCrawler crawler;
	
	public Response makeResponse(FitNesseContext context, Request request) 
	throws Exception {
		String pageName = getPageNameOrDefault(request, "FrontPage");
		loadPage(pageName, context);
		if (page == null)
			return notFoundResponse(context, request)
		else
			return makePageResponse(context);
	}
	
	private String getPageNameOrDefault(Request request, String defaultPageName) {
		...
	}
	
	protected void loadPage(String resource, FitNesseContext context) {
		...
	}
	
	private SImpleResponse makePageResponse(FitNesseContext context) {
		...
	}
}
```

### 개념적 유사성

- 어떤 코드는 서로 끌어당긴다.
- 개념적인 친화도가 높기 때문이다.
- 친화도가 높을수록 코드를 가까이 배치한다.
    
    ```java
    public class Assert {
    	static public void assertTrue(String message, boolean condition) {
    		if (!condition)
    			fail(message);
    	}
    	
    	static public void assertTrue(boolean condition) {
    		assertTrue(null, condition);
    	}
    	
    	static public void assertFalse(String message, boolean condition) {
    		assertTrue(message, !condition);
    	}
    	
    	static public void assertFalse(boolean condition) {
    		assertFalse(null, condition);
    	}
    	
    	...
    }
    ```
    
- 위 함수들은 개념적인 친화도가 매우 높다.
- 명명 법도 같고 기본 기능이 유사하고 간단하다.
- 서로 서로를 호출하는 관계는 부차적인 요인이다.
- 종속적인 관계가 없더라도 가까이 배치할 함수들이다.

### 세로 순서

- 일반적으로 함수 호출 종속성은 아래 방향으로 유지한다.
다시 말해, 호출되는 함수를 호출하는 함수보다 나중에 배치한다.
- 그러면 소스 코드 모듈이 고차원에서 저차원으로 자연스럽게 내려간다.

### 가로 공백과 밀집도

- 가로로는 공백을 사용해 밀접한 개념과 느슨한 개념을 표현한다.
    
    ```java
    private void measureLine(String line) {
    	lineCount++;
    	int lineSize = line.length();
    	totalChars += lineSize;
    	lineWidthHistogram.addLine(lineSize, lineCount);
    	recordWidesLine(lineSize);
    }
    ```
    
- 할당 연산자를 강조하기 위해 앞 뒤 공백을 줬다.
- 할당문은 왼쪽 요소와 오른쪽 요소가 분명히 나뉜다.
- 공백을 넣으면 2가지 주요 요소가 확실히 나뉜다는 사실이 더욱 분명해진다.
- 반면 함수 이름과 이어지는 괄호 사이에는 공백을 넣지 않았다.
함수와 인수는 서로 밀접하기 때문이다.
- 함수를 호출하는 코드에서 괄호 안 인수는 공백으로 분리했다.
쉼표를 강조해 인수가 별개라는 사실을 보여주기 위해서다.
- 연산자 우선순위를 강조하기 위해서도 공백을 사용한다.
    
    ```java
    public class Quadratic {
    	public static double root1(double a, double b, double c) {
    		double determinant = determinant(a, b, c);
    		return (-b + Math.sqrt(determinant)) / (2 * a);
    	}
    	
    	public static double root2(int a, int b, int c) {
    		double determinant = determiant(a, b, c);
    		return (-b - Math.sqrt(determinant)) / (2 * a);
    	}
    	
    	private static double determinant(double a, double b, double c) {
    		return b * b - 4 * a * c;
    	}
    }
    ```
    
- 수식을 읽기 아주 편하다.
- 승수 사이는 공백이 없다.
- 곱셈은 우선 순위가 가장 높기 때문이다.
- 항 사이에는 공백이 들어간다.
- 덧셈과 뺄셈은 우선순위가 곱셈보다 낮기 때문이다.

### 들여쓰기

- 소스 파일은 윤곽도와 계층이 비슷하다.
- 파일 전체에 적용되는 정보가 있고, 
파일 내 개별 클래스에 적용되는 정보가 있고, 
클래스 내 각 메서드에 적용되는 정보가 있고,
블록 내 블록에 재귀적으로 적용되는 정보가 있다.
- 계층에서 각 수준은 이름을 선언하는 범위이자 선언문과 실행문을 해석하는 범위다.
- 이렇듯 범위로 이뤄진 계층을 표현하기 위해 우리는 코드를 들여쓴다.
들여쓰는 정도는 계층에서 코드가 자리잡은 수준에 비례한다.

```java
public class FitNesseServer implements SocketServer {
private FitnesseContext context;
public FitNesseServer(FitNesseContext context) {
this.context = context;
}

public void serve(Socket s) {
serve(s, 10000);
}

public void serve(Socket s, long requestTimeout) {
try {
FitNesseExpediter sender = new FitNesseExpediter(s, context);
sender.setRequestParsingTimeLimit(requestTimeout);
sender.start();
} catch (Exception e) {
e.printStackTrace();
}
}
}
```

```java
public class FitNesseServer implements SocketServer {
	private FitnesseContext context;
	public FitNesseServer(FitNesseContext context) {
		this.context = context;
	}
	
	public void serve(Socket s) {
		serve(s, 10000);
	}
	
	public void serve(Socket s, long requestTimeout) {
		try {
			FitNesseExpediter sender = new FitNesseExpediter(s, context);
			sender.setRequestParsingTimeLimit(requestTimeout);
			sender.start();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

- 들여쓰기 하지 않은 코드는 열심히 분석하지 않는 한 거의 불가하다.

### 가짜 범위

- 때로는 빈 while 문이나 for 문을 접한다.
- 빈 블록을 올바로 들여쓰고 괄호로 감싼다.

```java
while (dis.read(buf, 0, readBufferSize) != -1)
;
```

### 팀 규칙

- 팀은 한 가지 규칙에 합의해야 한다.
    - 그리고 모든 팀원은 그 규칙을 따라야 한다.
    - 그래야 소프트웨어가 일관적인 스타일을 보인다.
    - 개개인이 따로국밥처럼 맘대로 짜대는 코드는 피해야 한다.
- 좋은 소프트웨어 시스템은 읽기 쉬운 문서로 이뤄진다는 사실을 기억하기 바란다.
    - 스타일은 일관적이고 매끄러워야한다.
