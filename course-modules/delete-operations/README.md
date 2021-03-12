# Delete Operations

<br/>

## deleteOne() 

<br/>

<p>
Delete Only One Document that Matches a Condition.

To delete at most a single document that matches a specified filter (even though multiple documents may match the specified filter) use the db.collection.deleteOne() method.
</p>

<br/>

```sh
> db.users.deleteOne({name: "Chris"})
```

<br/>

## deleteMany()

<br/>

<p>
Removes all documents that match the filter from a collection.

To delete all documents from a collection, pass an empty filter document {} to the db.collection.deleteMany() method.
</p>

<br/>

```sh
> db.users.deleteMany({})
```

<br/>

<p>
You can specify criteria, or filters, that identify the documents to delete. The filters use the same syntax as read operations.
</p>

<br/>

```sh
> db.users.deleteMany({totalAge: {$exists: false}, isSporty: true})
```

<br/>

## drop()

<p>
Removes a collection or view from the database. The method also removes any indexes associated with the dropped collection. 
The method provides a wrapper around the drop command.

Should only be done by sysAdmins
</p>

<br/>

> Drops the collection
```
> db.users.drop()
```

<br/>

## dropDatabase()

<br/>

<p>
Removes the current database, deleting the associated data files.
</p>

<br/>

> Drops the database
```
> db.dropDatabase()
```

<br/>