# Update Operations

<br/>

## Update Operators

<br/>

### \$set

<br/>

<p>
The $set operator replaces the value of a field with the specified value.

If the field does not exist, $set will add a new field with the specified value, provided that the new field does not violate a type constraint. 
If you specify a dotted path for a non-existent field, $set will create the embedded documents as needed to fulfill the dotted path to the field.

</p>

<br/>

> Updating one field in one document

```sh
> db.users.updateOne({_id: ObjectId("5f765f11224c6744c2b0a946")}, {$set: {hobbies: [{title: "Sports", frequency: 5}, {title: "Music", frequency: 7}, {title: "Football", frequency: 2}]}})
```

<br/>

> Updating one field in many documents

```sh
> db.users.updateMany({"hobbies.title": "Sports"}, {$set: {isSporty: true}})
```

<br/>

> Updating multiple fields in one document

```sh
> db.users.updateOne({_id: ObjectId("5f765f11224c6744c2b0a946")}, {$set: {age: 42, phone: 985348777}})
```

<br/>

### \$inc

<br/>

<p>
The $inc operator increments a field by a specified value.

The $inc operator accepts positive and negative values.
If the field does not exist, $inc creates the field and sets the field to the specified value.
Use of the $inc operator on a field with a null value will generate an error.
$inc is an atomic operation within a single document.

</p>

<br/>

```sh
> db.users.updateOne({name: "dummy2"}, {$inc: {age: 2}})
```

### \$min

<br/>

<p>
The $min updates the value of the field to a specified value if the specified value is less than the current value of the field. 
The $min operator can compare values of different types, using the BSON comparison order.

If the field does not exist, the \$min operator sets the field to the specified value.

</p>

<br/>

```sh
> db.users.updateOne({name: "dummy5"}, {$min: {age: 40}})
```

### \$max

<br/>

<p>
The $max operator updates the value of the field to a specified value if the specified value is greater than the current value of the field. The $max operator can compare values of different types, using the BSON comparison order.

If the field does not exist, the \$max operator sets the field to the specified value.

</p>

<br/>

```sh
> db.users.updateOne({name: "dummy2"}, {$max: {age: 40}})
```

### \$mull

<br/>

<p>
Multiply the value of a field by a number.

If the field does not exist in a document, \$mul creates the field and sets the value to zero of the same numeric type as the multiplier.

</p>

<br/>

```sh
> db.users.updateOne({name: "dummy2"}, {$mul: {age: 2}})
```

<br/>

### \$unset

<br/>

<p>
The $unset operator deletes a particular field.

If the field does not exist, then \$unset does nothing (i.e. no operation).

When used with $ to match an array element, $unset replaces the matching element with null rather than removing the matching element from the array. This behavior keeps consistent the array size and element positions.

</p>

<br/>

```sh
> db.users.updateMany({isSporty: true}, {$unset: {phone: ""}})
```

<br/>

### \$rename

<br/>

<p>
The $rename operator updates the name of a field.

The $rename operator logically performs an $unset of both the old name and the new name, and then performs a \$set operation with the new name. As such, the operation may not preserve the order of the fields in the document; i.e. the renamed field may move within the document.

If the document already has a field with the <newName>, the \$rename operator removes that field and renames the specified <field> to <newName>.

If the field to rename does not exist in a document, \$rename does nothing (i.e. no operation).

For fields in embedded documents, the $rename operator can rename these fields as well as move the fields in and out of embedded documents. $rename does not work if these fields are in array elements.

</p>

<br/>

```sh
> db.users.updateMany({}, {$rename: {age: "totalAge"}})
```

<br/>

### upsert()

<br/>

<p>
If updateOne(), updateMany(), or replaceOne() includes upsert : true and no documents match the specified filter, then the operation creates a new document and inserts it. If there are matching documents, then the operation modifies or replaces the matching document or documents.
</p>

<br/>

```sh
> db.users.updateOne({name: "dummy4"}, {$set: {age: 29, hobbies: [{title: "Nice dinning", frequency: 3}], isSporty: true}}, {upsert: true})
```

## Array Update Operators

### \$

<p>
The positional $ operator identifies an element in an array to update without explicitly specifying the position of the element in the array.

    * To project, or return, an array element from a read operation, see the $ projection operator instead.
    * To update all elements in an array, see the all positional operator $[] instead.
    * To update all elements that match an array filter condition or conditions, see the filtered positional operator instead $[<identifier>].

</p>

```sh
> db.users.updateMany({hobbies: {$elemMatch: {title: "Sports", frequency: {$gte: 3}}}}, {$set: {"hobbies.$.highFrequency": true}})
```

<br/>

### \$[]

<br/>

<p>
The all positional operator $[] indicates that the update operator should modify all elements in the specified array field.

If an upsert operation results in an insert, the query must include an exact equality match on the array field in order to use the \$[] positional operator in the update statement.

