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

---

## 🔥 LEVEL 3: ADVANCED (Production-ready Architecture)

### Q13: Replication kya hai? Replica Set kaise kaam karta hai?

**Answer:**
Replication ek mechanism hai jisne data ki copies multiple servers pe hoti hain, high availability aur fault tolerance provide karta hai.

**Deep Explanation:**

**Replica Set = Primary + Secondary + Arbiter**

```
┌──────────────┐         ┌──────────────┐         ┌──────────────┐
│   PRIMARY    │◄────────►│  SECONDARY   │         │   SECONDARY  │
│   (Active)   │  Sync    │   (Standby)  │         │  (Standby)   │
└──────────────┘         └──────────────┘         └──────────────┘
      ↓                         ↑
   Writes                    Reads
   Reads                  (optional)
      ↓
┌──────────────┐
│   ARBITER    │
│  (Vote only) │
└──────────────┘
```

**How Replication Works:**

1. **Write to Primary**: Client writes primary ko
2. **Apply Locally**: Primary apne local storage mein apply karta hai
3. **Replicate**: Primary operation log (oplog) ko secondary ko bhejta hai
4. **Apply on Secondary**: Secondary woh operations apply karta hai
5. **Acknowledge**: Replication complete hone ke baad client ko ack milta hai

```javascript
// Replica Set Configuration (3 nodes)
// Primary (port 27017)
// Secondary 1 (port 27018)
// Secondary 2 (port 27019)

// Initialize replica set
rs.initiate({
  _id: "myReplicaSet",
  members: [
    { _id: 0, host: "server1:27017", priority: 1 },  // Primary
    { _id: 1, host: "server2:27017", priority: 0 },  // Secondary
    { _id: 2, host: "server3:27017", priority: 0 }   // Secondary
  ]
})

// Status check
rs.status()

// Configuration
rs.conf()
```

**Replica Set Members:**

| Member | Role | Writes | Reads | Priority |
|--------|------|--------|-------|----------|
| Primary | Active | Yes | Yes | High |
| Secondary | Standby | No | Yes* | Low |
| Arbiter | Vote only | No | No | N/A |
| Hidden | Backup | No | No | 0 |
| Delayed | Historical | No | No | 0 |

*Secondary se reads take करना risky hai (slightly stale data)

**Automatic Failover (Election):**
```
If Primary goes down → Secondaries elect naya Primary

Timeline:
T=0s: Primary crashes
T=1-5s: Secondaries detect
T=5-10s: Election happens
T=10s: New Primary elected
```

```javascript
// Node.js Connection String
const uri = "mongodb://server1:27017,server2:27017,server3:27017/?replicaSet=myReplicaSet";

const client = new MongoClient(uri);

// Automatic reconnection aur failover handle ho jayega
```

**Write Concern (How many replicas confirm likhna chahiye):**
```javascript
// Write concern levels
// w: 0 - No confirmation
// w: 1 - Primary confirm (default)
// w: 2 - Primary + 1 Secondary
// w: "majority" - Majority of nodes (most safe)

db.users.insertOne(
  { name: "Rahul" },
  { writeConcern: { w: "majority", j: true } }
  // j: true = Journal mein likho (durable)
)
```

**Read Preference (Kahan se read karna?):**
```javascript
// primary - Sirf primary (consistent, slow)
// primaryPreferred - Primary prefer, agar down ho toh secondary
// secondary - Sirf secondary (fast, stale data)
// secondaryPreferred - Secondary prefer, fallback to primary
// nearest - Lowest latency

db.users.find({}).readPref("secondary")
```

**Interview Tip:** Replica set minimum 3 nodes hone chahiye (1 primary + 2 secondary), 5 nodes recommended. Arbiter se cost save ho sakta hai agar vote chahiye without storing data.

---

### Q14: Sharding kya hai? Data ko scale kaise karte ho?

**Answer:**
Sharding horizontal scaling hai jisne data multiple servers (shards) mein distribute karta hai. Jab data bahut badh jaye toh sharding se load distribute hota hai.

**Deep Explanation:**

**Sharding Architecture:**
```
┌─────────────────────────────────────────┐
│        Client Application               │
└──────────────────┬──────────────────────┘
                   │
        ┌──────────┴──────────┐
        │  Mongos Router      │
        │  (Query Router)     │
        └──────────┬──────────┘
        ┌─────────┬─────────┬─────────┐
        ▼         ▼         ▼         ▼
    ┌────────┐┌────────┐┌────────┐
    │Shard 1 ││Shard 2 ││Shard 3 │
    │Data:   ││Data:   ││Data:   │
    │A-H     ││I-Q     ││R-Z     │
    └────────┘└────────┘└────────┘
        │         │         │
    ┌─────────────┴─────────────┐
    │   Config Servers (3)      │
    │   Metadata & Shard Map    │
    └──────────────────────────┘
```

**Shard Key Selection (Most Important!):**
```javascript
// GOOD Shard Key (cardinality high, evenly distributed)
db.users.createIndex({ userId: 1 })
sh.shardCollection("mydb.users", { userId: 1 })

// BAD Shard Key (low cardinality, uneven distribution)
sh.shardCollection("mydb.users", { country: 1 })
// Sirf 195 countries hain, some shard mein data zyada hoga
// "Hot Shard" problem

// EVEN WORSE (monotonically increasing)
sh.shardCollection("mydb.users", { timestamp: 1 })
// Latest records sirf ek shard mein jayenge
// Write bottleneck
```

**Shard Key Characteristics:**
1. **High Cardinality** - Zaada unique values
2. **Low Frequency** - Values frequently change nahi honge
3. **Non-Monotonic** - Continuously increasing nahi hone
4. **Evenly Distributed** - Balanced distribution

**Sharding Process:**
```javascript
// Step 1: Enable sharding on database
sh.enableSharding("mydb")

// Step 2: Create index on shard key
db.users.createIndex({ userId: 1 })

// Step 3: Shard collection
sh.shardCollection("mydb.users", { userId: 1 })

// Step 4: Check shard status
db.printShardingStatus()
```

