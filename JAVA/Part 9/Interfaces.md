Interfaces define behavior through method names and their return values. However, they don't always include the actual implementations of the methods. A visibility attribute on interfaces is not marked explicitly as they're always `public`.

```java
public interface Readable {
    String read();
}
```

They're defined the same way that regular Java classes are, but "`public interface ...`" is used instead of "`public class ...` " at the beginning of the class.

The classes that implement the interface decide _how_ the methods defined in the interface are implemented. A class implements the interface by adding the keyword _implements_ 
after the class name followed by the name of the interface being implemented.

If `TextMessage` class implements the `Readable` interface (`public class TextMessage implements Readable`), the `TextMessage` class _must_ contain an implementation of the `public String read()` method. Implementations of methods defined in the interface must always have public as their visibility attribute.

# Interface as Variable Type

The type of a variable is always stated as its introduced. There are two kinds of type, the primitive-type variables (int, double, ...) and reference-type variables (all objects).

The text message has multiple types. Because the `TextMessage` class implements the `Readable` interface, it has a `Readable` type in addition to the `TextMessage` type.

```java
TextMessage message = new TextMessage("ope", "Something cool's about to happen);
Readable readable = new TextMessage("ope", "The text message is Readable!");
```

Note that although the `TextMessage` class that inherits the `Readable` interface class is always of the interface's type, not all classes that implement the `Readable` interface are of type `TextMessage`. You can assign an object created from the `TextMessage` class to a `Readable`-type variable, but it does not work the other way without a separate type conversion.

```java
Readable readable = new TextMessage("ope", "TextMessage is Readable!"); // works
TextMessage message = readable; // doesn't work

TextMessage castMessage = (TextMessage) readable; // works if, and only if, readable is of text message type
```

Type conversion succeeds if, and only if, the variable is of the type that it's being converted to.

# Interfaces as Method Parameters

```java
public class Printer {
    public void print(Readable readable) {
        System.out.println(readable.read());
    }
}
```

The value of the `print` method of the `printer` class lies in the fact that it can be given _any_ class that implements the `Readable` interface as a parameter.

```java
TextMessage message = new TextMessage("ope", "Oh wow, this printer knows how to print these as well!");

Printer printer = new Printer();
printer.print(message);
```

```Output
Oh wow, this printer knows how to print these as well!
```

# Built-in Interfaces
Java offers a considerable amount of built-in interfaces. 
Four commonly used interfaces: [List](http://docs.oracle.com/javase/8/docs/api/java/util/List.html), [Map](http://docs.oracle.com/javase/8/docs/api/java/util/Map.html), [Set](http://docs.oracle.com/javase/8/docs/api/java/util/Set.html), and [Collection](http://docs.oracle.com/javase/8/docs/api/java/util/Collection.html).

### The List Interface

The [List](http://docs.oracle.com/javase/8/docs/api/java/util/List.html) interface defines the basic functionality related to lists. Because the ArrayList class implements the `List` interface, one can also use it through the `List` interface.

```java
List<String> strings = new ArrayList<>();
strings.add("string objects inside an arraylist object!");
```

The internal structures of `ArrayList` and `LinkedList` differ quite a bit. ArrayList saves objects to an array where fetching an object with a specific index is very fast. On the other hand LinkedList constructs a list where each element contains a reference to the next element in the list. When one searches for an object by index in a linked list, one has to go though the list from the beginning until the index.

# The Map Interface

The [Map](http://docs.oracle.com/javase/8/docs/api/java/util/Map.html) interface defines the basic behavior associated with hash tables. Because the HashMap class implements the `Map` interface, it can also be accessed through the `Map` interface.

```java
Map<String, String> maps = new HashMap<>();
maps.put("ganbatte", "good luck");
```

# The Set Interface

The [Set](http://docs.oracle.com/javase/8/docs/api/java/util/Set.html) interface describes functionality related to sets. In Java, sets always contain either 0 or 1 amounts of any given object. As an example, the set interface is implemented by [HashSet](http://docs.oracle.com/javase/8/docs/api/java/util/HashSet.html). Here's how to go through the elements of a set.

  

```java
Set<String> set = new HashSet<>();
set.add("one");
set.add("one");
set.add("two");

for (String element: set) {
    System.out.println(element);
}
```

```Output
one 
two
```

If objects created from custom classes are added to the HashSet object, they must have both the `equals` and `hashCode` methods defined.

# The Collection Interface

The [Collection](http://docs.oracle.com/javase/8/docs/api/java/util/Collection.html) interface describes functionality related to collections. Among other things, lists and sets are categorized as collections in Java — both the List and Set interfaces implement the Collection interface. The Collection interface provides, for instance, methods for checking the existence of an item (the method `contains`) and determining the size of a collection (the method `size`).

The Collection interface also determines how the collection is iterated over. Any class that implements the Collection interface, either directly or indirectly, inherits the functionality required for a `for-each` loop.