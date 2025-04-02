## 왜 테스트 코드가 실제 코드만큼 중요할까?

- **내가 만든 코드가 안전한지 확인하는 보증 수표**
- 테스트  코드는 실제 코드의 유연성, 유지보수성, 재사용성을 보존하고 강화할 수 있는 수단이다.
- 테스트 코드가 없다면 실제 코드에서의 모든 변경이 잠정적인 버그이다.
- 깨끗한 테스트 코드를 만들면, **미지의 버그**를 걱정하지 않고 실제 코드를 변경할 수 있다.
---
## TDD 법칙 세가지

- 첫째, 실패하는 단위 테스트를 작성할 때까지 실제 코드를 작성하지 않는다.
    - 이 코드가 어떤 동작을 해야하는지 명확하게 파악할 수 있다. 의도에 따라 실제 코드를 작성하기 위함이다.
- 둘째, 컴파일은 실패하지 않으면서 실행이 실패하는 정도로만 단위 테스트를 작성한다.
    - 테스트 코드로 요구사항에 맞게 구현하고, 실제 코드에서는 가짜를 구현해 컴파일은 실패하지 않게 한다. 테스트 주도 개발을 위한 시작점이라고 할 수 있다.
- 셋째, 현재 실패하는 테스트를 통과할 정도로만 실제 코드를 작성한다.
    - 과도한 구현을 방지하기 위함인데, 이는 빠른 피드백을 통해 안정적이고 유연한 개발을 가능하게 한다.

### 테스트 당 assert 하나

- assert 문은 테스트 하나에 하나만 사용해야 한다.
- 테스트 코드에서 가장 중요한 특징은 가독성이다. 테스트 코드를 이해하기 쉬워야 실제 코드가 수행하는 기능을 이해하기 쉽다.
- assert 문이 하나인 함수는 결론이 하나라서 코드를 이해하기 쉽고 빠르다.

### 테스트 당 개념 하나

- 하나의 테스트 안에 여러 개를 테스트하는 경우, 코드에 내재된 일반적인 규칙을 이해하기 어렵다. 코드가 의도하는 일반적인 규칙을 먼저 이해하고, 이 규칙들을 확인할 수 있는 개념들을 각각 표현해야 한다.
    - 날짜에 어떤 달을 더하면 날짜는 그 달의 마지막 날짜보다 커지지 못한다.
        - 30일 → 30일로 끝나는 달의 마지막 날짜에 한 달을 더하면 30일이 되어야지 31일이 되면 안된다.
        - 31일 → 31일로 끝나는 달의 마지막 날짜에 한 달을 더하면 30일이 되어야지 31일이 되면 안된다.
        - 28일 → 28일로 끝나는 달의 마지막 날짜에 한 달을 더하면 28일이 되어야지 31일이 되면 안된다.
---
## 깨끗한 테스트 코드

### F.I.R.S.T.

- Fast: 테스트는 자주 돌려 실제 코드의 가용성을 확인해야 한다. 특히, 초반에 자주 돌리지 않으면 문제를 찾아내기 어렵다.
- Independent: 각 테스트는 서로 의존하면 안된다. 순서에 상관없이 테스트 가능해야한다. 테스트가 연결되어 있다면 실패가 발생하였을 때 원인 추적이 어려워지고 실패 이후 테스트에 대한 버그를 찾기 어렵다.
- Repeatable: 환경에 상관없이 반복 가능한 테스트 코드여야 한다. 테스트 코드가 돌아가지 않는 환경이 하나라도 있다면 실제 코드의 구멍을 발견하는 것이다.
- Self-Validating: 성공 아니면 실패로 결과가 나와야 한다. 통과 여부를 알기 위해서 제 3자의 작업이 끼어들어 비교문을 사용한다든지, 로그 파일을 읽게 만들어서는 안된다.
- Timely: 테스트 작성은 적시에 해야한다. 실제 코드를 구현한 다음에 테스트 코드를 만들면 실제 코드가 테스트하기 어렵다는 사실을 발견할 수 있다.

### TDD 설계와 깨끗한 테스트 코드 법칙에 따른 test 코드 예시

- 기능 테스트를 위해 필요한 데이터들을 정리(변수, 매개변수 명을 통해 쉽게 확인 가능)
- 리포지토리 동작을 위한 설정이 추가됨(when 절을 통해 확인)
- createProduct 라는 상품 생성 메서드 호출
- 결론은 하나로 설정(assertThat 은 하나만 사용해 상품 등록이 되었는지 확인)

```java
@Test
	@Description("상품 등록 테스트 - 성공")
	void createProduct() {
		// given
		ProductCreateRequestServiceDto requestDto = new ProductCreateRequestServiceDto(
			COMPANY_ID,
			PRODUCT_CODE,
			PRODUCT_NAME,
			PRICE,
			STOCK
		);
		CompanyDetailsSearchResponseDto company = new CompanyDetailsSearchResponseDto(
			COMPANY_ID, COMPANY_NAME, COMPANY_MANAGER_ID, COMPANY_TYPE_SUPPLIER, COMPANY_MANAGE_HUB_ID, COMPANY_ADDRESS
		);

		HubSearchResponseDto hub = new HubSearchResponseDto(
			COMPANY_MANAGE_HUB_ID, HUB_NAME, HUB_ADDRESS, HUB_LATITUDE, HUB_LONGITUDE, HUB_MANAGER_ID
		);

		when(companyClient.getCompanyById(COMPANY_ID)).thenReturn(Optional.of(company));
		when(hubClient.getHubById(COMPANY_MANAGE_HUB_ID)).thenReturn(Optional.of(hub));
		when(productRepository.existsByProductCodeAndIsDeletedFalse(PRODUCT_CODE)).thenReturn(false);
		when(productApplicationMapper.toCreateEntity(requestDto, COMPANY_NAME, COMPANY_MANAGE_HUB_ID)).thenReturn(product);
		when(productRepository.save(product)).thenReturn(product);

		// when
		ProductCreateResponseServiceDto response = productService.createProduct(requestDto, HUB_MANAGER_ID, UserRoleType.HUB_MANAGER);

		// then
		assertThat(response.id()).isEqualTo(PRODUCT_ID);
	}
```