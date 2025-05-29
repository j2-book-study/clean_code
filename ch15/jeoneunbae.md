# JUnit

JUnit 코드 내에서 클린 코드에 맞춰 코드를 수정한다.

- f 가 앞에 있는 모든 변수에서 f 를 없앤다.
    - `fExpected` → `expected`
- 부정문은 긍정문보다 이해하기 더 어려우므로 긍정문으로 변환한다.
    - `shouldNotCompacted` → `canBeCompacted`
- 함수 명과 기대값이 일치하지 않으면 함수 명을 바꾼다.
    - `canBeCompacted` → `formatCompactedComparison`

리팩터링은 코드가 어느 수준에 이를 때까지 수많은 시행착오를 반복하는 작업이기에 원래 했던 변경을 되돌리는 경우가 흔하다.

세상에 개선이 불필요한 모듈은 없다.

코드를 처음보다 조금 더 깨끗하게 만드는 책임은 우리 모두에게 있다.
