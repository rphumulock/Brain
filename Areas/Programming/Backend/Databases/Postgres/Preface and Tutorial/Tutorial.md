>[PostgreSQL: Documentation: 15: Part I. Tutorial](https://www.postgresql.org/docs/current/tutorial.html)

---
### Architectural Fundamentals
>[PostgreSQL: Documentation: 15: 1.2. Architectural Fundamentals](https://www.postgresql.org/docs/current/tutorial-arch.html)

PostgreSQL uses a client/server model. A PostgreSQL session consists of the following cooperating processes (programs):

A server process, which manages the database files, accepts connections to the database from client applications, and performs database actions on behalf of the clients. The database server program is called postgres.

The user's client (frontend) application that wants to perform database operations. Client applications can be very diverse in nature.

![](assets/images/postgres/overview/Screen%20Shot%202022-12-24%20at%202.11.12%20PM.png)

---
### Creating a Database
>[PostgreSQL: Documentation: 15: 1.3. Creating a Database](https://www.postgresql.org/docs/current/tutorial-createdb.html)

A running PostgreSQL server can manage many databases. Typically, a separate database is used for each project or for each user.
![](assets/images/postgres/overview/Screen%20Shot%202023-01-02%20at%2011.29.53%20PM.png)

---
### Accessing the Database
>[PostgreSQL: Documentation: 15: 1.4. Accessing a Database](https://www.postgresql.org/docs/current/tutorial-accessdb.html)

Once you have created a database, you can access it by:
- Running the PostgreSQL interactive terminal program (psql).
- Using an existing graphical frontend tool like pgAdmin or an office suite with ODBC or JDBC support.
- Writing a custom application, using one of the several available language bindings. These possibilities are discussed further in Part IV.

---
### General Interaction
>[PostgreSQL: Documentation: 15: Chapter 2. The SQL Language](https://www.postgresql.org/docs/current/tutorial-sql.html)

[2.1. Introduction](https://www.postgresql.org/docs/current/tutorial-sql-intro.html)
[2.2. Concepts](https://www.postgresql.org/docs/current/tutorial-concepts.html)
[2.3. Creating a New Table](https://www.postgresql.org/docs/current/tutorial-table.html)
[2.4. Populating a Table With Rows](https://www.postgresql.org/docs/current/tutorial-populate.html)
[2.5. Querying a Table](https://www.postgresql.org/docs/current/tutorial-select.html)
[2.6. Joins Between Tables](https://www.postgresql.org/docs/current/tutorial-join.html)
[2.7. Aggregate Functions](https://www.postgresql.org/docs/current/tutorial-agg.html)
[2.8. Updates](https://www.postgresql.org/docs/current/tutorial-update.html)
[2.9. Deletions](https://www.postgresql.org/docs/current/tutorial-delete.html)

---
### Advanced Features
> https://www.postgresql.org/docs/current/tutorial-advanced.html

[3.1. Introduction](https://www.postgresql.org/docs/current/tutorial-advanced-intro.html)
[3.2. Views](https://www.postgresql.org/docs/current/tutorial-views.html)
[3.3. Foreign Keys](https://www.postgresql.org/docs/current/tutorial-fk.html)
[3.4. Transactions](https://www.postgresql.org/docs/current/tutorial-transactions.html)
[3.5. Window Functions](https://www.postgresql.org/docs/current/tutorial-window.html)
[3.6. Inheritance](https://www.postgresql.org/docs/current/tutorial-inheritance.html)
[3.7. Conclusion](https://www.postgresql.org/docs/current/tutorial-conclusion.html)
