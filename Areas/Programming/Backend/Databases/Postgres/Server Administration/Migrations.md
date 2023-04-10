  

## Overview

> https://docs.digitalocean.com/products/databases/postgresql/how-to/migrate/

Migrations should be coordinated with whatever API changes we make to be able to communicate with the new changes to the database.

---
### Schema Migration Example

Changes need to be done simultaneously otherwise you can have an API that refers to old bad data for the database or a database that has old bad data and the API is referring to the newer version. They must be coordinated.

![](assets/images/postgres/migrations/Screen%20Shot%202023-01-17%20at%2010.40.34%20PM.png)

![](assets/images/postgres/migrations/Screen%20Shot%202023-01-17%20at%2010.43.59%20PM.png)

![](assets/images/postgres/migrations/Screen%20Shot%202023-01-17%20at%2010.44.22%20PM.png)

---
### Schema Migration

Migration files are used that describe an UP - the changes you want to make as well as a DOWN  - that describes how to undo this UP change.

![](assets/images/postgres/migrations/Screen%20Shot%202023-01-17%20at%2010.40.52%20PM.png)

![](assets/images/postgres/migrations/Screen%20Shot%202023-01-17%20at%2010.41.03%20PM.png)

![](assets/images/postgres/migrations/Screen%20Shot%202023-01-17%20at%2010.41.21%20PM.png)

![](assets/images/postgres/migrations/Screen%20Shot%202023-01-17%20at%2010.36.22%20PM.png)

---
### Libraries that help with migrations

![](assets/images/postgres/migrations/Screen%20Shot%202023-01-17%20at%2010.44.45%20PM.png)

---
### How to connect to your database from the example NPM library.

![](assets/images/postgres/migrations/Screen%20Shot%202023-01-17%20at%2010.46.17%20PM.png)

![](assets/images/postgres/migrations/Screen%20Shot%202023-01-17%20at%2010.46.39%20PM.png)

Run `npm migrate up/down` to undo changes.

---
### Data Migration

Data Migrations should almost always be done via a transaction.

![](assets/images/postgres/migrations/Screen%20Shot%202023-01-18%20at%2012.42.31%20AM.png)

![](assets/images/postgres/migrations/Screen%20Shot%202023-01-18%20at%2012.43.30%20AM.png)

If we try to do everything in one step - we can get into some trouble.
![](assets/images/postgres/migrations/Screen%20Shot%202023-01-18%20at%2012.44.37%20AM.png)

If it takes time - things can happen with our data during that time. If new data is added but that new data takes on the old format - it will now be out of sync. We'd want to do it in a transaction but also be able to have data interactions still occur.
![](assets/images/postgres/migrations/Screen%20Shot%202023-01-18%20at%2012.41.42%20AM.png)

![](assets/images/postgres/migrations/Screen%20Shot%202023-01-18%20at%2010.50.44%20PM.png)

![](assets/images/postgres/migrations/Screen%20Shot%202023-01-18%20at%2010.51.11%20PM.png)

A solution is to do things piecemeal.
![](assets/images/postgres/migrations/Screen%20Shot%202023-01-18%20at%2010.56.34%20PM.png)