- 어떤 프로그램이든 가장 기본적인 단위가 함수다.

### 작게 만들어라!

- 함수를 만드는 첫째 규칙은 ‘작게!’ 다.
    
    함수를 만드는 둘째 규칙은 ‘더 작게!’ 다.
    
    얼마나 작고 짧아야하는지! 예제만큼 작아야한다.
    
    ```java
    public static String renderPageWithSetupsAndTeardowns (
    	PageData pageData, Boolean isSuite) throws Exception {
    	if (isTestPage(pageData))
    		includeSetupAndTeardownPages(pageData, isSuite);
    		
    	return pageData.getHtml();	
    }
    ```
    

### 블록과 들여쓰기

- if 문 / else 문 / while 문 등에 들어가는 블록은 한 줄이어야 한다는 의미다.
대개 거기서 함수를 호출한다.
그러면 바깥을 감싸는 함수(EnclosingFunction)가 작아질 뿐 아니라, 블록 안에서 호출하는 함수 이름을 적절히 짓는다면,
코드를 이해하기도 쉬워진다.
    
    이 말은 중첩 구조가 생길만큼 함수가 커져서는 안 된다는 뜻이다.
    그러므로 함수에서 들여쓰기 수준은 1단이나 2단을 넘어서면 안 된다.
    당연한 말이지만, 그래야 함수는 읽고 이해하기 쉬워진다.
    

### 한 가지만 해라!

- 함수는 한 가지를 해야 한다.
그 한 가지를 잘 해야 한다.
그 한 가지만을 해야 한다.

- 함수는 간단한 TO 문단으로 기술할 수 있다.
    
    ```java
    TO RenderPageWithSetupsAndTeardowns, 페이지가 테스트 페이지인지 확인한 후 테스트 페이지라면
    설정 페이지와 해제 페이지를 넣는다.
    테스트 페이지든 아니든 페이지를 HTML로 렌더링한다.
    ```
    
    지정된 함수 이름 아래에서 추상화 수준이 하나인 단계만 수행한다면 그 함수는 한 가지 작업만 한다.
    
    어쨌거나 우리가 함수를 만드는 이유는 큰 개념을 (다시 말해, 함수 이름을) 다음 추상화 수준에서 여러 단계로 나눠
    수행하기 위해서가 아니던가!
    
    → 함수를 만드는 목적
    

- 함수가 ‘한 가지’만 하는지 판단하는 방법이 하나 더 있다.
단순히 다른 표현이 아니라 의미 있는 이름으로 다른 함수를 추출할 수 있다면 그 함수는 여러 작업을 하는 셈이다.

### 함수 내 섹션

- `generatePrimes` 함수는 `declartions`, `initializations`, `sieve` 라는 3 섹션으로 나눠진다.
    
    한 함수에서 여러 작업을 한다는 증거다.
    한 가지 작업만 하는 함수는 자연스럽게 섹션으로 나누기 어렵다.
    

### 함수 당 추상화 수준은 하나로!

- 함수가 확실히 ‘한 가지’ 작업만 하려면 함수 내 모든 문장의 추상화 수준이 동일해야 한다.

- `getHtml()` 은 추상화 수준이 아주 높으며,
`String pagePathName = PathParser.render(pagepath);` 는 추상화 수준이 중간이다.
그리고 `.append(”\n”)` 와 같은 코드는 추상화 수준이 아주 낮다.

- 한 함수 내에 추상화 수준을 섞으면 코드를 읽는 사람이 헷갈린다.
특정 표현이 근본 개념인지 아니면 세부사항인지 구분하기 어려운 탓이다.
하지만 문제는 이 정도로 그치지 않는다.
근본 개념과 세부사항을 뒤섞기 시작하면, 깨어진 창문처럼 사람들이 함수에 세부사항을 점점 더 추가한다.

### 위에서 아래로 코드 읽기: 내려가기 규칙

- 코드는 위에서 아래로 이야기처럼 읽혀야 좋다.
즉, 위에서 아래로 프로그램을 읽으면 함수 추상화 수준이 한 번에 한 단계씩 낮아진다.
이것을 내려가기 규칙이라 부른다.

### Switch 문

- 본질적으로 switch 문은 N가지를 처리한다.

- 문제 있는 코드

```java
public Money calculatePay(Employee e) throws InvalidEmployeeType {
	switch (e.type) {
		case COMMISSIONED:
			return calculateCommissionedPay(e);
			
		case HOURLY:
			return calculateHourlyPay(e);
			
		case SALARIED:
			return calculateSalariedPay(e);
			
		default:
			throw new InvalidEmployeeType(e.type);
	}
}
```