**Data Distribution:**
```
User IDs: 1-1000

Shard 1: 1-333
Shard 2: 334-666
Shard 3: 667-1000

When insert { userId: 500 }
→ Mongos calculates shard key
→ Determines Shard 2
→ Inserts into Shard 2
```

**Chunk Migration (Automatic Balancing):**
```
If Shard 1 has too much data →
→ Balancer moves chunks to other shards
→ Load balanced!
```

**Query Routing:**
```javascript
// Targeted Query (GOOD - goes to 1 shard)
db.users.find({ userId: 500 })
// Mongos knows userId 500 is in Shard 2
// Query only Shard 2 (fast)

// Scatter-Gather Query (BAD - hits all shards)
db.users.find({ country: "India" })
// No shard key in query
// Mongos has to ask all 3 shards
// Slower
```

**Sharding vs Replica Set:**

| Aspect | Replica Set | Sharding |
|--------|-------------|----------|
| Purpose | High Availability | Horizontal Scaling |
| Data | Same on all nodes | Different on each shard |
| Scalability | Limited | Unlimited |
| Complexity | Simple | Complex |
| Use When | Small-Medium data | Large data (100GB+) |

**Often Together:**
```
Shard 1: Replica Set (Primary + 2 Secondary)
Shard 2: Replica Set (Primary + 2 Secondary)
Shard 3: Replica Set (Primary + 2 Secondary)
```

**Interview Tip:** "Shard key sirf ek baar choose hota hai, baad mein change nahi kar sakte" - ye zaroor mention karo. Shard key wrong choose karne se "Hot Shard" ya uneven distribution ho sakta hai jo performance ruin kar deta hai.

---

### Q15: Transactions - ACID Properties kaise?

**Answer:**
MongoDB 4.0 mein transactions add hue, jisne multi-document ACID guarantees provide kiye. Pehle sirf single document atomic tha.

**Deep Explanation:**

**ACID Properties:**
- **Atomicity**: Sab operations success ya sab fail (all or nothing)
- **Consistency**: Data always valid state mein
- **Isolation**: Concurrent transactions nahi interfere
- **Durability**: Committed data permanent

**Single Document Transaction (Always Atomic):**
```javascript
// Pehle se atomic tha
db.accounts.updateOne(
  { _id: "account1" },
  { $inc: { balance: -100 } }
)
// Ye sab operations atomic hain: read → modify → write

// Agar server crash ho update ke beech → rollback automatically
```

**Multi-Document Transaction (4.0+):**
```javascript
// Money transfer: Account1 se Account2 ko transfer

// WITHOUT Transaction (Risky!)
db.accounts.updateOne(
  { _id: "account1" },
  { $inc: { balance: -100 } }
)
// Agar crash ho yahan → Account1 se paise gaye but Account2 ko nahi!

// WITH Transaction (Safe!)
const session = db.getMongo().startSession();

session.startTransaction();
try {
  db.accounts.updateOne(
    { _id: "account1" },
    { $inc: { balance: -100 } },
    { session }
  );

  db.accounts.updateOne(
    { _id: "account2" },
    { $inc: { balance: +100 } },
    { session }
  );

  session.commitTransaction();
  // Dono updates committed, ya dono rollback
} catch (error) {
  session.abortTransaction();
}
session.endSession();
```

**Node.js Example:**
```javascript
const { MongoClient } = require('mongodb');

async function transferMoney(amount) {
  const client = new MongoClient(uri);
  const session = client.startSession();

  try {
    await session.withTransaction(async () => {
      const accountsDb = client.db("bank").collection("accounts");

      // Debit from account1
      await accountsDb.updateOne(
        { _id: "account1" },
        { $inc: { balance: -amount } },
        { session }
      );

      // Credit to account2
      await accountsDb.updateOne(
        { _id: "account2" },
        { $inc: { balance: amount } },
        { session }
      );
    });
    console.log("Transfer successful");
  } catch (error) {
    console.log("Transfer failed, rolled back");
  } finally {
    await session.endSession();
    await client.close();
  }
}
```

**Isolation Levels:**

**1. Read Uncommitted (Default)**
```javascript
// Transaction incomplete hone se pehle read kar sakte ho
// Dirty reads possible
// Performance best

db.startTransaction({
  readConcern: { level: "local" }
})
```

**2. Read Committed**
```javascript
// Sirf committed data read
// Dirty reads nahi hote
// Performance medium

db.startTransaction({
  readConcern: { level: "committed" }
})
```

**3. Snapshot**
```javascript
// Consistent snapshot
// Phantom reads nahi hote
// Performance slow but secure

db.startTransaction({
  readConcern: { level: "snapshot" }
})
```

**Transaction Limitations:**
- Sirf **Replica Sets** pe kaam karte hain (single node pe nahi)
- Transactions mein **oplog size limit** (16MB)
- **Sharded transactions** simp complicated aur slow hain

**Interview Tip:** Transactions MongoDB mein expensive hain (performance cost), sirf zaruri jagah use karo. 99% time single document operations atomic hote hain, unnecessary transactions avoid karo.

---

### Q16: MongoDB Atlas - Cloud Platform

**Answer:**
MongoDB Atlas fully-managed cloud database service hai. Server manage, backup, scaling - sab automatically.

**Key Features:**

**1. Deployment Options:**
```
- Shared Cluster (free tier, limited)
- Dedicated Cluster (production)
- Serverless (pay-per-request)
```

**2. Automatic Backups:**
```
- Continuous backups (every 5 minutes)
- Point-in-time recovery
- Disaster recovery
```

**3. Auto-Scaling:**
```javascript
// Storage automatically scale
// Throughput auto-scale
// No manual intervention
```

**4. Monitoring & Alerts:**
```
- Real-time metrics
- Performance advisor
- Query profiler
- Slow query log
```

