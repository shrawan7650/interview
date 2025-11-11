# MongoDB Interview Preparation Guide

> **Complete roadmap: Beginner → Intermediate → Advanced → Coding → Tricky Questions**

---

## 🎯 LEVEL 1: BASICS (Foundation Strong Karo)

### Q1: MongoDB kya hai? SQL se kaise different hai?

**Answer:**
MongoDB ek NoSQL, document-oriented database hai jo data ko JSON-like format (BSON) mein store karta hai.

**Deep Explanation:**
- **SQL (Relational)**: Tables, Rows, Columns, Fixed Schema
- **MongoDB (NoSQL)**: Collections, Documents, Fields, Flexible Schema

**Key Differences:**

| Feature | SQL | MongoDB |
|---------|-----|---------|
| Data Structure | Tables (rows & columns) | Collections (documents) |
| Schema | Fixed, predefined | Flexible, dynamic |
| Relations | Foreign Keys, JOINs | Embedded docs or References |
| Scalability | Vertical (more CPU/RAM) | Horizontal (sharding) |
| Query Language | SQL | MongoDB Query Language |

**Example:**
```javascript
// SQL Table
Users Table:
| id | name  | email            |
|----|-------|------------------|
| 1  | Rahul | rahul@email.com  |

// MongoDB Document
{
  "_id": ObjectId("507f1f77bcf86cd799439011"),
  "name": "Rahul",
  "email": "rahul@email.com",
  "address": {  // Nested data easily store kar sakte ho
    "city": "Mumbai",
    "pincode": 400001
  }
}
```

**Interview Tip:** Always mention BSON (Binary JSON) - ye batao ki MongoDB internally BSON use karta hai jo JSON se faster aur more data types support karta hai.

---

### Q2: Collection aur Document kya hota hai?

**Answer:**
- **Collection**: SQL ke table ki tarah hota hai, documents ka group
- **Document**: SQL ke row ki tarah, ek record jo JSON format mein hota hai

**Deep Explanation:**
```javascript
// Collection: "users"
// Document 1:
{
  "_id": ObjectId("..."),
  "name": "Priya",
  "age": 25,
  "skills": ["Node.js", "React"]
}

// Document 2:
{
  "_id": ObjectId("..."),
  "name": "Amit",
  "age": 30,
  "skills": ["Python", "Django"],
  "city": "Delhi"  // Flexible schema - extra field add kar sakte ho
}
```

**Key Point:** Same collection mein different structure ke documents rakh sakte ho (schema flexibility).

---

### Q3: CRUD Operations kaise karte hain?

**Answer:**
CRUD = Create, Read, Update, Delete

**1. CREATE (Insert)**
```javascript
// Single document insert
db.users.insertOne({
  name: "Rohan",
  email: "rohan@test.com",
  age: 28
})

// Multiple documents insert
db.users.insertMany([
  { name: "Sneha", age: 22 },
  { name: "Vikram", age: 35 }
])
```

**2. READ (Find/Query)**
```javascript
// Sab documents
db.users.find()

// Specific query
db.users.find({ age: { $gte: 25 } })  // age >= 25

// Single document
db.users.findOne({ name: "Rohan" })

// Projection (specific fields chahiye)
db.users.find({}, { name: 1, email: 1, _id: 0 })
```

**3. UPDATE**
```javascript
// Single document update
db.users.updateOne(
  { name: "Rohan" },
  { $set: { age: 29 } }
)

// Multiple documents update
db.users.updateMany(
  { age: { $lt: 25 } },
  { $set: { status: "young" } }
)

// Replace entire document
db.users.replaceOne(
  { name: "Rohan" },
  { name: "Rohan", age: 30, email: "new@test.com" }
)
```

**4. DELETE**
```javascript
// Single document delete
db.users.deleteOne({ name: "Rohan" })

// Multiple documents delete
db.users.deleteMany({ age: { $lt: 20 } })
```

**Interview Tip:** `updateOne` vs `replaceOne` ka difference batao - updateOne sirf specified fields change karta hai, replaceOne pura document replace kar deta hai.

---

### Q4: _id field kya hai? ObjectId kya hota hai?

**Answer:**
`_id` har document ka unique identifier hai, automatically MongoDB generate karta hai agar aap nahi dete.

**Deep Explanation:**
```javascript
ObjectId("507f1f77bcf86cd799439011")
//        |-------||-|--|--------|
//        timestamp|m|p|  counter
//        (4 bytes) |5|2| (3 bytes)
//                  |bytes|
//                  machine|process
```

**Components:**
- **4 bytes**: Timestamp (creation time)
- **5 bytes**: Random value (machine + process identifier)
- **3 bytes**: Counter (incrementing)

**Why ObjectId?**
- Globally unique
- Indexable
- Sortable by creation time
- Distributed system mein bhi collision nahi hota

```javascript
// Custom _id bhi de sakte ho
db.users.insertOne({
  _id: "user_12345",  // String bhi ho sakta hai
  name: "Test"
})
```

**Interview Tip:** Batao ki ObjectId distributed systems ke liye perfect hai kyunki central ID generator ki zaroorat nahi padti.

---

### Q5: Query Operators explain karo

**Answer:**
MongoDB mein query operators se complex conditions likhte hain.

