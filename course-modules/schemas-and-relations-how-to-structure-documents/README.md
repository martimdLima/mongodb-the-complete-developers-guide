# Schemas & Relations - How to Structure Documents

<br/>

# Structuring Documents

```sh
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

<br/>

# Data Types

```sh
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

<br/>

# Database Stats

```sh
> db.stats()
```

<br/>

# Check the type of a value

```sh
> typeof db.colname.findOne().key
```

<br/>

# Insert a document with a number with a default 64 bit value

```sh
> db.colname.insertOne({key: 1})
```

<br/>

# Insert a product with a number with an int32 value instead of 64 bit

```sh
> db.colname.insertOne({key: NumberInt(1)})
```

<br/>

# Important data type limits are:

<br/>

> Normal integers (int32) can hold a maximum value of +-2,147,483,647

> Long integers (int64) can hold a maximum value of +-9,223,372,036,854,775,807

> Text can be as long as you want - the limit is the 16mb restriction for the overall document

<br/>

It's also important to understand the difference between int32 (NumberInt), int64 (NumberLong) and a normal number as you can enter it in the shell. The same goes for a normal double and NumberDecimal.

<br/>

> NumberInt creates a int32 value => NumberInt(55)

> NumberLong creates a int64 value => NumberLong(7489729384792)

<br/>

