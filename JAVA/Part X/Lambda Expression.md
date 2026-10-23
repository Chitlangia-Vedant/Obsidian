A Lambda Expression is basically a short way to write a function — without creating a separate class or method.

**Benefits**
- Reduce the line of Codes
- Sequential and Parallel execution support by passing behavior as an argument in methods
- To call APIs very effectively
- To write more readable, maintainable and concise code

**Without Lambda**
```java
Collections.sort(names, new Comparator<String>() {
    public int compare(String a, String b) {
        return a.compareTo(b);
    }
});
```

**With Lambda**
```java
names.sort((a, b) -> a.compareTo(b));
```

# Lambdas with forEach
## Lambda
```java
List<String> list = Arrays.asList("A", "B", "C");

// Lambda
list.forEach(item -> System.out.println(item));
```

```Output
A
B
C
```
## Method Reference
```java
List<String> list = Arrays.asList("A", "B", "C");

// Method reference
list.forEach(System.out::println);
```

```Output
A
B
C
```

The `::` operator is called the method reference operator.
# Lambdas with [[../Part 10/1. Handling collections as streams.md#|Streams]]

```java
List<Integer> nums = Arrays.asList(2,5,7,8,10);

nums.stream()
    .filter(n -> n % 2 == 0)  // keep even numbers
    .map(n -> n * n)          // square them
    .forEach(System.out::println);

```