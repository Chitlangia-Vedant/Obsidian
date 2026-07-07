## Lists
Java ArrayList uses an array. 
The type of the elements in the array is defined by the type parameter given to the ArrayList. 
### Creating a new list
The List has a generic array — the type of the elements in the array is defined on run time using type parameters. Let's set the size of the array to `10`. The array is created as type object, and changed to type generic with `(A[]) new Object[10];` — this is done because Java does not support the call `new A[10];` for now.

```java
public class List<Type> {
    private Type[] values;

    public List() {
        this.values = (Type[]) new Object[10];
    }
}
```

### Adding values to the list

```java
public class List<Type> {

    private Type[] values;
    private int firstFreeIndex;

    public List() {
        this.values = (Type[]) new Object[10];
        this.firstFreeIndex = 0;
    }

    public void add(Type value) {
        this.values[this.firstFreeIndex] = value;
        this.firstFreeIndex++; // same as this.firstFreeIndex = this.firstFreeIndex + 1;
    }
}
```

### Adding values to a list part 2
There is a small problem with the `add` method. The problem occurs when the following code is run:

```java
List<String> myList = new List<>();
for (int i = 0; i < 11; i++) {
    myList.add("hello");
}
```

```Output
Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: 10 at dataStructures.List.add(List.java:14) at dataStructures.Program.main(Program.java:8)
```

We'll create a new method `grow` for increasing the size of the array. The method is available only for other methods in the class (it is `private`).

```java
private void grow() {
    int newSize = this.values.length + this.values.length / 2;
    Type[] newValues = (Type[]) new Object[newSize];
    for (int i = 0; i < this.values.length; i++) {
        newValues[i] = this.values[i];
    }

    this.values = newValues;
}
```

Let's modify the `add` method so that the size of the array grows when needed.

```java
public void add(Type value) {
    if(this.firstFreeIndex == this.values.length) {
        grow();
    }

    this.values[this.firstFreeIndex] = value;
    this.firstFreeIndex++;
}
```

### Checking the existence of a value
We can implement the `contains` method so, that it only checks the indexes in the array which contain a value.

```java
public boolean contains(Type value) {
    for (int i = 0; i < this.firstFreeIndex; i++) {
        if (this.values[i].equals(value)) {
            return true;
        }
    }

    return false;
}
```

### Removing a value
Let's implement the method `public void remove(Type value)`, which removes _one_ value type `value`.

```java
public void remove(Type value) {
    for (int i = 0; i < this.firstFreeIndex; i++) {
        if (value == this.values[i] || this.values[i].equals(value)) {
            this.values[i] = null;
            this.firstFreeIndex--;
            return;
        }
    }
}
```

The above implementation is however problematic, because it leaves "empty" slots to the List, which would lead to the contains method not working.

```java
public void remove(T value) {
    boolean found = false;
    for (int i = 0; i < this.firstFreeIndex; i++) {
        if (found) {
            this.values[i - 1] = this.values[i];
        } else if (value == this.values[i] || this.values[i].equals(value)) {
            this.firstFreeIndex--;
            found = true;
        }
    }
}
```

We will split the functionality into two methods: `private int indexOfValue(Type value)`, which searches for the index of the value given to it as a parameter, and `private void moveToTheLeft(int fromIndex)`, which moves the elements above the given index to the left.

First let's implement the method `private int indexOfValue(Type value)`, which searches for the index of the given value. The method returns -1 if the value is not found.

```java
private int indexOfValue(Type value) {
    for (int i = 0; i < this.firstFreeIndex; i++) {
        if (this.values[i].equals(value)) {
            return i;
        }
    }

    return -1;
}
```

Then we will implement the method `private void moveToTheLeft(int fromIndex)`, which moves values from the given index one place to the left.

```java
private void moveToTheLeft(int fromIndex) {
    for (int i = fromIndex; i < this.firstFreeIndex - 1; i++) {
        this.values[i] = this.values[i + 1];
    }
}
```

Now we can implement the method `remove` using these two methods.

```java
public void remove(Type value) {
    int indexOfValue = indexOfValue(value);
    if (indexOfValue < 0) {
        return; // not found
    }

    moveToTheLeft(indexOfValue);
    this.firstFreeIndex--;
}
```