**Comparison Operators:**
```javascript
// $eq (equal)
db.users.find({ age: { $eq: 25 } })  // age === 25

// $ne (not equal)
db.users.find({ status: { $ne: "inactive" } })

// $gt, $gte (greater than, greater than or equal)
db.users.find({ age: { $gte: 18 } })

// $lt, $lte (less than, less than or equal)
db.users.find({ age: { $lt: 30 } })

// $in (value in array)
db.users.find({ city: { $in: ["Mumbai", "Delhi", "Bangalore"] } })

// $nin (value not in array)
db.users.find({ status: { $nin: ["banned", "deleted"] } })
```

**Logical Operators:**
```javascript
// $and
db.users.find({
  $and: [
    { age: { $gte: 18 } },
    { city: "Mumbai" }
  ]
})

// $or
db.users.find({
  $or: [
    { age: { $lt: 18 } },
    { age: { $gt: 60 } }
  ]
})

// $not
db.users.find({ age: { $not: { $gte: 18 } } })

// $nor (neither this nor that)
db.users.find({
  $nor: [
    { status: "banned" },
    { verified: false }
  ]
})
```

**Array Operators:**
```javascript
// $all (array mein sab values honi chahiye)
db.users.find({ skills: { $all: ["JavaScript", "MongoDB"] } })

// $elemMatch (array element condition match kare)
db.users.find({
  scores: { $elemMatch: { $gte: 80, $lt: 90 } }
})

// $size (array length)
db.users.find({ skills: { $size: 3 } })
```

**Element Operators:**
```javascript
// $exists (field exists ya nahi)
db.users.find({ phone: { $exists: true } })

// $type (field ka data type check)
db.users.find({ age: { $type: "number" } })
```

---

### Q6: Indexing kya hai? Kyun zaruri hai?

**Answer:**
Index ek data structure hai jo queries ko fast banata hai, jaise book mein index hota hai.

**Deep Explanation:**
Bina index ke MongoDB ko har document scan karna padta hai (Collection Scan). Index se directly data mil jata hai.

**Example:**
```javascript
// Bina Index (SLOW - 1 million documents scan)
db.users.find({ email: "test@email.com" })
// Time: ~500ms

// Index create karo
db.users.createIndex({ email: 1 })  // 1 = ascending, -1 = descending

// Ab same query (FAST - direct lookup)
db.users.find({ email: "test@email.com" })
// Time: ~5ms
```

**Index Types:**

**1. Single Field Index**
```javascript
db.users.createIndex({ email: 1 })
```

**2. Compound Index (Multiple fields)**
```javascript
db.users.createIndex({ city: 1, age: -1 })
// city ascending, age descending
```

**3. Multikey Index (Array fields)**
```javascript
db.users.createIndex({ skills: 1 })
// skills array ke har element pe index
```

**4. Text Index (Full-text search)**
```javascript
db.articles.createIndex({ content: "text" })
db.articles.find({ $text: { $search: "mongodb tutorial" } })
```

**5. Unique Index**
```javascript
db.users.createIndex({ email: 1 }, { unique: true })
// Email duplicate nahi ho sakta
```

**Trade-offs:**
- **Pros**: Fast queries, sorting, searching
- **Cons**: Extra storage, slow writes (insert/update mein index update karna padta hai)

**Interview Tip:** Batao ki "too many indexes" bhi problem hai - memory waste hota hai aur writes slow ho jate hain. Sirf frequently queried fields pe index banao.

---

### Q7: Schema Design - Embedded vs Referenced Documents?

**Answer:**
MongoDB mein data model karne ke 2 ways hain:
1. **Embedded Documents** (nested data)
2. **Referenced Documents** (foreign keys ki tarah)

**1. EMBEDDED Documents (Recommended for 1-to-Few)**
```javascript
// User with embedded address
{
  _id: ObjectId("..."),
  name: "Rahul",
  email: "rahul@test.com",
  address: {  // Embedded document
    street: "MG Road",
    city: "Pune",
    pincode: 411001
  },
  orders: [  // Embedded array
    { orderId: "ORD001", amount: 500 },
    { orderId: "ORD002", amount: 1200 }
  ]
}
```

**When to Use Embedded:**
- Data frequently together access hota hai
- One-to-Few relationship (limited embedded data)
- Data update rarely hota hai
- Single query mein sab data chahiye

**2. REFERENCED Documents (Recommended for 1-to-Many)**
```javascript
// Users Collection
{
  _id: ObjectId("user123"),
  name: "Priya",
  email: "priya@test.com"
}

// Orders Collection
{
  _id: ObjectId("order456"),
  userId: ObjectId("user123"),  // Reference
  product: "Laptop",
  amount: 50000
}

// Query with lookup (JOIN ki tarah)
db.users.aggregate([
  {
    $lookup: {
      from: "orders",
      localField: "_id",
      foreignField: "userId",
      as: "userOrders"
    }
  }
])
```

**When to Use Referenced:**
- One-to-Many relationship (bahut saare related docs)
- Data independently access hota hai
- Data frequently update hota hai
- Document size 16MB limit cross kar sakta hai