- 첫 째, 함수가 길다.
- 둘 째, 함수가 ‘한 가지’ 작업만 수행하지 않는다.
- 세 째, SRP(Single Responsibility Principle) 를 위반한다.
- 네 째, OCP(Open Closed Principle) 를 위반한다.

### 서술적인 이름을 사용하라!

- 함수가 하는 일을 좀 더 잘 표현하므로 훨씬 좋은 이름이다.
private 함수에도 `isTestable`, `iscludeSetupAndTeardownPages` 등과 같이 서술적인 이름을 지었다.

- 좋은 이름이 주는 가치는 아무리 강조해도 지나치지 않다.

- “코드를 읽으면서 짐작했던 기능을 각 루틴이 그대로 수행한다면 깨끗한 코드라 불러도 되겠다.” - 워드

- 함수가 작고 단순할수록 서술적인 이름을 고르기도 쉬워진다.
    - 이름이 길어도 괜찮다.
    - 겁먹을 필요없다.
    - 길고 서술적인 이름이 짧고 어려운 이름보다 좋다.
    - 길고 서술적인 이름이 길고 서술적인 주석보다 좋다.
    - 함수 이름을 정할 때는 여러 단어가 쉽게 읽히는 명명 법을 사용한다.
    그런 다음, 여러 단어를 사용해 함수 기능을 잘 표현하는 이름을 선택한다.
    - 이름을 정하느라 시간을 들여도 괜찮으며, 이런저런 이름을 넣어 코드를 읽어보면 더 좋다.
    - IDE 를 사용하여 이런저런 이름을 시도한 후 최대한 서술적인 이름을 골라도 좋다.

- 서술적인 이름을 사용하면 개발자 머릿속에서도 설계가 뚜렷해지므로 코드를 개선하기 쉬워진다.
    - 좋은 이름을 고른 후 코드를 더 좋게 재구성하는 사례도 없지 않다.

- 이름을 붙일 때는 일관성이 있어야 한다.
    - 모듈 내에서 함수 이름은 같은 문구, 명사, 동사를 사용한다.

### 함수 인수

- 함수에서 이상적인 인수 개수는 0개(무항)다.
다음은 1개(단항)고, 다음은 2개(이항)다.
특별한 이유가 있어도 사용하면 안 된다.

- 인수는 어렵다.
인수는 개념을 이해하기 어렵게 만든다.
(필자가 우리 예제에서 인수를 거의 없앤 이유)
    - `includeSetupPageInto(new PageContent)` 보다 `includeSetupPage()`가 이해하기 더 쉽다.
    - 테스트 관점에서 보면 인수는 더 어렵다.
    갖가지 인수 조합으로 함수를 검증하는 테스트 케이스를 작성한다고 상상해보라!
    인수가 없다면 간단하다.
    인수가 하나라도 괜찮다.

### 많이 쓰는 단항 형식

- 함수에 인수 1개를 넘기는 이유로 가장 흔한 경우는 2가지이다.
하나는 인수에 질문을 던지는 경우, 다른 하나는 인수를 뭔가로 변환해 결과를 반환하는 경우이다.
    
    ```java
    boolean fileExists("MyFile")
    
    InputStream fileOpen("MyFile")
    -> String 형의 파일 이름을 InputStream 으로 변환한다.
    ```
    

- 아주 유용한 단항 함수 형식이 이벤트다.
이벤트 함수는 입력 인수만 있고, 출력 인수는 없다.
    - 프로그램은 함수 호출을 이벤트로 해석해 입력 인수로 시스템 상태를 바꾼다.
- 이벤트 함수는 조심해서 사용한다.
    - 이벤트라는 사실이 코드에 명확히 드러나야 한다.
    그러므로 이름과 문맥을 주의해서 선택한다.
- 입력 인수를 변환하는 함수라면 변환 결과는 반환값으로 돌려준다.

### 플래그 인수

- 플래그 인수는 추하다
    - 함수로 부울 값을 넘기는 관례는 정말 끔직하다.

### 이항 함수

- 인수가 2개인 함수는 인수가 1개인 함수보다 이해하기 어렵다.
    - 예를 들어, writeField(name)는 writeField(outputStream, name) 보다 이해하기 쉽다.
        - 의미는 명백하지만 전자가 더 쉽게 읽히고 더 빨리 이해된다.
        후자는 잠시 주춤하며 첫 인수를 무시해야 한다는 사실을 깨닫는 시간이 필요하다.
        - 어떤 코드든 절대로 무시하면 안된다. 그 곳에서 오류가 숨어있으니..!

- 이항 함수가 무조건 나쁘다는 소리는 아니다.
    - 하지만 그만큼 위험이 따른다는 사실을 이해하고 가능하면 단항 함수로 바꾸도록 애써야 한다.

### 삼항 함수

