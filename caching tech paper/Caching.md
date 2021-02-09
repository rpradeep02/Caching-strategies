 # Caching Strategies
## _**INTRODUCTION**_ :
   Caching technique is used to speed up the data lookups. Instead of reading the data directly from the source, which could be a database or any other remote system, the data is directly read from a cache on the computer that needs the data. It is also a storge area tha is closer to the entity needing it than the original source. Accessing this cache is faster than accessing the data from its original source. A cache  can be stored in memory or on disk.
###  _**Benefits of using cache :**_ :
- Through the caching we can reduce worloads for database and make data easied to access this improves the performance drastically
- Due to the distributed to the cache system  the Workload of backend query has lower costs and allows more flexibility in processing of data.
- cache can still provide continuous service to the application, making the system more resilient to failures.even if backend database server is unavailable
## _**1.CACHING IN SOFTWARE SYSTEM**_ :
    Caching of data can occur at many different levels in a software system. For Example:
           
            1.1. Database
            1.2. Web Server
            1.3. Browser
![title](Images/caching.png)

![title](Images/intro2.png)

### 1.1. _**Database**_ :
        The database may cache data in memory so it does not have to read it from disk.
        A three-tier architecture is a model are,

                1.1.1. Presentation Layer
                1.1.2. Application Layer
                1.1.3. Data Tier
![title](Images/database1.png)
> 1.1.1. _Presentation Layer_ :
> 
        Presentation Layer is the User interface that users uses to interact with. It translates tasks and results from the servers so that the users can understand.

> 1.1.2. _Application Layer_ :
> 
        Application Layer is known as the coordinator of the application. It processes commands, makes logical decisions and processes data between the user interface and the database.

> 1.1.3. _Data Tier_ :
> 
        This is where the data and files are actually stored.


### 1.2 _**Web Server**_:
        The web server may cache static files like images, CSS files, JavaScript etc. in memory instead of reading that from disk every time they are needed. 

### 1.3. _**Browser**_ :
        The web application may cache data read from the database, so it does not have to access that in the database via the network every time it is needed. And finally the browser may cache static files and data too. In HTML5 browsers have local storage, an application cache, and a web SQL database

## _**2.Populating the Cache**_ :
        There are two ways to populate the cache

            2.1. Upfront population
            2.2. Lazy population

>2.1. _Upfront population_ :
        
**Upfront population**  refers to that we can populate the cache with all the needed values when the system keeping the cache is starting up. To do this we need to know what data to populate the cache with
        


>2.2. _Lazy population_ :

**Lazy Pupulation** is a caching strategy that loads the data  to the cache only when its necessary. . If the data doesn't exist in the cache it requests the data from data store then the data store then returns the data to the application




| Populations      | Advantages | Disadvantages    |
| :---        | :----   |          :--- |
| Upfront population     | Upfront population has no one-off cache read delays like lazy population has.      | The initial build of the cache make take a long time and    |
Upfront population      | ...|  You risk caching data that is never read.
| Lazy Pupulation                | Lazy population only caches data that is actually read.        | Lazy population of the cache will not have any speedup from the first cache read, since this is the time the data is fetched from the remote system and inserted into the cache. This may result in an inconsistent user experience.    |
Lazy population|Lazy population has no upfront cache build delay.|..|


## _**3.Maintain Cache and Remote System in Sync :**_
        The performence of a software can be affected if we dont maintain the dara stored in the cache he data stored in the remote system in a sync

### _**3.1 Write-through Caching :**_
        A write-through cache is a cache which allows both reading and writing to it. If the new data is written to a cache that is already exiting, the data is also written to the remote system and the writes are written to the remote system
        Write-through caching works if the remote system can only be updated via the computer keeping the cache. If all data writes goes through the computer with the cache, it is easy to forward the writes to the remote system and update the cache correspondingly

### _**3.2 Active Expiry :**_
        the data in the cache is made up-to-date as fast as possible after the update in the remote system and we don't have any unnecessary expirations for data that has not changed, as you may have with time based expiration. We need to be able to detect changes to the remote system. If your remote system is a relational database is considered as a disadvantage, and this database can be updated through different mechanisms, each of these mechanisms need to be able to report what data they have updated. Otherwise you cannot send an expiration message to the computer keeping the cache.

### _**3.3 Managing Cache Size :**_

        This is used to manage how much data you store in the cache and  managing the cache size is typically done by evicting data from the cache, to make space for new data

                3.3.1. Time based eviction.
                3.3.2. First in, first out (FIFO).
                3.3.3. First in, last out (FILO).
                3.3.4. Least accessed.
                3.3.5. Least time between access.

> 3.3.1. _Time based eviction._ :
            
        Time based eviction can be used for keeping the cache in sync with the remote system and also to keep the cache size down. Either you have a separate thread running which monitors the cache, or the clean up is done when attempting to read or write a new value to the cache.

> 3.3.2. _First in, first out_ :

        when you attempt to insert a new value into the cache, you remove the earliest inserted value to make space for the new one.

