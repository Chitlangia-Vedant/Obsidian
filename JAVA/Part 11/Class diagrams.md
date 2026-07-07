- A class diagram is a diagram used in designing and modeling software to describe classes and their relationships.
- Class diagrams enable us to model software in a high level of abstraction and without having to look at the source code.
- Classes in a class diagram correspond with classes in the source code.
- The diagram shows the names and attributes of the classes, connections between the classes, and sometimes also the methods of the classes.

# Describing class and class attributes
First we will describe one class and its attributes.

```java
public class Person {
    private String name;
    private int age;
}
```

In a class diagram, a class is represented by a rectangle with the name of the class written on top. A line below the name of the class divides the name from the list of attributes (names and types of the class variables). The attributes are written one attribute per line.

In a class diagram, class attributes are written "attributeName: attributeType". 
A 
- \+ before the attribute name means the attribute is public, and a 
- \- means the attribute is private.
![[Pasted image 20260221173328.png]]
# Describing class constructor
Below we have the source code for a constructor for our Person class. The constructor gets the name of the person as a parameter.

```java
public class Person {
    private String name;
    private int age;

    public Person(String initialName) {
        this.name = initialName;
        this.age = 0;
    }
}
```

In a class diagram, we list the constructor (and all other methods) below the attributes. A line below the attributes list separates it from the method list. 
Methods are written with +/- (depending on the visibility of the method), method name, parameters, and their types. The constructor above is written `+Person(initialName:String)`

The parameters are written the same way class attributes are — "parameterName: parameterType".

![[Pasted image 20260221173629.png]]

# Describing class methods

Below we have added a method printPerson() which returns void to the Person class.

```java
public class Person {
    private String name;
    private int age;

    public Person(String initialName) {
        this.name = initialName;
        this.age = 0;
    }

    public void printPerson() {
        System.out.println(this.name + ", age " +   this.age + " years");
    }

    public String getName() {
        return this.name;
    }
}
```

In a class diagram, we list all class methods including the constructors; constructors are listed first and then all class methods. We also write the return type of a method in the class diagram.

![[Pasted image 20260221174312.png]]
# A class diagram describes classes, constructors and methods

A class diagram describes classes and their attributes, constructors and methods as well as the connections between classes. However a class diagram tells us nothing about the implementation of the constructors or the methods. Therefore a class diagram describes the structure of an object but not its functionality.

For example the method `printPerson` uses the class attributes `name` and `age`, but this cannot be seen from the class diagram.

# Connections between classes

In a class diagram, the connections between classes are shown as arrows. The arrows also show the direction of the connection.

Below we have a class Book.
```java
public class Book {
    private String name;
    private String publisher;
    private Person author;

    // constructors and methods
}
```

In a class diagram variables which refer to other objects are not written with the rest of the class attributes, but are shown as connections between the classes. In the class diagram below we have the classes Person and Book, and the connection between them.
![[Pasted image 20260221174713.png]]
The arrow shows the direction of the connection. The connection above shows that a Book knows its author but a Person does not know about books they are the author of. We can also add a label to the arrow to describe the connection. In the above diagram the arrow has an accompanying label telling us that a Book has an author.

If a book has multiple authors, the authors are saved to a list.

```java
public class Book {
    private String name;
    private String publisher;
    private ArrayList<Person> authors;

    // constructor

    public ArrayList<Person> getAuthors() {
        return this.authors;
    }

    public void addAuthor(Person author) {
        this.authors.add(author);
    }
}
```

In a class diagram, this situation is described by adding a star to the end of the arrow showing the connection between the classes. The star tells us that a book can have between 0 and unlimited number of authors.
![[Pasted image 20260221174916.png]]

If there is no arrowhead in a connection, both classes know about each other. Below is an example where a book knows about its author and a person knows about a book they have written.
![[Pasted image 20260221174953.png]]

```java
import java.util.ArrayList;

public class Person {
    private String name;
    private int age;
    private ArrayList<Book> books;

    // ...
}
```

```java
public class Book {
    private String name;
    private String publisher;
    private ArrayList<Person> authors;

    // ..
}
```

# Describing inheritance
In a class diagram inheritance is described by an arrow with a triangle head. The triangle points to the class being inherited from. In the below example the Engine inherits the class Part.

![[Pasted image 20260221181614.png]]

Inheritance of abstract classes is described almost the same way as regular classes. However we add the description `<<abstract>>` above the name of the class. The name of the class and its abstract methods are also written in cursive.

![[Pasted image 20260221181707.png]]

# Describing interfaces
In class diagrams, interfaces are written `<<interface>>` NameOfTheInterface. Below we describe an interface Readable.

```java
public interface Readable {

}
```

![[Pasted image 20260221181844.png]]

Methods are described just like they are for a class.

Implementing an interface is shown as a dashed arrow with a triangle arrowhead. Below, we describe a situation where Book implements interface Readable.

![[Pasted image 20260221181852.png]]
