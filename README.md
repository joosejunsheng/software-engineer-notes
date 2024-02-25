# software-engineer-notes

## Bit Shifting
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

### Database

## What happened when you run a SQL Query?
### 1) Parsing
- Database server parses the SQL query to understand structure.
- Creates an AST (Abstract Syntax Tree), which captures the hierarchical structure of the SQL query
### 2) Binding
- Algebrizer (a process in query execution after parsing) will take the parsed AST as an input for binding. To validate if the query is correct / semantically meaningful.
### 3) Optimize 
- Receives input from Algebrizer to optimize.
- Query optimizer will try different execution methods (eg. reading rows, using different indexes) to come out with the most efficient execution plan.
- Cache the plan in memory for future use for similar query.
### 4) Execute
- SQL Server Storage Engine will execute the plan.


## ACID - Database Transaction Properties

### 1) Atomicity
- Each statement is a transaction (which consists beginTransaction, commitTransaction, rollbackTransaction)
- It's either all actions performed, or none of them are performed in the end of a transaction
- Transaction is a single unit of work
### 2) Consistency
- Data consistent among all tables, follows data integrity constraints
- For example, during checkout in systems, check foreign key where the item ID is valid in ITEM Table
### 3) Isolation
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
### 4) Durability
- Makes sure database is persistent
- Database replication in multiple different regions, able to recover, high availability
