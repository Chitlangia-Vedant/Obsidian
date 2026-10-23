Packages are practically directories in which the source code files are organised.

The package of a class (the package in which the class is stored) is noted at the beginning of the source code file with the statement `package *name-of-package*;`. Below, the class Program is in the package `library`.

```java
package library;

public class Program {

    public static void main(String[] args) {
        System.out.println("Hello packageworld!");
    }
}
```

Every package, including the default package, may contain other packages. For instance, in the package definition `package library.domain` the package `domain` is inside the package `library`. The word `domain` is often used to refer to the storage space of the classes that represent the concepts of the problem domain. For example, the class `Book` could be inside the package `library.domain`.

A class can access classes inside a package by using the `import` statement. The class `Book` in the package `library.domain` is made available for use with the statement `import library.domain.Book`.

# Packages and access modifiers
 The modifier `private` is used to define variables (and methods) that are only visible inside the class where they are defined. They cannot be used from outside that class. 
 The methods and variables defined with `public`, on the other hand, are available for everyone to use.
 If the access modifier is missing, the methods and variables are only visible inside the same package. We call this the default or package modifier.