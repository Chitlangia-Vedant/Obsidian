The [Comparable](http://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html) interface defines the `compareTo` method used to compare objects. If a class implements the Comparable interface, objects created from that class can be sorted using Java's sorting algorithms.

The `compareTo` method required by the Comparable interface receives as its parameter the object to which the "this" object is compared. If the "this" object comes before the object received as a parameter in terms of sorting order, the method should return a negative number. If, on the other hand, the "this" object comes after the object received as a parameter, the method should return a positive number. Otherwise, 0 is returned. The sorting resulting from the `compareTo` method is called _natural ordering_.

In fact, objects of any class that implement the `Comparable` interface can be sorted using the `sorted` method.

# Implementing Multiple Interfaces

A class can implement multiple interfaces. Multiple interfaces are implemented by separating the implemented interfaces with commas (`public class ... implements *FirstInterface*, *SecondInterface* ...`). Implementing multiple interfaces requires us to implement all of the methods for which implementations are required by the interfaces.

# Sorting Method as a Lambda Expression
Both the `sort` method of the `Collections` class and the stream's `sorted` method accept a lambda expression as a parameter that defines the sorting criteria. More specifically, both methods can be provided with an object that implements the [Comparator](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html) interface, which defines the desired order - the lambda expression is used to create this object.

```java
ArrayList<Person> persons = new ArrayList<>();
persons.add(new Person("Ada Lovelace", 1815));
persons.add(new Person("Irma Wyman", 1928));
persons.add(new Person("Grace Hopper", 1906));
persons.add(new Person("Mary Coombs", 1929));

persons.stream().sorted((p1, p2) -> {
    return p1.getBirthYear() - p2.getBirthYear();
}).forEach(p -> System.out.println(p.getName()));

System.out.println();

persons.stream().forEach(p -> System.out.println(p.getName()));

System.out.println();

Collections.sort(persons, (p1, p2) -> p1.getBirthYear() - p2.getBirthYear());

persons.stream().forEach(p -> System.out.println(p.getName()));
```

```Output
Ada Lovelace 
Grace Hopper 
Irma Wyman 
Mary Coombs

Ada Lovelace 
Irma Wyman 
Grace Hopper 
Mary Coombs

Ada Lovelace 
Grace Hopper 
Irma Wyman 
Mary Coombs
```