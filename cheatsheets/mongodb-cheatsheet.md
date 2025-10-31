# MongoDB Cheatsheet

A comprehensive reference guide for MongoDB, covering everything from basic operations to advanced use cases and optimization techniques.

## Table of Contents
- [What is MongoDB?](#what-is-mongodb)
- [Installation & Setup](#installation--setup)
- [Basic Concepts](#basic-concepts)
- [CRUD Operations](#crud-operations)
- [Query Operations](#query-operations)
- [Aggregation Pipeline](#aggregation-pipeline)
- [Indexing](#indexing)
- [Data Modeling](#data-modeling)
- [Performance Optimization](#performance-optimization)
- [Common Use Cases](#common-use-cases)
- [Uncommon Use Cases](#uncommon-use-cases)
- [Security](#security)
- [Deployment & Monitoring](#deployment--monitoring)
- [Best Practices](#best-practices)

---

## What is MongoDB?

MongoDB is a NoSQL document database that stores data in flexible, JSON-like documents called BSON (Binary JSON). It's designed for horizontal scaling and high performance.

### Key Features:
- **Document-based**: Data stored as documents (similar to JSON)
- **Schema-less**: No fixed schema required
- **Horizontal scaling**: Sharding for large datasets
- **Rich queries**: Complex queries with aggregation
- **Indexing**: Multiple index types for performance
- **Replication**: High availability with replica sets

---

## Installation & Setup

### Installation

#### macOS (using Homebrew)
```bash
# Install MongoDB Community Edition
brew tap mongodb/brew
brew install mongodb-community

# Start MongoDB service
brew services start mongodb/brew/mongodb-community
```

#### Ubuntu/Debian
```bash
# Import MongoDB public key
wget -qO - https://www.mongodb.org/static/pgp/server-7.0.asc | sudo apt-key add -

# Add MongoDB repository
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list

# Install MongoDB
sudo apt-get update
sudo apt-get install -y mongodb-org

# Start MongoDB
sudo systemctl start mongod
sudo systemctl enable mongod
```

#### Docker
```bash
# Run MongoDB in Docker
docker run -d -p 27017:27017 --name mongodb mongo:latest

# With persistent data
docker run -d -p 27017:27017 -v mongodb_data:/data/db --name mongodb mongo:latest
```

### Connection

#### MongoDB Shell (mongosh)
```bash
# Connect to local MongoDB
mongosh

# Connect to remote MongoDB
mongosh "mongodb://username:password@host:port/database"

# Connect with connection string
mongosh "mongodb+srv://username:password@cluster.mongodb.net/database"
```

#### Node.js
```javascript
const { MongoClient } = require('mongodb');

const uri = "mongodb://localhost:27017";
const client = new MongoClient(uri);

async function connect() {
    try {
        await client.connect();
        console.log("Connected to MongoDB");
    } catch (error) {
        console.error("Connection failed:", error);
    }
}
```

#### Python
```python
from pymongo import MongoClient

# Connect to MongoDB
client = MongoClient('mongodb://localhost:27017/')

# Or with connection string
client = MongoClient('mongodb+srv://username:password@cluster.mongodb.net/')
```

---

## Basic Concepts

### Database
```javascript
// Switch to database (creates if doesn't exist)
use mydatabase

// List all databases
show dbs

// Current database
db.getName()
```

### Collections
```javascript
// Create collection
db.createCollection("users")

// List collections
show collections

// Drop collection
db.users.drop()
```

### Documents
```javascript
// Document structure (BSON)
{
  _id: ObjectId("507f1f77bcf86cd799439011"),
  name: "John Doe",
  age: 30,
  email: "john@example.com",
  address: {
    street: "123 Main St",
    city: "New York",
    zipCode: "10001"
  },
  hobbies: ["reading", "gaming", "cooking"],
  createdAt: new Date()
}
```

### Data Types
```javascript
// Common BSON data types
{
  string: "Hello World",
  number: 42,
  boolean: true,
  date: new Date(),
  objectId: ObjectId(),
  array: [1, 2, 3],
  null: null,
  undefined: undefined,
  binary: BinData(0, "base64data"),
  regex: /pattern/i,
  code: Code("function() { return 'hello'; }")
}
```

---

## CRUD Operations

### Create (Insert)

#### Single Document
```javascript
// Insert one document
db.users.insertOne({
  name: "John Doe",
  age: 30,
  email: "john@example.com"
})

// Insert with custom _id
db.users.insertOne({
  _id: "user123",
  name: "Jane Doe",
  age: 25
})
```

#### Multiple Documents
```javascript
// Insert many documents
db.users.insertMany([
  { name: "Alice", age: 28 },
  { name: "Bob", age: 32 },
  { name: "Charlie", age: 25 }
])

// Insert with options
db.users.insertMany([
  { name: "David", age: 30 }
], {
  ordered: false,  // Continue on errors
  writeConcern: { w: "majority" }
})
```

### Read (Query)

#### Basic Queries
```javascript
// Find all documents
db.users.find()

// Find with projection
db.users.find({}, { name: 1, email: 1 })

// Find one document
db.users.findOne({ name: "John Doe" })

// Count documents
db.users.countDocuments({ age: { $gte: 30 } })

// Limit results
db.users.find().limit(5)

// Skip documents
db.users.find().skip(10).limit(5)
```

#### Query Operators
```javascript
// Comparison operators
db.users.find({ age: { $gt: 25 } })        // Greater than
db.users.find({ age: { $gte: 25 } })       // Greater than or equal
db.users.find({ age: { $lt: 30 } })        // Less than
db.users.find({ age: { $lte: 30 } })       // Less than or equal
db.users.find({ age: { $ne: 25 } })        // Not equal
db.users.find({ age: { $in: [25, 30, 35] } }) // In array
db.users.find({ age: { $nin: [25, 30] } })    // Not in array

// Logical operators
db.users.find({
  $and: [
    { age: { $gte: 25 } },
    { age: { $lte: 35 } }
  ]
})

db.users.find({
  $or: [
    { name: "John" },
    { age: { $gt: 30 } }
  ]
})

db.users.find({
  $nor: [
    { age: { $lt: 18 } },
    { age: { $gt: 65 } }
  ]
})

db.users.find({
  $not: { age: { $lt: 18 } }
})
```

#### Array Queries
```javascript
// Array contains element
db.users.find({ hobbies: "reading" })

// Array contains all elements
db.users.find({ hobbies: { $all: ["reading", "gaming"] } })

// Array size
db.users.find({ hobbies: { $size: 3 } })

// Array element at position
db.users.find({ "hobbies.0": "reading" })

// Array with specific conditions
db.users.find({
  hobbies: {
    $elemMatch: {
      $regex: /^r/i
    }
  }
})
```

#### Nested Document Queries
```javascript
// Query nested fields
db.users.find({ "address.city": "New York" })

// Query with dot notation
db.users.find({ "address.zipCode": { $regex: /^10/ } })

// Query nested arrays
db.users.find({ "orders.items.name": "Laptop" })
```

### Update

#### Update One Document
```javascript
// Update single document
db.users.updateOne(
  { name: "John Doe" },
  { $set: { age: 31 } }
)

// Update with upsert (insert if not found)
db.users.updateOne(
  { email: "new@example.com" },
  {
    $set: {
      name: "New User",
      age: 25
    }
  },
  { upsert: true }
)
```

#### Update Multiple Documents
```javascript
// Update many documents
db.users.updateMany(
  { age: { $lt: 18 } },
  { $set: { status: "minor" } }
)
```

#### Update Operators
```javascript
// Set field value
db.users.updateOne(
  { name: "John" },
  { $set: { lastLogin: new Date() } }
)

// Unset field
db.users.updateOne(
  { name: "John" },
  { $unset: { tempField: "" } }
)

// Increment numeric value
db.users.updateOne(
  { name: "John" },
  { $inc: { loginCount: 1 } }
)

// Multiply numeric value
db.users.updateOne(
  { name: "John" },
  { $mul: { score: 1.1 } }
)

// Rename field
db.users.updateOne(
  { name: "John" },
  { $rename: { "oldField": "newField" } }
)

// Add to array
db.users.updateOne(
  { name: "John" },
  { $push: { hobbies: "swimming" } }
)

// Add to array if not exists
db.users.updateOne(
  { name: "John" },
  { $addToSet: { hobbies: "reading" } }
)

// Remove from array
db.users.updateOne(
  { name: "John" },
  { $pull: { hobbies: "gaming" } }
)

// Remove first/last from array
db.users.updateOne(
  { name: "John" },
  { $pop: { hobbies: 1 } }  // 1 for last, -1 for first
)

// Update array element
db.users.updateOne(
  { "hobbies": "gaming" },
  { $set: { "hobbies.$": "video games" } }
)
```

### Delete

#### Delete Documents
```javascript
// Delete one document
db.users.deleteOne({ name: "John Doe" })

// Delete many documents
db.users.deleteMany({ age: { $lt: 18 } })

// Delete all documents in collection
db.users.deleteMany({})
```

---

## Query Operations

### Sorting
```javascript
// Sort ascending
db.users.find().sort({ name: 1 })

// Sort descending
db.users.find().sort({ age: -1 })

// Sort by multiple fields
db.users.find().sort({ age: -1, name: 1 })
```

### Projection
```javascript
// Include specific fields
db.users.find({}, { name: 1, email: 1 })

// Exclude specific fields
db.users.find({}, { password: 0, _id: 0 })

// Include nested fields
db.users.find({}, { "address.city": 1, "address.zipCode": 1 })
```

### Text Search
```javascript
// Create text index
db.users.createIndex({ name: "text", email: "text" })

// Text search
db.users.find({ $text: { $search: "john doe" } })

// Text search with score
db.users.find(
  { $text: { $search: "john doe" } },
  { score: { $meta: "textScore" } }
).sort({ score: { $meta: "textScore" } })
```

### Regular Expressions
```javascript
// Case insensitive regex
db.users.find({ name: /john/i })

// Regex with options
db.users.find({ email: { $regex: /@gmail\.com$/, $options: "i" } })

// Regex with anchors
db.users.find({ name: { $regex: /^J/ } })  // Starts with J
db.users.find({ name: { $regex: /e$/ } })  // Ends with e
```

### Exists and Type
```javascript
// Field exists
db.users.find({ phone: { $exists: true } })

// Field doesn't exist
db.users.find({ phone: { $exists: false } })

// Field is null
db.users.find({ phone: null })

// Field is not null
db.users.find({ phone: { $ne: null } })

// Check data type
db.users.find({ age: { $type: "number" } })
db.users.find({ age: { $type: "string" } })
```

---

## Aggregation Pipeline

### Basic Aggregation
```javascript
// Simple aggregation
db.users.aggregate([
  { $match: { age: { $gte: 25 } } },
  { $group: { _id: "$city", count: { $sum: 1 } } },
  { $sort: { count: -1 } }
])
```

### Aggregation Stages

#### $match
```javascript
// Filter documents
db.users.aggregate([
  { $match: { age: { $gte: 18, $lte: 65 } } }
])
```

#### $group
```javascript
// Group by field
db.users.aggregate([
  { $group: {
    _id: "$city",
    totalUsers: { $sum: 1 },
    avgAge: { $avg: "$age" },
    maxAge: { $max: "$age" },
    minAge: { $min: "$age" }
  }}
])

// Group by multiple fields
db.users.aggregate([
  { $group: {
    _id: { city: "$city", status: "$status" },
    count: { $sum: 1 }
  }}
])

// Group all documents
db.users.aggregate([
  { $group: {
    _id: null,
    totalUsers: { $sum: 1 },
    avgAge: { $avg: "$age" }
  }}
])
```

#### $project
```javascript
// Reshape documents
db.users.aggregate([
  { $project: {
    name: 1,
    age: 1,
    isAdult: { $gte: ["$age", 18] },
    fullName: { $concat: ["$firstName", " ", "$lastName"] }
  }}
])

// Add computed fields
db.users.aggregate([
  { $project: {
    name: 1,
    age: 1,
    ageGroup: {
      $switch: {
        branches: [
          { case: { $lt: ["$age", 18] }, then: "Minor" },
          { case: { $lt: ["$age", 65] }, then: "Adult" }
        ],
        default: "Senior"
      }
    }
  }}
])
```

#### $lookup (Join)
```javascript
// Join collections
db.users.aggregate([
  { $lookup: {
    from: "orders",
    localField: "_id",
    foreignField: "userId",
    as: "orders"
  }}
])

// Join with pipeline
db.users.aggregate([
  { $lookup: {
    from: "orders",
    let: { userId: "$_id" },
    pipeline: [
      { $match: {
        $expr: { $eq: ["$userId", "$$userId"] },
        status: "completed"
      }},
      { $project: { total: 1, date: 1 } }
    ],
    as: "completedOrders"
  }}
])
```

#### $unwind
```javascript
// Unwind array
db.users.aggregate([
  { $unwind: "$hobbies" },
  { $group: { _id: "$hobbies", count: { $sum: 1 } } }
])

// Unwind with preserveNullAndEmptyArrays
db.users.aggregate([
  { $unwind: {
    path: "$hobbies",
    preserveNullAndEmptyArrays: true
  }}
])
```

#### $sort
```javascript
// Sort in aggregation
db.users.aggregate([
  { $sort: { age: -1, name: 1 } }
])
```

#### $limit and $skip
```javascript
// Pagination
db.users.aggregate([
  { $sort: { createdAt: -1 } },
  { $skip: 20 },
  { $limit: 10 }
])
```

### Advanced Aggregation Examples

#### User Analytics
```javascript
// User engagement analysis
db.users.aggregate([
  { $match: { createdAt: { $gte: new Date("2023-01-01") } } },
  { $group: {
    _id: {
      year: { $year: "$createdAt" },
      month: { $month: "$createdAt" }
    },
    newUsers: { $sum: 1 },
    avgAge: { $avg: "$age" }
  }},
  { $sort: { "_id.year": 1, "_id.month": 1 } }
])
```

#### Product Recommendations
```javascript
// Find users with similar purchase history
db.users.aggregate([
  { $match: { _id: ObjectId("user123") } },
  { $lookup: {
    from: "orders",
    localField: "_id",
    foreignField: "userId",
    as: "userOrders"
  }},
  { $unwind: "$userOrders" },
  { $unwind: "$userOrders.items" },
  { $lookup: {
    from: "orders",
    let: { productId: "$userOrders.items.productId" },
    pipeline: [
      { $match: {
        $expr: { $in: ["$$productId", "$items.productId"] },
        userId: { $ne: "$_id" }
      }},
      { $project: { userId: 1 } }
    ],
    as: "similarUsers"
  }},
  { $unwind: "$similarUsers" },
  { $group: { _id: "$similarUsers.userId", commonProducts: { $sum: 1 } } },
  { $sort: { commonProducts: -1 } },
  { $limit: 10 }
])
```

---

## Indexing

### Index Types

#### Single Field Index
```javascript
// Create single field index
db.users.createIndex({ email: 1 })

// Create descending index
db.users.createIndex({ createdAt: -1 })
```

#### Compound Index
```javascript
// Create compound index
db.users.createIndex({ city: 1, age: -1 })

// Compound index with multiple fields
db.users.createIndex({
  status: 1,
  city: 1,
  createdAt: -1
})
```

#### Multikey Index (Arrays)
```javascript
// Index on array field
db.users.createIndex({ hobbies: 1 })

// Index on nested array
db.users.createIndex({ "orders.items.productId": 1 })
```

#### Text Index
```javascript
// Create text index
db.users.createIndex({
  name: "text",
  email: "text",
  bio: "text"
})

// Text index with weights
db.users.createIndex(
  { name: "text", email: "text" },
  { weights: { name: 10, email: 5 } }
)
```

#### Geospatial Index
```javascript
// 2dsphere index for geospatial queries
db.places.createIndex({ location: "2dsphere" })

// 2d index for flat coordinates
db.places.createIndex({ coordinates: "2d" })
```

#### Partial Index
```javascript
// Index only documents matching condition
db.users.createIndex(
  { email: 1 },
  { partialFilterExpression: { status: "active" } }
)
```

#### Sparse Index
```javascript
// Index only documents with the field
db.users.createIndex(
  { phone: 1 },
  { sparse: true }
)
```

### Index Management
```javascript
// List all indexes
db.users.getIndexes()

// Drop index
db.users.dropIndex({ email: 1 })

// Drop all indexes except _id
db.users.dropIndexes()

// Get index usage stats
db.users.aggregate([{ $indexStats: {} }])
```

### Index Optimization
```javascript
// Explain query execution
db.users.find({ email: "john@example.com" }).explain()

// Explain with execution stats
db.users.find({ email: "john@example.com" }).explain("executionStats")

// Analyze query performance
db.users.find({
  city: "New York",
  age: { $gte: 25 }
}).explain("executionStats")
```

---

## Data Modeling

### Embedding vs Referencing

#### Embedding (One-to-One, One-to-Few)
```javascript
// User with embedded profile
{
  _id: ObjectId(),
  name: "John Doe",
  email: "john@example.com",
  profile: {
    bio: "Software developer",
    avatar: "avatar.jpg",
    preferences: {
      theme: "dark",
      notifications: true
    }
  }
}

// Blog post with embedded comments (few comments)
{
  _id: ObjectId(),
  title: "MongoDB Best Practices",
  content: "...",
  comments: [
    { author: "Alice", text: "Great post!", date: new Date() },
    { author: "Bob", text: "Very helpful", date: new Date() }
  ]
}
```

#### Referencing (One-to-Many, Many-to-Many)
```javascript
// User collection
{
  _id: ObjectId("user123"),
  name: "John Doe",
  email: "john@example.com"
}

// Orders collection
{
  _id: ObjectId("order456"),
  userId: ObjectId("user123"),
  items: [
    { productId: ObjectId("prod789"), quantity: 2 },
    { productId: ObjectId("prod101"), quantity: 1 }
  ],
  total: 99.99
}
```

### Schema Design Patterns

#### Tree Pattern
```javascript
// Hierarchical categories
{
  _id: ObjectId(),
  name: "Electronics",
  path: "/electronics",
  children: [
    { name: "Computers", path: "/electronics/computers" },
    { name: "Phones", path: "/electronics/phones" }
  ]
}
```

#### Bucket Pattern
```javascript
// Time-series data bucketing
{
  _id: ObjectId(),
  sensorId: "sensor001",
  date: ISODate("2023-01-01"),
  measurements: [
    { timestamp: ISODate("2023-01-01T00:00:00Z"), value: 25.5 },
    { timestamp: ISODate("2023-01-01T00:01:00Z"), value: 25.7 }
  ]
}
```

#### Polymorphic Pattern
```javascript
// Different document types in same collection
{
  _id: ObjectId(),
  type: "book",
  title: "MongoDB Guide",
  author: "John Doe",
  pages: 300
}

{
  _id: ObjectId(),
  type: "movie",
  title: "MongoDB Documentary",
  director: "Jane Smith",
  duration: 120
}
```

---

## Performance Optimization

### Query Optimization
```javascript
// Use projection to limit data transfer
db.users.find({ city: "New York" }, { name: 1, email: 1 })

// Use limit to reduce result set
db.users.find({ status: "active" }).limit(100)

// Use covered queries (index-only queries)
db.users.createIndex({ email: 1, name: 1 })
db.users.find({ email: "john@example.com" }, { _id: 0, name: 1 })
```

### Connection Pooling
```javascript
// Node.js connection pooling
const { MongoClient } = require('mongodb');

const client = new MongoClient(uri, {
  maxPoolSize: 10,
  minPoolSize: 2,
  maxIdleTimeMS: 30000,
  serverSelectionTimeoutMS: 5000,
  socketTimeoutMS: 45000,
});
```

### Caching Strategies
```javascript
// Application-level caching
const cache = new Map();

async function getUser(userId) {
  if (cache.has(userId)) {
    return cache.get(userId);
  }

  const user = await db.users.findOne({ _id: userId });
  cache.set(userId, user);
  return user;
}
```

### Sharding
```javascript
// Enable sharding
sh.enableSharding("mydatabase")

// Shard collection
sh.shardCollection("mydatabase.users", { userId: "hashed" })

// Add shard
sh.addShard("shard1.example.com:27017")
sh.addShard("shard2.example.com:27017")
```

---

## Common Use Cases

### E-commerce Platform
```javascript
// Product catalog
{
  _id: ObjectId(),
  name: "Laptop",
  category: "Electronics",
  price: 999.99,
  inventory: {
    inStock: 50,
    reserved: 5
  },
  attributes: {
    brand: "Dell",
    model: "XPS 13",
    specifications: {
      ram: "16GB",
      storage: "512GB SSD",
      processor: "Intel i7"
    }
  },
  reviews: [
    {
      userId: ObjectId(),
      rating: 5,
      comment: "Great laptop!",
      date: new Date()
    }
  ]
}

// Order processing
db.orders.insertOne({
  userId: ObjectId(),
  items: [
    { productId: ObjectId(), quantity: 2, price: 999.99 }
  ],
  shipping: {
    address: "123 Main St",
    method: "standard"
  },
  payment: {
    method: "credit_card",
    status: "completed"
  },
  status: "processing",
  total: 1999.98,
  createdAt: new Date()
})
```

### Content Management System
```javascript
// Blog posts with versioning
{
  _id: ObjectId(),
  title: "MongoDB Tutorial",
  slug: "mongodb-tutorial",
  content: "...",
  author: ObjectId(),
  tags: ["mongodb", "database", "tutorial"],
  status: "published",
  publishedAt: new Date(),
  versions: [
    {
      version: 1,
      content: "Initial version",
      updatedAt: new Date()
    }
  ],
  metadata: {
    views: 1250,
    likes: 45,
    comments: 12
  }
}

// User roles and permissions
{
  _id: ObjectId(),
  username: "admin",
  email: "admin@example.com",
  roles: ["admin", "editor"],
  permissions: [
    "create_post",
    "edit_post",
    "delete_post",
    "manage_users"
  ]
}
```

### Social Media Platform
```javascript
// User profiles
{
  _id: ObjectId(),
  username: "johndoe",
  displayName: "John Doe",
  bio: "Software developer",
  avatar: "avatar.jpg",
  followers: [ObjectId(), ObjectId()],
  following: [ObjectId(), ObjectId()],
  posts: [ObjectId(), ObjectId()],
  createdAt: new Date()
}

// Posts with engagement
{
  _id: ObjectId(),
  author: ObjectId(),
  content: "Just learned MongoDB!",
  media: ["image1.jpg", "image2.jpg"],
  likes: [ObjectId(), ObjectId()],
  comments: [
    {
      author: ObjectId(),
      content: "Great job!",
      createdAt: new Date()
    }
  ],
  shares: 5,
  createdAt: new Date()
}
```

### IoT Data Collection
```javascript
// Sensor data with time-series
{
  _id: ObjectId(),
  sensorId: "temp_sensor_001",
  location: {
    building: "Building A",
    floor: 3,
    room: "Room 301"
  },
  readings: [
    {
      timestamp: new Date(),
      temperature: 22.5,
      humidity: 45.2,
      pressure: 1013.25
    }
  ],
  metadata: {
    model: "DHT22",
    firmware: "1.2.3",
    battery: 85
  }
}

// Aggregated sensor data
db.sensor_data.aggregate([
  { $match: { sensorId: "temp_sensor_001" } },
  { $unwind: "$readings" },
  { $group: {
    _id: {
      year: { $year: "$readings.timestamp" },
      month: { $month: "$readings.timestamp" },
      day: { $dayOfMonth: "$readings.timestamp" },
      hour: { $hour: "$readings.timestamp" }
    },
    avgTemp: { $avg: "$readings.temperature" },
    maxTemp: { $max: "$readings.temperature" },
    minTemp: { $min: "$readings.temperature" }
  }},
  { $sort: { "_id.year": 1, "_id.month": 1, "_id.day": 1, "_id.hour": 1 } }
])
```

---

## Uncommon Use Cases

### Real-time Analytics Dashboard
```javascript
// Real-time user activity tracking
{
  _id: ObjectId(),
  userId: ObjectId(),
  sessionId: "session_123",
  events: [
    {
      type: "page_view",
      page: "/products",
      timestamp: new Date(),
      metadata: {
        referrer: "google.com",
        userAgent: "Mozilla/5.0...",
        ip: "192.168.1.1"
      }
    },
    {
      type: "click",
      element: "add_to_cart",
      productId: ObjectId(),
      timestamp: new Date()
    }
  ],
  sessionStart: new Date(),
  sessionEnd: new Date()
}

// Real-time aggregation
db.user_sessions.aggregate([
  { $match: { sessionStart: { $gte: new Date(Date.now() - 3600000) } } },
  { $unwind: "$events" },
  { $match: { "events.type": "purchase" } },
  { $group: {
    _id: { $minute: "$events.timestamp" },
    purchases: { $sum: 1 },
    revenue: { $sum: "$events.metadata.amount" }
  }},
  { $sort: { _id: 1 } }
])
```

### Machine Learning Feature Store
```javascript
// User behavior features
{
  _id: ObjectId(),
  userId: ObjectId(),
  features: {
    // Engagement features
    avgSessionDuration: 1200, // seconds
    pagesPerSession: 4.2,
    bounceRate: 0.15,

    // Purchase behavior
    totalOrders: 12,
    avgOrderValue: 89.99,
    daysSinceLastOrder: 5,

    // Content preferences
    favoriteCategories: ["electronics", "books"],
    preferredPriceRange: { min: 10, max: 200 },

    // Temporal patterns
    preferredShoppingHours: [19, 20, 21], // 7-9 PM
    preferredShoppingDays: [5, 6], // Friday, Saturday

    // Derived features
    customerLifetimeValue: 1079.88,
    churnProbability: 0.12,
    nextPurchasePrediction: new Date("2023-12-15")
  },
  lastUpdated: new Date()
}

// Feature engineering pipeline
db.user_features.aggregate([
  { $lookup: {
    from: "orders",
    localField: "userId",
    foreignField: "userId",
    as: "orders"
  }},
  { $addFields: {
    "features.totalOrders": { $size: "$orders" },
    "features.avgOrderValue": {
      $avg: "$orders.total"
    },
    "features.lastOrderDate": {
      $max: "$orders.createdAt"
    }
  }},
  { $addFields: {
    "features.daysSinceLastOrder": {
      $divide: [
        { $subtract: [new Date(), "$features.lastOrderDate"] },
        86400000 // milliseconds in a day
      ]
    }
  }}
])
```

### Blockchain Data Storage
```javascript
// Blockchain transaction storage
{
  _id: ObjectId(),
  blockNumber: 12345678,
  transactionHash: "0x1234567890abcdef...",
  from: "0xabcdef1234567890...",
  to: "0x9876543210fedcba...",
  value: "1000000000000000000", // in wei
  gasUsed: 21000,
  gasPrice: "20000000000", // 20 gwei
  timestamp: new Date(),
  status: "success",
  logs: [
    {
      address: "0xcontract123...",
      topics: ["0xevent_signature..."],
      data: "0xdata..."
    }
  ]
}

// Token transfer tracking
{
  _id: ObjectId(),
  tokenAddress: "0xtoken123...",
  from: "0xuser1...",
  to: "0xuser2...",
  amount: "1000000000000000000",
  blockNumber: 12345678,
  transactionHash: "0x1234567890abcdef...",
  timestamp: new Date()
}
```

### Geospatial Applications
```javascript
// Location-based services
{
  _id: ObjectId(),
  name: "Coffee Shop",
  type: "restaurant",
  location: {
    type: "Point",
    coordinates: [-74.0059, 40.7128] // [longitude, latitude]
  },
  address: "123 Main St, New York, NY",
  rating: 4.5,
  priceRange: "$$",
  hours: {
    monday: "7:00-20:00",
    tuesday: "7:00-20:00"
  },
  amenities: ["wifi", "outdoor_seating", "pet_friendly"]
}

// Geospatial queries
// Find nearby restaurants
db.places.find({
  location: {
    $near: {
      $geometry: {
        type: "Point",
        coordinates: [-74.0059, 40.7128]
      },
      $maxDistance: 1000 // 1km in meters
    }
  },
  type: "restaurant"
})

// Find restaurants within polygon
db.places.find({
  location: {
    $geoWithin: {
      $geometry: {
        type: "Polygon",
        coordinates: [[
          [-74.01, 40.71],
          [-74.00, 40.71],
          [-74.00, 40.72],
          [-74.01, 40.72],
          [-74.01, 40.71]
        ]]
      }
    }
  }
})
```

### Multi-tenant SaaS Applications
```javascript
// Tenant-aware data modeling
{
  _id: ObjectId(),
  tenantId: ObjectId("tenant123"),
  userId: ObjectId("user456"),
  email: "user@company.com",
  role: "admin",
  permissions: ["read", "write", "delete"],
  settings: {
    theme: "dark",
    notifications: true
  }
}

// Tenant isolation with compound indexes
db.users.createIndex({ tenantId: 1, email: 1 }, { unique: true })

// Tenant-scoped queries
function getUsersForTenant(tenantId) {
  return db.users.find({ tenantId: tenantId })
}

// Cross-tenant analytics (with proper permissions)
db.users.aggregate([
  { $group: {
    _id: "$tenantId",
    userCount: { $sum: 1 },
    activeUsers: {
      $sum: { $cond: [{ $eq: ["$status", "active"] }, 1, 0] }
    }
  }}
])
```

### Event Sourcing
```javascript
// Event store
{
  _id: ObjectId(),
  aggregateId: "order_123",
  aggregateType: "Order",
  eventType: "OrderCreated",
  eventData: {
    orderId: "order_123",
    customerId: "customer_456",
    items: [
      { productId: "prod_789", quantity: 2, price: 29.99 }
    ],
    total: 59.98
  },
  metadata: {
    userId: "user_789",
    timestamp: new Date(),
    version: 1
  },
  sequenceNumber: 1
}

// Event replay for rebuilding aggregates
db.events.aggregate([
  { $match: { aggregateId: "order_123" } },
  { $sort: { sequenceNumber: 1 } },
  { $group: {
    _id: "$aggregateId",
    events: { $push: "$$ROOT" }
  }}
])
```

---

## Security

### Authentication
```javascript
// Create user with roles
use admin
db.createUser({
  user: "appuser",
  pwd: "securepassword",
  roles: [
    { role: "readWrite", db: "myapp" },
    { role: "dbAdmin", db: "myapp" }
  ]
})

// Create user with custom role
db.createRole({
  role: "dataAnalyst",
  privileges: [
    {
      resource: { db: "myapp", collection: "analytics" },
      actions: ["find", "aggregate"]
    }
  ],
  roles: []
})
```

### Authorization
```javascript
// Field-level security
db.users.createIndex({ tenantId: 1, userId: 1 })

// Row-level security with application logic
function getUserData(userId, tenantId) {
  return db.users.findOne({
    _id: userId,
    tenantId: tenantId
  })
}
```

### Encryption
```javascript
// Client-side field level encryption
const { ClientEncryption } = require('mongodb-client-encryption');

const encryption = new ClientEncryption(client, {
  keyVaultNamespace: 'encryption.__keyVault',
  kmsProviders: {
    local: {
      key: Buffer.from('32byteslongkeyforencryption123456')
    }
  }
});

// Encrypt sensitive data
const encryptedSSN = await encryption.encrypt('123-45-6789', {
  keyId: keyId,
  algorithm: 'AEAD_AES_256_CBC_HMAC_SHA_512-Deterministic'
});
```

### Network Security
```javascript
// Bind to specific interfaces
net:
  port: 27017
  bindIp: 127.0.0.1,10.0.0.1

// Enable SSL/TLS
net:
  ssl:
    mode: requireSSL
    certificateKeyFile: /path/to/server.pem
    CAFile: /path/to/ca.pem
```

---

## Deployment & Monitoring

### Replica Sets
```javascript
// Initialize replica set
rs.initiate({
  _id: "rs0",
  members: [
    { _id: 0, host: "mongodb1:27017" },
    { _id: 1, host: "mongodb2:27017" },
    { _id: 2, host: "mongodb3:27017" }
  ]
})

// Add member to replica set
rs.add("mongodb4:27017")

// Remove member from replica set
rs.remove("mongodb4:27017")
```

### Sharding
```javascript
// Enable sharding
sh.enableSharding("mydatabase")

// Shard collection
sh.shardCollection("mydatabase.users", { userId: "hashed" })

// Add shard
sh.addShard("shard1.example.com:27017")
sh.addShard("shard2.example.com:27017")

// Check shard status
sh.status()
```

### Monitoring
```javascript
// Server status
db.serverStatus()

// Database stats
db.stats()

// Collection stats
db.users.stats()

// Index usage
db.users.aggregate([{ $indexStats: {} }])

// Current operations
db.currentOp()

// Kill operation
db.killOp(operationId)
```

### Backup and Restore
```bash
# Backup database
mongodump --host localhost:27017 --db mydatabase --out /backup/

# Restore database
mongorestore --host localhost:27017 --db mydatabase /backup/mydatabase/

# Backup with compression
mongodump --host localhost:27017 --db mydatabase --gzip --archive=backup.gz

# Restore from compressed backup
mongorestore --host localhost:27017 --gzip --archive=backup.gz
```

---

## Best Practices

### Schema Design
```javascript
// 1. Use appropriate data types
{
  _id: ObjectId(),
  age: 25,           // Number, not string
  isActive: true,    // Boolean, not string
  createdAt: new Date() // Date, not string
}

// 2. Use consistent field names
{
  firstName: "John",    // camelCase
  lastName: "Doe",     // camelCase
  emailAddress: "john@example.com" // camelCase
}

// 3. Design for your queries
// If you query by email frequently, index it
db.users.createIndex({ email: 1 })

// 4. Use embedded documents for related data
{
  _id: ObjectId(),
  name: "John Doe",
  address: {
    street: "123 Main St",
    city: "New York",
    zipCode: "10001"
  }
}
```

### Performance
```javascript
// 1. Use projection to limit fields
db.users.find({}, { name: 1, email: 1 })

// 2. Use limit for large result sets
db.users.find().limit(100)

// 3. Use covered queries
db.users.createIndex({ email: 1, name: 1 })
db.users.find({ email: "john@example.com" }, { _id: 0, name: 1 })

// 4. Use compound indexes efficiently
db.users.createIndex({ city: 1, age: -1, createdAt: -1 })
```

### Security
```javascript
// 1. Use authentication
db.createUser({
  user: "appuser",
  pwd: "securepassword",
  roles: [{ role: "readWrite", db: "myapp" }]
})

// 2. Use authorization
db.createRole({
  role: "readOnly",
  privileges: [
    { resource: { db: "myapp", collection: "" }, actions: ["find"] }
  ],
  roles: []
})

// 3. Use network security
// Bind to specific interfaces
// Use SSL/TLS
// Use firewalls
```

### Maintenance
```javascript
// 1. Regular index maintenance
db.users.reIndex()

// 2. Monitor slow queries
db.setProfilingLevel(2, { slowms: 100 })
db.system.profile.find().sort({ ts: -1 }).limit(5)

// 3. Regular backups
// Use mongodump/mongorestore
// Test restore procedures

// 4. Monitor performance
db.serverStatus()
db.stats()
db.users.stats()
```

---

## Common Patterns and Anti-patterns

### Good Patterns
```javascript
// 1. Use ObjectId for references
{
  userId: ObjectId("507f1f77bcf86cd799439011"),
  orderId: ObjectId("507f1f77bcf86cd799439012")
}

// 2. Use arrays for one-to-few relationships
{
  _id: ObjectId(),
  name: "Blog Post",
  tags: ["mongodb", "database", "nosql"]
}

// 3. Use embedded documents for related data
{
  _id: ObjectId(),
  name: "User",
  profile: {
    bio: "Software developer",
    avatar: "avatar.jpg"
  }
}
```

### Anti-patterns
```javascript
// 1. Don't use arrays for large one-to-many relationships
// BAD: Storing thousands of comments in an array
{
  _id: ObjectId(),
  title: "Blog Post",
  comments: [/* thousands of comments */]
}

// GOOD: Use separate collection
{
  _id: ObjectId(),
  postId: ObjectId(),
  author: ObjectId(),
  content: "Great post!"
}

// 2. Don't use strings for numbers
// BAD
{
  age: "25",
  price: "99.99"
}

// GOOD
{
  age: 25,
  price: 99.99
}

// 3. Don't create too many indexes
// Each index uses storage and slows writes
// Only create indexes for queries you actually use
```

---

## Troubleshooting

### Common Issues

#### Connection Issues
```javascript
// Check connection
db.runCommand({ ping: 1 })

// Check server status
db.serverStatus()

// Check current operations
db.currentOp()
```

#### Performance Issues
```javascript
// Enable profiling
db.setProfilingLevel(2, { slowms: 100 })

// Check slow queries
db.system.profile.find().sort({ ts: -1 }).limit(10)

// Analyze query performance
db.users.find({ email: "john@example.com" }).explain("executionStats")
```

#### Memory Issues
```javascript
// Check memory usage
db.serverStatus().mem

// Check index usage
db.users.aggregate([{ $indexStats: {} }])

// Check collection stats
db.users.stats()
```

---

## Useful Commands Reference

### Database Operations
```javascript
// Show databases
show dbs

// Use database
use mydatabase

// Show collections
show collections

// Drop database
db.dropDatabase()
```

### Collection Operations
```javascript
// Create collection
db.createCollection("users")

// Drop collection
db.users.drop()

// Rename collection
db.users.renameCollection("customers")
```

### Index Operations
```javascript
// List indexes
db.users.getIndexes()

// Create index
db.users.createIndex({ email: 1 })

// Drop index
db.users.dropIndex({ email: 1 })

// Rebuild indexes
db.users.reIndex()
```

### Utility Commands
```javascript
// Get help
help

// Show current database
db.getName()

// Show database stats
db.stats()

// Show collection stats
db.users.stats()

// Show server status
db.serverStatus()

// Show current operations
db.currentOp()

// Kill operation
db.killOp(operationId)
```

---
---

# MongoDB Cheat Sheet 🧩

*A complete guide with real-world use cases*

---

## 🏁 Getting Started

### Connect MongoDB Shell

```bash
mongo                             # Connect to localhost (mongodb://127.0.0.1:27017)
mongo --host <host> --port <port> -u <user> -p <pwd>
mongo "mongodb://192.168.1.1:27017"
mongo "mongodb+srv://cluster.mongodb.net/<dbname>" --username <username>
```

**💡 Use case:**
You connect locally during development (`mongo`), but use Atlas in production:
`mongo "mongodb+srv://mycluster.mongodb.net/appdb" --username admin`.

---

### Helpers

```js
show dbs                // list all databases
use <database_name>     // switch database
show collections         // list all collections
db                      // show current database
load("script.js")       // run JS script
```

**💡 Use case:**
Before testing your query in production, switch safely:
`use staging`, then `show collections`.

---

## 🧱 CRUD Operations

### Create

```js
db.users.insertOne({ name: "Alice", createdAt: ISODate() })
db.products.insertMany([{ name: "Phone" }, { name: "Tablet" }])
```

**💡 Use case:**
When a user signs up:

```js
db.users.insertOne({
  username: "johndoe",
  email: "john@example.com",
  status: "active",
  createdAt: ISODate()
})
```

---

### Read (Find)

```js
db.users.findOne()
db.users.find().pretty()
db.users.find({ age: { $gte: 18 } })
db.users.find({}).limit(10).skip(20).sort({ createdAt: -1 })
```

**💡 Use case:**
Paginate a “User List” page:

```js
db.users.find({ status: "active" }).sort({ createdAt: -1 }).skip(20).limit(10)
```

---

### Update

```js
db.products.updateOne({ _id: 1 }, { $set: { price: 200 } })
db.products.updateMany({ category: "electronics" }, { $inc: { stock: 10 } })
```

**💡 Use case:**
Black Friday sale — reduce all electronics price by 10%:

```js
db.products.updateMany({ category: "electronics" }, { $mul: { price: 0.9 } })
```

---

### Delete

```js
db.users.deleteOne({ username: "john" })
db.logs.deleteMany({ level: "debug" })
```

**💡 Use case:**
Automatically delete expired session data:

```js
db.sessions.deleteMany({ expiresAt: { $lt: new Date() } })
```

---

### Upsert (Update or Insert)

```js
db.settings.updateOne(
  { key: "theme" },
  { $set: { value: "dark" } },
  { upsert: true }
)
```

**💡 Use case:**
If user settings don’t exist, create them with default preferences.

---

### Replace

```js
db.users.replaceOne({ username: "olduser" }, { username: "newuser", email: "new@example.com" })
```

**💡 Use case:**
Full document replacement during data migration.

---

### Array Operations

```js
db.users.updateOne({ _id: 1 }, { $push: { tags: "premium" } })
db.users.updateOne({ _id: 1 }, { $pull: { tags: "trial" } })
db.users.updateOne({ _id: 1 }, { $addToSet: { tags: "verified" } })
```

**💡 Use case:**
In a subscription app, tag users based on purchase:

```js
db.users.updateOne({ _id: userId }, { $addToSet: { tags: "subscribed" } })
```

---

## 🔍 Query Operators

### Comparison

```js
db.orders.find({ total: { $gt: 100 } })
db.orders.find({ total: { $lte: 500 } })
db.orders.find({ status: { $ne: "cancelled" } })
```

**💡 Use case:**
Find all orders with total between 100 and 500:

```js
db.orders.find({ total: { $gte: 100, $lte: 500 } })
```

---

### Logical

```js
db.products.find({ $or: [{ stock: { $lt: 10 } }, { sale: true }] })
```

**💡 Use case:**
Dashboard alert for low-stock or on-sale products.

---

### Element Queries

```js
db.users.find({ email: { $exists: true } })
db.users.find({ phone: { $type: "string" } })
```

**💡 Use case:**
Validate data completeness — find users missing phone number.

---

### Regex (Pattern Match)

```js
db.customers.find({ name: /^John/i })
```

**💡 Use case:**
Search user by first name (case-insensitive) in a CRM search bar.

---

## 📊 Aggregation Framework

### Basic Pipeline

```js
db.orders.aggregate([
  { $match: { status: "completed" } },
  { $group: { _id: "$customerId", totalSpent: { $sum: "$amount" } } },
  { $sort: { totalSpent: -1 } }
])
```

**💡 Use case:**
E-commerce dashboard: top 10 customers by total spending.

---

### Count, Distinct

```js
db.orders.countDocuments({ status: "completed" })
db.orders.distinct("customerId")
```

**💡 Use case:**
Count how many customers have made at least one order.

---

### Array Aggregation

```js
db.students.aggregate([
  { $unwind: "$grades" },
  { $group: { _id: "$_id", avgGrade: { $avg: "$grades" } } }
])
```

**💡 Use case:**
Calculate each student’s average score.

---

## 🧭 Indexing

### Create Index

```js
db.users.createIndex({ email: 1 }, { unique: true })
db.products.createIndex({ category: 1, price: -1 })
db.locations.createIndex({ coordinates: "2dsphere" })
```

**💡 Use case:**

* Unique email validation in user registration
* Geolocation search for nearby stores:

```js
db.stores.find({
  location: {
    $near: {
      $geometry: { type: "Point", coordinates: [106.8, -6.2] },
      $maxDistance: 1000
    }
  }
})
```

---

### TTL Index

```js
db.sessions.createIndex({ expiresAt: 1 }, { expireAfterSeconds: 0 })
```

**💡 Use case:**
Automatically delete expired sessions after `expiresAt` time.

---

### Text Search

```js
db.articles.createIndex({ title: "text", body: "text" })
db.articles.find({ $text: { $search: "mongodb indexing" } })
```

**💡 Use case:**
Blog search feature that finds articles by keyword relevance.

---

## 🗂 Database & Collection Management

### Create Collection with Validation

```js
db.createCollection("users", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["username", "email"],
      properties: {
        username: { bsonType: "string" },
        email: { bsonType: "string", pattern: "@example\\.com$" }
      }
    }
  }
})
```

**💡 Use case:**
Ensure that every user has a valid company email before insert.

---

### Drop Collection or Database

```js
db.logs.drop()
db.dropDatabase()
```

**💡 Use case:**
After migrating log data to cloud storage, safely drop old logs.

---

### Rename Collection

```js
db.old_users.renameCollection("users_backup")
```

**💡 Use case:**
Before refactoring user schema, back up old collection.

---

## ⚙️ Administration

### Users and Roles

```js
use admin
db.createUser({
  user: "admin",
  pwd: passwordPrompt(),
  roles: ["root"]
})
```

**💡 Use case:**
Grant full privileges to DevOps lead managing cluster operations.

---

### Server & Stats

```js
db.stats()
db.serverStatus()
db.printCollectionStats()
```

**💡 Use case:**
Monitor database size, index usage, and performance metrics.

---

## 🧩 Replica Sets

```js
rs.initiate({
  _id: "replicaApp",
  members: [
    { _id: 0, host: "127.0.0.1:27017" },
    { _id: 1, host: "127.0.0.1:27018" }
  ]
})
rs.status()
```

**💡 Use case:**
Set up local replication for failover testing before deploying production.

---

## 🌍 Sharding

```js
sh.addShard("rs1/mongo1.example.net:27017")
sh.shardCollection("app.users", { region: 1 })
```

**💡 Use case:**
Distribute data by `region` to scale globally — Asia, Europe, and US.

---

## ⚡ Change Streams (Real-time updates)

```js
const stream = db.orders.watch([{ $match: { operationType: "insert" } }])
while (stream.hasNext()) {
  print("New order:", tojson(stream.next()))
}
```

**💡 Use case:**
Real-time notification system — trigger webhook when a new order is placed.

---
