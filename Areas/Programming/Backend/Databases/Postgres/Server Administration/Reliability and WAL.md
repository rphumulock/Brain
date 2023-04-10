>https://www.postgresql.org/docs/current/wal.html

---
#### Asynchronous Commit
>https://www.postgresql.org/docs/current/wal-async-commit.html

As described in the previous section, transaction commit is normally _synchronous_: the server waits for the transaction's WAL records to be flushed to permanent storage before returning a success indication to the client. The client is therefore guaranteed that a transaction reported to be committed will be preserved, even in the event of a server crash immediately after. However, for short transactions this delay is a major component of the total transaction time. Selecting asynchronous commit mode means that the server returns success as soon as the transaction is logically completed, before the WAL records it generated have actually made their way to disk. This can provide a significant boost in throughput for small transactions.