</p>

<br/>

```sh
> db.users.updateMany({totalAge: {$gt: 30}}, {$inc: {"hobbies.$[].frequency": -1}})
```

<br/>

### \$[<identifier>]

<br/>

<p>
The filtered positional operator $[<identifier>] identifies the array elements that match the arrayFilters conditions for an update operation, e.g. db.collection.update() and db.collection.findAndModify().
</p>

<br/>

```sh
> db.users.updateMany({"hobbies.frequency": {$gt: 2}}, {$set: {"hobbies.$[el].highFrequency": false}}, {arrayFilters: [{"el.frequency": {$gt: 2}} ]})
```

<br/>

### \$push

<br/>

<p>
The $push operator appends a specified value to an array.

f the field is absent in the document to update, \$push adds the array field with the value as its element.

If the field is not an array, the operation will fail.

If the value is an array, $push appends the whole array as a single element. To add each element of the value separately, use the $each modifier with \$push. For an example, see Append Multiple Values to an Array.

</p>

<br/>

```sh
> db.users.updateOne({name: "dummy4"}, {$push: {hobbies: {title: "Nice dinning", frequency: 2}}})
```

<br/>

### \$each

<br/>

<p>
The $each modifier is available for use with the $addToSet operator and the $push operator.

Use with the \$addToSet operator to add multiple values to an array <field> if the values do not exist in the <field>.

Use with the \$push operator to append multiple values to an array <field>.

The $push operator can use $each modifier with other modifiers.

</p>

<br/>

```sh
> db.users.updateOne({name: "dummy4"}, {$push: {hobbies: {$each: [{title: "nice dinning", frequency: 2}, {title: "Sports", frequency: 2}]}}})
```

<br/>

### \$sort

<br/>

<p>
The $sort modifier orders the elements of an array during a $push operation.

To use the $sort modifier, it must appear with the $each modifier. You can pass an empty array [] to the $each modifier such that only the $sort modifier has an effect.

The $sort modifier can sort array elements that are not documents. In previous versions, the $sort modifier required the array elements be documents.

If the array elements are documents, the modifier can sort by either the whole document or by a specific field in the documents. In previous versions, the \$sort modifier can only sort by a specific field in the documents.

Trying to use the $sort modifier without the $each modifier results in an error. The $sort no longer requires the $slice modifier.

</p>

<br/>

```sh
> db.users.updateOne({name: "dummy3"}, {$push: {hobbies: {$each: [{title: "Yoga", frequency: 2}, {title: "Dancing", frequency: 2}], $sort: {frequency: -1}}}})
```

<br/>

### \$slice

<br/>

<p>
The $slice modifier limits the number of array elements during a $push operation. To project, or return, a specified number of array elements from a read operation, see the $slice projection operator instead.

To use the $slice modifier, it must appear with the $each modifier. You can pass an empty array [] to the $each modifier such that only the $slice modifier has an effect.

The order in which the modifiers appear is immaterial. Previous versions required the $each modifier to appear as the first modifier if used in conjunction with $slice. For a list of modifiers available for \$push, see Modifiers.

Trying to use the $slice modifier without the $each modifier results in an error.

</p>

<br/>

```sh
> db.users.updateOne({name: "dummy3"}, {$push: {hobbies: {$each: [{title: "Yoga", frequency: 2}, {title: "Dancing", frequency: 2}], $sort: {frequency: -1}, $slice: 1}}})
```

<br/>

### \$pull

<br/>

<p>
The $pull operator removes from an existing array all instances of a value or values that match a specified condition.

If you specify a <condition> and the array elements are embedded documents, \$pull operator applies the <condition> as if each array element were a document in a collection. See Remove Items from an Array of Documents for an example.

If the specified <value> to remove is an array, \$pull removes only the elements in the array that match the specified <value> exactly, including order.

If the specified <value> to remove is a document, \$pull removes only the elements in the array that have the exact same fields and values. The ordering of the fields can differ.

</p>

<br/>

```sh
> db.users.updateOne({name: "dummy5"}, {$pull: {hobbies: {title: "Music"}}})
```

### \$sort

<br/>

<p>
The $pop operator removes the first or last element of an array. Pass $pop a value of -1 to remove the first element of an array and 1 to remove the last element in an array.

The \$pop operation fails if the <field> is not an array.

If the \$pop operator removes the last item in the <field>, the <field> will then hold an empty array.

</p>

<br/>

```sh
> db.users.updateOne({name: "dummy4"}, {$pop: {hobbies: 1}})
```

<br/>

### $addToSet

<p>
The $addToSet operator adds a value to an array unless the value is already present, in which case $addToSet does nothing to that array.
</p>

<br/>

```sh
> db.users.updateOne({name: "dummy4"}, {$addToSet: {hobbies: {title: "Hiking", frequency: 2}}})
```

