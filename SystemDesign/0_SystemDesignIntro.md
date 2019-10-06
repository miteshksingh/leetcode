## Steps(RIEDHDB) to follow during SDIs

* Requirement Classifications
* System interface definitions
* Estimations
* Defining Data Model
* High Level Design
* Detailed Design
* Identifying and Resolving Bottlenecks

## Requirement Classifications

* Functional requirements - Most Important Ones
* Non Functional requirements - Latency, Availability, Eventual Consistency
* Extended requirements - Search, etc

### Estimation
* Traffic Estimation (#write tps, #read tps for 2 major APIs)
* Storage Estimation (#bytes for text, #image/videos storage, #bytes for metadat, storage need for next 5 years)
* Network Bandwidth Estimation (how to load balance)
* Cache Estimation (80:20 rule)

### Data Model
Entities, Relationships, NoSQL(key value store for reliability?)  vs SQL, Transportation
Which DB columns will have encryption?
Which DB columns will have index?

Ex - Tweet, User, Account, UserFollow, Photo, UserPhoto

### High Level Design
Clients -> Load Balancer -> Application Servers -> Databases/File Storage

* Storage for image/videos - Generally distributed filed storage HDFS/S3
* Separate server for reads and writes?
* DB should support huge number of reads?
* Estimates per second are average numbers, we should always prep for more. Ex - 325K read tweets ~ 1M read tweets

### Detailed Design
Pros & Cons of different approaches keeping constraints in mind

* Partition our data to distribute it to multiple databases?
* Store all data of a user on the same DB?
* Handling hot users who tweet a lot?
* When to introduce Cache?
* Should we add load balancing for DB?

### Identifying Bottlenecks

* Any single point of failure in our system?
* Enough data replicas, enough node copies running?
* How to monitor performance?







