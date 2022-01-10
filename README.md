# Design-and-Architecture
Design and Architecture Notes

## How Hashmap works internally
## How Git works internally

* File system + hash + tree + blob \
Every commit is a hash \
Every hash is stored in file system as a folder. First two characters of hash as folder name. Remaining part as files inside folder. \
Each file is either a tree or a blob. \
tree refer to other blobs. \
Each blob is the actual file committed as part of that commit. \
Utlities available to show wach file. \
	
* Git diff utility \
Algorithm based on String - Minimum changes required to transform string1 to string2. \

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

**Requirement:** For a given long url , create a short url( may be of 7 characters). \
**How to create 7 character unique short url:** 
+ We cannot use UUID or GUID as they are longer. 
+ We can use Base62 strings - 26 small alphabets, 26 capital alphabets and 10 numbers. 26 + 26 + 10 = 62 characters. 7 positions. Total 62 ^ 7 unique combinations possible(3.2 trillion). They can be used as tiny urls. Each app server generate a new number and get the base62 encoded string. 
+ We can use MD5 hash for the long url. MD5 character length is more, so we can use first 7 characters. 
	
**Problem:** 
+ If we use Base62: If we have multiple app servers that generate random numbers, it can result in same tiny url for two different long url. 
+ If we use MD5: It is possible that first 7 chracters are same for two different long urls. It can result in same tiny url for two different long url. 

**Solution:** 
1. After generating tiny url, check if it is present in DB. Problem: This can result in race condition. Not a best solution.
2. Use RDBMS, unique key constraint. Problem:Not possible for NoSql DBs.
3. Use a single server service which generates sequential counters. All app servers get key from this service. Problem: This can cause Single Point of failure.
4. Each app server keeps a counter. Counter can be range based. 0 to one lakh for first app server,etc. Problem:Coordintion is difficult. How to manage if range is exhausted?Service is lost?
5. Use zookeeper. Provides distributed coordination. Keep a range and allocate to each appserver. Appserver use the counter to generate encoded base62 string.

**What database we can use?**
Nosql dbs : Cassandra, DynamoDB or Riak

**APIs**
1. createTinyUrl(longUrl)
2. getLongUrl(tinyurl)

**Cloud Services**
* Lambda
* DynamoDB
* API gateway

**Ref**
https://aws.amazon.com/blogs/compute/build-a-serverless-private-url-shortener/ \
https://www.youtube.com/watch?v=JQDHz72OA3c

## Design Whatzapp messager
## Design Twitter
## Design Uber
## Design Instagram
## Design a search engine
## Design Facebook
## Design StackOverFlow
## Netflix

Client: \
Backend: In AWS EC2 instance. 3 AWS regions. Within each region, 3 AZ. Videos in S3. \
CDN: Netflix's custom global CDN is called OpenConnect. OpenConnect stores videos in different edge locations around the world.


## How to process large files?
## Generate Unique ID

UUID : Are globally unique. But they are very big. \
Centalized system to generate unique id : Can result in bottelnecks. \
Custom generator: Unique Id based on epoch time + Machine address + Counter \

## Design IoT system

## Microservices

Service-oriented architecture composed of loosely coupled elements that have bounded contexts. \
Consists of multiple services(Order processing, Shipping...). Each one is a microservice.Each service has a well-defined boundary in the form of an RPC or message driven API. \
Each service has its own database -  polyglot persistence architecture.

Drawbacks:
* inter-process communication : need to handle partial failures.
* partitioned database architecture: Transactions are difficult in distributed databases. Need to update multiple databases owned by different services. Need to use eventual consistency based approach.
* Testing is complicated.
* Service discovery is needed.
* Cloud Foundry(PaaS) or Kubernetes(your own PaaS) - Read more on CF

Protocols
* HTTP
* Websocket
* STOMP
* Trift binary RPC
* AMQP messaging protocol

Formats
* JSON
* XML
* Binary
	* Avro
	* Protocol Buffers

Microservice inter process communication
1. asynchronous, messagging based mechanism - Notification, Pub/sub, Async response (using RxJava, CompletableFuture). Message Broker: JMS or AMQP. Brokerless: Zeromq. Messaging systems:  RabbitMQ, Apache Kafka, Apache ActiveMQ, and NSQ.
2. synchronous : such as HTTP or Thrift

Service Discovery \
* Client Side: Client queries the service registry. Client selects one of the available service instance(load balancing need to be implemented at client side). Application specific routing can be implemented). Eg: Netflix OSS, Netflix Ribbon.
* Server Side: Client makes a request to a loadbalancer. LB queries the service registry. Eg: AWS ELB
* In both cases , clients registers with a service registry (which is a database of available service instances). Eg: Netflix Eureka, etcd, consul , zookeeper

