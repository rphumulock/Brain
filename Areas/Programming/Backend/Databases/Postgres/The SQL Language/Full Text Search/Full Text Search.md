>[PostgreSQL: Documentation: 15: Chapter 12. Full Text Search](https://www.postgresql.org/docs/current/textsearch.html)

Full Text Searching (or just _text search_) provides the capability to identify natural-language _documents_ that satisfy a _query_, and optionally to sort them by relevance to the query. The most common type of search is to find all documents containing given _query terms_ and return them in order of their _similarity_ to the query. Notions of `query` and `similarity` are very flexible and depend on the specific application. The simplest search considers `query` as a set of words and `similarity` as the frequency of query words in the document.

>https://www.postgresql.org/docs/15/datatype-textsearch.html

PostgreSQL provides two data types that are designed to support full text search, which is the activity of searching through a collection of natural-language _documents_ to locate those that best match a _query_. The `tsvector` type represents a document in a form optimized for text search; the `tsquery` type similarly represents a text query. 

- `tsvector` turns words into lexemes and removes common words without much meaning like "the" and establishes their position within the document
- `tsquery` searches a `tsvector` using conditional logic such as "fox && dog" which would check if fox and dog are both in the document