If you just use a number (e.g. insertOne({a: 1}), this will get added as a normal double into the database.
The reason for this is that the shell is based on JS which only knows float/ double values and doesn't differ between integers and floats.

<br/>

> NumberDecimal creates a high-precision double value => NumberDecimal("12.99") => This can be helpful for cases where you need (many) exact decimal places for calculations.

<br/>

When not working with the shell but a MongoDB driver for your app programming language (e.g. PHP, .NET, Node.js, ...), you can use the driver to create these specific numbers.

<br/>

## [Example for Node.js](http://mongodb.github.io/node-mongodb-native/3.1/api/Long.html)

<br/>

> This will allow building a NumberLong value:

```sh
    const Long = require('mongodb').Long;

    db.collection('wealth').insert( {
        value: Long.fromString("121949898291")
    });
```

<br/>

# Relations

<br/>

## One to One Relations - Nested

```sh
> use hospital

> db.patients.insertOne(
    }
        name: "dummy",
        age: 30,
        diseaseSummary: "summary-dummy-1"
    }
)

> db.diseases.insertOne(
    {
        _id: "summary-dummy-1",
        diseases: ["dummyDisease1", "dummyDisease2"]
    }
)

> db.patients.findOne({name: "dummy"}).diseaseSummary

> var dsid = db.patients.findOne({name: "dummy"}).diseaseSummary

> dsid

>  db.diseases.find({_id: dsid})

> db.patients.deleteMany({})

> db. patients.insertOne(
    {
        name: "dummy",
        age: 30,
        diseases: ["disease1", "disease2"}
    }
)
```

<br/>

## One to One Relations - References

```sh
> use carData

> db.persons.insertOne(
    {
        name: "dummy",
        car: {model: "ZTY", price: 4000}
    }
)

> db.persons.findOne()

> db.persons.deleteMany({})

> db.persons.insertOne(
    {
        name: "dummy",
        age: 30,
        salary: 3000
    }
)

> db.cars.insertOne(
    {
        model: "ZTY",
        price: 4000,
        owner: ObjectId("5f75175f742f302b6b694e13")
    }
)

> db.persons.find({_id: ObjectId("5f75175f742f302b6b694e13")})
```

<br/>

## One To Many - Embedded

```sh
> use support

> db.questionThreads.insertOne({creator: "dummy", question: "Is this a dummy question?", answers: ["q1a1", "q1a2"]})

> db.questionThreads.find().pretty()

> db.answers.insertMany(
    [
        {_id: "q1a1", text: "Yes, this is a dummy question"},
        {_id: "q1a2", text: "No, this isn't a dummy question"}
    ]
)

> db.answers.find()

> db.questionThreads.deleteMany({})

> db.questionThreads.insertOne(
    {
        creator: "dummy",
        question: "Is this a dummy question?",
        answers: [
                    {text: "Yes, this is a dummy question"},
                    {text: "No, this isn't a dummy question"}
                 ]
    }
)

> db.questionThreads.findOne()

```

<br/>

## One To Many - References

```sh

> use cityData

> db.cities.insertOne({name: "New York City", coordinates: {lat: 21, lng: 55}})

> db.cities.findOne()

> db.citizens.insertMany(
    [
        {name: "dummy1" , cityId: ObjectId("5f751ca7958492cdcb480a21")},
        {name: "dummy2" , cityId: ObjectId("5f751ca7958492cdcb480a21")},
        {name: "dummy3" , cityId: ObjectId("5f751ca7958492cdcb480a21")},
        {name: "dummy4" , cityId: ObjectId("5f751ca7958492cdcb480a21")},
    ]
)

> db.citizens.find().pretty()

```

<br/>

## Many To Many - Embedded

```sh

> use shop

> db.products.insertOne(
    {
      title: "dummyProduct",
      price: 23.99
    }
)

> db.customers.insertOne(
    {
        name: "dummy",
        age: 30
    }
)

> db.orders.insertOne(
    {
        productId: ObjectId("5f751f61958492cdcb480a27"),
        customerId: ObjectId("5f751f19958492cdcb480a26")
    }
)

> db.orders.drop()

> db.orders.find()

> db.customers.updateOne({}, {$set: {orders: [{productId: ObjectId("5f751f61958492cdcb480a27"), quantity: 3}]}})

> db.customers.findOne()

> db.customers.updateOne({}, {$set: {orders: [{title: "dummyProduct", price: 23.99, quantity: 3}]}})

```

<br/>

## Many To Many - References

```sh
> use bookRegistry

> db.books.insertOne(
    {
        name: "Dummy Book",
        authors: [{name: "dummy1", age:30}, {name: "dummy2", age: 40}]
    }
)

> db.authors.insertMany(
    [
        {name: "dummy1", age: 30, address: "DeathStar"},
        {name: "dummy2", age: 40, address: "Aldooran"}
    ]
)

> db.authors.find().pretty()

> db.books.updateOne({}, {$set: {authors: [ObjectId("5f752335958492cdcb480a2a"), ObjectId("5f752335958492cdcb480a2b")]}})

> db.books.findOne()
```

<br/>

# Using "lookUp()" for Merging Reference Relations

<br/>

> Performs a left outer join to an unsharded collection in the same database to filter in documents from the “joined” collection for processing.
> To each input document, the $lookup stage adds a new array field whose elements are the matching documents from the “joined” collection. 
> The $lookup stage passes these reshaped documents to the next stage.

<br/>

```sh
> db.books.aggregate([{$lookup: {from: "authors", localField: "authors", foreignField: "_id", as: "creators"}}]).pretty()
```

<br/>

# Planning and Implementing an example exercise

<br/>

# Collection Document Validation

```sh
db.createCollection('posts', {
    validator: {
      $jsonSchema: {
        bsonType: 'object',
        required: ['title', 'text', 'creator', 'comments'],
        properties: {
          title: {
            bsonType: 'string',
            description: 'must be a string and is required'
          },
          text: {
            bsonType: 'string',
            description: 'must be a string and is required'
          },
          creator: {
            bsonType: 'objectId',
            description: 'must be an objectid and is required'
          },
          comments: {
            bsonType: 'array',
            description: 'must be an array and is required',
            items: {
              bsonType: 'object',
              required: ['text', 'author'],
              properties: {
                text: {
                  bsonType: 'string',
                  description: 'must be a string and is required'
                },
                author: {
                  bsonType: 'objectId',
                  description: 'must be an objectid and is required'
                }
              }
            }
          }
        }
      }
    }
  });
```

<br/>

# Changing Validation Action

```sh
> db.runCommand({
    collMod: 'posts',
    validator: {
      $jsonSchema: {
        bsonType: 'object',
        required: ['title', 'text', 'creator', 'comments'],
        properties: {
          title: {
            bsonType: 'string',
            description: 'must be a string and is required'
          },
          text: {
            bsonType: 'string',
            description: 'must be a string and is required'
          },
          creator: {
            bsonType: 'objectId',
            description: 'must be an objectid and is required'
          },
          comments: {
            bsonType: 'array',
            description: 'must be an array and is required',
            items: {
              bsonType: 'object',
              required: ['text', 'author'],
              properties: {
                text: {
                  bsonType: 'string',
                  description: 'must be a string and is required'
                },
                author: {
                  bsonType: 'objectId',
                  description: 'must be an objectid and is required'
                }
              }
            }
          }
        }
      }
    },
    validationAction: 'warn'
});
```
