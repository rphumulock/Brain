>https://www.postgresql.org/docs/15/tablefunc.html

The `crosstab` function is used to produce “pivot” displays, wherein data is listed across the page rather than down. For example, we might have data like:

| Name | Subject | Score |
| - |  - |  - |
| Adam | Math | 80 |
| Linda | Science | 30 |
| Adam | English | 20 |
| Adam | Science | 40 |
| Linda | Math | 90 |
| Linda | English | 65 |

And we turn it into a Pivot table like:

| Name | Math | Science | English |
| - |  - |  - | - |
| Adam | 80 | 40 | 20 |
| Linda | 90 | 30 | 65 |

---
### Static

These cross tabs have columns that are explicitly defined. This makes them less fluid and also less practical (in cases where you have a lot of colums).

Syntax:

```
Select * from crosstab
(
	-> Select query expression to get the data <-
) as alias
(
col1 type,
col2 type
...
)
```

The select must return 3 columns:
- X axis - Fields which become the columns.
- Y axis - Field that will be used as an identifer (should be distinct) - which becomes the rows.
- Z - Value generated for each cell.

`SELECT rowid, attributes (columns), value for column FROM crosstab('...') AS ct(row_name text, category_1 text, category_2 text);`

- `row_name` would be essentially the distinct (or unique) identifer - in our example the name column
- `category` is the columns we want
- The value in our query becomes the values for each column. 
- Order in the query matters.

Example:
```
// Static
SELECT * from crosstab
(
	$$
		SELECT
		name,
		subject,
		score
		FROM scores
		ORDER BY 1, 2
	$$
) AS ct
(
	name VARCHAR,
	MATH NUMERIC,
	SCIENCE NUMERIC,
	ENGLISH NUMERIC
)
```

It is possible to build cross tabs with generic queries in SQL but it might not be efficient if you have a lot of data.
```
// Using CASE
SELECT
y,
aggregate_function(CASE WHEN x = "value1" THEN value END),
aggregate_function(CASE WHEN x = "value2" THEN value END)
...
FROM table
GROUP BY y
ORDER BY 1

// Using FILTER
SELECT
y,
aggregate_function() FILTER (WHERE x = "value1") AS "value1" ,
aggregate_function() FILTER (WHERE x = "value2") AS "value2" ,
...
FROM table
GROUP BY y
ORDER BY 1

```

---
### Dynamic

- Crosstab provides a second argument you can use to supply the columns dynamically that you want to utilize. This will also avoid complications with null values and is the preferred way to run crosstabs.
- You can also create dynamic crosstabs by utilizing json or array aggregation functions.

Above example but done dynamically:
```
// Dynamic
SELECT * from crosstab
(
	$$
		SELECT
		name,
		subject,
		score
		FROM scores
		ORDER BY 1, 2
	$$,
	$$
		SELECT DISTINCT(subject) from scores
	$$
) AS ct
(
	name VARCHAR,
	MATH NUMERIC,
	SCIENCE NUMERIC,
	ENGLISH NUMERIC
)
```

Using aggregates:
```
CREATE OR REPLACE FUNCTION pivotcode (
	tablename VARCHAR,
	myrow VARCHAR,
	mycol VARCHAR,
	mycell VARCHAR,
	celldatatype VARCHAR
) RETURNS VARCHAR
LANGUAGE PLPGSQL AS
$$
DECLARE
	dynsql1 VARCHAR;
	dynsql2 VARCHAR;
	columnlist VARCHAR;
BEGIN

-- 1 retrive list of all DISTINCT column name

-- SELECT DISTINCT(column_name) FROM table_name

dynsql1 = 'SELECT STRING_AGG(DISTINCT ''_''||'||mycol||'||'' '||celldatatype||''','','' ORDER BY ''_''||'||mycol||'||'' '||celldatatype||''') FROM '||tablename||';';

EXECUTE dynsql1 INTO columnlist;

-- 2. setup the crosstab query

dynsql2 = 'SELECT * FROM crosstab (

''SELECT '||myrow||','||mycol||','||mycell||' FROM '||tablename||' GROUP BY 1,2 ORDER BY 1,2'',

''SELECT DISTINCT '||mycol||' FROM '||tablename||' ORDER BY 1''

)

AS newtable (

'||myrow||' VARCHAR,'||columnlist||'

);';

-- 3. return the query

RETURN dynsql2;

END

$$
```

---
### Terminal interaction

Can use psql in the terminal to run a query and run /crosstabview to see the data in cross tab format.