> 3.3.3. _First in, last out_ :

        This method is useful if the first stored values are also the ones that are typically accessed the most. its the opposite of FIFO    

> 3.3.4. _Least accessed._ :

        The cache values that have been accessed the least number of times are evicted first, but the cache has to keep track of how many times a given value has been accessed. this technique is used to avoid having re-read and store often read values. 
> 
> 3.3.5. _Least time between access_ :

        This process takes the time between accesses of a value into account. When a value is accessed the cache marks the t`ime the value was accessed and increases the access count. When the value is accessed the next time, the cache increments the access count, and calculates the average time between all accesses. Values that were once accessed a lot but fade in popularity will have a dropping average time between accesses. Sooner or later the average may drop low enough that the value will be evicted.

## _**4. Clustered cache :**_
        A simple write-through cache cannot be used in a cluster of servers because unlike a single serve your application is distributed across a cluster of servers. If a write operation is executed on a server the cache on the other servers will know nothing of that write operation. In this case we can use either time based expiration or active expiration to make sure that all caches are in sync with the remote system.

![title](Images/cluster.png)        

## _**5. Caching Strategies :**_

### _**5.1 Cache-Aside :**_
![title](Images/cacheaside.png) 

    The application first checks the cache. then If the data is found in cache, we’ve cache hit. The data is read and returned to the client.If the data is not found in cache, we’ve cache miss. The application has to do some extra work. It asks the database to read the data, returns it to the client and stores the data in cache so the subsequent reads for the same data results in a cache hit.
    Cache-aside caches are usually general purpose and work best for read-heavy workloads. Systems using cache-aside are resilient to cache failures. even if the cache cluster goes down, the system can still operate by going directly to the database.
    the most common write strategy cache-aside is to write data to the database directly. so that cache may become inconsistent with the database. we can use time to live (TTL) and continue serving stale data until TTL expires. we can either invalidate the cache entry or use an appropriate write strategy

### _**5.2 Read-Through Cache :**_
![title](Images/readthrough.png) 

        When there is a cache miss, it loads missing data from database, populates the cache and returns it to the application. Both cache-aside and read-through strategies load data lazily only when it is first read. here the data model is different than that of the database in cache-aside.this process is supported by the library or stand-alone cache provider.
        This caches work best for read-heavy workloads when the same data is requested many times. 
        And the disadvantage of this is that when the data is requested the first time, it always results in cache miss and incurs the extra penalty of loading data to the cache. we can deal with this by ‘warming’ or ‘pre-heating’ the cache by issuing queries manually. 

### _**5.3 Write-Through Cache :**_
![title](Images/writethrough.png) 

        In Write-Through Cache data is first written to the cache and then to the database. The cache sits in-line with the database and writes always go through the cache to the main database. The data is written to the cache first and then to the main database because of that they introduce extra write latency
        To avoid this we can  pair it with read-through caches, this way we get all the benefits of read-through and we also get data consistency guarantee, freeing us from using cache invalidation techniques.
        DynamoDB Accelerator (DAX) is a good example of read-through / write-through cache. It sits inline with DynamoDB and your application. Reads and writes to DynamoDB can be done through DAX.

### _**5.4 Write-Around :**_
![title](Images/writearound.png) 

        Here, data is written directly to the database and only the data that is read makes it way into the cache. To provides good performance in situations where data is written once and read less frequently or never. we can combine read-through with Write-around

### _**5.5 write-back:**_
![title](Images/write-back.png)

        This writes the data back to the database. also the application writes data to the cache which acknowledges immediately and after some delay.
        This cache can improve the write performance and are good for write-heavy workloads. and it can be combined with read-through.This cache is also good for mixed workloads, where the most recently updated and accessed data is always available in cache.It’s resilient to database failures and can tolerate some database downtime. If batching or coalescing is supported, it can reduce overall writes to the database, which decreases the load and reduces costs, if the database provider charges by number of requests e.g. DynamoDB. Keep in mind that DAX is write-through so you won’t see any reductions in costs if your application is write heavy.
        we can use Redis for both cache-aside and write-back to better absorb spikes during peak load. but that disadvantage is that if there’s a cache failure, the data may be permanently lost.

## _**CONCLUSION :**_
        The objecyive of this paper is to improve the Performance and scaling issues that encountered during the new project this paper containns different caching strategies and their pros and cons. we can carefully evaluate our goals, understand data access (read/write) patterns and choose the best strategy or a combination.
## _**REFERENCES :**_
1.Referance on what is caching[Link](https://www.youtube.com/watch?v=_F1U6Rh0wfo&ab_channel=CodingSimplified)

[Link2](https://www.youtube.com/watch?v=RgPf5RDv4-s&ab_channel=Udacity)

2.caching strategies[Link](https://aws.amazon.com/builders-library/caching-challenges-and-strategies/)

3.Understanding and Implementing Cache[Link](https://www.instana.com/blog/scaling-microservices-understanding-and-implementing-cache/)
