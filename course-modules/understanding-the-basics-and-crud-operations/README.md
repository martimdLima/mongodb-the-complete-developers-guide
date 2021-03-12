# Understanding Crud Operations 

# Show/Create a DB

## Show all databases
 ```sh
>  show dbs
```

## Use a database
 ```sh
> use dbname
```

# Create Documents and Collections

## Create a document in a collection
 ```sh
> db.dbname.insertOne({name: "dummy", price: 12.99})
```

## Create a document with an embbeded document
 ```sh
> db.dbname.insertOne({
        key1: "dummy", 
        key2: 12.99, 
        key3: "A dummy product", 
        key4: {
            key4subkey1: "key4subval1", 
            key4subkey1: key4subkey2
        }
    })
```

## Create various documents in a collection

 ```sh
 > db.dbname.insertMany([{
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
    ])
```

# Find Documents in a Collection

find() doesn't return an array of all the documents in a collection, but a cursor

pretty() can only be applied when the result of the operation applied returns a cursor while find() returns a cursor, findOne() returns a document, so it can only be applied to find()

## Find all documents in a collection
 ```sh
 > db.dbname.find()
 ```

## Find all documents in a collection prettified
 ```sh
 > db.dbname.find().pretty()
 ```

## Find all documents in a collection and convert it to array (This operation returns every document in a collection, despite the size)
 ```sh
 > db.dbname.find().toArray()
 ```

## Find all documents in a collection and apply certain operations on each one
 ```sh
 > db.dbname.find().forEach((colData) => {printjson(colData)})
 ```
 
## Find all documents in a collection that match a specific key/value
 ```sh
 > db.dbname.find({key: "value"})
 ```

## find all documents where the value of the key is greater than something
 ```sh
 > db.dbname.find({key: {$gt: value}})
 ```

## Find the first document in a collection that match a specific key/value
 ```sh
 > db.dbname.findOne({key: "value"})
 ```

## find one document where the value of the key is greater than something
 ```sh
 > db.dbname.findOne({key: {$gt: value}}) 
 ```

# [Update Documents in a Collection]

## Update one document
 ```sh
 > db.dbname.updateOne({key: "value"}, {$set: {key: "updtValue"}})
 ```

## Update many documents
 ```sh
 > db.dbname.updateMany({}, {$set: {key: "updtValue"}})
 ```

## Update a document replacing all key/value pairs
 ```sh
 > db.dbname.update({key: "value"}, {{key: "updtValue"}})
 ```

# [Replace Documents]

## Replace a document
 ```sh
 > db.dbname.replaceOne(  {
    key1: "value1",
    key2: "value2",
    key3: "value3",
    key4: "value4",
  })
 ```

# [Delete Documents]

## Delete one document
 ```sh
 > db.dbname.deleteOne({key: "value"})
 ```

## Delete all documents that have a common key/value pair
 ```sh
 > db.dbname.deleteMany({commonKey: "commonValue"})
 ```

## Delete all documents desregarding their key/value pairs
 ```sh
 > db.dbname.deleteMany({})
 ```

# [Projections]

## Using projections to return all the documents with the choosen properties  
 ```sh
 > db.dbname.find({}, {key: value}).pretty()
 ```

## Using projections to return all the documents in the collection with the choosen properties, excluding some 
 ```sh
 > db.dbname.find({}, {key: value, _key: value}).pretty()
 ```

# Working with nested documents

# Update a document with a nested document
 ```sh
 > db.dbname.updateMany({}, {$set: {key: {nestedKey1: "nestedValue1", nestedKey2: "nestedValue2"}}})
 ```

# Update a document with a nested document
 ```sh
 > db.dbname.updateMany({}, {$set: {key: {nKey1: "nVal1", nKey2: "nVal2", nK3: {nk3a: "nk3aVal"}}}})
 ```

# [Working With Arrays]
 ```sh
 > db.dbname.updateOne({key: "val"}, {$set: {newKey: ["val1", "val2"]}})
 ```

# Acessing Structured Data

## Acessing a specific field of a document
 ```sh
 > db.dbname.findOne({key: "val"}).key2
 ```

## Searching for documents with a specific field
 ```sh
 > db.dbname.find({key: "value"}).pretty()
 ```

## Searching for documents that have a field with a specific subfield and value present
 ```sh
 > db.dbname.find({"key.subkey": "subkeyVal"}).pretty()
 ```

## Searching for documents that have a field that has a subfield with a specific subfield and value present
 ```sh
 > db.dbname.find({"key.subkey.subsubkey": "subsubkeyVal"}).pretty()
 ```
