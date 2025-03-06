# 형식 맞추기

- 모듈을 읽으며 두 눈이 휘둥그레 놀라면 좋겠다.
- 전문가가 짰다는 인상을 심어주면 좋겠다.

## 형식을 맞추는 목적

- 코드 형식은 의사소통의 일환으로 매우 중요하다!
- 원활한 소통을 장려하는 코드 형식을 지켜야 한다.

## 적절한 행 길이를 유지하라

- 일반적으로 큰 파일보다 작은 파일이 이해하기 쉽다.

### 신문 기사처럼 작성하라

- 이름은 간단하면서도 설명이 가능하게 한다.
- 소스 파일 첫 부분은 고차원 개념과 알고리즘을 설명한다.
- 아래로 내려갈수록 의도를 세세하게 묘사한다.
- 마지막에는 가장 저차원 함수와 세부 내역이 나온다.

### 개념은 빈 행으로 분리하라

- 빈 행을 추가함으로써 보기에 편하도록 할 수 있다.

### 세로 밀집도

- 서로 밀접한 코드 행은 세로로 가까이 놓여야 한다.

### 수직 거리

- 서로 밀접한 개념은 한 파일에 속해야 마땅하다.
    - protected 변수를 피해야 하는 이유 중 하나다.
- 변수 선언
    - 변수는 사용하는 위치에 최대한 가까이 선언한다.
- 인스턴스 변수
    - 인스턴스 변수는 클래스 맨 처음에 선언한다.
    - 그렇지 않고 퍼져 있는 경우에는 변수를 발견하기 힘들다.
- 종속 함수
    - 한 함수가 다른 함수를 호출한다면 두 함수는 세로로 가까이 배치한다.
    - 가능하다면 호출하는 함수를 호출되는 함수보다 먼저 배치한다.
    - 추가적으로
        
        ```java
        public Response makeResponse(FitNesseContext context, Request request)
          throws Exception {
          String pageName = getPageNameOrDefault(request, "Frontpage");
          loadPage(pageName, context);
          if (page = null)
            return notFoundResponse(context, request);
          else
            return makePageResponse(context);
        }
        
        private String getPageNameOrDefauIt(Request request, String defaultPageName)
        {
          String pageName = request.getResource();
          if (StringUtil.isBlank(pageName))
            pageName = defaultPageName;
          return pageName;
        }
        ```
        
        - 이러한 코드에서 “Frontpage” 라는 상수 값을 getPageNameOrDefault() 메서드 내부에 두지 않는 이유는 기대와는 달리 잘 알려진 상수가 적절치 않은 저차원 함수에 묻힐 수 있기 때문이다.
        - 상수를 알아야 마땅한 함수에서 실제로 사용하는 함수로 상수를 넘겨주는 방법이 더 좋다.
            - **(주호)** DIP 원칙의 의도와 비슷하다는 생각이 든다.
- 개념적 유사성
    - 개념적인 친화도가 높을수록 코드를 가까이 배치한다.ㄷ
    - 예시
        - 종속 관계의 함수
        - 변수와 그 변수를 사용하는 함수
        - 비슷한 동작을 수행하는 일군의 함수

### 세로 순서

- 일반적으로 함수 호출 종속성은 아래 방향으로 유지한다.
- 소스 코드 모듈이 고차원에서 저차원으로 자연스럽게 내려간다.

## 가로 형식 맞추기

- 너무 제한할 필요는 없지만, 120자 정도가 적절해 보인다.

### 가로 공백과 밀집도

```java
private void measureLine(String line) {
  lineCount++;
  int lineSize = line.length();
  totalChars += lineSize;
  lineWidthHistogram.addLineClineSize, lineCount);
  recordWidestLine(lineSize);
}
```

### 가로 정렬

- 불필요하다.

### 들여쓰기

- 들여쓰기 무시하기
    - 간단한 if문, 짧은 while 문, 짧은 함수에서도 들여쓰기를 꼭 넣자!

### 가짜 범위

- 빈 while 문이나 for 문은 피해야 하지만, 그렇지 못하면 빈 블록을 올바로 들여쓰고 괄호로 감싸자.

```java
// 이러면 코드가 명확하지 않다.
while (dis.read(buf, 0, readBufferSize) != -1)
;

----
// 이렇게 빈 블록을 괄호로 감싸서 명시해주자.
while (dis.read(buf, 0, readBufferSize) != -1) **{
}**
```

## 팀 규칙

- 내가 원하지 않더라도 팀원으로서 팀 규칙을 지켜야 한다.
- 일관적이고 매끄러운 스타일로 작성된 코드는 다른 소스 파일에서도 같은 규칙이 적용될 것이라는 신뢰감을 준다.

## 밥 아저씨의 형식 규칙

- 책 참고