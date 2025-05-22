### 점진적인 개선이 필요한 일반적인 경우

기능은 되지만 구조적으로 단순화되어 있지 않고 복잡하게 되어 있는 경우, 변경에 취약하고 너무 많은 조건문이 하나의 메서드 안에 포함되어 있는 경우, 캡슐화가 부족해 테스트의 어려움을 겪는 경우 등 점진적인 개선이 필요한 소프트웨어의 특징은 아래와 같다.

- 기능 중심으로 구현
    - 기능은 되지만 구조는 엉킴
    - ex) 데이터 처리, 필터링, 출력이 한 메서드에 몰려 있음
    
    ```java
    public class ReportGenerator {
        public void generate() {
            // 데이터 로딩
            List<String> data = loadFromFile();
    
            // 데이터 필터링
            List<String> filtered = new ArrayList<>();
            for (String row : data) {
                if (row.contains("ERROR")) {
                    filtered.add(row);
                }
            }
    
            // 출력
            for (String row : filtered) {
                System.out.println("Report: " + row);
            }
        }
    
        private List<String> loadFromFile() {
            return List.of("INFO: start", "ERROR: fail", "DEBUG: done");
        }
    }
    ```
    
- 타입/조건별 분기 사용
    - 변경에 취약하고 조건문이 다수 존재
    - ex) 새로운 회원 등급 추가 시 매번 if문 추가
    
    ```bash
    public class DiscountService {
        public int getDiscount(String userType) {
            if ("VIP".equals(userType)) {
                return 20;
            } else if ("GOLD".equals(userType)) {
                return 10;
            } else if ("SILVER".equals(userType)) {
                return 5;
            } else {
                return 0;
            }
        }
    }
    ```
    
- 데이터와 동작이 분리되지 않음
    - 캡슐화되어 있지 않고 테스트 어려움
    - ex) User 클래스가 자신의 행동(전체 이름 만들기)을 외부에 맡김
    
    ```java
    public class User {
        public String firstName;
        public String lastName;
    }
    
    public class UserService {
        public String getFullName(User user) {
            return user.firstName + " " + user.lastName;
        }
    }
    ```
    
- 하나의 클래스가 많은 책임을 짐
    - SRP를 위반해 응집도가 낮음
    - ex) 하나의 클래스가 도메인, 검증, 저장, 알림까지 모두 담당
    
    ```java
    public class OrderManager {
        public void placeOrder(String productId) {
            // 1. 유효성 검사
            if (productId == null || productId.isEmpty()) {
                throw new IllegalArgumentException("Invalid product ID");
            }
    
            // 2. 재고 확인
            System.out.println("Checking stock for " + productId);
    
            // 3. 주문 저장
            System.out.println("Saving order for " + productId);
    
            // 4. 이메일 전송
            System.out.println("Sending confirmation email");
        }
    }
    ```
    

### 확장성이 부족한 초기 구조

기능은 모두 정상적으로 작동하지만 **확장성이 부족한 모듈**을 어떻게 점진적으로 개선하는지 그 방법에 대해 소개하겠다.

이 장에서는 명령행 인수 파서를 사례를 들어 점차 구조를 개선해 나가는 과정을 보여준다.

초기 Args 클래스는 다음과 같은 특징을 가진다:

- 스키마 문자열을 통해 인수의 타입을 정의한다.
- 입력된 명령행 인수를 타입별로 파싱하여 내부에 저장한다.
- getBoolean(), getInt(), getString() 등의 메서드를 통해 값을 조회한다.

작고 간단한 명령행 인수 처리에는 잘 작동하지만,

하나의 메서드 안에 인수 및 스키마 파싱, 값 저장 로직 등이 포함되어 있어 SRP를 위반할 뿐만 아니라 새로운 타입이 생기면 기존 로직에서 모두 수정이 발생해 문제가 존재한다.

### 점진적인 개선 방향

1. 타입별 파싱 전략을 분리
    1. ArgMarshaler라는 인터페이스를 도입해 타입별 파서(BooleanArgumentMarshaler, StringArgumentMarshaler, IntegerArgumentMarshaler 등)를 분리
2. Map<Character, ArgMarshaler> 구조로 전환
    1. 각 인수 타입마다 고유의 파싱/저장 책임을 가진 클래스를 등록
    2. getXXX(char) 메서드도 각 ArgMarshaler에 위임하여 간결해짐

### 개선 후 효과

- **유지보수성 향상**: 코드 수정 없이 기능 확장 가능
- **테스트 용이성 증가**: 각 타입별 파서를 개별 단위로 테스트 가능
- **가독성과 의도 전달력 증가**: 클래스 및 메서드 이름만 봐도 역할을 이해할 수 있음