The class List now contains some repeated code. The method `contains` is very similar to the method `indexOfValue`. Let's modify the method `contains` so that it uses the method `indexOfValue`.

```java
public boolean contains(Type value) {
    return indexOfValue(value) >= 0;
}
```

### Searching from an index
Method `public Type value(int index)`, which returns the value in the given index of the List. 
If the user searches for a value in an index outside of the Array, `IndexOutOfBoundsException` is thrown.

```java
public Type value(int index) {
    if (index < 0 || index >= this.firstFreeIndex) {
        throw new ArrayIndexOutOfBoundsException("Index " + index + " outside of [0, " + this.firstFreeIndex + "]");
    }

    return this.values[index];
}
```

Modify the method `indexOfValue(Type value)` so it can be used by everyone, so it is `public` instead of `private`.

```java
public int indexOfValue(Type value) {
    for (int i = 0; i < this.firstFreeIndex; i++) {
        if (this.values[i].equals(value)) {
            return i;
        }
    }

    return -1;
}
```

### Size of the List
Lastly we will add a method for checking the size of the List. The size of the list can be determined by the variable `firstFreeIndex`.

```java
public int size() {
    return this.firstFreeIndex;
}
```

## Hash map
Hash map is implemented as an array, in which every element includes a list. The lists contain (key, value) pairs. The user can search from the hash map based on the key, and they can also add new key-value pairs into it. Each key can appear at most once in the hash map.

The functioning of the hash map is based on the hash value of the key. When a new (key, value) pair is stored in a hash map, we calculate a hash value based on the key to be stored. The hash value decides the index of the internal array that will be used for storing. The (key, value) pair is stored in the list that can be found at that index.
### Key-value pair
Let's start by creating the class `Pair` that represents a key-value pair. 

```java
public class Pair<K, V> {

    private K key;
    private V value;

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public K getKey() {
        return key;
    }

    public V getValue() {
        return value;
    }

    public void setValue(V value) {
        this.value = value;
    }
}
```

### Creating a hash map
A hash map contains an array of lists. Each value on the list is a pair (described in the previous section) that contains a key and a value. A hash map also knows the number of the values. Here we have at our disposal the previously created class `List`.

```java
public class HashMap<K, V> {

    private List<Pair<K, V>>[] values;
    private int firstFreeIndex;

    public HashMap() {
        this.values = new List[32];
        this.firstFreeIndex = 0;
    }
}
```

### Retrieving a value
Let's implement a method called `public V get(K key)`. It can be used to search for a value based on a key.

The method begins by calculating a hash value for the key, and using it to figure out which is the relevant index of the internal array of the hash map. The hash value is calculated with the `hashCode` method that each object has.
Then modulo (remainder of division operation) is used for ensuring that the index stays within the size boundaries of the internal array.

If there is no list in the calculated index, no key-value pairs have been added to that index. This means that there are no key-value pairs with this key that have been stored. In this case we'll return the `null` reference. Otherwise, the program goes through the list at the index, and we compare the parameter key to the key of every key-value pair on that list. If some of the keys matches the parameter key, the method returns the value of that key-value pair.

```java
public V get(K key) {
    int hashValue = Math.abs(key.hashCode() % this.values.length);
    if (this.values[hashValue] == null) {
        return null;
    }

    List<Pair<K, V>> valuesAtIndex = this.values[hashValue];

    for (int i = 0; i < valuesAtIndex.size(); i++) {
        if (valuesAtIndex.value(i).getKey().equals(key)) {
            return valuesAtIndex.value(i).getValue();
        }
    }

    return null;
}
```

### Adding to hash map

The method first calculates the hash value for the key, and uses it to determine the suitable index in the internal array. If there is no value in that index, we create a list into that index. After this the method goes through the list at the index, and looks for a key-value pair whose key matches the key of the key-value pair to be added. If the matching key is found, the value related to it is updated to match the new value. Otherwise the method adds a new key-value pair in the list — in which case the number of stored values is also incremented by one.