**5. Security Features:**
```
- IP Whitelist / VPC Peering
- TLS/SSL encryption
- Database Users & Roles
- Audit logs
```

**6. Multi-Region Replication:**
```
Data multiple regions mein:
- Global redundancy
- Low-latency reads
- Disaster recovery
```

**Connection Example:**
```javascript
const uri = "mongodb+srv://username:password@cluster.mongodb.net/dbname";
const client = new MongoClient(uri);
```

**Interview Tip:** Atlas production mein use karte ho, managed service, no ops overhead. Sirf develop/testing ke liye local MongoDB use karo.

---

### Q17: Security - Authentication aur Authorization

**Answer:**
MongoDB mein user access control, encryption, audit trails important hain.

**1. Authentication (Who are you?)**

**SCRAM-SHA-256 (Default):**
```javascript
// User create karo
db.createUser({
  user: "rahul",
  pwd: "securePassword123",
  roles: ["readWrite"]
})

// Connect with auth
const uri = "mongodb://rahul:securePassword123@localhost:27017/mydb";
const client = new MongoClient(uri);
```

**2. Authorization (What can you do?)**

**Built-in Roles:**
```javascript
// Read-only
db.createUser({
  user: "analyst",
  pwd: "password",
  roles: [{ role: "read", db: "mydb" }]
})

// Read-Write
db.createUser({
  user: "app",
  pwd: "password",
  roles: [{ role: "readWrite", db: "mydb" }]
})

// Admin
db.createUser({
  user: "admin",
  pwd: "password",
  roles: [{ role: "dbAdmin", db: "mydb" }]
})
```

**Custom Roles:**
```javascript
db.createRole({
  role: "customRole",
  privileges: [
    {
      resource: { db: "mydb", collection: "users" },
      actions: ["find", "insert"]  // Sirf ye operations
    }
  ],
  roles: []
})
```

**3. Encryption:**

**In Transit (TLS/SSL):**
```javascript
const client = new MongoClient(uri, {
  tls: true,
  tlsCAFile: "/path/to/ca.pem"
})
```

**At Rest:**
```
Atlas mein encryption-at-rest automatic
Self-managed mein:
- File system encryption
- Key management systems
```

**4. Network Security:**

**IP Whitelist:**
```
Atlas → Network Access → IP Whitelist
Allow sirf trusted IPs ko
```

**VPC Peering:**
```
Private network connection
Extra security layer
```

**5. Audit Logs:**
```javascript
// Enable audit logging
mongod --auditDestination file --auditFormat JSON --auditPath /path/to/audit.log

// Commands logged:
// - Authentication attempts
// - Database/Collection access
// - Schema modifications
```

**Interview Tip:** "Never store passwords in code, use environment variables." RLS jaisa access control important hai, har user ko sirf apna data access karna chahiye.

---

### Q18: Backup & Disaster Recovery

**Answer:**
Data loss worst nightmare hai, regular backups + recovery plan must-have.

**Backup Types:**

**1. Logical Backup (Application-level):**
```bash
# mongodump - entire database export
mongodump --out /backup/path

# Export specific collection
mongodump --db mydb --collection users --out /backup/path

# Restore
mongorestore --db mydb /backup/path/mydb
```

**2. Physical Backup (File-level):**
```bash
# Copy DBPath files (faster)
cp -r /var/lib/mongod/data /backup/mongod-data-$(date +%Y%m%d)

# Must stop mongod first:
mongod --shutdown
```

**3. Cloud Snapshots (Atlas):**
```
Atlas → Backup → Automated Snapshots
- Every 6 hours automatic
- Last 35 days retention
- One-click restore
```

**Backup Strategy:**

**RPO (Recovery Point Objective)** - Kitni data loss acceptable?
- Daily backup = 24 hour RPO (worst case 1 day data loss)
- Continuous backup = 5 minute RPO

**RTO (Recovery Time Objective)** - Kitna time recovery?
- Database 100GB = 2-4 hours restore time
- Replica set failover = 10-30 seconds

**Real-world Plan:**
```
1. Automated snapshots (Atlas)
   - Every 6 hours
   - 35 days retention

2. Weekly full export
   - mongodump to S3
   - Separate storage

3. Read-only secondary
   - Point-in-time recovery
   - Standalone testing

4. Geo-redundant backup
   - Multiple regions
   - Disaster recovery
```

**Interview Tip:** Production mein "Test your backups regularly" - kitni bar backups successful take liya lekin restore nahi ho paya? Real scenario mein practice karo.

---

### Q19: Query Optimization Deep Dive - Real Scenarios

**Scenario 1: E-commerce Product Search**
```javascript
// Collection
{
  _id: ObjectId("..."),
  name: "iPhone 14",
  category: "Electronics",
  price: 79900,
  rating: 4.5,
  reviews: [{ user: "Rahul", text: "Good!" }, ...],
  inStock: true,
  tags: ["smartphone", "Apple", "5G"]
}

// Query: Search products by category, price range, sort by rating
const query = {
  category: "Electronics",
  price: { $gte: 30000, $lte: 100000 },
  inStock: true
};

// WITHOUT Optimization
db.products.find(query).sort({ rating: -1 })
// COLLSCAN - 1M documents scanned, 5000ms

// WITH Optimization
db.products.createIndex({
  category: 1,      // E (Equality)
  rating: -1,       // S (Sort)
  price: 1          // R (Range)
})

db.products.find(query, { name: 1, price: 1, rating: 1 })  // Projection
  .sort({ rating: -1 })
  .limit(20)

// Result: IXSCAN - 20 documents examined, 5ms (1000x faster!)
```

