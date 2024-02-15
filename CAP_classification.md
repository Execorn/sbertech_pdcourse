# Homework 0. Biryukov Nikita.
## CAP theorem
In theoretical computer science, the CAP theorem, also named Brewer's theorem after computer scientist Eric Brewer, states that any distributed data store can provide only two of the following three guarantees:

### Consistency
- Every read receives the most recent write or an error.

### Availability
- Every request receives a (non-error) response, without the guarantee that it contains the most recent write.

### Partition tolerance
- The system continues to operate despite an arbitrary number of messages being dropped (or delayed) by the network between nodes.

\
When a network partition failure happens, it must be decided whether to do one of the following:
1) cancel the operation and thus decrease the availability but ensure consistency
2) proceed with the operation and thus provide availability but risk inconsistency.

Thus, if there is a network partition, one has to choose between consistency or availability. Note that consistency as defined in the CAP theorem is quite different from the consistency guaranteed in ACID database transactions.


## DBMS according to the CAP theorem
### DragonFly DBMS 
Let's note that a DragonFly is a Redis replacement, and that Redis is a PC system.\
\
When considering the specific case of DragonFly, it is important to note that High Availability encompasses more than just being Partition Tolerant. DragonFly utilizes a Master-Slave architecture, where in the event of a Master failure, DragonFly Sentinels promote a Slave to assume the role of the new Master, ensuring the overall solution remains highly available.\ It is worth mentioning that a Master can fail or become unavailable due to various factors, such as running out of memory, and it is not solely attributed to a Network Partition.\
Additionally, we need to address the scenario of a Network Partition (P). If a Network Partition occurs, DragonFly becomes unavailable in the minority partition. Consequently, from a CAP (Consistency, Availability, Partition Tolerance) perspective, DragonFly is classified as CP since it becomes unavailable in minority partitions. It is important to note, however, that DragonFly will still remain available in the majority partition.\
\
Furthermore, DragonFly is referred to as eventually consistent because in the event of a Network Partition (P), the minority partition remains available for a brief period of time. Any writes performed during this period on the minority partition will eventually be discarded.

### ScyliaDB
ScyllaDB is a highly scalable NoSQL database that aims to provide high availability and partition tolerance. It is designed as an AP (Availability and Partition tolerance) system. ScyllaDB prioritizes the ability to handle network partitions and ensure continuous availability over strong consistency. In the event of network partitions, ScyllaDB allows different replicas to diverge temporarily, providing eventual consistency.

### ArenadataDB
Based on the information provided on the [official website](https://arenadata.tech/en/products/arenadata-db/), we can classify Arenadata DB (ADB) as a system that prioritizes consistency and partition tolerance, while sacrificing availability.

ADB focuses on providing reliable and scalable enterprise data storage, which suggests a focus on consistency and partition tolerance. It distributes load and data across multiple servers in a cluster, ensuring data is evenly distributed and allowing users to operate it as a simple non-cluster system without knowledge of the underlying infrastructure. This indicates a strong emphasis on partition tolerance.

Furthermore, I'd consider ADB as a PC system.
