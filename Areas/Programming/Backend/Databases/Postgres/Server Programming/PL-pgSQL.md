>https://www.postgresql.org/docs/15/plpgsql.html

---
### Overview
>https://www.postgresql.org/docs/15/plpgsql-overview.html

SQL is the language PostgreSQL and most other relational databases use as query language. It's portable and easy to learn. But every SQL statement must be executed individually by the database server.

That means that your client application must send each query to the database server, wait for it to be processed, receive and process the results, do some computation, then send further queries to the server. All this incurs interprocess communication and will also incur network overhead if your client is on a different machine than the database server.

With PL/pgSQL you can group a block of computation and a series of queries _inside_ the database server, thus having the power of a procedural language and the ease of use of SQL, but with considerable savings of client/server communication overhead.

- Extra round trips between client and server are eliminated
- Intermediate results that the client does not need do not have to be marshaled or transferred between server and client
- Multiple rounds of query parsing can be avoided

This can result in a considerable performance increase as compared to an application that does not use stored functions.

Also, with PL/pgSQL you can use all the data types, operators and functions of SQL.

---
###  Structure
>https://www.postgresql.org/docs/15/plpgsql-structure.html

How to write a basic funtion.

---
### Variable Declarations
>https://www.postgresql.org/docs/15/plpgsql-declarations.html

- [43.3.1. Declaring Function Parameters](https://www.postgresql.org/docs/15/plpgsql-declarations.html#PLPGSQL-DECLARATION-PARAMETERS)
- [43.3.2. `ALIAS`](https://www.postgresql.org/docs/15/plpgsql-declarations.html#PLPGSQL-DECLARATION-ALIAS)
- [43.3.3. Copying Types](https://www.postgresql.org/docs/15/plpgsql-declarations.html#PLPGSQL-DECLARATION-TYPE)
- [43.3.4. Row Types](https://www.postgresql.org/docs/15/plpgsql-declarations.html#PLPGSQL-DECLARATION-ROWTYPES)
- [43.3.5. Record Types](https://www.postgresql.org/docs/15/plpgsql-declarations.html#PLPGSQL-DECLARATION-RECORDS)
- [43.3.6. Collation of PL/pgSQL Variables](https://www.postgresql.org/docs/15/plpgsql-declarations.html#PLPGSQL-DECLARATION-COLLATION)

---
### Basic Statements
>https://www.postgresql.org/docs/15/plpgsql-statements.html

- [43.5.1. Assignment](https://www.postgresql.org/docs/15/plpgsql-statements.html#PLPGSQL-STATEMENTS-ASSIGNMENT)
- [43.5.2. Executing SQL Commands](https://www.postgresql.org/docs/15/plpgsql-statements.html#PLPGSQL-STATEMENTS-GENERAL-SQL)
- [43.5.3. Executing a Command with a Single-Row Result](https://www.postgresql.org/docs/15/plpgsql-statements.html#PLPGSQL-STATEMENTS-SQL-ONEROW)
- [43.5.4. Executing Dynamic Commands](https://www.postgresql.org/docs/15/plpgsql-statements.html#PLPGSQL-STATEMENTS-EXECUTING-DYN)
- [43.5.5. Obtaining the Result Status](https://www.postgresql.org/docs/15/plpgsql-statements.html#PLPGSQL-STATEMENTS-DIAGNOSTICS)
- [43.5.6. Doing Nothing At All](https://www.postgresql.org/docs/15/plpgsql-statements.html#PLPGSQL-STATEMENTS-NULL)

---
### Expressions
>https://www.postgresql.org/docs/15/plpgsql-expressions.html

---
### Control Structures
>https://www.postgresql.org/docs/15/plpgsql-control-structures.html

- [43.6.1. Returning from a Function](https://www.postgresql.org/docs/15/plpgsql-control-structures.html#PLPGSQL-STATEMENTS-RETURNING)
- [43.6.2. Returning from a Procedure](https://www.postgresql.org/docs/15/plpgsql-control-structures.html#PLPGSQL-STATEMENTS-RETURNING-PROCEDURE)
- [43.6.3. Calling a Procedure](https://www.postgresql.org/docs/15/plpgsql-control-structures.html#PLPGSQL-STATEMENTS-CALLING-PROCEDURE)
- [43.6.4. Conditionals](https://www.postgresql.org/docs/15/plpgsql-control-structures.html#PLPGSQL-CONDITIONALS)
- [43.6.5. Simple Loops](https://www.postgresql.org/docs/15/plpgsql-control-structures.html#PLPGSQL-CONTROL-STRUCTURES-LOOPS)
- [43.6.6. Looping through Query Results](https://www.postgresql.org/docs/15/plpgsql-control-structures.html#PLPGSQL-RECORDS-ITERATING)
- [43.6.7. Looping through Arrays](https://www.postgresql.org/docs/15/plpgsql-control-structures.html#PLPGSQL-FOREACH-ARRAY)
- [43.6.8. Trapping Errors](https://www.postgresql.org/docs/15/plpgsql-control-structures.html#PLPGSQL-ERROR-TRAPPING)
- [43.6.9. Obtaining Execution Location Information](https://www.postgresql.org/docs/15/plpgsql-control-structures.html#PLPGSQL-CALL-STACK)

---
### Transaction Management
>https://www.postgresql.org/docs/15/plpgsql-transactions.html

---
### Error Messages
>https://www.postgresql.org/docs/15/plpgsql-errors-and-messages.html