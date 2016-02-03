---
layout: page
title: Programming with Python
---
The best way to learn how to program is to do something useful,
so this introduction to Python is built around a common scientific task:
data analysis.

Our real goal isn't to teach you Python,
but to teach you the basic concepts that all good scientific programming depends on.
We use Python in our lessons because:

1.  we have to use *something* for examples;
2.  it's free, well-documented, and runs almost everywhere;
3.  it has a large (and growing) user base among scientists; and
4.  experience shows that it's easier for novices to learn than most other languages.

We are using NCDC climate records and USGS streamgage data as examples in this set of lessons. Some of these data sets are stored in [comma-separated values](reference.html#comma-separated-values) (CSV) format:
each row holds information for a given time period,
and the columns represent different measurements. These files are messy - they have headers and time formats that we have to deal before using the data. In later lessons, we will also write scripts that directly request data files from servers through the USGS web services. Automatically accessing data with scripts using APIs is one way to further automate our workflow.

> ## Prerequisites {.prereq}
>
> Learners need to understand the concepts of files and directories
> (including the working directory) and how to start a Python
> interpreter before tackling this lesson. This lesson references the Jupyter (IPython)
> Notebook, although it can be taught through any Python interpreter.
> The commands in this lesson pertain to **Python 2.7**.

> ## Getting ready {.getready}
>
> You need to download some files to follow this lesson:
>
> 1. Make a new folder in your Desktop called `python-lessons`.
> 2. Download [python-novice-geo.zip](./python-novice-geo.zip) and move the file to this folder.
> 3. If it's not unzipped yet, double-click on it to unzip it. You should end up with a new folder called `data`.
> 4. Access this folder from the Unix shell with:
>
> ~~~ {.input}
> $ cd && cd Desktop/python-novice-geo/data
> ~~~

## Topics

1.  [Analyzing Temperature Data](01-numpy.html)
2.  [Storing Multiple Values in Lists](02-lists.html)
2.  [Repeating Actions with Loops](03-loops.html)
4.  [Using Pandas with tabular data](04-pandas.html)
5.  [Automating with Web Services](05-WebServices.html)
6.  [Creating Functions](06-functions.html)
7.  [Errors and Exceptions](07-errors.html)
8.  [Defensive Programming](08-defensive.html)
9.  [Debugging](09-debugging.html)
10.  [Command-Line Programs](10-cmdline.html)


## Other Resources

*   [Reference](reference.html)
*   [Discussion](discussion.html)
*   [Instructor's Guide](instructors.html)
