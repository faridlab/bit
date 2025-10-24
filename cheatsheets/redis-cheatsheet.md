# Redis Cheatsheet

## Table of Contents
1. [Redis Fundamentals](#redis-fundamentals)
2. [Data Structures](#data-structures)
3. [Basic Commands](#basic-commands)
4. [Advanced Features](#advanced-features)
5. [Common Use Cases](#common-use-cases)
6. [Uncommon/Advanced Use Cases](#uncommonadvanced-use-cases)
7. [Performance & Optimization](#performance--optimization)
8. [Monitoring & Debugging](#monitoring--debugging)
9. [Best Practices](#best-practices)

---

## Redis Fundamentals

### What is Redis?
Redis (Remote Dictionary Server) is an open-source, in-memory data structure store that can be used as a database, cache, message broker, and streaming engine. It supports various data structures such as strings, hashes, lists, sets, sorted sets, bitmaps, hyperloglogs, geospatial indexes, and streams.

### Key Characteristics
- **In-Memory**: Data stored in RAM for ultra-fast access
- **Persistence**: Optional disk-based persistence (RDB snapshots, AOF logs)
- **Single-threaded**: Commands executed atomically
- **Network Protocol**: TCP-based with RESP (Redis Serialization Protocol)
- **Client-Server Architecture**: Multiple clients can connect to one server

### Installation & Setup
```bash
# Ubuntu/Debian
sudo apt update
sudo apt install redis-server

# macOS
brew install redis

# Docker
docker run -d -p 6379:6379 --name redis redis:latest

# Start Redis server
redis-server

# Connect to Redis CLI
redis-cli
```

---

## Data Structures

### 1. Strings
Most basic Redis data type. Can store strings, integers, or floating point numbers.

```redis
# Basic string operations
SET key value
GET key
DEL key
EXISTS key
EXPIRE key seconds
TTL key

# Numeric operations
INCR key          # Increment by 1
INCRBY key 5      # Increment by 5
DECR key          # Decrement by 1
DECRBY key 3      # Decrement by 3

# String operations
APPEND key "suffix"
STRLEN key
GETRANGE key 0 4  # Get substring
SETRANGE key 0 "new"  # Replace substring
```

### 2. Hashes
Hash maps with string fields and values. Perfect for objects.

```redis
# Hash operations
HSET user:1 name "John" age 30 email "john@example.com"
HGET user:1 name
HGETALL user:1
HDEL user:1 age
HEXISTS user:1 name
HKEYS user:1
HVALS user:1
HLEN user:1

# Numeric operations on hash fields
HINCRBY user:1 age 1
HINCRBYFLOAT user:1 score 0.5
```

### 3. Lists
Ordered collections of strings. Implemented as linked lists.

```redis
# List operations
LPUSH mylist "item1" "item2"  # Push to left
RPUSH mylist "item3"         # Push to right
LPOP mylist                  # Pop from left
RPOP mylist                  # Pop from right
LLEN mylist                  # Get length
LRANGE mylist 0 -1          # Get all elements
LRANGE mylist 0 2           # Get first 3 elements
LINDEX mylist 1             # Get element at index
LSET mylist 1 "newvalue"    # Set element at index
LTRIM mylist 0 2            # Keep only first 3 elements
```

### 4. Sets
Unordered collections of unique strings.

```redis
# Set operations
SADD myset "member1" "member2" "member3"
SMEMBERS myset
SISMEMBER myset "member1"
SCARD myset                 # Get cardinality
SREM myset "member1"        # Remove member

# Set operations between multiple sets
SUNION set1 set2            # Union
SINTER set1 set2            # Intersection
SDIFF set1 set2             # Difference
SUNIONSTORE result set1 set2  # Store union in result
```

### 5. Sorted Sets (ZSets)
Sets with scores for ordering. Perfect for leaderboards and rankings.

```redis
# Sorted set operations
ZADD leaderboard 100 "player1" 200 "player2" 150 "player3"
ZRANGE leaderboard 0 -1 WITHSCORES
ZRANK leaderboard "player1"     # Get rank (0-based)
ZREVRANK leaderboard "player1"  # Get reverse rank
ZSCORE leaderboard "player1"   # Get score
ZCARD leaderboard              # Get cardinality

# Range operations
ZRANGEBYSCORE leaderboard 100 200
ZREVRANGE leaderboard 0 2      # Top 3 players
ZCOUNT leaderboard 100 200     # Count elements in score range

# Increment score
ZINCRBY leaderboard 50 "player1"
```

### 6. Bitmaps
String values treated as sequences of bits.

```redis
# Bitmap operations
SETBIT mykey 7 1              # Set bit at position 7
GETBIT mykey 7                # Get bit at position 7
BITCOUNT mykey                # Count set bits
BITOP AND result key1 key2    # Bitwise AND
BITOP OR result key1 key2     # Bitwise OR
BITOP XOR result key1 key2    # Bitwise XOR
```

### 7. HyperLogLog
Probabilistic data structure for counting unique elements.

```redis
# HyperLogLog operations
PFADD hll "element1" "element2" "element3"
PFCOUNT hll                   # Get approximate count
PFMERGE result hll1 hll2     # Merge HyperLogLogs
```

### 8. Geospatial
Store and query geographical coordinates.

```redis
# Geospatial operations
GEOADD cities 13.361389 38.115556 "Palermo" 15.087269 37.502669 "Catania"
GEODIST cities "Palermo" "Catania" km
GEORADIUS cities 15 37 100 km WITHDIST
GEORADIUSBYMEMBER cities "Palermo" 200 km
```

### 9. Streams
Append-only log data structure for event sourcing.

```redis
# Stream operations
XADD mystream * field1 value1 field2 value2
XREAD STREAMS mystream 0
XRANGE mystream - +
XREVRANGE mystream + -
XGROUP CREATE mygroup mystream 0
XREADGROUP GROUP mygroup consumer1 STREAMS mystream >
```

---

## Basic Commands

### Connection & Server
```redis
# Connection
AUTH password
PING
ECHO "Hello"
QUIT

# Server info
INFO
INFO memory
INFO stats
CONFIG GET "*"
CONFIG SET parameter value

# Database operations
SELECT 0        # Switch to database 0
FLUSHDB         # Clear current database
FLUSHALL        # Clear all databases
```

### Key Management
```redis
# Key operations
KEYS pattern           # Find keys matching pattern
SCAN cursor [MATCH pattern] [COUNT count]  # Iterate keys safely
TYPE key              # Get data type
RENAME oldkey newkey  # Rename key
RENAMENX oldkey newkey # Rename only if newkey doesn't exist
MOVE key db           # Move key to another database
```

### Expiration
```redis
# Expiration commands
EXPIRE key seconds
EXPIREAT key timestamp
PEXPIRE key milliseconds
PEXPIREAT key millisecond-timestamp
PERSIST key           # Remove expiration
```

---

## Advanced Features

### 1. Pub/Sub (Publish/Subscribe)
```redis
# Publishing
PUBLISH channel "message"

# Subscribing
SUBSCRIBE channel1 channel2
PSUBSCRIBE pattern*    # Pattern subscription
UNSUBSCRIBE channel1
PUNSUBSCRIBE pattern*

# Example
# Terminal 1:
SUBSCRIBE news

# Terminal 2:
PUBLISH news "Breaking news!"
```

### 2. Transactions
```redis
# Multi/Exec transaction
MULTI
SET key1 "value1"
SET key2 "value2"
EXEC

# Watch for optimistic locking
WATCH key1
MULTI
SET key1 "newvalue"
EXEC  # Fails if key1 was modified

# Discard transaction
MULTI
SET key1 "value1"
DISCARD
```

### 3. Lua Scripting
```redis
# Execute Lua script
EVAL "return redis.call('get', KEYS[1])" 1 mykey

# Store and execute script
SCRIPT LOAD "return redis.call('get', KEYS[1])"
EVALSHA sha1_hash 1 mykey

# Example: Atomic counter
EVAL "
local current = redis.call('GET', KEYS[1])
if current == false then
    current = 0
end
local new = current + ARGV[1]
redis.call('SET', KEYS[1], new)
return new
" 1 counter 1
```

### 4. Pipelining
```redis
# Batch multiple commands
# Client-side example (pseudo-code):
pipeline = redis.pipeline()
pipeline.set("key1", "value1")
pipeline.set("key2", "value2")
pipeline.get("key1")
results = pipeline.execute()
```

### 5. Clustering
```redis
# Cluster commands
CLUSTER NODES
CLUSTER INFO
CLUSTER MEET ip port
CLUSTER ADDSLOTS slot1 slot2
CLUSTER REPLICATE node-id
```

---

## Common Use Cases

### 1. Caching
```redis
# Simple cache with expiration
SET cache:user:123 '{"name":"John","age":30}' EX 3600
GET cache:user:123

# Cache-aside pattern
# 1. Check cache
GET cache:user:123
# 2. If miss, get from database
# 3. Store in cache
SET cache:user:123 user_data EX 3600

# Cache invalidation
DEL cache:user:123
```

### 2. Session Storage
```redis
# Store session data
HSET session:abc123 user_id 123 last_activity 1640995200
EXPIRE session:abc123 1800  # 30 minutes

# Update session
HSET session:abc123 last_activity 1640995800
EXPIRE session:abc123 1800

# Get session
HGETALL session:abc123
```

### 3. Rate Limiting
```redis
# Sliding window rate limiting
# Lua script for atomic rate limiting
EVAL "
local key = KEYS[1]
local window = tonumber(ARGV[1])
local limit = tonumber(ARGV[2])
local now = tonumber(ARGV[3])

redis.call('ZREMRANGEBYSCORE', key, 0, now - window)
local current = redis.call('ZCARD', key)

if current < limit then
    redis.call('ZADD', key, now, now)
    redis.call('EXPIRE', key, window)
    return 1
else
    return 0
end
" 1 rate_limit:user:123 60 10 1640995200
```

### 4. Real-time Analytics
```redis
# Page view counter
INCR page:views:homepage
INCR page:views:about

# Unique visitors (using HyperLogLog)
PFADD visitors:today user123 user456 user789
PFCOUNT visitors:today

# Real-time leaderboard
ZADD leaderboard:today 100 "player1" 200 "player2"
ZREVRANGE leaderboard:today 0 9 WITHSCORES
```

### 5. Message Queues
```redis
# Simple queue using lists
LPUSH queue:emails "email1@example.com"
LPUSH queue:emails "email2@example.com"
RPOP queue:emails  # Process email

# Priority queue using sorted sets
ZADD priority:queue 1 "low_priority_task"
ZADD priority:queue 10 "high_priority_task"
ZPOPMIN priority:queue  # Get highest priority task
```

### 6. Distributed Locks
```redis
# Simple distributed lock
SET lock:resource "owner" NX EX 30

# Release lock (only if you own it)
EVAL "
if redis.call('GET', KEYS[1]) == ARGV[1] then
    return redis.call('DEL', KEYS[1])
else
    return 0
end
" 1 lock:resource "owner"
```

---

## Uncommon/Advanced Use Cases

### 1. Time Series Data
```redis
# Store time series data using sorted sets
ZADD temperature:2024-01-15 1640995200 "25.5"
ZADD temperature:2024-01-15 1640995800 "26.1"
ZADD temperature:2024-01-15 1640996400 "25.8"

# Query time range
ZRANGEBYSCORE temperature:2024-01-15 1640995200 1640996400

# Calculate average temperature
EVAL "
local data = redis.call('ZRANGEBYSCORE', KEYS[1], ARGV[1], ARGV[2])
local sum = 0
for i = 1, #data do
    sum = sum + tonumber(data[i])
end
return sum / #data
" 1 temperature:2024-01-15 1640995200 1640996400
```

### 2. Geospatial Applications
```redis
# Store restaurant locations
GEOADD restaurants 13.361389 38.115556 "Restaurant A"
GEOADD restaurants 15.087269 37.502669 "Restaurant B"

# Find restaurants within 10km of user
GEORADIUS restaurants 13.3 38.1 10 km WITHDIST

# Find restaurants near another restaurant
GEORADIUSBYMEMBER restaurants "Restaurant A" 5 km
```

### 3. Machine Learning Features
```redis
# User behavior tracking
SADD user:123:viewed_products "product1" "product2" "product3"
SADD user:123:purchased_products "product1" "product4"

# Collaborative filtering
SINTER user:123:viewed_products user:456:viewed_products
SUNION user:123:viewed_products user:456:viewed_products

# Feature vectors using hashes
HSET user:123:features age 25 income 50000 location "NYC"
HSET user:123:features preferences "tech,gaming"
```

### 4. Event Sourcing with Streams
```redis
# Event store
XADD events * type "user_registered" user_id 123 email "user@example.com"
XADD events * type "user_login" user_id 123 timestamp 1640995200
XADD events * type "user_logout" user_id 123 timestamp 1640995800

# Read all events for a user
XRANGE events - + | grep user_id:123

# Consumer groups for processing
XGROUP CREATE event_processors events 0
XREADGROUP GROUP event_processors worker1 STREAMS events >
```

### 5. Probabilistic Data Structures
```redis
# Count unique visitors (HyperLogLog)
PFADD visitors:2024-01-15 "user1" "user2" "user3"
PFCOUNT visitors:2024-01-15

# Bloom filter simulation using bitmaps
SETBIT bloom:emails 1000 1  # Mark email as seen
GETBIT bloom:emails 1000    # Check if email was seen

# Count distinct elements in sliding window
EVAL "
local key = KEYS[1]
local window = tonumber(ARGV[1])
local now = tonumber(ARGV[2])
local element = ARGV[3]

redis.call('ZREMRANGEBYSCORE', key, 0, now - window)
redis.call('ZADD', key, now, element)
return redis.call('ZCARD', key)
" 1 unique_users:window 3600 1640995200 "user123"
```

### 6. Graph Database Simulation
```redis
# Store graph relationships
SADD user:123:follows "user456" "user789"
SADD user:456:followers "user123"
SADD user:789:followers "user123"

# Find mutual connections
SINTER user:123:follows user:456:follows

# Find friends of friends
SUNION user:123:follows user:456:follows
```

### 7. Financial Applications
```redis
# Real-time stock prices
HSET stock:AAPL price 150.25 volume 1000000 timestamp 1640995200
HSET stock:GOOGL price 2800.50 volume 500000 timestamp 1640995200

# Portfolio tracking
ZADD portfolio:user123 10 "AAPL" 5 "GOOGL" 3 "MSFT"

# Market depth (order book)
ZADD orderbook:AAPL:buy 150.20 1000  # price, quantity
ZADD orderbook:AAPL:buy 150.15 2000
ZADD orderbook:AAPL:sell 150.30 500
```

### 8. IoT Data Collection
```redis
# Sensor data
XADD sensors:temperature * device_id "sensor1" value 25.5 location "room1"
XADD sensors:humidity * device_id "sensor2" value 60.2 location "room1"

# Device status tracking
HSET device:sensor1 status "online" last_seen 1640995200 battery 85
HSET device:sensor2 status "offline" last_seen 1640995000 battery 20

# Alert system
ZADD alerts 1 "device:sensor2 battery_low"
ZADD alerts 2 "device:sensor1 temperature_high"
```

---

## Performance & Optimization

### Memory Optimization
```redis
# Use appropriate data types
# Instead of: SET user:123:name "John" SET user:123:age "30"
# Use: HSET user:123 name "John" age "30"

# Use compression for large values
SET large_data:123 "compressed_data" COMPRESSION

# Monitor memory usage
INFO memory
MEMORY USAGE key
MEMORY STATS
```

### Persistence Configuration
```redis
# RDB snapshots
CONFIG SET save "900 1 300 10 60 10000"

# AOF (Append Only File)
CONFIG SET appendonly yes
CONFIG SET appendfsync everysec

# Hybrid persistence
CONFIG SET save "900 1"
CONFIG SET appendonly yes
```

### Connection Pooling
```redis
# Client configuration (pseudo-code)
redis_client = redis.Redis(
    host='localhost',
    port=6379,
    db=0,
    max_connections=20,
    retry_on_timeout=True
)
```

### Clustering Best Practices
```redis
# Key distribution
# Use consistent hashing for key distribution
# Example: user:{user_id} -> slot calculation

# Cluster commands
CLUSTER KEYSLOT mykey
CLUSTER GETKEYSINSLOT 0 10
```

---

## Monitoring & Debugging

### Health Checks
```redis
# Basic health check
PING

# Server info
INFO server
INFO memory
INFO stats
INFO clients

# Monitor commands in real-time
MONITOR

# Slow log
SLOWLOG GET 10
CONFIG SET slowlog-log-slower-than 10000
```

### Performance Metrics
```redis
# Key metrics to monitor
INFO stats
INFO memory
INFO clients
INFO persistence
INFO replication

# Custom metrics
INCR metrics:requests:total
INCR metrics:requests:errors
```

### Debugging
```redis
# Key debugging
DEBUG OBJECT key
OBJECT ENCODING key
OBJECT IDLETIME key

# Memory debugging
MEMORY USAGE key
MEMORY DOCTOR

# Latency monitoring
LATENCY LATEST
LATENCY HISTORY command
```

---

## Best Practices

### 1. Key Naming Conventions
```redis
# Use descriptive, hierarchical names
user:123:profile
session:abc123:data
cache:product:456:details
queue:emails:pending
counter:page:views:homepage
```

### 2. Data Modeling
```redis
# Choose appropriate data structures
# Strings: Simple key-value, counters
# Hashes: Objects, user profiles
# Lists: Queues, timelines
# Sets: Tags, categories, unique items
# Sorted Sets: Rankings, leaderboards
# Streams: Event logs, message queues
```

### 3. Memory Management
```redis
# Set appropriate expiration
EXPIRE cache:key 3600  # 1 hour
EXPIRE session:key 1800  # 30 minutes

# Use memory-efficient data types
# Prefer HSET over multiple SET commands
# Use HyperLogLog for approximate counting
```

### 4. Security
```redis
# Authentication
AUTH password

# Network security
CONFIG SET bind 127.0.0.1
CONFIG SET protected-mode yes

# Disable dangerous commands
CONFIG SET rename-command FLUSHDB ""
CONFIG SET rename-command FLUSHALL ""
```

### 5. Backup & Recovery
```redis
# RDB backup
BGSAVE

# AOF backup
BGREWRITEAOF

# Replication
SLAVEOF master_ip master_port
```

### 6. Error Handling
```redis
# Check if key exists before operations
EXISTS key
if key exists:
    GET key

# Use transactions for atomicity
MULTI
SET key1 value1
SET key2 value2
EXEC
```

### 7. Scaling Strategies
```redis
# Read replicas
SLAVEOF master_ip master_port

# Clustering
CLUSTER MEET node_ip node_port
CLUSTER ADDSLOTS 0 1 2 3

# Sharding (application level)
# Route keys to different Redis instances based on hash
```

---

## Common Patterns

### 1. Cache-Aside Pattern
```redis
# Application logic
def get_user(user_id):
    # 1. Check cache
    cached = redis.get(f"user:{user_id}")
    if cached:
        return json.loads(cached)

    # 2. Get from database
    user = database.get_user(user_id)

    # 3. Store in cache
    redis.setex(f"user:{user_id}", 3600, json.dumps(user))
    return user
```

### 2. Write-Through Pattern
```redis
# Application logic
def update_user(user_id, data):
    # 1. Update database
    database.update_user(user_id, data)

    # 2. Update cache
    redis.setex(f"user:{user_id}", 3600, json.dumps(data))
```

### 3. Circuit Breaker Pattern
```redis
# Check circuit breaker state
def is_circuit_open(service_name):
    state = redis.get(f"circuit:{service_name}")
    return state == "open"

# Open circuit breaker
redis.setex(f"circuit:{service_name}", 60, "open")
```

### 4. Leader Election
```redis
# Try to become leader
SET leader:service "node1" NX EX 30

# Check if I'm the leader
GET leader:service

# Renew leadership
EXPIRE leader:service 30
```

---

## Redis Modules & Extensions

### RediSearch
```redis
# Full-text search
FT.CREATE idx:users ON HASH PREFIX 1 user: SCHEMA name TEXT SORTABLE age NUMERIC SORTABLE
FT.SEARCH idx:users "John" LIMIT 0 10
FT.SEARCH idx:users "@age:[20 30]" SORTBY age ASC
```

### RedisJSON
```redis
# JSON operations
JSON.SET user:123 $ '{"name":"John","age":30}'
JSON.GET user:123 $.name
JSON.SET user:123 $.age 31
JSON.ARRAPPEND user:123 $.hobbies "reading"
```

### RedisTimeSeries
```redis
# Time series data
TS.CREATE temperature:room1 LABELS room room1 sensor temperature
TS.ADD temperature:room1 1640995200000 25.5
TS.RANGE temperature:room1 1640995200000 1640995800000
TS.MRANGE 1640995200000 1640995800000 FILTER room=room1
```

---

## Troubleshooting

### Common Issues
1. **Memory Issues**: Monitor `INFO memory`, use `MEMORY USAGE`
2. **Slow Queries**: Check `SLOWLOG GET`, optimize queries
3. **Connection Issues**: Check `INFO clients`, adjust `maxclients`
4. **Persistence Issues**: Check `INFO persistence`, verify disk space

### Performance Tuning
```redis
# Optimize for memory
CONFIG SET maxmemory 2gb
CONFIG SET maxmemory-policy allkeys-lru

# Optimize for performance
CONFIG SET tcp-keepalive 60
CONFIG SET timeout 300
```

---

This comprehensive Redis cheatsheet covers everything from basic operations to advanced use cases. Redis is incredibly versatile and can be used for much more than simple caching - from real-time analytics to machine learning features, from financial applications to IoT data collection. The key is understanding the right data structure for your use case and leveraging Redis's atomic operations and data structures effectively.
