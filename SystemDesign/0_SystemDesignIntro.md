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
  * Sharding by UserId? Problem of hot users
  * Sharding by TweetId? Problem of querying all servers
  * Sharding by tweet creation time? Problem of unbalanced load - only few servers will have all the load
  * Sharding by tweetid and tweet creation time
* Store all data of a user on the same DB?
* Handling hot users who tweet a lot?
* Cache (may be cache of hot tweets and users/ cache of tweets in last 3 days/ cache photos or videos)?
  * Have different servers for cache(say Memcache) which will be queried by app servers
  * Data structure used for Cache? May be ownerId and list of tweetids for that owner? 
  * Which cache replacement policy? LRU?
  * Distribute load on cache even if one server can cache all data(100 GB)?
  * Intelligent Cache? 
    * 20% of tweets generating 80% of read traffic which means such tweets are so popular that majority of people read them
    * Thus cache 20% of read volume per shard
* Should we add load balancing for DB?

### Identifying Bottlenecks

* Any single point of failure in our system?
* Enough data replicas, enough node copies running?
* How to monitor performance?

### Monitoring

* Collect metrics/counters like
  * tweets per/second, what's the daily peak?
  * how many tweets per day are we delivering?
  * Average latency for refresh timeline
* This helps us in understanding if we need more LBs


## Types of System Design Questions
1. Social Media Apps (Facebook, Insta, Twitter)
   * Graph Query 
   
2. Chat Apps (Whatsapp, Messenger)
3. Streaming Apps (Youtube, Netflix)
   * Encoding
   * Dedup
   * Thumbnails
   * Live Videos (UDP vs TCP)
4. Location Apps (Restaurants, Movies, Events)
   * Live Locations









