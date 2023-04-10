### Overview

  

Generally want to make a "test" database.

  

![](assets/images/postgres/testing/Screen%20Shot%202023-01-26%20at%204.37.55%20PM.png)

  

- Can use migrations to seed or remove data before or after tests.

- Can also clean up after tests.

  

Can create client test runners.

  

![](assets/images/postgres/testing/Screen%20Shot%202023-01-26%20at%204.24.47%20PM.png)

  

[Example Code for a test](assets/code/postgres/testing/src/test/routes/users.test.js)

  

---

### Synching Tests

  

Running multiple tests at once on one database can create race conditions.

  

![](assets/images/postgres/testing/Screen%20Shot%202023-01-26%20at%204.47.39%20PM.png)

  

A solution is to use [Schemas](a.programming.backend.postgres.datadef.schemas.md) (not the same schema as for data).

  

![](assets/images/postgres/testing/Screen%20Shot%202023-01-26%20at%204.52.19%20PM.png)

  

Can create a test. schema instead of using public.schema.

  

![](assets/images/postgres/testing/Screen%20Shot%202023-01-26%20at%204.55.16%20PM.png)

  

Create a schema per test case by creating a new user and role per each test.

  

![](assets/images/postgres/testing/Screen%20Shot%202023-01-26%20at%205.03.26%20PM.png)

  

[Example Code](assets/code/postgres/testing/src/test/context.js)