**Real-world Example:**
```javascript
// Blog System

// EMBEDDED (Comments limited hain)
{
  _id: ObjectId("post123"),
  title: "MongoDB Tutorial",
  content: "...",
  comments: [  // Max 50-100 comments ho sakte hain
    { user: "Amit", text: "Great post!" },
    { user: "Sneha", text: "Very helpful" }
  ]
}

// REFERENCED (Users alag manage karni hain)
// Posts Collection
{
  _id: ObjectId("post123"),
  title: "MongoDB Tutorial",
  authorId: ObjectId("user456")  // Reference
}

// Users Collection
{
  _id: ObjectId("user456"),
  name: "Vikram",
  email: "vikram@test.com"
}
```

**Interview Tip:** 16MB document limit ko zaroor mention karo. Embedded data agar unlimited grow kar sakta hai toh Referenced use karo.

---

## ✅ Checkpoint 1: Basic Level Complete!

**Key Topics Covered:**
✅ MongoDB vs SQL
✅ Collections & Documents
✅ CRUD Operations
✅ ObjectId
✅ Query Operators
✅ Indexing Basics
✅ Schema Design (Embedded vs Referenced)

---

## 🚀 Ready for LEVEL 2: INTERMEDIATE?

**Next Topics:**
- Aggregation Pipeline (powerful queries)
- Advanced Indexing Strategies
- Data Modeling Patterns
- Performance Optimization
- Relationships in Detail
- Update Operators ($set, $push, $pull, etc.)

**Type "next" to continue to Intermediate Level!**

---

## 📊 LEVEL 2: INTERMEDIATE (Ab Real Power Dekhenge)

### Q8: Aggregation Pipeline kya hai? find() se kaise different?

**Answer:**
Aggregation Pipeline ek framework hai jo data ko multi-stage process karke complex queries execute karta hai. find() sirf documents return karta hai, aggregation transform, group, calculate kar sakta hai.

**Deep Explanation:**
Pipeline mein multiple stages hote hain jo sequentially execute hote hain:
```
Input Documents → Stage 1 → Stage 2 → Stage 3 → Output
```

**Basic Aggregation Example:**
```javascript
db.orders.aggregate([
  { $match: { status: "completed" } },        // Stage 1: Filter
  { $group: { _id: "$userId", total: { $sum: "$amount" } } },  // Stage 2: Group
  { $sort: { total: -1 } }                    // Stage 3: Sort
])
```

**Common Stages:**

**1. $match (Filter karo - find ki tarah)**
```javascript
db.users.aggregate([
  { $match: { age: { $gte: 25 } } }
])
```

**2. $project (Select specific fields)**
```javascript
db.users.aggregate([
  {
    $project: {
      name: 1,
      email: 1,
      fullName: { $concat: ["$firstName", " ", "$lastName"] }
    }
  }
])
```

**3. $group (Data ko group karke calculations)**
```javascript
// Category-wise total sales
db.products.aggregate([
  {
    $group: {
      _id: "$category",
      totalSales: { $sum: "$price" },
      avgPrice: { $avg: "$price" },
      count: { $sum: 1 }
    }
  }
])
```

**4. $sort (Sort results)**
```javascript
db.users.aggregate([
  { $sort: { age: -1 } }  // Descending order
])
```

**5. $limit & $skip (Pagination)**
```javascript
db.users.aggregate([
  { $skip: 10 },   // First 10 skip
  { $limit: 5 }    // Next 5 docs
])
```

**6. $lookup (JOIN - Referenced documents)**
```javascript
// Users aur unke orders join
db.users.aggregate([
  {
    $lookup: {
      from: "orders",           // Join with orders collection
      localField: "_id",        // users._id
      foreignField: "userId",   // orders.userId
      as: "userOrders"         // Result array name
    }
  }
])
```

**7. $unwind (Array ko flatten karo)**
```javascript
// User with skills array
{ name: "Rahul", skills: ["JS", "Python", "MongoDB"] }

db.users.aggregate([
  { $unwind: "$skills" }
])

// Output (3 separate documents):
{ name: "Rahul", skills: "JS" }
{ name: "Rahul", skills: "Python" }
{ name: "Rahul", skills: "MongoDB" }
```

**Real-world Complex Example:**
```javascript
// E-commerce: Category-wise sales analysis
db.orders.aggregate([
  // Stage 1: Completed orders only
  { $match: { status: "completed" } },

  // Stage 2: Unwind products array
  { $unwind: "$products" },

  // Stage 3: Lookup product details
  {
    $lookup: {
      from: "products",
      localField: "products.productId",
      foreignField: "_id",
      as: "productInfo"
    }
  },

  // Stage 4: Flatten product info
  { $unwind: "$productInfo" },

  // Stage 5: Group by category
  {
    $group: {
      _id: "$productInfo.category",
      totalRevenue: { $sum: { $multiply: ["$products.quantity", "$products.price"] } },
      totalOrders: { $sum: 1 },
      avgOrderValue: { $avg: "$products.price" }
    }
  },

  // Stage 6: Sort by revenue
  { $sort: { totalRevenue: -1 } },

  // Stage 7: Top 5 categories
  { $limit: 5 }
])
```

