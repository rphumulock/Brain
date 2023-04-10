>https://www.postgresql.org/docs/15/extend.html

This part is about extending the server functionality with user-defined functions.

---
### How extensibility works
>https://www.postgresql.org/docs/15/extend-how.html

PostgreSQL is extensible because its operation is catalog-driven. If you are familiar with standard relational database systems, you know that they store information about databases, tables, columns, etc., in what are commonly known as system catalogs. (Some systems call this the data dictionary.) The catalogs appear to the user as tables like any other, but the DBMS stores its internal bookkeeping in them. One key difference between PostgreSQL and standard relational database systems is that PostgreSQL stores much more information in its catalogs: not only information about tables and columns, but also information about data types, functions, access methods, and so on. These tables can be modified by the user, and since PostgreSQL bases its operation on these tables, this means that PostgreSQL can be extended by users. By comparison, conventional database systems can only be extended by changing hardcoded procedures in the source code or by loading modules specially written by the DBMS vendor.

---
### Type System
>https://www.postgresql.org/docs/15/extend-type-system.html

PostgreSQL data types can be divided into base types, container types, domains, and pseudo-types.

---
### User Defined Functions
>https://www.postgresql.org/docs/15/xfunc.html

PostgreSQL provides four kinds of functions:
- Query language functions (functions written in SQL) ([Section 38.5](https://www.postgresql.org/docs/15/xfunc-sql.html "38.5. Query Language (SQL) Functions"))
- Procedural language functions (functions written in, for example, PL/pgSQL or PL/Tcl) ([Section 38.8](https://www.postgresql.org/docs/15/xfunc-pl.html "38.8. Procedural Language Functions"))
- Internal functions ([Section 38.9](https://www.postgresql.org/docs/15/xfunc-internal.html "38.9. Internal Functions"))
- C-language functions ([Section 38.10](https://www.postgresql.org/docs/15/xfunc-c.html "38.10. C-Language Functions"))

Every kind of function can take base types, composite types, or combinations of these as arguments (parameters). In addition, every kind of function can return a base type or a composite type. Functions can also be defined to return sets of base or composite values.

Many kinds of functions can take or return certain pseudo-types (such as polymorphic types), but the available facilities vary.

---
### User Defined Procedures
>https://www.postgresql.org/docs/current/xproc.html

A procedure is a database object similar to a function. The key differences are:

- Procedures are defined with the [`CREATE PROCEDURE`](https://www.postgresql.org/docs/current/sql-createprocedure.html "CREATE PROCEDURE") command, not `CREATE FUNCTION`.
- Procedures do not return a function value; hence `CREATE PROCEDURE` lacks a `RETURNS` clause. However, procedures can instead return data to their callers via output parameters.
- While a function is called as part of a query or DML command, a procedure is called in isolation using the [`CALL`](https://www.postgresql.org/docs/current/sql-call.html "CALL") command.
- A procedure can commit or roll back transactions during its execution (then automatically beginning a new transaction), so long as the invoking `CALL` command is not part of an explicit transaction block. A function cannot do that.
- Certain function attributes, such as strictness, don't apply to procedures. Those attributes control how the function is used in a query, which isn't relevant to procedures.
- Can return a value using `INOUT` mode.
- Procedures are compiled objects - so they tend to be faster than functions.

---
### SQL Functions
>https://www.postgresql.org/docs/15/xfunc-sql.html

SQL functions execute an arbitrary list of SQL statements, returning the result of the last query in the list.

- [38.5.1. Arguments for SQL Functions](https://www.postgresql.org/docs/15/xfunc-sql.html#XFUNC-SQL-FUNCTION-ARGUMENTS)
- [38.5.2. SQL Functions on Base Types](https://www.postgresql.org/docs/15/xfunc-sql.html#XFUNC-SQL-BASE-FUNCTIONS)
- [38.5.3. SQL Functions on Composite Types](https://www.postgresql.org/docs/15/xfunc-sql.html#XFUNC-SQL-COMPOSITE-FUNCTIONS)
- [38.5.4. SQL Functions with Output Parameters](https://www.postgresql.org/docs/15/xfunc-sql.html#XFUNC-OUTPUT-PARAMETERS)
- [38.5.5. SQL Procedures with Output Parameters](https://www.postgresql.org/docs/15/xfunc-sql.html#XFUNC-OUTPUT-PARAMETERS-PROC)
- [38.5.6. SQL Functions with Variable Numbers of Arguments](https://www.postgresql.org/docs/15/xfunc-sql.html#XFUNC-SQL-VARIADIC-FUNCTIONS)
- [38.5.7. SQL Functions with Default Values for Arguments](https://www.postgresql.org/docs/15/xfunc-sql.html#XFUNC-SQL-PARAMETER-DEFAULTS)
- [38.5.8. SQL Functions as Table Sources](https://www.postgresql.org/docs/15/xfunc-sql.html#XFUNC-SQL-TABLE-FUNCTIONS)
- [38.5.9. SQL Functions Returning Sets](https://www.postgresql.org/docs/15/xfunc-sql.html#XFUNC-SQL-FUNCTIONS-RETURNING-SET)
- [38.5.10. SQL Functions Returning `TABLE`](https://www.postgresql.org/docs/15/xfunc-sql.html#XFUNC-SQL-FUNCTIONS-RETURNING-TABLE)
- [38.5.11. Polymorphic SQL Functions](https://www.postgresql.org/docs/15/xfunc-sql.html#XFUNC-SQL-POLYMORPHIC-FUNCTIONS)
- [38.5.12. SQL Functions with Collations](https://www.postgresql.org/docs/15/xfunc-sql.html#id-1.8.3.8.21)

---
### User Defined Indexes
>[PostgreSQL: Documentation: 15: 38.16. Interfacing Extensions to Indexes](https://www.postgresql.org/docs/15/xindex.html)

The procedures described thus far let you define new types, new functions, and new operators. However, we cannot yet define an index on a column of a new data type. To do this, we must define an _operator class_ for the new data type.

Example:
![[extendedindex2.png]]
![[extendedindex3.png]]
![[extendedindex4.png]]
![[extendedindex5.png]]
![[extendedindex6.png]]
![[extendedindex7.png]]
![[extendedindex8.png]]

---
### User Defined Aggregate Functions
>[PostgreSQL: Documentation: 15: 38.12. User-Defined Aggregates](https://www.postgresql.org/docs/15/xaggr.html)

Aggregate functions in PostgreSQL are defined in terms of _state values_ and _state transition functions_. That is, an aggregate operates using a state value that is updated as each successive input row is processed. To define a new aggregate function, one selects a data type for the state value, an initial value for the state, and a state transition function.

![[extendedagg1.png]]
![[extendedagg2.png]]
![[extendedagg3.png]]
![[extendedagg4.png]]
![[extendedagg5.png]]
![[extendedagg6.png]]