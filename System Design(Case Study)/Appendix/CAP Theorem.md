# Partition tolerance and theorem definition
A network partition is a (temporary) network failure between nodes. Partition tolerance means being able to keep the nodes in a distributed database running even when there are network partitions. 

The theorem states that, in a distributed database, you can only ensure consistency or availability in the case of a network partition.

**Example Scenario**: To better understand the situation with network partitions, we give an example scenario. In a multi-primary architecture, data can be written to multiple nodes. Let’s pretend that there are two nodes in this example database. When a write is sent to Node A, Node A syncs the update with Node B, and any read requests sent to either of the two nodes will return the updated result.

![](<../../Attachment Folder/Pasted image 20261103161416.png>)

If there is a network partition between Node A and Node B, then updates cannot be synced. Node B wouldn’t know that Node A received an update, and Node A wouldn’t know if Node B received an update. The behavior of either node after receiving a write request depends on whether the database prioritizes consistency or availability.

![](<../../Attachment Folder/Pasted image 20261103161422.png>)

# Consistency
Consistency is the property that after a write is sent to a database, all read requests sent to any node should return that updated data. In the example scenario where there is a network partition, both Node A and Node B would reject any write requests sent to them. This would ensure that the state of the data on the two nodes are the same. Or else, only Node A would have the updated data, and Node B would have stale data.

![](<../../Attachment Folder/Pasted image 20261103161735.png>)

# Availability
Availability means that we prioritize nodes to successfully complete requests sent to them. Available databases also tend to have eventual consistency, which means that after some undetermined amount of time when the network partition is resolved, eventually, all nodes will sync with each other to have the same, updated data. In this case, Node A will receive the update first, and after some time, Node B will be updated as well.

![](<../../Attachment Folder/Pasted image 20261103162135.png>)

# When should consistency or availability be prioritized?
If you’re working with data that you know needs to be up-to-date, then it may be better to store it in a database that prioritizes consistency over availability. 

On the other hand, if it’s fine that the queried data can be slightly out-of-date, then storing it in an available database may be the better choice.
# Popular databases that prioritize consistency vs. availability

| Prioritize Consistency | Prioritize Availability |
| :--------------------- | :---------------------- |
| SQL Databases          | Cassandra               |
| MongoDB                | AWS DynamoDB            |
| Redis                  | CouchDB                 |
| Google BigTable        |                         |
| HBase                  |                         |
