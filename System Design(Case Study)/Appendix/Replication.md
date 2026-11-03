Database replication is basically what you think it is: copying data from one data source to another, thus replicating it in one or more places. 

There are many reasons for copying data - to protect against data loss during system failures, to serve increased traffic, to improve latency when pursuing a regional strategy, etc.

# How does it work?
## Leader-follower (or primary-replica)

A query writes to a single designated leader. The leader then replicates the updated data to followers.

![](<../../Attachment Folder/Pasted image 20261103165232.png>)

**Synchronous replication** requires that both the leader and followers must commit before the write is considered successful. While this ensures follower data is up-to-date, the real-world implications can be tricky or even unacceptable. If a follower in the chain goes down, the write query will fail, and even if the whole system is up, waiting for a follower located halfway across the world will raise latency considerably.

**Asynchronous replication** may be an option for use cases where transaction speed is more important than consistently accurate data. With asynchronous replication, the leader sends writes to its followers and moves on without waiting for acknowledgment. Faster, yes, but this introduces inconsistency between the leader and followers, which can be a huge problem if the leader goes down and the most up-to-date data is lost.

 In the case of a simple leader-follower strategy when the leader fails (also called a **failover**), the replica is promoted to be the leader and takes over.

Failover is a huge problem under asynchronous replication, but it's not great under synchronous either. Without a leader, you lose the ability to handle writes

## Leader failures and consensus algorithms

A simple way to mitigate leader failure is to designate more than one leader. **Leader-leader or multi-leader replication** simply means that more than one database is available to take writes, meaning, if one leader goes down, the other can step in. This does introduce a slight lag as data must be replicated to both (or more) leaders and engineers must contend with more complexity — mainly conflict resolution when discrepancies arise between leaders — but the added durability mostly outweighs the additional lag time in the real world.

**Consensus algorithms** can be used to "elect" a new leader if one of more leaders goes down, adding another layer of protection to the system. The most common consensus algorithm is **Paxos**. Many consider it a difficult algorithm to understand, perhaps because the leader election process is part of a larger process which aims to reach agreement through data replication.

A newer alternative to Paxos called **Raft** effectively breaks the agreement process into two steps, thereby making leader election easier to understand.
## Leaderless replication
There are methods for dealing with the chaos that comes with managing a network of read-AND-write-capable replicas.

- **Read repair** allows clients to detect errors (e.g. several nodes return a consistent value, but one node returns something else) and fix them by sending a write request to the inconsistent node.
- Background processes that fix errors exist in many cloud-based products. For example, Amazon's DynamoDB uses an "anti-entropy" function. Read more below.
- **Quorums** allow replicas to pull up-to-date information quickly in asynchronous leaderless replication by specifying a minimum number of replicas that need to accept a write before reading.

# When to implement a replication strategy
- **Need to service lots of reads?** Go with a simple leader-follower replication strategy. Read replicas are simple and cheap. This is the best option if you have a read-heavy application like an online news source, or if your read-heavy system is scaling globally and you want to provide a consistent user experience.
- **Need to increase the reliability of your system?** Go with multi-leader replication so that if and when a leader goes down, you can continue to operate without data loss. You will have to include some sort of conflict resolution strategy, though. More specifically, multi-leader is most often used when you're scaling across multiple data centers, because you'd want to have one leader _in each data center_ that can perform writes — and then replicate to other data centers.
- **Need to service lots of writes or scale-up globally?** Consider a leaderless solution. If your system runs on-premise as opposed to in the cloud, make sure you build in appropriate conflict resolution strategies.
- **Pursuing a multi-region strategy?** Use replicas as database backups for disaster recovery on a per-region basis. For example, you want to be able to handle major outages or natural disasters that affect particular regions, so you implement a multi-leader strategy in affected regions to handle writes in case of failover.
# Further Reading

- Read [Amazon's original DynamoDB paper](https://www.allthingsdistributed.com/2007/10/amazons_dynamo.html) for an interesting discussion around the challenges the team faced in fusing techniques like consistent hashing, quorum, anti-entropy-based recovery, and more.
- Try your luck with [Paxos Made Simple](https://www.microsoft.com/en-us/research/publication/paxos-made-simple/?from=http%3A%2F%2Fresearch.microsoft.com%2Fen-us%2Fum%2Fpeople%2Flamport%2Fpubs%2Fpaxos-simple.pdf), the slightly frustrated attempt by brilliant computer scientist Leslie Lamport to explain his widely-used but little-understood consensus algorithm.