# StringBuilder

Java's ready-made StringBuilder class provides a way to concatenate strings without the need to create them. A new StringBuilder object is created with a `new StringBuilder()` call, and content is added to the object using the overloaded `append` method, i.e., there are variations of it for different types of variables. Finally, the StringBuilder object provides a string using the `toString` method.

In the example below, only one string is created.

```java
StringBuilder numbers = new StringBuilder();
for (int i = 1; i < 5; i++) {
    numbers.append(i);
}
System.out.println(numbers.toString());
```

Using StringBuilder is more efficient than creating strings with the `+` operator.

# Regular Expressions
A regular expression defines a set of strings in a compact form.
Regular expressions are used, among other things, to verify the correctness of strings.

A student number should begin with "01" followed by 7 digits between 0–9.
We can then use the `matches` method of the `String` class, which checks whether the string matches the regular expression given as a parameter. For the student number, the appropriate regular expression is `"01[0-9]{7}"`, and checking the student number entered by a user is done as follows:

```java
System.out.print("Provide a student number: ");
String number = scanner.nextLine();

if (number.matches("01[0-9]{7}")) {
    System.out.println("Correct format.");
} else {
    System.out.println("Incorrect format.");
}
```
### Alternation (Vertical Line)

A vertical line indicates that parts of a regular expressions are optional. For example, `00|111|0000` defines the strings `00`, `111` and `0000`. The `respond` method returns `true` if the string matches any one of the specified group of alternatives.

```java
String string = "00";

if (string.matches("00|111|0000")) {
    System.out.println("The string contained one of the three alternatives");
} else {
    System.out.println("The string contained none of the alternatives");
}
```

```Output
The string contained one of the three alternatives
```

The regular expression `00|111|0000` demands that the string is exactly it specifies it to be - there is no _"contains"_ functionality.

```java
String string = "1111";

if (string.matches("00|111|0000")) {
    System.out.println("The string contained one of the three alternatives");
} else {
    System.out.println("The string contained none of the three alternatives");
}
```

```Output
The string contained none of the three alternatives
```
### Affecting Part of a String (Parentheses)
You can use parentheses to determine which part of a regular expression is affected by the rules inside the parentheses.

Say we want to allow the strings `00000` and `00001`. We can do that by placing a vertical bar in between them this way `00000|00001`. Parentheses allow us to limit the option to a specific part of the string. The expression `0000(0|1)` specifies the strings `00000` and `00001`.

Similarly, the regular expression `car(|s|)` defines the singular (car) and plural (cars) forms of the word car.
### Quantifiers
What is often desired is that a particular sub-string is repeated in a string. The following expressions are available in regular expressions:

- The quantifier **`*`** repeats 0 ... times, for example;
```java
String string = "trolololololo";

if (string.matches("trolo(lo)*")) {
    System.out.println("Correct form.");
} else {
    System.out.println("Incorrect form.");
}
```

```Output
Correct form.
```
---
- The quantifier **`+`** repeats 1 ... times, for example;
```java
String string = "trolololololo";

if (string.matches("tro(lo)+")) {
    System.out.println("Correct form.");
} else {
    System.out.println("Incorrect form.");
}
```

```Output
Correct form.
```

```java
String string = "nananananananana Batmaan!";

if (string.matches("(na)+ Batmaan!")) {
    System.out.println("Correct form.");
} else {
    System.out.println("Incorrect form.");
}
```

```Output
Correct form.
```
---
- The quantifier **`?`** repeats 0 or 1 times, for example:
```java
String string = "You have to accidentally the whole meme";

if (string.matches("You have to accidentally (delete )?the whole meme")) {
    System.out.println("Correct form.");
} else {
    System.out.println("Incorrect form.");
}
```

```Output
Correct form.
```

- The quantifier **`{a}`** repeats `a` times, for example:
```java
String string = "1010";

if (string.matches("(10){2}")) {
    System.out.println("Correct form.");
} else {
    System.out.println("Incorrect form.");
}
```

```Output
Correct form.
```
---
- The quantifier **`{a,b}`** repeats `a` ... `b` times, for example:
```java
String string = "1";

if (string.matches("1{2,4}")) {
    System.out.println("Correct form.");
} else {
    System.out.println("Incorrect form.");
}
```

```Output
Incorrect form.
```
---
- The quantifier **`{a,}`** repeats `a` ... times, for example:
```java
String string = "11111";

if (string.matches("1{2,}")) {
    System.out.println("Correct form.");
} else {
    System.out.println("Incorrect form.");
}
```

```Output
Correct form.
```