Service Registeration \
* Self Registeration: a service instance is responsible for registering and deregistering itself with the service registry.  Netflix OSS Eureka client, Spring Cloud makes it easier with annotations.
* Third Party registeration pattern:  NetflixOSS Prana,  Registrator project

How to deploy a Eureka Server in AWS - Runs with Elastic IP. One or more Eureka Server in each AZ. 

Clarification: With kubernetes , Marathon and  AWS service discovery is handled by framework itself. Kubernetes and Marathon handle service instance registration and deregistration.

API Gateway \
For displaying a single page in client application(UI), multiple backend microservices need to be called. Problem: Client code will be complicated, multiple request over public internet. A better approach is to use API gateway. Act as a single entry point to the system. Eg: Netfliz API Gateway, Kong API Gateway, Azure API Gateway, AWS API Gateway. Can handle request routing, composition of response ( a single product api , aggregates product information and recommendation) and protocol transmission.

To implement a scalable API Gateway- On the JVM you can use one of the NIO-based frameworks such Netty, Vertx, Spring Reactor, or JBoss Undertow.Netflix created RxJava for the JVM specifically to use in their API Gateway.

Implementing a microservice architecture- considerations \
Performance and Scalability
Using a Reactive Programming Model
Service Invocation or inter process communication
Service Discovery
Handling Partial Failures : Should never block  indefinitely waiting for a downstream service. Staregies: Network timeouts.Use timeouts when waiting for a response. Ensures that resources are never tied up indefinitely. Based on scenarion handle failures. If recommendation service is down, return product information and replace recommendation with empty list or hardcoded top ten default list. If product information is failing return error code or return it from cache.Netflix Hystrix: Implements a circuit breaker pattern. If you are using the JVM you should definitely consider using Hystrix.


Handling transactions across microservices : Event driven architecture
	Need to implement compensating transactions
	Challenge: Dealing with duplicate events
		   Dealing with Atomicity: update the database and publish the event. These two operation should be done atomically.
		   Solution: Publishing Events Using Local Transactions: The trick is to have an EVENT table, which functions as a message queue, in the database that stores the state of the business entities. The application begins a (local) database transaction, updates the state of the business entities, inserts an event into the EVENT table, and commits the transaction. A separate application thread or process queries the EVENT table, publishes the events to the Message Broker, and then uses a local transaction to mark the events as published.
		Order Service
			Begin Transaction
				Insert Order table
				Insert Event table
			Commit Transaction
		Event Publisher	
			Queries event table and publish the event and update event table  as published

		Problem: Database need to support transactions. May not be supported by NoSql dbs.
		Solution: Mining a Database Transaction Log Eg DynamoDB Streams
		Event Sourcing: stores a sequence of state-changing events. 

Handling queries that retrieve data from multiple microservices :Materialized view using a Doucment Db(MongoDB). A service that listens for events and update the view.

Monolithic to Microservice:
* Strangler Application: https://martinfowler.com/bliki/StranglerFigApplication.html
* incrementally refactor your monolithic application
* run it in conjunction with your monolithic application
* Add new features in standalone microservice
* Split Frontend and Backend

etcd : service discovery and shared configuration . Key value pair\
zookeeper: orchestration between different services. To maintain who is alive and active. \
Eureka: Service Registration and Discovery \
Ribbon: Dynamic Routing and Load Balancer \
Hystrix: Circuit Breaker \
Zuul: Edge Server \

https://microservices.io/patterns/microservices.html \
https://github.com/merikbest/ecommerce-spring-reactjs \
Read more: https://queue.acm.org/detail.cfm?id=1394128

## Openshift

WebConsole for K8s. provides monitoring and logging integration. Add concepts of projects and users.

* Pod: one or more containers guaranteed to run on same host.The containers within pod share a unique IP address. They communicate each other using "localhost" and also share volumes(persistent storage).
* Service: A set of pods that represent same application.A Service has its on IP address and DNS name. This represents the end point for applications. Pods can die and start again, but service end point IP remanins the same. Service DNS name is accessible only with Openshift cluster. 
* Route: To make a service accessible, a route needs to be created. Creating a route automatically set up a haproxy router with an externally addressable DNS name, exposing the service and load balancing traffic across the pods.

Discussions: \
Service to pod : http or https? where is the certificate? \
Kong to openshift: what certificates

Sample Yaml
```
Kind:Service 
spec
	ports: port, targetport
	type:ClusterIP

kind:Route
matadata: annotations
		haproxy.route.openshift.io/ip_whitelist
		haproxy.route.openshift.io/timeout
spec: 
	port: sourceport, targetport
	tls 
		termination:edge 
		insecureEdgeTerminationPolicy: Redirect
	to
		kind:Service
		name:
		weight:100  
```

## Kubernetes

### Spring cloud vs Kubernetes
https://dzone.com/articles/deploying-microservices-spring-cloud-vs-kubernetes
## Java Streams

## Database Concepts

