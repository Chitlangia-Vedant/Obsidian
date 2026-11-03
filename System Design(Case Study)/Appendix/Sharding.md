The process of breaking up large tables into horizontal data partitions, each of which contains a subset of the whole table, and putting each partition on a separate database server is called **sharding**, and each partition is called a **shard**.

# Sharding techniques
## Sharding techniques

Data is partitioned based on the user’s location, such as the continent of origin, or a similarly large area (e.g. “East US”, “West US”). Typically, a static location is chosen, such as the user’s location when their account was created.

This technique allows users to be routed to the node closest to their location, thus reducing latency. However, there may not be an even distribution of users in the various geographical areas.

## Range-based sharding
Range-based sharding divides the data based on the ranges of the key value. For example, choosing the first letter of the user’s first name as the shard key will divide the data into 26 buckets (assuming English names). This makes partition computation very simple, but can lead to uneven splits across data partitions.

## Hash-based
This uses a hashing algorithm to generate a hash based on the key value, and then uses the hash value to compute the partition. A good hash algorithm will distribute data evenly across partitions, thus reducing the risk of hotspots. However, it is likely to assign related rows to different partitions, so the server can’t enhance performance by trying to predict and pre-load future queries.

# Manual vs. automatic
Some database systems support automatic sharding, where the system will manage the data partitioning. Automatic sharding will dynamically re-partition the data when it detects an uneven distribution of the data (or queries) among the shards, leading to higher performance and better scalability.

Many monolithic databases do not support automatic sharding. If you need to continue using these databases, but have increasing data demands, then the sharding needs to be done at the application layer. However, this has some significant downsides.

1. One downside is a significant increase in development complexity. The application needs to choose the appropriate sharding technique, and decide the number of shards based on the projected data trends. If those underlying assumptions change, the application has to figure out how to re-balance the data partitions. At runtime, the application has to figure out which shard the data resides in, and how to access that shard.
2. Another challenge with manual sharding is that it typically results in an uneven distribution of data among the shards, especially as data trends differ from what they were when the sharding technique was chosen. Hotspots created due to this uneven distribution can lead to performance issues and server crashes.
3. If the number of shards chosen initially is too low, re-partitioning will be required in order to address performance regression as data increases. This can be a significantly complex operation, especially if the system needs to have no downtime.
4. Operational processes, such as changes to the database schema, also become rather complex. If schema changes are not backward compatible, the system will need to ensure that all shards have the same schema copy and the data is migrated from the old schema to the new one correctly on all shards.

# Advantages

1. Sharding allows a system to scale out as the size of data increases. It allows the application to deal with a larger amount of data than can be done using a traditional RDBMS.
2. Having a smaller set of data in each shard also means that the indexes on that data are smaller, which results in faster query performance.
3. If an unplanned outage takes down a shard, the majority of the system remains accessible while that shard is restored. Downtime doesn’t take out the whole system.
4. Smaller amounts of data in each shard mean that the nodes can run on commodity hardware, and do not require expensive high-end hardware to deliver acceptable performance.

# Disadvantages
1. Not all data is amenable to sharding.
2. Foreign key relationships can only be maintained within a single shard.
3. Manual sharding can be very complex and can lead to hotspots.
4. Because each shard runs on a separate database server, some types of cross-shard queries (such as table joins) are either very expensive or not possible.
5. Once sharding has been set up, it is very hard (if not impossible) on some systems to undo sharding or to change the shard key.
6. Each shard is a live production database server, so needs to ensure high-availability (via replication or other techniques). This increases the operational cost compared to a single RDBMS.
# Further reading

- [This post](https://developer.squareup.com/blog/sharding-cash/), featured on Square's engineering blog has it all - technical detail, business context, drama, and a few helpful gifs.