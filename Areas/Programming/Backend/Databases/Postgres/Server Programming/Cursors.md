>https://www.postgresql.org/docs/15/plpgsql-cursors.html

Rather than executing a whole query at once, it is possible to set up a _cursor_ that encapsulates the query, and then read the query result a few rows at a time.

- Good for pagination.

- [43.7.1. Declaring Cursor Variables](https://www.postgresql.org/docs/15/plpgsql-cursors.html#PLPGSQL-CURSOR-DECLARATIONS)
- [43.7.2. Opening Cursors](https://www.postgresql.org/docs/15/plpgsql-cursors.html#PLPGSQL-CURSOR-OPENING)
- [43.7.3. Using Cursors](https://www.postgresql.org/docs/15/plpgsql-cursors.html#PLPGSQL-CURSOR-USING)
- [43.7.4. Looping through a Cursor's Result](https://www.postgresql.org/docs/15/plpgsql-cursors.html#PLPGSQL-CURSOR-FOR-LOOP)