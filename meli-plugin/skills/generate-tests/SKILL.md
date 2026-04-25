---
description: Generates the complete test suite at three levels: unit tests (JUnit 5 + Mockito), controller tests (MockMvc) and E2E integration tests (@SpringBootTest + H2). Covers happy paths, validations, errors and edge cases of the contract.
---

Generate the tests using the implemented code and the problem contract available in the conversation. To generate the complete suite, call the `test-designer` agent with the code and the contract as context.

## Naming convention
`methodName_stateUnderTest_expectedBehavior`
Example: `createProduct_withDuplicateName_returnsConflict`

## Level 1 — Unit Tests (JUnit 5 + Mockito)
No Spring context. Test:
- Domain entities: constructors with validation, behavior methods, invariants
- Value Objects: validation on construction, equality by value
- Application Services: each business rule, each branch, each error case
- Mappers/Adapters: correct conversion in both directions

```java
@ExtendWith(MockitoExtension.class)
class ProductServiceTest {
    @Mock ProductRepository repository;
    @InjectMocks ProductService service;
}
```

## Level 2 — Controller Tests (MockMvc + @WebMvcTest)
Web layer only, services mocked with `@MockBean`. Verify:
- Valid request → correct status code and body
- Missing / invalid fields → `400 Bad Request`
- Not found → `404`, conflict → `409`, business validation → `422`
- Response headers (e.g. `Location` in POST that creates resources)

## Level 3 — E2E Integration (@SpringBootTest + H2)
Complete HTTP → DB flow. Verify:
- All endpoints: complete happy path
- Error cases with real DB state
- DB state after write operations (`repository.findById(...)`)
- Sequences: create → read → update → delete
- `@BeforeEach` clears state between tests

```java
@SpringBootTest(webEnvironment = RANDOM_PORT)
@AutoConfigureMockMvc
@ActiveProfiles("test")
class ProductIntegrationTest {
    @Autowired MockMvc mockMvc;
    @Autowired ProductRepository repository;

    @BeforeEach void setUp() { repository.deleteAll(); }
}
```

## Required configuration
`src/test/resources/application-test.properties`:
```properties
spring.datasource.url=jdbc:h2:mem:testdb;DB_CLOSE_DELAY=-1;MODE=MySQL
spring.datasource.driver-class-name=org.h2.Driver
spring.jpa.hibernate.ddl-auto=create-drop
spring.jpa.show-sql=false
```

## Output structure
```
src/test/java/
├── domain/           ← unit tests for entities and value objects
├── application/      ← unit tests for services and use cases
├── infrastructure/
│   ├── web/          ← controller tests (@WebMvcTest)
│   └── integration/  ← E2E tests (@SpringBootTest)
src/test/resources/
└── application-test.properties
```

After finishing test generation:
1. Ask the user if the problem is complete and ready to mark as solved.
2. If confirmed: look in the conversation context for the current problem path (set when `list-problems` selected it) and update its entry in `progress.json` to `solved`. If there is no path in context (the user invoked the skill directly), ask for the path.
3. Suggest running `/hackerrank-java:token-summary` to see the total session consumption.
4. Suggest running `/hackerrank-java:list-problems` to load the next problem.
