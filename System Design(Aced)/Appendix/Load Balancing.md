A _load balancer_ is a type of server that distributes incoming web traffic across multiple backend servers.
![](<../../Attachment Folder/Pasted image 20261027013832.png>)

# How load balancers work
A good load balancer will efficiently distribute incoming traffic to maximize the system's capacity utilization and minimize the queueing time. Load balancers can distribute traffic using several different strategies:

- Round robin: Servers are assigned in a repeating sequence, so that the next server assigned is guaranteed to be the least recently used.
- Least connections: Assigns the server currently handling the fewest number of requests.
- Consistent hashing: Similar to database sharding, the server can be assigned consistently based on IP address or URL.

Depending on the chosen strategy and performance requirements, load balancers can operate at higher or lower network layers (HTTP vs. TCP/IP) or even be implemented in hardware.

# Advantages of load balancers
- **Scalability.** Load balancers make it easier to scale up and down with demand by adding or removing backend servers.
- **Reliability**. Load balancers provide redundancy and can minimize downtime by automatically detecting and replacing unhealthy servers.
- **Performance**. By distributing the workload evenly across servers, load balancers can improve the average response time.