**Scenario 2: Complex Aggregation - Top Users by Revenue**
```javascript
// Bad aggregation
db.orders.aggregate([
  { $lookup: { from: "users", localField: "userId", foreignField: "_id", as: "userInfo" } },
  { $lookup: { from: "products", localField: "productId", foreignField: "_id", as: "productInfo" } },
  { $unwind: "$userInfo" },
  { $unwind: "$productInfo" },
  { $match: { status: "completed" } },  // ❌ Match end mein
  {
    $group: {
      _id: "$userInfo._id",
      totalRevenue: { $sum: "$productInfo.price" },
      userName: { $first: "$userInfo.name" }
    }
  },
  { $sort: { totalRevenue: -1 } },
  { $limit: 10 }
])
// 10M documents processed, 2 lookups, slow!

// Good aggregation (optimized)
db.orders.aggregate([
  { $match: { status: "completed" } },  // ✅ Match first (data reduce)
  {
    $group: {
      _id: "$userId",
      totalRevenue: { $sum: "$amount" }
    }
  },
  { $sort: { totalRevenue: -1 } },
  { $limit: 10 },
  {
    $lookup: {  // ✅ Lookup last (sirf 10 users)
      from: "users",
      localField: "_id",
      foreignField: "_id",
      as: "userInfo"
    }
  }
])
// 10K documents processed (1000x less), 1 lookup, fast!
```

**Scenario 3: High-Write Collection (IoT Sensors)**
```javascript
// Problem: 1M writes/second, indexes slow down writes

// Bad: Too many indexes
db.sensors.createIndex({ sensorId: 1 })
db.sensors.createIndex({ timestamp: 1 })
db.sensors.createIndex({ temperature: 1 })
db.sensors.createIndex({ location: 1 })
// 4 indexes = 4 updates per write operation
// Write latency high

// Good: Minimal indexes + Bucket pattern
db.sensors.createIndex({ sensorId: 1, date: 1 })  // Only necessary

// Bucket pattern (reduce documents)
{
  _id: ObjectId("..."),
  sensorId: "sensor123",
  date: ISODate("2025-01-15"),
  readings: [
    { temp: 25.5, timestamp: ISODate("2025-01-15T10:00:00Z") },
    // ... 3600 readings (1 hour)
  ]
}

// 1M readings/sec → 1000 bucket writes/sec (1000x less!)
```

**Interview Tip:** Real problems aate hain production mein - "We have 10M documents, queries taking 30 seconds" - indexing strategy, aggregation optimization, schema redesign - sab consider karna padta hai.

---

### Q20: MongoDB in Microservices Architecture

**Answer:**
Har microservice ka apna MongoDB database hona chahiye (database per service pattern).

**Architecture:**
```
┌──────────────┐
│ User Service │─ MongoDB: users collection
└──────────────┘

┌──────────────┐
│ Order Service│─ MongoDB: orders collection
└──────────────┘

┌──────────────┐
│ Product Svc  │─ MongoDB: products collection
└──────────────┘
```

**Service Communication (Event-driven):**
```javascript
// User Service: User creation
const newUser = await usersDb.users.insertOne({
  _id: ObjectId("user123"),
  name: "Rahul",
  email: "rahul@test.com"
});

// Publish event
await publishEvent('user.created', {
  userId: newUser._id,
  email: newUser.email
});

// Order Service: Subscribe to event
subscribeToEvent('user.created', async (event) => {
  // User create hone pe order service mein record add
  await ordersDb.users.insertOne({
    userId: event.userId,
    email: event.email,
    orders: []
  });
});
```

**Data Consistency Challenges:**

**Problem: Distributed Transaction**
```
User Service: User create ✓
Order Service: Order create ❌ (failed)

Now inconsistent state!
```

**Solution 1: Saga Pattern**
```
Step 1: User Service - Create user
Step 2: Order Service - Create order
Step 3: Payment Service - Process payment

Agar koi step fail → compensating transactions (rollback)
```

**Solution 2: Event Sourcing**
```
har change ek event log mein
eventual consistency
```

**Interview Tip:** Microservices architecture mein "eventual consistency" accept karna padta hai, immediate consistency difficult. CAP theorem mention karo - consistency, availability, partition tolerance - sirf 2 guarantee kar sakte ho simultaneously.

---

### Q21: Real-world System Design - Recommendation Engine

**Problem:** Netflix jaisa recommendation engine - millions of users, products, interactions.

**Data Model:**
```javascript
// Users Collection
{
  _id: ObjectId("user123"),
  name: "Rahul",
  email: "rahul@test.com",
  preferences: ["Action", "Comedy", "Thriller"],
  totalWatches: 150
}

// Movies Collection
{
  _id: ObjectId("movie456"),
  title: "Avatar",
  genre: ["Action", "Sci-Fi"],
  rating: 4.8,
  releaseDate: ISODate("2022-12-16"),
  cast: ["Sam Worthington", "Zoe Saldana"]
}

// Interactions Collection (3rd normal form)
{
  _id: ObjectId("inter789"),
  userId: ObjectId("user123"),
  movieId: ObjectId("movie456"),
  action: "watched",  // watched, rated, clicked
  rating: 5,
  timestamp: ISODate("2025-01-15T10:30:00Z"),
  watchDuration: 9000  // seconds
}

// Pre-computed Recommendations (Computed Pattern)
{
  _id: ObjectId("user123"),
  recommendations: [
    { movieId: ObjectId("movie789"), score: 0.95, reason: "similar_genre" },
    { movieId: ObjectId("movie101"), score: 0.88, reason: "popular_in_genre" }
  ],
  lastUpdated: ISODate("2025-01-15T02:00:00Z")
}
```

