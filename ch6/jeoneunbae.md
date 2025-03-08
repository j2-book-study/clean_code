# 객체와 자료구조

- 변수를 비공개(private)로 정의하는 이유가 있다.
    - 남들이 변수에 의존하지 않게 만들고 싶어서다.
    - 충동이든 변덕이든, 변수 타입이나 구현을 맘대로 바꾸고 싶어서다.
- 그렇다면 어째서 수많은 프로그래머가 조회 함수와 설정 함수를 당연하게 공개하여 비공개 변수를
외부에 노출할까?

## 자료 추상화

- 두 클래스 모두 2차원 점을 표현한다.
    - 한 클래스는 구현을 외부로 노출한다.
    - 한 클래스는 구현을 완전히 숨긴다.
    
    ```java
    public class Point {
    	public double x;
    	public double y;
    }
    ```
    
    ```java
    public interface Point {
    	double getX();
    	double getY();
    	void setCartesian(double x, double y);
    	double getR();
    	double getTheta();
    	void setPolar(double r, double theta);
    }
    ```
    

- 변수 사이에 함수라는 계층을 넣는다고 구현이 저절로 감춰지지는 않는다.
    - 구현을 감추려면 추상화가 필요하다!
    - 그저 조회 함수와 설정 함수로 변수를 다룬다고 클래스가 되지는 않는다.
    - 그보다는 추상 인터페이스를 제공해 사용자가 구현을 모른 채 자료의 핵심을 조작할 수 있어야 진정한
    의미있는 클래스다.

- 한 클래스는 연료 상태를 구체적인 숫자 값으로 알려준다.
- 한 클래스는 연료 상태를 백분율이라는 추상적인 개념으로 알려준다.
    - 정보가 어디서 오는지 전혀 드러나지 않는다.

```java
public interface Vehicle {
	double getFuelTankCapacityInGallons();
	double getGallonsOfGasoline();
}
```

```java
public interface Vehicle {
	double getPercentFuelRemaining();
}
```

- 자료를 세세하게 공개하기보다 추상적인 개념으로 표현하는 편이 좋다.
    - 인터페이스나 조회/설정 함수만으로는 추상화가 이뤄지지 않는다.
    - 개발자는 객체가 포함하는 자료를 어떻게 표현할지 심각하게 고민해야 한다.
    - 아무 생각 없이 조회/설정 함수를 추가하는 방법이 가장 나쁘다.

## 자료/객체 비대칭

- 객체와 자료 구조는 근본적으로 양분된다.
    - (자료 구조를 사용하는) 절차적인 코드는 기존 자료 구조를 변경하지 않으면서 새 함수를 추가하기 쉽다.
    반면, 객체 지향 코드는 기존 함수를 변경하지 않으면서 새 클래스를 추가하기 쉽다.
    - 절차적인 코드는 새로운 자료 구조를 추가하기 어렵다.
    그러기 위해 모든 함수를 고쳐야 한다.
    객체 지향 코드는 새로운 함수를 추가하기 어렵다.
    그러려면 모든 함수를 고쳐야 한다.
- 다시 말해, 객체 지향코드에서 어려운 변경은 절차적인 코드에서 쉬우며!
절차적인 코드에서 어려운 변경은 객체 지향 코드에서 쉽다!

- 복잡한 시스템을 짜다 보면 새로운 함수가 아니라 새로운 자료 타입이 필요한 경우가 생긴다.
    - 이 때는 클래스와 객체 지향 기법이 가장 적합하다.
- 반면, 새로운 자료 타입이 아니라 새로운 함수가 필요한 경우도 생긴다.
    - 이 때는 절차적인 코드와 자료 구조가 좀 더 적합하다.

## 디미터 법칙

- 잘 알려진 휴리스틱, 모듈은 자신이 조작하는 객체의 속사정을 몰라야 한다는 법칙이다.
- 디미터 법칙은 “클래스 C 의 메서드 f 는 다음과 같은 객체의 메서드만 호출해야 한다”고 주장한다.
    - 클래스 C
    - f 가 생성한 객체
    - f 인수로 넘어온 객체
    - C 인스턴스 변수에 저장된 객체
- 위 객체에서 허용된 메서드가 반환하는 객체의 메서드는 호출하면 안 된다.

- 디미터 법칙을 어기는 예시이다.
    
    ```java
    final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();
    ```
    
    - `getOptions()` 함수가 반환하는 객체의 `getScratchDir()` 함수를 호출한 후 `getScratchDir()` 함수가반환하는 객체의 `getAbsolutePath()` 함수를 호출하기 때문이다.