You can use more than one quantifier in a single regular expression. For example, the regular expression `5{3}(1|0)*5{3}` defines strings that begin and end with three fives. An unlimited number of ones and zeros are allowed in between.
### Character Classes (Square Brackets)
A character class can be used to specify a set of characters in a compact way. Characters are enclosed in square brackets, and a range is indicated with a dash. For example, `[145]` means `(1|4|5)` and `[2-36-9]` means `(2|3|6|7|8|9)`. Similarly, the entry `[a-c]*` defines a regular expression that requires the string to contain only `a`, `b` and `c`.

# Enumerated Type - Enum
If we know the possible values ​​of a variable in advance, we can use a class of type `enum`, i.e., _enumerated type_ to represent the values. Enumerated types are their own type in addition to being normal classes and interfaces. An enumerated type is defined by the keyword `enum`. For example, the following `Suit` enum class defines four constant values: `DIAMOND`, `SPADE`, `CLUB` and `HEART`.

```java
public enum Suit {
    DIAMOND, SPADE, CLUB, HEART
}
```

In its simplest form, `enum` lists the constant values ​​it declares, separated by a comma. Enum types, i.e., constants, are conventionally written with capital letters.

An Enum is (usually) written in its own file, much like a class or interface.

Each enum field gets a unique number code, and they can be compared using the equals sign. Just as other classes in Java, these values also inherit the Object class and its equals method. The equals method compares this numeric identifier in enum types too.

The numeric identifier of an enum field value can be found with `ordinal()`. The method actually returns an order number - if the enum value is presented first, its order number is 0. If its second, the order number is 1, and so on.

```java
public enum Suits {
    DIAMOND, CLUB, HEART, SPADE
}
```

```java
System.out.println(Suit.DIAMOND.ordinal());
System.out.println(Suit.HEART.ordinal());
```

```Output
0 
3
```

## Object References In Enums
Enumerated types may contain object reference variables. The values ​​of the reference variables should be set in an internal constructor of the class defining the enumerated type, i.e., within a constructor having a `private` access modifier. Enum type classes cannot have a `public` constructor.

In the following example, we have an enum `Color` that contains the constants ​​RED, GREEN and BLUE. The constants have been declared with object reference variables referring to their [color codes](https://www.w3schools.com/colors/colors_picker.asp):

```java
public enum Color {
    // constructor parameters are defined as
    // the constants are enumerated
    RED("#FF0000"),
    GREEN("#00FF00"),
    BLUE("#0000FF");

    private String code;        // object reference variable

    private Color(String code) { // constructor
        this.code = code;
    }

    public String getCode() {
        return this.code;
    }
}
```

The enum `Color` can be used like so:

```java
System.out.println(Color.GREEN.getCode());
```

```Output 
#00FF00
```

# Iterator
ArrayList and other "object containers" that implement the _Collection_ interface implement the _Iterable_ interface, and they can also be iterated over with the help of an _iterator_ - an object specifically designed to go through a particular type of object collection. The following is a version of printing the cards that uses an iterator:

```java
public void print() {
	    Iterator<Card> iterator = cards.iterator();

    while (iterator.hasNext()) {
        System.out.println(iterator.next());
    }
}
```

The iterator is requested from the `cards` list containing cards.

The iterator offers a few methods. The `hasNext()` method is used to ask if there are any objects still to be iterated over. If there are, the next object in line can be requested from the iterator using the `next()` method. This method returns the next object in line to be processed and moves the iterator.

Let's now consider a use case for an iterator. We'll first approach the issue problematically to provide motivation for the coming solution. We attempt to create a method that removes cards from a given stream with a value lower than the given value.

```java
public class Hand {
    // ...

    public void removeWorst(int value) {
        this.cards.stream().forEach(card -> {
            if (card.getValue() < value) {
                cards.remove(card);
            }
        });
    }
}
```

Executing the method results in an error.
```Output
Exception in thread "main" java.util.ConcurrentModificationException at ... Java Result: 1
```

The reason for this error lies in the fact that when a list is iterated over using the forEach method, it's assumed that the list is not modified during the traversal. Modifying the list (in this case deleting elements) causes an error - we can think of the forEach method as getting "confused" here.

If you want to remove some of the objects from the list during a traversal, you can do so using an iterator. Calling the `remove` method of the iterator object neatly removes form the list the item returned by the iterator with the previous `next` call. Here's a working example of the version of the method:

```java
public class Hand {
    // ...

    public void removeWorst(int value) {
        Iterator<Card> iterator = cards.iterator();

        while (iterator.hasNext()) {
            if (iterator.next().getValue() < value) {
                // removing from the list the element returned by the previous next-method call
                iterator.remove();
            }
        }
    }
}
```