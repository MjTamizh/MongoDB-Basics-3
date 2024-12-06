# MongoDB-Basics

## MongoDB Basic Course - 1 part -3
  1. Aggregation Stages
  2. CRUD options

  3.
### Aggregation Stages, pagination  

#### Sort skip limit with find

```bash
db.students.find().sort({ age: 1 }).skip(1).limit(2);       
```
#### Sort skip limit with aggregate

```bash
db.students.aggregate([ { $sort: { age: 1 } },{ $skip: 1 },{ $limit: 2 } ]);
```

### CRUD Operations with Special Options

#### Ordered
```bash
db.students.insertMany(
  [
    { _id: 1, name: "Tamil", scores: [85, 90, 95] },
    { _id: 2, name: "Karthick", scores: [75, 80, 85] },
    { _id: 1, name: "Dhanush", scores: [65, 70, 75] } 
  ],
  { ordered: false } 
```

#### Slice

```bash
db.students.find( { name: "Dhanush" },  { scores: { $slice: 2 } } );
```

#### Upsert

```bash
db.students.updateOne(
  { name: "David" },  { $set: { scores: [80, 85, 90] } },  { upsert: true }
);
```

### Operators
  #### Comparison Operators (eq,ne,gt,get,lt,let,in,nin)
  
   ```bash
  db.students.find({ age: { $eq: 18 } });
  ```
  ```bash
db.students.find({ age: { $ne: 18 } });
  ```
   ```bash
db.students.find({ marks: { $gt: 80 } });

  ```
 ```bash
db.students.find({ marks: { $gte: 85 } });

```
 ```bash
db.students.find({ age: { $lt: 20 } });
  ```
```bash
db.students.find({ marks: { $lte: 75 } });
  ```
```bash
 db.students.find({ marks: { $in: [72, 90] } });
  ```

```bash
db.students.find({ marks: { $nin: [72, 90] } });
```


  #### Logical Operators (and,or,nor,not )


```bash
db.students.find({ $and: [{age: 18 },{marks:{$gt:80}}] });
```
```bash
db.students.find({ $or: [ { age: 18 }, { marks: { $gt: 80 } } ] });
```
```bash
db.students.find({ $nor: [ { age: 18 }, { marks: { $gt: 80 } } ] });
```
```bash
db.students.find({ marks: { $not: { $gt: 80 } } });
```

#### Combining Logical Operators

```bash
db.students.find({ $or: [ { age: 18 }, { marks: { $gt: 85 } } ], $not: { marks: { $lt: 75 } } });
```
```bash
db.students.find({age: { $ne: 18 }, $or: [ { marks: { $gte: 72 } }, { marks: { $gt: 80 } } ] });
```

#### Element Operators (exists, type, JSON schema,expr)

```bash
db.students.find({ age: { $exists: true } });
```
```bash
db.students.find({ marks: { $type: "int" } });
```

```bash
db.students.find({ marks: { $jsonSchema: { bsonType: "int", minimum: 80 } } });
```

```bash
db.students.find({ $expr: { $gt: ["$highestMark", { $avg: "$marks" }] } });
```
```bash
db.students.find({ $expr: { $eq: ["$highestMark", { $max: "$marks" }] } });

```
```bash
db.students.find({ $expr: { $and: [{ $gt: ["$highestMark", 85] }, { $lt: ["$age", 19] }] } });

```
```bash
db.students.find({ $expr: { $or: [{ $gt: ["$highestMark", 90] }, { $eq: ["$age", 20] }] } });

```
```bash
db.students.find({ $expr: { $gt: [{ $arrayElemAt: ["$marks", 0] }, 70] } });

```
```bash
db.students.find({ $expr: { $not: { $eq: ["$highestMark", { $max: "$marks" }] } } });

```

#### Array Operators (elemMatch,all,size,push,addToSet,pop,pull,unwind)

```bash
db.students.find({ marks: { $elemMatch: { $gt: 80, $lt: 90 } } });

```
```bash
db.students.find({ marks: { $all: [75, 85] } });
```

```bash
db.students.find({ marks: { $size: 3 } });
```
```bash
db.students.updateOne({ name: "Alice" }, { $push: { marks: 95 } });
```
```bash
db.students.updateOne({ name: "Alice" }, { $addToSet: { marks: 95 } });
```

```bash
db.students.updateOne({ name: "Alice" }, { $pop: { marks: 1 } });
```
```bash
db.students.updateOne({ name: "Alice" }, { $pop: { marks: -1 } });
```
```bash
db.students.updateOne({ name: "Alice" }, { $pull: { marks: { $lt: 80 } } });
```

```bash
db.students.aggregate([ { $unwind: "$marks" } ]);
```
