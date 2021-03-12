- [Basics & Crud Operations](#orgbaf2e96)
  - [Check databases / Create a Database](#org3352473)
  - [Create a document](#org3351352)
  - [Find documents](#org5a07ac6)
  - [Update Documents](#org5fdd5e9)
  - [Replace Documents](#orgdd34708)
  - [Delete Documents](#orgd1d2f0e)
  - [Projections](#org5e21e3f)
  - [Working with nested documents](#orge861fe5)
  - [Working with arrays](#org0221a8f)
  - [Acessing structured data](#orgec7b842)
- [Schemas & Relations - How to Structure Documents](#org60dd7a6)
  - [Structuring documents](#orgdf1f004)
  - [Data Types](#org6ec604a)
  - [Database Stats](#orgfda85b9)
  - [Check the type of an value](#org9cda73c)
  - [Insert a document with a number with a default 64 bit value](#org98d8522)
  - [Insert a product with a number with an int32 value instead of 64 bit](#orgbfa9c7e)
  - [Important data type limits](#org68e34e6)
- [Exploring the Shell and the Server](#org95b8979)
- [Create Operations](#org7d97d15)
  - [insert() Methods](#org8815b37)
  - [writeConcern](#orgd281707)
  - [Atomicity](#org93476f7)
  - [Multi-Document Transactions](#org40e94b2)
  - [Importing Data](#org08c31ee)
- [Read Operations](#org12480dc)
- [Query & Projection Operators](#orgd14c0d9)
  - [Comparison Query Operators](#orgf27b97b)
  - [Understanding findOne() and find()](#org9ab06be)
  - [Working with Comparison Operators](#orgaa66bcc)
  - [Querying Embedded Fields & Arrays](#orgb761b00)
  - [Comparison Query Operators](#orgdf3aca2)
  - [Element Query Operators](#org38ac9e4)
  - [Evaluation Query Operators](#orge271961)
  - [Array Query Operators](#org518388e)
  - [Cursors](#org243fcd2)
  - [Projection Operators](#org48d18b3)
- [Update Operations](#org215eae6)
- [Update Operations](#org6685534)
  - [Update Operators](#orgeacb58b)
  - [Array Update Operators](#org3a26124)
- [Delete Operations](#orgb1f34be)
- [Delete Operations](#orgc73f2e5)
  - [deleteOne()](#orgd9b0737)
  - [deleteMany()](#org45804ef)
  - [drop()](#orge06b967)
  - [dropDatabase()](#org9c255a5)
- [Indexes](#org752a583)
- [Working With Indexes](#orgb2ba985)
  - [Query Diagnosis and Query Planning](#org357b240)
  - [Single Find Indexes](#org9664954)
  - [dropIndex()](#org5e37aa4)
  - [Compound Indexes](#org7e65ce0)
  - [Index Properties](#org8840bdd)
  - [Query Optimization](#org0c7b68a)
  - [Query Plans](#orga3732db)
  - [Multi-Key Indexes](#org7041c6f)
- [GeoSpatial Data](#org63a0289)
- [Working with GeoSpatial Data](#org30dcdbe)
  - [Creating GeoJSON Data](#org4456b20)
  - [Running Geo Queries](#org6528c3c)
  - [Adding additional locations](#org71830ff)
  - [Finding if a user inside a specific area](#orgd7bb040)
  - [Finding Places Within a Certain Radius](#orgcffa1ca)
- [Numeric Data](#org8943759)
- [Working With Numeric Data](#org9cba691)
  - [Integer](#org4122994)
  - [Doing Maths with Floats int32s & int64](#orgf4c21a3)
  - [Double](#org54742ca)
- [Aggregation Framework](#org84013b1)
  - [Pipeline Concept](#org620bef0)
  - [Group Stage](#org973d3d2)
  - [Project Stage](#org0298d0f)
  - [Turning the Location Into a geoJSON Object](#org507ab12)
  - [Transforming the Birthdate](#org53e86e1)
  - [Using Shortcuts for Transformations](#org1df846a)
  - [Understanding the $isoWeekYear Operator](#org3843f91)
  - [Pushing Elements Into Newly Created Arrays](#org11fa94d)
  - [unWind Stage](#orgab1f6af)
  - [addToSet Stage](#org9612695)
  - [slice operator](#orga78cd7a)
  - [$slice operator](#orgdcc0e25)
  - [$filter operator](#org55a0d42)
  - [Aplying Multiple Operations to an array](#org54ac867)
  - [$bucket Stage](#org640f58e)
  - [Aditional Stages](#orgc0b6e30)
  - [Writing Pipelines into a new collection](#org4eabc7e)
  - [\*geoNear Stage](#org38d8ef7)
- [MongoDB & Securtiy](#org032cda4)
  - [Starting mongo servers with authorization](#orgaa1a96a)
  - [Create a user](#org0429669)
  - [with admin privileges](#org86f81b7)
  - [with restricted privileges](#org4129140)
  - [Update a user](#orga61b784)
  - [Logging to the server with auth](#org5d0deb4)
  - [Adding SSL Transport Encryption](#org87b19a1)
- [Performance, Fault Tolerancy & Deployment](#org7b0313f)
- [Performance, Fault Tolerancy & Deployment](#orge413d80)
  - [Capped Collections](#org8a31447)
  - [Sharding](#org52d09bf)
  - [Using MongoDB Atlas](#org4a51c38)
- [Transactions](#orgc96303d)
- [Useful Resources and Links](#orgc19aba9)

---


<a id="orgbaf2e96"></a>

# Basics & Crud Operations

An overview of the basics of mongodb and crud operations


<a id="org3352473"></a>

## Check databases / Create a Database

```javascript
// Show all databases
>  show dbs

// Use the specified database
> use dbname
```


<a id="org3351352"></a>

## Create a document

```javascript
// Create a Document
> db.dbname.insertOne(
    {name: "product1", price: 12.99}
)

// Create a document with an embbeded document
> db.dbname.insertOne(
    {
        key1: "dummy",
        key2: 12.99,
        key3: "A dummy product",
        key4: {
            key4subkey1: "key4subval1",
            key4subkey1: key4subkey2
            }
    }
)

// Create various documents in a collection
> db.dbname.insertMany(
    [
        {
            key1: "value1",
            key2: "value2",
            key3: "value3",
            key4: "value4",
        },
        {
            key1: "value1",
            key2: "value2",
            key3: "value3",
            key4: "value4",
        }
    ]
)

```


<a id="org5a07ac6"></a>

## Find documents

find() doesn&rsquo;t return an array of all the documents in a collection, but a cursor pretty() can only be applied when the result of the operation applied returns a cursor while find() returns a cursor, findOne() returns a document, so it can only be applied to find().

```javascript
// Find all documents in a collection
> db.dbname.find()

// Find all documents in a collection in a prettified form
> db.dbname.find().pretty()

// Find all documents in a collection and convert it to array (This operation returns every document in a collection, despite the size)
> db.dbname.find().toArray()

// Find all documents in a collection and apply certain operations on each one
> db.dbname.find().forEach((colData) => {printjson(colData)})

// Find all documents in a collection that match a specific key/value
> db.dbname.find({key: value})

// Find all documents in a collection that are greater than the specificied key/value
> db.dbname.find({key: {$gt: value}})

// Find one document in a collection that match a specific key/value
> db.dbname.findOne({key: value})

// Find all documents in a collection that is greater than the specific key/value
> db.dbname.findOne({key: {$gt: value}})
```


<a id="org5fdd5e9"></a>

## Update Documents

```javascript
// Update one document
> db.dbname.updateOne({key: "value"}, {$set: {key: "updtValue"}})

// Update many documents
> db.dbname.updateMany({}, {$set: {key: "updtValue"}})

// Update a document replacing all key/value pairs
> db.dbname.update({key: "value"}, {{key: "updtValue"}})i
```


<a id="orgdd34708"></a>

## Replace Documents

```javascript
// Replace a document
> db.dbname.replaceOne(  {
    key1: "value1",
    key2: "value2",
    key3: "value3",
    key4: "value4",
  })
```


<a id="orgd1d2f0e"></a>

## Delete Documents

```javascript
// Delete one document
> db.dbname.deleteOne({key: "value"})

// Delete all documents that have common key/pair values
> db.dbname.deleteMany({commonKey: "commonValue"})

// Delete many documents
> db.dbname.deleteMany({})
```


<a id="org5e21e3f"></a>

## Projections

```javascript
// Using projections to return all the documents with the choosen properties
> db.dbname.find({}, {key: value}).pretty()

// Using projections to return all the documents in the collection with the choosen properties, excluding some
> db.dbname.find({}, {key: value, _key: value}).pretty()
```


<a id="orge861fe5"></a>

## Working with nested documents

```javascript
// Update a document with a nested document
> db.dbname.updateMany({}, {$set: {key: {nestedKey1: "nestedValue1", nestedKey2: "nestedValue2"}}})

// Update a document with a nested document
> db.dbname.updateMany({}, {$set: {key: {nKey1: "nVal1", nKey2: "nVal2", nK3: {nk3a: "nk3aVal"}}}})
```


<a id="org0221a8f"></a>

## Working with arrays

```javascript
> db.dbname.updateOne({key: "val"}, {$set: {newKey: ["val1", "val2"]}})
```


<a id="orgec7b842"></a>

## Acessing structured data

```javascript
// Acessing a specific field of a document
 > db.dbname.findOne({key: "val"}).key2

// Searching for documents with a specific field
 > db.dbname.find({key: "value"}).pretty()

// Searching for documents that have a field with a specific subfield and value present
 > db.dbname.find({"key.subkey": "subkeyVal"}).pretty()

// Searching for documents that have a field that has a subfield with a specific subfield and value present
 > db.dbname.find({"key.subkey.subsubkey": "subsubkeyVal"}).pretty()
```

---


<a id="org60dd7a6"></a>

# Schemas & Relations - How to Structure Documents


<a id="orgdf1f004"></a>

## Structuring documents

```javascript
 > db.colname.insertMany([
  {
    name: "Dummy1",
    price: 12.99,
    details: null
  },
  {
    name: "Dummy1",
    price: 1234,
    details: null
  },
  {
    name: "Dummy3",
    price: 29.99,
    details: {name: "nestedDummy"}
  }
])
```


<a id="org6ec604a"></a>

## Data Types

```javascript
> db.colname.insertOne(
    {
      name: "Dummy Corp Inc",
      isStartup: true,
      employees: 35,
      funding: 01234567899876543210,
      details: {ceo: "Dummy CEO"},
      tags: [{title: "test1"}, {title: "teste2"}],
      foundingDate: new Date(),
      insertedAt: new Timestamp()
    }
)
```


<a id="orgfda85b9"></a>

## Database Stats

```javascript
> db.stats()
```


<a id="org9cda73c"></a>

## Check the type of an value

```javascript
> typeof db.colname.findOne().key
```


<a id="org98d8522"></a>

## Insert a document with a number with a default 64 bit value

```javascript
> db.colname.insertOne({key: 1})
```


<a id="orgbfa9c7e"></a>

## Insert a product with a number with an int32 value instead of 64 bit

```javascript
> db.colname.insertOne({key: NumberInt(1)})
```


<a id="org68e34e6"></a>

## Important data type limits

-   **Normal integers (int32)** can hold a maximum value of +-2,147,483,647

-   **Long integers (int64)** can hold a maximum value of +-9,223,372,036,854,775,807

-   **Text** can be as long as you want - the limit is the 16mb restriction for the overall document

It&rsquo;s also important to understand the difference between int32 (NumberInt), int64 (NumberLong) and a normal number as you can enter it in the shell. The same goes for a normal double and NumberDecimal.

-   **NumberInt** creates a int32 value => NumberInt(55)

-   **NumberLong** creates a int64 value => NumberLong(7489729384792)

If you just use a number (e.g. insertOne({a: 1}), this will get added as a normal double into the database. The reason for this is that the shell is based on JS which only knows float/ double values and doesn&rsquo;t differ between integers and floats.

-   **NumberDecimal** creates a high-precision double value => NumberDecimal(&ldquo;12.99&rdquo;) => This can be helpful for cases where you need (many) exact decimal places for calculations.

When not working with the shell but a MongoDB driver for your app programming language (e.g. PHP, .NET, Node.js, &#x2026;), you can use the driver to create these specific numbers.


<a id="org95b8979"></a>

# Exploring the Shell and the Server


<a id="org7d97d15"></a>

# Create Operations


<a id="org8815b37"></a>

## insert() Methods

```javascript

```


### unordered inserts

```javascript
> use contactData

> db.persons.insertOne(
    {
        name: "dummy",
        age: 30,
        hobbies: ["Sports", "Cooking"]
    }
)

> db.persons.insertOne(
    {
        name: "dummy2",
        age: 35,
        hobbies: ["Music", "Cinema"]
    }
)

> db.persons.insertMany(
    [
        {
            name: "dummy4",
            age: 18,
        },
                {
            name: "dummy4",
            age: 39,
        },
    ]
)

> db.persons.insert(
        {
            name: "dummy5",
            age: 18,
        }
)
```


### ordered inserts

```javascript
> db.hobbies.insertMany(
    [
        {_id: "sports", name: "Sports"},
        {_id: "cooking", name: "Cooking"},
        {_id: "cars", name: "Cars"},
    ]
)
```

As it is the default behaviour of mongoDB, if we try to insert a value already added before, it will cancel the operation and output an error, but since mongoDB by defaul treats each operation as standalone, it will insert the first value and cancel the others inserts when it gets to the second element.

```javascript
> db.hobbies.insertMany(
    [
        {_id: "astronomy", name: "astronomy"},
        {_id: "cooking", name: "Cooking"},
        {_id: "hiking", name: "Hiking"},
    ]
)
```

To override this behaviour, we can pass a second argument to insertMany(). This second argument is a document used to configure this operation. In this case, it will fail the first two values and insert the last two values.

```javascript
db.hobbies.insertMany(
    [
        {_id: "sports", name: "Sports"},
        {_id: "cooking", name: "Cooking"},
        {_id: "astronomy", name: "astronomy"},
        {_id: "hiking", name: "Hiking"},
    ],
    {ordered: false}
)
```


<a id="orgd281707"></a>

## writeConcern

Write concern describes the level of acknowledgment requested from MongoDB for write operations to a standalone mongod or to replica sets or to sharded clusters. In sharded clusters, mongos instances will pass the write concern on to the shards.

Starting in MongoDB 4.4, replica sets and sharded clusters support setting a global default write concern. Operations which do not specify an explicit write concern inherit the global default write concern settings.

```javascript
// w can be set to either 1 or 0, for server acknowledgment.
> db.persons.insertOne(
    {
        name: "dummy6",
        age: 38,
    },
    {
        writeConcern: {w: 1}
    }
)

// j can be set to either true or false, to trigger jornaling of the entry
> db.persons.insertOne(
    {
        name: "dummy7",
        age: 38,
    },
    {
        writeConcern: {w: 1, j: true}
    }
)

// wtimeout can be set to the timeout of the write operation
> db.persons.insertOne(
    {
        name: "dummy7",
        age: 38,
    },
    {
        writeConcern: {w: 1, j: true, wtimeout: 2000}
    }
)
```


<a id="org93476f7"></a>

## Atomicity

In MongoDB, a write operation is atomic on the level of a single document, even if the operation modifies multiple embedded documents within a single document.


<a id="org40e94b2"></a>

## Multi-Document Transactions

When a single write operation (e.g. db.collection.updateMany()) modifies multiple documents, the modification of each document is atomic, but the operation as a whole is not atomic.

When performing multi-document write operations, whether through a single write operation or multiple write operations, other operations may interleave.


<a id="org08c31ee"></a>

## Importing Data

in the terminal shell

```shell
mongoimport tv-shows.json -d movieData -c movies --jsonArray --drop
```


<a id="org12480dc"></a>

# Read Operations


<a id="orgd14c0d9"></a>

# Query & Projection Operators


<a id="orgf27b97b"></a>

## Comparison Query Operators

-   Purpose - Locate Data
-   Types - Comparison, Evalution, Logical, Array, Element, Comments, Geospatial


<a id="org9ab06be"></a>

## Understanding findOne() and find()


#### findOne()

returns the first document that meets the criteria

```javascrip
> db.movies.findOne()
```

using a filter

```javascrip
> db.movies.findOne({name: "The Last Ship"})
```


#### find()

find() returns a cursor

```javascrip
> db.movies.find()
```

using a filter

```javascrip
> db.movies.find({name: "The Last Ship"})
> db.movies.find({runtime: 60})
```


<a id="orgaa66bcc"></a>

## Working with Comparison Operators

finding values that are equal to the specified value

```javascrip
> db.movies.find({runtime: {$eq: 70}})
```

finding values that aren&rsquo;t equal to the specified value

```javascrip
> db.movies.find({runtime: {$ne: 70}})
```

finding values that are lower than the specified value

```javascrip
> db.movies.find({runtime: {$lt: 40}})
```

finding values that are greater than the specified value

```javascrip
> db.movies.find({runtime: {$gt: 40}})
```

finding values that are lower or equal than the specified value

```javascrip
> db.movies.find({runtime: {$lte: 40}})
```

finding values that are greater or equal than the specified value

```javascrip
> db.movies.find({runtime: {$gte: 40}})
```


<a id="orgb761b00"></a>

## Querying Embedded Fields & Arrays

querying all documents that have the field average embedded in the rating field

```javascrip
> db.movies.find({"rating.average": {$gt: 7.0}})
```

querying all documents that have the genre drama/Drama value in the field genre which is an array

```javascrip
> db.movies.find({genres: "drama"})
> db.movies.find({genres: "Drama"})
```

querying all documents that have and genre array equaling Drama

```javascrip
> db.movies.find({genres: ["Drama"]}).pretty()
```


### Understanding &ldquo;$in&rdquo; and &ldquo;$nin&rdquo;

The $in operator selects the documents where the value of a field equals any value in the specified array.

```javascrip
> db.movies.find({runtime: {$in: [30, 42]}})
```

$nin selects the documents where:

-   the field value is not in the specified array or
-   the field does not exist.

```javascrip
> db.movies.find({runtime: {$nin: [30, 42]}})
```


<a id="orgdf3aca2"></a>

## Comparison Query Operators


### &ldquo;$or&rdquo; and &ldquo;$nor&rdquo;


#### &ldquo;$or&rdquo;

The $or operator performs a logical OR operation on an array of two or more <expressions> and selects the documents that satisfy at least one of the <expressions>.

```javascrip
> db.movies.find(
    {$or: [
        {"rating.average": {$lt: 5}},
        {"rating.average": {$gt: 9.3}}
          ]
    }
)
```


#### &ldquo;$nor&rdquo;

$nor performs a logical NOR operation on an array of one or more query expression and selects the documents that fail all the query expressions in the array.

```javascrip
> db.movies.find(
    {$nor: [
        {"rating.average": {$lt: 5}},
        {"rating.average": {$gt: 9.3}}
          ]
    }
)
```


### &ldquo;$and&rdquo;

$and performs a logical AND operation on an array of one or more expressions (e.g. <expression1>, <expression2>, etc.) and selects the documents that satisfy all the expressions in the array. The $and operator uses short-circuit evaluation. If the first expression (e.g. <expression1>) evaluates to false, MongoDB will not evaluate the remaining expressions.

```javascrip
> db.movies.find(
    {$and: [
        {"rating.average": {$gt: 9}},
        {genres: "Drama"}
          ]
    }

```


### &ldquo;$not&rdquo;

$not performs a logical NOT operation on the specified <operator-expression> and selects the documents that do not match the <operator-expression>. This includes documents that do not contain the field.

```javascrip
> db.movies.find(
    {
        runtime: {$not: {$eq: 60}}
    }
)
```


<a id="org38ac9e4"></a>

## Element Query Operators


### &ldquo;$exists&rdquo;

When <boolean> is true, $exists matches the documents that contain the field, including documents where the field value is null. If <boolean> is false, the query returns only the documents that do not contain the field.

```javascrip
> use user

> db.users.insertMany(
    [
        {
            name: "dummy",
            hobbies:
                [
                    {title: "Sports", frequency: 3},
                    {title: "Cooking", frequency: 5}
                ],
            phone: 985438732
        },
                {
            name: "dummy2",
            hobbies:
                [
                    {title: "Music", frequency: 9},
                    {title: "Cinema", frequency: 5}
                ],
            phone: "985543732",
            age: 30
        }
    ]
)

> db.users.find({age: {$exists: true}}).pretty()

> db.users.find({age: {$exists: true, $gte: 30}}).pretty()

> db. users.insertOne(
            {
            name: "dummy3",
            hobbies:
                [
                    {title: "Yoga", frequency: 5},
                    {title: "Dancing", frequency: 2}
                ],
            phone: 981234732,
            age: null
        },
)

> db.users.find({age: {$exists: true, $ne: null}})

```


### &ldquo;$type&rdquo;

$type selects documents where the value of the field is an instance of the specified BSON type(s). Querying by data type is useful when dealing with highly unstructured data where data types are not predictable.

```javascrip
> db.users.find({phone: {$type: "number"}})

> db.users.find({phone: {$type: "string"}})

> db.users.find({phone: {$type: ["double", "string"]}})
```


<a id="orge271961"></a>

## Evaluation Query Operators


### &ldquo;$regex&rdquo;

Provides regular expression capabilities for pattern matching strings in queries. MongoDB uses Perl compatible regular expressions (i.e. “PCRE” ) version 8.42 with UTF-8 support.

In MongoDB, you can also use regular expression objects (i.e. *pattern*) to specify regular expressions.

```javascrip
> db.movies.find({summary: {$regex: /musical/}})
```


### &ldquo;$expr&rdquo;

Allows the use of aggregation expressions within the query language. The arguments can be any valid aggregation expression.

```javascrip
//find all documents where the field volume is bigger than the field target
> use financialData

> db.sales.insertMany(
    [
        {volume: 100, target: 120},
        {volume: 89, target: 80},
        {volume: 200, target: 177}
    ]
)

> db.sales.find({$expr: {$gt: ["$volume", "$target"]}}).pretty()
```

```javascrip
find all documents that meet the condition
> db.sales.find(
    {
  $expr: {
    $gt: [
      {
        $cond: {
          if: { $gte: [$volume, 190] },
          then: { $subtract: [$volume, 10] },
          else: $volume
        }
      },
      $target
    ]
  }
}
)
```


<a id="org518388e"></a>

## Array Query Operators


### &ldquo;$size&rdquo;

The $size operator matches any array with the number of elements specified by the argument.

$size does not accept ranges of values. To select documents based on fields with different numbers of elements, create a counter field that you increment when you add elements to a field.

```javascrip
> db.users.find({hobbies: {$size: 3}})
```


### &ldquo;$all&rdquo;

The $all operator selects the documents where the value of a field is an array that contains all the specified elements.

```javascrip
> db.movieStats.find({genre: {$all: ["action", "thriller"]}})
```


### &ldquo;$elemMatch&rdquo;

The $elemMatch operator matches documents that contain an array field with at least one element that matches all the specified query criteria. If you specify only a single <query> condition in the $elemMatch expression, and are not using the $not or $ne operators inside of $elemMatch, $elemMatch can be omitted. See Single Query Condition.

```javascrip
> db.users.find({hobbies: {$elemMatch: {title: "Sports", frequency: {$gte: 3}}}}).pretty()
```


<a id="org243fcd2"></a>

## Cursors


### next()

Returns the next document in a cursor.

```javascrip
> use movieData

> const dataCursor = db.movies.find()

> dataCursor.next()
```


### hasNext()

Returns true if the cursor has documents and can be iterated.

```javascrip
> dataCursor.hasNext()
```


### forEach()

Applies a JavaScript function for every document in a cursor.

```javascrip
> dataCursor.forEach(doc => {
    printjson(doc)
})
```


### sort()

Returns results ordered according to a sort specification.

lower to higher: 1

higher to lower: -1

```javascrip
> db.movies.find().sort({"ratings.average": 1}).pretty()

> db.movies.find().sort({"ratings.average": 1, runtime: -1}).pretty()
```


### skip()

Returns a cursor that begins returning results only after passing or skipping a number of documents.

```javascrip
> db.movies.find().sort({"ratings.average": 1, runtime: -1}).skip(10).pretty()
```


### limit()

Constrains the size of a cursor’s result set.

```javascrip
> db.movies.find().sort({"ratings.average": 1, runtime: -1}).skip(10).limit(10).pretty()
```


<a id="org48d18b3"></a>

## Projection Operators


### $ (projection)

The positional $ operator limits the contents of an <array> to return either:

-   The first element that matches the query condition on the array.
-   The first element if no query condition is specified for the array (Starting in MongoDB 4.4).

Use $ in the projection document of the find() method or the findOne() method when you only need one particular array element in selected documents.

```javascrip
> db.movies.find({}, {name: 1, genres: 1, runtime: 1, rating: 1}).pretty()

> db.movies.find({genres: "Drama"}, {"genres.$": 1}).pretty()

> db.movies.find({genres: {$all: ["Drama", "Horror"]}}, {"genres.$": 1})
```


### $elemMatch (projection)

The $elemMatch operator limits the contents of an <array> field from the query results to contain only the first element matching the $elemMatch condition

```javascrip
> db.movies.find({genres: "Drama"}, {genres: {$elemMatch: {$eq: "Horror"}}}).pretty()
```


### $slice (projection)

The $slice projection operator specifies the number of elements in an array to return in the query result.

$slice: <number>

-   Specify a positive number n to return the first n elements.
-   Specify a negative number n to return the last n elements.

```javascrip
> db.movies.find({"rating.average": {$gt: 9}}, {genres: {$slice: 2}, name: 1})
```

$slice: [ < number to skip >, < number to return > ] Specifies the number of elements to return in the <arrayField> after skipping the specified number of elements starting from the first element. You must specify both elements.

```javascrip
> db.movies.find({"rating.average": {$gt: 9}}, {genres: {$slice: [1, 2]}, name: 1})
```


<a id="org215eae6"></a>

# Update Operations


<a id="org6685534"></a>

# Update Operations


<a id="orgeacb58b"></a>

## Update Operators


### \\$set

The $set operator replaces the value of a field with the specified value.

If the field does not exist, $set will add a new field with the specified value, provided that the new field does not violate a type constraint. If you specify a dotted path for a non-existent field, $set will create the embedded documents as needed to fulfill the dotted path to the field.

```javascript
// Updating one field in one document
> db.users.updateOne({_id: ObjectId("5f765f11224c6744c2b0a946")}, {$set: {hobbies: [{title: "Sports", frequency: 5}, {title: "Music", frequency: 7}, {title: "Football", frequency: 2}]}})

// Updating one field in many documents
> db.users.updateMany({"hobbies.title": "Sports"}, {$set: {isSporty: true}})

// Updating multiple fields in one document
> db.users.updateOne({_id: ObjectId("5f765f11224c6744c2b0a946")}, {$set: {age: 42, phone: 985348777}})
```


### \\$inc

The $inc operator increments a field by a specified value.

The $inc operator accepts positive and negative values. If the field does not exist, $inc creates the field and sets the field to the specified value. Use of the $inc operator on a field with a null value will generate an error. $inc is an atomic operation within a single document.

```javascript
> db.users.updateOne({name: "dummy2"}, {$inc: {age: 2}})
```


### \\$min

The $min updates the value of the field to a specified value if the specified value is less than the current value of the field. The $min operator can compare values of different types, using the BSON comparison order.

If the field does not exist, the \\$min operator sets the field to the specified value.

```javascript
> db.users.updateOne({name: "dummy5"}, {$min: {age: 40}})
```


### \\$max

The $max operator updates the value of the field to a specified value if the specified value is greater than the current value of the field. The $max operator can compare values of different types, using the BSON comparison order.

If the field does not exist, the \\$max operator sets the field to the specified value.

```javascript
> db.users.updateOne({name: "dummy2"}, {$max: {age: 40}})
```


### \\$mull

Multiply the value of a field by a number.

If the field does not exist in a document, \\$mul creates the field and sets the value to zero of the same numeric type as the multiplier.

```javascript
> db.users.updateOne({name: "dummy2"}, {$mul: {age: 2}})
```


### \\$unset

The $unset operator deletes a particular field.

If the field does not exist, then \\$unset does nothing (i.e. no operation).

When used with $ to match an array element, $unset replaces the matching element with null rather than removing the matching element from the array. This behavior keeps consistent the array size and element positions.

```javascript
> db.users.updateMany({isSporty: true}, {$unset: {phone: ""}})
```


### \\$rename

The $rename operator updates the name of a field.

The $rename operator logically performs an $unset of both the old name and the new name, and then performs a \\$set operation with the new name. As such, the operation may not preserve the order of the fields in the document; i.e. the renamed field may move within the document.

If the document already has a field with the <newName>, the \\$rename operator removes that field and renames the specified <field> to <newName>.

If the field to rename does not exist in a document, \\$rename does nothing (i.e. no operation).

For fields in embedded documents, the $rename operator can rename these fields as well as move the fields in and out of embedded documents. $rename does not work if these fields are in array elements.

```javascript
> db.users.updateMany({}, {$rename: {age: "totalAge"}})
```


### upsert()

If updateOne(), updateMany(), or replaceOne() includes upsert : true and no documents match the specified filter, then the operation creates a new document and inserts it. If there are matching documents, then the operation modifies or replaces the matching document or documents.

```javascript
> db.users.updateOne({name: "dummy4"}, {$set: {age: 29, hobbies: [{title: "Nice dinning", frequency: 3}], isSporty: true}}, {upsert: true})
```


<a id="org3a26124"></a>

## Array Update Operators


### \\$

The positional $ operator identifies an element in an array to update without explicitly specifying the position of the element in the array.

-   To project, or return, an array element from a read operation, see the $ projection operator instead.
-   To update all elements in an array, see the all positional operator $[] instead.
-   To update all elements that match an array filter condition or conditions, see the filtered positional operator instead $[<identifier>].

```javascript
> db.users.updateMany({hobbies: {$elemMatch: {title: "Sports", frequency: {$gte: 3}}}}, {$set: {"hobbies.$.highFrequency": true}})
```


### \\$[]

The all positional operator $[] indicates that the update operator should modify all elements in the specified array field.

If an upsert operation results in an insert, the query must include an exact equality match on the array field in order to use the \\$[] positional operator in the update statement.

```javascript
> db.users.updateMany({totalAge: {$gt: 30}}, {$inc: {"hobbies.$[].frequency": -1}})
```


### \\$[<identifier>]

The filtered positional operator $[<identifier>] identifies the array elements that match the arrayFilters conditions for an update operation, e.g. db.collection.update() and db.collection.findAndModify().

```javascript
> db.users.updateMany({"hobbies.frequency": {$gt: 2}}, {$set: {"hobbies.$[el].highFrequency": false}}, {arrayFilters: [{"el.frequency": {$gt: 2}} ]})
```


### \\$push

The $push operator appends a specified value to an array.

f the field is absent in the document to update, \\$push adds the array field with the value as its element.

If the field is not an array, the operation will fail.

If the value is an array, $push appends the whole array as a single element. To add each element of the value separately, use the $each modifier with \\$push. For an example, see Append Multiple Values to an Array.

```javascript
> db.users.updateOne({name: "dummy4"}, {$push: {hobbies: {title: "Nice dinning", frequency: 2}}})
```


### \\$each

The $each modifier is available for use with the $addToSet operator and the $push operator.

Use with the \\$addToSet operator to add multiple values to an array <field> if the values do not exist in the <field>.

Use with the \\$push operator to append multiple values to an array <field>.

The $push operator can use $each modifier with other modifiers.

```javascript
> db.users.updateOne({name: "dummy4"}, {$push: {hobbies: {$each: [{title: "nice dinning", frequency: 2}, {title: "Sports", frequency: 2}]}}})
```


### \\$sort

The $sort modifier orders the elements of an array during a $push operation.

To use the $sort modifier, it must appear with the $each modifier. You can pass an empty array [] to the $each modifier such that only the $sort modifier has an effect.

The $sort modifier can sort array elements that are not documents. In previous versions, the $sort modifier required the array elements be documents.

If the array elements are documents, the modifier can sort by either the whole document or by a specific field in the documents. In previous versions, the \\$sort modifier can only sort by a specific field in the documents.

Trying to use the $sort modifier without the $each modifier results in an error. The $sort no longer requires the $slice modifier.

```javascript
> db.users.updateOne({name: "dummy3"}, {$push: {hobbies: {$each: [{title: "Yoga", frequency: 2}, {title: "Dancing", frequency: 2}], $sort: {frequency: -1}}}})
```


### \\$slice

The $slice modifier limits the number of array elements during a $push operation. To project, or return, a specified number of array elements from a read operation, see the $slice projection operator instead.

To use the $slice modifier, it must appear with the $each modifier. You can pass an empty array [] to the $each modifier such that only the $slice modifier has an effect.

The order in which the modifiers appear is immaterial. Previous versions required the $each modifier to appear as the first modifier if used in conjunction with $slice. For a list of modifiers available for \\$push, see Modifiers.

Trying to use the $slice modifier without the $each modifier results in an error.

```javascript
> db.users.updateOne({name: "dummy3"}, {$push: {hobbies: {$each: [{title: "Yoga", frequency: 2}, {title: "Dancing", frequency: 2}], $sort: {frequency: -1}, $slice: 1}}})
```


### \\$pull

The $pull operator removes from an existing array all instances of a value or values that match a specified condition.

If you specify a <condition> and the array elements are embedded documents, \\$pull operator applies the <condition> as if each array element were a document in a collection. See Remove Items from an Array of Documents for an example.

If the specified <value> to remove is an array, \\$pull removes only the elements in the array that match the specified <value> exactly, including order.

If the specified <value> to remove is a document, \\$pull removes only the elements in the array that have the exact same fields and values. The ordering of the fields can differ.

```javascript
> db.users.updateOne({name: "dummy5"}, {$pull: {hobbies: {title: "Music"}}})
```


### \\$sort

The $pop operator removes the first or last element of an array. Pass $pop a value of -1 to remove the first element of an array and 1 to remove the last element in an array.

The \\$pop operation fails if the <field> is not an array.

If the \\$pop operator removes the last item in the <field>, the <field> will then hold an empty array.

```javascript
> db.users.updateOne({name: "dummy4"}, {$pop: {hobbies: 1}})
```


### $addToSet

The $addToSet operator adds a value to an array unless the value is already present, in which case $addToSet does nothing to that array.

```javascript
> db.users.updateOne({name: "dummy4"}, {$addToSet: {hobbies: {title: "Hiking", frequency: 2}}})
```


<a id="orgb1f34be"></a>

# Delete Operations


<a id="orgc73f2e5"></a>

# Delete Operations


<a id="orgd9b0737"></a>

## deleteOne()

Delete Only One Document that Matches a Condition.

To delete at most a single document that matches a specified filter (even though multiple documents may match the specified filter) use the db.collection.deleteOne() method.

```javascript
> db.users.deleteOne({name: "Chris"})
```


<a id="org45804ef"></a>

## deleteMany()

Removes all documents that match the filter from a collection.

To delete all documents from a collection, pass an empty filter document {} to the db.collection.deleteMany() method.

```javascript
> db.users.deleteMany({})
```

You can specify criteria, or filters, that identify the documents to delete. The filters use the same syntax as read operations.

```javascript
> db.users.deleteMany({totalAge: {$exists: false}, isSporty: true})
```


<a id="orge06b967"></a>

## drop()

Removes a collection or view from the database. The method also removes any indexes associated with the dropped collection. The method provides a wrapper around the drop command.

Should only be done by sysAdmins

> Drops the collection \#+END<sub>SRC</sub> > db.users.drop() \#+END<sub>SRC</sub>


<a id="org9c255a5"></a>

## dropDatabase()

Removes the current database, deleting the associated data files.

> Drops the database \#+END<sub>SRC</sub> > db.dropDatabase() \#+END<sub>SRC</sub>


<a id="org752a583"></a>

# Indexes


<a id="orgb2ba985"></a>

# Working With Indexes

Indexes support the efficient execution of queries in MongoDB. Without indexes, MongoDB must perform a collection scan, i.e. scan every document in a collection, to select those documents that match the query statement. If an appropriate index exists for a query, MongoDB can use the index to limit the number of documents it must inspect.

Indexes are special data structures that store a small portion of the collection’s data set in an easy to traverse form. The index stores the value of a specific field or set of fields, ordered by the value of the field. The ordering of the index entries supports efficient equality matches and range-based query operations. In addition, MongoDB can return sorted results by using the ordering in the index.

![img](https://docs.mongodb.com/manual/_images/index-for-sort.bakedsvg.svg "Example of working with indexes")

Fundamentally, indexes in MongoDB are similar to indexes in other database systems. MongoDB defines indexes at the collection level and supports indexes on any field or sub-field of the documents in a MongoDB collection.


<a id="org357b240"></a>

## Query Diagnosis and Query Planning


### db.collection.explain()

Returns information on the query plan for the following methods:

-   aggregate()
-   count()
-   find()
-   remove()
-   update()
-   distinct()
-   findAndModify()
-   mapReduce()

The db.collection.explain() method has the following parameter:

-   verbosity - Optional. Specifies the verbosity mode for the explain output. The possible modes are:
    -   &ldquo;queryPlanner&rdquo; (Default)
    -   &ldquo;executionStats&rdquo;
    -   &ldquo;allPlansExecution&rdquo;

```javascript
> db.contacts.explain("executionStats").find({"dob.age": {$gt: 60}})
```


<a id="org9664954"></a>

## Single Find Indexes

MongoDB provides complete support for indexes on any field in a collection of documents. By default, all collections have an index on the \_id field, and applications and users may add additional indexes to support important queries and operations.

![img](https://docs.mongodb.com/manual/_images/index-ascending.bakedsvg.svg "Single Find Index Example")


### createIndex()

Creates indexes on collections.

db.collection.createIndex() takes the following parameters:

-   keys - A document that contains the field and value pairs where the field is the index key and the value describes the type of index for that field. For an ascending index on a field, specify a value of 1; for descending index, specify a value of -1.
-   options - Optional. A document that contains a set of options that controls the creation of the index.
-   commitQuorum - Optional. The minimum number of data-bearing voting replica set members (i.e. commit quorum), including the primary, that must report a successful index build before the primary marks the indexes as ready. A “voting” member is any replica set member where members[n].votes is greater than 0.

```javascript
> db.contacts.createIndex({"dob.age": 1})
```


<a id="org5e37aa4"></a>

## dropIndex()

Drops or removes the specified index from a collection. The db.collection.dropIndex() method provides a wrapper around the dropIndexes command.

The db.collection.dropIndex() method takes the following parameter:

-   index - Optional. Specifies the index to drop. You can specify the index either by the index name or by the index specification document.

```javascript
> db.contacts.dropIndex({"dob.age": 1})
```


<a id="org7e65ce0"></a>

## Compound Indexes

MongoDB supports compound indexes, where a single index structure holds references to multiple fields within a collection’s documents. The following diagram illustrates an example of a compound index on two fields:

\![Compound Indexes](![img](https://docs.mongodb.com/manual/_images/index-compound-key.bakedsvg.svg))

[(![img](https://docs.mongodb.com/manual/_images/index-compound-key.bakedsvg.svg "This is the caption for the next figure link (or table)")[]]

Compound indexes can support queries that match on multiple fields.


### Create a Compound Index

```javascript
> db.contacts.createIndex({"dob.age": 1, gender: 1})

> db.contacts.explain("executionStats").find({"dob.age": 35, gender: "male"})
```

The value of the field in the index specification describes the kind of index for that field. For example, a value of 1 specifies an index that orders items in ascending order. A value of -1 specifies an index that orders items in descending order.


### Sorting Indexes

```javascript
> db.contacts.explain().find({"dob.age": 35}).sort({gender: 1})
```


### getIndexes()

Returns an array that holds a list of documents that identify and describe the existing indexes on the collection, including hidden indexes. You must call db.collection.getIndexes() on a collection.

```javascript
> db.contacts.getIndexes()
```


<a id="org8840bdd"></a>

## Index Properties


### Unique Indexes

The unique property for an index causes MongoDB to reject duplicate values for the indexed field. Other than the unique constraint, unique indexes are functionally interchangeable with other MongoDB indexes.

```javascript
> db.contacts.createIndex({email: 1}, {unique: true})
```


### Partial Indexes

Partial indexes only index the documents in a collection that meet a specified filter expression. By indexing a subset of the documents in a collection, partial indexes have lower storage requirements and reduced performance costs for index creation and maintenance.

Partial indexes offer a superset of the functionality of sparse indexes and should be preferred over sparse indexes.

```javascript
> db.contacts.createIndex({"dob.age": 1}, {partialFilterExpression: {gender: "male"}})

> db.contacts.explain().find({"dob.age": {$gt: 60}, gender: "male"})
```


### Sparse Indexes

The sparse property of an index ensures that the index only contain entries for documents that have the indexed field. The index skips documents that do not have the indexed field.

You can combine the sparse index option with the unique index option to prevent inserting documents that have duplicate values for the indexed field(s) and skip indexing documents that lack the indexed field(s).

```javascript
> db.users.insertMany(
    [
        {name: "user1", email: "user1@test.com"},
        {name: "user2"}
    ]
)

> db.users.createIndex({email: 1}, {unique: true})

// This will throw an duplicate key error, because the null value is already used
*+BEGIN_SRC javascript
> db.users.insertOne({name: "user3"})

// To avoid this defaul behaviour
> db.users.dropIndex({email: 1}, {unique: true})

> db.users.createIndex({email: 1}, {unique: true, partialFilterExpression: {email: {$exists: true}}})

// Now the insertion of an user without an email succeds without any errors
> db.users.insertOne({name: "user3"})
```


### TTL Indexes

TTL indexes are special indexes that MongoDB can use to automatically remove documents from a collection after a certain amount of time. This is ideal for certain types of information like machine generated event data, logs, and session information that only need to persist in a database for a finite amount of time.

```javascript
//inserts s recored with the current Date
> db.sessions.insertOne({data: "randomData1", createdAt: new Date()})

//create a index with the expireAfterSeconds to remove documents after a finite amount of time
> db.sessions.createIndex({createdAt: 1}, {expireAfterSeconds: 10})

//this new record will be delete after 10 seconds
> db.sessions.insertOne({data: "randomData2", createdAt: new Date()})
```


### Text Indexes

To create a text index, use the db.collection.createIndex() method. To index a field that contains a string or an array of string elements, include the field and specify the string literal &ldquo;text&rdquo; in the index document.

```javascript
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


#### Create Compound Text Indexes

You can index multiple fields for the text index. The following example creates a text index on the fields subject and comments.

```javascript
> db.products.dropIndex("description_text")

> db.products.createIndex({title: "text", description: "text"})

> db.products.insertOne({title: "item", description: "A specific item description"})

> db.products.find({$text: {$search: "item"}})
```


#### Text Indexes and sorting

```javascript
> db.products.find({$text: {$search: "random description"}}, {score: {$meta: "textScore"}}).pretty()


> db.products.find({$text: {$search: "random description"}}, {score: {$meta: "textScore"}}).sort({score: {$meta: "textScore"}}).pretty()
```


#### Using Text Indexes to exclude words

```javascript
> db.products.insertMany(
    [
        {title: "product5", description: "a description"},
        {title: "product6", description: "a specific description"},
        {title: "product7", description: "a special description"}
    ]
)

> db.products.find({$text: {$search: "description -random"}}).pretty()
```


#### Seting default language and specifying Weights

```javascript
> db.products.dropIndex("title_text_description_text")

> db.products.createIndex({title: "text", description: "text"}, {default_language: "english", weights: {title: 1, description: 10}})

> db.products.find({$text: {$search: "specific"}}, {score: {$meta: "textScore"}}).pretty()
```


#### Building Indexes

```javascript
> mongo credit-rating.js

> db.ratings.createIndex({age: 1}, {background: true})
```


<a id="org0c7b68a"></a>

## Query Optimization


### Covered Query

A covered query is a query that can be satisfied entirely using an index and does not have to examine any documents. An index covers a query when all of the following apply:

-   all the fields in the query are part of an index, ****and****
-   all the fields returned in the results are in the same index.
-   no fields in the query are equal to ****null**** (i.e. {****&ldquo;field&rdquo;**** : ****null****} or {****&ldquo;field&rdquo;**** : {****$eq**** : ****null****}} ).

```javascript
> db.customers.insertMany(
    [
        {name: "user1", age: 30, salary: 3000},
        {name: "user2", age: 40, salary: 5000}
    ]
)

> db.customers.createIndex({name: 1})
```

```javascript
// Returns all the fields that are also the indexed fields
> db.customers.explain("executionStats").find({name: "user1"}, {_id: 0, name: 1})
```

```javascript
> db.customers.getIndexes()

> db.customers.createIndexes({age: 1, name: 1})
```


<a id="orga3732db"></a>

## Query Plans

For a query, the MongoDB query optimizer chooses and caches the most efficient query plan given the available indexes. The evaluation of the most efficient query plan is based on the number of “work units” (works) performed by the query execution plan when the query planner evaluates candidate plans.

The associated plan cache entry is used for subsequent queries with the same query shape.


### Query Plan and Cache Information

To view the query plan information for a given query, you can use db.collection.explain() or the cursor.explain() .

Starting in MongoDB 4.2, you can use the $planCacheStats aggregation stage to view plan cache information for a collection. Plan Cache Flushes

The query plan cache does not persist if a mongod restarts or shuts down. In addition:

-   Catalog operations like index or collection drops clear the plan cache.
-   Least recently used (LRU) cache replacement mechanism clears the least recently accessed cache entry, regardless of state.

Users can also:

-   Manually clear the entire plan cache using the PlanCache.clear() method.
-   Manually clear specific plan cache entries using the PlanCache.clearPlansByQuery() method.


<a id="org7041c6f"></a>

## Multi-Key Indexes

To index a field that holds an array value, MongoDB creates an index key for each element in the array. These multikey indexes support efficient queries against array fields. Multikey indexes can be constructed over arrays that hold both scalar values [1] (e.g. strings, numbers) and nested documents.

![img](https://docs.mongodb.com/manual/_images/index-multikey.bakedsvg.svg "an example of a multikey index")

```javascript
> db.contacts.insertOne({name: "user1", hobbies: ["hobbie1", "hoobie2"], addresses: [{street: "street nowhere"}, {street: "street anywhere"}]})

> db.contacts.find().pretty()

> db.contacts.createIndex({hobbies: 1})

> db.contacts.find({hobbies: "hobbie1"}).pretty()

> db.contacts.explain("executionStats").find({hobbies: "hobbie1"})

> db.contacts.createIndex({addresses: 1})

> db.contacts.explain("executionStats").find({"addresses.street": "street nowhere"})

> db.contacts.explain("executionStats").find({addresses: {street: "street nowhere"}})
```


### Create Multikey Index

To create a multikey index, use the db.collection.createIndex() method.

MongoDB automatically creates a multikey index if any indexed field is an array; you do not need to explicitly specify the multikey type.


### Index Bounds

If an index is multikey, then computation of the index bounds follows special rules.


### Unique Multikey Index

For unique indexes, the unique constraint applies across separate documents in the collection rather than within a single document.

Because the unique constraint applies to separate documents, for a unique multikey index, a document may have array elements that result in repeating index key values as long as the index key values for that document do not duplicate those of another document.


### Limitations

For a compound multikey index, each indexed document can have at most one indexed field whose value is an array.

-   You cannot create a compound multikey index if more than one to-be-indexed field of a document is an array.
-   Or, if a compound multikey index already exists, you cannot insert a document that would violate this restriction.


<a id="org63a0289"></a>

# GeoSpatial Data


<a id="org30dcdbe"></a>

# Working with GeoSpatial Data

To specify GeoJSON data, use an embedded document with:

-   a field named type that specifies the GeoJSON object type and

-   a field named coordinates that specifies the object’s coordinates. If specifying latitude and longitude coordinates, list the longitude first and then latitude:
    -   Valid longitude values are between -180 and 180, both inclusive.
    -   Valid latitude values are between -90 and 90, both inclusive.


<a id="org4456b20"></a>

## Creating GeoJSON Data

```javascript
> use awesomeplaces

> db.places.insertOne({name: "Torre dos Clerigos", location: { type: "Point", coordinates: [ -8.6167872, 41.1456793 ] }})
```

```javascript
// Adding a Geospatial Index
> db.places.createIndex({location: "2dsphere"})
```


<a id="org6528c3c"></a>

## Running Geo Queries

```javascript
> db.places.find({location: {$near: {$geometry: {type: "Point", coordinates: [-8.6188578, 41.1458833]}}}})

> db.places.find({location: {$near: {$geometry: {type: "Point", coordinates: [-8.6188578, 41.1458833]}, $maxDistance: 200, $minDistance: 100 }}}).pretty()

```


<a id="org71830ff"></a>

## Adding additional locations

```javascript
> db.places.insertMany(
    [
       {name: "Aliados", location: { type: "Point", coordinates: [ -8.6172163, 41.1474184 ] }},
       {name: "Palacio Cristal", location: { type: "Point", coordinates: [ -8.6224198, 41.1468932 ] }},
       {name: "Alfandega do porto", location: { type: "Point", coordinates: [ -8.6224198, 41.1447926 ] }}
    ]
)

> const p1 = [ -8.60643 ,41.1508 ]
> const p2 = [-8.61197, 41.14106]
> const p3 = [ -8.63302, 41.14778]
> const p4 = [ -8.61628, 41.1589]

> db.places.find({location: {$geoWithin: {$geometry: {type: "Polygon", coordinates: [[p1, p2, p3, p4, p1]]}}}})
```


<a id="orgd7bb040"></a>

## Finding if a user inside a specific area

```javascript
> db.areas.insertOne({name: "Porto", area: { type: "Polygon", coordinates: [[p1, p2, p3, p4, p1]] }})

> db.areas.createIndex({area: "2dsphere"})

> db.areas.find({area: {$geoIntersects: {$geometry: {type: "Point", coordinates: [ -8.62224, 41.15858 ] }}}})
```


<a id="orgcffa1ca"></a>

## Finding Places Within a Certain Radius

```
> db.places.find({location: {$geoWithin: {$centerSphere: [[-8.62224, 41.15858], 1 / 6378.1] }}})
```


<a id="org8943759"></a>

# Numeric Data


<a id="org9cba691"></a>

# Working With Numeric Data

MongoDB Numeric Data types supported:

-   ****double****
-   ****32-bit integer****
-   ****64-bit integer****
-   ****decimal****


<a id="org4122994"></a>

## Integer

Simple plain numbers without decimal points, 32 bytes or 64 bytes, are saved as Int and retrieves as same.


### 32-bit integer

```javascript
> db.persons.insertOne({age: NumberInt(32)})

> optimal method
> db.persons.insertOne({age: NumberInt("32")})
```


### 64-bit integer

```javascript
> db.companies.insertOne({valuation: NumberLong(5000000000)})

> optimal method
> db.companies.insertOne({valuation: NumberLong("5000000000")})
```


<a id="orgf4c21a3"></a>

## Doing Maths with Floats int32s & int64

```javascript
> db.accounts.insertOne({name: "testUser", amount: NumberInt("10")})

> db.accounts.updateOne({}, {$inc:  {amount: NumberInt("10")}})
```

```javascript
> db.companies.insertOne({valuation: NumberLong("25432000000")})

> db.companies.updateOne({}, {$inc:  {valuation: NumberLong("360555")}})
```


<a id="org54742ca"></a>

## Double

Double is implemented for storing floating-point data in MongoDB.

```javascript
> db.science.insertOne({a: NumberDecimal("3.141592"), b: NumberDecimal("3.141")})

> db.science.aggregate([{$project: {result: {$subtract: ["$a", "$b"]}}}])
```


<a id="org84013b1"></a>

# Aggregation Framework

Aggregation operations process data records and return computed results. Aggregation operations group values from multiple documents together, and can perform a variety of operations on the grouped data to return a single result. MongoDB provides three ways to perform aggregation: the aggregation pipeline, the map-reduce function, and single purpose aggregation methods.


<a id="org620bef0"></a>

## Pipeline Concept

In UNIX command, shell pipeline means the possibility to execute an operation on some input and use the output as the input for the next command and so on. MongoDB also supports same concept in aggregation framework. There is a set of possible stages and each of those is taken as a set of documents as an input and produces a resulting set of documents (or the final resulting JSON document at the end of the pipeline). This can then in turn be used for the next stage and so on.

Following are the possible stages in aggregation framework:

-   $project − Used to select some specific fields from a collection.
-   $match − This is a filtering operation and thus this can reduce the amount of documents that are given as input to the next stage
-   $group − This does the actual aggregation as discussed above.
-   $sort − Sorts the documents.
-   $skip − With this, it is possible to skip forward in the list of documents for a given amount of documents.
-   $limit − This limits the amount of documents to look at, by the given number starting from the current positions.
-   $unwind − This is used to unwind document that are using arrays. When using an array, the data is kind of pre-joined and this operation will be undone with this to have individual documents again. Thus with this stage we will increase the amount of documents for the next stage.


<a id="org973d3d2"></a>

## Group Stage

Groups input documents by the specified \_id expression and for each distinct grouping, outputs a document. The \_id field of each output document contains the unique group by value. The output documents can also contain computed fields that hold the values of some accumulator expression.

```javascript
> mongoimport persons.json -d analytics -c persons --jsonArray

> db.persons.aggregate(
    [
        { $match: {gender: "female"}}
    ]
).pretty()
```

```javascript
> db.persons.aggregate(
    [
        { $match: {gender: "female"}},
        { $group: { _id: { state: "$location.state" }, totalPersons: { $sum: 1} }}
    ]
).pretty()

> db.persons.find({"location.state": "sinop"}).pretty()
```

```javascript
> db.persons.aggregate(
    [
        { $match: {gender: "female"}},
        { $group: { _id: { state: "$location.state" }, totalPersons: { $sum: 1} }},
        { $sort: { totalPersons: -1 }}
    ]
).pretty()
```


<a id="org0298d0f"></a>

## Project Stage

```javascript
> db.persons.aggregate(
    [
        { $project:
            { _id: 0, fullName: {
                $concat: [
                    { $toUpper: { $substrCP: ["$name.first", 0, 1]}},
                    { $substrCP: ["$name.first", 1, { $subtract: [ {$strLenCP: "$name.first"}, 1 ] } ]},
                    " ",
                    { $toUpper: { $substrCP: ["$name.last", 0, 1]}},
                    { $substrCP: ["$name.last", 1, { $subtract: [ {$strLenCP: "$name.last"}, 1 ] } ]}
                    ]
            }

            }
        }
    ]
).pretty()
```


<a id="org507ab12"></a>

## Turning the Location Into a geoJSON Object

```javascript
> db.persons.aggregate(
    [
        {
            $project: {
                _id: 0,
                name: 1,
                email: 1,
                location: {
                    type: "Point",
                    coordinates: [
                        { $convert: { input: "$location.coordinates.longitude", to: "double", onError: 0.0, onNull: 0.0}},
                        { $convert: { input:  "$location.coordinates.latitude", to: "double", onError: 0.0, onNull: 0.0}}
                    ]
                }
            }
        },
        { $project:
            { gender: 1, email: 1, location: 1, fullName: {
                $concat: [
                    { $toUpper: { $substrCP: ["$name.first", 0, 1]}},
                    { $substrCP: ["$name.first", 1, { $subtract: [ {$strLenCP: "$name.first"}, 1 ] } ]},
                    " ",
                    { $toUpper: { $substrCP: ["$name.last", 0, 1]}},
                    { $substrCP: ["$name.last", 1, { $subtract: [ {$strLenCP: "$name.last"}, 1 ] } ]}
                    ]
            }

            }
        }
    ]
).pretty()
```


<a id="org53e86e1"></a>

## Transforming the Birthdate

```javascript
> db.persons.aggregate(
    [
        {
            $project: {
                _id: 0,
                name: 1,
                email: 1,
                birthdate: { $convert: { input: "$dob.date", to: "date", onError: 0.0, onNull: 0.0 } },
                age: "$dob.age",
                location: {
                    type: "Point",
                    coordinates: [
                        { $convert: { input: "$location.coordinates.longitude", to: "double", onError: 0.0, onNull: 0.0}},
                        { $convert: { input:  "$location.coordinates.latitude", to: "double", onError: 0.0, onNull: 0.0}}
                    ]
                }
            }
        },
        { $project: {
            gender: 1,
            email: 1,
            location: 1,
            birthdate: 1,
            age: 1,
            fullName: {
                $concat: [
                    { $toUpper: { $substrCP: ["$name.first", 0, 1]}},
                    { $substrCP: ["$name.first", 1, { $subtract: [ {$strLenCP: "$name.first"}, 1 ] } ]},
                    " ",
                    { $toUpper: { $substrCP: ["$name.last", 0, 1]}},
                    { $substrCP: ["$name.last", 1, { $subtract: [ {$strLenCP: "$name.last"}, 1 ] } ]}
                    ]
            }

            }
        }
    ]
).pretty()
```


<a id="org1df846a"></a>

## Using Shortcuts for Transformations

```javascript
> db.persons.aggregate(
    [
        {
            $project: {
                _id: 0,
                name: 1,
                email: 1,
                birthdate: { $toDate: "$dob.date" },
                age: "$dob.age",
                location: {
                    type: "Point",
                    coordinates: [
                        { $convert: { input: "$location.coordinates.longitude", to: "double", onError: 0.0, onNull: 0.0}},
                        { $convert: { input:  "$location.coordinates.latitude", to: "double", onError: 0.0, onNull: 0.0}}
                    ]
                }
            }
        },
        { $project: {
            gender: 1,
            email: 1,
            location: 1,
            birthdate: 1,
            age: 1,
            fullName: {
                $concat: [
                    { $toUpper: { $substrCP: ["$name.first", 0, 1]}},
                    { $substrCP: ["$name.first", 1, { $subtract: [ {$strLenCP: "$name.first"}, 1 ] } ]},
                    " ",
                    { $toUpper: { $substrCP: ["$name.last", 0, 1]}},
                    { $substrCP: ["$name.last", 1, { $subtract: [ {$strLenCP: "$name.last"}, 1 ] } ]}
                    ]
            }

            }
        }
    ]
).pretty()
```


<a id="org3843f91"></a>

## Understanding the $isoWeekYear Operator

```javascript
> db.persons.aggregate(
    [
        {
            $project: {
                _id: 0,
                name: 1,
                email: 1,
                birthdate: { $toDate: "$dob.date" },
                age: "$dob.age",
                location: {
                    type: "Point",
                    coordinates: [
                        { $convert: { input: "$location.coordinates.longitude", to: "double", onError: 0.0, onNull: 0.0}},
                        { $convert: { input:  "$location.coordinates.latitude", to: "double", onError: 0.0, onNull: 0.0}}
                    ]
                }
            }
        },
        {
            $project: {
                gender: 1,
                email: 1,
                location: 1,
                birthdate: 1,
                age: 1,
                fullName: {
                    $concat: [
                        { $toUpper: { $substrCP: ["$name.first", 0, 1]}},
                        { $substrCP: ["$name.first", 1, { $subtract: [ {$strLenCP: "$name.first"}, 1 ] } ]},
                        " ",
                        { $toUpper: { $substrCP: ["$name.last", 0, 1]}},
                        { $substrCP: ["$name.last", 1, { $subtract: [ {$strLenCP: "$name.last"}, 1 ] } ]}
                        ]
                }
            }
        },
        {
            $group: { _id: { birthYear: { $isoWeekYear: "$birthdate"} }, numPersons: { $sum: 1 } },
        },
        {
            $sort: { numPersons: -1}
        }
    ]
).pretty()
```


<a id="org11fa94d"></a>

## Pushing Elements Into Newly Created Arrays

```javascript
> mongoimport array-data.json -d analytics -c friends --jsonArray

> db.friends.aggregate(
    [
        { $group: { _id: { age: "$age"}, allHobbies: { $push: "$hobbies" } } }
    ]
).pretty()

```


<a id="orgab1f6af"></a>

## unWind Stage

```javascript
> db.friends.aggregate(
    [
        { $unwind: "$hobbies"},
        { $group: { _id: { age: "$age"}, allHobbies: { $push: "$hobbies" } } }
    ]
).pretty()
```


<a id="org9612695"></a>

## addToSet Stage

```javascript
> db.friends.aggregate(
    [
        { $unwind: "$hobbies"},
        { $group: { _id: { age: "$age"}, allHobbies: { $addToSet: "$hobbies" } } }
    ]
).pretty()
```


<a id="orga78cd7a"></a>

## slice operator

```javascript
> db.friends.aggregate(
    [
        { $project: { _id: 0, examScore: { $slice: ["$examScores", 2, 1]}}}
    ]
).pretty()
```


<a id="orgdcc0e25"></a>

## $slice operator

```javascript
> db.friends.aggregate(
    [
        { $project: { _id: 0, numScores: { $size: "$examScores"}}}
    ]
).pretty()
```


<a id="org55a0d42"></a>

## $filter operator

```javascript
> db.friends.aggregate(
    [
        {
            $project: {
                _id: 0,
                scores: { $filter: { input: "$examScores", as: "sc", cond: { $gt: ["$$sc.score", 60] }}}
            }
        }
    ]
).pretty()
```


<a id="org54ac867"></a>

## Aplying Multiple Operations to an array

```javascript
> db.friends.aggregate(
    [
        { $unwind: "$examScores"},
        { $project: { _id: 1, name: 1, age: 1, score: "$examScores.score"}},
        { $sort: { score: -1}},
        { $group: { _id: "$_id", name: { $first: "$name"}, maxScore: { $max: "$score"}}},
        { $sort: { maxScore: -1}}
    ]
).pretty()
```


<a id="org640f58e"></a>

## $bucket Stage

```javascript
> db.persons.aggregate(
    [
        {
            $bucket: {
                groupBy: "$dob.age",
                boundaries: [0, 18, 30, 50, 80, 120],
                output: {
                    numPersons: { $sum: 1 },
                    averageAge: { $avg: '$dob.age' }
                }
            }
        }
    ]
).pretty()
```


<a id="orgc0b6e30"></a>

## Aditional Stages

```javascript
> db.persons.aggregate(
    [
        {
            $match: {
                gender: "male"
            }
        },
        {
             $project: {
                 _id: 0,
                 name: { $concat: ["$name.first", " ", "$name.last"]},
                 gender: 1,
                 birthdate: { $toDate: "$dob.date"}
            }
        },
        {
            $sort: {
                birthdate: 1
            }
        },
        {
            $skip: 10
        },
        {
            $limit: 10
        }
    ]
).pretty()
```


<a id="org4eabc7e"></a>

## Writing Pipelines into a new collection

```javascript
db.persons.aggregate([
    {
      $project: {
        _id: 0,
        name: 1,
        email: 1,
        birthdate: { $toDate: '$dob.date' },
        age: "$dob.age",
        location: {
          type: 'Point',
          coordinates: [
            {
              $convert: {
                input: '$location.coordinates.longitude',
                to: 'double',
                onError: 0.0,
                onNull: 0.0
              }
            },
            {
              $convert: {
                input: '$location.coordinates.latitude',
                to: 'double',
                onError: 0.0,
                onNull: 0.0
              }
            }
          ]
        }
      }
    },
    {
      $project: {
        gender: 1,
        email: 1,
        location: 1,
        birthdate: 1,
        age: 1,
        fullName: {
          $concat: [
            { $toUpper: { $substrCP: ['$name.first', 0, 1] } },
            {
              $substrCP: [
                '$name.first',
                1,
                { $subtract: [{ $strLenCP: '$name.first' }, 1] }
              ]
            },
            ' ',
            { $toUpper: { $substrCP: ['$name.last', 0, 1] } },
            {
              $substrCP: [
                '$name.last',
                1,
                { $subtract: [{ $strLenCP: '$name.last' }, 1] }
              ]
            }
          ]
        }
      }
    },
    { $out: "transformedPersons" }
  ]).pretty();
```


<a id="org38d8ef7"></a>

## \*geoNear Stage

```javascript
> db.transformedPersons.createIndex({location: "2dsphere"})

> db.transformedPersons.aggregate([
    {
      $geoNear: {
        near: {
          type: 'Point',
          coordinates: [-18.4, -42.8]
        },
        maxDistance: 1000000,
        query: { age: { $gt: 30 } },
        distanceField: 'distance'
      }
    },
    { $limit : 10 }
  ]).pretty()

```


<a id="org032cda4"></a>

# MongoDB & Securtiy

MongoDB provides various features, such as authentication, access control, encryption, to secure your MongoDB deployments. Some key security features include:

-   Authentication
-   Authorization
-   TLS/SSL
-   Encryption


<a id="orgaa1a96a"></a>

## Starting mongo servers with authorization


<a id="org0429669"></a>

## Create a user


<a id="org86f81b7"></a>

## with admin privileges

```javascript
> use admin

> db.createUser({user: "user", pwd: "password", roles: ["userAdminAnyDatabase"]})
```


<a id="org4129140"></a>

## with restricted privileges

```javascript
> use shop

> db.createUser({user: "appdev", pwd: "appdevpwd", roles: ["readWrite"]})

> db.auth("appdev", "appdevpwd")

> db.products.insertOne({name: "Book"})
```


<a id="orga61b784"></a>

## Update a user

```javascript
> db.updateUser("appdev", {roles: ["readWrite", {role: "readWrite", db: "blog"}]})

> db.blog.insertOne({title: "Test blog post"})
```


<a id="org5d0deb4"></a>

## Logging to the server with auth

```javascript
> db.auth("user", "password")
```


<a id="org87b19a1"></a>

## Adding SSL Transport Encryption

```javascript
> generate a self certified certificate and create a mongodb.pem file

openssl req -newkey rsa:2048 -new -x509 -days 365 -nodes -out mongodb-cert.crt -keyout mongodb-cert.key

cat mongodb-cert.key mongodb-cert.crt >mongodb.pem

> start mongo server with tsl encryption
> mongod --tlsMode requireTLS --tlsCertificateKeyFile /home/mdlima/.certs/mongodb.pem --dbpath /home/mdlima/

> start mongo client with tsl encryption
> mongo --tls --tlsCAFile /home/mdlima/.certs/mongodb.pem --host localhost
```


<a id="org7b0313f"></a>

# Performance, Fault Tolerancy & Deployment


<a id="orge413d80"></a>

# Performance, Fault Tolerancy & Deployment


<a id="org8a31447"></a>

## Capped Collections

Capped collections are fixed-size collections that support high-throughput operations that insert and retrieve documents based on insertion order. Capped collections work in a way similar to circular buffers: once a collection fills its allocated space, it makes room for new documents by overwriting the oldest documents in the collection.

```javascript
> use performance

> db.createCollection("capped", {capped: true, size: 10000, max: 3})

> db.capped.inserOne({name: "user1"})

> db.capped.inserOne({name: "user2"})

> db.capped.insertOne({name: "user3"})

> db.capped.find().sort({$natural: -1}).pretty()

> db.capped.inserOne({name: "user4"})
```


<a id="org52d09bf"></a>

## Sharding

Sharding is a method for distributing data across multiple machines. MongoDB uses sharding to support deployments with very large data sets and high throughput operations.

Database systems with large data sets or high throughput applications can challenge the capacity of a single server. For example, high query rates can exhaust the CPU capacity of the server. Working set sizes larger than the system’s RAM stress the I/O capacity of disk drives.

There are two methods for addressing system growth: vertical and horizontal scaling:

-   Vertical Scaling
-   Horizontal Scaling


### Sharded Cluster

A MongoDB sharded cluster consists of the following components:

-   shard: Each shard contains a subset of the sharded data. Each shard can be deployed as a replica set.
-   mongos: The mongos acts as a query router, providing an interface between client applications and the sharded cluster. Starting in MongoDB 4.4, mongos can support hedged reads to minimize latencies.
-   config servers: Config servers store metadata and configuration settings for the cluster.

![img](https://docs.mongodb.com/manual/_images/sharded-cluster-production-architecture.bakedsvg.svg "interaction of components within a sharded cluster")


<a id="org4a51c38"></a>

## Using MongoDB Atlas


### Connecting to the cluster

To create and use a cluster on MongoDB is necessary to:

-   create and account, then create a cluster with the desired space and configurations.
-   create a user or users with the desired permissions.
-   allow the ip&rsquo;s necessary for the connection.
-   connect using the link provided.

```javascript
> mongo "mongodb+sr*V://.8qaxe.mongodb.net/products" --username mdlima
```


<a id="orgc96303d"></a>

# Transactions

In MongoDB, an operation on a single document is atomic. Because you can use embedded documents and arrays to capture relationships between data in a single document structure instead of normalizing across multiple documents and collections, this single-document atomicity obviates the need for multi-document transactions for many practical use cases.

For situations that require atomicity of reads and writes to multiple documents (in a single or multiple collections), MongoDB supports multi-document transactions. With distributed transactions, transactions can be used across multiple operations, collections, databases, documents, and shards.

```javascript
> use blog

> db.users.insertOne({name: "user1"})

> db.posts.insertMany(
    [
        {title: "test post one", userId: ObjectId("5f8108c80ad78b612ea094f1")},
        {title: "test post two", userId: ObjectId("5f8108c80ad78b612ea094f1")}
    ]
)

> const session = db.getMongo().startSession()

> session.startTransaction()

> const usersCol = session.getDatabase("blog").users

> const postsCol = session.getDatabase("posts").posts

> db.users.find({ObjectId("5f8102850ad78b612ea094eb")}).pretty()

> usersCol.deleteOne({_id: ObjectId("5f8108c80ad78b612ea094f1")})

> postsCol.deleteMany({userId: ObjectId("5f8107110ad78b612ea094ee")})

// to commit the changes
> session.commitTransaction()

// to abort the transacation
> session.abortTransaction()
```


<a id="orgc19aba9"></a>

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
