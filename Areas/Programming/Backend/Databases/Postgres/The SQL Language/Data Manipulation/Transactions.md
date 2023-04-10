>https://www.postgresql.org/docs/15/tutorial-transactions.html

ACID Database through transactions

---
### ACID

What are the ACID properties of a transaction? ACID is an acronym that stands for atomicity, consistency, isolation, and durability. Together, these ACID properties ensure that a set of database operations (grouped together in a transaction) leave the database in a valid state even in the event of unexpected errors.

### Atomicity
Atomicity guarantees that all of the commands that make up a transaction are treated as a single unit and either succeed or fail together. This is important as in the case of an unwanted event, like a crash or power outage, we can be sure of the state of the database. The transaction would have either completed successfully or been rollbacked if any part of the transaction failed.

### Consistency
Consistency guarantees that changes made within a transaction are consistent with database constraints. This includes all rules, constraints, and triggers. If the data gets into an illegal state, the whole transaction fails.

### Isolation
Isolation ensures that all transactions run in an isolated environment. That enables running transactions concurrently because transactions don’t interfere with each other.

### Durability
Durability guarantees that once the transaction completes and changes are written to the database, they are persisted. This ensures that data within the system will persist even in the case of system failures like crashes or power outages.

## What is an ACID transaction example?
- **Atomicity**: Money needs to both be removed from one account and added to the other, or the transaction will be aborted. Removing money from one account without adding it to another would leave the data in an inconsistent state.
- **Consistency**: Consider a database constraint that an account balance cannot drop below zero dollars. All updates to an account balance inside of a transaction must leave the account with a valid, non-negative balance, or the transaction should be aborted.
- **Isolation**: Consider two concurrent requests to transfer money from the same bank account. The final result of running the transfer requests concurrently should be the same as running the transfer requests sequentially.
- **Durability**: Consider a power failure immediately after a database has confirmed that money has been transferred from one bank account to another. The database should still hold the updated information even though there was an unexpected failure.


---

EDIT

## Overview

> https://www.postgresql.org/docs/current/tutorial-transactions.html

  

---

### Example

  

![](assets/images/postgres/transactions/Screen%20Shot%202023-01-17%20at%203.32.02%20PM.png)

  

How do we do this in isolation? We use a Transaction.

  

We use "BEGIN" to start a transaction. This will occur on it's own isolated connection - not effecting the main database.

  

![](assets/images/postgres/transactions/Screen%20Shot%202023-01-17%20at%204.54.01%20PM.png)

  

![](assets/images/postgres/transactions/Screen%20Shot%202023-01-17%20at%203.32.35%20PM.png)

  

- If we don't like the changes - we **can** rollback.

- If transaction has an error - we **must** rollback.

- If connection is lost - postgres will **automatically** rollback.

- If we like it - we can **COMMIT** it to the main database.

  

---

### Transaction Locks

  

Once BEGIN is called - the transaction will lock on the data that it's processing.