**Interview Tip:** Batao ki aggregation pipeline SQL ke GROUP BY, JOIN, HAVING jaise operations efficiently handle karta hai. Performance ke liye $match aur $project stages ko pipeline ke starting mein use karo (data jaldi filter ho jaye).

---

### Q9: Update Operators explain karo ($set, $inc, $push, etc.)

**Answer:**
Update operators se documents ko precisely modify kar sakte ho bina pura document replace kiye.

**1. $set (Field set/update karo)**
```javascript
db.users.updateOne(
  { _id: ObjectId("...") },
  { $set: { age: 30, city: "Mumbai" } }
)
```

**2. $unset (Field remove karo)**
```javascript
db.users.updateOne(
  { _id: ObjectId("...") },
  { $unset: { tempField: "" } }  // tempField delete ho jayega
)
```

**3. $inc (Number increment/decrement)**
```javascript
// Likes badhao
db.posts.updateOne(
  { _id: ObjectId("...") },
  { $inc: { likes: 1 } }  // likes++
)

// Price kam karo
db.products.updateOne(
  { _id: ObjectId("...") },
  { $inc: { price: -100 } }  // price -= 100
)
```

**4. $mul (Number multiply)**
```javascript
// Price 10% badha do
db.products.updateMany(
  { category: "electronics" },
  { $mul: { price: 1.1 } }
)
```

**5. $rename (Field ka naam change)**
```javascript
db.users.updateMany(
  {},
  { $rename: { "phone": "mobile" } }
)
```

**Array Update Operators:**

**6. $push (Array mein add karo)**
```javascript
// Single value
db.users.updateOne(
  { _id: ObjectId("...") },
  { $push: { skills: "MongoDB" } }
)

// Multiple values with $each
db.users.updateOne(
  { _id: ObjectId("...") },
  { $push: { skills: { $each: ["React", "Node.js"] } } }
)

// Sorted insertion with $sort
db.users.updateOne(
  { _id: ObjectId("...") },
  {
    $push: {
      scores: {
        $each: [85, 92, 78],
        $sort: -1  // Descending sort
      }
    }
  }
)
```

**7. $addToSet (Duplicate avoid karke add)**
```javascript
// "JavaScript" already hai toh add nahi hoga
db.users.updateOne(
  { _id: ObjectId("...") },
  { $addToSet: { skills: "JavaScript" } }
)

// Multiple unique values
db.users.updateOne(
  { _id: ObjectId("...") },
  { $addToSet: { skills: { $each: ["Python", "Go"] } } }
)
```

**8. $pop (Array se remove - first ya last)**
```javascript
// Last element remove
db.users.updateOne(
  { _id: ObjectId("...") },
  { $pop: { skills: 1 } }
)

// First element remove
db.users.updateOne(
  { _id: ObjectId("...") },
  { $pop: { skills: -1 } }
)
```

**9. $pull (Specific value remove)**
```javascript
// "PHP" skill remove karo
db.users.updateOne(
  { _id: ObjectId("...") },
  { $pull: { skills: "PHP" } }
)

// Condition se remove (score < 60)
db.students.updateOne(
  { _id: ObjectId("...") },
  { $pull: { scores: { $lt: 60 } } }
)
```

**10. $pullAll (Multiple values remove)**
```javascript
db.users.updateOne(
  { _id: ObjectId("...") },
  { $pullAll: { skills: ["PHP", "Java", "C++"] } }
)
```

**11. $ (Positional Operator - Array element update)**
```javascript
// Array mein specific element update
db.students.updateOne(
  { _id: ObjectId("..."), "grades.subject": "Math" },
  { $set: { "grades.$.score": 95 } }
)
// "grades" array mein jo element "Math" subject hai, uska score update ho jayega
```

**12. $[] (All array elements update)**
```javascript
// Sab scores mein 5 marks add
db.students.updateOne(
  { _id: ObjectId("...") },
  { $inc: { "grades.$[].score": 5 } }
)
```

**13. $[identifier] (Filtered array update)**
```javascript
// Sirf high scores (>80) ko update
db.students.updateOne(
  { _id: ObjectId("...") },
  { $inc: { "grades.$[elem].score": 10 } },
  { arrayFilters: [{ "elem.score": { $gt: 80 } }] }
)
```

**Real-world Example:**
```javascript
// E-commerce Cart Management

// Cart mein product add
db.users.updateOne(
  { _id: ObjectId("user123") },
  {
    $push: {
      cart: {
        productId: "PROD456",
        name: "Laptop",
        price: 50000,
        quantity: 1
      }
    }
  }
)

// Cart mein product quantity badhao
db.users.updateOne(
  { _id: ObjectId("user123"), "cart.productId": "PROD456" },
  { $inc: { "cart.$.quantity": 1 } }
)

// Cart se product remove
db.users.updateOne(
  { _id: ObjectId("user123") },
  { $pull: { cart: { productId: "PROD456" } } }
)
```

**Interview Tip:** $set vs $push vs $addToSet ka clear difference batao. Batao ki array operations mein positional operators ($ aur $[]) se specific elements target kar sakte ho bina pura array replace kiye.

---

### Q10: Indexing Strategies - Kab kaun sa index?

**Answer:**
Sahi index strategy se query performance 100x fast ho sakti hai. Wrong indexes memory waste aur slow writes.

