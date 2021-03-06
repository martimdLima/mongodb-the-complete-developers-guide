# Read Operations

<br/>

# Query & Projection Operators

<br/>

## Comparison Query Operators

<br/>

> Purpose - Locate Data

<br/>

> Types - Comparison, Evalution, Logical, Array, Element, Comments, Geospatial

<br/>

## Understanding findOne() and find()

<br/>

> findOne()

<br/>

> returns the first document that meets the criteria

<br/>

```sh
> db.movies.findOne()
```

<br/>

> using a filter

<br/>

```sh
> db.movies.findOne({name: "The Last Ship"})
```

<br/>

> find()

<br/>

> find() returns a cursor

<br/>

```sh
> db.movies.find()
```

<br/>

> using a filter

<br/>

```sh
> db.movies.find({name: "The Last Ship"})
> db.movies.find({runtime: 60})
```

<br/>

## Working with Comparison Operators

<br/>

> finding values that are equal to the specified value

<br/>

```sh
> db.movies.find({runtime: {$eq: 70}})
```

<br/>

> finding values that aren't equal to the specified value

<br/>

```sh
> db.movies.find({runtime: {$ne: 70}})
```

<br/>

> finding values that are lower than the specified value

<br/>

```sh
> db.movies.find({runtime: {$lt: 40}})
```

<br/>

> finding values that are greater than the specified value

<br/>

```sh
> db.movies.find({runtime: {$gt: 40}})
```

<br/>

> finding values that are lower or equal than the specified value

<br/>

```sh
> db.movies.find({runtime: {$lte: 40}})
```

<br/>

> finding values that are greater or equal than the specified value

<br/>

```sh
> db.movies.find({runtime: {$gte: 40}})
```

<br/>

## Querying Embedded Fields & Arrays

<br/>

> querying all documents that have the field average embedded in the rating field

<br/>

```sh
> db.movies.find({"rating.average": {$gt: 7.0}})
```

<br/>

> querying all documents that have the genre drama/Drama value in the field genre which is an array

<br/>

```sh
> db.movies.find({genres: "drama"})
> db.movies.find({genres: "Drama"})
```

<br/>

> querying all documents that have and genre array equaling Drama

<br/>

```sh
> db.movies.find({genres: ["Drama"]}).pretty()
```

<br/>

### Understanding "$in" and "$nin"

<br/>

> The $in operator selects the documents where the value of a field equals any value in the specified array.

<br/>

```sh
> db.movies.find({runtime: {$in: [30, 42]}})

<br/>

```
> $nin selects the documents where:
>    * the field value is not in the specified array or
>    * the field does not exist.

<br/>

```sh
> db.movies.find({runtime: {$nin: [30, 42]}})
```

<br/>

## Comparison Query Operators

<br/>

### "$or" and "$nor"

<br/>

> "$or"
> The $or operator performs a logical OR operation on an array of two or more <expressions> 
> and selects the documents that satisfy at least one of the <expressions>. 

<br/>

```sh
> db.movies.find(
    {$or: [
        {"rating.average": {$lt: 5}}, 
        {"rating.average": {$gt: 9.3}}
          ]
    }
)
```

<br/>

> "$nor"
> $nor performs a logical NOR operation on an array of one or more query expression 
> and selects the documents that fail all the query expressions in the array. 

<br/>

```sh
> db.movies.find(
    {$nor: [
        {"rating.average": {$lt: 5}}, 
        {"rating.average": {$gt: 9.3}}
          ]
    }
)
```

<br/>

### "$and"

<br/>

> $and performs a logical AND operation on an array of one or more expressions (e.g. <expression1>, <expression2>, etc.) 
> and selects the documents that satisfy all the expressions in the array. The $and operator uses short-circuit evaluation. 
> If the first expression (e.g. <expression1>) evaluates to false, MongoDB will not evaluate the remaining expressions.

<br/>

```sh
> db.movies.find(
    {$and: [
        {"rating.average": {$gt: 9}}, 
        {genres: "Drama"}
          ]
    }
)
```

<br/>

### "$not"

> $not performs a logical NOT operation on the specified <operator-expression> and 
> selects the documents that do not match the <operator-expression>. 
> This includes documents that do not contain the field.

<br/>

```sh
> db.movies.find(
    {
        runtime: {$not: {$eq: 60}}
    }
)
```

<br/>

## Element Query Operators

<br/>

### "$exists"

<br/>

> When <boolean> is true, $exists matches the documents that contain the field, including documents where the field value is null. 
> If <boolean> is false, the query returns only the documents that do not contain the field.

<br/>

