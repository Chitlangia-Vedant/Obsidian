A _cache_ is any data store that can store and retrieve data quickly for future use, enabling faster response times and decreasing load on other parts of your system
# How caching works

Caching can be implemented in many different ways in modern systems:

**In-memory application cache.** Storing data directly in the application's memory is a fast and simple option, but each server must maintain its own cache, which increases overall memory demands and cost of the system.

**Distributed in-memory cache.** A separate caching server such as Memcached or Redis can be used to store data so that multiple servers can read and write from the same cache.

![database cache](https://exponent-content.s3.amazonaws.com/database_cache_1cef5d1245.png)

**Database cache.** A traditional database can still be used to cache commonly requested data or store the results of pre-computed operations for retrieval later.

**File system cache.** A file system can also be used to store commonly accessed files; CDNs are one example of a distributed file system that take advantage of geographic locality.

![cdn cache](https://exponent-content.s3.amazonaws.com/cdn_cache_b36d25deaf.png)
# Caching policies
Since caching is meant to be fast and temporary, it is often implemented with more expensive and less resilient hardware than other types of storage. 
For this reason, caches are typically smaller than the primary data storage system and must selectively choose which data to keep and which to remove (or _evict_). 
This selection process, known as a **caching policy**, helps the cache free up space for the most relevant data that will be needed in the future.