* Performance, Fault Tolerancy & Deployment

** Capped Collections
 
Capped collections are fixed-size collections that support high-throughput operations that insert and retrieve documents based on insertion order. 
Capped collections work in a way similar to circular buffers: once a collection fills its allocated space, it makes room for new documents by overwriting the oldest documents in the collection.

#+BEGIN_SRC javascript
> use performance

> db.createCollection("capped", {capped: true, size: 10000, max: 3})

> db.capped.inserOne({name: "user1"})

> db.capped.inserOne({name: "user2"})

> db.capped.insertOne({name: "user3"})

> db.capped.find().sort({$natural: -1}).pretty()

> db.capped.inserOne({name: "user4"})
#+END_SRC

** Sharding

Sharding is a method for distributing data across multiple machines. MongoDB uses sharding to support deployments with very large data sets and high throughput operations.

Database systems with large data sets or high throughput applications can challenge the capacity of a single server. 
For example, high query rates can exhaust the CPU capacity of the server. Working set sizes larger than the system’s RAM stress the I/O capacity of disk drives.

There are two methods for addressing system growth: vertical and horizontal scaling:
+ Vertical Scaling
+ Horizontal Scaling


*** Sharded Cluster

A MongoDB sharded cluster consists of the following components:

+ shard: Each shard contains a subset of the sharded data. Each shard can be deployed as a replica set.
+ mongos: The mongos acts as a query router, providing an interface between client applications and the sharded cluster. Starting in MongoDB 4.4, mongos can support hedged reads to minimize latencies.
+ config servers: Config servers store metadata and configuration settings for the cluster.

![interaction of components within a sharded cluster](https://docs.mongodb.com/manual/_images/sharded-cluster-production-architecture.bakedsvg.svg)
#+CAPTION: interaction of components within a sharded cluster
#+NAME:   interaction-ofcomponents-shared-cluster
[[https://docs.mongodb.com/manual/_images/sharded-cluster-production-architecture.bakedsvg.svg]]

** Using MongoDB Atlas

*** Connecting to the cluster

To create and use a cluster on MongoDB is necessary to:

+ create and account, then create a cluster with the desired space and configurations.
+ create a user or users with the desired permissions.
+ allow the ip's necessary for the connection.
+ connect using the link provided.


#+BEGIN_SRC javascript
> mongo "mongodb+sr*V://.8qaxe.mongodb.net/products" --username mdlima
#+END_SRC
