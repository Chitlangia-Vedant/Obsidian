[Test-driven development](https://en.wikipedia.org/wiki/Test-driven_development) is a software development process that's based on constructing a piece of software in small iterations. In test-driven software development, the first thing a programmer always does is write an [[Introduction to Testing#Unit Testing|automatically-executable test]], which tests a single piece of the computer program.

The test will not pass because the functionality that satisfies the test, i.e., the part of the computer program to be examined, is missing. Once the test has been written, functionality that meets the test requirements is added to the program. The tests are then run again. If all tests pass, a new test is added, or alternatively, if the tests fail, the already-written program is corrected. If necessary, the internal structure of the program will be corrected or refactored, so that the functionality of the program remains the same, but the structure becomes clearer.

Test-driven software development consists of five steps that are repeated until the functionality of the program is complete.

1. Write a test. The programmer decides which program functionality to test and writes a test for it.
2. Run the tests and check if the tests pass. When a new test is written, the tests are run. If the test passes, the test is most likely erroneous and should be corrected - the test should only test functionality that hasn't yet been implemented.
3. Write the functionality that meets the test's requirements. The programmer implements functionality that only meets the test requirements. Note: this doesn't do things that the test does not require - functionality is only added in small increments.
4. Perform the tests. If the tests fail, there is likely to be an error in the functionality written. Correct the functionality - or, if there is no error in the functionality, fix the latest test that was performed.
5. Repair the internal structure of the program. As the size of the program increases, its internal structure is adjusted as needed. Methods that are too long are broken down into multiple parts and classes representing concepts are isolated. The tests are not modified, but are instead used to verify the correctness of the changes made to the program's internal structure - if a change in the program structure changes the functionality of the program, the tests will produce a warning and the programmer can remedy the situation.

![[Pasted image 20241021211127.png]]

# Example: exercise management

We want the exercise management software to include the possibilities to list, add, and remove exercises, as well as the ability mark an exercise as completed.
## Part 1. Creating the project and including the JUnit library

Let’s start developing by creating an empty project. Do this by selecting in NetBeans File -> New Project. Choose the project category “Maven” and the project type as “Java Application”

Now we get to fill in the information for our new project. Set the project name as “exerciseManagement”. The project path tells where in the storage system the project files are located

Keep the package field empty.

![[Pasted image 20241022121656.png]]

When we press Finish, the new project is created. You can view it on the left side of NetBeans.

Next we’ll add the JUnit library. It’s meant for writing unit tests. (JUnit is included in the exercise templates downloaded from TMC.)

Click open the folder Project Files, and double click the ‘pom.xml’ file. The file details the structure of our project and the libraries it uses.

Copy the JUnit library before the line</project>:

```xml
<dependencies>
<dependency>
<groupId>
junit
</groupId>
<artifactId>
junit
</artifactId>
<version>
4.12
</version>
<scope>
test
</scope>
</dependency>
</dependencies>
```

![[Pasted image 20241022122103.png]]

## Part 2. Creating the class for unit tests

Let’s create the first unit test. You can create unit tests by right-clicking the project and choosing new -> Other.

This opens a view for creating a new file. Choose the category as “Unit Tests” and the type of the created file as “JUnit Test”.

Then press “Next”.

![[Pasted image 20241022122546.png]]

Set the class name as ‘ExerciseManagementTest’ and choose not to generate code in the class.

Ensure that the class name ends with “Test”.

Press Finish when ready.

![[Pasted image 20241022122748.png]]

Now the project folder “Test Packages” contains the class “ExerciseManagementTest”

![[Pasted image 20241022122835.png]]

## Part 3. [[Introduction to Testing#Unit Testing|Unit test]]

The test uses a class called ExerciseManagement, and expects it to have a method called exerciseList that returns the list of exercises.

The test method assertEquals receives two values as parameters -- the first is the expected value, and the second the value returned by the program.

Here the method is used for checking the size of a new exercise list: a new list should be empty.

```java
import static org.junit.Assert.assertEquals;

import org.junit.Test;

public class ExerciseManagementTest
{
	@Test
	public void exerciseListEmptyAtBeginning ()
	{
		ExerciseManagement management = new ExerciseManagement();
		assertEquals(0, management.exerciseList().size());
	}
}
```

The test is in the class ExerciseManagementTest. Once it has been created, it’s clear that it won’t pass: the class that the test uses, ExerciseManagement, is missing.

Nevertheless, let’s run the tests. This is done by right-clicking on the project and choosing “Test”.

We notice the error “Failed to execute...”

![[Pasted image 20241022123746.png]]

## Part 4. Implementing the functionality that is required by the unit tests

Let’s create the class ExerciseManagement (the class is added to the folder Source Packages). Then let’s give it a method called exerciseList that returns a list.

```java
import java.util.ArrayList;

public class ExerciseManagement{
	public ArrayList<String> exerciseList(){
		return new ArrayList<>();
	}
}
```

Run the tests by right-clicking on the project  and choosing “Test”.

The tests pass. There is no refactoring, so we’ll continue and write the next test.

![[Pasted image 20241022124301.png]]

## Summary

In test-driven development the functionality of the program is constructed in small steps. The programmer first writes a test that examines the wished functionality, and then writes the program code that passes that test.
