- [What is MongoDB?](#org1296ce7)
- [Definition](#org9de5afa)
- [Understanding](#org8af1b48)
- [How does MongoDB make working so easy?](#orgf683a5c)
- [Advantages](#org1c14626)
- [Course Structure](#orgf57c870)
- [Useful Resources and Links](#org889c87e)

---


<a id="org1296ce7"></a>

# What is MongoDB?

It’s an open-source NoSQL database, developed for high performance, high availability, and easy scalability. Collection and document are the two primarily used terms/concepts in MongoDB. Here, Collection is referred to a group of these documents, which is like an RDBMS table.

![img](./resources/imgs/mongodb-collection.jpg "Collection")


<a id="org9de5afa"></a>

# Definition

These are a NoSQL database, which is cross-platform document-oriented. It uses BSON format for document storage and communication with its client. BSON is a binary form of JSON.

Let’s see some basic difference between MongoDB and RDBMS:

| Difference Between MongoDB and RDBMS | RDBMS                 | MongoDB                                                                    |
|------------------------------------ |--------------------- |-------------------------------------------------------------------------- |
| Schema                               | Fixed Schema          | Schema-less                                                                |
| Transactions                         | Supports transactions | Compromises on transactions, by giving high availability and partitioning. |
| Sharding                             | No                    | Yes                                                                        |
| Query Caching                        | No query caching here | Query caching happens here, which leads to a faster access to data.        |


<a id="org8af1b48"></a>

# Understanding

Let’s understand the core of MongoDB. It works on an extended version of JSON kn0wn as BSON (Binary JSON), which is:

1.  Lightweight
2.  Traversable
3.  Efficient

These drivers are responsible for sending and receiving data in BSON format. It stores the data as a BSON Object. Encoding to BSON and Decoding to BSON again happens very quickly, and so it’s so efficient. Here are a few terms related to MongoDB, which is used while using it. Let’s get familiar with that:

-   **Collection**: Its group of MongoDB documents. This can be thought similar to a table in RDBMS like Oracle, MySQL. This collection doesn’t enforce any structure. Hence schema-less MongoDB is so popular.
-   **Document**: Document is referred to as a record in MongoDB collection.

-   **Field**: It is a name-value pair in a document. A document has zero or more fields. Fields are like columns in relational databases.

![img](./resources/imgs/collection-hierarchy.jpg "Collection Hierarchy")


<a id="orgf683a5c"></a>

# How does MongoDB make working so easy?

-   MongoDB is based on the schema-less format for data storage and BSON format for communicating with its client, it manages and innovatively stores the data. With increased internet access, the world has drifted towards more heavy traffic flow which can be unmanageable. MongoDB is very much capable of handling heavy traffic flow for all websites and applications with ease.

-   It is based on sharding concept. It relies on vertical scaling, where you need to add more CPU and memory with the increasing demand for processing power. Here one can use more than one server to fulfil the requirement of processing power. It follows the principle of the distributed system.

-   It uses internal memory for storing the working data sets, enabling faster access to the data. It also Optimizes your schema for most frequent use cases.

-   It has a rich set of queries for performing fast and easy operations.


<a id="org1c14626"></a>

# Advantages

-   **Easy on Use**: This easy to install and setup makes it outstanding from its other document-based NoSQL database. Plus, no more good coding background is required. You should know JSON to understand the tree structure of collection here.
-   **No Complex Joins**: MongoDB is based on BSON format – key-value pair, hence no complex joins here.
-   **Many Supported Platform**: MongoDB supports wide varieties of platforms: Windows, Ubuntu, Debian, Solaris, macOS. This makes it very popular among developers.
-   **Agility**: With quickly evolving needs of requirement, the flexible data model is needed to address all those. A fixed schema-based data model can’t address that all, schema-less data model MongoDB makes itself popular because of its ability to scale and its highly dynamic nature. It’s exceptionally easy to add or change fields in MongoDB.
-   **Faster Access to Data**: Due to its nature of using the storage’s internal memory, it provides fast access to the data.
-   **Horizontal Scaling**: It relies on horizontal and not on vertical scaling like RDBMS. The expense gets reduced, as no CPU and memory needs to be added on the same server. One can utilize more server with more processing requirements. This not only reduces cost expenses of CPU, memory addition but also reduces the maintenance cost.
    
    ![img](./resources/imgs/mongodb-scalability.png "Horizontal Scaling")


<a id="orgf57c870"></a>

# Course Structure

-   Introduction
-   Understanding the Basics & CRUD Operations
-   Schemas & Relations: How to structure Documetns
-   Exploring the Shell & The Server
-   Using the MongoDB Compass to Explore Data Visually
-   Diving Into Create Operations
-   Read Operations - A Closer Look
-   Update Operations
-   Understanding Delete Operations
-   Working with Indexes
-   Working with Geospatial Data
-   Understanding the Aggregation Framework
-   Working with Numeric Data
-   MongoDB & Security
-   Performance, Fault Tolerancy & Deployment
-   Transactions
-   From Shell to Driver
-   Introducing Stitch


<a id="org889c87e"></a>

# Useful Resources and Links

-   [Mongo Docs](https://docs.mongodb.com/manual/tutorial/getting-started/)

-   [MongoDB Drivers](https://docs.mongodb.com/ecosystem/drivers/)

-   [Schema Validation](https://docs.mongodb.com/manual/core/schema-validation/)

-   [Data Type Limits](https://docs.mongodb.com/manual/reference/limits/)

-   [Suported Types](https://docs.mongodb.com/manual/reference/bson-types/)

-   [Config Files](https://docs.mongodb.com/manual/reference/configuration-options/)

-   [Shell (mongo) Options](https://docs.mongodb.com/manual/reference/program/mongo/)

-   [Server (mongod) Options](https://docs.mongodb.com/manual/reference/program/mongod/)

-   [InsertOne() Method](https://docs.mongodb.com/manual/reference/method/db.collection.insertOne/)

-   [InsertMany() Method](https://docs.mongodb.com/manual/reference/method/db.collection.insertMany/)

-   [Atomicity](https://docs.mongodb.com/manual/core/write-operations-atomicity/#atomicity)

-   [Write Concern](https://docs.mongodb.com/manual/reference/write-concern/)

-   [Using mongoimport](https://docs.mongodb.com/manual/reference/program/mongoimport/index.html)

-   [find()](https://docs.mongodb.com/manual/reference/method/db.collection.find/)

-   [Cursors](https://docs.mongodb.com/manual/tutorial/iterate-a-cursor/)

-   [Query Operator Reference](https://docs.mongodb.com/manual/reference/operator/query/)

-   [Updating Documents](https://docs.mongodb.com/manual/tutorial/update-documents/)

-   [Deleting Documents ](https://docs.mongodb.com/manual/tutorial/remove-documents/)
-   [partialFilterExpressions](https://docs.mongodb.com/manual/core/index-partial/)

-   [Supported default<sub>languages</sub>](https://docs.mongodb.com/manual/reference/text-search-languages/#text-search-languages)

-   [Different Languages in the same index](https://docs.mongodb.com/manual/tutorial/specify-language-for-text-index/#create-a-text-index-for-a-collection-in-multiple-languages)

-   [GeoSpatial Documents](https://docs.mongodb.com/manual/geospatial-queries/)

-   [Geospatial Query Operators](https://docs.mongodb.com/manual/reference/operator/query-geospatial/)

-   [Aggregation Framework Documentation](https://docs.mongodb.com/manual/core/aggregation-pipeline/)

-   [$cond](https://docs.mongodb.com/manual/reference/operator/aggregation/cond/)

-   [Float vs Double vs Decimal - A Discussion on Precision](https://stackoverflow.com/questions/618535/difference-between-decimal-float-and-double-in-net)

-   [Number Ranges](https://social.msdn.microsoft.com/Forums/vstudio/en-US/d2f723c7-f00a-4600-945a-72da23cbc53d/can-anyone-explain-clearly-about-float-vs-decimal-vs-double-?forum=csharpgeneral)

-   [Modelling Number/ Monetary Data in MongoDB](https://docs.mongodb.com/manual/tutorial/model-monetary-data/)

-   [Official &ldquo;Encryption at Rest&rdquo; Docs](https://docs.mongodb.com/manual/core/security-encryption-at-rest/)

-   [Official Security Checklist](https://docs.mongodb.com/manual/administration/security-checklist/)

-   [What is SSL/ TLS?](https://www.acunetix.com/blog/articles/tls-security-what-is-tls-ssl-part-1/)

-   [Official MongoDB SSL Setup Docs](https://docs.mongodb.com/manual/tutorial/configure-ssl/)

-   [Official MongoDB Users & Auth Docs](https://docs.mongodb.com/manual/core/authentication/)

-   [Official Built-in Roles Docs](https://docs.mongodb.com/manual/core/security-built-in-roles/)

-   [Official Custom Roles Docs](https://docs.mongodb.com/manual/core/security-user-defined-roles/)

-   [Official Documentation on Replica Sets](https://docs.mongodb.com/manual/replication/)

-   [Official Documentation on Sharding](https://docs.mongodb.com/manual/sharding/)

-   [Transactions](https://docs.mongodb.com/manual/core/transactions/)

-   [Official Stitch Docs](https://docs.mongodb.com/stitch/)

-   [Complete Stitch Username + Password Auth Flow](https://docs.mongodb.com/stitch/authentication/userpass/)

-   [Stitch Services (AWS S3)](https://docs.mongodb.com/stitch/reference/partner-services/amazon-service/)
