Every class derives from `Object`, but it's also possible to derive from other classes. When we examine the API (Application Programming Interface) of Java's [ArrayList](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html), we notice that `ArrayList` has the superclass `AbstractList`. `AbstractList`, in turn, has the class `Object` as its superclass.

[java.lang.Object](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html)
	[java.util.AbstractCollection](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractCollection.html)
		[java.util.AbstractList](https://docs.oracle.com/javase/8/docs/api/java/AbstractList.html)
			java.util.ArrayList

Each class can directly extend only one class. However, a class indirectly inherits all the properties of the classes it extends. So the `ArrayList` class derives from the class `AbstractList`, and indirectly derives from the classes `AbstractCollection` and `Object`. So `ArrayList` has at its disposal all the variables and methods of the classes `AbstractList`, `AbstractCollection`, and `Object`.

# Super Keyword

_The `super` call bears some resemblance to the `this` call in a constructor; `this` is used to call a constructor of this class, while `super` is used to call a constructor of the superclass. If a constructor uses the constructor of the superclass by calling `super` in it, the `super` call must be on the first line of the constructor. This is similar to the case with calling `this` (must also be the first line of the constructor)._
## Calling a superclass constructor

You use the keyword `super` to call the constructor of the superclass. The call receives as parameters the types of values that the superclass constructor requires. If there are multiple constructors in the superclass, the parameters of the super call dictate which of them is used.

## Calling a superclass method

You can call the methods defined in the superclass by prefixing the call with `super`, just as you can call the methods defined in this class by prefixing the call with `this`

# The actual type of an object dictates which method is executed

An object's type decides what the methods provided by the object are. For instance, we implemented the class `Student` earlier. If a reference to a `Student` type object is stored in a `Person` type variable, only the methods defined in the `Person` class (and its superclass and interfaces) are available:

```java
Person ollie = new Student("Ollie", "6381 Hollywood Blvd. Los Angeles 90028");
ollie.credits();        // DOESN'T WORK!
ollie.study();              // DOESN'T WORK!
System.out.println(ollie);   // ollie.toString() WORKS
```

In the last exercise we wrote a new `toString` implementation for Student to override the method that it inherits from Person. The class Person had already overriden the toString method it inherited from the class Object. If we handle an object by some other type than its actual type, which version of the object's method is called?

```java
Student ollie = new Student("Ollie", "6381 Hollywood Blvd. Los Angeles 90028");
System.out.println(ollie);
Person olliePerson = new Student("Ollie", "6381 Hollywood Blvd. Los Angeles 90028");
System.out.println(olliePerson);
Object ollieObject = new Student("Ollie", "6381 Hollywood Blvd. Los Angeles 90028");
System.out.println(ollieObject);
```

```Output
Ollie 
  6381 Hollywood Blvd. Los Angeles 90028 
  credits 0 
Ollie 
  6381 Hollywood Blvd. Los Angeles 90028 
  credits 0 
Ollie 
  6381 Hollywood Blvd. Los Angeles 90028 
  credits 0
```

The method to be executed is chosen based on the actual type of the object, which means the class whose constructor is called when the object is created. If the method has no definition in that class, the version of the method is chosen from the class that is closest to the actual type in the inheritance hierarchy.

# Polymorphism

Regardless of the type of the variable, the method that is executed is always chosen based on the actual type of the object. Objects are polymorphic, which means that they can be used via many different variable types. The executed method always relates to the actual type of the object. This phenomenon is called polymorphism.

# Abstract classes

An abstract class combines interfaces and inheritance. You cannot create instances of them — you can only create instances of subclasses of an abstract class. They can include normal methods which have a method body, but it's also possible to define abstract methods that only contain the method definition. Implementing the abstract methods is the responsibility of subclasses.

To define an abstract class or an abstract method the keyword `abstract` is used. An abstract class is defined with the phrase `public abstract class *NameOfClass*`; an abstract method is defined by `public abstract returnType nameOfMethod`.

```java
public abstract class Operation {

    private String name;

    public Operation(String name) {
        this.name = name;
    }

    public String getName() {
        return this.name;
    }

    public abstract void execute(Scanner scanner);
}
```

The greatest difference between interfaces and abstract classes is that abstract classes can contain object variables and constructors in addition to methods. Since you can also define functionality in abstract classes, you can use them to define e.g. default behavior.