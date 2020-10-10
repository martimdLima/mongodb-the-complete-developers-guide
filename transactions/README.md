# Transactions

<div align="center">
In MongoDB, an operation on a single document is atomic. Because you can use embedded documents and arrays to capture relationships between data in a single document structure instead of normalizing across multiple documents and collections, this single-document atomicity obviates the need for multi-document transactions for many practical use cases.

For situations that require atomicity of reads and writes to multiple documents (in a single or multiple collections), MongoDB supports multi-document transactions. With distributed transactions, transactions can be used across multiple operations, collections, databases, documents, and shards.
</div>

<br/>

``` sh

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

# to commit the changes
> session.commitTransaction()

# to abort the transacation
> session.abortTransaction()

```

# Useful Resources and Links
* [Transactions](https://docs.mongodb.com/manual/core/transactions/index.html)
