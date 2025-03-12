
# Software Engineer Notes 📘

A collection of useful software engineering concepts, database insights, and system design notes.

## Table of Contents
- [Software Engineering Fundamentals](#software-engineering-fundamentals)
  - [What is Bit Shifting?](#what-is-bit-shifting)
  - [Where Is Memory Usually Stored?](#where-is-memory-usually-stored)
  - [Stack in Terms of Memory vs. Data Structure](#stack-in-terms-of-memory-vs-data-structure)
  - [Value Type vs. Reference Type](#value-type-vs-reference-type)
- [Databases](#databases)
  - [What Happens When You Run a SQL Query?](#what-happens-when-you-run-a-sql-query)
  - [What Is an LSM Tree?](#what-is-an-lsm-tree)
  - [What Is a B-Tree?](#what-is-a-b-tree)
  - [ACID Database Transaction Properties](#acid-database-transaction-properties)
- [System Design](#system-design)
  - [Design an Efficient System to Track Likes](#design-an-efficient-system-to-track-likes)

---

## Software Engineering Fundamentals

### What is Bit Shifting? 🧮
```c
x << y
```
`x << y` is a bitwise left shift operation that shifts integer `x` to the left by `y` positions, equivalent to multiplying `x` by `2^y`.

#### Example:
```c
int x = 3;
int y = 5;
int res = x << y; // 3 * (2^5) = 96
```
#### Applications:
- Converting IP addresses to integers for efficient storage and comparison.

---

### Where Is Memory Usually Stored? 💾
Memory is typically allocated in:
- **Stack**: Stores local function variables, follows LIFO (Last In, First Out).
- **Heap**: Stores dynamically allocated memory, persists beyond function execution.

---

### Stack in Terms of Memory vs. Data Structure 🏗️
A **stack (data structure)** follows FILO (First In, Last Out). In memory management:
- Functions are pushed onto the **call stack**.
- Each function gets its own **stack frame** for local variables.
- When a function completes, its stack frame is **popped** off.

#### Example:
```python
def add(a, b):
    return a + b

def main():
    result = add(5, 3)
    print(result)
```
Call stack:
1. `main()` is called → pushed onto stack.
2. `add(5,3)` is called → pushed onto stack.
3. `add()` finishes → popped.
4. `main()` resumes and prints the result.

---

### Value Type vs. Reference Type 🔗
- **Value Type**: Directly holds data (e.g., `int`, `float`).
- **Reference Type**: Stores a pointer to the actual data in heap memory (e.g., objects, slices in Go).

---

## Databases

### What Happens When You Run a SQL Query? 🧐
#### SQL Query Execution Steps:
1. **Parsing**: Converts query into an Abstract Syntax Tree (AST).
2. **Binding**: Validates table and column names.
3. **Optimization**: Creates the most efficient execution plan.
4. **Execution**: Database engine processes the query.

---

### What Is an LSM Tree? 🌲
A **Log-Structured Merge Tree (LSM Tree)** is a key-value store used in databases like **Cassandra** and **RocksDB**.

#### Features:
- **Append-only writes** for high write throughput.
- **Memtable + SSTables**: Data is first written to an in-memory table (`memtable`), then periodically flushed to disk (`SSTable`).
- **Compaction**: Old values are merged and redundant data is removed.

✅ *Pros*: Fast writes, low latency.  
❌ *Cons*: Slower reads, requires bloom filters for optimization.

---

### ACID Database Transaction Properties 🔥
| Property    | Description |
|------------|------------|
| **Atomicity**  | All operations succeed or fail as a whole. |
| **Consistency** | Database remains in a valid state before/after a transaction. |
| **Isolation**   | Transactions are executed independently. |
| **Durability**  | Once committed, data is permanently stored. |

**Isolation Levels:**
- **Read Uncommitted**: Can read uncommitted data.
- **Read Committed**: Only reads committed data.
- **Repeatable Read**: Prevents non-repeatable reads.
- **Serializable**: Strongest isolation, forces sequential execution.

---

## System Design

### Design an Efficient System to Track Likes ❤️
#### Storage Options:
- **MySQL**: Simple but can cause high read/write contention.
- **Redis**: Fast lookups but data persistence is limited.
- **Hybrid Approach**: Use Redis for real-time tracking, batch updates to MySQL.

#### Example Schema:
```sql
CREATE TABLE likes (
    user_id INT,
    post_id INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (user_id, post_id)
);
```

#### Optimization Techniques:
- **Sharding**: Distribute data across multiple servers.
- **Batch Writes**: Reduce database load.
- **Bloom Filters**: Avoid unnecessary DB queries.

---
