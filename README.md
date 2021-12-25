# Design-and-Architecture
Design and Architecture Notes

## How Hashmap works internally
## How Git works internally
## How Guava Cache works internally

## Design LRU Cache
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

Client:
Backend: In AWS EC2 instance. 3 AWS regions. Within each region, 3 AZ. Videos in S3.
CDN: Netflix's custom global CDN is called OpenConnect. OpenConnect stores videos in different edge locations around the world. 


## How to process large files?
## Generate Unique ID

## Design IoT system

## Kubernetes

## Java Streams

## Database Concepts

### Sharding

Sharding is the method of distributing a single dataset across multiple machines. It is a form of Horizontal Scaling. 
Advantage: Increase scaling capability. Increased read/write throughput.
Disadvantage: It increases system complexity as we need to distribute data based on shard and properly route requests. 

Types:
Ranged Sharding: Allocates a range for a field on a shard. Need a lookup table.
Hashed Sharding: Apply hashing alogorithm on a field. No need for lookup table. Rehasing results in an outage
Entity based Sharding: Keep related data together.
GeoSharding: Shared key based on geography and shards themselves are geo-located.

### Load Balancing

## Reference
https://github.com/jwasham/coding-interview-university
https://github.com/joydeep1982/system-design-interview
https://github.com/checkcheckzz/system-design-interview
https://github.com/donnemartin/system-design-primer
https://github.com/codersguild/System-Design
http://highscalability.com/all-time-favorites/
