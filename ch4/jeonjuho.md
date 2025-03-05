# 주석

### 나쁜 코드에 주석을 달지 마라. 새로 짜라.

- 프로그래밍 언어 자체가 표현력이 풍부하다면, 
프로그래밍 언어를 치밀하게 사용해 의도를 표현한다면,
주석은 거의 필요하지 않을 것이다.
- 시간이 지남에 따라서 주석이 코드와 분리되어 부정확해질 수 있다.

## 주석은 나쁜 코드를 보완하지 못한다.

## 코드로 의도를 표현하라!

- 주석으로 달려는 설명을 함수로 만들어 표현해도 충분하다.

## 좋은 주석

### 법적인 주석

- 저작권 정보와 소유권 정보

### 정보를 제공하는 주석

- 때때로 편할 수는 있다.

### 의도를 설명하는 주석

```java
public int compareTo(Object o)
{
	if(o instanceof WikiPagePath)
	{
		WikiPagePath p = (WikiPagePath) o;
		String compressedName = StringUtil.join(names,;
		String compressedArgumentName = StringUtil.join(p.names, "*');
		return comp ressedName.compa reTo(comp ressedArgumentName);
	}
	return 1; // 오른쪽 유형이므로 정렬 순위가 더 높다.
}
```

- 두 객체를 비교할 때 높은 순위를 주기 위해서 저자의 의도가 설명된다.

```java
public void testConcurrentAddWidgets() throws Exception {
  WidgetBuilder widgetBuilder =
    new WidgetBuilder(new Class[]{BoldWidget.class});
  String text = "'''bold text'''"**;**
  ParentWidget parent =
    new BoldWidget(new MockWidgetRoot(), "'''bold text'''")； 
  AtomicBoolean failFlag = new AtomicBoolean();
  failFlag.set(false);
  
  // 스레드・ 대량 생성하는 방법으로 어떻게든 경쟁 조건을 만들려 시도한다.
  for (int i = 0; i < 25000; i++) {
    WidgetBuilderThread widgetBuilderThread =
    new WidgetBuilderThread(widgetBuilder, text, parent, failFlag);
    Thread thread = new Thread(widgetBuilderThread);
    thread.start();
  }
  assertEquals(false, failFlag.get());
}
```

- 앞선 예시보다 저자의 의도를 분명히 드러낸다.

### 의미를 명료하게 밝히는 주석

- 인수나 반환값 자체를 명확하게 만들면 더 좋겠지만, 어쩔 수 없을 때 의미를 명료하게 밝혀주기 위해 사용하면 유용하다.
- 하지만 주석이 올바른지 검증하기 어렵기 때문에 주의해야 한다.

### 결과를 경고하는 주석

- @Ignore 을 활용하여 주석을 대체할 수 있다.

### TODO 주석

- 당장 구현하기 어려운 업무를 기술한다.
- 나쁜 코드를 남겨 놓는 핑계가 되어서는 안 된다.
- 주기적으로 TODO 주석을 점검하는 것을 권한다.

### 중요성을 강조하는 주석

### 공개 API에서 Javadocs

- 공개 API에 중요한 설명서가 되어 주지만, 주석과 동일하게 잘못된 정보를 전달할 가능성이 존재하니 주의해야 한다.

## 나쁜 주석

### 주절거리는 주석

- 주석을 달기로 결정했다면 최고의 주석을 달도록 노력해야 한다.
- 코드를 통해 할 수 있는 정확한 판단이 흐려질 수 있다.

### 같은 이야기를 중복하는 주석

- 코드의 내용으로 이해할 수 있는 내용을 주석에 다는 것은 불필요하다.

### 오해할 여지가 있는 주석

- 주석이 오히려 명확하지 않다면 혼돈을 야기한다.

### 의무적으로 다는 주석

- 모든 함수/변수에 주석을 달면 오히려 코드를 복잡하게 만든다.

### 이력을 기록하는 주석

### 있으나 마나 한 주석

- 불필요한 주석이 있으면, 필요한 주석을 놓치게 된다.

### 무서운 잡음

### 함수나 변수로 표현할 수 있다면 주석을 달지 마라

### 위치를 표시하는 주석

```java
// Actions
```

- 너무 자주 사용하지 않는 배너는 눈에 띄어 주의를 환기한다.
- 하지만 꼭 필요할 때만 사용하지 않으면 잡음이 된다.

### 닫는 괄호에 다는 주석

### 공로를 돌리거나 저자를 표시하는 주석

### 주석으로 처리한 코드

- 의미가 있을까 하고 다른 사람들이 지우기를 주저한다.
- 소스 코드 관리 시스템이 있으니 그냥 삭제하라.

### HTML 주석

### 전역 정보

- 근처에 있는 코드만 기술하고 시스템의 전반적인 정보는 기술하지 마라.

### 너무 많은 정보

### 모호한 관계

- 주석과 주석이 설명하는 코드는 둘 사이 관계가 명백해야 한다.

### 함수 헤더

- 이름을 잘 붙이는 것으로 대체한다.

### 비공개 코드에서 Javadocs