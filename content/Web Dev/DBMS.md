[[Web Dev/index|index]]

ACID explained in fun way :
https://www.instagram.com/reel/C2j8YlvsX4c/?utm_source=ig_web_copy_link

Log Sampling :
https://www.instagram.com/reel/C5_T3RToueG/?utm_source=ig_web_copy_link

### CAP Theorem
In distributed systems, you can only guarantee two out of three properties:

- **Consistency**: All nodes see the same data at the same time.
- **Availability**: Every request gets a response, even if some nodes fail.
- **Partition Tolerance**: System continues to function despite of network partitions (network failure splits the system into isolated subsets of nodes that cannot communicate with each other).

### ACID
https://www.youtube.com/watch?v=pomxJOFVcQs

- **Atomicity** => All or nothing (transactions, if fails rollback all)
- **Consistency** => Database should enforce consistency among data in all nodes and enforce all rules and properties defined for data
- **Isolation** => Concurrent transactions should be isolated  
- **Durability** => Once a transaction is completed it must permanent even id database loose power / crashes etc. 
	- logging and replicating data are methods to achieve this

### Mutex and Semaphore
- Mutex => Mutual Exclusion => single thread access => lock is binary
- Semaphore => multiple (but limited) threads can access => lock is a count => it's like ;load balancing if all servers are busy then queue the request

### Techniques that Databases use
- [Ben Dicken (Planet Scale Led Dev)](https://www.youtube.com/@benjdicken)

