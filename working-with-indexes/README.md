# Working With Indexes

<br/>

<p>
Indexes support the efficient execution of queries in MongoDB. Without indexes, MongoDB must perform a collection scan, i.e. scan every document in a collection, to select those documents that match the query statement. If an appropriate index exists for a query, MongoDB can use the index to limit the number of documents it must inspect.

Indexes are special data structures that store a small portion of the collection’s data set in an easy to traverse form. 
The index stores the value of a specific field or set of fields, ordered by the value of the field. 
The ordering of the index entries supports efficient equality matches and range-based query operations. 
In addition, MongoDB can return sorted results by using the ordering in the index.
</p>

<br/>

![working with indexes](https://docs.mongodb.com/manual/_images/index-for-sort.bakedsvg.svg "working with indexes")

<br/>

<p>
Fundamentally, indexes in MongoDB are similar to indexes in other database systems. 
MongoDB defines indexes at the collection level and supports indexes on any field or sub-field of the documents in a MongoDB collection.
</p>

<br/>

## Query Diagnosis and Query Planning

<br/>

### db.collection.explain()

<br/>
<p>
 Returns information on the query plan for the following methods:
</p>

* aggregate()
* count()
* find()
* remove()
* update()
* distinct()
* findAndModify()
* mapReduce()

<br/>

<p>
The db.collection.explain() method has the following parameter:
</p>

* verbosity - Optional. Specifies the verbosity mode for the explain output. The possible modes are:       
    * "queryPlanner" (Default)
    * "executionStats"
    * "allPlansExecution"

</p>

<br/>

```sh
> db.contacts.explain("executionStats").find({"dob.age": {$gt: 60}})
```

<br/>

## Single Find Indexes

<br/>

<p>
MongoDB provides complete support for indexes on any field in a collection of documents. By default, all collections have an index on the _id field, and applications and users may add additional indexes to support important queries and operations.

![Single Find Indexes](https://docs.mongodb.com/manual/_images/index-ascending.bakedsvg.svg)
</p>

<br/>

### createIndex()

<br/>

<p>
Creates indexes on collections.

db.collection.createIndex() takes the following parameters:
</p>

* keys - A document that contains the field and value pairs where the field is the index key and the value describes the type of index for that field. For an   ascending index on a field, specify a value of 1; for descending index, specify a value of -1.
* options - Optional. A document that contains a set of options that controls the creation of the index.
* commitQuorum - Optional. The minimum number of data-bearing voting replica set members (i.e. commit quorum), including the primary, that must report a successful index build before the primary marks the indexes as ready. A “voting” member is any replica set member where members[n].votes is greater than 0.


<br/>

```sh
> db.contacts.createIndex({"dob.age": 1})
```

<br/>

## dropIndex()

<br/>

<p>
Drops or removes the specified index from a collection. The db.collection.dropIndex() method provides a wrapper around the dropIndexes command.

The db.collection.dropIndex() method takes the following parameter:
</p>

* index - Optional. Specifies the index to drop. You can specify the index either by the index name or by the index specification document.


<br/>

```sh
> db.contacts.dropIndex({"dob.age": 1})
```

<br/>

## Compound Indexes

<br/>

<p>
MongoDB supports compound indexes, where a single index structure holds references to multiple fields within a collection’s documents. 
The following diagram illustrates an example of a compound index on two fields:

![Compound Indexes](https://docs.mongodb.com/manual/_images/index-compound-key.bakedsvg.svg)


Compound indexes can support queries that match on multiple fields.
</p>

<br/>

### Create a Compound Index

<br/>

```sh
> db.contacts.createIndex({"dob.age": 1, gender: 1})

> db.contacts.explain("executionStats").find({"dob.age": 35, gender: "male"})
```

<br/>

<p>
The value of the field in the index specification describes the kind of index for that field. 
For example, a value of 1 specifies an index that orders items in ascending order. 
A value of -1 specifies an index that orders items in descending order.
</p>

<br/>

### Sorting Indexes

<br/>

```sh
> db.contacts.explain().find({"dob.age": 35}).sort({gender: 1})
```

<br/>

### getIndexes()

<br/>

<p>
Returns an array that holds a list of documents that identify and describe the existing indexes on the collection, including hidden indexes. 
You must call db.collection.getIndexes() on a collection. 
</p>

<br/>

```sh
> db.contacts.getIndexes()
```

<br/>

## Index Properties

<br/>

### Unique Indexes

<br/>

<p>
The unique property for an index causes MongoDB to reject duplicate values for the indexed field. 
Other than the unique constraint, unique indexes are functionally interchangeable with other MongoDB indexes.
</p>

<br/>

```sh
> db.contacts.createIndex({email: 1}, {unique: true})
```

<br/>

### Partial Indexes

<br/>

<p>
Partial indexes only index the documents in a collection that meet a specified filter expression. 
By indexing a subset of the documents in a collection, partial indexes have lower storage requirements and reduced performance costs for index creation and maintenance.

Partial indexes offer a superset of the functionality of sparse indexes and should be preferred over sparse indexes.
</p>

<br/>

```sh
> db.contacts.createIndex({"dob.age": 1}, {partialFilterExpression: {gender: "male"}})

> db.contacts.explain().find({"dob.age": {$gt: 60}, gender: "male"})
```

<br/>

### Sparse Indexes

<br/>

<p>
The sparse property of an index ensures that the index only contain entries for documents that have the indexed field. The index skips documents that do not have the indexed field.

You can combine the sparse index option with the unique index option to prevent inserting documents that have duplicate values for the indexed field(s) and skip indexing documents that lack the indexed field(s).
</p>

<br/>

```sh
> db.users.insertMany(
    [
        {name: "user1", email: "user1@test.com"},
        {name: "user2"}
    ]
)

> db.users.createIndex({email: 1}, {unique: true})
```

<br/>

> This will throw an duplicate key error, because the null value is already used
```sh
> db.users.insertOne({name: "user3"})
```

<br/>

> To avoid this defaul behaviour
```sh
> db.users.dropIndex({email: 1}, {unique: true})

> db.users.createIndex({email: 1}, {unique: true, partialFilterExpression: {email: {$exists: true}}})
```

<br/>

> Now the insertion of an user without an email succeds without any errors 
```
> db.users.insertOne({name: "user3"})
```

<br/>

### TTL Indexes

<br/>

<p>
TTL indexes are special indexes that MongoDB can use to automatically remove documents from a collection after a certain amount of time. 
This is ideal for certain types of information like machine generated event data, logs, and session information that only need to persist in a database for a finite amount of time.
</p>


<br/>

> inserts s recored with the current Date
```sh
> db.sessions.insertOne({data: "randomData1", createdAt: new Date()})
```

<br/>

> create a index with the expireAfterSeconds to remove documents after a finite amount of time
```sh
> db.sessions.createIndex({createdAt: 1}, {expireAfterSeconds: 10})
```

<br/>

> this new record will be delete after 10 seconds
```sh
> db.sessions.insertOne({data: "randomData2", createdAt: new Date()})
```

<br/>

### Text Indexes

<p>
To create a text index, use the db.collection.createIndex() method. To index a field that contains a string or an array of string elements, include the field and specify the string literal "text" in the index document.
</p>

```sh
> db.products.insertMany(
    [
        {title: "product1", description: "Some random description"},
        {title: "product2", description: "Another random description"},
        {title: "product3", description: "Yet another random description"}
    ]
)

> db.products.find().pretty()

> db.products.createIndex({description: "text"})

> db.products.find({$text: {$search: "description"}}).pretty()

> db.products.find({$text: {$search: "\"another random description\""}}).pretty()
```

#### **Create Compound Text Indexes**

<br/>


<p>
You can index multiple fields for the text index. The following example creates a text index on the fields subject and comments.
</p>

<br/>


```sh
> db.products.dropIndex("description_text")

> db.products.createIndex({title: "text", description: "text"})

> db.products.insertOne({title: "item", description: "A specific item description"})

> db.products.find({$text: {$search: "item"}})
```

<br/>

#### **Text Indexes and sorting**

```sh
> db.products.find({$text: {$search: "random description"}}, {score: {$meta: "textScore"}}).pretty()


> db.products.find({$text: {$search: "random description"}}, {score: {$meta: "textScore"}}).sort({score: {$meta: "textScore"}}).pretty()
```

<br/>

#### **Using Text Indexes to exclude words** 
```sh
> db.products.insertMany(
    [
        {title: "product5", description: "a description"},
        {title: "product6", description: "a specific description"},
        {title: "product7", description: "a special description"}
    ]
)

> db.products.find({$text: {$search: "description -random"}}).pretty()
```

<br/>

#### **Seting default language and specifying Weights**

> db.products.dropIndex("title_text_description_text")

> db.products.createIndex({title: "text", description: "text"}, {default_language: "english", weights: {title: 1, description: 10}})

> db.products.find({$text: {$search: "specific"}}, {score: {$meta: "textScore"}}).pretty()

#### **Building Indexes**


> mongo credit-rating.js

> db.ratings.createIndex({age: 1}, {background: true})


<br/>

## Query Optimization

<br/>

### Covered Query

<br/>

<p>
A covered query is a query that can be satisfied entirely using an index and does not have to examine any documents. An index covers a query when all of the following apply:
</p>

* all the fields in the query are part of an index, **and**
* all the fields returned in the results are in the same index.
* no fields in the query are equal to **null** (i.e. {**"field"** : **null**} or {**"field"** : {**$eq** : **null**}} ).

<br/>

```sh
> db.customers.insertMany(
    [
        {name: "user1", age: 30, salary: 3000},
        {name: "user2", age: 40, salary: 5000}
    ]
)

> db.customers.createIndex({name: 1})
```

<br/>

> Returns all the fields that are also the indexed fields
```sh
> db.customers.explain("executionStats").find({name: "user1"}, {_id: 0, name: 1})
```

<br/>

```sh
> db.customers.getIndexes()

> db.customers.createIndexes({age: 1, name: 1})
```

<br/>

## Query Plans

<br/>

<p>
For a query, the MongoDB query optimizer chooses and caches the most efficient query plan given the available indexes. 
The evaluation of the most efficient query plan is based on the number of “work units” (works) performed by the query execution plan when the query planner evaluates candidate plans.

The associated plan cache entry is used for subsequent queries with the same query shape.
</p>

<br/>

### Query Plan and Cache Information

<br/>

<p>
To view the query plan information for a given query, you can use db.collection.explain() or the cursor.explain() .

Starting in MongoDB 4.2, you can use the $planCacheStats aggregation stage to view plan cache information for a collection.
Plan Cache Flushes
</p>

<br/>

<p>
The query plan cache does not persist if a mongod restarts or shuts down. In addition:
</p>

* Catalog operations like index or collection drops clear the plan cache.
* Least recently used (LRU) cache replacement mechanism clears the least recently accessed cache entry, regardless of state.

<br/>

<p>
Users can also:
</p>

* Manually clear the entire plan cache using the PlanCache.clear() method.
* Manually clear specific plan cache entries using the PlanCache.clearPlansByQuery() method.

<br/>

## Multi-Key Indexes

<br/>

<p>
To index a field that holds an array value, MongoDB creates an index key for each element in the array. These multikey indexes support efficient queries against array fields. Multikey indexes can be constructed over arrays that hold both scalar values [1] (e.g. strings, numbers) and nested documents.

![Single Find Indexes](https://docs.mongodb.com/manual/_images/index-multikey.bakedsvg.svg)
</p>

<br/>

> db.contacts.insertOne({name: "user1", hobbies: ["hobbie1", "hoobie2"], addresses: [{street: "street nowhere"}, {street: "street anywhere"}]})

> db.contacts.find().pretty()

> db.contacts.createIndex({hobbies: 1})

> db.contacts.find({hobbies: "hobbie1"}).pretty()

> db.contacts.explain("executionStats").find({hobbies: "hobbie1"})

> db.contacts.createIndex({addresses: 1})

> db.contacts.explain("executionStats").find({"addresses.street": "street nowhere"})

> db.contacts.explain("executionStats").find({addresses: {street: "street nowhere"}})

<br/>

### Create Multikey Index

<br/>

<p>
To create a multikey index, use the db.collection.createIndex() method.

MongoDB automatically creates a multikey index if any indexed field is an array; you do not need to explicitly specify the multikey type.
</p>

<br/>

### Index Bounds

<br/>

<p>
If an index is multikey, then computation of the index bounds follows special rules.
</p>

<br/>

### Unique Multikey Index

<br/>

<p>
For unique indexes, the unique constraint applies across separate documents in the collection rather than within a single document.

Because the unique constraint applies to separate documents, for a unique multikey index, a document may have array elements that result in repeating index key values as long as the index key values for that document do not duplicate those of another document.
</p>

<br/>

### Limitations

<br/>

<p>
For a compound multikey index, each indexed document can have at most one indexed field whose value is an array.
</p>

* You cannot create a compound multikey index if more than one to-be-indexed field of a document is an array. 
* Or, if a compound multikey index already exists, you cannot insert a document that would violate this restriction.