**Index Selection Guidelines:**

**1. Query Patterns Analyze Karo**
```javascript
// Agar ye queries frequently run hoti hain:
db.users.find({ email: "test@test.com" })
db.users.find({ city: "Mumbai", age: { $gte: 25 } })

// Toh ye indexes banao:
db.users.createIndex({ email: 1 })
db.users.createIndex({ city: 1, age: 1 })
```

**2. Compound Index Order (ESR Rule)**
```
E = Equality (exact match)
S = Sort
R = Range (>, <, >=, <=)
```

```javascript
// Query: city=Mumbai, age>=25, sort by name
db.users.find({ city: "Mumbai", age: { $gte: 25 } }).sort({ name: 1 })

// GOOD Index (ESR order):
db.users.createIndex({ city: 1, name: 1, age: 1 })
//                     E        S         R

// BAD Index:
db.users.createIndex({ age: 1, city: 1, name: 1 })  // Range first (galat)
```

**3. Covered Queries (Fastest - Index se hi data mil jaye)**
```javascript
// Index
db.users.createIndex({ email: 1, name: 1, age: 1 })

// Query (covered - index se hi answer mil jayega)
db.users.find(
  { email: "test@test.com" },
  { email: 1, name: 1, age: 1, _id: 0 }  // _id: 0 zaruri
)
// Disk access nahi, sirf index scan
```

**4. Unique Index (Duplicate prevent)**
```javascript
db.users.createIndex({ email: 1 }, { unique: true })
// Duplicate email insert nahi hoga
```

**5. Partial Index (Selective indexing - space save)**
```javascript
// Sirf active users ko index
db.users.createIndex(
  { email: 1 },
  { partialFilterExpression: { status: "active" } }
)
// Inactive users index mein nahi (space save)
```

**6. TTL Index (Auto-delete old documents)**
```javascript
// Sessions 1 hour baad auto-delete
db.sessions.createIndex(
  { createdAt: 1 },
  { expireAfterSeconds: 3600 }
)
```

**7. Text Index (Full-text search)**
```javascript
// Blog posts search
db.posts.createIndex({ title: "text", content: "text" })

// Search
db.posts.find({ $text: { $search: "mongodb aggregation" } })
```

**8. Wildcard Index (Dynamic fields)**
```javascript
// Flexible schema - sab fields index
db.products.createIndex({ "$**": 1 })
// Sab fields pe search kar sakte ho
```

**Index Analysis Commands:**

**explain() - Query performance check**
```javascript
db.users.find({ city: "Mumbai" }).explain("executionStats")

// Output check karo:
{
  executionStats: {
    executionTimeMillis: 5,      // 5ms (good!)
    totalDocsExamined: 1000,     // 1000 docs scan
    totalKeysExamined: 1,        // Index use hua
    executionStages: {
      stage: "IXSCAN"            // Index scan (good!)
      // stage: "COLLSCAN"       // Collection scan (bad!)
    }
  }
}
```

**getIndexes() - Current indexes dekhna**
```javascript
db.users.getIndexes()
```

**dropIndex() - Unused index remove**
```javascript
db.users.dropIndex("email_1")
```

**Index Stats**
```javascript
db.users.aggregate([{ $indexStats: {} }])
// Kaun sa index kitni baar use hua
```

**Real-world Strategy Example:**
```javascript
// E-commerce Products Collection

// Products schema
{
  _id: ObjectId("..."),
  name: "iPhone 14",
  category: "Electronics",
  brand: "Apple",
  price: 79900,
  rating: 4.5,
  inStock: true,
  createdAt: ISODate("...")
}

// Common queries:
// 1. Category + price range + sort by rating
// 2. Search by name
// 3. Brand + inStock
// 4. Top rated products

// Indexes:
db.products.createIndex({ category: 1, rating: -1, price: 1 })  // ESR rule
db.products.createIndex({ name: "text" })                        // Search
db.products.createIndex({ brand: 1, inStock: 1 })               // Filter
db.products.createIndex({ rating: -1 })                          // Top rated
```

**Common Mistakes:**
```javascript
// ❌ BAD: Har field pe index
db.users.createIndex({ name: 1 })
db.users.createIndex({ email: 1 })
db.users.createIndex({ age: 1 })
db.users.createIndex({ city: 1 })
// Memory waste + slow writes

// ✅ GOOD: Query patterns ke according compound indexes
db.users.createIndex({ email: 1 }, { unique: true })  // Email login
db.users.createIndex({ city: 1, age: 1 })             // Common filter
```

**Interview Tip:** ESR rule zaroor mention karo. Batao ki "too many indexes" se memory aur write performance suffer hoti hai. explain() command se query plans analyze karna aana chahiye.

---

### Q11: Data Modeling Patterns - Real-world use cases

**Answer:**
MongoDB mein different scenarios ke liye specific patterns hain jo scalability aur performance improve karte hain.

**1. ATTRIBUTE PATTERN (Flexible Schema with Common Fields)**

**Problem:** Products ka schema vary karta hai (electronics ke alag attributes, clothes ke alag).