- 인수가 3개인 함수는 인수가 2개인 함수보다 훨씬 더 이해하기 어렵다.

### 인수 목록

- 때로는 인수 개수가 가변적인 함수도 필요하다.
    - `String.format` 메서드가 좋은 예이다.
        
        ```java
        String.format("%s worked %.2f" hours.", name, hours);
        ```
        

- 가변 인수를 취하는 함수는 단항, 이항, 삼항 함수로 취급할 수 있다.
하지만 이를 넘어서는 인수를 사용할 경우에는 문제가 있다.

### 동사나 키워드

- 함수의 의도나 인수의  순서와 의도를 제대로 표현하려면 좋은 함수 이름이 필수다.
- 단항 함수는 함수와 인수가 동사/명사 쌍을 이뤄야 한다.
    - `writeField(name)` 처럼 이름이 필드라는 사실이 분명히 드러난다.

- 마지막 예제는 함수 이름에 키워드를 추가하는 형식이다.
즉, 함수 이름에 인수를 넣는다.
    - assertEquals 보다 assertExpectedEqualsActual(expected, actual) 이 더 좋다.
        - 이렇게 작성하면 인수 순서를 기억할 필요가 없어진다.

### 부수 효과를 일으키지 마라!

- 부수 효과는 거짓말이다.
함수에서 한 가지를 하겠다고 약속하고 남 몰래 다른 짓도 하니까…!
    
    ```java
    public class UserValidator {
    	private Cryptographer cryptographer;
    	
    	public boolean checkPassword(String userName, String password) {
    		User user = UserGateway.findByName(userName);
    		
    		if (user != User.NULL) {
    			String codedPhrase = user.getPhraseEncodedByPassword();
    			String phrase = cryptographer.decrypt(codedPhrase, password);
    			if ("Valid Password".equals(phrase)) {
    				Session.initialize();
    				return true;
    			}
    		}
    		return false;
    	}
    }
    ```
    
    - 여기서 함수가 일으키는 부수 효과는 `Session.initialize()` 호출이다.
    - `checkPassword` 함수는 이름 그대로 암호를 확인한다.
    이름만 봐서는 세션을 초기화하는 사실이 드러나지 않는다.
    그래서 함수 이름만 보고 함수를 호출하는 사용자는 사용자를 인증하면서 기존 세션 정보를 지워버릴 
    위험에 처한다.
    - 이런 부수 효과가 시간적인 결합을 초래한다.

### 출력 인수

- 일반적으로 우리는 인수를 함수 입력으로 해석한다.
- 인수를 출력으로 사용하는 함수에 어색함을 느낀다.

### 명령과 조회를 분리하라!

- 함수는 뭔가를 수행하거나 뭔가에 답하거나 둘 중 하나만 해야 한다.
둘 다 하면 안된다.
- 객체 상태를 변경하거나 아니면 객체 정보를 반환하거나 둘 중 하나다.
- 둘 다 하면 혼란을 초래한다.
- 진짜 해결책은 명령과 조회를 분리해 애초에 뿌리뽑는 방법이다.
    
    ```java
    if (set("username", "unclebob")) ... ->
    
    if (attributeExists("username")) {
    	setAttribute("username", "unclebob"));
    	...
    }
    ```
    

### Try/Catch 블록 뽑아내기

- try/catch 블록은 원래 추하다.
코드 구조에 혼란을 일으키며, 정상 동작과 오류 처리 동작을 뒤섞는다.
- try/catch 블록을 별도 함수로 뽑아내는 편이 좋다.
    
    ```java
    public void delete(Page page) {
    	try {
    		deletePageAndAllReferences(page);
    	}
    	catch (Exception e) {
    		logError(e);
    	}
    }
    ```
    
    ```java
    private void deletePageAndAllReferences(Page page) throws Exception {
    	deletePage(page);
    	registry.deleteReference(page.name);
    	configKeys.deleteKty(page.name.makeKey());
    }
    
    private void logError(Exception e) {
    	logger.log(e.getMessage());
    }
    ```
    

### 반복하지 마라

- 중복은 문제다.
- 코드 길이가 늘어날 뿐 아니라 알고리즘이 변하면 모두 손 봐야하니까..

### 구조적 프로그래밍

- 모든 함수와 함수 내 모든 블록에 입구와 출구 하나만 존재해야 한다고 말했다.
    
    즉, 함수는 return 문이 하나여야 한다.
    
- 함수가 아주 클 때만 상당한 이익을 제공한다.
    
    함수를 작게 만든다면 간혹 return, break, continue 를 여러 차례 사용해도 괜찮다.
    
    오히려 때로는 단일 입/출구 규칙보다 의도를 표현하기 쉬워진다.
