---
layout: page
title: Databases and SQL
subtitle: Filtering
minutes: 30
---
> ## Learning Objectives {.objectives}
>
> *   Write queries that select records that satisfy user-specified conditions.
> *   Explain the order in which the clauses in a query are executed.

One of the most powerful features of a database is
the ability to [filter](reference.html#filter) data,
i.e.,
to select only those records that match certain criteria.
For example,
suppose we want to see when a particular site was visited.
We can select these records from the `Visited` table
by using a `WHERE` clause in our query:

~~~ {.sql}
SELECT * FROM Visited WHERE site='DR-1';
~~~

|ident|site|dated     |
|-----|----|----------|
|619  |DR-1|1927-02-08|
|622  |DR-1|1927-02-10|
|844  |DR-1|1932-03-22|

The database manager executes this query in two stages.
First,
it checks each row in the `Visited` table
to see which ones satisfy the `WHERE`.
It then uses the column names following the `SELECT` keyword
to determine which columns to display.

This processing order means that
we can filter records using `WHERE`
according to values in columns that we are not selecting:

~~~ {.sql}
SELECT ident FROM Visited WHERE site='DR-1';
~~~

|ident|
|-----|
|619  |
|622  |
|844  |

<img src="fig/sql-filter.svg" alt="SQL Filtering in Action" />

## The `AND` and `OR` operators

The clause `WHERE` performs a Boolean operation on the data by determining which entries in the table satisfy the condition (are True) and which don't (are False). We can use many other Boolean operators to filter our data and we can combine them for more complex filtering.
For example,
we can ask for all information from the DR-1 site collected before 1930:

~~~ {.sql}
SELECT * FROM Visited WHERE site='DR-1' AND dated<'1930-01-01';
~~~

|ident|site|dated     |
|-----|----|----------|
|619  |DR-1|1927-02-08|
|622  |DR-1|1927-02-10|

> ## Data types for dates and times {.callout}
>
> Most database managers have a special data type for dates.
> In fact, many have two:
> one for dates,
> such as "May 31, 1971",
> and one for durations,
> such as "31 days".
>
> SQLite is special:
> instead of using a special type,
> it stores dates as either text
> (in the ISO-8601 standard format "YYYY-MM-DD HH:MM:SS.SSSS"),
> real numbers
> (the number of days since November 24, 4714 BCE),
> or integers
> (the number of seconds since midnight, January 1, 1970).
> While this all sounds very complicated, being aware of how your particular
> database manager deals with dates will prevent a lot of headaches later on.

If we want to find out what measurements were taken by either Lake or Roerich,
we can combine the tests on their names using `OR`:

~~~ {.sql}
SELECT * FROM Survey WHERE person='lake' OR person='roe';
~~~

|taken|person|quant|reading|
|-----|------|-----|-------|
|734  |lake  |sal  |0.05   |
|751  |lake  |sal  |0.1    |
|752  |lake  |rad  |2.19   |
|752  |lake  |sal  |0.09   |
|752  |lake  |temp |-16.0  |
|752  |roe   |sal  |41.6   |
|837  |lake  |rad  |1.46   |
|837  |lake  |sal  |0.21   |
|837  |roe   |sal  |22.5   |
|844  |roe   |rad  |11.25  |

We can also use the clause `IN` to see if a value is in a specific set:

~~~ {.sql}
SELECT * FROM Survey WHERE person IN ('lake', 'roe');
~~~

|taken|person|quant|reading|
|-----|------|-----|-------|
|734  |lake  |sal  |0.05   |
|751  |lake  |sal  |0.1    |
|752  |lake  |rad  |2.19   |
|752  |lake  |sal  |0.09   |
|752  |lake  |temp |-16.0  |
|752  |roe   |sal  |41.6   |
|837  |lake  |rad  |1.46   |
|837  |lake  |sal  |0.21   |
|837  |roe   |sal  |22.5   |
|844  |roe   |rad  |11.25  |

We can combine `AND` with `OR`,
but we need to be careful about which operator is executed first.
For example, if we *don't* use parentheses,
we get this:

~~~ {.sql}
SELECT * FROM Survey WHERE quant='sal' AND person='lake' OR person='roe';
~~~

|taken|person|quant|reading|
|-----|------|-----|-------|
|734  |lake  |sal  |0.05   |
|751  |lake  |sal  |0.1    |
|752  |lake  |sal  |0.09   |
|752  |roe   |sal  |41.6   |
|837  |lake  |sal  |0.21   |
|837  |roe   |sal  |22.5   |
|844  |roe   |rad  |11.25  |

which displays salinity measurements by Lake,
and *any* measurement by Roerich.
We probably want this instead:

~~~ {.sql}
SELECT * FROM Survey WHERE quant='sal' AND (person='lake' OR person='roe');
~~~

|taken|person|quant|reading|
|-----|------|-----|-------|
|734  |lake  |sal  |0.05   |
|751  |lake  |sal  |0.1    |
|752  |lake  |sal  |0.09   |
|752  |roe   |sal  |41.6   |
|837  |lake  |sal  |0.21   |
|837  |roe   |sal  |22.5   |

Finally,
we can use `DISTINCT` with `WHERE`
to give a second level of filtering:

~~~ {.sql}
SELECT DISTINCT person, quant FROM Survey WHERE person='lake' OR person='roe';
~~~

|person|quant|
|------|-----|
|lake  |sal  |
|lake  |rad  |
|lake  |temp |
|roe   |sal  |
|roe   |rad  |

But remember:
`DISTINCT` is applied to the combined values in the chosen columns,
not to the entire rows as they are being processed.

> What we have just done is how most people "grow" their SQL queries.
> We started with something simple that did part of what we wanted,
> then added more clauses one by one,
> testing their effects as we went.
> This is a good strategy --- in fact,
> for complex queries it's often the *only* strategy --- but
> it depends on quick turnaround,
> and on us recognizing the right answer when we get it.
>
> The best way to achieve quick turnaround is often
> to put a subset of data in a temporary database
> and run our queries against that,
> or to fill a small database with synthesized records.
> For example,
> instead of trying our queries against an actual database of 20 million Australians,
> we could run it against a sample of ten thousand,
> or write a small program to generate ten thousand random (but plausible) records
> and use that.

> ## Finding Outliers {.challenge}
>
> Normalized salinity readings are supposed to be between 0.0 and 1.0.
> Write a query that selects all records from `Survey`
> with salinity values outside this range.


## Filtering by partial matches

If we want to know something just about the site names beginning with "DR" we can use the `LIKE` keyword. The `LIKE` operator is used to match text fields against a pattern using [wildcard](reference.html#wildcard). There are two wildcards used with the `LIKE` operator: the percent sign (%) and the underscore (_).

The percent symbol (%) matches zero, one, or multiple numbers or characters at that location in the search pattern. The underscore (_) represents a single number or character. Wildcards can be used at the beginning, middle, or end of the string and can combined in a single string.

~~~ {.sql}
SELECT * FROM Visited WHERE site LIKE 'DR%';
~~~

|ident|site |dated     |
|-----|-----|----------|
|619  |DR-1 |1927-02-08|
|622  |DR-1 |1927-02-10|
|734  |DR-3 |1930-01-07|
|735  |DR-3 |1930-01-12|
|751  |DR-3 |1930-02-26|
|752  |DR-3 |          |
|844  |DR-1 |1932-03-22|


> ## Matching Patterns {.challenge}
>
> Which of these expressions are true?
>
> * `LIKE 'a'` would find `'a'`
> * `LIKE '%a'` would find `'a'`
> * `LIKE '%a'` would find `'beta'`
> * `LIKE '_a'` would find `'a'`
> * `LIKE 'a%%'` would find `'alpha'`
> * `LIKE '%a%'` would find `'alpha'`
> * `LIKE '_a%'` would find `'alpha'`
> * `LIKE 'a%p%'` would find `'alpha'`
> * `LIKE 'a%p_'` would find `'alpha'`

> ## Matching dates {.challenge}
>
> Write a query that searches the `Visited` table only for entries dated
>
> * 1930-01-12
> * 1932-01-14
>
> using the LIKE operator and wildcards. Grow your query gradually until all other
> entries are removed.
