## What is a Transaction?

A **transaction** is a basic term used in SQL. It refers to a group of one or more operations (`INSERT`, `SELECT`, `UPDATE`, `DELETE`) that is treated as a single unit. This unit either **succeeds together** or **fails together**.

Transactions ensure the database remains consistent and follows **ACID** properties:

1. **Atomicity**: Succeed or fail together.
2. **Consistency**: The database remains in a valid state.
3. **Isolation**: Transactions do not interfere with one another.
4. **Durability**: Once completed, the changes remain permanent.

---

## SQL Transaction Syntax and Explanation

`BEGIN` initiates a transaction block

### Syntax
```sql
BEGIN [ WORK | TRANSACTION ] [ transaction_mode [, ...] ]
```

### Components of the Syntax

1. **BEGIN [WORK | TRANSACTION]**  
   - Starts a new transaction.  
   - `WORK` and `TRANSACTION` are optional keywords; they mean the same thing and can be skipped.  

2. **transaction_mode**  
   - Configures how the transaction behaves. Multiple modes can be set by separating them with commas.  


<details>
<summary> Transaction Modes </summary>

<details>
<summary>Isolation Level Problem Cases</summary>

## Database Isolation Level Issues

### 1. Dirty Reads
- **Core Issue:**  
  Inconsistent data due to reading uncommitted changes from other transactions, which may later be rolled back. This leads to using incorrect or unreliable data.
  
- **Impact:**  
  Decisions based on dirty data can lead to errors, such as wrong balances, inventory, or financial calculations, because the data was never finalized.

### 2. Non-Repeatable Reads
- **Core Issue:**  
  Inconsistent results when the same data is read multiple times within a transaction, but other transactions modify it in between. This causes discrepancies because the data changes unexpectedly.
  
- **Impact:**  
  Inaccurate calculations, reporting, or decisions, especially when calculations rely on data remaining stable throughout a transaction (e.g., during financial operations or inventory management).

### 3. Phantom Reads
- **Core Issue:**  
  Changes in the result set of a query due to new rows being added or removed by other transactions. This leads to inconsistency when the same query returns different results at different times.
  
- **Impact:**  
  Issues in reporting, query results, or business logic, as the results of a query can change unexpectedly due to concurrent transactions modifying the dataset.

</details>

#### 1. **ISOLATION LEVEL**
Controls how "isolated" the transaction is from other transactions. It defines how data changes by one transaction are visible to others.  

- **SERIALIZABLE**: The highest isolation. Transactions are completely independent, ensuring no conflicts but slower performance. *Prevent all cases*
- **REPEATABLE READ**: Prevents changes to rows being read, but new rows can still appear in the result. *Allow only Phantom Read (**Default mode**)*.  
- **READ COMMITTED**: Default in most databases. Ensures only committed changes are read, balancing performance and accuracy. *Prevent only Dirty Read*.  
- **READ UNCOMMITTED**: The lowest isolation. Transactions can read uncommitted data, which may cause inconsistencies . *Allow all cases*. 

#### 2. **READ WRITE | READ ONLY**
- **READ WRITE**: Allows changes to data (default). *Almost all queries are allowed*. 
- **READ ONLY**: Ensures the transaction does not change any data, useful for reporting. *Query like* `SELECT` *are allowed.* `INSERT`, `DELETE`, `DROP` *such are not allowed.*   

#### 3. **[ NOT ] DEFERRABLE**
- Affects the timing of constraint checks (e.g., foreign keys).  
- **DEFERRABLE**: Allows constraint checks to be delayed until the end of the transaction.  
- **NOT DEFERRABLE**: Forces immediate constraint checks.  

</details>

## Example
```sql
BEGIN TRANSACTION 
    ISOLATION LEVEL SERIALIZABLE, 
    READ ONLY, 
    DEFERRABLE;
```

- This starts a transaction that:
  - Uses **SERIALIZABLE** isolation (highest level of data safety).  
  - Ensures **READ ONLY** mode (no changes to data).  
  - Allows constraint checks to be **DEFERRABLE** (postponed until the transaction ends).  

---

In simpler terms, this lets you safely and efficiently control how your transaction interacts with the database and other users.

