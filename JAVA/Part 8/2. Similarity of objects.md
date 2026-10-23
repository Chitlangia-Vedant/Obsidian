# Method to Test For Equality - "equals"

The [equals method](http://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#equals-java.lang.Object) checks by default whether the object given as a parameter has the same reference as the object it is being compared to. In other words, the default behaviour checks whether the two objects are the same. If the reference is the same, the method returns `true`, and `false` otherwise.

```java
Book bookObject = new Book("Book object", 2000, "...");

Book anotherBookObject = bookObject;
System.out.println(bookObject.equals(anotherBookObject));

anotherBookObject = new Book("Book object", 2000, "...");
System.out.println(bookObject.equals(anotherBookObject));
```

```Output
true
false
```

 This is because the references are the same in the first case, i.e., the object is compared to itself. The second comparison is about two different entities, even though the variables have the same values.

For strings, `equals` works as expected in that it declares two strings _identical in content_ to be 'equal' even if they are two separate objects. The String class has replaced the default `equals` with its own implementation.

If we want to compare our own classes using the `equals` method, then it must be defined inside the class. The method created accepts an `Object`-type reference as a parameter, which can be any object. The comparison first looks at the references. This is followed by checking the parameter object's type with the `instanceof` operation - if the object type does not match the type of our class, the object cannot be the same. We then create a version of the object that is of the same type as our class, after which the object variables are compared against each other.

```java
public boolean equals(Object comparedObject) {
    // if the variables are located in the same place, they're the same
    if (this == comparedObject) {
        return true;
    }

   // if comparedObject is not of type Book, the objects aren't the same
   if (!(comparedObject instanceof Book)) {
        return false;
    }

    // let's convert the object to a Book-olioksi
    Book comparedBook = (Book) comparedObject;

    // if the instance variables of the objects are the same, so are the objects
    if (this.name.equals(comparedBook.name) &&
        this.published == comparedBook.published &&
        this.content.equals(comparedBook.content)) {
        return true;
    }

    // otherwise, the objects aren't the same
    return false;
}
```

# Approximate Comparison With HashMap

The `hashCode` method can also be used for **approximate comparison of objects**. The method creates from the object a "hash code", i.e, a number, that tells a bit about the object's content. If two objects have the same hash value, they may be equal. On the other hand, if two objects have different hash values, they are certainly unequal.

```java
HashMap<Book, String> borrowers = new HashMap<>();

Book bookObject = new Book("Book Object", 2000, "...");
borrowers.put(bookObject, "Pekka");
borrowers.put(new Book("Test Driven Development", 1999, "..."), "Arto");

System.out.println(borrowers.get(bookObject));
System.out.println(borrowers.get(new Book("Book Object", 2000, "...")));
System.out.println(borrowers.get(new Book("Test Driven Development", 1999, "...")));

```

```Output
Pekka 
null 
null
```

When searching by the exact same book but with a different object, a borrower isn't found, and we get the _null_ reference instead. The reason lies in the default implementation of the `hashCode` method in the `Object` class. The default implementation creates a `hashCode` value based on the object's reference, which means that books having the same content that are nonetheless different objects get different results from the hashCode method.

For the HashMap to work in the way we want it to, that is, to return the borrower when given an object with the correct _content_ (not necessarily the same object as the original key), the class that the key belongs to must overwrite the `hashCode` method in addition to the `equals` method. The method must be overwritten so that it gives the same numerical result for all objects with the same content. Also, some objects with different contents may get the same result from the hashCode method. However, with the HashMap's performance in mind, it is essential that objects with different contents get the same hash value as rarely as possible.

We've previously used `String` objects as HashMap keys, so we can deduce that the `String` class has a well-functioning `hashCode` implementation of its own. We'll _delegate_, i.e., transfer the computational responsibility to the `String` object.

```java
public int hashCode() {
    return this.name.hashCode();
}
```

If `name` is _null_, we see a `NullPointerException` error. Let's fix this by defining a condition: if the value of the `name` variable is _null_, we'll return the year of publication as the hash value.

Improve it further so that the year of publication is also taken into account in the hash value calculation that's based on the book title.

```java
public int hashCode() {
    if (this.name == null) {
        return this.published;
    }

    return this.published + this.name.hashCode();
}
```