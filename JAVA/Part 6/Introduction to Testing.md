# Stack Trace

When an error occurs in a program, the program typically prints something called a stack trace, i.e., the list of method calls that resulted in the error.

For example, a stack trace might look like this:

```Output
Exception in thread "main" ... at Program.main(Program.java:15)
```

The type of error is stated at the beginning of the list, and the following line tells us where the error occurred. The line "at Program.main(Program.java:15)" says that the error occurred at line number 15 in the Program.java file.

[[JAVA/Extra#Checklist for Troubleshooting|Checklist for Troubleshooting]]

# Passing Test Input to Scanner

The test input can be given as a string to the Scanner object in the constructor. Each line break in the test feed is marked on the string with a combination of a backslash and an n character "\n".

```java
String input = "one\n" + "two\n"  +
                "three\n" + "four\n" +
                "five\n" + "one\n"  +
                "six\n";

Scanner reader = new Scanner(input);

ArrayList<String> read = new ArrayList<>();

while (true) {
    System.out.println("Enter an input: ");
    String line = reader.nextLine();
    if (read.contains(line)) {
        break;
    }

    read.add(line);
}

System.out.println("Thank you!");

if (read.contains("six")) {
    System.out.println("A value that should not have been added to the group was added to it.");
}
```


# Unit Testing

One solution to this is unit testing, where small parts of the program are tested in isolation.

Unit testing refers to the testing of individual components in the source code, such as classes and their provided methods. The writing of tests reveals whether each class and method observes or deviates from the guideline of each method and class having a single, clear responsibility. The more responsibility the method has, the more complex the test. If a large application is written in a single method, writing tests for it becomes very challenging, if not impossible. Similarly, if the application is broken into clear classes and methods, then writing tests is straightforward.

>Ready-made unit test libraries are commonly used in writing tests, which provide methods and help classes for writing tests. The most common unit testing library in Java is [JUnit](http://junit.org/) , which is also supported by almost all programming environments. For example, NetBeans can automatically search for JUnit tests in a project — if any are found, they will be displayed under the project in the Test Packages folder.

## Example

Let's assume that we have the following Calculator class and want to write automated tests for it.

```java
public class Calculator {

    private int value;

    public Calculator() {
        this.value = 0;
    }

    public void add(int number) {
        this.value = this.value + number;
    }

    public void subtract(int number) {
        this.value = this.value + number;
    }

    public int getValue() {
        return this.value;
    }
}
```

Unit test writing begins by creating a test class, which is created under the Test-Packages folder. When testing the `Calculator` class, the test class is to be called `CalculatorTest`. The string `Test` at the end of the name tells the programming environment that this is a test class. Without the string Test, the tests in the class will not be executed.

**(Note: Tests are created in NetBeans under the Test Packages folder.)**

Tests are methods of the test class where each test tests an individual unit. Let's begin testing the class — we start off by creating a test method that confirms that the newly created calculator's value is intially 0.

```java
import static org.junit.Assert.assertEquals;
import org.junit.Test;

public class CalculatorTest {

    @Test
    public void calculatorInitialValueZero() {
        Calculator calculator = new Calculator();
        assertEquals(0, calculator.getValue());
    }
}
```

In the calculatorInitialValueZero method a calculator object is first created. The assertEquals method provided by the JUnit test framework is then used to check the value. The method is imported from the Assert class with the import Static command, and it's given the expected value as a parameter - 0 in this instance - and the value returned by the calculator. If the values of the assertEquals method values ​​differ, the test will not pass. Each test method should have an "annotation" `@ Test`. This tells the JUnit test framework that this is an executable test method.

To run the tests, select the project with the right-mouse button and click Test.

>Running the tests prints to the output tab (typically at the bottom of NetBeans) that contains some information specific to each test class. In the example below, tests of the CalculatorTest class from the package are executed. The number of tests executed were 1, none of which failed — failure in this context means that the functionality tested by the test did not work as expected. ->

```Sample output
Testsuite: CalculatorTest Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.054 sec

test-report: test: BUILD SUCCESSFUL (total time: 0 seconds)
```

Let's add functionality for adding and subtracting to the test class.

```java
import static org.junit.Assert.assertEquals;
import org.junit.Test;

public class CalculatorTest {

    @Test
    public void calculatorInitialValueZero() {
        Calculator calculator = new Calculator();
        assertEquals(0, calculator.getValue());
    }

    @Test
    public void valueFiveWhenFiveAdded() {
        Calculator calculator = new Calculator();
        calculator.add(5);
        assertEquals(5, calculator.getValue());
    }

    @Test
    public void valueMinusTwoWhenTwoSubstracted() {
        Calculator calculator = new Calculator();
        calculator.subtract(2);
        assertEquals(-2, calculator.getValue());
    }
}
```

Executing the tests produces the following output.

```Output
Testsuite: CalculatorTest Tests run: 3, Failures: 1, Errors: 0, Skipped: 0, Time elapsed: 0.059 sec

Testcase: valueMinusTwoWhenTwoSubstracted(CalculatorTest): FAILED expected:<-2> but was:<2> junit.framework.AssertionFailedError: expected:<-2> but was:<2> at CalculatorTest.valueMinusTwoWhenTwoSubstracted(CalculatorTest.java:25)

Test CalculatorTest FAILED test-report: test: BUILD SUCCESSFUL (total time: 0 seconds)
```