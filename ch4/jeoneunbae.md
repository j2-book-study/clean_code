# 주석

- 잘 달린 주석은 그 어떤 정보보다 유용하다
- 경솔하고 근거 없는 주석은 코드를 이해하기 어렵게 만든다.
- 우리에게 프로그래밍 언어를 치밀하게 사용해 의도를 표현할 능력이 있다면, 주석은 거의 필요없다.

- 개발자들은 주석을 엄결하게 관리해야 한다고, 복구성과 관련성과 정확성이 언제나 높아야 한다고
주장할지 모른다.
→ 필자는 코드를 깔끔하게 정리하고 표현력을 강화하는 방향으로 그래서 애초에 주석이 필요 없는 방향으로
힘을 쏟겠다고 한다.
- 부정확한 주석은 아예 없는 주석보다 훨씬 나쁘다.
    
    부정확한 주석은 결코 이뤄지지 않을 기대를 심어주기 때문이다.
    
- 우리는 주석을 가능한 사용하지 않도록 꾸준히 노력해야 한다.
    
    → 공감 포인트 - 주석을 사용하지 않고 변수명이나 함수명으로 코드를 표현하도록 노력하기
    

### 주석은 나쁜 코드를 보완하지 못한다.

- 코드에 주석을 추가하는 이유는 코드 품질이 나쁘기 때문이다.
    
    즉, 표현력이 풍부하고 깔끔하며 주석이 거의 없는 코드가 복잡하고 어수선하며 주석이 많이 달린 코드보다
    훨씬 좋다.
    
    - 자신이 저지른 난장판을 주석으로 설명하려 애쓰는 대신 그 난장판을 깨끗하게 치우는데 시간을 보내라

### 코드로 의도를 표현하라!

- 확실히 코드만 가지고 의도를 설명하기 어려운 경우가 존재한다.
    
    → 불행히 많은 개발자가 이를 코드는 훌륭한 수단이 아니라는 의미로 해석한다.
    

```java
// 직원에게 복지 혜택을 받을 자격이 있는지 검사한다.
if ((employee.flags & HOURLY_FLAG) && (employee.age > 65))
```

```java
if (employee.isEligibleForFullBenefits())
```

- 몇 초만 더 생각하면 코드로 대다수 의도를 표현할 수 있다.
- 많은 주석으로 달려는 설명을 함수로 만들어 표현해도 충분하다.

## 좋은 주석

- 어떤 주석은 필요하거나 유익하다.

### 법적인 주석

- 각 소스 파일 첫머리에 주석으로 들어가는 저작권 정보와 소유권 정보는 필요하고도 타당하다.

```java
// Copyright (C) 2003, 2004, 2005 by Object Mentor, Inc. All rights reserved.
// GNU General Public License 버전 2 이상을 따르는 조건으로 배포
```

- 소스 파일 첫머리에 들어가는 주석이 반드시 계약 조건이나 법적인 정보일 필요는 없다.
    - 모든 조항과 조건을 열거하는 대신에 가능하다면, 표준 라이선스나 외부 문서를 참조해도 된다.

### 정보를 제공하는 주석

```java
// 테스트 중인 Responder 인스턴스를 반환한다.
protected abstract Responder responderInstance();
```

### 의도를 설명하는 주석

```java
public void testConcurrentAddWidgets() throws Exception {
	WidgetBuilder widgetBuilder = new WidgetBuilder(new class[]{BoldWidget.class});
	String text = "'''bold text'''";
	ParentWidget parent = new BoldWidget(new MockWidgetRoot(), text);
	AtomicBoolean failFlag = new AtomicBoolean();
	failFlag.set(false);
	
	/ 스레드를 대량 생성하는 방법으로 어떻게든 경쟁 조건을 만들려 시도한다.
	for (int i = 0; i < 25000; i++) {
		WidgetBuilderThread widgetBuilderThread = 
			new WidgetBuilderThread(widgetBuilder, text, parent, failFlag);
			Thread thread = new Thread(widgetBuilderThread);
			thread.start();
	}
	assertEquals(false, failFlage.get()));
}
```

### 결과를 경고하는 주석

```java
// 여유 시간이 충분하지 않다면 실행하지 마십시오.
public void _testWithReallyBigFile() {
	...
}
```

### TODO 주석

- 앞으로 할 일을 //TODO 주석으로 남겨두면 편하다.

```java
// TODO: MdM 현재 필요하지 않다.
// 체크아웃 모델을 도입하면 함수가 필요 없다.
protected VersionInfo makeVersion() throws Exception {
	return null;
}
```

- 유용한 부분
    - 더 이상 필요 없는 기능을 삭제하는 알림
    - 누군가에게 문제를 봐달라는 요청
    - 더 좋은 이름을 떠올려달라는 부탁
    - 앞으로 발생할 이벤트에 맞춰 코드를 고치라는 주의

## 나쁜 주석

- 대다수 주석은 허술한 코드를 지탱하거나 엉성한 코드를 변명하거나 미숙한 결정을 합리화하는 등
프로그래머가 주절거리는 독백에서 크게 벗어나지 못한다.

### 주절거리는 주석

```java
public void loadProperties() {
	try {
		String propertiesPath = propertiesLocation + "/" + PROPERTIES_FILE;
		FileInputStream propertiesStream = new FileInputStream(propertiesPath);
		loadedProperties.load(propertiesStream);
	}
	catch(IOException e) {
		// 속성 파일이 없다면 기본 값을 모두 메모리로 읽어 들였다는 의미
	}
}
```

