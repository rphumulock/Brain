>https://www.postgresql.org/docs/15/tutorial-window.html

A _window function_ performs a calculation across a set of table rows that are somehow related to the current row. This is comparable to the type of calculation that can be done with an aggregate function. However, window functions do not cause rows to become grouped into a single output row like non-window aggregate calls would.

>List of Window Functions: 
>https://www.postgresql.org/docs/9.3/functions-window.html

>Syntax: 
>https://www.postgresql.org/docs/9.3/sql-expressions.html#SYNTAX-WINDOW-FUNCTIONS

- Can call aggregation functions on specific ranges of data called "frames"
- When a query involves multiple window functions, it is possible to write out each one with a separate `OVER` clause, but this is duplicative and error-prone if the same windowing behavior is wanted for several functions. Instead, each windowing behavior can be named in a `WINDOW` clause and then referenced in `OVER`.

![[windowfunc1.png]]

![[windowfunc2.png]]