# Java Coverage Specification

## Tools
- Jacoco (Gradle, Maven)

## Commands
- With Gradle:
    - `./gradlew test`
    - `./gradlew jacocoTestReport`
    - Open `build/reports/jacoco/test/html/index.html` in your web browser for detailed coverage results.
- With Maven:
    - `mvn test`
    - `mvn jacoco:report`
    - Open `target/site/jacoco/index.html` in your web browser for detailed coverage results.

## Automation
- Add coverage commands to scripts or CI/CD workflows
- For Maven projects, use the provided automation script:
    - `./scripts/check-coverage.sh`
    - This script runs tests, generates the JaCoCo HTML report, and opens the coverage report in your browser (macOS/Linux) and prints a summary if `lynx` or `w3m` is installed.
    - Example:
      ```sh
      ./scripts/check-coverage.sh
      open target/site/jacoco/index.html
      ```
- Example GitHub Actions snippet:
    - name: Run Jacoco coverage
      run: |
        ./gradlew test
        ./gradlew jacocoTestReport

## Visualization
- After running coverage, open the HTML report in your browser for visual review.
- For Maven: `open target/site/jacoco/index.html` (macOS/Linux)
- For Gradle: `open build/reports/jacoco/test/html/index.html` (macOS/Linux)
- Highlight untested code paths using the HTML report.

# Java 11 Compatibility

- This project is restricted to Java 11.
- Ensure all test coverage tools, plugins, and dependencies are compatible with Java 11.
- For Maven Surefire, use version 3.0.0 or lower (not 3.1.0+), as higher versions may require Java 17+.
- Jacoco 0.8.11 is compatible with Java 11.
- Avoid Surefire 3.1.0+ and JUnit Jupiter 5.10+ unless upgrading to Java 17 or higher.

## JUnit 4

- Tests use `org.junit.Test`, `org.junit.Before`, `org.junit.Assert`, etc., with **`junit:junit`** (JUnit 4). Prefer **4.13.x** on Java 11; the version is often inherited from **Spring Boot** when you use **`spring-boot-starter-test`**.
- **JaCoCo** does not care whether tests are JUnit 4 or 5; coverage is collected from the same JVM agent during **`test`**.
- For **Spring Boot** services, declare **`spring-boot-starter-test`** (test scope) and let it pull the JUnit stack that matches your **Spring Boot BOM**—including what **Surefire 3.x** needs to run tests on **JUnit Platform** alongside older JUnit 4 tests. Do not exclude nested JUnit / test-engine dependencies from **`spring-boot-starter-test`** unless you understand the impact (misconfiguration can show up as tests running as **0** or not at all).
- Example (Spring Boot + JUnit 4-style tests, Surefire **3.0.0**):

```
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>3.0.0</version>
</plugin>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```

## JUnit 5 (Jupiter)

- For JUnit 5, use **Jupiter 5.9.x or lower** for Java 11 compatibility.
- Example configuration:

```
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>3.0.0</version>
</plugin>
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-engine</artifactId>
    <version>5.9.3</version>
    <scope>test</scope>
</dependency>
```
