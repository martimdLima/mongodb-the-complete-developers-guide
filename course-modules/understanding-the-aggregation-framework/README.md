# Aggregation Framework

<p>
Aggregation operations process data records and return computed results. 
Aggregation operations group values from multiple documents together, and can perform a variety of operations on the grouped data to return a single result. 
MongoDB provides three ways to perform aggregation: the aggregation pipeline, the map-reduce function, and single purpose aggregation methods.
</p>


## Pipeline Concept

<p>
In UNIX command, shell pipeline means the possibility to execute an operation on some input and use the output as the input for the next command and so on. MongoDB also supports same concept in aggregation framework. There is a set of possible stages and each of those is taken as a set of documents as an input and produces a resulting set of documents (or the final resulting JSON document at the end of the pipeline). This can then in turn be used for the next stage and so on.
</p>

<p>
Following are the possible stages in aggregation framework:
</p>

* $project − Used to select some specific fields from a collection.
* $match − This is a filtering operation and thus this can reduce the amount of documents that are given as input to the next stage
* $group − This does the actual aggregation as discussed above.
* $sort − Sorts the documents.
* $skip − With this, it is possible to skip forward in the list of documents for a given amount of documents.
* $limit − This limits the amount of documents to look at, by the given number starting from the current positions.
* $unwind − This is used to unwind document that are using arrays. When using an array, the data is kind of pre-joined and this operation will be undone with this to have individual documents again. Thus with this stage we will increase the amount of documents for the next stage.


## Group Stage

<p>
Groups input documents by the specified _id expression and for each distinct grouping, outputs a document. 
The _id field of each output document contains the unique group by value. 
The output documents can also contain computed fields that hold the values of some accumulator expression.

</p>

```sh
> mongoimport persons.json -d analytics -c persons --jsonArray

> db.persons.aggregate(
    [
        { $match: {gender: "female"}}
    ]
).pretty()
```

```sh
> db.persons.aggregate(
    [
        { $match: {gender: "female"}},
        { $group: { _id: { state: "$location.state" }, totalPersons: { $sum: 1} }}
    ]
).pretty()

> db.persons.find({"location.state": "sinop"}).pretty()
```

```sh
> db.persons.aggregate(
    [
        { $match: {gender: "female"}},
        { $group: { _id: { state: "$location.state" }, totalPersons: { $sum: 1} }},
        { $sort: { totalPersons: -1 }}
    ]
).pretty()
```

## Project Stage

```sh
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

## Turning the Location Into a geoJSON Object


```sh
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


## Transforming the Birthdate

```sh
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

## Using Shortcuts for Transformations

```sh
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


## Understanding the $isoWeekYear Operator

```sh
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

## Pushing Elements Into Newly Created Arrays

```sh
> mongoimport array-data.json -d analytics -c friends --jsonArray

> db.friends.aggregate(
    [
        { $group: { _id: { age: "$age"}, allHobbies: { $push: "$hobbies" } } }
    ]
).pretty()

```


## unWind Stage

```sh
> db.friends.aggregate(
    [
        { $unwind: "$hobbies"},
        { $group: { _id: { age: "$age"}, allHobbies: { $push: "$hobbies" } } }
    ]
).pretty()
```

## addToSet Stage

```sh
> db.friends.aggregate(
    [
        { $unwind: "$hobbies"},
        { $group: { _id: { age: "$age"}, allHobbies: { $addToSet: "$hobbies" } } }
    ]
).pretty()
```

## slice operator

```sh
> db.friends.aggregate(
    [
        { $project: { _id: 0, examScore: { $slice: ["$examScores", 2, 1]}}}
    ]
).pretty()
```

## $slice operator

```sh
> db.friends.aggregate(
    [
        { $project: { _id: 0, numScores: { $size: "$examScores"}}}
    ]
).pretty()
```

## $filter operator

```sh
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

## Aplying Multiple Operations to an array

```sh
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

## $bucket Stage

```sh
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

## Aditional Stages

```sh
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

## Writing Pipelines into a new collection

```sh
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

## #geoNear Stage

```sh
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