**Recommendation Algorithm (Aggregation):**
```javascript
// Step 1: Get user's watch history
// Step 2: Find similar movies
// Step 3: Rank by score
// Step 4: Return top 10

db.interactions.aggregate([
  // User's recent watches
  { $match: { userId: ObjectId("user123"), action: "watched" } },
  { $sort: { timestamp: -1 } },
  { $limit: 20 },

  // Get movie details
  {
    $lookup: {
      from: "movies",
      localField: "movieId",
      foreignField: "_id",
      as: "movieInfo"
    }
  },
  { $unwind: "$movieInfo" },

  // Extract genres
  { $unwind: "$movieInfo.genre" },

  // Group by genre
  {
    $group: {
      _id: "$movieInfo.genre",
      count: { $sum: 1 }
    }
  },

  // Now find similar movies in these genres
  {
    $lookup: {
      from: "movies",
      let: { genre: "$_id" },
      pipeline: [
        {
          $match: {
            $expr: { $in: ["$$genre", "$genre"] },
            rating: { $gte: 4.0 }
          }
        },
        { $limit: 50 }
      ],
      as: "similarMovies"
    }
  }
])
```

**Optimization:**
```javascript
// Pre-compute recommendations hourly (Bucket Pattern)
// Cache in Redis for instant lookup
// Only recalculate if user watches new movie
```

**Indexes:**
```javascript
db.interactions.createIndex({ userId: 1, timestamp: -1 })
db.interactions.createIndex({ movieId: 1, action: 1 })
db.movies.createIndex({ genre: 1, rating: -1 })
db.recommendations.createIndex({ _id: 1 })
```

**Interview Tip:** Real recommendations complex hain - user preferences, similar items, popularity, freshness sab consider karte ho. Pre-computation se user ko instant results milte hain.

---

## ✅ Checkpoint 3: Advanced Level Complete!

**Key Topics Covered:**
✅ Replication & Replica Sets (High Availability)
✅ Sharding & Horizontal Scaling
✅ Multi-Document Transactions (ACID)
✅ MongoDB Atlas (Cloud Platform)
✅ Security (Authentication, Authorization, Encryption)
✅ Backup & Disaster Recovery
✅ Query Optimization (Real Scenarios)
✅ Microservices Architecture
✅ System Design (Recommendation Engine)

---

---

## 💻 LEVEL 4: CODING CHALLENGES (Interview ke Real Questions)

### Challenge 1: Complex Aggregation - E-commerce Analytics

**Problem Statement:**
E-commerce platform pe analytics dashboard chahiye. Output:
- Top 10 categories by revenue
- Average order value per category
- Total customers per category
- Month-wise revenue trend

**Data Schema:**
```javascript
// Orders Collection
{
  _id: ObjectId("..."),
  userId: ObjectId("user123"),
  products: [
    {
      productId: ObjectId("prod456"),
      quantity: 2,
      price: 500,
      category: "Electronics"
    }
  ],
  totalAmount: 1000,
  orderDate: ISODate("2025-01-15T10:30:00Z"),
  status: "completed"
}
```

**Solution:**
```javascript
// Top 10 categories by revenue + analytics
db.orders.aggregate([
  // Filter: Only completed orders
  { $match: { status: "completed" } },

  // Unwind products array
  { $unwind: "$products" },

  // Group by category to get revenue
  {
    $group: {
      _id: "$products.category",
      totalRevenue: { $sum: { $multiply: ["$products.quantity", "$products.price"] } },
      totalOrders: { $sum: 1 },
      avgPrice: { $avg: "$products.price" },
      avgQuantity: { $avg: "$products.quantity" }
    }
  },

  // Calculate average order value
  {
    $addFields: {
      avgOrderValue: { $divide: ["$totalRevenue", "$totalOrders"] }
    }
  },

  // Sort by revenue descending
  { $sort: { totalRevenue: -1 } },

  // Top 10
  { $limit: 10 },

  // Project final output
  {
    $project: {
      _id: 0,
      category: "$_id",
      totalRevenue: { $round: ["$totalRevenue", 2] },
      totalOrders: 1,
      avgOrderValue: { $round: ["$avgOrderValue", 2] },
      avgPrice: { $round: ["$avgPrice", 2] }
    }
  }
])

// Month-wise revenue trend
db.orders.aggregate([
  { $match: { status: "completed" } },
  {
    $group: {
      _id: {
        year: { $year: "$orderDate" },
        month: { $month: "$orderDate" }
      },
      totalRevenue: { $sum: "$totalAmount" },
      orderCount: { $sum: 1 }
    }
  },
  { $sort: { "_id.year": 1, "_id.month": 1 } },
  {
    $project: {
      _id: 0,
      month: { $dateToString: { format: "%Y-%m", date: { $dateFromParts: { year: "$_id.year", month: "$_id.month", day: 1 } } } },
      totalRevenue: 1,
      orderCount: 1
    }
  }
])
```

**Interview Tips:**
- `$unwind` se array flatten hota hai (zaruri multiple times use)
- `$group` + `$sum` से aggregation
- `$addFields` se calculated fields
- `$round` से decimal places

---

### Challenge 2: Find Duplicate Users (Email/Phone)

**Problem:**
User collection mein duplicate emails ya phones ho sakte hain. Duplicates find karo aur merge strategy decide karo.

**Data:**
```javascript
{
  _id: ObjectId("..."),
  email: "rahul@test.com",
  phone: "9876543210",
  name: "Rahul Kumar",
  createdAt: ISODate("2025-01-01"),
  orders: 5
}
```

**Solution:**
```javascript
// Find duplicate emails
db.users.aggregate([
  {
    $group: {
      _id: "$email",
      count: { $sum: 1 },
      users: { $push: { id: "$_id", name: "$name", createdAt: "$createdAt", orders: "$orders" } }
    }
  },
  // Filter: Only duplicates (count > 1)
  { $match: { count: { $gt: 1 } } },
  // Sort by count descending
  { $sort: { count: -1 } }
])

// Merge duplicates (keep oldest, delete newer)
function mergeDuplicateUsers(email) {
  const duplicates = db.users.find({ email: email }).sort({ createdAt: 1 }).toArray();

  const keepUser = duplicates[0];  // Oldest user
  const deleteIds = duplicates.slice(1).map(u => u._id);

  // Merge orders from all users to oldest
  const totalOrders = duplicates.reduce((sum, u) => sum + u.orders, 0);

  // Update oldest user
  db.users.updateOne(
    { _id: keepUser._id },
    { $set: { orders: totalOrders } }
  );

  // Delete duplicates
  db.users.deleteMany({ _id: { $in: deleteIds } });

  console.log(`Merged ${duplicates.length} users with email ${email}`);
}

// Find and merge all duplicates
const duplicateEmails = db.users.aggregate([
  { $group: { _id: "$email", count: { $sum: 1 } } },
  { $match: { count: { $gt: 1 } } }
]).toArray();

duplicateEmails.forEach(dup => {
  mergeDuplicateUsers(dup._id);
});
```

