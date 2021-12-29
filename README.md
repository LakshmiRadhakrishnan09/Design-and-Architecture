# Design-and-Architecture
Design and Architecture Notes

## How Hashmap works internally
## How Git works internally


## Design LRU Cache

1. Using HashMap and Doubly linked list
HashMap : For random access \
Doubly linked list: to keep track of least recetly used item \
    add: Insert item at tail \
    evict: Remove item at head \
    get: remove item(from anywhere) and add at tail \
    Note: If a singlelinkedlist is used, to remove an item at middle, we need to traverse the entire list. If an array is used, then also removal needs rearranging items. With doubly linked list removal of middle item can be acheieved in o(1) time. \
    
    ![image](https://user-images.githubusercontent.com/33679023/147654848-9a493bc0-ca8e-4566-b20b-7a43e3be9bbd.png)

Ref: https://www.baeldung.com/java-lru-cache

LRUCache(K,V> \
    Map<K, LinkedListNode<CacheElement<K,V>>> map = new ConcurrentHashMap<>(size); \
    DoublyLinkedList<CachedElement<K,V>> = new DoublyLinkedList<>(); \
    ReentrantReadWriteLock lock = new ReentrantReadWriteLock(); //gives more flexibility than synchronized methods. \

2. Using LinkedHashMap
LinkedHashMap: Maintains iteration order(insertion-order or access-order - controlled by parameter "accessOrder" passed to constructor). Uses a doubly-liked list through all of its entries. removeEldestEntry(eldestEntry) internal method handles removal of eldest entry. This method can be overridden. This is not thread-safe. Use Collections.synchronizedMap(new LinkedHashMap) or synchronized methods.


## How Guava Cache works internally

How Gauva Cache expires items:\
There is no thread to remove items at expiry time. Expired items are removed during read() or write() operations. Guava Cache maintains a linked list of entries from least recent access to most recent access. When an entry is accessed, it removes itself from its position in the linked list and moves itself to end of queue. When evict need to be performed, items are removed from linked list from front of the queue until it finds an unexpired entry.


## Design Parking Lot
## Design Railway Reservation System
## Design Elevator System

## Design a Tiny URL system
## Design Whatzapp messager
## Design Twitter
## Design a search engine
## Design Facebook
## Design StackOverFlow
## Netflix

Client: \
Backend: In AWS EC2 instance. 3 AWS regions. Within each region, 3 AZ. Videos in S3. \
CDN: Netflix's custom global CDN is called OpenConnect. OpenConnect stores videos in different edge locations around the world.


## How to process large files?
## Generate Unique ID

## Design IoT system

## Kubernetes

## Java Streams

## Database Concepts

### Sharding

Sharding is the method of distributing a single dataset across multiple machines. It is a form of Horizontal Scaling. \
Advantage: Increase scaling capability. Increased read/write throughput. \
Disadvantage: It increases system complexity as we need to distribute data based on shard and properly route requests.

Types:\
Ranged Sharding: Allocates a range for a field on a shard. Need a lookup table. \
Hashed Sharding: Apply hashing alogorithm on a field. No need for lookup table. Rehasing results in an outage \
Entity based Sharding: Keep related data together. \
GeoSharding: Shared key based on geography and shards themselves are geo-located.

## Load Balancing

Ensure Availability: Direct traffic to only healthy servers. \
Ensure Scalability: Can add more resources at backend. \

### Algorithm Techniques
<ul>
  <li>RoundRobin</li>
  <li>Weighted RoundRobin</li>
  <li>Least Connection</li>
  <li>Resourse Based</li>
  <li>Source IP Hash</li>
</ul>

### Use case discussions
1. In a shopping cart application, how added items can be stored?
   * Locally in browser. If you are using a load balancer, all request from client should be sent to same backend server during the duration of session. 
   * How to store session in browser?
2.  



## Reference
https://github.com/jwasham/coding-interview-university \
https://github.com/joydeep1982/system-design-interview \
https://github.com/checkcheckzz/system-design-interview \
https://github.com/donnemartin/system-design-primer \
https://github.com/codersguild/System-Design \
http://highscalability.com/all-time-favorites/
