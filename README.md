# MongoDB-Basics

## MongoDB Basic Course - 1 part -3
  1. Aggregation Stages
  2. CRUD options
  3. Relations



     
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
  { name: "Dhanush" },  { $set: { scores: [80, 85, 90] } },  { upsert: true }
);
```

### Relations
  ####  One-to-One Relationship
  
   ```bash
 db.employee.aggregate([
  {
    $lookup: {
      from: "profile",         // Foreign collection name
      localField: "profile_id", // Field in `employee`
      foreignField: "_id",      // Field in `profile`
      as: "profileDetails"      // Output array field
    }
  }
]);

  ```

####  One-to-Many Relationship
  ```bash
db.company.aggregate([
  {
    $lookup: {
      from: "employee",          // Foreign collection
      localField: "_id",         // Field in `company`
      foreignField: "company_id", // Field in `employee`
      as: "employees"            // Output field
    }
  },
  {
    $lookup: {
      from: "profile",            // Foreign collection
      localField: "employees._id", // Array field in `employees`
      foreignField: "employee_id", // Field in `profile`
      as: "profiles"              // Output field
    }
  }
]);

  ```

####  Many-to-Many Relationship
   ```bash
db.company.aggregate([
  {
    $lookup: {
      from: "employee",
      localField: "_id",
      foreignField: "company_id",
      as: "employees"
    }
  },
  {
    $unwind: "$employees" // Unwind employees to create one-to-one pairs
  },
  {
    $lookup: {
      from: "profile",
      localField: "employees._id",
      foreignField: "employee_id",
      as: "employeeProfile"
    }
  },
  {
    $unwind: "$employeeProfile" // Unwind profiles
  },
  {
    $group: {
      _id: "$_id",
      companyName: { $first: "$name" },
      employees: {
        $push: {
          employeeName: "$employees.name",
          profile: "$employeeProfile"
        }
      }
    }
  }
]);


  ```




 