**Interview Tips:**
- `$group` से duplicates find
- `$match` से filter duplicates
- `$push` से array mein collect
- Transaction use karo production mein (rollback safety)

---

### Challenge 3: Pagination with Sorting

**Problem:**
Product listing page pe 20 items per page, sort by price/rating, pagination implement karo.

**Solution:**
```javascript
function getPaginatedProducts(page = 1, limit = 20, sortBy = "price", order = 1) {
  const skip = (page - 1) * limit;

  // Create sort object
  const sortObj = {};
  sortObj[sortBy] = order;  // 1 = ascending, -1 = descending

  // Get total count
  const total = db.products.countDocuments({});

  // Get paginated results
  const products = db.products
    .find({})
    .sort(sortObj)
    .skip(skip)
    .limit(limit)
    .toArray();

  return {
    data: products,
    pagination: {
      currentPage: page,
      totalPages: Math.ceil(total / limit),
      totalItems: total,
      itemsPerPage: limit,
      hasNextPage: page < Math.ceil(total / limit)
    }
  };
}

// Usage
const result = getPaginatedProducts(2, 20, "rating", -1);  // Page 2, sort by rating descending

// With filters
function getPaginatedProductsWithFilter(page = 1, limit = 20, filter = {}, sortBy = "price") {
  const skip = (page - 1) * limit;
  const sortObj = { [sortBy]: 1 };

  const total = db.products.countDocuments(filter);
  const products = db.products
    .find(filter)
    .sort(sortObj)
    .skip(skip)
    .limit(limit)
    .toArray();

  return {
    data: products,
    pagination: {
      currentPage: page,
      totalPages: Math.ceil(total / limit),
      total,
      hasNext: page < Math.ceil(total / limit)
    }
  };
}

// Usage with filter
const electronics = getPaginatedProductsWithFilter(
  1, 20,
  { category: "Electronics", price: { $lt: 50000 } },
  "rating"
);
```

**Node.js Implementation:**
```javascript
const express = require('express');
const app = express();

app.get('/products', async (req, res) => {
  try {
    const page = parseInt(req.query.page) || 1;
    const limit = parseInt(req.query.limit) || 20;
    const sortBy = req.query.sortBy || "price";
    const order = req.query.order === "desc" ? -1 : 1;

    const skip = (page - 1) * limit;
    const sortObj = { [sortBy]: order };

    const total = await db.collection("products").countDocuments({});
    const products = await db.collection("products")
      .find({})
      .sort(sortObj)
      .skip(skip)
      .limit(limit)
      .toArray();

    res.json({
      data: products,
      pagination: {
        currentPage: page,
        totalPages: Math.ceil(total / limit),
        total,
        hasNext: page < Math.ceil(total / limit)
      }
    });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

app.listen(3000);
```

**Interview Tips:**
- `skip()` aur `limit()` से pagination
- Always `countDocuments()` total ke liye
- Sort parameters user-provided safe banao (whitelist)
- Hasura/GraphQL tools mein cursor-based pagination use karte ho (offset-based se efficient)

---

### Challenge 4: Update Nested Array Elements

**Problem:**
User ke cart mein product quantity update karna. Specific product ka quantity badhao.

**Data:**
```javascript
{
  _id: ObjectId("user123"),
  cart: [
    { productId: "PROD001", name: "Laptop", price: 50000, quantity: 1 },
    { productId: "PROD002", name: "Mouse", price: 500, quantity: 2 }
  ]
}
```

**Solutions:**

**1. Increment quantity (positional operator)**
```javascript
// Product PROD001 ke quantity ko 1 se badhao
db.users.updateOne(
  { _id: ObjectId("user123"), "cart.productId": "PROD001" },
  { $inc: { "cart.$.quantity": 1 } }
);

// Result: PROD001 quantity = 2
```

**2. Update multiple fields in array element**
```javascript
db.users.updateOne(
  { _id: ObjectId("user123"), "cart.productId": "PROD001" },
  {
    $set: {
      "cart.$.quantity": 5,
      "cart.$.updatedAt": new Date()
    }
  }
);
```

**3. Remove item from cart**
```javascript
db.users.updateOne(
  { _id: ObjectId("user123") },
  { $pull: { cart: { productId: "PROD001" } } }
);
```

**4. Update all cart items (e.g., apply discount)**
```javascript
db.users.updateOne(
  { _id: ObjectId("user123") },
  {
    $mul: { "cart.$[].price": 0.9 }  // 10% discount
  }
);
```

**5. Update conditional (e.g., mark items below price)**
```javascript
db.users.updateOne(
  { _id: ObjectId("user123") },
  {
    $set: { "cart.$[item].onSale": true }
  },
  {
    arrayFilters: [{ "item.price": { $lt: 1000 } }]
  }
);
```

**Node.js Function:**
```javascript
async function updateCartQuantity(userId, productId, newQuantity) {
  const client = new MongoClient(uri);

  try {
    const usersCollection = client.db("ecommerce").collection("users");

    if (newQuantity <= 0) {
      // Remove item
      await usersCollection.updateOne(
        { _id: new ObjectId(userId) },
        { $pull: { cart: { productId } } }
      );
    } else {
      // Update quantity
      await usersCollection.updateOne(
        { _id: new ObjectId(userId), "cart.productId": productId },
        { $set: { "cart.$.quantity": newQuantity } }
      );
    }

    return { success: true };
  } finally {
    await client.close();
  }
}
```

