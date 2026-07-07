### Array vs. Hash table

We can implement the same functionality using a hash table. Wouldn't a hash table be a better option, since we do not have to worry about increasing its size at any point?

When we search for a value of a key from a hash table, we use the hashCode method to find the index to search from. There can be multiple values at the same index (on a list). Then we have to compare the key we want to find the value for to the key of each key-value pair on the list using the equals method.

When we search for a value of a key — or index — in an array, we do not have to do any of that.

An array either contains a certain value or it does not, so there is a small performance benefit on using arrays.

This performance benefit comes with some added workload and proneness to errors. Hash tables have tested and proven functionality for increasing the size of the table. Arrays do not come with this benefit, and when implementing a new functionality we might cause errors which increases the workload. However, errors are accepted and natural part of software development.

When we consider the memory usage, hash tables might — in some situations — have some benefits. When an array is created, enough memory for the whole array is allocated for it. If we do not have values in each element of the array, some of the memory stays unused. With hash tables this does not happen — the size of the hash table is increased only when necessary.