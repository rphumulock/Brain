[4.1. Lexical Structure](https://www.postgresql.org/docs/15/sql-syntax-lexical.html)
[4.2. Value Expressions](https://www.postgresql.org/docs/15/sql-expressions.html)
[4.3. Calling Functions](https://www.postgresql.org/docs/15/sql-syntax-calling-funcs.html)



--- EDIT


## Overview

> https://www.postgresql.org/docs/15/sql-syntax.html

  

---

### Introduction

![](assets/images/postgres/overview/Screen%20Shot%202022-12-24%20at%202.43.47%20PM.png)

  

Postgres and many other databases use SQL. Postgres can sometimes use SQL syntax that's unique to Postgres.

  

---

### Lexical Structure

> https://www.postgresql.org/docs/15/sql-syntax-lexical.html

  

SQL input consists of a sequence of commands. A command is composed of a sequence of tokens, terminated by a semicolon (“;”). The end of the input stream also terminates a command. Which tokens are valid depends on the syntax of the particular command.

  

A token can be a key word, an identifier, a quoted identifier, a literal (or constant), or a special character symbol. Tokens are normally separated by whitespace (space, tab, newline), but need not be if there is no ambiguity (which is generally only the case if a special character is adjacent to some other token type).

  

---

### Value Expressions

> https://www.postgresql.org/docs/15/sql-expressions.html

  

Value expressions are used in a variety of contexts, such as in the target list of the SELECT command, as new column values in INSERT or UPDATE, or in search conditions in a number of commands. The result of a value expression is sometimes called a scalar, to distinguish it from the result of a table expression (which is a table). Value expressions are therefore also called scalar expressions (or even simply expressions). The expression syntax allows the calculation of values from primitive parts using arithmetic, logical, set, and other operations.

  

---

### Calling Functions

> https://www.postgresql.org/docs/15/sql-syntax-calling-funcs.html

  

PostgreSQL allows functions that have named parameters to be called using either positional or named notation or mixed.

  

---

### Commands

> https://www.postgresql.org/docs/15/sql-commands.html

  

---

### Keywords

> https://www.postgresql.org/docs/current/sql-keywords-appendix.html