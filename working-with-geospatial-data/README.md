# Working with GeoSpatial Data

<p>
To specify GeoJSON data, use an embedded document with:
</p>

* a field named type that specifies the GeoJSON object type and

* a field named coordinates that specifies the object’s coordinates.<br/>
    If specifying latitude and longitude coordinates, list the longitude first and then latitude:
    * Valid longitude values are between -180 and 180, both inclusive.
    * Valid latitude values are between -90 and 90, both inclusive.


## Creating GeoJSON Data

```sh
> use awesomeplaces

> db.places.insertOne({name: "Torre dos Clerigos", location: { type: "Point", coordinates: [ -8.6167872, 41.1456793 ] }})
```

## Adding a Geospatial Index

```sh
> db.places.createIndex({location: "2dsphere"})
```

## Running Geo Queries

```sh
> db.places.find({location: {$near: {$geometry: {type: "Point", coordinates: [-8.6188578, 41.1458833]}}}})

> db.places.find({location: {$near: {$geometry: {type: "Point", coordinates: [-8.6188578, 41.1458833]}, $maxDistance: 200, $minDistance: 100 }}}).pretty()

```

## Adding additional locations

```sh
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

## Finding if a user inside a specific area

> db.areas.insertOne({name: "Porto", area: { type: "Polygon", coordinates: [[p1, p2, p3, p4, p1]] }})

> db.areas.createIndex({area: "2dsphere"})

> db.areas.find({area: {$geoIntersects: {$geometry: {type: "Point", coordinates: [ -8.62224, 41.15858 ] }}}}) 

## Finding Places Within a Certain Radius

> db.places.find({location: {$geoWithin: {$centerSphere: [[-8.62224, 41.15858], 1 / 6378.1] }}})

