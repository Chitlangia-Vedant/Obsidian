# HashSet vs LinkedHashSet vs TreeSet

- arr = [9 1 6 8 4 7 3 1 2 5 4]
- HashSet = [9 1 6 8 4 7 3 2 5] OR [5 2 1 3 4 7 8 6 9]
- LinkedHashSet = [9 1 6 8 4 7 3 2 5]
- TreeSet = [1 2 3 4 5 6 7 8 9]

- HashSet = Order NOT Guaranteed, Can Store in Any Order
- LinkedHashSet = Order is Guaranteed, Same Order as In Original Array
- TreeSet = Store in Ascending Order

## C++:
unordered_set: HashSet: (Without Any Order)
ordered_set: Default Set: TreeSet (Sorted Order)

## Py:
dict = Sorted
set = Sorted


## Map:
{"a": 1, "b":2, "c":3}

Add: 
{"a":5}

OP:
{"a": 5, "b":2, "c":3}
## Set: 
[1,2,3]

add: 1

OP: [1,2,3]
	|	
	original not the new 


# CODE:

```
public class Main 
{
    public static void main(String[] args) 
    {
        // HashMap
        HashMap<String, Integer> hashMap = new HashMap<>();
        hashMap.put("a", 1);
        hashMap.put("b", 2);
        hashMap.put("c", 3);
        System.out.println("HashMap Before: " + hashMap);
        hashMap.put("a", 5);
        System.out.println("HashMap After: " + hashMap);
        
        // HashSet
        HashSet<Integer> hashSet = new HashSet<>();
        hashSet.add(1);
        hashSet.add(2);
        hashSet.add(3);
        System.out.println("HashSet Before: " + hashSet);
        hashSet.add(1);
        System.out.println("HashSet After: " + hashSet);        
    }
}
```

```Output
HashMap Before: {a=1, b=2, c=3}
HashMap After: {a=5, b=2, c=3}
HashSet Before: [1, 2, 3]
HashSet After: [1, 2, 3]
```

# TC:

## Map:

Insert: O(1)
Lookup: O(1) --- Best Case and Avg case

Worst Case, Lookup: O(log N) - BST

## Set:

Insert: O(1)
Lookup: O(1) --- Best Case and Avg case

Worst Case, Lookup: O(log N)- BST

# SC: 
## O(N) for Both Map and Set