### Normalization
1NF - Each row should have a primary key \
2NF - All non-key attribute must depend on the entire PK \
3NF - Remove transitive dependencies

why we need to normalize data? \
To eliminate redundancies. \
To properly insert and update records( using a primary key)

### Sharding

Sharding is the method of distributing a single dataset across multiple machines. It is a form of Horizontal Scaling. \
Advantage: Increase scaling capability. Increased read/write throughput. \
Disadvantage: It increases system complexity as we need to distribute data based on shard and properly route requests.

Types:\
Ranged Sharding: Allocates a range for a field on a shard. Need a lookup table. \
Hashed Sharding: Apply hashing alogorithm on a field. No need for lookup table. Rehasing results in an outage \
Entity based Sharding: Keep related data together. \
GeoSharding: Shared key based on geography and shards themselves are geo-located.

### Consistent Hashing
Hashing: maps a wide range of input to a fixed range of output.  \
Distributed HashTable: A hashtable that is distributed among multiple nodes. Due to limitation of memory, hash table is split among multiple servers. The keys are distributed among several servers. \
Scenario: Load balancing, Distributed Caching(key-value pair)
server = hash(key) mod N \
The key is retrieved from corresponding server.  \
Problem: what if no of servers change. Keys need to be redestributed to include the new server. Lot of change in distribution. Most of keys are redistributed. \
Solution:Consistent Hashing \
Distributes position on a hash ring. Index is independent of number of servers.   \
server = between 0 and 360 ( circle) \
distribute keys and servers in the ring(same circle) \
Each object key will belong in the server whose key is closest, in a clounterclock wise direction ( or clockwise). To find out which server to ask for a given key, we need to locate the key on the circle and move in the ascending angle direction until we find a server. \


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

Layer 4 : Based on source and destination IP addresses and ports. Not based on contents. \
Layer 7 : Based on http request

HAProxy: LoadBalancer
nginx: LoadBalancer + Web Server

### Use case discussions
1. In a shopping cart application, how added items can be stored?
   * Locally in browser. If you are using a load balancer, all request from client should be sent to same backend server during the duration of session. 
   * How to store session in browser?
2.  

Ref:
https://www.nginx.com/blog/introduction-to-microservices/
https://www.nginx.com/resources/glossary/layer-4-load-balancing/#layers
https://www.nginx.com/resources/glossary/layer-7-load-balancing/


## Microservice Monitoring

### Spring boot - Prometheus - Grafana
Enable micrometer-registry-prometheus \
Run prometheus - time series data model. time series metric collected via a PULL model over HTTP. You configure prometheus to pull data from yout spring boot application. \
Grafana - For dashboards

### Distributed tracing

### Code instrumentation
Add code to application code while ClassLoader loads or application starts.
What are java agents? \
Java agents are part of the Java Instrumentation API. The class that implements Java Agent must implement a method called 
	''' public static void premain(String agentArgs, Instrumentation inst) '''
This method forms the entry point of the agent, much like the entry point to a regular Java program is the main method. \
ByteBuddy library. \
	''' java -javaagent:appdynamics.jar -jar app.jar '''
MainApplication.jar and javaagent jar with premain method. JavaAgent will have hooks like OnEntry and OnExit to add additional behavious to code.

## Testing

### Component benchmarking
Given memory, CPU cores, DiskIO, network, start with 1 user and increase load. See when application memory consumption reaches 85%, with no errors and response time < 500ms. That is the component benchmark. Say component1 need 3gb, 2 core and supports 10 users, component2 need 6gb, 3 core and supports 100 users. For a system that need both these components, to suppot 100 users, we need to scale component1 resources to 3gb * 10 and 2 core * 10. Test both horizontal and vertical scaling.

### BDD Testing
Java - Cucumber \
Add feature files. Add steps for Given, when, then. \
Calls API of the service to be tested. Test service is deployed as a container and test against k8s service end point.

### Performance Testing
Java - Locust script written in python. \


### Restrict concurrent access to a session
Check when multiple tabs are opened in abrowser how sessionIds are created?
Requirment: I want only one user session at a time for an account.

1. What is Saga Pattern
2. U have 30 microservices using RDBMS

## Networking
Company has a DNS Server. When you are in VPN, all requests goes to a DNS server. DNS server has A record or C record. \
When we add a endpoint/route in Kong, raise a request for creating a CNAME record pointing to corresponding enviornment's kong gateway DNS name. For "service1.endpoint" return "kongurl". \
In Kong add certificate for "kongurl". Configure Kong with backend service.

## Reference
https://github.com/jwasham/coding-interview-university \
https://github.com/joydeep1982/system-design-interview \
https://github.com/checkcheckzz/system-design-interview \
https://github.com/donnemartin/system-design-primer \
https://github.com/codersguild/System-Design \
http://highscalability.com/all-time-favorites/
