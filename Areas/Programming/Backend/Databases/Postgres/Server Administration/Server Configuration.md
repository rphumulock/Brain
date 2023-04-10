>[PostgreSQL: Documentation: 15: Chapter 20. Server Configuration](https://www.postgresql.org/docs/15/runtime-config.html)

---
### Internationalization
>https://www.postgresql.org/docs/15/multibyte.html

Can set database compatibility with different languaes.

---
### Security
>https://www.postgresql.org/docs/15/ddl-priv.html

- Should start from zero level security and revoke all rights when configuring security.
- Can use /z command in psql to view table security configuration.

![[Screen Shot 2023-03-16 at 11.21.09 PM.png]]



---
### Data Encryption
>https://www.postgresql.org/docs/15/encryption-options.html

PostgreSQL offers encryption at several levels, and provides flexibility in protecting data from disclosure due to database server theft, unscrupulous administrators, and insecure networks. Encryption might also be required to secure sensitive data such as medical records or financial transactions.

Extension
>https://www.postgresql.org/docs/15/pgcrypto.html

---
### Roles
>https://www.postgresql.org/docs/15/user-manag.html

PostgreSQL manages database access permissions using the concept of _roles_. A role can be thought of as either a database user, or a group of database users, depending on how the role is set up. Roles can own database objects (for example, tables and functions) and can assign privileges on those objects to other roles to control who has access to which objects. Furthermore, it is possible to grant _membership_ in a role to another role, thus allowing the member role to use privileges assigned to another role.

The concept of roles subsumes the concepts of “users” and “groups”. In PostgreSQL versions before 8.1, users and groups were distinct kinds of entities, but now there are only roles. Any role can act as a user, a group, or both.
