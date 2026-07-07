_Generics_ relates to how classes that store objects can store objects of a freely chosen type. The choice is based on the generic type parameter in the definition of the classes, which makes it possible to choose the type(s) _at the moment of the object's creation_. Using generics is done in the following manner: after the name of the class, follow it with a chosen number of type parameters. Each of them is placed between the 'smaller than' and 'greater than' signs, like this: `public class Class<TypeParameter1, TypeParameter2, ...>`. The type parameters are usually defined with a single character.

Let's implement our own generic class `Locker` that can hold one object of any type.

```java
public class Locker<T> {
    private T element;

    public void setValue(T element) {
        this.element = element;
    }

    public T getValue() {
        return element;
    }
}
```

The definition `public class Locker<T>` indicates that the `Locker` class must be given a type parameter in its constructor.

Creating generic interfaces is very similar to creating generic classes. Below you can study the generic interface `List` (our own definition, which is not as extensive as the existing Java [List](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)).

```java
public interface List<T> {
    void add(T value);
    T get(int index);
    T remove(int index);
}
```

There are two ways for a class to implement a generic interface. 
- One is to decide the type parameter in the definition of the class
```java
public class MovieList implements List<Movie>
```
- The other is to define the implementing class with a type parameter as well.
```java
public class GeneralList<T> implements List<T>
```