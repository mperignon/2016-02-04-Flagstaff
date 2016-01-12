---
layout: page
title: Databases and SQL
subtitle: Sorting and Removing Duplicates
minutes: 30
---
> ## Learning Objectives {.objectives}
>
> *   Write queries that display results in a particular order.
> *   Write queries that eliminate duplicate values from data.


In beginning our examination of the Antarctic data, we want to know:

* what types of measurements were taken at each site; 
* which scientists took measurements on the expedition;
* where each scientist took each measurement

## Selecting unique data

We can examine the `Survey` table to determine which measurements were taken at each site.
Data is often redundant,
so queries often return redundant information.
For example,
if we select the measured quantities
from the `Survey` table,
we get this:

~~~ {.sql}
SELECT quant FROM Survey;
~~~

<table>
<tr><th>quant</th></tr>
<tr><td>rad  </td></tr>
<tr><td>sal  </td></tr>
<tr><td>rad  </td></tr>
<tr><td>sal  </td></tr>
<tr><td>rad  </td></tr>
<tr><td>sal  </td></tr>
<tr><td>temp </td></tr>
<tr><td>rad  </td></tr>
<tr><td>sal  </td></tr>
<tr><td>temp </td></tr>
<tr><td>rad  </td></tr>
<tr><td>temp </td></tr>
<tr><td>sal  </td></tr>
<tr><td>rad  </td></tr>
<tr><td>sal  </td></tr>
<tr><td>temp </td></tr>
<tr><td>sal  </td></tr>
<tr><td>rad  </td></tr>
<tr><td>sal  </td></tr>
<tr><td>sal  </td></tr>
<tr><td>rad  </td></tr>
</table>

This result makes it difficult to see the unique types of `quant` listed in the Survey table.
We can eliminate the redundant output to make the result more readable by adding the `DISTINCT` keyword
to our query:

~~~ {.sql}
SELECT DISTINCT quant FROM Survey;
~~~

<table>
<tr><th>quant</th></tr>
<tr><td>rad  </td></tr>
<tr><td>sal  </td></tr>
<tr><td>temp </td></tr>
</table>

If we want to determine where each type of measurement was collected, 
we can use the `DISTINCT` keyword on multiple columns.
If we select two columns,
the `DISTINCT` keyword returns the unique *pairs* of values in those columns:

~~~ {.sql}
SELECT DISTINCT taken, quant FROM Survey;
~~~

|taken|quant|
|-----|-----|
|619  |rad  |
|619  |sal  |
|622  |rad  |
|622  |sal  |
|734  |rad  |
|734  |sal  |
|734  |temp |
|735  |rad  |
|735  |sal  |
|735  |temp |
|751  |rad  |
|751  |temp |
|751  |sal  |
|752  |rad  |
|752  |sal  |
|752  |temp |
|837  |rad  |
|837  |sal  |
|844  |rad  |

Notice in both cases that duplicates are removed
even if the rows they come from didn't appear to be adjacent in the database table.

## Sorting data

Our next task is to identify the scientists that were on the expedition. Their names are listed in the `Person` table.
As we mentioned earlier,
database records are not stored in any particular order.
This means that query results aren't necessarily sorted in any useful way,
and even if they are,
we might want to sort them differently,
e.g., by their identifier instead of by their personal name.
We can do this in SQL by adding an `ORDER BY` clause to our query:

~~~ {.sql}
SELECT * FROM Person ORDER BY ident;
~~~

|ident  |personal |family  |
|-------|---------|--------|
|danfort|Frank    |Danforth|
|dyer   |William  |Dyer    |
|lake   |Anderson |Lake    |
|pb     |Frank    |Pabodie |
|roe    |Valentina|Roerich |

By default,
results are sorted in ascending order
(i.e.,
from least to greatest).
We can sort in the opposite order using `DESC` (for "descending"):

~~~ {.sql}
SELECT * FROM Person ORDER BY ident DESC;
~~~

|ident  |personal |family  |
|-------|---------|--------|
|roe    |Valentina|Roerich |
|pb     |Frank    |Pabodie |
|lake   |Anderson |Lake    |
|dyer   |William  |Dyer    |
|danfort|Frank    |Danforth|

(And if we want to make it explicit that we're sorting in ascending order,
we can use `ASC` instead of `DESC`.)


We can also sort on several fields at once.
For example,
this query sorts results from the 'Survey' table first in ascending order by `taken`,
and then in descending order by `person`
within each group of equal `taken` values:

~~~ {.sql}
SELECT taken, person, quant FROM Survey ORDER BY taken ASC, person DESC;
~~~
|taken|person|quant|
|-----|------|-----|
|619  |dyer  |rad  |
|619  |dyer  |sal  |
|622  |dyer  |rad  |
|622  |dyer  |sal  |
|734  |pb    |rad  |
|734  |pb    |temp |
|734  |lake  |sal  |
|735  |pb    |rad  |
|735  |-null-|sal  |
|735  |-null-|temp |
|751  |pb    |rad  |
|751  |pb    |temp |
|751  |lake  |sal  |
|752  |roe   |sal  |
|752  |lake  |rad  |
|752  |lake  |sal  |
|752  |lake  |temp |
|837  |roe   |sal  |
|837  |lake  |rad  |
|837  |lake  |sal  |
|844  |roe   |rad  |

The field `person` is an identifier for the scientist who took each measurement, so this query gives us a good idea of which scientist was at which site 
and what measurements they performed while they were there.

## Combining clauses

Looking at the table, 
it seems like some scientists specialized in certain kinds of measurements. 
We can examine which scientists performed which measurements by selecting the appropriate columns and removing duplicates. 

~~~ {.sql}
SELECT DISTINCT quant, person FROM Survey ORDER BY quant ASC;
~~~

|quant|person|
|-----|------|
|rad  |dyer  |
|rad  |pb    |
|rad  |lake  |
|rad  |roe   |
|sal  |dyer  |
|sal  |lake  |
|sal  |-null-|
|sal  |roe   |
|temp |pb    |
|temp |-null-|
|temp |lake  |

> ## Finding Distinct Dates {.challenge}
>
> Write a query that selects unique dates from the `Visited` table and sorts them in ascending order.

> ## Displaying Full Names {.challenge}
>
> Write a query that displays the full names of the scientists in the `Person` table,
> ordered by family name.
