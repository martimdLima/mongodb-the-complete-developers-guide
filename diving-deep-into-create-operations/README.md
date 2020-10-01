# Diving deep into Create Operations

<br/>

# Understanding insert() Methods

```sh
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

<br/>

# Ordered Inserts

```sh
> db.hobbies.insertMany(
    [
        {_id: "sports", name: "Sports"},
        {_id: "cooking", name: "Cooking"},
        {_id: "cars", name: "Cars"},
    ]
)
```

<br/>

> As it is the default behaviour of mongoDB, if we try to insert a value already added before,
> it will cancel the operation and output an error, but since mongoDB by defaul treats each operation
> as standalone, it will insert the first value and cancel the others inserts when it gets to the second
> element.

<br/>

```sh
> db.hobbies.insertMany(
    [
        {_id: "astronomy", name: "astronomy"},
        {_id: "cooking", name: "Cooking"},
        {_id: "hiking", name: "Hiking"},
    ]
)
```

<br/>

> To override this behaviour, we can pass a second argument to insertMany().
> This second argument is a document used to configure this operation.
> In this case, it will fail the first two values and insert the last two values.

<br/>

```sh
> db.hobbies.insertMany(
    [
        {_id: "sports", name: "Sports"},
        {_id: "cooking", name: "Cooking"},
        {_id: "astronomy", name: "astronomy"},
        {_id: "hiking", name: "Hiking"},
    ],
    {ordered: false}
)
```

<br/>

# writeconcern

<br/>

> Write concern describes the level of acknowledgment requested from MongoDB for write operations to a standalone mongod or to replica sets or to sharded clusters. 
> In sharded clusters, mongos instances will pass the write concern on to the shards.
>
> Starting in MongoDB 4.4, replica sets and sharded clusters support setting a global default write concern. 
> Operations which do not specify an explicit write concern inherit the global default write concern settings.

<br/>

## Practice

<br/>

> w can be set to either 1 or 0, for server acknowledgment.

<br/>

```sh
> db.persons.insertOne(        
    {
        name: "dummy6",
        age: 38,
    },
    {
        writeConcern: {w: 1}
    }
)
```

<br/>

> j can be set to either true or false, to trigger jornaling of the entry

<br/>

```sh
> db.persons.insertOne(        
    {
        name: "dummy7",
        age: 38,
    },
    {
        writeConcern: {w: 1, j: true}
    }
)
```

<br/>

> wtimeout can be set to the timeout of the write operation

<br/>

```sh
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

<br/>

# Atomicity

<br/>

> In MongoDB, a write operation is atomic on the level of a single document, 
> even if the operation modifies multiple embedded documents within a single document.

<br/>

## Multi-Document Transactions

<br/>

> When a single write operation (e.g. db.collection.updateMany()) modifies multiple documents, 
> the modification of each document is atomic, but the operation as a whole is not atomic.
>
> When performing multi-document write operations, whether through a single write operation or multiple write operations, 
> other operations may interleave.

<br/>

# Module Assignment - Create Operations

<br/>

> Basic Creation Operations - InsertOne() and InsertMany() 

<br/>

```sh
> use companyData

> db.companies.insertOne(
    {
        name: "dummy Corp",
        sector: "Industrial Chemicals",
        numEmployees: 32,
        products: ["Pharmaceutical Products", "Beauty Products"]
    }
)

> db.companies.insertMany(
    [
        {
            name: "dummy Corp2",
            sector: "Lab Chemicals",
            numEmployees: 32,
            products: ["Reagents", "Starters", "Cleaning chemicals", "Solvents"]
        },
        {
            name: "dummy Corp3",
            sector: "Auto",
            numEmployees: 43,
            products: ["cars", "car parts"] 
        },
        {
            name: "dummy Corp4",
            sector: "Auto",
            numEmployees: 10,
            products: ["pcbs", "electronic parts"]
        },
        {
            name: "dummy Corp5",
            sector: "Food",
            numEmployees: 90,
            products: ["cookies", "chips", "cakes"] 
        }
    ]
)
```

<br/>

> Using ordered to prevent mongoDB default behaviour and transact the last two items

<br/>

```sh
> db.companies.insertMany(
    [
        {
            _id: ObjectId("5f760a4aae20e131a9073ee1"),
            name: "dummy Corp4",
            sector: "Lab Chemicals",
            numEmployees: 32,
            products: ["Reagents", "Starters", "Cleaning chemicals", "Solvents"]
        },
        {
            _id: ObjectId("5f760a4aae20e131a9073ee2"),
            name: "dummy Corp5",
            sector: "Auto",
            numEmployees: 43,
            products: ["cars", "car parts"] 
        },
        {
            name: "dummy Corp6",
            sector: "Auto",
            numEmployees: 10,
            products: ["pcbs", "electronic parts"]
        },
        {
            name: "dummy Corp7",
            sector: "Food",
            numEmployees: 90,
            products: ["cookies", "chips", "cakes"] 
        }
    ],
    {
        ordered: false
    }
)
```

<br/>

> Creating a new entry with journaling on and off

<br/>

```sh
> db.companies.insertOne(
        {
            name: "dummy Corp8",
            sector: "Computers",
            numEmployees: 650,
            products: ["Computers", "Laptops"]
        },
        {
            writeConcern: {w: 1, j: true}
        }
)

> db.companies.insertOne(
        {
            name: "dummy Corp9",
            sector: "Graphic Cards",
            numEmployees: 300,
            products: ["Graphic Cards", "Connectors"] 
        },
        {
            writeConcern: {w: 1, j: false}
        }
)
```

# Importing Data

> in the terminal shell
```sh
> mongoimport tv-shows.json -d movieData -c movies --jsonArray --drop
```