**Solution:**
```javascript
// BAD: Fixed schema (electronics ko color ki zaroorat nahi)
{
  _id: "prod1",
  name: "Laptop",
  type: "electronics",
  screenSize: "15 inch",
  RAM: "16GB",
  color: null,      // Waste
  size: null        // Waste
}

// GOOD: Attribute Pattern
{
  _id: "prod1",
  name: "Laptop",
  type: "electronics",
  attributes: [
    { key: "screenSize", value: "15 inch" },
    { key: "RAM", value: "16GB" },
    { key: "processor", value: "Intel i7" }
  ]
}

{
  _id: "prod2",
  name: "T-Shirt",
  type: "clothing",
  attributes: [
    { key: "size", value: "M" },
    { key: "color", value: "Blue" },
    { key: "material", value: "Cotton" }
  ]
}

// Index for efficient search
db.products.createIndex({ "attributes.key": 1, "attributes.value": 1 })

// Query
db.products.find({ "attributes": { $elemMatch: { key: "RAM", value: "16GB" } } })
```

**Use Case:** E-commerce products, dynamic forms, multi-type data

---

**2. BUCKET PATTERN (Time-series Data)**

**Problem:** IoT sensors, logs - har second data aa raha hai (millions of documents).

**Solution:**
```javascript
// BAD: Har reading ek document (100M documents!)
{
  _id: ObjectId("..."),
  sensorId: "sensor123",
  temperature: 25.5,
  timestamp: ISODate("2025-01-15T10:30:00Z")
}

// GOOD: Bucket Pattern (1 hour ka data ek document mein)
{
  _id: ObjectId("..."),
  sensorId: "sensor123",
  date: ISODate("2025-01-15T10:00:00Z"),
  readings: [
    { temperature: 25.5, timestamp: ISODate("2025-01-15T10:00:01Z") },
    { temperature: 25.7, timestamp: ISODate("2025-01-15T10:00:02Z") },
    { temperature: 25.6, timestamp: ISODate("2025-01-15T10:00:03Z") },
    // ... 3600 readings (1 hour)
  ],
  count: 3600,
  avgTemp: 25.6
}

// Index
db.sensorData.createIndex({ sensorId: 1, date: 1 })
```

**Benefits:** 100M documents → 27K documents (3600x reduction!), fast queries, less memory

**Use Case:** IoT data, server logs, analytics, stock prices

---

**3. OUTLIER PATTERN (Handle Extreme Cases)**

**Problem:** 99% users ke 10 orders, 1% users ke 10,000 orders (celebrities, bulk buyers).

**Solution:**
```javascript
// Normal users (embedded)
{
  _id: ObjectId("user123"),
  name: "Rahul",
  email: "rahul@test.com",
  orders: [
    { orderId: "ORD001", amount: 500 },
    { orderId: "ORD002", amount: 1200 }
    // ... max 100 orders
  ],
  isOutlier: false
}

// Outlier user (referenced)
{
  _id: ObjectId("user999"),
  name: "BigBuyer",
  email: "bigbuyer@test.com",
  orders: [],  // Empty (orders alag collection mein)
  isOutlier: true
}

// Separate orders for outliers
db.outlierOrders.insertMany([
  { userId: ObjectId("user999"), orderId: "ORD001", amount: 500 },
  { userId: ObjectId("user999"), orderId: "ORD002", amount: 1200 },
  // ... 10,000 orders
])

// Query logic
if (user.isOutlier) {
  orders = db.outlierOrders.find({ userId: user._id })
} else {
  orders = user.orders
}
```

**Use Case:** Social media (celebrities with millions of followers), e-commerce (bulk buyers)

---

**4. COMPUTED PATTERN (Pre-calculate Heavy Operations)**

**Problem:** Har baar aggregation run karna slow hai (dashboard stats).

**Solution:**
```javascript
// BAD: Har baar calculate (slow!)
db.orders.aggregate([
  { $match: { userId: ObjectId("user123") } },
  {
    $group: {
      _id: null,
      totalOrders: { $sum: 1 },
      totalSpent: { $sum: "$amount" },
      avgOrderValue: { $avg: "$amount" }
    }
  }
])

// GOOD: Pre-computed values (fast!)
{
  _id: ObjectId("user123"),
  name: "Rahul",
  email: "rahul@test.com",
  stats: {  // Pre-calculated
    totalOrders: 25,
    totalSpent: 35000,
    avgOrderValue: 1400,
    lastUpdated: ISODate("2025-01-15T10:30:00Z")
  }
}

// Update karte waqt stats bhi update
db.users.updateOne(
  { _id: ObjectId("user123") },
  {
    $inc: {
      "stats.totalOrders": 1,
      "stats.totalSpent": 1500
    },
    $set: {
      "stats.avgOrderValue": ...,
      "stats.lastUpdated": new Date()
    }
  }
)
```

**Use Case:** Dashboards, analytics, leaderboards, social media stats

---

**5. EXTENDED REFERENCE PATTERN (Denormalization for Performance)**

**Problem:** Har baar user details fetch karne ke liye JOIN (slow).

