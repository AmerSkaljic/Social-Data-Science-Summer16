---
layout: post
title:  "Topic 1: Just Enough To Be DangeRous"
categories: posts
---

> There are only two kinds of languages: the ones people complain about and the ones nobody uses 
> [Bjarne Stroustrup](http://www.stroustrup.com/)

By now, you should have R and RStudio up and running. If this is not the case, please read and follow the instruction in the [getting started](/sds/posts/2015/08/31/getting-started.html) post again. 

There is a long and pretty interesting history behind R, but we won't have time to dive too much into that in this course. R builds on an earlier program called S, developed by John Chambers at the Bell Telephone Laboratories, originally part of AT&T Corp. R was written by Ross Ihaka and Robert Gentleman in the Department of Statistics at the University of Auckland. Two things about R are important to know as they are in large part the reasons behind R's popularity

1. R is *free* - R is completely open source.[^gnunote] That means that you are free to download, use, modify, redistribute, and basically do whatever you want with the program. For us, this is important because it means that you do not need an expensive university license to use R.
2. R has a philosophy that encourages participation: the philosophy behind R is perhaps best described by [John Chambers](), the inventor of R's predecessor S: 

> We wanted users to be able to begin in an interactive environment, where they did not consciously think of themselves as programming. Then as their needs became clearer and their sophistication increased, they should be able to slide gradually into programming, when the language and system aspects would become more important.

The point here is the transition from *user* to *developer*. When you start out you are simply using what other people have contributed. But at some point you realize that if no one feels like implementing your favorite method, well, then it’s *your* job to implement it. This is how R grows, and it is a cornerstone of open source software. 

[^gnunote]: To be precise, R is published under the [GNU General Public License, version 2](http://www.gnu.org/licenses/gpl-2.0.html). You can visit the [Free Software Foundation ](http://www.fsf.org/)’s web site if you want learn a lot more about free software.  

These days, R is widely used in academia as well as in major tech companies such as Twitter, Google, Facebook. Even the [New York Times](http://chartsnthings.tumblr.com/post/47670081904/climate-change-crowbars-and-strikeouts) use R's data munging and graphing capabilities to prepare their spectacular data visualizations. 

## R basics

Before we begin with some basic operations in R we need to address two important topics

- getting help: learning how to get help when you're stuck is one of the most important skills to learn in this course. There is a great guide for getting help in R [here](http://pols503.github.io/pols_503_sp15/getting_help_with_r.html).
- installing packages: when you download R it can't do all that much. In order to maximize R's functionality we need to add additional bundles of code. In R, these bundles of code are called packages. A package bundles together code, data, documentation, and tests. Once a package is installed you load it into R with the `library("x")` command (where "x" is the name of the package you want to load). In this course we will intall packages from two sources
  - from [CRAN](https://cran.r-project.org/), the Comprehensive R Archive Network. As of January 2015, there were over 6,000 packages available on CRAN. You can download a package from CRAN by writing `install.packages("x")`, where `x` is the name of the package you want to install. 
  - from [github](https://github.com/). Many new packages are distributed via github instead of CRAN. Because things are never easy, we need a package from CRAN before we can install packages from github. The first thing to do is therefore to run `install.packages("devtools")`. After this, packages can be installed from github with the `devtools::install_github` command. More about this later. 
   
Below is a short introduction to some basics in R. However, I will not be able to cover everything you need to know in order to be just a little bit dangerous with R yet. Therefore, I want you to do the following before class

1. Open up RStudio and type along while reading the text below. Try to think about what we're doing instead of just zoning out and copying my code. 
2. Once you've done that, watch [this](https://www.youtube.com/watch?v=5AQM-yUX9zg&index=6&list=PLjTlxb-wKvXNSDfcKPFH2gzHGyjpeCZmJ) 30 minute video by Roger Peng from his fantastic Coursera course on R programming. I want you to pay special attention when Roger talks about data frames and lists. 

R works by assigning information into objects. There are two assignment operators, `<-` and `=`. Everyone recommends the first, but I am lazy and prefer the second. It doesn't matter which one you choose in this course. 

Let's make our first assignment and inspect the object that we create


{% highlight r %}
x = 5
x
{% endhighlight %}



{% highlight text %}
## [1] 5
{% endhighlight %}

You can read this as "take the number 5 and store it in the object x". `x` is now an object and it stores the number 5. 

Let's make another slightly more complicated object


{% highlight r %}
sebs_number = 5*2
{% endhighlight %}

We now have a new object, `sebs_number`. To inspect this, try out RStudio’s completion facility: type the first few characters, press TAB, add characters until you disambiguate, then press return.

We can try to inspect the object 


{% highlight r %}
sebsnumber
{% endhighlight %}



{% highlight text %}
## Error in eval(expr, envir, enclos): object 'sebsnumber' not found
{% endhighlight %}
Oops! R threw an error message. Take a moment and think about why this might be the case. 

The reason R threw an error message is because there is no object called `sebsnumber`. The correct name was `sebs_number`. This illustrates an important point: computers will do tedious computations for you. But at the same time, they are incredibly stupid and will need completely precise instructions. Therefore, typos matter. Case matters. We all need to get better at typing. 

Here are some examples of R objects

- character string (e.g. words)
- number
- vector
- matrix
- data frame 
- list 

Character strings store words instead of numbers. Let's create another object, `z`, and store some words in it


{% highlight r %}
z = "learning R is fun"
z
{% endhighlight %}



{% highlight text %}
## [1] "learning R is fun"
{% endhighlight %}

We can learn about the class of an object by running the `class` function 


{% highlight r %}
class(z)
{% endhighlight %}



{% highlight text %}
## [1] "character"
{% endhighlight %}

The `class` function informed us (as we already knew) that our object `z` is of class `character`. 

R also has some special values. A couple of examples that are relevant to us are 

- `NA`: not available, missing
- `NULL`: does not exist, is undefined
- `TRUE`: logical true
- `FALSE`: logical false

R has built in functions for finding special value. For example, the `is.na` function will return a logical value for whether an object is true or false


{% highlight r %}
absent = NA
present = "yes"
is.na(present)
{% endhighlight %}



{% highlight text %}
## [1] FALSE
{% endhighlight %}



{% highlight r %}
is.na(absent)
{% endhighlight %}



{% highlight text %}
## [1] TRUE
{% endhighlight %}

Above, we created two objects, `absent` and `present`. When running the `is.na` function on both we found out that `absent` had a missing value but `present` didn't. 

The most basic type of R object is a vector. There is really only one rule about vectors in R, which is that **a vector can only contain objects of the same class**.

Let's create a vector object named `my_numeric_vector` of numbers from 1 to 5


{% highlight r %}
my_numeric_vector = c(1, 2, 3, 4, 5)
{% endhighlight %}

Here we used the `c()` function to concatenate (or combine) the numbers together. 

Because vectors can only contain objects of the same class, R will automatically convert inputs of different classes to the same class. This is sometimes called *implicit conversion*. 

Let's create a vector with one character string and a number


{% highlight r %}
my_mixed_vector = c("hello", 10)
{% endhighlight %}

Again, we use the `class` function to ask R to provide us the class of the object


{% highlight r %}
class(my_mixed_vector)
{% endhighlight %}



{% highlight text %}
## [1] "character"
{% endhighlight %}

R has implicitly converted the number 10 to a character "10". This may not seem important right now, but it actually turns out to be. For instance, we can't do mathematical operations on characters.

If you were able to complete all the commands above in R you should continue by watching the Roger Peng video listed in the beginning of the post. If you were unable to generate the same output as I this is probably a great time to consult the tips for getting help introduced in the beginning. 


## References

This post uses some code and ideas from Jenny Bryant, [R basics, workspace and working directory, RStudio projects](http://www.stat.ubc.ca/~jenny/STAT545A/block01_basicsWorkspaceWorkingDirProject.html). The John Chambers quote is quoted from Roger Peng's [R Programming for Data Science](https://leanpub.com/rprogramming).  





