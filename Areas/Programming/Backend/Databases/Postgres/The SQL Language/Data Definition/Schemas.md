### Overview
> [PostgreSQL: Documentation: 15: 5.9. Schemas](https://www.postgresql.org/docs/15/ddl-schemas.html)

A database contains one or more named _schemas_, which in turn contain tables. Schemas also contain other kinds of named objects, including data types, functions, and operators. The same object name can be used in different schemas without conflict; for example, both `schema1` and `myschema` can contain tables named `mytable`. Unlike databases, schemas are not rigidly separated: a user can access objects in any of the schemas in the database they are connected to, if they have privileges to do so.

- Must be unique
- Function like a namespace
- Allows you to organize database objects
- Enable multiple users to use one database without interfering with each other (ex for testing)
- Postgres automatically creates a schema called public
- A database can contain multiple schemas
- A schema can only belong to one database
- Schema physical hierarchy: host > cluster > database > schema > object
- Can copy schemas with pg_dump
- Users can only access schemas that they own
- By default every user has `Create` and `Usage` rights on the public schema

---
### Public Schema
> https://www.postgresql.org/docs/15/ddl-schemas.html#DDL-SCHEMAS-PUBLIC

By default such tables (and other objects) are automatically put into a schema named “public”.

---
### Search Paths
> https://www.postgresql.org/docs/15/ddl-schemas.html#DDL-SCHEMAS-PATH

- Can view seach path with: `SHOW search_path`
- You can set the search path with: `SET search_path TO 'test', 'public';`
- $user will sign us on to the schema that is named after the current user. It's the prioritized schema.

![[schemas.png]]
  
