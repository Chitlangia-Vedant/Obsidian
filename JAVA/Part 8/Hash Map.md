The hash map is used whenever data is stored as key-value pairs, where values can be added, retrieved, and deleted using keys.

The internal state of the hash map created above looks like this. Each key refers to some value.

![A value in a hashmap is looked up based on a key.](https://java-programming.mooc.fi/static/98ddf66b2a09fa1450ba17e15e9044d8/fc2a6/part8.2-hashmap.png "A value in a hashmap is looked up based on a key.")
If the hash map does not contained the key used for the search, its `get` method returns a `null` reference.

```java
import java.util.HashMap;

HashMap<String, Integer> hashmap = new HashMap<>();

hashmap.put(*key*, *value*);
hashmap.get(*key*);
```

## Hash Map Keys Correspond to a Single Value at Most

The hash map has a maximum of one value per key. If a new key-value pair is added to the hash map, but the key has already been associated with some other value stored in the hash map, the old value will vanish from the hash map.

```java
HashMap<String, String> numbers = new HashMap<>();
numbers.put("Uno", "One");
System.out.println(numbers.get("Uno"));
numbers.put("Uno", "Ein");
System.out.println(numbers.get("Uno"));
```

```Output
One
Ein
```

## When Should Hash Maps Be Used?

>The hash map is implemented internally in such a way that searching by a key is very fast. The hash map generates a "hash value" from the key, i.e. a piece of code, which is used to store the value of a specific location. When a key is used to retrieve information from a hash map, this particular code identifies the location where the value associated with the key is. In practice, it's not necessary to go through all the key-value pairs in the hash map when searching for a key; the set that's checked is significantly smaller

## Going Through A Hash Map

### Keys

```java
HashMap<String, Integer> hashmap = new HashMap<>();

// hashmap.keySet() return set of keys
for(String s:hashmap.keySet()){
}
```

### Values

```java
HashMap<String, Integer> hashmap = new HashMap<>();

// hashmap.values() return list of values
for(String s:hashmap.values()){
}
```

## Primitive Variables In Hash Maps

A hash map expects that only reference-variables are added to it (in the same way that `ArrayList` does). 
Java converts primitive variables to their corresponding reference-types when using any Java's built in data structures (such as ArrayList and HashMap). 
	Although the value `1` can be represented as a value of the primitive `int` variable, its type should be defined as `Integer` when using ArrayLists and HashMaps.

```java
HashMap<Integer, String> hashmap = new HashMap<>(); // works
hashmap.put(1, "Ole!");
HashMap<int, String> map2 = new HashMap<>(); // doesn't work
```

**A hash map's key and the object to be stored are always reference-type variables.**

Java converts primitive variables to reference-types automatically as they are added to either a HashMap or an ArrayList. This automatic conversion to a reference-type variable is termed _auto-boxing_ in Java, i.e. putting something in a box automatically. The automatic conversion is also possible in the other direction.

There is, however, some risk in type conversions. If we attempt to convert a `null` reference - a value not in HashMap, for instance - to an integer, we witness a **_java.lang.reflect.InvocationTargetException_** error.

```java
	hashmap.get(5);
```

```Output
java.lang.reflect.InvocationTargetException
```

### Solution

The `getOrDefault` method of the HashMap searches for the key passed to it as a parameter from the HashMap. If the key is not found, it returns the value of the second parameter passed to it.

```java
	hashmap.getOrDefault(5,0);
```

```Output
0
```