**Solution:**
```javascript
// BAD: Pure references (extra query)
// Posts Collection
{
  _id: ObjectId("post123"),
  title: "MongoDB Tutorial",
  content: "...",
  authorId: ObjectId("user456")  // Sirf ID
}
// Author details ke liye extra query

// GOOD: Extended Reference (frequently needed data copy)
{
  _id: ObjectId("post123"),
  title: "MongoDB Tutorial",
  content: "...",
  authorId: ObjectId("user456"),
  author: {  // Frequently accessed data
    name: "Vikram",
    profilePic: "https://...",
    username: "@vikram"
  }
}
// Author ki basic details already available (no extra query!)
```

**Trade-off:** Agar author name change ho toh sab posts update karne padenge. But read-heavy apps ke liye perfect.

**Use Case:** Social media posts, comments, reviews

---

**6. SUBSET PATTERN (Frequently Accessed Data)**

**Problem:** Product reviews - 1000+ reviews hain but user ko sirf recent 10 chahiye.

**Solution:**
```javascript
// Product document (sirf recent reviews)
{
  _id: ObjectId("prod123"),
  name: "iPhone 14",
  price: 79900,
  recentReviews: [  // Last 10 reviews only
    { user: "Rahul", rating: 5, text: "Awesome!" },
    { user: "Priya", rating: 4, text: "Good phone" },
    // ... 10 reviews
  ],
  totalReviews: 1500,
  avgRating: 4.5
}

// Separate reviews collection (sab reviews)
db.reviews.insertMany([
  { productId: ObjectId("prod123"), user: "Rahul", rating: 5, ... },
  { productId: ObjectId("prod123"), user: "Priya", rating: 4, ... },
  // ... 1500 reviews
])

// User ko recent reviews dikha do (fast)
// "See all reviews" pe click kare toh reviews collection se fetch
```

**Use Case:** Product reviews, social media posts, comments

---

**7. POLYMORPHIC PATTERN (Different Types in Same Collection)**

**Problem:** Athletes ka data store karna - cricket players, football players, swimmers (different stats).

**Solution:**
```javascript
// Cricket Player
{
  _id: ObjectId("athlete1"),
  name: "Virat Kohli",
  sport: "cricket",
  age: 35,
  stats: {
    matches: 254,
    runs: 12400,
    average: 58.67,
    centuries: 43
  }
}

// Football Player
{
  _id: ObjectId("athlete2"),
  name: "Sunil Chhetri",
  sport: "football",
  age: 39,
  stats: {
    matches: 150,
    goals: 94,
    assists: 30
  }
}

// Query by sport type
db.athletes.find({ sport: "cricket" })
db.athletes.createIndex({ sport: 1 })
```

**Use Case:** Multi-type entities, event logging, notifications

---

**Interview Tip:** Batao ki "One size doesn't fit all" - requirements ke according pattern choose karo. Read-heavy apps mein denormalization (Extended Reference, Computed) best hai. Write-heavy mein normalization (Referenced) better.

---

### Q12: Performance Optimization Techniques

**Answer:**
Production mein slow queries identify karke optimize karna sabse important skill hai.

**1. Slow Query Analysis**
```javascript
// Slow query log enable (mongod.conf)
systemLog:
  verbosity: 1
  slowOpThresholdMs: 100  // 100ms se slow queries log hongi

// Current operations dekhna
db.currentOp({ secs_running: { $gte: 3 } })  // 3 sec se running queries

// Kill slow query
db.killOp(operationId)
```

**2. explain() - Query Plan Analysis**
```javascript
db.users.find({ city: "Mumbai", age: { $gte: 25 } })
  .explain("executionStats")

// Check karo:
{
  executionStats: {
    executionTimeMillis: 500,      // Time taken
    totalDocsExamined: 100000,     // Documents scanned (high = bad)
    totalKeysExamined: 1000,       // Index keys scanned
    executionStages: {
      stage: "COLLSCAN"            // Collection scan (BAD!)
      // "IXSCAN" hona chahiye (Index scan)
    }
  }
}

// Optimization: Index add karo
db.users.createIndex({ city: 1, age: 1 })

// Ab explain() run karo
{
  executionStats: {
    executionTimeMillis: 5,        // 500ms → 5ms (100x faster!)
    totalDocsExamined: 50,
    totalKeysExamined: 50,
    executionStages: {
      stage: "IXSCAN"              // Index scan (GOOD!)
    }
  }
}
```

**Key Metrics:**
- **totalDocsExamined / nReturned ratio**: Should be close to 1 (ideal)
  - 100000 docs examined, 10 returned = BAD (99.99% waste!)
  - 10 docs examined, 10 returned = GOOD
- **COLLSCAN**: Collection scan (slow, needs index)
- **IXSCAN**: Index scan (fast)

**3. Projection - Sirf Zaruri Fields**
```javascript
// BAD: Sab fields fetch (network bandwidth waste)
db.users.find({ city: "Mumbai" })

// GOOD: Sirf required fields
db.users.find(
  { city: "Mumbai" },
  { name: 1, email: 1, _id: 0 }  // Sirf name aur email
)
```

**4. Limit Results**
```javascript
// BAD: Sab documents return (memory issue)
db.users.find({ city: "Mumbai" })

// GOOD: Pagination
db.users.find({ city: "Mumbai" }).limit(20)
```