**Interview Tips:**
- `$` (positional operator) specific array element target karta hai
- `$[]` (all elements) sab elements update
- `$[identifier]` (filtered) conditional update
- `arrayFilters` se complex conditions possible

---

### Challenge 5: Batch Processing - Process Large Dataset

**Problem:**
10 million users ka data process karna, har user ke stats compute karna. Memory efficient banao.

**Bad Approach (Memory mein load nahi hota):**
```javascript
// ❌ WRONG - Sab users memory mein load
const allUsers = db.users.find({}).toArray();  // 10M docs!
allUsers.forEach(user => {
  // Process
});
```

**Good Approach (Batch Processing):**
```javascript
async function processTenMillionUsers() {
  const batchSize = 1000;
  const client = new MongoClient(uri);

  try {
    const usersCollection = client.db("mydb").collection("users");

    // Get total count
    const totalCount = await usersCollection.countDocuments({});

    // Process in batches
    for (let i = 0; i < totalCount; i += batchSize) {
      console.log(`Processing batch: ${i}/${totalCount}`);

      // Fetch 1000 users
      const batch = await usersCollection
        .find({})
        .skip(i)
        .limit(batchSize)
        .toArray();

      // Process each user
      for (const user of batch) {
        const stats = calculateStats(user);

        await usersCollection.updateOne(
          { _id: user._id },
          { $set: { stats } }
        );
      }
    }

    console.log("Processing complete!");
  } finally {
    await client.close();
  }
}

function calculateStats(user) {
  return {
    totalOrders: user.orders ? user.orders.length : 0,
    totalSpent: user.orders ? user.orders.reduce((sum, o) => sum + o.amount, 0) : 0,
    avgOrderValue: user.orders && user.orders.length > 0
      ? user.orders.reduce((sum, o) => sum + o.amount, 0) / user.orders.length
      : 0
  };
}
```

**Better: Aggregation Pipeline (Server-side Processing):**
```javascript
// Aggregation se directly compute aur update
db.users.aggregate([
  {
    $addFields: {
      totalOrders: { $size: { $ifNull: ["$orders", []] } },
      totalSpent: { $sum: { $ifNull: ["$orders.amount", 0] } }
    }
  },
  {
    $addFields: {
      avgOrderValue: {
        $cond: [
          { $gt: ["$totalOrders", 0] },
          { $divide: ["$totalSpent", "$totalOrders"] },
          0
        ]
      }
    }
  },
  {
    $merge: {
      into: "users",
      whenMatched: "merge",
      whenNotMatched: "insert"
    }
  }
])
```

**Best: Bulk Operations (Fastest):**
```javascript
async function processTenMillionUsersBulk() {
  const batchSize = 10000;
  const client = new MongoClient(uri);

  try {
    const usersCollection = client.db("mydb").collection("users");
    const totalCount = await usersCollection.countDocuments({});

    for (let i = 0; i < totalCount; i += batchSize) {
      console.log(`Processing batch: ${i}/${totalCount}`);

      const batch = await usersCollection
        .find({})
        .skip(i)
        .limit(batchSize)
        .toArray();

      // Prepare bulk operations
      const bulkOps = batch.map(user => ({
        updateOne: {
          filter: { _id: user._id },
          update: {
            $set: {
              stats: calculateStats(user),
              processedAt: new Date()
            }
          }
        }
      }));

      // Execute bulk
      if (bulkOps.length > 0) {
        await usersCollection.bulkWrite(bulkOps);
      }
    }

    console.log("Bulk processing complete!");
  } finally {
    await client.close();
  }
}
```

**Interview Tips:**
- Batch processing memory efficient
- Bulk operations fastest (ek hi database call)
- Aggregation pipeline server-side processing (less network)
- Large data mein cursor-based iteration use karo (offset-based skip nahi)
- Progress logging important (monitoring)

---

### Challenge 6: Real Interview Question - User Session Tracking

**Problem:**
User login/logout track karna. Active sessions, session duration, last activity.

**Data Model:**
```javascript
// Sessions Collection
{
  _id: ObjectId("..."),
  userId: ObjectId("user123"),
  sessionId: "uuid-xxx",
  loginTime: ISODate("2025-01-15T10:00:00Z"),
  logoutTime: ISODate("2025-01-15T18:00:00Z"),
  duration: 28800,  // seconds
  deviceInfo: {
    userAgent: "Chrome/...",
    ipAddress: "192.168.1.1"
  },
  isActive: false,
  lastActivityTime: ISODate("2025-01-15T17:59:00Z")
}

// Users Collection (Denormalized)
{
  _id: ObjectId("user123"),
  name: "Rahul",
  activeSessions: 1,
  lastLogin: ISODate("2025-01-15T10:00:00Z"),
  lastLogout: ISODate("2025-01-15T18:00:00Z")
}
```

**Solution:**

