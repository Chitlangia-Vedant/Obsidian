Let's get to know collections, such as lists, as streams of values. Stream is a way of going through a collection of data such that the programmer determines the operation to be performed on each value. No record is kept of the index or the variable being processed at any given time.

With streams, the programmer defines a sequence of events that is executed for each value in a collection. An event chain may include dumping some of the values, converting values ​​from one form to another, or calculations. A stream does not change the values ​​in the original data collection, but merely processes them. If you want to retain the transformations, they need to be compiled into another data collection.

## Example 01
```java
// counting the number of values divisible by three
long numbersDivisibleByThree = inputs.stream()
    .mapToInt(s -> Integer.valueOf(s))
    .filter(number -> number % 3 == 0)
    .count();
```

A stream can be formed from any object that implements the [Collection](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html) interface (e.g., ArrayList, HashSet, HashMap, ...) with the `stream()` method. The string values ​​are then converted ("mapped") to integer form using the stream's `mapToInt(value -> conversion)` method. The conversion is implemented by the `valueOf` method of the `Integer` class, which we've used in the past. We then use the `filter (value -> filter condition)` method to filter out only those numbers that are divisible by three for further processing. Finally, we call the stream's `count()` method, which counts the number of elements in the stream and returns it as a `long` type variable.

## Example 02
```java
/ working out the average
double average = inputs.stream()
    .mapToInt(s -> Integer.valueOf(s))
    .average()
    .getAsDouble();
```

