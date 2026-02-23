# Java Test Specification

## Table of Contents
1. [Purpose](#purpose)
2. [Supported Frameworks & Java Version](#supported-frameworks--java-version)
3. [Test Framework Detection](#test-framework-detection)
4. [Test Structure](#test-structure)
5. [Dependency Mocking and Test Setup Requirements](#dependency-mocking-and-test-setup-requirements)
6. [Best Practices](#best-practices)
7. [Coverage & Testing Instructions](#coverage--testing-instructions)
8. [Checklist for Every Method](#checklist-for-every-method)
9. [Advanced Mockito Usage for Private/Protected Methods](#advanced-mockito-usage-for-privateprotected-methods)
10. [Examples](#examples)

## Purpose
Define strict requirements and best practices for generating production-ready Java unit and integration tests, ensuring high coverage and maintainability.

## Supported Frameworks & Java Version
- **Frameworks:** JUnit 4 (default), Mockito (mocking), Spring Test, Spring Boot Starter Test, REST Docs MockMvc, Trustwave Test Util, SLF4J Simple (test logging)
- **Version:** Java 11 only (use only Java 11 features)
- **Library Versions:**
  - JUnit: 4.x
  - **Maven Surefire:** Use the latest Java 11 compatible version (currently 3.0.0-M9 or newer, but not requiring Java 17+)
  - **JaCoCo:** Use the latest Java 11 compatible version (currently 0.8.11)
  - Avoid versions requiring Java 17+.

## Test Framework Detection
- Detect frameworks from build files (pom.xml, build.gradle).
- Use JUnit and Mockito unless another is detected.

## Test Structure
- Test files: `*Test.java`
- Test classes: Use `@Test` for each test method.
- Use `@Before`/`@After` for setup/teardown.
- Always import from the original class to avoid import errors.
- **Follow the Arrange-Act-Assert (AAA) pattern in all test methods for clarity:**
    - Arrange: Set up test data and mocks
    - Act: Call the method under test
    - Assert: Verify the result
- **Test Method Naming Convention:**
    - Use the pattern: `methodUnderTest_condition_expectedResult`
    - Example: `add_twoPositiveNumbers_returnsSum`, `login_invalidPassword_throwsException`
    - This improves readability and makes test intent clear.

## Dependency Mocking and Test Setup Requirements
- **If the class under test does not use constructor injection for its dependencies, refactor it to use constructor injection before writing or generating tests.**
    - Constructor injection makes all dependencies explicit, improves testability, and avoids issues with field/setter injection or hidden dependencies.
    - This ensures that all required dependencies can be easily mocked and injected in tests, and that the test setup matches the actual implementation.
- Always read the full constructor and class implementation of the class under test.
- Identify all required constructor arguments and their types.
- Mock all required dependencies as per the constructor signature.
- Only stub/mock methods that are actually called by the method under test.
- Ensure the test setup matches the actual implementation, preventing missing or extra mocks.
- If the constructor changes, update the test setup accordingly.
- Never assume a single dependency—always verify against the actual code.
- **Declare all shared mocks for dependencies (e.g., services, repositories, APIs) as private class-level variables in the test class.**
- **Initialize these shared mocks in the `@Before` (JUnit 4) or `@BeforeEach` (JUnit 5) setup method.**
- **Always inject these mocks into the class under test when constructing it, to ensure the correct mock is used and to avoid issues with mock configuration or verification.**
- **Avoid creating new mocks inside individual test methods unless the mock’s behavior must be unique to that test and cannot be shared.**

## Best Practices
- Use assertions for validation.
- Isolate tests; mock dependencies with Mockito.
- Use Spring Test utilities for integration/unit tests.
- Use REST Docs MockMvc for API documentation tests.
- Indirectly test private methods via public methods.

## Coverage & Testing Instructions
- Test every public method.
- Cover all logical branches (if/else, switch, loops).
- Cover exception/error paths.
- Test edge cases, invalid/null/empty/boundary inputs.
- Use parameterized tests for multiple scenarios.
- Add integration tests for service/repository layers.
- Cover all business logic, data transformations, and error handling.
- Regularly review coverage reports and add tests for uncovered lines/branches.
- Iterate until 95%+ coverage is achieved and all tests pass.
- Fix all import and compatibility issues.

## Checklist for Every Method
- [ ] All public methods tested
- [ ] All logical branches (if/else, switch, loops)
- [ ] Exception/error paths
- [ ] Edge cases, invalid/null/empty/boundary inputs
- [ ] Parameterized tests for multiple scenarios
- [ ] Integration tests for service/repository layers
- [ ] Business logic, data transformations, error handling
- [ ] Tests match actual method behavior (exceptions vs. error responses)
- [ ] Coverage reviewed and iterated until 95%+ achieved
- [ ] Summary of coverage and justification for any uncovered code

## Advanced Mockito Usage for Private/Protected Methods
- **Do not attempt to mock, stub, or verify private methods.** Tests should trust that private methods work according to their current implementation.
- If you must control the behavior of a protected or package-private method for testing, you may use Mockito.spy and doReturn()/doThrow() to stub those methods in the class under test.
- Avoid using PowerMock or reflection to access private methods, as it breaks encapsulation and maintainability.
- Example:
```java
MyService spyService = spy(new MyService());
doReturn(mockedValue).when(spyService).packagePrivateOrProtectedMethod(args);
```
- Always prefer testing private logic through public APIs. Only use spies for legacy or hard-to-refactor code.

## When to Use Mocks vs Real Objects

- **Use mocks** for dependencies (services, repositories, APIs, etc.) to control their behavior, simulate errors, or verify interactions. Mock when the real object is slow, has side effects, or is hard to set up.
- **Use real objects** for simple data holders (POJOs/DTOs), the unit under test, and standard library classes such as ObjectMapper. Use real objects when you need to ensure fields and getters/setters work as expected, or to avoid issues with mock behavior (e.g., unstubbed methods returning null).
- **Do not mock or stub ObjectMapper or other standard library classes in normal test scenarios.** Only mock ObjectMapper if you need to simulate deserialization/serialization errors or other non-standard behavior, and document the reason in the test. Ensure the service under test is always injected with the same ObjectMapper instance as the test, if mocking is required.
- **Rule of thumb:**
    - Mock dependencies and collaborators.
    - Use real objects for simple data classes, the class under test, and standard library classes (e.g., ObjectMapper).

**Example:**
```java
// Good: mock a service dependency
MyService service = mock(MyService.class);
when(service.doSomething()).thenReturn("result");

// Good: use a real POJO for input data
MyPojo pojo = new MyPojo();
pojo.setField("value");

// Good: use a real object for the class under test
MyComponent component = new MyComponent(service);

// Good: use a real ObjectMapper for JSON operations
ObjectMapper objectMapper = new ObjectMapper();
MyJsonClass obj = objectMapper.readValue(jsonString, MyJsonClass.class);
```

## Examples

**Simple Unit Test (with Naming Convention)**
```java
import org.junit.Test;
import static org.junit.Assert.*;

public class CalculatorTest {
    @Test
    public void add_twoPositiveNumbers_returnsSum() {
        // Arrange
        int a = 1;
        int b = 2;
        // Act
        int result = Calculator.add(a, b);
        // Assert
        assertEquals(3, result);
    }
}
```

**Mockito Example (with Naming Convention)**
```java
import org.junit.Test;
import org.mockito.Mockito;
import static org.junit.Assert.*;

public class ServiceTest {
    @Test
    public void fetchValue_dependencyReturns42_returns42() {
        // Arrange
        Dependency dep = Mockito.mock(Dependency.class);
        Mockito.when(dep.getValue()).thenReturn(42);
        Service service = new Service(dep);
        // Act
        int value = service.fetchValue();
        // Assert
        assertEquals(42, value);
    }
}
```
