When program execution ends with an error, an exception is thrown. 
- For example, A program might call a method with a _null_ reference and throw a `NullPointerException`
- The program might try to refer to an element outside an array and result in an `IndexOutOfBoundsException`
# Handling exceptions
We use the `try {} catch (Exception e) {}` block structure to handle exceptions. The keyword `try` starts a block containing the code which _might_ throw an exception. the block starting with the keyword `catch` defines what happens if an exception is thrown in the `try` block. The keyword `catch` is followed by the type of the exception handled by that block, for example "all exceptions" `catch (Exception e)`.

```java
try {
    // code which possibly throws an exception
} catch (Exception e) {
    // code block executed if an exception is thrown
}
```

The code in the `catch` block is executed immediately if the code in the `try` block throws an exception

```java
Scanner reader = new Scanner(System.in);

System.out.print("Give a number: ");
int readNumber = -1;

try {
    readNumber = Integer.parseInt(reader.nextLine());
    System.out.println("Good job!");
} catch (Exception e) {
    System.out.println("User input was not a numer.");
}
```

```Output
Give a number: 5 
Good job!
```

```Output
Give a number: no! 
User input was not a number.
```

The user input, in this case the string `no!`, is given to the `Integer.parseInt` method as a parameter. The method throws an error if the string cannot be parsed into an integer. Note, that the code within the `catch` block is executed _only_ if an exception is thrown.

# Exceptions and resources

There is separate exception handling for reading operating system resources such as files. With so called try-with-resources exception handling, the resource to be opened is added to a non-compulsory part of the try block declaration in brackets.

 Reading a file might throw an exception, so it requires a try-catch block.

```java
ArrayList<String> lines =  new ArrayList<>();

// create a Scanner object for reading files
try (Scanner reader = new Scanner(new File("file.txt"))) {

    // read all lines from a file
    while (reader.hasNextLine()) {
        lines.add(reader.nextLine());
    }
} catch (Exception e) {
    System.out.println("Error: " + e.getMessage());
}

// do something with the lines
```

The try-with-resources approach is useful for handling resources, because the program closes the used resources automatically. Now references to files can "disappear", because we do not need them anymore. If the resources are not closed, the operating system sees them as being in use until the program is closed.

# Shifting the responsibility
Methods and constructors can throw exceptions. There are roughly two categories of exceptions. There are exceptions we have to handle, and exceptions we do not have to handle. We can handle exceptions by wrapping the code into a `try-catch` block or _throwing them out of the method_.

The code below reads the file given to it as a parameter line by line. Reading a file can throw an exception — for example, the file might not exist or the program does not have read rights to the file. This kind of exception has to be handled. We handle the exception by wrapping the code into a `try-catch` block. In this example we do not really care about the exception, but we do print a message to the user about it.

```java
public List<String> readLines(String fileName){
    List<String> lines =  new ArrayList<>();

    try {
        Files.lines(Paths.get("file.txt")).forEach(line -> lines.add(line));
    } catch (Exception e) {
        System.out.println("Error: " + e.getMessage());
    }

    return lines;
}
```

A programmer can also leave the exception unhandled and shift the responsibility for handling it to whomever calls the method. We can shift the responsibility of handling an exception forward by throwing the exception out of a method, and adding notice of this to the declaration of the method. Notice on throwing an exception forward `throws *ExceptionType*` is added before the opening bracket of a method.

```java
public List<String> readLines(String fileName) throws Exception {
    ArrayList<String> lines =  new ArrayList<>();
    Files.lines(Paths.get(fileName)).forEach(line -> lines.add(line));
    return lines;
}
```

Even the `main` method can throw an exception to the caller:

```java
public class MainProgram {
   public static void main(String[] args) throws Exception {
       // ...
   }
}
```

Now the exception is thrown to the program executor, or the Java virtual machine. It stops the program execution when an error causing an exception to be thrown occurs.

# Throwing exceptions
The `throw` command throws an exception.

One exception that the user does not have to prepare for is `IllegalArgumentException`. The `IllegalArgumentException` tells the user that the values given to a method or a constructor as parameters are _wrong_. It can be used when we want to ensure certain parameter values.

 The grade has to be an integer between 0 and 5. If it is something else, we want to _throw an exception_. Let's add a conditional statement to the constructor, which checks if the grade fills the criteria. If it does not, we throw the `IllegalArgumentException` with `throw new IllegalArgumentException("Grade must be between 0 and 5.");`.

```java
public class Grade {
    private int grade;

    public Grade(int grade) {
        if (grade < 0 || grade > 5) {
            throw new IllegalArgumentException("Grade must be between 0 and 5.");
        }

        this.grade = grade;
    }

    public int getGrade() {
        return this.grade;
    }
}
```

```java
Grade grade = new Grade(3);
System.out.println(grade.getGrade());

Grade illegalGrade = new Grade(22);
// exception happens, execution will not continue from here
```

```Output
3 
Exception in thread "..." java.lang.IllegalArgumentException: Grade must be between 0 and 5.
```

# Exceptions and Interfaces
An Interface can have methods which throw an exception. For example the classes implementing the following `FileServer` interface _might_ throw an exception from the methods `load` or `save`.

```java
public interface FileServer {
    String load(String fileName) throws Exception;
    void save(String fileName, String textToSave) throws Exception;
}
```

If an interface declares a `throws Exception` attribute to a method, so that these methods might throw an exception, the class implementing this interface must also have this attribute.

# Details of the exception
A `catch` block defines which exception to prepare for with `catch (Exception e)`. The details of the exception are saved to the `e` variable.

```java
try {
    // program code which might throw an exception
} catch (Exception e) {
    // details of the exception are stored in the variable e
}
```

The `Exception` class has some useful methods. For example `printStackTrace()` prints the _stack trace_, which shows how we ended up with an exception. Below is a stack trace printed by the `printStackTrace()` method.

```Output
Exception in thread "main" java.lang.NullPointerException 
at package.Class.print(Class.java:43) 
at package.Class.main(Class.java:29)
```

We read a stack trace from the bottom up. At the bottom is the first call, so the execution of the program has begun from the `main()` method of the `Class` class. Line 29 of the main method of the `Class` class calls the `print()` method. Line 43 of the `print` method has thrown a `NullPointerException` exception. The details of an exception are very useful when trying to pinpoint where an error happens.