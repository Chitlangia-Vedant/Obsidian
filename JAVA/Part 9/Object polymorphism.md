_All_ objects are of type `Object`, i.e., any given object can be represented as a `Object`-type variable in addition to its own type.

```java
String text = "text";
Object textString = "another string";
```

```java
String text = "text";
Object textString = text;
```

In the examples above, a string variable is represented as both a String type and an Object type. Also, a String-type variable is assigned to an Object-type variable. However, assignment in the other direction, i.e., setting an Object-type variable to a String type, will not work. This is because `Object`-type variables are not of type `String`

```java
Object textString = "another string";
String text = textString; // WON'T WORK!
```

## What is this all about?

In addition to each variable's original type, each variable can also be represented by the types of interfaces it implements and classes that it inherits. The String class inherits the Object class and, as such, String objects are always of type Object. The Object class does not inherit a String class, so Object-type variables are not automatically of type String. Take a closer look at the [String](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html) API documentation, in particular at the top of the HTML page.

![A screenshot of the String Class API documentation. The screenshot shows that the String class inherits the class Object.](https://java-programming.mooc.fi/static/3eb128a74f0e6fed7bfec6d6f9f12776/13a9a/string-api-perinta.png "A screenshot of the String Class API documentation. The screenshot shows that the String class inherits the class Object.")

The API documentation for the String class begins with a generic header followed by the class' package (`java.lang`). After the package details, the name of the class (`Class String`) is followed by the _inheritance hierarchy_ of the class.

[java.lang.Object](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html)
	java.lang.String

The inheritance hierarchy in the description is followed by a list of interfaces implemented by the class.

  All Implemented Interfaces:
  [Serializable](https://docs.oracle.com/javase/8/docs/api/java/io/Serializable.html), [CharSequence](https://docs.oracle.com/javase/8/docs/api/java/lang/CharSequence.html), [Comparable](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html)<[String](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html)>

The `String` class implements the `Serializable`, `CharSequence`, and `Comparable <String>` interfaces. An interface is also a type. According to the class' API description, the following interfaces can be set as the type of a String object.

```java
Serializable serializableString = "string";
CharSequence charSequenceString = "string";
Comparable<String> comparableString = "string";
```