### 기차 충돌

- 흔히 위와 같은 코드를 기차 충돌(`Train Wreck`) 이라 부른다.
- 여러 객차가 한 줄로 이어진 기차처럼 보이기 때문이다.
- 그래서 다음과 같이 나누면 좋다.
    
    ```java
    Options opts = ctxt.getOptions();
    File scratchDir = opts.getScratchDir();
    final String outputDir = scratchDir.getAbsolutePath();
    ```
    
- 위 예시 코드가 디미터 법칙을 위반하는지 여부는 `ctxt`, `Options`, `ScartchDir` 이 객체인지 아니면
자료 구조인지에 달렸다.
    - 객체라면 내부 구조를 숨겨야 하므로 확실히 디미터 법칙을 위반한다.
    - 반면, 자료 구조라면 당연히 내부 구조를 노출하므로 디미터 법칙이 적용되지 않는다.
- 그렇다면 ctxt 객체에 임시 파일을 생성하라고 시키면 어떨까?
    
    ```java
    BufferedOutputStream bos = ctxt.createScratchFileStream(classFileName);
    ```
    
    - 객체에게 맡기기에 적당한 임무로 보인다!
    - ctxt 는 내부 구조를 드러내지 않으며, 모듈에서 해당 함수는 자신이 몰라야 하는 여러 객체를 탐색할
    필요가 없다.
    - 따라서 디미터 법칙을 위반하지 않는다.

## 자료 전달 객체

- 자료 구조체의 전형적인 형태는 공개 변수만 있고 함수가 없는 클래스다.
- 이런 자료 구조체를 때로는 자료 전달 객체(DTO) 라고 한다.
- DTO 는 유용한 구조체이다.
    - 특히 데이터베이스와 통신하거나 소켓에서 받은 메시지 구문을 분석할 때 유용하다.
- 흔히 DTO 는 데이터베이스에 저장된 가공되지 않은 정보를 애플리케이션 코드에서 사용할 객체로
변환하는 일련의 단계에서 가장 처음으로 사용하는 구조체다.
- 좀 더 일반적인 형태는 빈(Bean) 구조다.
    - 빈은 비공개 변수를 조회/설정 함수로 조작한다.
    
    ```java
    @RequiredArgsConstruct
    public class Address {
    	private String street;
    	private String streetExtra;
    	private String city;
    	private String state;
    	private String zip;
    	
    	public String getStreet() {
    		return street;
    	}
    	
    	public String getStreetExtra() {
    		return streetExtra;
    	}
    	
    	public String getCity() {
    		return city;
    	}
    	
    	public String getState() {
    		return state;
    	}
    	
    	public String getZip() {
    		return zip;
    	}
    }
    ```
    

## 활성 레코드

- 활성 레코드는 DTO 의 특수한 형태다.
- 공개 변수가 있거나 비공개 변수에 조회/설정 함수가 있는 자료 구조지만, 대개 `save` 나 `find` 와 같은
탐색 함수도 제공한다.
- 활성 레코드는 데이터베이스 테이블이나 다른 소스에서 자료를 직접 변환한 결과다.
- 불행히 활성 레코드에 비즈니스 규칙 메서드를 추가하여 이런 자료 구조를 객체로 취급하는 개발자가
흔하다.
    - 바람직하지 않다.
    - 자료 구조도 아니고 객체도 아닌 잡종 구조가 나오기 때문이다.
- 해결책은 당연하다.
    - 활성 레코드는 자료 구조로 취급한다.
    - 비즈니스 규칙을 담으면서 내부 자료를 숨기는 객체는 따로 생성한다.
    (여기서 내부 자료는 활성 레코드의 인스턴스일 가능성이 높다.)

## 결론

- 객체는 동작을 공개하고 자료를 숨긴다.
    - 그래서 기존 동작을 변경하지 않으면서 새 객체 타입을 추가하기 쉬운 반면, 
    기존 객체에 새 동작을 추가하기는 어렵다.
    - 그래서 기존 자료 구조에 새 동작을 추가하기는 쉬우나, 기존 함수에 새 자료 구조를 추가하기는 어렵다.
- (어떤) 시스템을 구현할 때, 새로운 자료 타입을 추가하는 유연성이 필요하면 객체가 더 적합하다.
- 다른 경우로 새로운 동작을 추가하는 유연성이 필요하면 자료 구조와 절차적인 코드가 더 적합하다.
