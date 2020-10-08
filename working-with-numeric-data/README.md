# Working With Numeric Data

<br/>

<p> MongoDB Numeric Data types supported:</p>

* **double**
* **32-bit integer**
* **64-bit integer**
* **decimal**

<br/>

## Integer

<br/>

<p>Simple plain numbers without decimal points, 32 bytes or 64 bytes, are saved as Int and retrieves as same.</p>

<br/>

### **32-bit integer**

<br/>

```sh
> db.persons.insertOne({age: NumberInt(32)}) 

> optimal method
> db.persons.insertOne({age: NumberInt("32")}) 
```

<br/>

### **64-bit integer**

<br/>

```sh
> db.companies.insertOne({valuation: NumberLong(5000000000)})

> optimal method
> db.companies.insertOne({valuation: NumberLong("5000000000")})
```

<br/>

## Doing Maths with Floats int32s & int64s

<br/>

```sh
> db.accounts.insertOne({name: "testUser", amount: NumberInt("10")}) 

> db.accounts.updateOne({}, {$inc:  {amount: NumberInt("10")}}) 
```

<br/>

```sh
> db.companies.insertOne({valuation: NumberLong("25432000000")}) 

> db.companies.updateOne({}, {$inc:  {valuation: NumberLong("360555")}}) 
```

<br/>

## Double

<br/>

<p>Double is implemented for storing floating-point data in MongoDB.</p>

<br/>

```sh
> db.science.insertOne({a: NumberDecimal("3.141592"), b: NumberDecimal("3.141")})

> db.science.aggregate([{$project: {result: {$subtract: ["$a", "$b"]}}}])
```