```sh
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

<br/>

### "$type"

<br/>

> $type selects documents where the value of the field is an instance of the specified BSON type(s). 
> Querying by data type is useful when dealing with highly unstructured data where data types are not predictable.

<br/>

```sh
> db.users.find({phone: {$type: "number"}})
>
> db.users.find({phone: {$type: "string"}})
>
> db.users.find({phone: {$type: ["double", "string"]}})
```

<br/>

## Evaluation Query Operators

<br/>

### "$regex"

> Provides regular expression capabilities for pattern matching strings in queries. 
> MongoDB uses Perl compatible regular expressions (i.e. “PCRE” ) version 8.42 with UTF-8 support.
>
> In MongoDB, you can also use regular expression objects (i.e. /pattern/) to specify regular expressions.

```sh
> db.movies.find({summary: {$regex: /musical/}})
```

<br/>

### "$expr"

<br/>

> Allows the use of aggregation expressions within the query language.
> The arguments can be any valid aggregation expression.

<br/>

> find all documents where the field volume is bigger than the field target

<br/>

```sh
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

<br/>

> find all documents that meet the condition

<br/>

```sh
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

<br/>

## Array Query Operators

<br/>

### "$size"

<br/>

> The $size operator matches any array with the number of elements specified by the argument.
>
> $size does not accept ranges of values. To select documents based on fields with different numbers of elements, 
> create a counter field that you increment when you add elements to a field.

<br/>

```sh
> db.users.find({hobbies: {$size: 3}})
```

<br/>

### "$all"

<br/>

> The $all operator selects the documents where the value of a field is an array that contains all the specified elements.

<br/>

```sh
> db.movieStats.find({genre: {$all: ["action", "thriller"]}})
```

<br/>

### "$elemMatch"

<br/>

> The $elemMatch operator matches documents that contain an array field with at least one element that matches all the specified query criteria.
> If you specify only a single <query> condition in the $elemMatch expression, and are not using the $not or $ne operators inside of $elemMatch, 
> $elemMatch can be omitted. See Single Query Condition.

<br/>

```sh
> db.users.find({hobbies: {$elemMatch: {title: "Sports", frequency: {$gte: 3}}}}).pretty()
```

<br/>

## Cursors

<br/>

### next()

<br/>

> Returns the next document in a cursor.

<br/>

```sh
> use movieData

> const dataCursor = db.movies.find()

> dataCursor.next()
```

<br/>

### hasNext()

<br/>

> Returns true if the cursor has documents and can be iterated.

<br/>

```sh
> dataCursor.hasNext()
```

<br/>

### forEach()

<br/>

> Applies a JavaScript function for every document in a cursor.

<br/>

```sh
> dataCursor.forEach(doc => {
    printjson(doc)
})
```

<br/>

### sort()

<br/>

> Returns results ordered according to a sort specification.
>
> lower to higher: 1
>
> higher to lower: -1

<br/>

```sh
> db.movies.find().sort({"ratings.average": 1}).pretty()

> db.movies.find().sort({"ratings.average": 1, runtime: -1}).pretty()
```

<br/>

### skip()

<br/>

> Returns a cursor that begins returning results only after passing or skipping a number of documents.

<br/>

```sh
> db.movies.find().sort({"ratings.average": 1, runtime: -1}).skip(10).pretty()
```

<br/>

### limit()

<br/>

> Constrains the size of a cursor’s result set.

<br/>

```sh
> db.movies.find().sort({"ratings.average": 1, runtime: -1}).skip(10).limit(10).pretty()
```

<br/>

## Projection Operators

<br/>

### $ (projection)

<br/>

<p>
The positional $ operator limits the contents of an <array> to return either:

    The first element that matches the query condition on the array.
    The first element if no query condition is specified for the array (Starting in MongoDB 4.4).
<br/>
Use $ in the projection document of the find() method or the findOne() method when you only need one particular array element in selected documents.
</p>

<br/>

```sh
> db.movies.find({}, {name: 1, genres: 1, runtime: 1, rating: 1}).pretty()

> db.movies.find({genres: "Drama"}, {"genres.$": 1}).pretty()

> db.movies.find({genres: {$all: ["Drama", "Horror"]}}, {"genres.$": 1})
```

<br/>

### $elemMatch (projection)

<br/>

<p>
The $elemMatch operator limits the contents of an <array> field from the query results to contain only the first element matching the $elemMatch condition
</p>

<br/>

```sh
> db.movies.find({genres: "Drama"}, {genres: {$elemMatch: {$eq: "Horror"}}}).pretty()
```

<br/>

### $slice (projection)

<br/>

<p> 
The $slice projection operator specifies the number of elements in an array to return in the query result.
</p>

<br/>

<p> 
$slice: <number>
    * Specify a positive number n to return the first n elements.
    * Specify a negative number n to return the last n elements.
</p> 

<br/>

```sh
> db.movies.find({"rating.average": {$gt: 9}}, {genres: {$slice: 2}, name: 1})
```

<br/>

<p> 
$slice: [ < number to skip >, < number to return > ]
Specifies the number of elements to return in the <arrayField> after skipping the specified number of elements starting from the first element. 
You must specify both elements.
</p>

```sh
> db.movies.find({"rating.average": {$gt: 9}}, {genres: {$slice: [1, 2]}, name: 1})
```