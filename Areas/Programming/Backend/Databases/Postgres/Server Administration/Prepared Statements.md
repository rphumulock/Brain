> https://www.postgresql.org/docs/15/sql-prepare.html

PREPARE creates a prepared statement. A prepared statement is a server-side object that can be used to optimize performance. When the PREPARE statement is executed, the specified statement is parsed, analyzed, and rewritten. When an EXECUTE command is subsequently issued, the prepared statement is planned and executed. This division of labor avoids repetitive parse analysis work, while allowing the execution plan to depend on the specific parameter values supplied.

Typically you use a module to create an API that sits in front of PostGres that will create your prepared statements for you.

Example using the PG module
![[preparedstatements.png]]