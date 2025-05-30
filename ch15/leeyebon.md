설계 및 개발한 1차 모듈에 대해서 구조 및 가독성을 개선하는 방안에 대해 설명되어 있다.

보이스카우트 규칙을 따르면 개선이 가능하다.

### 보이스카웃 규칙

"코드를 만났을 때보다 더 깨끗하게 만들어 놓고 떠나라.”

소프트웨어 시스템은 시간이 지날수록 망가진다는 법칙이 있다. 보이스카웃 규칙을 지킨다면 잠재적인 오류를 방지하는데에 큰 도움이 된다.

모듈에 *새로 추가하는 내 코드 또한 깨끗하다*는 전제를 바탕으로 하기 때문에 순차적으로 코드를 개선해야 한다.

1. 코드 청소: 주석을 정리하거나 불필요한 코드를 제거하고, 변수 이름이나 함수 이름을 더 의미 있게 변경하는 등의 작은 수정으로 코드를 더 깨끗하게 만든다.
2. 리팩토링: 복잡하거나 중복된 코드를 단순화하고, 가독성을 높이기 위해 리팩토링을 수행한다.
3. 일관성 유지: 코드 스타일과 포맷을 일관되게 유지하여, 전체 코드베이스의 일관성을 높인다.
4. 테스트 추가: 기능을 수정하거나 추가할 때, 그에 따른 테스트 코드를 추가하여 코드의 신뢰성을 높인다.

출처: https://velog.io/@ensmart/202406051400

### 캡슐화

의도를 명확히 표현하려면 조건문을 캡슐화해야 한다. 이에 맞는 메서드명이 필요하다.

### 멤버변수와 지역변수의 구분

함수에서 멤버 변수와 이름이 똑같은 변수를 사용하는 이유를 검토하고 사용 목적에 맞게 변수명을 고쳐줘야 한다.

```java
private String expected;
private String actual;
public String compact(String message){
	...
	String expected = compactString(this.expected);
	String actual = compactString(this.actual);
}
```

```java
private String expected;
private String actual;
public String compact(String message){
	...
	String compactExpected = compactString(expected);
	String compactActual = compactString(actual);
}
```

### 부정문 vs 긍정문

부정문은 긍정문보다 이해하기 약간 더 어렵다. 그러므로 첫 문장 if를 긍정으로 만들어 조건문을 작성하는 것이 가독성면에서 더 좋다.

### 결합 검토

- SRP 검토
    
    canBeCompacted 메서드는 문자열을 압축하는 함수라지만 false라면 압축하지 않는다. 그러므로 함수에 compact라는 이름을 붙이면 오류 점검이라는 부가 단계가 숨겨진다.
    
    압축 로직과 형식을 갖춘 문자열을 반환하는 로직이 모두 담긴 compact 메서드에서 압축 로직을 따로 메서드 분리해 형식을 맞추는 작업은 formatCompactedComparision이 수행하고 compactExpectedAndActual 메서드는 압축만 수행해 SRP를 준수하게끔 재설계할 수 있다.
    
- 숨겨진 시간적인 결합 검토
    
    예시에서 비교값과 입력값의 공통되는 prefix와 suffix를 찾기 위해서는 prefix가 선행되어야 한다. 개선 이전의 코드에서는 이러한 시간적인 결합이 존재해 suffix를 찾는 메서드가 prefix를 찾는 메서드에 의존한다는 문제가 발생해 디버깅이 어렵다. 이를 해결하기 위해 prefixIndex를 인자로 받는 suffix 찾는 메서드를 만들 수 있다. 그러나 이 방법은 순서가 확실시 되지만 prefixIndex가 반드시 존재해야 하는가에 대한 존재적인 의문이 생길 수 있다. 그렇기 때문에 따로 메서드를 정의하여 해당 메서드 안에서 prefix와 suffix를 찾는 로직을 수행하게 할 수 있다.
    

### AS-IS