```javascript
// User login
async function userLogin(userId, deviceInfo) {
  const sessionId = generateUUID();
  const loginTime = new Date();

  const session = {
    userId: new ObjectId(userId),
    sessionId,
    loginTime,
    deviceInfo,
    isActive: true,
    lastActivityTime: loginTime
  };

  // Insert session
  const result = await db.sessions.insertOne(session);

  // Update user stats
  await db.users.updateOne(
    { _id: new ObjectId(userId) },
    {
      $set: { lastLogin: loginTime },
      $inc: { activeSessions: 1 }
    }
  );

  return sessionId;
}

// User logout
async function userLogout(sessionId) {
  const logoutTime = new Date();

  // Get session
  const session = await db.sessions.findOne({ sessionId });

  if (!session) throw new Error("Session not found");

  // Calculate duration
  const duration = (logoutTime - session.loginTime) / 1000;  // seconds

  // Update session
  await db.sessions.updateOne(
    { sessionId },
    {
      $set: {
        logoutTime,
        isActive: false,
        duration
      }
    }
  );

  // Update user stats
  await db.users.updateOne(
    { _id: session.userId },
    {
      $set: { lastLogout: logoutTime },
      $inc: { activeSessions: -1 }
    }
  );
}

// Update last activity (on every API call)
async function recordActivity(sessionId) {
  await db.sessions.updateOne(
    { sessionId },
    { $set: { lastActivityTime: new Date() } }
  );
}

// Get user active sessions
async function getActiveSessions(userId) {
  return db.sessions.find({
    userId: new ObjectId(userId),
    isActive: true
  }).toArray();
}

// Force logout stale sessions (30 minutes inactive)
async function forceLogoutStaleSessions() {
  const thirtyMinutesAgo = new Date(Date.now() - 30 * 60 * 1000);

  const staleSessions = await db.sessions.find({
    isActive: true,
    lastActivityTime: { $lt: thirtyMinutesAgo }
  }).toArray();

  for (const session of staleSessions) {
    await userLogout(session.sessionId);
  }
}

// Session analytics
async function getSessionAnalytics(userId) {
  return db.sessions.aggregate([
    { $match: { userId: new ObjectId(userId) } },
    {
      $group: {
        _id: null,
        totalSessions: { $sum: 1 },
        avgDuration: { $avg: "$duration" },
        totalDuration: { $sum: "$duration" },
        lastSession: { $max: "$logoutTime" }
      }
    },
    {
      $project: {
        _id: 0,
        totalSessions: 1,
        avgDuration: { $round: ["$avgDuration", 2] },
        totalDurationHours: { $divide: ["$totalDuration", 3600] },
        lastSession: 1
      }
    }
  ]).toArray();
}
```

**Indexes:**
```javascript
db.sessions.createIndex({ userId: 1, isActive: 1 })
db.sessions.createIndex({ sessionId: 1 }, { unique: true })
db.sessions.createIndex({ lastActivityTime: 1 }, { expireAfterSeconds: 86400 })  // Auto-delete after 24h
db.users.createIndex({ lastLogin: -1 })
```

**Interview Tips:**
- Session management critical production feature
- TTL index से auto-cleanup
- Denormalization (activeSessions) fast queries ke liye
- Activity tracking mein batch updates use karo
- Concurrent sessions handle karo

---

### Challenge 7: Search Implementation

**Problem:**
Product search - user "laptop" search kare to relevant results.

**Solution 1: Text Index (Full-text Search)**
```javascript
// Create text index
db.products.createIndex({ name: "text", description: "text", tags: "text" })

// Search
db.products.find({
  $text: { $search: "laptop gaming" }
}, {
  score: { $meta: "textScore" }
}).sort({
  score: { $meta: "textScore" }
}).limit(20)
```

**Solution 2: Regex Search (Simple but Slow)**
```javascript
db.products.find({
  name: { $regex: "laptop", $options: "i" }  // case-insensitive
}).limit(20)
```

**Solution 3: Autocomplete with Prefix**
```javascript
// Data model
{
  _id: ObjectId("..."),
  name: "MacBook Pro",
  nameTokens: ["m", "ma", "mac", "macb", "macbo", "macboo", "macbook", ...]
}

// Create index
db.products.createIndex({ nameTokens: 1 })

// Search for prefix "mac"
db.products.find({ nameTokens: "mac" }).limit(20)
```

**Solution 4: Aggregation Search (Most Flexible)**
```javascript
db.products.aggregate([
  {
    $match: {
      $or: [
        { name: { $regex: "laptop", $options: "i" } },
        { tags: { $in: ["laptop"] } },
        { category: "Electronics" }
      ]
    }
  },
  {
    $addFields: {
      relevance: {
        $cond: [
          { $regexMatch: { input: "$name", regex: "laptop" } },
          2,  // Exact match in name
          1   // Tag/category match
        ]
      }
    }
  },
  { $sort: { relevance: -1, rating: -1 } },
  { $limit: 20 }
])
```

---

### Challenge 8: Tricky Question - Circular References

**Problem:**
User A follow User B, User B follow User C, User C follow User A (circular). Find cycles.

**Solution:**
```javascript
async function findFollowCycles() {
  // Get follow relationships
  const users = await db.users.find({}).toArray();

  const graph = {};
  users.forEach(user => {
    graph[user._id.toString()] = user.following || [];
  });

  function findCycleDFS(userId, visited, recStack) {
    visited.add(userId);
    recStack.add(userId);

    const following = graph[userId] || [];

    for (const followingId of following) {
      if (!visited.has(followingId)) {
        if (findCycleDFS(followingId, visited, recStack)) {
          return true;
        }
      } else if (recStack.has(followingId)) {
        // Cycle found
        console.log(`Cycle detected: ${userId} -> ... -> ${followingId}`);
        return true;
      }
    }

    recStack.delete(userId);
    return false;
  }

  const visited = new Set();
  const cycles = [];

  for (const userId of Object.keys(graph)) {
    if (!visited.has(userId)) {
      const recStack = new Set();
      if (findCycleDFS(userId, visited, recStack)) {
        cycles.push(userId);
      }
    }
  }

  return cycles;
}
```

---

## ✅ Checkpoint 4: Coding Challenges Complete!

**Practiced:**
✅ Complex aggregation queries
✅ Duplicates finding & merging
✅ Pagination with sorting
✅ Nested array updates
✅ Batch processing (large datasets)
✅ Session management
✅ Search implementation
✅ Circular reference detection

---

## 🎯 Ready for LEVEL 5: TRICKY INTERVIEW QUESTIONS?

**Next Topics:**
- Questions interviewers commonly ask
- Edge cases & corner scenarios
- Performance problem diagnosis
- Architectural decision questions
- Confidence-building answers

**Type "next" for Tricky Questions!**