Calculating the average is possible from a stream that has the `mapToInt` method called on it. A stream of integers has an `average` method that returns an [OptionalDouble](https://docs.oracle.com/javase/8/docs/api/java/util/OptionalDouble.html)-type object. The object has `getAsDouble()` method that returns the average of the list values as a `double` type variable.

## A brief summary of the stream methods

|Purpose and method|Assumptions|
|---|---|
|Stream formation: `stream()`|The method is called on collection that implements the Collection interface, such as an ArrayList Object. Something is done on the created stream.|
|Converting a stream into an integer stream: `mapToInt(value -> another)`|The stream transforms into one containing integers. A stream containing strings can be converted using, for instance, the valueOf method of the Integer class. Something is done with the stream containing integers.|
|Filtering values: `filter(value -> filter condition)`|The elements that do not satisfy the filter condition are removed from the string. On the right side of the arrow is a statement that returns a boolean. If the boolean is `true`, the element is accepted into the stream. If the boolean evaluates to false, the value is not accepted into the stream. Something is done with the filtered values.|
|Calculating the average: `average()`|Returns a OptionalDouble-type object that has a method `getAsDouble()` that returns a value of type `double`. Calling the method `average()` works on streams that contain integers - they can be created with the `mapToInt` method.|
|Counting the number of elements in a stream: `count()`|Returns the number of elements in a stream as a `long`-type value.|

# Lambda Expressions
 A _lambda expression_, is shorthand provided by Java for anonymous methods that do not have an "owner", i.e., they are not part of a class or an interface. The function contains both the parameter definition and the function body. The same function can be written in several different ways.
 
```java
// original
*stream*.filter(value -> value > 5).*furtherAction*

// is the same as
*stream*.filter((Integer value) -> {
    if (value > 5) {
        return true;
    }

    return false;
}).*furtherAction*
```

The same can be written explicitly so that a static method is defined in the program, which gets used within the function passed to the stream as a parameter.

```java
public class Screeners {
    public static boolean greaterThanFive(int value) {
        return value > 5;
    }
}
```

```java
// original
*stream*.filter(value -> value > 5).*furtherAction*

// is the same as
*stream*.filter(value -> Screeners.greaterThanFive(value)).*furtherAction*
```

The function can also be passed directly as a parameter. The syntax found below `Screeners::greaterThanFive` is saying: "use the static `greaterThanFive` method that's in the `Screeners` class".

```java
// is the same as
*stream*.filter(Screeners::greaterThanFive).*furtherAction*
```

Functions that handle stream elements ​​cannot change values ​​of variables outside of the function. This has to do with how static methods behave - during a method call, there is no access to any variables outside of the method. With functions, the values ​​of variables outside the function can be read, assuming that those values of those variables do not change in the program.

```java
int numberOfMappedValues = 0;

// determining the number of values divisible by three
long numbersDivisibleByThree = inputs.stream()
    .mapToInt(s -> {
        // variables declared outside of an anonymous function cannot be used, so this won't work
        numberOfMappedValues++;
        return Integer.valueOf(s);
    }).filter(value -> value % 3 == 0)
    .count();
```

# Stream Methods

Stream methods can be roughly divided into two categories: 
	1. intermediate operations intended for processing elements ​​ 
	2. terminal operations that end the processing of elements

### Intermediate operations
Intermediate operations return a value that can be further processed - you could, in practice, have an infinite number of intermediate operations chained sequentially (& separated by a dot).

Ex. `filter` and `mapToInt`

### Terminal operation
A terminal operation returns a value to be processed, which is formed from, for instance, stream elements.

Ex `average`

## How stream works.

The starting point 
- (1) is a list with values. When the `stream()` method is called on a list, 
- (2) a stream of list values ​​is created. The values ​​are then dealt with individually. The stream values ​​can be 
- (3) filtered by the `filter` method, which removes values ​​that fail to meet the condition from the stream. The stream's `map` method 
- (4) can be used to map values ​​in a stream from one form to another. The `collect` method 
- (5) collects the values ​​in a stream into a collection provided to it, such as a list.

![[Pasted image 20260220170348.png]]

```java
List<Integer> list = new ArrayList<>();
list.add(3);
list.add(7);
list.add(4);
list.add(2);
list.add(6);

ArrayList<Integer> values = list.stream()
    .filter(value -> value > 5)
    .map(value -> value * 2)
    .collect(Collectors.toCollection(ArrayList::new));
```

## Terminal Operations

Let's take a look at four terminal operations: 
- the `count` method for counting the number of values on a list, 
- the `forEach` method for going a through list values, 
- the `collect` method for gathering the list values ​​into a data structure, and 
- the `reduce` method for combining the list items.

### count
The `count` method informs us of the number of values in the stream as a `long`-type variable.

```java
List<Integer> values = new ArrayList<>();
values.add(3);
values.add(2);
values.add(17);
values.add(6);
values.add(8);

System.out.println("Values: " + values.stream().count());
```

```Output
Values: 5
```

### forEach
The `forEach` method defines what is done to each list value and terminates the stream processing. In the example below, we first create a list of numbers, after which we only print the numbers that are divisible by two.

```java
List<Integer> values = new ArrayList<>();
values.add(3);
values.add(2);
values.add(17);
values.add(6);
values.add(8);

values.stream()
    .filter(value -> value % 2 == 0)
    .forEach(value -> System.out.println(value));
```

```Output
2 
6 
8
```

### collect

You can use the `collect` method to collect stream values into another collection. The example below creates a new list containing only positive values. The `collect` method is given as a parameter to the [Collectors](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html) object to which the stream values ​​are collected - for example, calling `Collectors.toCollection(ArrayList::new)` creates a new ArrayList object that holds the collected values.

```java
List<Integer> values = new ArrayList<>();
values.add(3);
values.add(2);
values.add(-17);
values.add(-6);
values.add(8);

ArrayList<Integer> positives = values.stream()
    .filter(value -> value > 0)
    .collect(Collectors.toCollection(ArrayList::new));

positives.stream()
    .forEach(value -> System.out.println(value));
```

```Output
3 
2
8
```

### reduce

The `reduce` method is useful when you want to combine stream elements to some other form. The parameters accepted by the method have the following format: `reduce(*initialState*, (*previous*, *object*) -> *actions on the object*)`.

As an example, you can calculate the sum of an integer list using the reduce method as follows.

```java
ArrayList<Integer> values = new ArrayList<>();
values.add(7);
values.add(3);
values.add(2);
values.add(1);

int sum = values.stream()
    .reduce(0, (previousSum, value) -> previousSum + value);
System.out.println(sum);
```

```Output
13
```

In the same way, we can form a combined row-separated string from a list of strings.

```java
ArrayList<String> words = new ArrayList<>();
words.add("First");
words.add("Second");
words.add("Third");
words.add("Fourth");

String combined = words.stream()
    .reduce("", (previousString, word) -> previousString + word + "\n");
System.out.println(combined);
```

```Output
First 
Second 
Third 
Fourth
```

## Intermediate Operations

Intermediate stream operations are methods that return a stream. Since the value returned is a stream, we can call intermediate operations sequentially. 
- Converting a value from one form to another using `map`
	- Its more specific form `mapToInt` used for converting a stream to an integer stream. 
- Filtering values with `filter`, 
- Identifying unique values with `distinct`, 
- Arranging values with `sorted` (if possible).

_Problem 1: You'll receive a list of persons. Print the number of persons born before the year 1970._

We'll use the `filter` method for filtering through only those persons who were born before the year 1970. We then count their number using the method `count`.

```java
// suppose we have a list of persons
// ArrayList<Person> persons = new ArrayList<>();

long count = persons.stream()
    .filter(person -> person.getBirthYear() < 1970)
    .count();
System.out.println("Count: " + count);
```

_Problem 2: You'll receive a list of persons. How many persons' first names start with the letter "A"?_

Let's use the `filter`-method to narrow down the persons to those whose first name starts with the letter "A". Afterwards, we'll calculate the number of persons with the `count`-method.

```java
// suppose we have a list of persons
// ArrayList<Person> persons = new ArrayList<>();

long count = persons.stream()
    .filter(person -> person.getFirstName().startsWith("A"))
    .count();
System.out.println("Count: " + count);
```

_Problem 3: You'll receive a list of persons. Print the number of unique first names in alphabetical order_

First we'll use the `map` method to change a stream containing person objects into a stream containing first names. After that we'll call the `distinct`-method, that returns a stream that only contains unique values. Next, we call the method `sorted`, which sorts the strings. Finally, we call the method `forEach`, that is used to print the strings.

```java
// suppose we have a list of persons
// ArrayList<Person> persons = new ArrayList<>();

persons.stream()
    .map(person -> person.getFirstName())
    .distinct()
    .sorted()
    .forEach(name -> System.out.println(name));
```

The `distinct`-method described above uses the `equals`-method that is in all objects for comparing whether two strings are the same. The `sorted`-method on the other hand is able to sort objects that contain some kind of order — examples of this kind of objects are for example numbers and strings.

# Files and Streams

The file is read in stream form using Java's ready-made [Files](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html) class. The `lines` method in the files class allows you to create an input stream from a file, allowing you to process the rows one by one. The `lines` method gets a path as its parameter, which is created using the `get` method in the [Paths](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Paths.html) class. The `get` method is provided a string describing the file path.

The example below reads all the lines in "file.txt" and adds them to the list.

```java
List<String> rows = new ArrayList<>();

try {
    Files.lines(Paths.get("file.txt")).forEach(row -> rows.add(row));
} catch (Exception e) {
    System.out.println("Error: " + e.getMessage());
}

// do something with the read lines
```

If the file is both found and read successfully, the lines of the "file.txt" file will be in the `rows` list variable at the end of the program. However, if a file cannot be found or read, an error message will be displayed. Below is one possibility:

```Output
Error: file.txt (No such file or directory)
```

# Sorting By Multiple Criteria
We sometimes want to sort items based on a number of things.
We'll make use of Java's [Comparator](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html) class here, which offers us the functionality required for sorting.

The `Comparator` class provides two essential methods for sorting: `comparing` and `thenComparing`. The `comparing` method is passed the value to be compared first, and the `thenComparing` method is the next value to be compared. The `thenComparing` method can be used many times by chaining methods, which allows virtually unlimited values ​​to be used for comparison.

When we sort objects, the `comparing` and `thenComparing` methods are given a reference to the object's type - the method is called in order and the values ​​returned by the method are compared. The method reference is given as `Class::method`.

```java
List<Film> films = new ArrayList<>();
films.add(new Film("A", 2000));
films.add(new Film("B", 1999));
films.add(new Film("C", 2001));
films.add(new Film("D", 2000));

for (Film e: films) {
    System.out.println(e);
}

Comparator<Film> comparator = Comparator
              .comparing(Film::getReleaseYear)
              .thenComparing(Film::getName);

Collections.sort(films, comparator);

for (Film e: films) {
    System.out.println(e);
}
```

```Output
A (2000) 
B (1999) 
C (2001) 
D (2000)

B (1999) 
A (2000) 
D (2000) 
C (2001)
```