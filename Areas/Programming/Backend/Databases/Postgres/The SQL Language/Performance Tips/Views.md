>https://www.postgresql.org/docs/15/tutorial-views.html

A view is essentially like using the DRY philosophy in Postgres for queries. It also has some speed advantages because Postgres has all the information for how to fetch the data.

Making liberal use of views is a key aspect of good SQL database design. Views allow you to encapsulate the details of the structure of your tables, which might change as your application evolves, behind consistent interfaces. It also provides a level of security and obfuscation.

- Views allow you to reuse queries
- Views allow you another security layer on your tables

---
### Updatable Views
>[PostgreSQL: Documentation: 15: CREATE VIEW](https://www.postgresql.org/docs/15/sql-createview.html)

Simple views are automatically updatable: the system will allow `INSERT`, `UPDATE` and `DELETE` statements to be used on the view in the same way as on a regular table. A view is automatically updatable if it satisfies all of the following conditions:

- The view must have exactly one entry in its `FROM` list, which must be a table or another updatable view.
- The view definition must not contain `WITH`, `DISTINCT`, `GROUP BY`, `HAVING`, `LIMIT`, or `OFFSET` clauses at the top level.
- The view definition must not contain set operations (`UNION`, `INTERSECT` or `EXCEPT`) at the top level.
- The view's select list must not contain any aggregates, window functions or set-returning functions.

###### With Check Option
This option controls the behavior of automatically updatable views. When this option is specified, `INSERT` and `UPDATE` commands on the view will be checked to ensure that new rows satisfy the view-defining condition (that is, the new rows are checked to ensure that they are visible through the view). If they are not, the update will be rejected. If the `CHECK OPTION` is not specified, `INSERT` and `UPDATE` commands on the view are allowed to create rows that are not visible through the view. The following check options are supported:

`LOCAL`
New rows are only checked against the conditions defined directly in the view itself. Any conditions defined on underlying base views are not checked (unless they also specify the `CHECK OPTION`).

`CASCADED`
New rows are checked against the conditions of the view and all underlying base views. If the `CHECK OPTION` is specified, and neither `LOCAL` nor `CASCADED` is specified, then `CASCADED` is assumed.

The `CHECK OPTION` may not be used with `RECURSIVE` views.

---
## Materialized Views
>https://www.postgresql.org/docs/15/rules-materializedviews.html

Materialized views in PostgreSQL use the rule system like views do, but persist the results in a table-like form.

While access to the data stored in a materialized view is often much faster than accessing the underlying tables directly or through a view, the data is not always current and it will need to be refreshed.

- Caches results of a heavy query
- Can add indexes and primary keys on them
- Supports vacuum and analyze which are good if you refresh the view often
- Run `WITH DATA` to start the view with data
- Need to be maintained



---

EDIT 


---

  

## Overview

> https://www.postgresql.org/docs/15/tutorial-views.html

  

> https://www.postgresql.org/docs/15/rules-views.html#id-1.8.6.7.7

  

---

#### When to use a View

- Use a View when a query or portion of a query is very commonly used and can be reused all over.

- Database needs a new table and it's hard to fit it in.

  

Example:

  

Tables already designed and you cannot change them. Is it a bad design? Not neccessarily.

  

![](views8.png)

  

![](views1.png)

  

![](views2.png)

  

![](views3.png)

  

Create a View

![](views5.png)

  

Now you can swap complicated queries to use the View. Any query can use it now.

  

![](views10.png)

  

To

  

![](views7.png)

  

![](views4.png)

  

![](views6.png)

  

Can Create/Update and Drop Views.

  

---

### Materialized Views

> https://www.postgresql.org/docs/15/rules-materializedviews.html

  

![](views12.png)

  

![](views13.png)

  

![](views14.png)

  

Create View of slow query that you can run once and get data from. Then can run once a certain interval that is needed (week/month?) only. Will have to refresh that View manually or cron job:

  

`REFRESH MATERIALIZED VIEW weekly_likes;`

  

![](views15.png)