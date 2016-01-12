---
layout: page
title: Databases and SQL
subtitle: Calculating New Values
minutes: 30
---
> ## Learning Objectives {.objectives}
>
> *   Write queries that calculate new values for each selected record.

After carefully re-reading the expedition logs,
we realize that the radiation measurements they report
may need to be corrected upward by 5%.
Rather than modifying the stored data,
we can do this calculation on the fly
as part of our query:

~~~ {.sql}
SELECT 1.05 * reading FROM Survey WHERE quant='rad';
~~~

|1.05 * reading|
|--------------|
|10.311        |
|8.19          |
|8.8305        |
|7.581         |
|4.5675        |
|2.2995        |
|1.533         |
|11.8125       |

When we run the query,
the expression `1.05 * reading` is evaluated for each row and displayed, 
although the entries in the database remain unchanged.
Expressions can use any of the fields,
all of usual arithmetic operators,
and a variety of common functions
(Exactly which ones depends on which database manager is being used).
For example,
we can convert temperature readings from Fahrenheit to Celsius
and round to two decimal places:

~~~ {.sql}
SELECT taken, round(5*(reading-32)/9, 2) FROM Survey WHERE quant='temp';
~~~

|taken|round(5\*(reading-32)/9, 2)|
|-----|---------------------------|
|734  |-29.72                     |
|735  |-32.22                     |
|751  |-28.06                     |
|752  |-26.67                     |

We can also combine values from different fields to create new strings by using the string concatenation operator `||`:

~~~ {.sql}
SELECT personal || ' ' || family FROM Person;
~~~

|personal || ' ' || family|
|-------------------------|
|William Dyer             |
|Frank Pabodie            |
|Anderson Lake            |
|Valentina Roerich        |
|Frank Danforth           |

Beware! These operators will work on any field regardless of type, and the answers they give
might be meaningless (Remember than in SQLite dates are strings).

~~~ {.sql}
SELECT 2 * dated FROM Visited;
~~~

|2 * dated|
|-------------------------|
|3854 |
|3854|
|3878|
|3860|
|3860|
| |
|3864|
|3864|

> ## Fixing Salinity Readings {.challenge}
>
> After further reading,
> we realize that Valentina Roerich
> was reporting salinity as percentages.
> Write a query that returns Roerich's salinity measurements
> from the `Survey` table
> with the values divided by 100.

## Unions {.challenge}

The `UNION` operator combines the results of two queries:

~~~ {.sql}
SELECT * FROM Person WHERE ident='dyer' UNION SELECT * FROM Person WHERE ident='roe';
~~~

|ident|personal |family |
|-----|-------- |-------|
|dyer |William  |Dyer   |
|roe  |Valentina|Roerich|

> ## Displaying Salinity {.challenge}
> Use `UNION` to create a consolidated list of salinity measurements
> in which Roerich's, and only Roerich's,
> have been corrected as described in the previous challenge
> (the operator for "not equal" is `!=`).
> The output should be something like:
>
> |taken|reading|
> |-----|-------|
> |619  |0.13   |
> |622  |0.09   |
> |734  |0.05   |
> |751  |0.1    |
> |752  |0.09   |
> |752  |0.416  |
> |837  |0.21   |
> |837  |0.225  |
