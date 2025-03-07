# 객체와 자료구조

## 자료 추상화

- 추상 인터페이스를 제공해 사용자가 구현을 모른 채 자료의 핵심을 조작할 수 있어야 진정한 의미의 클래스다.
- 자료를 세세하게 공개하기보다는 추상적인 개념으로 표현하는 편이 좋다.

```java
// 구체적인 Vehicle 클래스
public interface Vehicle {
  double getFuelTankCapacityInGallons();
  double getGallonsOfGasoline();
}

---

// 추상적인 Vehicle 클래스
public interface Vehicle {
  double getPercentFuelRemaining();
}
```

## 자료/객체 비대칭

- 객체는 추상화 뒤로 자료를 숨긴 채 자료를 다루는 함수만 공개하고,
자료 구조는 자료를 그대로 공개하며 별다른 함수는 제공하지 않는다.
- 객체 지향 코드에서 어려운 변경은 절차적인 코드에서 쉬우며, 
절차적인 코드에서 어려운 변경은 객체 지향 코드에서 쉽다.
    - 새로운 자료 타입을 추가한다면 클래스와 객체 지향 기법이 적합하고,
    새로운 함수를 추가한다면 절차적인 코드와 자료구조가 적합하다.

## 디미터 법칙

- 모듈은 자신이 조작하는 객체의 속사정을 몰라야 한다는 법칙이다.
    - 객체는 자료를 숨기고 함수를 공개한다.
- 클래스 C의 메서드 f는 다음과 같은 객체의 메서드만 호출해야 한다.
    - 클래스 C
    - f가 생성한 객체
    - f 인수로 넘어온 객체
    - C 인스턴스 변수에 저장된 객체

### 기차 충돌

```java
final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();
```

위 코드를 다음과 같이 나눠보자.

```java
Options opts = ctxt.getOptions();
File scratchDir = opts.getScratchDir();
final String outputDir = scratchDir.getAbsolutePath();
```

이러면 디미터 법칙을 위반할까?

- ctxt, Options, ScratchDir이 객체라면 내부 구조로 숨겨야 하므로 위반,
자료 구조라면 내부 구조를 노출해야 하므로 위반하지 않는다.

### 잡종 구조

- 객체와 자료 구조의 특성을 모두 갖는 잡종 구조라면 새로운 함수와 새로운 자료 구조 모두 추가하기 어려우니 피하자.

### 구조체 감추기

```java
ctxt.getAbsolutePathOfScratchDirectoryOption();
// 혹은
ctx.getScratchDirectoryOption().getAbsolutePath()
```

- ctxt가 객체라면 “**뭔가를 해!”**라고 말해야지 속을 드러내면 안된다.
- 이 문장이 결론적으로 원하는 행동이 무엇인지 파악하고, 그 역할에 맞는 이름으로 함수를 만들자.

## 자료 전달 객체 (DTO)

### 활성 레코드

- 레코드는 자료 구조로 취급한다.
    - 비즈니스 규칙을 담으면서 내부 자료를 숨기는 객체는 따로 생성한다.

## 결론

- 객체는 동작을 공개하고 자료를 숨긴다.
    - 기존 동작을 변경하지 않으면서 새 객체 타입을 추가하기는 쉽다.
    - 기존 객체에 새 동작을 추가하기는 어렵다.
- 자료 구조는 별다른 동작 없이 자료를 노출한다.
    - 기존 자료 구조에 새 동작을 추가하기는 쉽다.
    - 기존 함수에 새 자료 구조를 추가하기는 어렵다.
- 직면한 문제에 최적인 해결책을 선택하자.