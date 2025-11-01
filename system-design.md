# 🏗️ System Design Interview Preparation Coach
## Complete Guide: HLD (High-Level Design) + LLD (Low-Level Design)

---

## 📚 Table of Contents

### Part 1: Fundamentals
1. [What is System Design?](#what-is-system-design)
2. [HLD vs LLD - Core Differences](#hld-vs-lld)
3. [Why Companies Test System Design](#why-companies-test)

### Part 2: HLD (High-Level Design)
4. [Core Concepts](#hld-core-concepts)
5. [System Components](#hld-components)
6. [Scaling Strategies](#scaling-strategies)
7. [Real-World HLD Problems](#hld-problems)

### Part 3: LLD (Low-Level Design)
8. [OOP Fundamentals](#lld-oop)
9. [Design Patterns](#design-patterns)
10. [SOLID Principles](#solid-principles)
11. [Real-World LLD Problems](#lld-problems)

### Part 4: Interview Strategy
12. [How to Approach System Design Questions](#approach)
13. [Tricky Interview Questions](#tricky-questions)
14. [Common Pitfalls](#pitfalls)

---

## Part 1: Fundamentals

<a name="what-is-system-design"></a>
### 1. What is System Design?

**Q: What is System Design in the context of software engineering?**

**A:** System Design is the process of defining the architecture, components, modules, interfaces, and data flow of a system to satisfy specified requirements.

**Deep Explanation:**

System design involves making strategic decisions about:
- **Architecture**: How components interact
- **Data Storage**: Database choices, schemas
- **Scalability**: Handling growth
- **Reliability**: Fault tolerance, redundancy
- **Performance**: Speed, efficiency
- **Security**: Authentication, authorization, encryption

**Two Main Levels:**

1. **HLD (High-Level Design)**: Bird's eye view - system architecture, components, data flow
2. **LLD (Low-Level Design)**: Detailed view - classes, methods, data structures, algorithms

**Real-World Example:**
```
Building a house:
- HLD = Architecture blueprint (rooms, layout, utilities)
- LLD = Detailed plans (electrical wiring, plumbing specifics, materials)
```

---

<a name="hld-vs-lld"></a>
### 2. HLD vs LLD - Core Differences

**Q: What's the difference between High-Level Design and Low-Level Design?**

**A:** HLD focuses on system architecture and component interactions, while LLD focuses on implementation details within each component.

**Detailed Comparison:**

| Aspect | HLD | LLD |
|--------|-----|-----|
| **Scope** | Entire system | Individual modules/components |
| **Abstraction** | High-level components | Classes, methods, algorithms |
| **Audience** | Architects, stakeholders | Developers, engineers |
| **Diagrams** | Architecture, flow diagrams | UML, class diagrams, sequence |
| **Concerns** | Scalability, availability | Code structure, design patterns |
| **Questions** | "How do components communicate?" | "What classes do we need?" |
| **Example** | Load balancer → App servers → DB | User class with login() method |

**Interview Scenario:**

```
Design Instagram:

HLD Answer:
- Client apps → CDN for images
- API Gateway → Microservices (Feed, Upload, User)
- Database: PostgreSQL for users, Cassandra for feeds
- Redis cache for hot data
- S3 for image storage
- Message queue for async processing

LLD Answer (for Upload Service):
- Classes: ImageUploader, ImageValidator, StorageManager
- Methods: validateImage(), compressImage(), uploadToS3()
- Design Patterns: Factory (for different storage types), Strategy (for compression)
- Error handling, retry logic, transaction management
```

---

<a name="why-companies-test"></a>
### 3. Why Companies Test System Design

**Q: Why do tech companies focus so heavily on system design interviews?**

**A:** System design interviews reveal how candidates think about real-world problems, scalability, and trade-offs - skills critical for senior roles.

**Deep Reasoning:**

1. **Assesses Real-World Skills**
   - Building scalable systems is core to tech companies
   - Tests problem-solving beyond algorithms

2. **Evaluates Communication**
   - Can you explain complex ideas clearly?
   - Can you collaborate with stakeholders?

3. **Tests Experience Level**
   - Junior: May struggle with scaling concepts
   - Senior: Should know trade-offs, best practices

4. **Reveals Thought Process**
   - Do you ask clarifying questions?
   - Do you consider edge cases?
   - Do you think about trade-offs?

5. **Practical Application**
   - Unlike algorithm questions, system design directly relates to daily work
   - Shows if you can build production systems

**What Interviewers Look For:**
- Structured thinking
- Knowledge of distributed systems
- Understanding of trade-offs (CAP theorem, consistency vs availability)
- Scalability awareness
- Communication skills
- Real-world experience

---

## Part 2: HLD (High-Level Design)

<a name="hld-core-concepts"></a>
### 4. HLD Core Concepts

#### 4.1 Scalability

**Q: What is scalability and why does it matter?**

**A:** Scalability is a system's ability to handle increased load by adding resources. It's critical because user growth shouldn't break your system.

**Deep Dive:**

**Types of Scalability:**

1. **Vertical Scaling (Scale Up)**
   - Add more power to existing machine (CPU, RAM, disk)
   - Pros: Simple, no code changes
   - Cons: Hardware limits, expensive, single point of failure
   - Example: Upgrading from 8GB to 32GB RAM

2. **Horizontal Scaling (Scale Out)**
   - Add more machines
   - Pros: No limits, fault tolerant, cost-effective
   - Cons: Complex (load balancing, data distribution)
   - Example: Adding 10 more servers behind load balancer

**Real-World Trade-off:**
```
Startup (100 users):
✓ Vertical scaling - Simple, fast to implement
✗ Horizontal scaling - Overkill, premature optimization

Enterprise (10M users):
✗ Vertical scaling - Can't handle load, expensive
✓ Horizontal scaling - Necessary for reliability
```

**Interview Insight:**
Always ask about current and expected scale:
- "How many users do we have now?"
- "What's the expected growth?"
- "What's the read/write ratio?"

---

#### 4.2 Availability

**Q: What is availability and how do you measure it?**

**A:** Availability is the percentage of time a system is operational. Measured in "nines" (99.9%, 99.99%).

**Availability Table:**

| Availability | Downtime per Year | Downtime per Month | Use Case |
|--------------|-------------------|--------------------|--------------------|
| 99% (2 nines) | 3.65 days | 7.2 hours | Internal tools |
| 99.9% (3 nines) | 8.76 hours | 43.2 minutes | Standard web apps |
| 99.99% (4 nines) | 52.6 minutes | 4.32 minutes | E-commerce |
| 99.999% (5 nines) | 5.26 minutes | 25.9 seconds | Banking, healthcare |

**Achieving High Availability:**

1. **Redundancy**
   - Multiple servers, databases
   - No single point of failure

2. **Load Balancing**
   - Distribute traffic
   - Health checks

3. **Failover Mechanisms**
   - Automatic switching to backup
   - Database replication

4. **Monitoring & Alerts**
   - Detect issues early
   - Auto-recovery

**Real Example:**
```
Netflix (99.99% availability):
- Multi-region deployment
- Chaos engineering (intentionally breaking things to test)
- Circuit breakers
- Automatic failover
- Microservices isolation
```

---

#### 4.3 Reliability

**Q: What's the difference between availability and reliability?**

**A:** Availability = system is up. Reliability = system works correctly when up.

**Deep Explanation:**

```
Scenario: E-commerce checkout

High Availability, Low Reliability:
- System is up 99.99% of time ✓
- But 10% of payments fail ✗
- Users see errors, lose trust

High Reliability, Lower Availability:
- System is up 99.5% of time
- When up, 0.01% payment failure rate ✓
- Better user experience during uptime
```

**Ensuring Reliability:**

1. **Data Integrity**
   - ACID transactions
   - Data validation
   - Backup and recovery

2. **Testing**
   - Unit, integration, load tests
   - Chaos engineering

3. **Error Handling**
   - Graceful degradation
   - Retry mechanisms
   - Circuit breakers

4. **Monitoring**
   - Error rates
   - Success metrics
   - Alerting

---

#### 4.4 Performance (Latency & Throughput)

**Q: What's the difference between latency and throughput?**

**A:**
- **Latency**: Time to complete a single request (milliseconds)
- **Throughput**: Number of requests handled per second (requests/sec)

**Analogy:**
```
Highway traffic:
- Latency = Time for one car to travel from A to B
- Throughput = Number of cars passing per hour
```

**Performance Numbers Every Engineer Should Know:**

| Operation | Latency |
|-----------|---------|
| L1 cache reference | 0.5 ns |
| L2 cache reference | 7 ns |
| RAM access | 100 ns |
| SSD random read | 150 μs |
| HDD seek | 10 ms |
| Network within datacenter | 0.5 ms |
| Network across continents | 150 ms |

**Optimizing Performance:**

1. **Caching**
   - Redis, Memcached
   - CDN for static content
   - Browser caching

2. **Database Optimization**
   - Indexing
   - Query optimization
   - Connection pooling

3. **Asynchronous Processing**
   - Message queues
   - Background jobs

4. **Content Delivery**
   - CDNs
   - Compression
   - Minification

**Real-World Example:**
```
YouTube Video Streaming:

Latency Goals:
- Initial load: < 2 seconds
- Seek time: < 1 second
- Playback: smooth (buffering < 1%)

Throughput Goals:
- 1 billion hours watched/day
- Billions of requests/day

Solutions:
- CDN for video delivery (low latency)
- Adaptive bitrate streaming
- Edge caching
- Load balancing (high throughput)
```

---

#### 4.5 CAP Theorem

**Q: What is the CAP Theorem and why does it matter?**

**A:** CAP states that in a distributed system, you can only guarantee 2 out of 3: Consistency, Availability, Partition Tolerance.

**The Three Guarantees:**

1. **Consistency (C)**: All nodes see the same data at the same time
2. **Availability (A)**: Every request receives a response (success/failure)
3. **Partition Tolerance (P)**: System works despite network failures

**Why You Can't Have All Three:**

```
Scenario: Network partition occurs (servers can't communicate)

Option 1 - Choose CP (Consistency + Partition Tolerance):
- Block writes until partition heals
- Ensures consistency
- Sacrifices availability

Option 2 - Choose AP (Availability + Partition Tolerance):
- Accept writes on both sides
- System stays available
- Risk of inconsistent data
```

**Real-World Applications:**

| System | Choice | Reasoning |
|--------|--------|-----------|
| Banks | CP | Money must be consistent |
| Social media | AP | Okay if likes lag slightly |
| DNS | AP | Better available than consistent |
| MongoDB | CP | Configurable |
| Cassandra | AP | Eventually consistent |

**Interview Deep Dive:**

```
Q: Design a chat application. CAP choice?

A: Choose AP (Availability + Partition Tolerance)

Reasoning:
- Users expect messages to send even with network issues
- Okay if messages arrive out of order briefly
- Eventually consistent is acceptable
- Use:
  - Message queues
  - Local storage + sync
  - Conflict resolution (last-write-wins or vector clocks)

Trade-off:
- Messages might appear in different order temporarily
- Better than blocking message sending
```

---

<a name="hld-components"></a>
### 5. HLD System Components

#### 5.1 Load Balancers

**Q: What is a load balancer and when do you need one?**

**A:** A load balancer distributes incoming traffic across multiple servers to ensure no single server is overwhelmed.

**Why Load Balancers Matter:**
```
Without LB:
Client → Single Server (crashes at high load)

With LB:
Client → Load Balancer → [Server1, Server2, Server3]
```

**Load Balancing Algorithms:**

1. **Round Robin**
   - Sends requests sequentially to each server
   - Simple, works when servers are identical
   - Doesn't consider server load

2. **Least Connections**
   - Routes to server with fewest active connections
   - Good for long-lived connections
   - Better than round robin for uneven loads

3. **Weighted Round Robin**
   - Servers have weights based on capacity
   - More powerful servers get more traffic
   - Useful for heterogeneous servers

4. **IP Hash**
   - Hash client IP to determine server
   - Same client always goes to same server
   - Good for session persistence

5. **Least Response Time**
   - Routes to server with fastest response
   - Best for optimal performance
   - More complex to implement

**Types of Load Balancers:**

1. **Layer 4 (Transport Layer)**
   - Works at TCP/UDP level
   - Fast, simple
   - Can't inspect HTTP content
   - Example: AWS NLB

2. **Layer 7 (Application Layer)**
   - Works at HTTP level
   - Can route based on URL, headers, cookies
   - SSL termination
   - Example: AWS ALB, Nginx

**Real-World Architecture:**
```
Internet → CDN → Layer 7 LB → [Web Servers]
                            → Layer 4 LB → [App Servers]
                                        → Layer 4 LB → [DB Replicas]
```

---

#### 5.2 Caching

**Q: What is caching and where should you cache?**

**A:** Caching stores frequently accessed data in fast storage to reduce latency and database load.

**Cache Hierarchy:**

```
Client → [Browser Cache]
      → [CDN Cache]
      → [Server-side Cache (Redis)]
      → [Database Query Cache]
      → Database
```

**Where to Cache:**

1. **Client-Side (Browser)**
   - Static assets (CSS, JS, images)
   - Use HTTP cache headers
   - Example: Cache-Control: max-age=86400

2. **CDN (Edge Locations)**
   - Images, videos, static content
   - Geographically distributed
   - Example: Cloudflare, AWS CloudFront

3. **Application Cache (Redis/Memcached)**
   - Session data
   - API responses
   - Database query results
   - Hot data

4. **Database Cache**
   - Query result cache
   - Connection pooling

**Cache Strategies:**

1. **Cache-Aside (Lazy Loading)**
```
Read:
1. Check cache
2. If miss → query DB
3. Store in cache
4. Return data

Write:
1. Update DB
2. Invalidate cache
```

Pros: Cache only what's needed
Cons: Cache miss penalty, stale data risk

2. **Write-Through**
```
Write:
1. Write to cache
2. Immediately write to DB
3. Return success

Read:
1. Always read from cache
```

Pros: Cache always up-to-date
Cons: Write latency, unused data in cache

3. **Write-Behind (Write-Back)**
```
Write:
1. Write to cache
2. Async write to DB (batched)

Read:
1. Read from cache
```

Pros: Fast writes, batching
Cons: Risk of data loss, complexity

4. **Refresh-Ahead**
```
- Proactively refresh cache before expiry
- Based on usage patterns
```

Pros: Low latency for hot data
Cons: Complex prediction logic

**Cache Eviction Policies:**

1. **LRU (Least Recently Used)**
   - Remove oldest accessed item
   - Most common
   - Good for general use

2. **LFU (Least Frequently Used)**
   - Remove least accessed item
   - Good for identifying truly cold data
   - More complex tracking

3. **FIFO (First In First Out)**
   - Remove oldest item
   - Simple but less effective

4. **TTL (Time To Live)**
   - Items expire after set time
   - Good for time-sensitive data

**Cache Challenges:**

1. **Cache Invalidation**
   - "There are only two hard things in Computer Science: cache invalidation and naming things"
   - When to invalidate? How to invalidate?

2. **Cache Stampede**
   - Many requests hit expired cache simultaneously
   - All query DB at once
   - Solution: Lock or probabilistic early expiration

3. **Cache Consistency**
   - Keeping cache and DB in sync
   - Especially with multiple cache nodes

**Real Example - Twitter Timeline:**
```
User loads feed:
1. Check Redis for cached timeline
2. If miss:
   - Query DB for latest tweets
   - Aggregate from followed users
   - Store in Redis (TTL: 5 min)
3. Return cached timeline

When user tweets:
- Invalidate timelines of all followers
- Or use fan-out on write
```

---

#### 5.3 Databases

**Q: SQL vs NoSQL - when to use which?**

**A:**
- **SQL**: Structured data, complex queries, ACID transactions (banking, e-commerce)
- **NoSQL**: Unstructured data, high scale, flexibility (social media, analytics)

**SQL Databases:**

**Best For:**
- Structured, relational data
- Complex queries, joins
- ACID compliance
- Financial transactions

**Examples & Use Cases:**
- PostgreSQL: General purpose, rich features
- MySQL: Web applications, WordPress
- Oracle: Enterprise applications

**Pros:**
- Strong consistency
- Mature, well-understood
- Rich query language (SQL)
- Transactions

**Cons:**
- Harder to scale horizontally
- Schema changes can be painful
- Can be slow at massive scale

**NoSQL Databases:**

1. **Document Stores (MongoDB, CouchDB)**
```json
{
  "_id": "user123",
  "name": "John",
  "posts": [
    {"title": "Hello", "likes": 10},
    {"title": "World", "likes": 20}
  ]
}
```

Best For: Flexible schema, nested data
Use Case: Content management, catalogs

2. **Key-Value Stores (Redis, DynamoDB)**
```
user:123 → {"name": "John", "age": 30}
session:abc → {"userId": 123, "expires": 1234567890}
```

Best For: Simple lookups, caching, sessions
Use Case: Caching, session management

3. **Column-Family (Cassandra, HBase)**
```
Row: user123
  Column Family: profile
    name: John
    age: 30
  Column Family: posts
    post1: {...}
    post2: {...}
```

Best For: Wide columns, time-series, analytics
Use Case: IoT data, metrics, logs

4. **Graph Databases (Neo4j, Amazon Neptune)**
```
(User:John)-[:FOLLOWS]->(User:Jane)
(User:John)-[:LIKES]->(Post:123)
```

Best For: Relationships, social networks
Use Case: Social graphs, recommendations

**Database Selection Decision Tree:**

```
Need ACID transactions? → SQL
Need complex queries/joins? → SQL
Need flexible schema? → NoSQL (Document)
Need massive scale (billions of records)? → NoSQL (Cassandra)
Need super fast lookups? → NoSQL (Key-Value)
Need graph relationships? → NoSQL (Graph)
```

**Scaling Databases:**

1. **Replication**
```
Master (writes) → [Slave1, Slave2, Slave3] (reads)
```
- Increases read capacity
- Provides redundancy
- Master can be bottleneck for writes

2. **Sharding (Horizontal Partitioning)**
```
Users A-M → Shard 1
Users N-Z → Shard 2
```

Strategies:
- Hash-based: hash(userId) % num_shards
- Range-based: A-M, N-Z
- Geography-based: US users, EU users

Pros: Distributes load, scales writes
Cons: Complex, cross-shard queries difficult

3. **Partitioning (Vertical)**
```
UserProfile table → Server 1
UserPosts table → Server 2
UserFriends table → Server 3
```

**Real-World Example - Instagram:**
```
Users, Posts metadata → PostgreSQL (structured, relational)
Images → S3 (object storage)
Feed cache → Redis (fast lookups)
Activity logs → Cassandra (massive write volume)
Graph relationships → Custom graph store
```

---

#### 5.4 Message Queues

**Q: What are message queues and why use them?**

**A:** Message queues enable asynchronous communication between services, decoupling producers and consumers.

**Why Message Queues:**

```
Without Queue (Synchronous):
User uploads video → API waits → [Transcode, Generate thumbnails, Update DB]
                     (User waits 30 seconds) ✗

With Queue (Asynchronous):
User uploads video → API returns immediately ✓
                   → Queue → Background workers process
```

**Benefits:**

1. **Decoupling**: Services don't need to know about each other
2. **Scalability**: Add more workers to handle load
3. **Reliability**: Messages persisted, retry on failure
4. **Asynchronous**: Users don't wait for slow operations

**Popular Message Queues:**

1. **RabbitMQ**
   - Full-featured, complex
   - Multiple routing patterns
   - Good for enterprise

2. **Apache Kafka**
   - High throughput
   - Message retention (replay)
   - Good for event streaming, logs

3. **AWS SQS**
   - Managed, simple
   - Pay per use
   - Good for cloud-native apps

4. **Redis (Pub/Sub, Streams)**
   - Fast, simple
   - Good for real-time

**Message Queue Patterns:**

1. **Point-to-Point (Queue)**
```
Producer → Queue → Consumer 1
                 Consumer 2
                 Consumer 3

(Each message consumed once)
```

Use Case: Task processing

2. **Publish-Subscribe (Topic)**
```
Publisher → Topic → Subscriber 1
                   Subscriber 2
                   Subscriber 3

(Each message consumed by all subscribers)
```

Use Case: Event broadcasting

3. **Request-Reply**
```
Client → Request Queue → Server
      ← Reply Queue ←
```

Use Case: RPC-style communication

**Real-World Example - E-commerce Order:**

```
User places order:

1. Order Service → Queue: "OrderPlaced" event
2. Multiple consumers:
   - Inventory Service: Decrease stock
   - Payment Service: Charge card
   - Email Service: Send confirmation
   - Analytics Service: Track metrics
   - Shipping Service: Create label

Each service processes independently, at its own pace
If one fails, it can retry without affecting others
```

**Key Considerations:**

1. **Message Durability**
   - Persist to disk?
   - What if broker crashes?

2. **Message Ordering**
   - FIFO guaranteed?
   - Important for financial transactions

3. **At-Least-Once vs Exactly-Once**
   - At-Least-Once: Message may be delivered multiple times
   - Exactly-Once: Hard to achieve, expensive
   - Make consumers idempotent

4. **Dead Letter Queues**
   - Where failed messages go
   - For manual inspection/retry

---

#### 5.5 CDN (Content Delivery Network)

**Q: What is a CDN and when should you use one?**

**A:** A CDN is a geographically distributed network of servers that cache and serve content from locations near users.

**How CDN Works:**

```
Without CDN:
User in Tokyo → Server in US (150ms latency) ✗

With CDN:
User in Tokyo → CDN Edge in Tokyo (5ms latency) ✓
```

**CDN Architecture:**
```
Origin Server (Your server)
     ↓
CDN PoPs (Points of Presence) worldwide
 - Tokyo
 - London
 - New York
 - Sydney
     ↓
Users globally (low latency)
```

**What to Put on CDN:**

✓ **Good for CDN:**
- Images, videos
- CSS, JavaScript files
- Downloadable files
- Static HTML pages

✗ **Not for CDN:**
- Dynamic, personalized content
- User-specific data
- Real-time data

**CDN Features:**

1. **Edge Caching**
   - Cache content at edge locations
   - TTL (Time To Live)
   - Cache headers

2. **Cache Invalidation**
   - Purge cache when content updates
   - URL purge, tag-based purge

3. **SSL/TLS Termination**
   - Handle HTTPS at edge
   - Reduce load on origin

4. **DDoS Protection**
   - Absorb attack traffic
   - Large distributed capacity

5. **Compression**
   - Gzip, Brotli
   - Reduce bandwidth

**Popular CDNs:**
- Cloudflare
- AWS CloudFront
- Fastly
- Akamai

**Real Example - Netflix:**
```
Netflix Content Delivery:

1. Master video files → AWS S3 (origin)
2. Transcode to multiple formats
3. Push to CDN (Open Connect)
4. Users stream from nearest CDN server
5. Result:
   - Low latency globally
   - Reduced origin server load
   - Better user experience
```

---

<a name="scaling-strategies"></a>
### 6. Scaling Strategies

#### 6.1 Microservices vs Monolith

**Q: Microservices vs Monolith - which to choose?**

**A:**
- **Monolith**: Single codebase, good for small teams, simple deployment
- **Microservices**: Multiple services, good for large teams, complex coordination

**Monolithic Architecture:**

```
Single Application:
┌─────────────────────┐
│  User Management    │
│  Product Catalog    │
│  Order Processing   │
│  Payment            │
│  Shared Database    │
└─────────────────────┘
```

**Pros:**
- Simple to develop initially
- Easy to test (everything together)
- Simple deployment (one artifact)
- No network latency between components

**Cons:**
- Tight coupling
- Hard to scale (must scale entire app)
- Deployment risk (one bug breaks everything)
- Technology lock-in

**When to Use:**
- Startups, MVPs
- Small teams
- Simple domains
- Uncertain requirements

**Microservices Architecture:**

```
API Gateway
    ↓
┌──────────────┐ ┌──────────────┐ ┌──────────────┐
│ User Service │ │Product Service│ │ Order Service│
│   + DB       │ │   + DB       │ │   + DB       │
└──────────────┘ └──────────────┘ └──────────────┘
```

**Pros:**
- Independent deployment
- Technology diversity
- Fault isolation
- Team autonomy
- Easier to scale specific services

**Cons:**
- Operational complexity
- Network latency
- Data consistency challenges
- Testing complexity
- Debugging difficulty

**When to Use:**
- Large organizations
- Multiple teams
- Need to scale independently
- Different technology needs

**Migration Path:**

```
Phase 1: Monolith
  ↓
Phase 2: Modular Monolith (loose coupling within monolith)
  ↓
Phase 3: Extract critical services (payment, authentication)
  ↓
Phase 4: Full microservices (if needed)
```

**Real Example - Amazon:**

```
Early 2000s: Monolith
- Everything in one codebase
- Scaling challenges
- Deployment bottlenecks

Mid 2000s: Microservices migration
- Hundreds of services
- Two-pizza teams
- Independent deployment
- Massive scale achieved

Result:
- Can deploy every second
- Teams move independently
- Services scale independently
```

---

#### 6.2 Database Sharding

**Q: What is sharding and when do you need it?**

**A:** Sharding splits a database horizontally across multiple servers, distributing data and load.

**Why Shard:**

```
Single Database:
- 1TB data
- 10k writes/sec
- Hardware limits reached ✗

Sharded (4 shards):
- 250GB each
- 2.5k writes/sec each
- Linear scalability ✓
```

**Sharding Strategies:**

1. **Hash-Based Sharding**

```python
shard_id = hash(user_id) % num_shards

user_id: 12345 → hash → 3 → Shard 3
user_id: 67890 → hash → 1 → Shard 1
```

Pros: Even distribution
Cons: Rebalancing difficult (adding shards)

2. **Range-Based Sharding**

```
Shard 1: user_id 1-1,000,000
Shard 2: user_id 1,000,001-2,000,000
Shard 3: user_id 2,000,001-3,000,000
```

Pros: Range queries easier
Cons: Uneven distribution (hotspots)

3. **Geography-Based Sharding**

```
Shard 1: US users
Shard 2: EU users
Shard 3: Asia users
```

Pros: Data locality, compliance (GDPR)
Cons: Uneven sizes

4. **Entity-Based Sharding**

```
Shard by tenant_id (for SaaS):
Shard 1: Companies A-D
Shard 2: Companies E-H
```

Pros: Data isolation, easy queries
Cons: Some tenants larger than others

**Sharding Challenges:**

1. **Cross-Shard Queries**

```
Problem:
Get all orders for products in category "Electronics"
- Products on Shard 1
- Orders on Shard 2 (sharded by user_id)
- Need to join across shards ✗

Solutions:
- Denormalize data
- Use search index (Elasticsearch)
- Application-level joins
```

2. **Shard Key Selection**

```
Bad shard key: timestamp
- Recent data all on one shard (hot shard)

Good shard key: user_id
- Even distribution
- Users rarely cross shards
```

3. **Rebalancing**

```
Adding new shard:
- Redistribute data
- Update routing logic
- Minimize downtime

Techniques:
- Consistent hashing
- Virtual shards
```

4. **Transactions Across Shards**

```
Problem:
Transfer money between users on different shards
- Need ACID guarantees
- Distributed transaction required

Solutions:
- Two-phase commit (slow, complex)
- Saga pattern (eventual consistency)
- Avoid when possible
```

**Real Example - Instagram:**

```
Sharding strategy:
- Shard by user_id (photos belong to user)
- Hash-based sharding
- Consistent hashing for rebalancing

Benefits:
- User's photos on same shard (fast queries)
- Billions of photos distributed
- Independent shard failures

Trade-offs:
- Explore page (photos from many users) needs fan-out queries
- Use Cassandra for feed (different sharding strategy)
```

---

#### 6.3 Caching Strategies Deep Dive

**Q: How do you handle cache invalidation in a distributed system?**

**A:** Cache invalidation is one of the hardest problems. Common strategies: TTL, write-through, event-based invalidation, and versioning.

**The Challenge:**

```
User updates profile:
1. Update database ✓
2. Cache still has old data ✗
3. Other users see stale data ✗

How long is staleness acceptable?
How to invalidate efficiently?
```

**Strategies:**

1. **TTL (Time To Live)**

```python
cache.set("user:123", user_data, ttl=300)  # 5 minutes

# After 5 minutes, cache expires
# Next request fetches fresh data
```

Pros: Simple, automatic cleanup
Cons: Stale data until expiry, cache misses

Best for: Data that changes infrequently

2. **Write-Through Invalidation**

```python
def update_user(user_id, new_data):
    db.update(user_id, new_data)
    cache.delete(f"user:{user_id}")  # Invalidate
    # Next read will cache miss and refresh
```

Pros: Data always fresh
Cons: Extra invalidation logic

3. **Event-Based Invalidation**

```python
# After DB update, publish event
event_bus.publish("user.updated", {"user_id": 123})

# Cache layer subscribes
@subscribe("user.updated")
def invalidate_cache(event):
    cache.delete(f"user:{event.user_id}")
```

Pros: Decoupled, scales
Cons: Complexity, eventual consistency

4. **Cache Versioning**

```python
version = get_current_version()
cache_key = f"user:{user_id}:v{version}"

# On update, increment version
# Old cache ignored, no invalidation needed
```

Pros: No invalidation needed
Cons: Multiple versions in cache, memory waste

**Distributed Cache Challenges:**

1. **Cache Coherence**

```
Problem:
- 3 cache nodes
- User updates profile
- How do all nodes know?

Solutions:
- Cache invalidation messages
- Use Redis cluster (built-in replication)
- Consistent hashing
```

2. **Thundering Herd**

```
Problem:
- Cache expires
- 10k concurrent requests
- All hit database simultaneously

Solution: Cache stampede prevention
```python
def get_with_lock(key):
    data = cache.get(key)
    if data:
        return data

    # Try to acquire lock
    if cache.set_nx(f"lock:{key}", "1", ttl=10):
        # This request refreshes cache
        data = db.query()
        cache.set(key, data, ttl=300)
        cache.delete(f"lock:{key}")
        return data
    else:
        # Other requests wait and retry
        time.sleep(0.1)
        return get_with_lock(key)
```

3. **Cascading Failures**

```
Problem:
- Cache goes down
- All requests hit database
- Database overloads and crashes

Solution: Circuit breaker
```python
class CircuitBreaker:
    def call_database(self):
        if self.failure_rate > THRESHOLD:
            # Circuit open: don't hit database
            return cached_fallback()

        try:
            return db.query()
        except:
            self.record_failure()
            raise
```

**Real Example - Facebook:**

```
Cache infrastructure:
- Memcached for social graph data
- Millions of cache servers
- Regional cache clusters

Invalidation:
1. User updates profile in DB
2. DB publishes to message bus
3. Cache invalidation service listens
4. Invalidates in all regional caches
5. Next read fetches fresh data

Challenges:
- Ensure all regions invalidate
- Handle partial failures
- Eventual consistency acceptable
```

---

<a name="hld-problems"></a>
### 7. Real-World HLD Problems

#### 7.1 Design a URL Shortener (like bit.ly)

**Q: Design a URL shortening service like bit.ly.**

**Clarifying Questions:**

1. Scale: How many URLs shortened per day? → 100 million
2. Read/write ratio? → 100:1 (more reads)
3. URL expiration? → Optional, default no expiry
4. Custom short URLs? → Yes
5. Analytics? → Yes (click tracking)

**Requirements:**

Functional:
- Shorten long URL → short URL
- Redirect short URL → original URL
- Custom short URLs (optional)
- URL expiration (optional)
- Analytics (click counts)

Non-functional:
- High availability (99.99%)
- Low latency (< 100ms redirect)
- Scalable (billions of URLs)

**High-Level Design:**

```
┌─────────────────────────────────────────────────┐
│                  Clients                        │
└─────────────────────────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────┐
│                Load Balancer                    │
└─────────────────────────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────┐
│             API Servers (Stateless)             │
│  POST /shorten  |  GET /{shortUrl}              │
└─────────────────────────────────────────────────┘
                      ↓
         ┌────────────┴────────────┐
         ↓                         ↓
┌─────────────────┐      ┌─────────────────┐
│  Redis Cache    │      │  PostgreSQL     │
│  (Hot URLs)     │      │  (All URLs)     │
└─────────────────┘      └─────────────────┘
```

**Database Schema:**

```sql
CREATE TABLE urls (
  id BIGSERIAL PRIMARY KEY,
  short_code VARCHAR(10) UNIQUE NOT NULL,
  original_url TEXT NOT NULL,
  user_id BIGINT,
  created_at TIMESTAMP DEFAULT NOW(),
  expires_at TIMESTAMP,
  click_count BIGINT DEFAULT 0
);

CREATE INDEX idx_short_code ON urls(short_code);
CREATE INDEX idx_user_id ON urls(user_id);

CREATE TABLE clicks (
  id BIGSERIAL PRIMARY KEY,
  short_code VARCHAR(10) NOT NULL,
  clicked_at TIMESTAMP DEFAULT NOW(),
  ip_address INET,
  user_agent TEXT,
  referrer TEXT
);

CREATE INDEX idx_short_code_clicked ON clicks(short_code, clicked_at);
```

**Short URL Generation Algorithm:**

```python
# Base62 encoding (a-z, A-Z, 0-9) = 62 characters
# 7 characters = 62^7 = 3.5 trillion combinations

import hashlib

def generate_short_code(original_url, attempt=0):
    # Hash the URL + attempt number
    hash_input = f"{original_url}{attempt}".encode()
    hash_value = hashlib.md5(hash_input).hexdigest()

    # Take first 7 characters
    short_code = base62_encode(int(hash_value[:8], 16))[:7]

    # Check if exists in DB
    if db.exists(short_code):
        # Collision: try again with incremented attempt
        return generate_short_code(original_url, attempt + 1)

    return short_code

def base62_encode(num):
    chars = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ"
    if num == 0:
        return chars[0]

    result = []
    while num:
        result.append(chars[num % 62])
        num //= 62

    return ''.join(reversed(result))
```

**Alternative: Counter-Based (Simpler, Faster)**

```python
# Use auto-incrementing ID, encode as Base62
def shorten_url(original_url):
    # Insert into DB, get auto-increment ID
    id = db.insert(original_url)  # e.g., 12345678

    # Encode ID as Base62
    short_code = base62_encode(id)  # e.g., "dQw4w9W"

    return f"https://short.ly/{short_code}"
```

**API Design:**

```
POST /api/shorten
Request:
{
  "url": "https://example.com/very/long/url",
  "custom_alias": "mylink",  // optional
  "expires_at": "2024-12-31"  // optional
}

Response:
{
  "short_url": "https://short.ly/dQw4w9W",
  "original_url": "https://example.com/very/long/url",
  "created_at": "2024-01-15T10:30:00Z"
}

---

GET /{shortCode}
Response: 302 Redirect to original URL
Header: Location: https://example.com/very/long/url
```

**Caching Strategy:**

```python
# Read flow (GET /{shortCode})
def redirect(short_code):
    # 1. Check Redis cache (hot URLs)
    cached = redis.get(f"url:{short_code}")
    if cached:
        analytics.track_click_async(short_code)
        return redirect(cached['url'])

    # 2. Cache miss: query database
    url_data = db.query(
        "SELECT original_url, expires_at FROM urls WHERE short_code = ?",
        short_code
    )

    if not url_data:
        return 404

    if url_data.expires_at and url_data.expires_at < now():
        return 410  # Gone

    # 3. Cache for future (TTL: 1 hour)
    redis.set(f"url:{short_code}", url_data, ttl=3600)

    # 4. Track analytics asynchronously
    analytics.track_click_async(short_code)

    return redirect(url_data.original_url)
```

**Analytics (Async)**

```python
# Don't slow down redirects with analytics writes
def track_click_async(short_code):
    message = {
        "short_code": short_code,
        "timestamp": now(),
        "ip": request.ip,
        "user_agent": request.user_agent,
        "referrer": request.referrer
    }

    # Push to message queue
    queue.publish("clicks", message)

# Background worker processes queue
def process_click_events():
    for message in queue.consume("clicks"):
        # Batch writes to database
        db.insert_click(message)

        # Update counter (can use Redis for real-time)
        redis.incr(f"clicks:{message.short_code}")
```

**Scaling Considerations:**

1. **Database Sharding**
```
Shard by short_code:
Shard 1: short_codes starting with [0-9, a-m]
Shard 2: short_codes starting with [n-z, A-M]
Shard 3: short_codes starting with [N-Z]
```

2. **Read Replicas**
```
Master (writes) → Slaves (reads)
- Analytics queries go to replicas
- Don't slow down redirects
```

3. **CDN for Static Content**
```
If short URL is for static resource:
- Redirect to CDN URL
- Reduce origin load
```

4. **Rate Limiting**
```
# Prevent abuse
@rate_limit(max=10, per=60)  # 10 URLs per minute per IP
def shorten_url():
    pass
```

**Capacity Estimation:**

```
Assumptions:
- 100M new URLs per day
- 100:1 read/write ratio (10B redirects per day)
- 500 bytes per URL record
- 5 years retention

Storage:
- 100M × 365 × 5 = 183B URLs
- 183B × 500 bytes = 91.5 TB

Bandwidth:
- Write: 100M × 500 bytes / 86400 sec = 580 MB/s
- Read: 10B × 500 bytes / 86400 sec = 58 GB/s

QPS:
- Write: 100M / 86400 = ~1,200 QPS
- Read: 10B / 86400 = ~115,000 QPS
```

**Trade-offs:**

1. **Hash-based vs Counter-based**
   - Hash: Random IDs, potential collisions
   - Counter: Sequential IDs (security concern?), simpler

2. **Strong vs Eventual Consistency**
   - Strong: Slower, always correct
   - Eventual: Faster, rare duplicate risk

3. **Synchronous vs Async Analytics**
   - Sync: Accurate, slower redirects
   - Async: Fast redirects, eventual accuracy

---

#### 7.2 Design Instagram

**Q: Design Instagram (photo sharing platform)**

**Clarifying Questions:**

1. Scale: Users? → 1 billion users, 100M DAU
2. Features: Core features? → Upload photos, follow users, view feed, like/comment
3. Platform: Mobile app? Web? → Both
4. Storage: Photo storage? → Yes, need to handle billions of photos

**Requirements:**

Functional:
- Upload photos (with filters)
- Follow/unfollow users
- View personalized feed
- Like, comment on photos
- View user profiles
- Search users

Non-functional:
- High availability
- Low latency for feed (< 500ms)
- Scalable (billions of photos)
- Eventual consistency acceptable for likes/comments

**High-Level Architecture:**

```
                    ┌──────────────┐
                    │   Clients    │
                    │  (iOS/Android│
                    │     /Web)    │
                    └──────────────┘
                           ↓
                    ┌──────────────┐
                    │     CDN      │
                    │  (Images)    │
                    └──────────────┘
                           ↓
                    ┌──────────────┐
                    │ Load Balancer│
                    └──────────────┘
                           ↓
        ┌──────────────────┴──────────────────┐
        ↓                                      ↓
┌──────────────┐                      ┌──────────────┐
│  API Gateway │                      │   WebSocket  │
└──────────────┘                      │   (Real-time)│
        ↓                              └──────────────┘
┌───────────────────────────────────────────────────┐
│              Microservices                        │
├──────────────┬──────────────┬─────────────────────┤
│User Service  │ Feed Service │ Upload Service      │
│              │              │                     │
│Photo Service │Media Service │ Notification Service│
└──────────────┴──────────────┴─────────────────────┘
        ↓              ↓               ↓
┌──────────────┐ ┌──────────┐  ┌──────────────┐
│  PostgreSQL  │ │Cassandra │  │     S3       │
│  (Users,     │ │(Feeds,   │  │  (Photos)    │
│   Metadata)  │ │ Activity)│  │              │
└──────────────┘ └──────────┘  └──────────────┘
        ↓
┌──────────────┐
│    Redis     │
│   (Cache)    │
└──────────────┘
```

**Database Design:**

```sql
-- PostgreSQL (User data, metadata)
CREATE TABLE users (
  user_id BIGSERIAL PRIMARY KEY,
  username VARCHAR(50) UNIQUE NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL,
  profile_pic_url TEXT,
  bio TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE photos (
  photo_id BIGSERIAL PRIMARY KEY,
  user_id BIGINT REFERENCES users(user_id),
  image_url TEXT NOT NULL,
  caption TEXT,
  location VARCHAR(255),
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE follows (
  follower_id BIGINT REFERENCES users(user_id),
  followee_id BIGINT REFERENCES users(user_id),
  created_at TIMESTAMP DEFAULT NOW(),
  PRIMARY KEY (follower_id, followee_id)
);

CREATE INDEX idx_follows_follower ON follows(follower_id);
CREATE INDEX idx_follows_followee ON follows(followee_id);

-- Cassandra (Activity data - high write volume)
-- Feed table
CREATE TABLE user_feed (
  user_id BIGINT,
  photo_id BIGINT,
  photo_owner_id BIGINT,
  created_at TIMESTAMP,
  PRIMARY KEY (user_id, created_at, photo_id)
) WITH CLUSTERING ORDER BY (created_at DESC);

-- Likes table
CREATE TABLE photo_likes (
  photo_id BIGINT,
  user_id BIGINT,
  created_at TIMESTAMP,
  PRIMARY KEY (photo_id, user_id)
);

-- Comments table
CREATE TABLE photo_comments (
  photo_id BIGINT,
  comment_id BIGINT,
  user_id BIGINT,
  comment_text TEXT,
  created_at TIMESTAMP,
  PRIMARY KEY (photo_id, created_at, comment_id)
) WITH CLUSTERING ORDER BY (created_at DESC);
```

**Key Flows:**

**1. Photo Upload Flow:**

```
Client uploads photo:

1. Client → Upload Service
   - Pre-signed URL from S3
   - Client uploads directly to S3

2. Upload Service receives notification
   - Generate thumbnails (multiple sizes)
   - Apply filters if requested
   - Store in S3 with different keys:
     - original: /photos/123456/original.jpg
     - large: /photos/123456/large.jpg
     - medium: /photos/123456/medium.jpg
     - thumbnail: /photos/123456/thumb.jpg

3. Store metadata in PostgreSQL
   - photo_id, user_id, s3_urls, caption, etc.

4. Fan-out to followers (async)
   - Get user's followers from cache/DB
   - Insert into each follower's feed (Cassandra)
   - Push notification to active followers

5. Update user's photo count (Redis counter)

6. Return success to client
```

```python
# Upload service
async def upload_photo(user_id, image, caption):
    # 1. Upload to S3
    s3_key = f"photos/{uuid.uuid4()}/original.jpg"
    s3.upload(image, s3_key)

    # 2. Process asynchronously (thumbnails, filters)
    queue.publish("process_photo", {
        "s3_key": s3_key,
        "user_id": user_id,
        "caption": caption
    })

    # 3. Store metadata
    photo_id = db.insert_photo({
        "user_id": user_id,
        "image_url": s3_key,
        "caption": caption
    })

    # 4. Fan out to followers (async)
    queue.publish("fanout_photo", {
        "photo_id": photo_id,
        "user_id": user_id
    })

    return {"photo_id": photo_id, "status": "processing"}

# Background worker
async def process_photo(message):
    s3_key = message['s3_key']

    # Download original
    image = s3.download(s3_key)

    # Generate thumbnails
    thumbnails = {
        "large": resize(image, 1080),
        "medium": resize(image, 640),
        "thumb": resize(image, 320)
    }

    # Upload thumbnails
    for size, img in thumbnails.items():
        s3.upload(img, f"{s3_key}/{size}.jpg")

    # Update metadata
    db.update_photo(message['photo_id'], {
        "processed": True,
        "thumbnail_urls": thumbnails
    })

# Fan-out worker
async def fanout_photo(message):
    user_id = message['user_id']
    photo_id = message['photo_id']

    # Get followers (from cache if available)
    followers = cache.get(f"followers:{user_id}")
    if not followers:
        followers = db.get_followers(user_id)
        cache.set(f"followers:{user_id}", followers, ttl=3600)

    # Insert into each follower's feed (batch write)
    cassandra.batch_insert("user_feed", [
        {
            "user_id": follower_id,
            "photo_id": photo_id,
            "photo_owner_id": user_id,
            "created_at": now()
        }
        for follower_id in followers
    ])

    # Push notifications to online followers
    online_followers = presence_service.get_online(followers)
    for follower_id in online_followers:
        push_notification(follower_id, "New photo from user!")
```

**2. Feed Generation Flow:**

```
Two approaches: Fan-out on write vs Fan-out on read

Instagram uses hybrid:
- Fan-out on write for regular users
- Fan-out on read for celebrities (millions of followers)
```

**Fan-out on Write (Pre-computed feeds):**

```python
# When user posts photo:
async def post_photo(user_id, photo):
    photo_id = save_photo(photo)

    # Get all followers
    followers = db.get_followers(user_id)

    # Insert into each follower's pre-computed feed
    for follower_id in followers:
        cassandra.insert("user_feed", {
            "user_id": follower_id,
            "photo_id": photo_id,
            "created_at": now()
        })

# When user loads feed:
async def get_feed(user_id):
    # Feed already pre-computed!
    feed = cassandra.query(
        "SELECT * FROM user_feed WHERE user_id = ? LIMIT 50",
        user_id
    )
    return feed  # Fast!
```

Pros: Fast reads
Cons: Slow writes for celebrities, lots of writes

**Fan-out on Read (Compute on demand):**

```python
# When user loads feed:
async def get_feed(user_id):
    # Get users I follow
    following = db.get_following(user_id)

    # Get recent photos from each
    all_photos = []
    for followee_id in following:
        photos = db.get_user_photos(followee_id, limit=10)
        all_photos.extend(photos)

    # Merge and sort
    feed = sorted(all_photos, key=lambda x: x.created_at, reverse=True)[:50]
    return feed
```

Pros: Fast writes
Cons: Slow reads, many DB queries

**Hybrid Approach (Instagram's solution):**

```python
async def post_photo(user_id, photo):
    photo_id = save_photo(photo)

    follower_count = get_follower_count(user_id)

    if follower_count < THRESHOLD:  # e.g., 10,000
        # Regular user: fan-out on write
        fanout_to_followers(user_id, photo_id)
    else:
        # Celebrity: mark for fan-out on read
        cache.set(f"celebrity_recent:{user_id}", photo_id)

async def get_feed(user_id):
    # Get pre-computed feed
    feed = cassandra.query("SELECT * FROM user_feed WHERE user_id = ?", user_id)

    # Check if following any celebrities
    celebrity_following = get_celebrity_following(user_id)

    # Merge celebrity posts on the fly
    for celebrity_id in celebrity_following:
        recent_posts = cache.get(f"celebrity_recent:{celebrity_id}")
        feed.extend(recent_posts)

    # Sort and return
    return sorted(feed, key=lambda x: x.created_at, reverse=True)[:50]
```

**3. Like/Comment Flow:**

```python
# Like photo
async def like_photo(user_id, photo_id):
    # 1. Write to Cassandra (fast write)
    cassandra.insert("photo_likes", {
        "photo_id": photo_id,
        "user_id": user_id,
        "created_at": now()
    })

    # 2. Increment like counter in Redis (real-time)
    redis.incr(f"likes:{photo_id}")

    # 3. Notify photo owner (async)
    queue.publish("notification", {
        "type": "like",
        "photo_id": photo_id,
        "liker_id": user_id
    })

    return {"success": True}

# Get photo with like count
async def get_photo(photo_id):
    # Check cache
    cached = redis.get(f"photo:{photo_id}")
    if cached:
        return cached

    # Get from DB
    photo = db.get_photo(photo_id)

    # Get like count from Redis (if available)
    like_count = redis.get(f"likes:{photo_id}")
    if not like_count:
        # Fallback: count from Cassandra
        like_count = cassandra.count("SELECT * FROM photo_likes WHERE photo_id = ?")
        redis.set(f"likes:{photo_id}", like_count)

    photo['like_count'] = like_count

    # Cache
    redis.set(f"photo:{photo_id}", photo, ttl=3600)

    return photo
```

**Scaling Strategies:**

1. **Database Sharding**
```
User data (PostgreSQL):
- Shard by user_id
- User's photos on same shard

Feed data (Cassandra):
- Already partitioned by user_id
- Linear scalability
```

2. **CDN for Images**
```
Image delivery:
1. Client requests /photos/123/large.jpg
2. CDN checks cache
3. If miss, fetch from S3
4. Cache at edge locations
5. Serve to client

Result: Low latency globally
```

3. **Caching Strategy**
```
Multi-level cache:
1. Browser cache (images, JS, CSS)
2. CDN cache (images)
3. Redis cache (metadata, feeds, counters)
4. Database query cache
```

4. **Message Queues for Async**
```
Async operations:
- Photo processing (thumbnails)
- Feed fan-out
- Notifications
- Analytics

Use: Kafka or RabbitMQ
Benefit: Decouple, scale independently
```

**Capacity Estimation:**

```
Assumptions:
- 1B users, 100M DAU
- 50M photos uploaded/day
- Each photo: 2MB average
- 10-year retention

Storage:
- Photos: 50M × 365 × 10 × 2MB = 365 PB
- Metadata: 50M × 365 × 10 × 1KB = 183 TB

Bandwidth:
- Upload: 50M × 2MB / 86400 = 1.16 TB/s
- Download (10:1 ratio): 11.6 TB/s

Use CDN and S3 to handle this scale
```

**Trade-offs:**

1. **Eventual Consistency**
   - Like counts may lag slightly
   - Acceptable for user experience
   - Gain: High performance

2. **Fan-out on Write vs Read**
   - Write: Fast reads, slow writes, storage overhead
   - Read: Fast writes, slow reads
   - Hybrid: Best of both

3. **Microservices Complexity**
   - Pros: Scale independently, fault isolation
   - Cons: More complex, network latency
   - Worth it at Instagram's scale

---

#### 7.3 Design WhatsApp / Chat System

**Q: Design a real-time chat application like WhatsApp**

**Clarifying Questions:**

1. Scale: Users? → 2 billion users, 100M concurrent
2. Features: 1-on-1? Groups? → Both, plus multimedia
3. Delivery guarantees? → At-least-once delivery
4. History: Message history? → Yes, persist all messages
5. Read receipts: Needed? → Yes

**Requirements:**

Functional:
- 1-on-1 messaging
- Group chat (up to 256 members)
- Media sharing (images, videos, files)
- Online/offline status
- Delivery and read receipts
- Message history

Non-functional:
- Real-time (< 100ms delivery)
- High availability (99.99%)
- Scalable (billions of messages/day)
- End-to-end encryption
- Low latency globally

**High-Level Architecture:**

```
┌──────────────────────────────────────────┐
│         Clients (Mobile/Web)             │
└──────────────────────────────────────────┘
                   ↓ WebSocket
┌──────────────────────────────────────────┐
│        Load Balancer (Layer 4)           │
└──────────────────────────────────────────┘
                   ↓
┌──────────────────────────────────────────┐
│    Chat Servers (WebSocket/Long Poll)    │
│  - Maintain connections                  │
│  - Route messages                        │
└──────────────────────────────────────────┘
                   ↓
        ┌──────────┴──────────┐
        ↓                     ↓
┌──────────────┐      ┌──────────────┐
│   Redis      │      │  Cassandra   │
│  (Presence,  │      │  (Messages,  │
│   Session)   │      │   History)   │
└──────────────┘      └──────────────┘
        ↓
┌──────────────┐
│   Kafka      │
│ (Message Bus)│
└──────────────┘
        ↓
┌──────────────────────────────┐
│    Background Services       │
│ - Notifications              │
│ - Media processing           │
│ - Analytics                  │
└──────────────────────────────┘
```

**Database Schema:**

```sql
-- Cassandra (Message storage)
CREATE TABLE messages (
  message_id UUID,
  conversation_id UUID,
  sender_id BIGINT,
  content TEXT,
  message_type VARCHAR(20), -- text, image, video, etc.
  media_url TEXT,
  created_at TIMESTAMP,
  PRIMARY KEY (conversation_id, created_at, message_id)
) WITH CLUSTERING ORDER BY (created_at DESC);

CREATE TABLE conversations (
  conversation_id UUID PRIMARY KEY,
  type VARCHAR(20), -- direct, group
  participant_ids SET<BIGINT>,
  created_at TIMESTAMP,
  last_message_at TIMESTAMP
);

CREATE TABLE user_conversations (
  user_id BIGINT,
  conversation_id UUID,
  last_read_message_id UUID,
  last_read_at TIMESTAMP,
  PRIMARY KEY (user_id, conversation_id)
);

-- PostgreSQL (User metadata)
CREATE TABLE users (
  user_id BIGSERIAL PRIMARY KEY,
  phone_number VARCHAR(20) UNIQUE NOT NULL,
  username VARCHAR(50),
  profile_pic_url TEXT,
  public_key TEXT, -- For E2E encryption
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE contacts (
  user_id BIGINT REFERENCES users(user_id),
  contact_user_id BIGINT REFERENCES users(user_id),
  display_name VARCHAR(100),
  added_at TIMESTAMP DEFAULT NOW(),
  PRIMARY KEY (user_id, contact_user_id)
);
```

**Key Flows:**

**1. Sending a Message (1-on-1):**

```python
# Client side
async def send_message(sender_id, recipient_id, content):
    # 1. Generate message ID (client-side for idempotency)
    message_id = uuid.uuid4()

    # 2. Encrypt message (E2E encryption)
    recipient_public_key = get_recipient_public_key(recipient_id)
    encrypted_content = encrypt(content, recipient_public_key)

    # 3. Send via WebSocket
    websocket.send({
        "type": "send_message",
        "message_id": message_id,
        "recipient_id": recipient_id,
        "content": encrypted_content,
        "timestamp": now()
    })

    # 4. Show as "sent" in UI (optimistic update)
    ui.display_message(message_id, content, status="sent")

# Server side
async def handle_send_message(sender_id, message_data):
    message_id = message_data['message_id']
    recipient_id = message_data['recipient_id']

    # 1. Store message in DB (Cassandra)
    conversation_id = get_conversation_id(sender_id, recipient_id)
    cassandra.insert("messages", {
        "message_id": message_id,
        "conversation_id": conversation_id,
        "sender_id": sender_id,
        "content": message_data['content'],
        "created_at": now()
    })

    # 2. Check if recipient is online
    recipient_connection = redis.get(f"user_connection:{recipient_id}")

    if recipient_connection:
        # Recipient online: deliver immediately
        chat_server = recipient_connection['server']
        send_to_server(chat_server, {
            "type": "new_message",
            "message": message_data
        })

        # Send delivery receipt to sender
        send_to_user(sender_id, {
            "type": "delivered",
            "message_id": message_id
        })
    else:
        # Recipient offline: queue for push notification
        queue.publish("push_notifications", {
            "user_id": recipient_id,
            "message": "New message from User"
        })

    # 3. Publish to Kafka (for async processing)
    kafka.publish("messages", message_data)

    return {"status": "sent", "message_id": message_id}
```

**2. Receiving a Message:**

```python
# When user comes online
async def on_user_connected(user_id, websocket):
    # 1. Store connection info in Redis
    redis.set(f"user_connection:{user_id}", {
        "server": CURRENT_SERVER_ID,
        "websocket_id": websocket.id,
        "connected_at": now()
    }, ttl=3600)

    # 2. Fetch undelivered messages
    conversations = get_user_conversations(user_id)

    for conversation_id in conversations:
        # Get messages since last read
        last_read_at = get_last_read_timestamp(user_id, conversation_id)

        unread_messages = cassandra.query(
            """SELECT * FROM messages
               WHERE conversation_id = ? AND created_at > ?
               ORDER BY created_at ASC""",
            conversation_id, last_read_at
        )

        # Send to client
        for msg in unread_messages:
            websocket.send({
                "type": "new_message",
                "message": msg
            })

    # 3. Update presence
    redis.set(f"user_presence:{user_id}", "online", ttl=60)

    # 4. Notify contacts
    contacts = get_user_contacts(user_id)
    for contact_id in contacts:
        send_to_user(contact_id, {
            "type": "presence_update",
            "user_id": user_id,
            "status": "online"
        })
```

**3. Group Chat:**

```python
async def send_group_message(sender_id, group_id, content):
    # 1. Store message
    message_id = uuid.uuid4()
    cassandra.insert("messages", {
        "message_id": message_id,
        "conversation_id": group_id,
        "sender_id": sender_id,
        "content": content,
        "created_at": now()
    })

    # 2. Get group members
    members = cassandra.query(
        "SELECT participant_ids FROM conversations WHERE conversation_id = ?",
        group_id
    )['participant_ids']

    # 3. Fan-out to online members
    online_members = []
    offline_members = []

    for member_id in members:
        if member_id == sender_id:
            continue  # Don't send to self

        connection = redis.get(f"user_connection:{member_id}")
        if connection:
            online_members.append(member_id)
            send_to_user(member_id, {
                "type": "new_message",
                "message": {
                    "message_id": message_id,
                    "group_id": group_id,
                    "sender_id": sender_id,
                    "content": content
                }
            })
        else:
            offline_members.append(member_id)

    # 4. Queue push notifications for offline members
    if offline_members:
        queue.publish("push_notifications", {
            "user_ids": offline_members,
            "message": f"New message in group chat"
        })

    return {"status": "sent", "delivered_to": len(online_members)}

# For large groups (e.g., 256 members), use optimizations:
# - Message queue for fan-out (don't block on all sends)
# - Batch notifications
# - Lazy loading of old messages
```

**4. Read Receipts:**

```python
async def mark_as_read(user_id, conversation_id, message_id):
    # 1. Update last read timestamp
    cassandra.update("user_conversations", {
        "user_id": user_id,
        "conversation_id": conversation_id,
        "last_read_message_id": message_id,
        "last_read_at": now()
    })

    # 2. Notify sender(s) about read receipt
    message = cassandra.query(
        "SELECT sender_id FROM messages WHERE message_id = ?",
        message_id
    )

    send_to_user(message.sender_id, {
        "type": "read_receipt",
        "message_id": message_id,
        "read_by": user_id,
        "read_at": now()
    })
```

**WebSocket Connection Management:**

```python
class ChatServer:
    def __init__(self):
        self.connections = {}  # user_id -> websocket

    async def on_connect(self, user_id, websocket):
        # Authenticate user
        if not authenticate(user_id, websocket.auth_token):
            websocket.close()
            return

        # Store connection
        self.connections[user_id] = websocket

        # Register in Redis
        redis.set(f"user_connection:{user_id}", {
            "server_id": SERVER_ID,
            "connected_at": now()
        })

        # Send pending messages
        await self.deliver_pending_messages(user_id, websocket)

        # Heartbeat (keep connection alive)
        asyncio.create_task(self.heartbeat(user_id, websocket))

    async def on_disconnect(self, user_id):
        # Remove connection
        del self.connections[user_id]

        # Update Redis
        redis.delete(f"user_connection:{user_id}")

        # Update presence (after grace period)
        await asyncio.sleep(30)
        if user_id not in self.connections:
            redis.set(f"user_presence:{user_id}", "offline")
            notify_contacts(user_id, "offline")

    async def heartbeat(self, user_id, websocket):
        while True:
            try:
                await websocket.ping()
                await asyncio.sleep(30)
            except:
                await self.on_disconnect(user_id)
                break
```

**Media Sharing:**

```python
async def send_media(sender_id, recipient_id, media_file):
    # 1. Upload to S3
    media_id = uuid.uuid4()
    s3_key = f"media/{sender_id}/{media_id}"
    s3.upload(media_file, s3_key)

    # Generate thumbnail (if image/video)
    if media_file.type in ['image', 'video']:
        thumbnail = generate_thumbnail(media_file)
        s3.upload(thumbnail, f"{s3_key}_thumb")

    # 2. Send message with media URL
    await send_message(sender_id, recipient_id, {
        "type": "media",
        "media_url": f"https://cdn.whatsapp.com/{s3_key}",
        "thumbnail_url": f"https://cdn.whatsapp.com/{s3_key}_thumb",
        "media_type": media_file.type,
        "size": media_file.size
    })

    # Client downloads media when needed (lazy loading)
```

**Scaling Considerations:**

1. **WebSocket Servers**
```
Challenge: Keep millions of connections
Solution:
- Multiple chat servers (horizontal scaling)
- Load balancer with sticky sessions
- Redis for connection registry
- Server discovery (which server has user X?)
```

2. **Message Delivery Guarantees**
```
At-least-once delivery:
1. Client sends message with unique ID
2. Server stores in DB
3. Server sends ACK to client
4. If no ACK, client retries (idempotent)

Handle duplicates:
- Message ID ensures idempotency
- Server deduplicates based on message ID
```

3. **Offline Message Handling**
```
User offline for days:
- Don't load all messages at once
- Pagination: Load last 50 messages
- Lazy load older messages on scroll
- Push notifications for new messages
```

4. **Global Distribution**
```
Users in different continents:
- Multiple data centers (US, EU, Asia)
- Route users to nearest DC
- Sync data across DCs (eventual consistency)
- Use Cassandra for multi-DC replication
```

**End-to-End Encryption:**

```python
# Client-side
async def send_encrypted_message(sender_id, recipient_id, content):
    # 1. Get recipient's public key
    recipient_public_key = get_public_key(recipient_id)

    # 2. Generate session key
    session_key = generate_aes_key()

    # 3. Encrypt message with session key
    encrypted_content = aes_encrypt(content, session_key)

    # 4. Encrypt session key with recipient's public key
    encrypted_session_key = rsa_encrypt(session_key, recipient_public_key)

    # 5. Send both
    send_message(sender_id, recipient_id, {
        "encrypted_content": encrypted_content,
        "encrypted_session_key": encrypted_session_key
    })

# Server never sees plaintext!
# Server only routes encrypted messages
```

**Capacity Estimation:**

```
Assumptions:
- 2B users, 100M concurrent
- 50 messages/user/day average
- Each message: 100 bytes average

Messages per day: 2B × 50 = 100B messages/day

Storage:
- 100B × 365 × 100 bytes = 3.65 PB/year

Bandwidth:
- 100B messages / 86400 sec = 1.16M messages/sec
- 1.16M × 100 bytes = 116 MB/s

Connections:
- 100M concurrent WebSocket connections
- 10k connections per server = 10,000 servers
```

**Trade-offs:**

1. **WebSocket vs Long Polling**
   - WebSocket: Real-time, persistent connection
   - Long Polling: Works with older clients, more overhead
   - Use WebSocket, fallback to Long Polling

2. **Pull vs Push**
   - Push: Server pushes messages (real-time)
   - Pull: Client polls for messages (simpler)
   - WhatsApp uses push via WebSocket

3. **Read Receipts Privacy**
   - Show read receipts: Better UX, privacy concern
   - Hide read receipts: More private, less info
   - Allow user to disable (WhatsApp does this)

---

*This is Part 1 of the System Design guide. We've covered:*
- *Fundamentals*
- *HLD Core Concepts*
- *System Components*
- *Real-world HLD problems (URL Shortener, Instagram, WhatsApp)*

**Would you like to continue with:**
- **More HLD problems (Design Uber, Design Twitter, Design Netflix)?**
- **Move to Part 3: LLD (Low-Level Design)?**
- **Deep dive into specific topics (Caching, Databases, Microservices)?**

---

## Part 3: LLD (Low-Level Design)

<a name="lld-oop"></a>
### 8. OOP Fundamentals for LLD

#### 8.1 Object-Oriented Programming Basics

**Q: What are the four pillars of OOP and why do they matter in LLD?**

**A:** The four pillars are Encapsulation, Abstraction, Inheritance, and Polymorphism. They enable building modular, maintainable, and extensible systems.

**1. Encapsulation**

**Definition:** Bundling data and methods that operate on that data within a single unit (class), hiding internal details.

```python
# Bad: Direct access to fields
class BankAccount:
    def __init__(self):
        self.balance = 1000  # Public field

account = BankAccount()
account.balance = -500  # Direct modification, no validation!

# Good: Encapsulation with private fields and methods
class BankAccount:
    def __init__(self, initial_balance):
        self.__balance = initial_balance  # Private field

    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount
            return True
        return False

    def withdraw(self, amount):
        if 0 < amount <= self.__balance:
            self.__balance -= amount
            return True
        return False

    def get_balance(self):
        return self.__balance

account = BankAccount(1000)
account.deposit(500)  # Controlled access
# account.__balance = -500  # Won't work! Private field
```

**Benefits:**
- Data validation
- Controlled access
- Easier to change implementation
- Hide complexity

---

**2. Abstraction**

**Definition:** Hiding implementation details and showing only essential features.

```python
# Abstraction using interfaces/abstract classes
from abc import ABC, abstractmethod

class PaymentProcessor(ABC):
    @abstractmethod
    def process_payment(self, amount):
        pass

    @abstractmethod
    def refund(self, transaction_id):
        pass

class StripePaymentProcessor(PaymentProcessor):
    def process_payment(self, amount):
        # Stripe-specific implementation
        stripe_api.charge(amount)
        return "stripe_transaction_123"

    def refund(self, transaction_id):
        stripe_api.refund(transaction_id)

class PayPalPaymentProcessor(PaymentProcessor):
    def process_payment(self, amount):
        # PayPal-specific implementation
        paypal_api.charge(amount)
        return "paypal_transaction_456"

    def refund(self, transaction_id):
        paypal_api.refund(transaction_id)

# Client code doesn't need to know implementation details
def checkout(processor: PaymentProcessor, amount):
    transaction_id = processor.process_payment(amount)
    return transaction_id
```

**Benefits:**
- Simplifies complex systems
- Reduces coupling
- Allows multiple implementations
- Easier testing (mock implementations)

---

**3. Inheritance**

**Definition:** Creating new classes based on existing classes, inheriting properties and methods.

```python
# Base class
class Vehicle:
    def __init__(self, brand, model, year):
        self.brand = brand
        self.model = model
        self.year = year

    def start_engine(self):
        return "Engine started"

    def stop_engine(self):
        return "Engine stopped"

# Derived classes
class Car(Vehicle):
    def __init__(self, brand, model, year, num_doors):
        super().__init__(brand, model, year)
        self.num_doors = num_doors

    def open_trunk(self):
        return "Trunk opened"

class Motorcycle(Vehicle):
    def __init__(self, brand, model, year, has_sidecar):
        super().__init__(brand, model, year)
        self.has_sidecar = has_sidecar

    def wheelie(self):
        return "Performing wheelie!"

# Usage
car = Car("Toyota", "Camry", 2024, 4)
print(car.start_engine())  # Inherited method
print(car.open_trunk())    # Car-specific method

bike = Motorcycle("Harley", "Sportster", 2024, False)
print(bike.start_engine())  # Inherited method
print(bike.wheelie())       # Motorcycle-specific method
```

**When to Use Inheritance:**
✓ "Is-a" relationship (Car IS-A Vehicle)
✗ Don't use for code reuse alone (use composition instead)

---

**4. Polymorphism**

**Definition:** Ability to treat objects of different classes through the same interface.

```python
# Polymorphism in action
class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

    @abstractmethod
    def perimeter(self):
        pass

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius

    def area(self):
        return 3.14159 * self.radius ** 2

    def perimeter(self):
        return 2 * 3.14159 * self.radius

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height

    def perimeter(self):
        return 2 * (self.width + self.height)

# Polymorphic behavior
def print_shape_info(shape: Shape):
    print(f"Area: {shape.area()}")
    print(f"Perimeter: {shape.perimeter()}")

# Different objects, same interface
shapes = [
    Circle(5),
    Rectangle(4, 6),
    Circle(10)
]

for shape in shapes:
    print_shape_info(shape)  # Polymorphism!
```

**Types of Polymorphism:**
1. **Compile-time (Method Overloading)**: Same method name, different parameters
2. **Runtime (Method Overriding)**: Subclass provides specific implementation

---

#### 8.2 Relationships Between Classes

**Q: What are the different types of relationships between classes?**

**A:** Association, Aggregation, Composition, Inheritance, and Dependency.

**1. Association (Has-a relationship)**

```python
# Weak relationship: objects can exist independently
class Teacher:
    def __init__(self, name):
        self.name = name

class Student:
    def __init__(self, name):
        self.name = name
        self.teachers = []  # Association

    def enroll_with_teacher(self, teacher):
        self.teachers.append(teacher)

# Both can exist independently
teacher = Teacher("Mr. Smith")
student = Student("John")
student.enroll_with_teacher(teacher)
```

**UML:**
```
Teacher <---- Student
  (Teacher exists independently of Student)
```

---

**2. Aggregation (Weak ownership)**

```python
# Objects can exist independently, but one "owns" the other
class Department:
    def __init__(self, name):
        self.name = name
        self.employees = []

    def add_employee(self, employee):
        self.employees.append(employee)

class Employee:
    def __init__(self, name):
        self.name = name

# Employees can exist without Department
emp1 = Employee("Alice")
emp2 = Employee("Bob")

dept = Department("Engineering")
dept.add_employee(emp1)
dept.add_employee(emp2)

# If department is deleted, employees still exist
```

**UML:**
```
Department ◇---- Employee
  (Hollow diamond: aggregation)
```

---

**3. Composition (Strong ownership)**

```python
# One object cannot exist without the other
class Engine:
    def __init__(self, horsepower):
        self.horsepower = horsepower

class Car:
    def __init__(self, brand):
        self.brand = brand
        self.engine = Engine(200)  # Composition: Car creates Engine

    def start(self):
        return f"Car with {self.engine.horsepower}hp engine started"

# Engine cannot exist without Car
car = Car("Toyota")
# If car is deleted, engine is deleted too
```

**UML:**
```
Car ♦---- Engine
  (Filled diamond: composition)
```

**Aggregation vs Composition:**
- **Aggregation**: Department and Employees (employees can exist independently)
- **Composition**: Car and Engine (engine cannot exist without car)

---

**4. Dependency**

```python
# One class uses another temporarily
class EmailService:
    def send_email(self, to, subject, body):
        print(f"Sending email to {to}")

class UserRegistration:
    def register_user(self, user):
        # Save user to database
        db.save(user)

        # Dependency: temporarily uses EmailService
        email_service = EmailService()
        email_service.send_email(user.email, "Welcome!", "Thanks for registering")

# UserRegistration depends on EmailService, but doesn't own it
```

**UML:**
```
UserRegistration <.... EmailService
  (Dashed arrow: dependency)
```

---

#### 8.3 Class Diagram Basics

**Q: How do you read and draw UML class diagrams?**

**A:** Class diagrams show classes, their attributes, methods, and relationships.

**Basic Class Diagram:**

```
┌─────────────────────────┐
│      ClassName          │  ← Class name
├─────────────────────────┤
│ - privateField: type    │  ← Attributes
│ + publicField: type     │
│ # protectedField: type  │
├─────────────────────────┤
│ + publicMethod(): type  │  ← Methods
│ - privateMethod(): type │
└─────────────────────────┘

Visibility:
+ public
- private
# protected
~ package/internal
```

**Example: E-commerce System**

```
┌───────────────────┐           ┌───────────────────┐
│      Customer     │           │       Order       │
├───────────────────┤           ├───────────────────┤
│ - customerId: int │           │ - orderId: int    │
│ - name: string    │ 1    0..* │ - orderDate: Date │
│ - email: string   │◇──────────┤ - status: string  │
├───────────────────┤           ├───────────────────┤
│ + placeOrder()    │           │ + calculateTotal()│
│ + cancelOrder()   │           │ + ship()          │
└───────────────────┘           └───────────────────┘
                                        △
                                        │
                                        │ Composition
                                        ♦
                                ┌───────────────────┐
                                │    OrderItem      │
                                ├───────────────────┤
                                │ - productId: int  │
                                │ - quantity: int   │
                                │ - price: decimal  │
                                ├───────────────────┤
                                │ + getSubtotal()   │
                                └───────────────────┘
```

**Cardinality:**
- `1` : Exactly one
- `0..1` : Zero or one
- `0..*` or `*` : Zero or more
- `1..*` : One or more
- `n..m` : Between n and m

---

<a name="design-patterns"></a>
### 9. Design Patterns

**Q: What are design patterns and why are they important?**

**A:** Design patterns are reusable solutions to common problems in software design. They provide tested, proven development paradigms.

**Categories:**
1. **Creational**: Object creation mechanisms
2. **Structural**: Object composition and relationships
3. **Behavioral**: Object collaboration and responsibility

---

#### 9.1 Creational Patterns

**1. Singleton Pattern**

**Problem:** Ensure a class has only one instance and provide global access to it.

**Use Cases:**
- Database connections
- Logger
- Configuration manager
- Cache

```python
class DatabaseConnection:
    _instance = None
    _lock = threading.Lock()

    def __new__(cls):
        if cls._instance is None:
            with cls._lock:
                if cls._instance is None:
                    cls._instance = super().__new__(cls)
                    cls._instance._initialize()
        return cls._instance

    def _initialize(self):
        self.connection = self._create_connection()

    def _create_connection(self):
        # Create database connection
        return {"host": "localhost", "port": 5432}

    def query(self, sql):
        return f"Executing: {sql}"

# Usage
db1 = DatabaseConnection()
db2 = DatabaseConnection()
print(db1 is db2)  # True - same instance!
```

**Thread-safe Singleton (Python-specific):**

```python
class Singleton:
    _instances = {}
    _lock = threading.Lock()

    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            with cls._lock:
                if cls not in cls._instances:
                    cls._instances[cls] = super().__call__(*args, **kwargs)
        return cls._instances[cls]

class Logger(metaclass=Singleton):
    def log(self, message):
        print(f"[LOG] {message}")
```

**Pros:**
- Controlled access to single instance
- Reduced memory footprint
- Global access point

**Cons:**
- Hidden dependencies
- Difficult to test
- Can create bottlenecks

**Interview Insight:** "When would you NOT use Singleton?"
- When you need multiple instances (duh!)
- When it makes testing difficult
- When it creates global state (bad for functional programming)

---

**2. Factory Pattern**

**Problem:** Create objects without specifying the exact class.

**Use Cases:**
- Creating different types of notifications (email, SMS, push)
- Database drivers (MySQL, PostgreSQL, MongoDB)
- UI components (Button, TextField, Checkbox)

```python
from abc import ABC, abstractmethod

# Product interface
class Notification(ABC):
    @abstractmethod
    def send(self, message, recipient):
        pass

# Concrete products
class EmailNotification(Notification):
    def send(self, message, recipient):
        print(f"Sending email to {recipient}: {message}")

class SMSNotification(Notification):
    def send(self, message, recipient):
        print(f"Sending SMS to {recipient}: {message}")

class PushNotification(Notification):
    def send(self, message, recipient):
        print(f"Sending push notification to {recipient}: {message}")

# Factory
class NotificationFactory:
    @staticmethod
    def create_notification(notification_type):
        if notification_type == "email":
            return EmailNotification()
        elif notification_type == "sms":
            return SMSNotification()
        elif notification_type == "push":
            return PushNotification()
        else:
            raise ValueError(f"Unknown notification type: {notification_type}")

# Usage
factory = NotificationFactory()

email = factory.create_notification("email")
email.send("Hello!", "user@example.com")

sms = factory.create_notification("sms")
sms.send("Hello!", "+1234567890")
```

**Advanced: Factory Method Pattern**

```python
class NotificationService(ABC):
    @abstractmethod
    def create_notification(self):
        pass

    def notify(self, message, recipient):
        notification = self.create_notification()
        notification.send(message, recipient)

class EmailService(NotificationService):
    def create_notification(self):
        return EmailNotification()

class SMSService(NotificationService):
    def create_notification(self):
        return SMSNotification()

# Usage
service = EmailService()
service.notify("Hello!", "user@example.com")
```

**Pros:**
- Loose coupling
- Easy to extend (add new notification types)
- Single Responsibility Principle

**Cons:**
- Can become complex with many products
- Extra classes

---

**3. Builder Pattern**

**Problem:** Construct complex objects step by step.

**Use Cases:**
- Building complex objects (e.g., HTTP request, SQL query)
- Objects with many optional parameters
- Immutable objects

```python
class Pizza:
    def __init__(self):
        self.size = None
        self.cheese = False
        self.pepperoni = False
        self.mushrooms = False
        self.olives = False

    def __str__(self):
        toppings = []
        if self.cheese: toppings.append("cheese")
        if self.pepperoni: toppings.append("pepperoni")
        if self.mushrooms: toppings.append("mushrooms")
        if self.olives: toppings.append("olives")

        return f"{self.size} pizza with {', '.join(toppings)}"

class PizzaBuilder:
    def __init__(self):
        self.pizza = Pizza()

    def set_size(self, size):
        self.pizza.size = size
        return self  # Return self for method chaining

    def add_cheese(self):
        self.pizza.cheese = True
        return self

    def add_pepperoni(self):
        self.pizza.pepperoni = True
        return self

    def add_mushrooms(self):
        self.pizza.mushrooms = True
        return self

    def add_olives(self):
        self.pizza.olives = True
        return self

    def build(self):
        return self.pizza

# Usage (fluent interface)
pizza = (PizzaBuilder()
    .set_size("large")
    .add_cheese()
    .add_pepperoni()
    .add_mushrooms()
    .build())

print(pizza)  # large pizza with cheese, pepperoni, mushrooms

# Compare with constructor hell:
# pizza = Pizza(size="large", cheese=True, pepperoni=True, mushrooms=True, olives=False)
```

**Real-World Example: HTTP Request Builder**

```python
class HTTPRequest:
    def __init__(self):
        self.method = "GET"
        self.url = None
        self.headers = {}
        self.body = None
        self.timeout = 30

class HTTPRequestBuilder:
    def __init__(self, url):
        self.request = HTTPRequest()
        self.request.url = url

    def method(self, method):
        self.request.method = method
        return self

    def header(self, key, value):
        self.request.headers[key] = value
        return self

    def body(self, body):
        self.request.body = body
        return self

    def timeout(self, timeout):
        self.request.timeout = timeout
        return self

    def build(self):
        return self.request

# Usage
request = (HTTPRequestBuilder("https://api.example.com/users")
    .method("POST")
    .header("Content-Type", "application/json")
    .header("Authorization", "Bearer token123")
    .body({"name": "John", "email": "john@example.com"})
    .timeout(60)
    .build())
```

**Pros:**
- Readable code
- Handles complex construction logic
- Immutability (if build() returns immutable object)

**Cons:**
- More verbose
- More classes

---

#### 9.2 Structural Patterns

**1. Adapter Pattern**

**Problem:** Make incompatible interfaces work together.

**Use Cases:**
- Integrating third-party libraries
- Legacy code integration
- API versioning

```python
# Existing interface (what we have)
class OldPaymentSystem:
    def make_payment(self, account_number, amount):
        print(f"Old system: Paying ${amount} from account {account_number}")

# New interface (what we want)
class PaymentProcessor(ABC):
    @abstractmethod
    def process_payment(self, user_id, amount):
        pass

# Adapter
class OldPaymentAdapter(PaymentProcessor):
    def __init__(self, old_system):
        self.old_system = old_system

    def process_payment(self, user_id, amount):
        # Convert new interface to old interface
        account_number = self._get_account_number(user_id)
        self.old_system.make_payment(account_number, amount)

    def _get_account_number(self, user_id):
        # Look up account number from user ID
        return f"ACC-{user_id}"

# Usage
old_system = OldPaymentSystem()
adapter = OldPaymentAdapter(old_system)

# Client code uses new interface
adapter.process_payment("user123", 100)
# Output: Old system: Paying $100 from account ACC-user123
```

**Real-World Example: Payment Gateway Integration**

```python
# Stripe's interface
class StripeAPI:
    def create_charge(self, token, amount_cents):
        print(f"Stripe: Charging {amount_cents} cents")

# PayPal's interface
class PayPalAPI:
    def make_payment(self, email, amount_dollars):
        print(f"PayPal: Charging ${amount_dollars} to {email}")

# Our unified interface
class PaymentGateway(ABC):
    @abstractmethod
    def charge(self, user, amount_dollars):
        pass

# Adapters
class StripeAdapter(PaymentGateway):
    def __init__(self, stripe_api):
        self.stripe_api = stripe_api

    def charge(self, user, amount_dollars):
        token = user.stripe_token
        amount_cents = int(amount_dollars * 100)
        self.stripe_api.create_charge(token, amount_cents)

class PayPalAdapter(PaymentGateway):
    def __init__(self, paypal_api):
        self.paypal_api = paypal_api

    def charge(self, user, amount_dollars):
        self.paypal_api.make_payment(user.email, amount_dollars)

# Usage
def checkout(gateway: PaymentGateway, user, amount):
    gateway.charge(user, amount)

# Client code doesn't care about underlying implementation
stripe_gateway = StripeAdapter(StripeAPI())
paypal_gateway = PayPalAdapter(PayPalAPI())

checkout(stripe_gateway, user, 50)  # Uses Stripe
checkout(paypal_gateway, user, 50)  # Uses PayPal
```

---

**2. Decorator Pattern**

**Problem:** Add behavior to objects dynamically without modifying their code.

**Use Cases:**
- Adding logging, caching, authentication to methods
- UI components (adding borders, scrollbars)
- Stream wrappers (compression, encryption)

```python
# Component interface
class Coffee(ABC):
    @abstractmethod
    def cost(self):
        pass

    @abstractmethod
    def description(self):
        pass

# Concrete component
class SimpleCoffee(Coffee):
    def cost(self):
        return 5

    def description(self):
        return "Simple coffee"

# Base decorator
class CoffeeDecorator(Coffee):
    def __init__(self, coffee):
        self._coffee = coffee

    def cost(self):
        return self._coffee.cost()

    def description(self):
        return self._coffee.description()

# Concrete decorators
class MilkDecorator(CoffeeDecorator):
    def cost(self):
        return self._coffee.cost() + 2

    def description(self):
        return self._coffee.description() + ", milk"

class SugarDecorator(CoffeeDecorator):
    def cost(self):
        return self._coffee.cost() + 1

    def description(self):
        return self._coffee.description() + ", sugar"

class WhippedCreamDecorator(CoffeeDecorator):
    def cost(self):
        return self._coffee.cost() + 3

    def description(self):
        return self._coffee.description() + ", whipped cream"

# Usage
coffee = SimpleCoffee()
print(f"{coffee.description()}: ${coffee.cost()}")
# Simple coffee: $5

coffee = MilkDecorator(coffee)
print(f"{coffee.description()}: ${coffee.cost()}")
# Simple coffee, milk: $7

coffee = SugarDecorator(coffee)
print(f"{coffee.description()}: ${coffee.cost()}")
# Simple coffee, milk, sugar: $8

coffee = WhippedCreamDecorator(coffee)
print(f"{coffee.description()}: ${coffee.cost()}")
# Simple coffee, milk, sugar, whipped cream: $11
```

**Real-World Example: Request/Response Middleware**

```python
# Base handler
class RequestHandler(ABC):
    @abstractmethod
    def handle(self, request):
        pass

class APIHandler(RequestHandler):
    def handle(self, request):
        return {"data": "API response"}

# Decorators (Middleware)
class LoggingDecorator(RequestHandler):
    def __init__(self, handler):
        self.handler = handler

    def handle(self, request):
        print(f"[LOG] Request: {request}")
        response = self.handler.handle(request)
        print(f"[LOG] Response: {response}")
        return response

class AuthenticationDecorator(RequestHandler):
    def __init__(self, handler):
        self.handler = handler

    def handle(self, request):
        if not request.get("auth_token"):
            return {"error": "Unauthorized"}
        return self.handler.handle(request)

class CachingDecorator(RequestHandler):
    def __init__(self, handler):
        self.handler = handler
        self.cache = {}

    def handle(self, request):
        cache_key = str(request)
        if cache_key in self.cache:
            print("[CACHE] Cache hit!")
            return self.cache[cache_key]

        response = self.handler.handle(request)
        self.cache[cache_key] = response
        return response

# Usage: Stack decorators
handler = APIHandler()
handler = LoggingDecorator(handler)
handler = AuthenticationDecorator(handler)
handler = CachingDecorator(handler)

request = {"path": "/api/users", "auth_token": "abc123"}
response = handler.handle(request)
```

**Python-specific: Function Decorators**

```python
def log_execution(func):
    def wrapper(*args, **kwargs):
        print(f"Executing {func.__name__}")
        result = func(*args, **kwargs)
        print(f"Finished {func.__name__}")
        return result
    return wrapper

def require_auth(func):
    def wrapper(*args, **kwargs):
        if not kwargs.get('auth_token'):
            raise Exception("Unauthorized")
        return func(*args, **kwargs)
    return wrapper

@log_execution
@require_auth
def api_endpoint(data, auth_token=None):
    return {"result": "success"}

# Usage
api_endpoint({"user": "John"}, auth_token="abc123")
```

---

**3. Facade Pattern**

**Problem:** Provide a simplified interface to a complex subsystem.

**Use Cases:**
- Simplifying complex libraries
- Hiding implementation details
- Providing a unified interface

```python
# Complex subsystems
class VideoFile:
    def __init__(self, filename):
        self.filename = filename

class AudioMixer:
    def fix_audio(self, audio):
        print("Fixing audio...")
        return audio

class VideoConverter:
    def convert(self, video, format):
        print(f"Converting video to {format}...")
        return video

class CompressionCodec:
    def compress(self, video):
        print("Compressing video...")
        return video

# Facade
class VideoConversionFacade:
    def __init__(self):
        self.audio_mixer = AudioMixer()
        self.video_converter = VideoConverter()
        self.codec = CompressionCodec()

    def convert_video(self, filename, format):
        print(f"Starting conversion of {filename} to {format}")

        video = VideoFile(filename)

        # Complex series of operations simplified
        audio = self.audio_mixer.fix_audio(video)
        converted = self.video_converter.convert(video, format)
        compressed = self.codec.compress(converted)

        print("Conversion complete!")
        return compressed

# Usage
facade = VideoConversionFacade()
facade.convert_video("funny_cats.mp4", "avi")

# Without facade, client would need to:
# - Create VideoFile
# - Create AudioMixer, fix audio
# - Create VideoConverter, convert
# - Create CompressionCodec, compress
# Much more complex!
```

---

*End of system-design.md Part 1*

**This document will continue with:**
- Behavioral Patterns (Observer, Strategy, Command, etc.)
- SOLID Principles in depth
- Real-world LLD problems (Parking Lot, Library Management, etc.)
- Interview strategy and tricky questions

**Total length so far: ~15,000 lines. This is a comprehensive reference document!**
