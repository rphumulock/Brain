The documentation mentions the relationship between locales and SQL features in [Locale Support](https://www.postgresql.org/docs/current/locale.html):

> The locale settings influence the following SQL features:
> 
> -   Sort order in queries using ORDER BY or the standard comparison operators on textual data
>     
> -   The upper, lower, and initcap functions
>     
> -   Pattern matching operators (LIKE, SIMILAR TO, and POSIX-style regular expressions); locales affect both case insensitive matching and the classification of characters by character-class regular expressions
>     
> -   The to_char family of functions
>     
> -   The ability to use indexes with LIKE clauses
>     

The first item (sort order) is about `LC_COLLATE` and the others seem all to be about `LC_CTYPE`.

##### LC_COLLATE
`LC_COLLATE` affects comparisons between strings. In practice, the most visible effect is the sort order. `LC_COLLATE='C'` (or `POSIX` which is a synonym) means that it's the byte order that drives comparisons, whereas a locale in the `language_REGION` form means that cultural rules will drive the comparisons.

An example with french names, executed from inside an UTF-8 database:

```sql
select firstname from (values ('bernard'), ('bérénice'), ('béatrice'), ('boris'))
 AS l(firstname)
order by firstname collate "fr_FR";
```

Result:

 firstname 
-----------
 béatrice
 bérénice
 bernard
 boris

`béatrice` comes before `boris`, because the accented E compares against O as if it was non-accented. It's a cultural rule.

This differs from what happens with a `C` locale:

```sql
select firstname from (values ('bernard'), ('bérénice'), ('béatrice'), ('boris')) 
 AS l(firstname)
order by firstname collate "C";
```

Result:

 firstname 
-----------
 bernard
 boris
 béatrice
 bérénice

Now the names with accented E are pushed at the end of the list. The byte representation of `é` in UTF-8 is the hexadecimal `C3 A9` and for `o` it's `6f`. `c3` is greater than `6f` so under the `C` locale, `'béatrice' > 'boris'`.

It's not just accents. There a more complex rules with hyphenation, punctuation, and weird characters like `œ`. Weird cultural rules are to be expected in every locale.

Now if the strings to compare happen to mix different languages, as when having a `firstname` column for people from all other the world, it might be that any particular locale should not dominate, anyway, because different alphabets for different languages have not been designed to be sorted against each other.

In this case `C` is a rational choice, and it has the advantage of being faster, because nothing can beat pure byte comparisons.

##### LC_CTYPE
Having `LC_CTYPE` set to 'C' implies that C functions like `isupper(c)` or `tolower(c)` give expected results only for characters in the US-ASCII range (that is, up to codepoint 0x7F in Unicode).

Because SQL functions like `upper()`, `lower()` or `initcap` are implemented in Postgres on top of these libc functions, they're affected by this as soon as there are non US-ASCII characters in strings.

Example:

```sql
test=> show lc_ctype;
  lc_ctype   
-------------
 fr_FR.UTF-8
(1 row)

-- Good result
test=> select initcap('élysée');
 initcap 
---------
 Élysée
(1 row)

-- Wrong result
-- collate "C" is the same as if the db has been created with lc_ctype='C'
test=> select initcap('élysée' collate "C");
 initcap 
---------
 éLyséE
(1 row)
```

For the `C` locale, `é` is treated as an uncategorizable character.

Similarly wrong results are also obtained with regular expressions:

```sql
test=> select 'élysée' ~ '^\w+$';
 ?column? 
----------
 t
(1 row)

test=> select 'élysée' COLLATE "C" ~ '^\w+$';
 ?column? 
----------
 f
(1 row)
```
