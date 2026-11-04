## Limitations of traditional hash tables

Traditional hash tables consist of three main components:

- A **key**, or, a piece of data that is being stored for easy access.
- A **hash function**, which takes the key as input and hashes it, or maps it to another piece of data (in most cases an integer) _within a given range_.
- The output of the hash function or **hash value**.

The hash function transforms key values into an array index for _faster lookup_, which is critical - faster access is the entire reason for caching in the first place.

### Example

hash values range from 00-05. Conventional methods for distributing data among caches take the `serverKey = (hash_function(key) % N)` to determine which of the servers' keys will ultimately be mapped, as seen below.

**Note that N, the number of servers in the network, is part of the mapping. Because of this, N must stay stable or _every_ key-value pair will need to be remapped.**

We can see in the standard case where the number of servers is the expected number N, then the mapping is straightforward.

![](<../../../Attachment Folder/Pasted image 20261103171037.png>)

But what happens when one of the servers (say server 2) fails. When server 2 fails, it is likely that most of the keys have to be redistributed across the other servers since `serverKey = (hash_function(key) % N)` will change since N has changed from 3 to 2.

![](<../../../Attachment Folder/Pasted image 20261103171210.png>)

A similar thing happens when servers are added.

# Consistent hashing solution:
### In Summary:

- Consistent hashing is a methodology for storing data (key and objects) across different servers. It's advantageous over traditional hashing methods because it efficiently adapts to the adding or removal of servers to the system architecture.

### Further Reading

- This article from [Facebook's engineering blog](https://engineering.fb.com/2020/08/24/production-engineering/scaling-services-with-shard-manager/) tracks the development of Shard Manager, a generic sharding solution developed in-house to manage sharding at an incredible scale: millions of shards, and hundreds of thousands of servers are monitored by Shard Manager. Consistent hashing pops up within the first few paragraphs, but the entire article is an excellent review of many of the concepts we've covered and will cover in coming less
