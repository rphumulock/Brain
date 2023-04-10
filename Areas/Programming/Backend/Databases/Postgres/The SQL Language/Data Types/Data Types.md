> https://www.postgresql.org/docs/current/datatype.html

### Booleans
- Can use true, 'true', 'y', 'yes' '1' 

---
### Date Time
> https://www.postgresql.org/docs/15/datatype-datetime.html

- Intervals are useful for time calculations rather than using a library.

---
### Char Varchar Text
- Text for large string data
- Varchar used most often
- Char will pad the strings with blanks if they aren't the exact size

---
### Numbers
> https://www.postgresql.org/docs/15/datatype-numeric.html
- Use big enough numbers to store data

---
### UUID
> https://www.postgresql.org/docs/15/datatype-uuid.html
- Good for generating unique ID's across different databases
- Need an extension to use

---
### Arrays
>[PostgreSQL: Documentation: 15: 8.15. Arrays](https://www.postgresql.org/docs/15/arrays.html)

Related:
[[Aggregates and Sequences#Array Functions|Array Functions]]

- Dimensions are ignored by Postgres
- Requires less storage than jsonb
- Indexing with gin is very fast
- Planner likely to make better decicions with Postgres array then jsonb as it collects statistics on it's contents but doesn't on jsonb
- Limited to one data type vs jsonb
- Have to follow strict order of array data input
- Arrays are some of the most useful data types for **storing lists of information.** Whether you have a list of baseball scores, blog tags, or favorite book titles, arrays can be found everywhere. The bigger question, however, is why you'd even consider using arrays. The answer comes down to performance and ease of use.
- Aggregating and joining data across tables and rows can be difficult, and depending on the type of data you're storing, it might get expensive too.
- PostgreSQL gives the opportunity **to define a column of a table as a variable length single or multidimensional array.**
- Arrays of any built-in or **user-defined base type, enum type, or composite type** can be created.
- Arrays can be used to **denormalize data and avoid lookup tables**. A good rule of thumb for using them that way is that you mostly use the array as a whole, even if you might at times search for elements in the array
- Heavier processing is going to be more complex than a lookup table
- PostgreSQL allows a table column to **contain multi-dimensional arrays** that can be of any built-in or user-defined data type
- Arrays have an advantage over large plain text fields in that **data remains in a discreet and addressable form**. PostgreSQL also has a lot of functions dealing with arrays

**One Dimensional Array**
- It allows random access and all the elements can be accessed with the help of their index.
- The size of the array is fixed

**Multi-dimensional array**
- It also allows random access and all the elements can be accessed with the help of their index.
- It can also be seen as a collection of 1D arrays. It is also known as the **Matrix**.
- Its dimension can be increased from 2 to 3 and 4 so on.
- Store a **‘list of lists’** of the element
- Represent multiple data items as a table consisting of rows and columns.
- Multi-dimensional arrays are used to implement matrices.

---
### Hstore
> https://www.postgresql.org/docs/15/hstore.html

---
### JSON
>[PostgreSQL: Documentation: 15: 8.14. JSON Types](https://www.postgresql.org/docs/15/datatype-json.html)

[[Aggregates and Sequences#JSON|Functions/Operators]]
[[Indexes#JSON|Index]]

- JsonB provides additional operators for querying which is huge
- Supports indexing
- JsonB array easier syntactically than arrays
- JsonB has to parse json data into binary
- JsonB tends to be slower than json when writing but faster when reading
- JsonB doesn't always keep the order of the object
- JSON nulls are not exactly the same as SQL nulls. In SQL, a zero-length string ("") is distinct from an SQL null value, which represents **the absence of a definite value.** In JSON, null is an actual value, and it is **represented by a JSON literal ("null").** JSON nulls must be distinguishable from SQL nulls. SQL/JSON syntax enables the application author to select whether SQL null values are included in a JSON object or array being constructed, or whether they should be omitted from the JSOn object or array being constructed.
- Can format JSON documents of all your tables and vice versa

---
### Network Types
> https://www.postgresql.org/docs/15/datatype-net-types.html

---
### Domain
> https://www.postgresql.org/docs/15/domains.html

A _domain_ is a user-defined data type that is based on another _underlying type_. Optionally, it can have constraints that restrict its valid values to a subset of what the underlying type would allow.

- Will only show for the schema is was defined in
- Useful for reuse across database definitions

###### Example:
```
CREATE DOMAIN positive_numeric INT NOT NULL CHECK (VALUE > 0);
```

---
### Composite
> https://www.postgresql.org/docs/15/rowtypes.html

A _composite type_ represents the structure of a row or record; it is essentially just a list of field names and their data types. PostgreSQL allows composite types to be used in many of the same ways that simple types can be used. For example, a column of a table can be declared to be of a composite type.

---
### Enum
> https://www.postgresql.org/docs/15/datatype-enum.html

---
### Conversions
> [PostgreSQL: Documentation: 15: Chapter 10. Type Conversion](https://www.postgresql.org/docs/15/typeconv.html)

There are two types of data conversions:
- Explicit conversions are done using CAST or conversion functions
- Implicit conversions are done automatically

###### Example:
```
-- Implicit

SELECT * FROM x WHERE id = ('1' vs 1);
SELECT 20 !;

-- Explicit

SELECT * FROM x WHERE id = INTEGER 1;
SELECT CAST('10' as INTEGER);
SELECT '10'::INTEGER
```

---
### Array
> https://www.postgresql.org/docs/15/arrays.html

> https://www.postgresql.org/docs/15/functions-array.html

> https://www.postgresql.org/docs/15/functions-comparisons.html

PostgreSQL allows columns of a table to be defined as variable-length multidimensional arrays.

---
### Range
> https://www.postgresql.org/docs/current/rangetypes.html

> https://www.postgresql.org/docs/9.3/functions-range.html

Range types are data types representing a range of values of some element type (called the range's _subtype_). For instance, ranges of `timestamp` might be used to represent the ranges of time that a meeting room is reserved.

---
### Formatting
> [PostgreSQL: Documentation: 15: 9.8. Data Type Formatting Functions](https://www.postgresql.org/docs/current/functions-formatting.html)















