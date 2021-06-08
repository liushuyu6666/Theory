# JUnit

## Architecture

- `TestJunit` class

  - The bottom
  - test case
  - `@Test` annotation, When `JUnitCore.runClasses(TestJunit.class)` gets called, JUnit finds all public methods annotated with `@Test` and invokes them reflectively.

- Test Suites

  - bundles unit test cases and run them together

  - `@RunWith` and `@Suite`

  - ```java
    @RunWith(Suite.class)
    
    @Suite.SuiteClasses({
            TestJunit1.class, TestJunit2.class
    })
    
    
    public class JunitTestSuite {
    }
    ```

- `TestRunner`

  - contains main function to test single case or suites

  - ```java
    import org.junit.runner.JUnitCore;
    import org.junit.runner.Result;
    import org.junit.runner.notification.Failure;
    
    public class TestRunner {
        public static void main(String[] args){
            Result result = JUnitCore.runClasses(TestJunit.class);
    
            for(Failure failure: result.getFailures()){
                System.out.println(failure.toString());
            }
    
            System.out.println(result.wasSuccessful());
        }
    }
    ```

  - 







## Annotation

### basic knowledge

[basic knowledge](https://www.tutorialspoint.com/junit/junit_using_assertion.htm)

### The difference between `Before` and `BeforeClass`

The code marked `@Before` is executed before each test, while `@BeforeClass` runs once before the entire test fixture. If your test class has ten tests, `@Before` code will be executed ten times, but `@BeforeClass` will be executed only once.

In general, you use `@BeforeClass` when multiple tests need to share the same computationally expensive setup code. Establishing a database connection falls into this category. You can move code from `@BeforeClass` into `@Before`, but your test run may take longer. Note that the code marked `@BeforeClass` is run as static initializer, therefore it will run before the class instance of your test fixture is created.

In [JUnit 5](http://junit.org/junit5/docs/current/user-guide/), the tags `@BeforeEach` and `@BeforeAll` are the equivalents of `@Before` and `@BeforeClass` in JUnit 4. Their names are a bit more indicative of when they run, loosely interpreted: 'before each tests' and 'once before all tests'.