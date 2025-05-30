### 단위 테스트의 중요성

모듈의 완성도를 높이기 위해서는 단위 테스트가 필수적이다.

코드 커버리지는 실행 가능한 문장 중 실제 실행한 문장의 비율을 나타내는 지표이다. 이를 통해 해당 모듈의 테스트 범위가 적절했는지 판단이 가능하다.(코드 커버리지 100%를 달성할 수 있는 모듈을 개발하는데에 목적을 둬야겠다는 생각을 했다)

### 명확한 추상화 수준의 네이밍

구현을 암시하는 클래스명보다는 추상화 수준에서 “무엇을 하는가”에 초점을 맞춰 메서드명을 정해야 한다.

ex) `calculateEndOfMonth()` vs `DateUtil.calculate()`  

### 타입 안전성과 제약 조건 반영

특성의 타입이 int라고 할지라도 해당 특성이 enum 형태로 구분된다면 enum을 활용하는 것이 더 좋다.(제약에 의한 값만 선택할 수 있게, 예를 들어 월은 12월까지인데 13이상이나 0이하를 아예 받지 못하게 하는 것)

### 클래스 간 의존성 역전

일반적으로 기반 클래스는 파생 클래스를 몰라야 바람직하다. 따라서 ABSTRACT FACTORY 패턴을 적용해 구현 factory 클래스를 만들고 추상 클래스의 인스턴스를 생성하고 구현하는 로직 메서드를 추가하는 방법을 활용하면 된다.

### 패턴 조합: 싱글톤, 데코레이터, 추상 팩토리

정적 메서드의 호출을 직접 구현에 의존하지 않고,

추상 메서드에 위임하는 구조를 구성함으로써 유연하고 확장 가능한 설계를 구현할 수 있다.

이때 인스턴스 생성 및 책임 분리는 다음 세 가지 패턴의 조합으로 구성된다:

1. Singleton
    - 팩토리 인스턴스를 전역에서 하나만 생성해 재사용
    - 애플리케이션 시작 시점에 초기화되며, 이후 정적 메서드를 통해 접근
2. Decorator
    - 핵심 팩토리 구현체를 감싸 부가기능을 동적으로 추가
    - 예: 로깅, 검증, 캐싱 등의 기능을 조건에 따라 구성 가능
3. Abstract Factory
    - 실제 구현체 생성 로직을 인터페이스로 추상화
    - 런타임에 서로 다른 구현체를 선택하거나, 테스트 대역으로 교체 가능

```java
// 정적 유틸 메서드
public class DateUtil {
    public static SerialDate createDate(int year, int month, int day) {
        return DateFactoryProvider.getInstance().createDate(year, month, day);
    }
}

// 싱글톤 팩토리 제공자
public class DateFactoryProvider {
    private static final DateFactory INSTANCE = createFactory();

    public static DateFactory getInstance() {
        return INSTANCE;
    }

    private static DateFactory createFactory() {
        DateFactory base = new RealDateFactory();
        return new LoggingDateFactory(base); // INSTANCE에 들어가는 값
    }
}

// 추상 팩토리 인터페이스
public interface DateFactory {
    SerialDate createDate(int year, int month, int day);
}

// 구현체 데코레이터(로깅)
public class LoggingDateFactory implements DateFactory {
    private final DateFactory delegate;

    public LoggingDateFactory(DateFactory delegate) {
        this.delegate = delegate;
    }

    @Override
    public SerialDate createDate(int year, int month, int day) {
        System.out.printf("Creating date: %d-%02d-%02d\n", year, month, day);
        return delegate.createDate(year, month, day);
    }
}

// 구현체(날짜 생성)
public class RealDateFactory implements DateFactory {
    @Override
    public SerialDate createDate(int year, int month, int day) {
        return new DayDate(year, month, day);
    }
}
```

- 이렇게 했을 때의 장점
    - 하드코딩된 내부 로직 대신 추상화된 객체에 위임하여 테스트와 교체가 용이함
    - 정적 메서드로 간결하게 호출 가능하지만, 내부는 객체지향 원칙에 따라 설계됨
    - 부가기능(로깅, 캐싱, 검증 등)을 데코레이터로 유연하게 추가/제거 가능
    - 테스트 환경에서는 팩토리만 교체함으로써 전체 로직을 쉽게 대체 가능

### SerialDate 리팩터링 과정

1. 처음에 나오는 너무 오래된 주석을 간단하게 고치고 개선했다.
2. enum을 모두 독자적인 소스 파일로 옮겼다.
3. 정적 변수와 정적 메서드를 DateUtil이라는 새 클래스로 옮겼다.
4. 일부 추상 메서드를 DayDate 클래스로 끌어올렸다.
5. Month.make를 Month.formInt로 변경했다. 다른 enum도 똑같이 변경했다. 또한 모든 enum에 toInt() 접근자를 생성하고 index 필드를 private으로 정의했다.
6. plusYears와 plusMonths에 흥미로운 중복이 있었다. correctLastDayOfMonth라는 새 메서드를 생성해 중복을 없앴다.
7. 숫자 1을 없앴다. 모두 Month.JANUARY.toInt() 혹은 Day.SUNDAY.toInt()로 적절히 변경했다. SpreadsheetDate 코드를 살펴보고 알고리즘을 손봤다.