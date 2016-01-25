---
layout: page
title: Programming with Python
---
The best way to learn how to program is to do something useful,
so this introduction to Python is built around a common scientific task:
data analysis.

Our real goal isn't to teach you Python,
but to teach you the basic concepts that all programming depends on.
We teach Python in our lessons because:

1.  we have to use *something* for examples;
2.  it's free, well-documented, and runs almost everywhere;
3.  it has a large (and growing) user base among scientists; and
4.  experience shows that it's easier for novices to learn than most other languages.

When learning to program, the two most important things are
to use whatever language your colleagues are using
so that you can share your work with them easily,
and to use that language *well*.

We are using USGS streamgage data to monitor flooding at multiple sites across the US. The data sets are stored in [comma-separated values](reference.html#comma-separated-values) (CSV) format:
each row holds information for a given time period,
and the columns represent different measurements. These files are messy - they have headers and time formats that we have to deal before using the data.

Our tasks are:

*   read the files and clean up the headers,
*   load the data into memory,
*   identify the flood stage,
*   plot the gage records.

To do all that, we'll have to learn a little bit about programming.

> ## Prerequisites {.prereq}
>
> Learners need to understand the concepts of files and directories
> (including the working directory) and how to start a Python
> interpreter before tackling this lesson. This lesson references the Jupyter (IPython)
> Notebook although it can be taught through any Python interpreter.
> The commands in this lesson pertain to **Python 2.7**.

> ## Getting ready {.getready}
>
> You need to download some files to follow this lesson:
>
> 1. Make a new folder in your Desktop called `python-lessons`.
> 2. Download [python-novice-inflammation-data.zip](./python-novice-inflammation-data.zip) and move the file to this folder.
> 3. If it's not unzipped yet, double-click on it to unzip it. You should end up with a new folder called `data`.
> 4. You can access this folder from the Unix shell with:
>
> ~~~ {.input}
> $ cd && cd Desktop/python-novice-inflammation/data
> ~~~

## Topics

1.  [Analyzing Patient Data](01-numpy.html)
2.  [Repeating Actions with Loops](02-loop.html)
3.  [Storing Multiple Values in Lists](03-lists.html)
4.  [Analyzing Data from Multiple Files](04-files.html)
5.  [Making Choices](05-cond.html)
6.  [Creating Functions](06-func.html)
7.  [Errors and Exceptions](07-errors.html)
8.  [Defensive Programming](08-defensive.html)
9.  [Debugging](09-debugging.html)
10.  [Command-Line Programs](10-cmdline.html)


## Other Resources

*   [Reference](reference.html)
*   [Discussion](discussion.html)
*   [Instructor's Guide](instructors.html)