- 이해가 안돼 다른 모듈까지 뒤져야 하는 주석은 독자와 제대로 소통하지 못하는 주석이다.

### 같은 이야기를 중복하는 주석

```java
// this.closed 가 true 일 때 반환되는 유틸리티 메서드다.
// 타임아웃에 도달하면 예외를 던진다.
public synchronized void waitForClose(final long timeoutMillis) {
	...
}
```

### 의무적으로 다는 주석

- 모든 함수에 Javadocs를 달거나 모든 변수에 주석을 달아야 한다는 규칙은 어리석기 그지없다.
- 이런 주석은 코드를 복잡하게 만들며 거짓말을 퍼뜨리고, 혼동과 무질서를 초래한다.

```java
/**
*
* @Param title CD 제목
* @Param author CD 저자
* @Param tracks CD 트랙 숫자
* ...
*/
```

### 있으나 마나 한 주석

- 너무 당연한 사실을 제공하여 새로운 정보를 제공하지 못하는 주석이다.

```java
/**
* 기본 생성자
*/
protected AnnualDateRule() {
}
```

### 무서운 잡음

- 오픈 소스 라이브러리에서 가져온 코드며 주석은 어떤 목적을 수행할까?
    - 없다. 단지 문서를 제공해야 한다는 잘못된 욕심으로 탄생한 잡음일 뿐이다

```java
/** The name. **/
private String name;

/** The version. **/
private String version;

/** The licenceName **/
private String licenceName;

/** The version **/
private String info;
```

### 함수나 변수로 표현할 수 있다면 주석을 달지 마라

```java
// 전역 목록 <smodule> 에 속하는 모듈이 우리가 속한 하위 시스템에 의존하는가?
if (smodule.getDependSubsystems().contains(subSysMod.getSubSystem())))
```

- 주석을 없애고 수정하면

```java
ArrayList moduleDependees = smodule.getDependSubsystems();
String ourSubSystem = subSysMod.getSubSystem();
if (moduleDependees.contains(ourSubSystem))
```

### 닫는 괄호에 다는 주석

- 닫는 괄호에 주석을 달아야 한다는 생각이 든다면 대신에 함수를 줄이려 시도하자.

```java
public class wc {
	public static void main(String[] args) {
		BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
		String line;
		int lineCount = 0;
		int charCount = 0;
		int wordCound = 0;
		
		try {
			while ((line = in.readLine) != null) {
				lineCount++;
				charCount += line.length();
				String words[] = line.split("\\W");
				wordCount += words.length;
			} // while
			...
		} // try
	}
} 
```

### 공로를 돌리거나 저자를 표시하는 주석

```java
/** 릭이 추가함 **/
```

- 소스 코드 관리 시스템은 누가 언제 무엇을 추가했는지 귀신처럼 기억한다.
- 저자 이름으로 코드를 오염시킬 필요가 없다.
- 현실적으로 이런 주석은 그냥 오랫동안 코드에 방치되어 점차 부정확하고 쓸모없는 정보로 변하기 쉽다.

### 주석으로 처리한 코드

```java
InputStreamResponse response = new InputStreamResponse();
response.setBody(formatter.getResultStream(), formatter.getByteCount());

// ...
// ...
// ...
```

- 주석으로 처리된 코드는 다른 사람들이 지우기를 주저한다.
- 이유가 있어 남겨 놓거나 중요하니까 지우면 안 된다고 생각한다.
- 그래서 질 나쁜 와인병 바닥에 앙금이 쌓이듯 쓸모 없는 코드가 점차 쌓여간다.

### HTML 주석

- 소스 코드에서 HTML 주석은 혐오 그 자체다.
- HTML 주석은 (주석을 읽기 쉬워야 하는) 편집기/IDE 에서 조차 읽기가 어렵다.
- (Javadocs와 같은) 도구로 주석을 뽑아 웹 페이지에 올릴 작정이라면 주석에 HTML 태그를 삽입해야 하는 책임은
프로그래머가 아니라 도구가 져야한다.

```java
/**
 * 적합성 테스트를 수행하기 위한 과업
 * 이 과업은 적합성 테스트를 수행해 결과를 출력한다.
 * <p/>
 * ...
 * </pre>
/**
```

### 전역 정보

- 주석을 달아야 한다면 근처에 있는 코드만 기술하라
- 코드 일부에 주석을 달면서 시스템의 전반적인 정보를 기술하지 마라

```java
/**
 * 적합성 테스트가 동작하는 포트: 기본 값은 <b>8082</b>
 *
 * @param fitnessePort
 */
public void setFitnessePort(int fitnessePort) {
	this.fitnessePort = fitnessePort;
}
```

### 모호한 관계

- 주석과 주석이 설명하는 코드는 둘 사이 관계가 명백해야 한다.
- 주석을 달았다면 적어도 독자가 주석과 코드를 읽어보고 무슨 소리인지 알아야한다.

```java
/*
* 모든 픽셀을 담을 만큼 충분한 배열로 시작한다. (여기에 필터 바이트를 더한다.)
* 그리고 헤더 정보를 위해 200바이트를 더한다.
*/
```

- 여기서 필터 바이트란 무엇일까?
- 위 내용을 해결할 수 없으므로 주석을 다는 목적은 코드만으로 설명이 부족해서이다. 
다시 설명을 요구하니 아쉬운 주석이다.

### 함수 헤더

- 짧은 함수는 긴 설명이 필요 없다.
짧고 한 가지만 수행하며 이름을 잘 붙인 함수가 주석으로 헤더를 추가한 함수보다 훨씬 좋다.
