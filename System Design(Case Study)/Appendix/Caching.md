A _cache_ is any data store that can store and retrieve data quickly for future use, enabling faster response times and decreasing load on other parts of your system
# How caching works

Caching can be implemented in many different ways in modern systems:

**In-memory application cache.** Storing data directly in the application's memory is a fast and simple option, but each server must maintain its own cache, which increases overall memory demands and cost of the system.

**Distributed in-memory cache.** A separate caching server such as Memcached or Redis can be used to store data so that multiple servers can read and write from the same cache.

![](<../../Attachment Folder/Pasted image 20261103161354.png>)

**Database cache.** A traditional database can still be used to cache commonly requested data or store the results of pre-computed operations for retrieval later.

**File system cache.** A file system can also be used to store commonly accessed files; CDNs are one example of a distributed file system that take advantage of geographic locality.

![](<../../Attachment Folder/Pasted image 20261103161402.png>)
# Caching policies
Since caching is meant to be fast and temporary, it is often implemented with more expensive and less resilient hardware than other types of storage. 
For this reason, caches are typically smaller than the primary data storage system and must selectively choose which data to keep and which to remove (or _evict_). 
This selection process, known as a **caching policy**, helps the cache free up space for the most relevant data that will be needed in the future.

Here are some common examples of caching policies:

- **First-in first-out (FIFO).** Similar to a queue, this policy evicts whichever item was added longest ago and keeps the most recently added items.
- **Least recently used (LRU).** This policy keeps track of when items were last retrieved and evicts whichever item has not been accessed recently.
- **Least frequently used (LFU).** This policy keeps track of how often items are retrieved and evicts whichever item is used least frequently, regardless of when it was last accessed.

In addition, caches can quickly become out-of-date from the true state of the system. This speed/accuracy tradeoff can be reduced by implementing an **eviction policy** - most commonly by **limiting the time-to-live (TTL)** of each cache entry, and updating the cache entry before or after database writes (write-through vs. write-behind).

# Cache coherence

- A **write-through cache** updates the cache and main memory simultaneously, meaning there's no chance either can go out of data. It also simplifies the system.
- In a **write-behind cache,** memory updates occur asynchronously. This may lead to inconsistency, but it speeds things up significantly.

Another option is called **cache-aside** or **lazy loading** where data is loaded into the cache on-demand. First, the application checks the cache for requested data. If it's not there (also called a "**cache miss**") the application fetches the data from the data store and updates the cache. This simple strategy keeps the data stored in the cache relatively "relevant" - as long as you choose a cache eviction policy and limited TTL combination that matches data access patterns.

| Cache Policy                | Pros                                                                                    | Cons                                                                                            |
| :-------------------------- | :-------------------------------------------------------------------------------------- | :---------------------------------------------------------------------------------------------- |
| Write-through               | Ensures consistency. A good option if your application requires more reads than writes. | Writes are slow.                                                                                |
| Write-behind                | Speed (both reads and writes.)                                                          | Risky. You’re more susceptible to issues with consistency and you may lose data during a crash. |
| Cache-aside or Lazy Loading | Simplicity and reliability. Also, only requested data is cached.                        | Cache misses cause delays. May result in stale data.                                            |
# Further reading
- In 2013, Facebook engineers published [Scaling Memcache at Facebook](https://www.usenix.org/system/files/conference/nsdi13/nsdi13-final170_update.pdf), a now-famous paper detailing improvements to _memcached_. The concept of _leases,_ a Facebook innovation addressing the speed/accuracy problem with caching, is introduced here.
- [This blog series](https://nickcraver.com/blog/2019/08/06/stack-overflow-how-we-do-app-caching/) written by Stack Overflow's Chief Architect on the company's approach to caching (and many other architectural topics) is both informative and fun.