```java
package junit.framework;

public class ComparisonCompactor {
	private int ctxt;
	private String s1;
	private String s2;
	private int pfx;
	private int sfx;
	
	public ComparisonCompactor(int ctxt, String s1, String s2){
		this.ctxt = ctxt;
		this.s1 = s1;
		this.s2 = s2;
	}
	
	public String compact(String msg) {
		if (s1 == null || s2 == null || s1.equals(s2)
			return Assert.format(msg,s1,s2);
			pfx = 0;
			for (; pfx < Math.min(s1.length(), s2.length()); pfx++){
				if (s1.charAt(pfx) != s2.charAt(pfx))
					break;
			}
			int sfx1 = s1.length() - 1;
			int sfx2 = s2.length() - 1;
			for (; sfx2 >= pfx && sfx1 >= pfx; sfx2--, sfx1--){
				if (s1.charAt(sfx1) != s2.charAt(sfx2))
					break;
			}
			sfx = s1.length() - sfx1;
			String cmp1 = compactString(s1);
			String cmp2 = compactString(s2);
			return Assert.format(msg,cmp1,cmp2);
	}
	
	public String compactString(String s) {
		String result =
			"[" + s.substring(pfx, s.length() - sfx + 1) + "]";
			if (pfx > 0)
				result = (pfx > ctxt ? "..." : "") + s1.substring(Math.max(0,pfx-ctxt), pfx) + result;
			if (sfx > 0) {
				int end = Math.min(s1.length() - sfx + 1 + ctxt, s1.length());
				result = result + (s1.substring(s1.length() - sfx + 1, end) +
					(s1.length() - sfx + 1 < s1.length() - ctxt ? "..." : ""));
			}
			return result;
	}
}
```

### TO-BE

```java
package junit.framework;

public class ComparisonCompactor {

    private static final String ELLIPSIS = "...";
    private static final String DELTA_END = "]";
    private static final String DELTA_START = "[";

    private int contextLength;
    private String expected;
    private String actual;
    private int prefixLength;
    private int suffixLength;

    public ComparisonCompactor(int contextLength, String expected, String actual) {
        this.contextLength = contextLength;
        this.expected = expected;
        this.actual = actual;
    }

    public String formatCompactedComparison(String message) {
        String compactExpected = expected;
        String compactActual = actual;

        if (shouldBeCompacted()) {
            findCommonPrefixAndSuffix();
            compactExpected = compact(expected);
            compactActual = compact(actual);
        }

        return Assert.format(message, compactExpected, compactActual);
    }

    private boolean shouldBeCompacted() {
        return !shouldNotBeCompacted();
    }

    private boolean shouldNotBeCompacted() {
        return expected == null || actual == null || expected.equals(actual);
    }

    private void findCommonPrefixAndSuffix() {
        findCommonPrefix();
        suffixLength = 0;

        for (; !suffixOverlapsPrefix(); suffixLength++) {
            if (charFromEnd(expected, suffixLength) != charFromEnd(actual, suffixLength)) {
                break;
            }
        }
    }

    private char charFromEnd(String s, int i) {
        return s.charAt(s.length() - i - 1);
    }

    private boolean suffixOverlapsPrefix() {
        return actual.length() - suffixLength <= prefixLength
            || expected.length() - suffixLength <= prefixLength;
    }

    private void findCommonPrefix() {
        prefixLength = 0;
        int end = Math.min(expected.length(), actual.length());

        for (; prefixLength < end; prefixLength++) {
            if (expected.charAt(prefixLength) != actual.charAt(prefixLength)) {
                break;
            }
        }
    }

    private String compact(String s) {
        return new StringBuilder()
            .append(startingEllipsis())
            .append(startingContext())
            .append(DELTA_START)
            .append(delta(s))
            .append(DELTA_END)
            .append(endingContext())
            .append(endingEllipsis())
            .toString();
    }

    private String startingEllipsis() {
        return prefixLength > contextLength ? ELLIPSIS : "";
    }

    private String startingContext() {
        int contextStart = Math.max(0, prefixLength - contextLength);
        int contextEnd = prefixLength;
        return expected.substring(contextStart, contextEnd);
    }

    private String delta(String s) {
        int deltaStart = prefixLength;
        int deltaEnd = s.length() - suffixLength;
        return s.substring(deltaStart, deltaEnd);
    }

    private String endingContext() {
        int contextStart = expected.length() - suffixLength;
        int contextEnd = Math.min(contextStart + contextLength, expected.length());
        return expected.substring(contextStart, contextEnd);
    }

    private String endingEllipsis() {
        return suffixLength > contextLength ? ELLIPSIS : "";
    }
}
```