**5. Covered Queries (Fastest)**
```javascript
// Index
db.users.createIndex({ email: 1, name: 1, age: 1 })

// Query (completely covered by index)
db.users.find(
  { email: "test@test.com" },
  { email: 1, name: 1, age: 1, _id: 0 }  // _id: 0 zaruri!
).explain()

// Output
{
  totalDocsExamined: 0,  // 0 documents accessed (fastest!)
  indexOnly: true
}
```

**6. Aggregation Pipeline Optimization**
```javascript
// BAD: Match end mein (sab data process)
db.orders.aggregate([
  { $lookup: { from: "products", ... } },  // Join first (slow)
  { $unwind: "$products" },
  { $match: { status: "completed" } }      // Filter end mein
])

// GOOD: Match pahle (data jaldi reduce)
db.orders.aggregate([
  { $match: { status: "completed" } },     // Filter first (fast)
  { $lookup: { from: "products", ... } },
  { $unwind: "$products" }
])
```

**Pipeline Optimization Rules:**
- $match aur $project jitna jaldi ho sake utna pahle
- $sort ko index se support karo
- $lookup ke baad $match use karo (joined data filter)

**7. Connection Pooling**
```javascript
// Node.js with MongoDB driver
const { MongoClient } = require('mongodb');

const client = new MongoClient(uri, {
  maxPoolSize: 50,        // Max 50 connections
  minPoolSize: 10,        // Min 10 connections always ready
  maxIdleTimeMS: 30000    // 30 sec idle timeout
});
```

**8. Read Preferences (Replica Sets)**
```javascript
// Primary se read (default - consistent but slow)
db.users.find({}).readPref("primary")

// Secondary se read (fast but slightly stale data)
db.users.find({}).readPref("secondary")

// Nearest se read (lowest latency)
db.users.find({}).readPref("nearest")
```

**Use Cases:**
- **Primary**: Critical data (orders, payments)
- **Secondary**: Analytics, reports, dashboards
- **Nearest**: Low latency apps

**9. Bulk Operations**
```javascript
// BAD: Loop mein individual inserts (100 queries!)
for (let i = 0; i < 100; i++) {
  db.users.insertOne({ name: `User${i}` })
}

// GOOD: Bulk insert (1 query!)
const users = [];
for (let i = 0; i < 100; i++) {
  users.push({ name: `User${i}` })
}
db.users.insertMany(users)
```

**10. Schema Design for Performance**
```javascript
// BAD: Deep nesting (slow $lookup)
// Users → Orders → Products → Categories (3 lookups!)

// GOOD: Denormalization
{
  _id: ObjectId("order123"),
  userId: ObjectId("user456"),
  userName: "Rahul",  // Denormalized (no lookup needed)
  products: [
    {
      productId: "prod789",
      name: "Laptop",  // Denormalized
      price: 50000
    }
  ]
}
```

**11. Database Profiling**
```javascript
// Profiling enable karo
db.setProfilingLevel(2)  // 0=off, 1=slow only, 2=all queries

// Profiling data dekhna
db.system.profile.find().sort({ ts: -1 }).limit(10)

// Slow queries filter
db.system.profile.find({ millis: { $gt: 100 } })
```

**12. MongoDB Monitoring Tools**
```javascript
// Server stats
db.serverStatus()

// Database stats
db.stats()

// Collection stats
db.users.stats()

// Index usage
db.users.aggregate([{ $indexStats: {} }])
```

**Real-world Optimization Example:**
```javascript
// BEFORE: Slow query (2000ms)
db.orders.find({
  status: "completed",
  createdAt: { $gte: ISODate("2025-01-01") }
}).sort({ totalAmount: -1 })

// Analysis:
// - No index on status, createdAt, totalAmount
// - Collection scan (COLLSCAN)
// - 100,000 documents examined

// AFTER: Optimization
// 1. Create compound index (ESR rule)
db.orders.createIndex({ status: 1, totalAmount: -1, createdAt: 1 })

// 2. Query with projection
db.orders.find(
  {
    status: "completed",
    createdAt: { $gte: ISODate("2025-01-01") }
  },
  { orderId: 1, totalAmount: 1, userId: 1 }  // Sirf required fields
).sort({ totalAmount: -1 }).limit(20)

// Result: 2000ms → 10ms (200x faster!)
```

**Interview Tip:** Always mention real metrics - "COLLSCAN to IXSCAN", "totalDocsExamined reduce kiya", "covered query banaya". Batao ki production mein slow query log aur monitoring tools regularly check karte ho.

---

## ✅ Checkpoint 2: Intermediate Level Complete!

**Key Topics Covered:**
✅ Aggregation Pipeline ($match, $group, $lookup, $unwind)
✅ Update Operators ($set, $inc, $push, $pull, array operators)
✅ Advanced Indexing (ESR rule, compound, partial, TTL)
✅ Data Modeling Patterns (Attribute, Bucket, Outlier, Computed, etc.)
✅ Performance Optimization (explain(), covered queries, profiling)

---

## 🔥 Ready for LEVEL 3: ADVANCED?

**Next Topics:**
- Replication & High Availability
- Sharding & Horizontal Scaling
- Transactions (ACID in MongoDB)
- MongoDB Atlas & Cloud Features
- Security (Authentication, Authorization, Encryption)
- Backup & Disaster Recovery
- Real-world System Design

**Type "next" to continue to Advanced Level!**
