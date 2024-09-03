# software-engineer-notes

- [Software Engineering Nutrition](#software-engineering-nutrition)
  - [Q: What is Bit Shifting?](#q-what-is-bit-shifting)
  - [Q: Where Is Memory Usually Stored?](#q-what-is-bit-shifting)
  - [Q: So What Is a Stack in Terms of Memory? Isn't It a Data Structure?](#q-so-what-is-a-stack-in-terms-of-memory-isnt-it-a-data-structure
)
- [Database](#database)
  - [Q: What Happens When You Run a SQL Query?](#q-what-happens-when-you-run-a-sql-query)
  - [Q: What Is a LSMTree](#q-what-is-a-lsmtree)
  - [Q: What Is a B-Tree](#q-what-is-a-b-tree)
  - [Q: What Does ACID Database Transaction Properties Mean?](#q-what-does-acid-database-transaction-properties-mean)

## Software Engineering Nutrition
### Q: What is Bit Shifting?
```mjs
x << y
```
`x << y` represents a bitwise left shifting operation, integer `x` to be shift to the left by `y` positons where `x` is a constant.

For example, 1 is represented as `0001` in binary, by applying bitwise left shifting operation by 2 positions to the binary `0001`, imagine the binary representation is zero-padded, it will become `0100`. By converting `0100` to hexadecimal, you will get an integer `4`.  We can conclude that the equation will be `2 ^ y` where `y = 2` in this case, `2 ^ 2 = 4`.

The full equation of `x << y` simply equals to `x * (2 ^ y)` where `x` is a constant.

```mjs
int x = 3;
int y = 5;
int res = x << y; // x * (2 ^ y) = 3 * (2 ^ 5) = 3 * 32 = 96
```

An example of bit shifting application is converting IP address into integer.

Merging IP octets into a single integer consumes less memory, more efficient when it comes to comparison operations. Integer representations of IP can be found internally in many network protocols for packet routing.

### Q: Where Is Memory Usually Stored?
Memory usually stores in heap and stack.

### Q: So what is a stack in terms of memory? Isn't it a data structure?
Yes, stack is a data structure. In order to understand stack in term of memory, we need to understand the stack data structure.

Stack data structure follows First-In, Last Out (FILO) structure, which means if you put something into the stack, you can only insert / put things onto the top so it stacks up - push. You can't take anything out from the bottom, you can't take anything out from the middle, the only thing you can do is to take the item on the top out 1 by 1, which we call the action - pop.

In applications, we have what we call - a call stack.

```python
def doAddition(self, a, b):
    total = a + b
    return total

def main():
    x = 5
    y = 3
    result = doAddition(x, y)
    print("Sum of the integers : ", result)
```
When the main() is called, the main() method will be added into an empty call stack. The doAddition(x, y) method will be added on top on the main() in call stack. A call stack keeps track of the methods and tell the application where should it continue after a method finished its execution. For example, after doAddition(x, y) finished its summation, it will be popped out from the call stack and the application will go back to the main() method to print out the sum of both integers.

In every method in call stack will have its own local variable for example variables x and y in main(), variable total in doAddition()

### Q: So What Is a Stack in Terms of Memory? Isn't It a Data Structure?
Variables in stack will disappeared after the specific method finished its execution, and here is where heap memory comes in if you need a variable to outlive stack memory. Heap memory is still accessible after all methods finished running.

Since heap is so powerful, why not we just use heap for every variables? Because its EXPENSIVE (causes overhead)

To go deeper into stack and heap memory, we need to first understand value type and reference type, how data is stored and manipulated in computer memory.

### Q: What is value type and reference type?

## Database

### Q: What Happens When You Run a SQL Query?
#### 1) Parsing
- Database server parses the SQL query to understand structure.
- Creates an AST (Abstract Syntax Tree), which captures the hierarchical structure of the SQL query
#### 2) Binding
- Algebrizer (a process in query execution after parsing) will take the parsed AST as an input for binding. To validate if the query is correct / semantically meaningful.
#### 3) Optimize 
- Receives input from Algebrizer to optimize.
- Query optimizer will try different execution methods (eg. reading rows, using different indexes) to come out with the most efficient execution plan.
- Cache the plan in memory for future use for similar query.
#### 4) Execute
- SQL Server Storage Engine will execute the plan.

### Q: What Is a LSMTree?
LSMTree (Log-Structured Merge Tree) is a data structure that stores data in key-value pairs that used in several database systems.

In LSMTree, it only appends to the end of the file no matter you insert or update a specific key. It practices an append-only approach for fast write operations.

When a write goes in, it will be constructed into tree structure, which we call a `memtable` here. When the `memtable` reaches a certain size, usually a few MB (megabytes), it will write onto disk as an SSTable file, the subsequent data will be written as the new `memtable`. Compaction will be done on old segment files to consolidate keys with latest value, by removing unused data to reduct size of the segment file.
- eg. `key1: 2, key2: 123, key2: 73, key1: 1` into `key1: 1, key1: 72`

It is a read-slow structure because when we try to read from a LSMTree, it will
1) Read from memtable, if it doesn't find the key
2) Read from segment files, file by file
3) Segment files will be merged and compacted on the fly

#### Overcome Read-Slow Performance
To overcome the read-slow performance issue, it uses bloom filter with it so it can tell if a key doesn't exist in the database

#### Overcome Database Crash While Updating
To avoid database crashing will inserting into a LSMTree, the recent writes that write in the memtable will be double written / keep in a separate log on disk. The logs will be used to restored data just in case the database crashed.

### Q: What Does ACID Database Transaction Properties Mean?
#### 1) Atomicity
- Each statement is a transaction (which consists beginTransaction, commitTransaction, rollbackTransaction)
- It's either all actions performed, or none of them are performed in the end of a transaction
- Transaction is a single unit of work
#### 2) Consistency
- Data consistent among all tables, follows data integrity constraints
- For example, during checkout in systems, check foreign key where the item ID is valid in ITEM Table
#### 3) Isolation
4 Isolation Levels
- Read Uncommitted (Weakest Isolation)
  * Allowed to read changes of records EVEN IF CHANGES NOT COMMITTED
- Read Committed
  * Allowed to read changes of records ONLY IF CHANGES COMMITTED
- Repeatable Read
  * Holds read lock on records / rows that are reading by a transaction so other transactions cannot update
  * Avoid Non-Repeatable Read (Runs same query twice and gets different results, caused by committed updates from other transactions in between two queries)
- Serializable (Strictest Isolation)
  * Makes sure all transactions RUNS SEQUENTIALLY, even if they are executed concurrently
#### 4) Durability
- Makes sure database is persistent
- Database replication in multiple different regions, able to recover, high availability


### Design an Efficient System That Keeps Track of Likes
```
// Redis? MySql? Where should I store??
```