```java
public void add(K key, V value) {
    int hashValue = Math.abs(key.hashCode() % values.length);
    if (values[hashValue] == null) {
        values[hashValue] = new List<>();
    }

    List<Pair<K, V>> valuesAtIndex = values[hashValue];

    int index = -1;
    for (int i = 0; i < valuesAtIndex.size(); i++) {
        if (valuesAtIndex.value(i).getKey().equals(key)) {
            index = i;
            break;
        }
    }

    if (index < 0) {
        valuesAtIndex.add(new Pair<>(key, value));
        this.firstFreeIndex++;
    } else {
        valuesAtIndex.value(index).setValue(value);
    }
```

The method is quite complex, so let's divide it into smaller parts. The first part is responsible for finding the list related to the key, and the second part is responsible for finding the key on that list.

```java
private List<Pair<K, V>> getListBasedOnKey(K key) {
    int hashValue = Math.abs(key.hashCode() % values.length);
    if (values[hashValue] == null) {
        values[hashValue] = new List<>();
    }

    return values[hashValue];
}

private int getIndexOfKey(List<Pair<K, V>> myList, K key) {
    for (int i = 0; i < myList.size(); i++) {
        if (myList.value(i).getKey().equals(key)) {
            return i;
        }
    }

    return -1;
}
```

Now we can write a somewhat clearer implementation of the method `public void add(K key, V value)`

```java
public void add(K key, V value) {
    List<Pair<K, V>> valuesAtIndex = getListBasedOnKey(key);
    int index = getIndexOfKey(valuesAtIndex, key);

    if (index < 0) {
        valuesAtIndex.add(new Pair<>(key, value));
        this.firstFreeIndex++;
    } else {
        valuesAtIndex.value(index).setValue(value);
    }
}
```

### Adding to hash table, part 2
The greatest fault in the functionality is that the size of the internal array is not increased when the number of values grows too large.

The responsible method should create a new array whose size is double that of the old array. After this it goes through the old array, index by index. The encountered key-value pairs are copied into the new array. Finally, the old array is replaced with the new one.

When copying, the location of each key-value pair is recalculated for the new array — this is done because the size of the internal array grows, and we want to distribute all the key-value pairs in that array as evenly as possible.

```java
private void copy(List<Pair<K, V>>[] newArray, int fromIdx) {
    for (int i = 0; i < this.values[fromIdx].size(); i++) {
        Pair<K, V> value = this.values[fromIdx].value(i);

        int hashValue = Math.abs(value.getKey().hashCode() % newArray.length);
        if(newArray[hashValue] == null) {
            newArray[hashValue] = new List<>();
        }

        newArray[hashValue].add(value);
    }
}
```

Now you can call the copy method from the grow method

```java
private void grow() {
    // create a new array
    List<Pair<K, V>>[] newArray = new List[this.values.length * 2];

    for (int i = 0; i < this.values.length; i++) {
        // copy the values of the old array into the new one
        copy(newArray, i);
    }

    // replace the old array with the new
    this.values = newArray;
}
```

Add the growing functionality to be a part of the `add` method. We want to grow the size of the hash map if the number of key-value pairs in it is greater than 75% of the size of the internal array.

```java
public void add(K key, V value) {
    List<Pair<K, V>> valuesAtIndex = getListBasedOnKey(key);
    int index = getIndexOfKey(valuesAtIndex, key);

    if (index < 0) {
        valuesAtIndex.add(new Pair<>(key, value));
        this.firstFreeIndex++;
    } else {
        valuesAtIndex.value(index).setValue(value);
    }

    if (1.0 * this.firstFreeIndex / this.values.length > 0.75) {
        grow();
    }
}
```

### Remove

The removal functionality returns null if the value cannot be found, and otherwise it will remove the value that is paired with the key to be removed.

```java
public V remove(K key) {
    List<Pair<K, V>> valuesAtIndex = getListBasedOnKey(key);
    if (valuesAtIndex.size() == 0) {
        return null;
    }

    int index = getIndexOfKey(valuesAtIndex, key);
    if (index < 0) {
        return null;
    }

    Pair<K, V> pair = valuesAtIndex.value(index);
    valuesAtIndex.remove(pair);
    return pair.getValue();
}
```