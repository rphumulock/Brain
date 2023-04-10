### Database Design Process

![[design1.png]]

![[design2.png]]

---
### Polymorphic Associations

A relationship from one type to multiple types.

Three ways you can represent these:
1. Just ID's for everything without foreign keys. This is bad because it ignores thje foreign key advantages of postgres.
1. Table that includes multiple similar columns with nulls where the option isn't available.
1. Just make new tables for each item - typically the best option.

![[design7.png]]

---
### Relationships

![[design5.png]]

![[design3.png]]

![[design4.png]]

![[design6.png]]

---
### Junction Table
>[Associative entity - Wikipedia](https://en.wikipedia.org/wiki/Associative_entity#:~:text=An%20associative%20(or%20junction)%20table,to%20the%20individual%20data%20tables)

Used as a many to many linking relationship.

```
CREATE TABLE movies_actors (
	movie_id INT REFERENCES movies (movie_id),
	actor_id INT REFERENCES actors (actor_id),
	PRIMARY KEY (movie_id, actor_id)
);
```

---
### Schema Designer
> https://dbdiagram.io/d

> https://ondras.zarovi.cz/sql/demo/?keyword=default