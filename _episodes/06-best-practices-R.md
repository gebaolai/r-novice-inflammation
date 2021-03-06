---
title: "Best Practices for Writing R"
teaching: 30
exercises: 0
questions:
- "How can I write R that other people can understand and use?"
objectives:
- "Define best formatting practices when writing code in R scripts."
- "Synthesize a consistent personal coding style to increase code readability, consistency, and repeatability."
- "Apply this style to one's own code."
keypoints:
- "Start each program with a description of what it does."
- "Then load all required packages."
- "Consider what working directory you are in when sourcing a script."
- "Use comments to mark off sections of code."
- "Put function definitions at the top of your file, or in a separate file if there are many."
- "Name and style code consistently."
- "Break code into small, discrete pieces."
- "Factor out common operations rather than repeating them."
- "Keep all of the source files for a project in one directory and use relative paths to access them."
- "Keep track of the memory used by your program."
- "Always start with a clean environment instead of saving session history."
- "Keep track of session information in your project folder."
- "Have someone else review your code."
- "Use version control."
---



1. Start your code with an annotated description of what the code does when it is run:


~~~
#This is code to replicate the analyses and figures from my 2014 Science paper.
#Code developed by Sarah Supp, Tracy Teal, and Jon Borelli
~~~
{: .r}

2. Next, load all of the packages that will be necessary to run your code (using `library`):


~~~
library(ggplot2)
library(reshape)
library(vegan)
~~~
{: .r}

3. Set your working directory before `source()`ing a script, or start `R` inside your project folder:

One should exercise caution when using `setwd()`. Changing directories in a script file can limit reproducibility:

* `setwd()` will return an error if the directory to which you're trying to change doesn't exit or if the user doesn't have the correct permissions to access that directory. This becomes a problem when sharing scripts between users who have organized their directories differently.
* If/when your script terminates with an error, you might leave the user in a different directory than the one they started in, and if they then call the script again, this will cause further problems. If you must use `setwd()`, it is best to put it at the top of the script to avoid these problems.

The following error message indicates that R has failed to set the working directory you specified:

```
Error in setwd("~/path/to/working/directory") : cannot change working directory
```

It is best practice to have the user running the script begin in a consistent directory on their machine and then use relative file paths from that directory to access files (see below).

4. Annotate and mark your code using `#` or `#-` to set off sections of your code and to make finding specific parts of your code easier.

5. If you create only one or a few custom functions in your script, put them toward the top of your code so they are among the first objects created. If you have written many functions, put them all in their own .R file and then `source` those files. `source` will define all of these functions so that your code can make use of them as needed. For the reasons listed above, try to avoid using `setwd()` (or other functions that have side-effects in the user's workspace) in scripts you `source`.


~~~
source("my_genius_fxns.R")
~~~
{: .r}

6. Use a consistent style within your code. For example, name all matrices something ending in `.mat`. Consistency makes code easier to read and problems easier to spot.

7. Keep your code in bite-sized chunks. If a single function or loop gets too long, consider looking for ways to break it into smaller pieces.

8. Don't repeat yourself--automate! If you are repeating the same code over and over, use a loop or a function to repeat that code for you. Needless repetition doesn't just waste time--it also increases the likelihood you'll make a costly mistake!

9. Keep all of your source files for a project in the same directory, then use relative paths as necessary to access them. For example, use


~~~
dat <- read.csv(file = "files/dataset-2013-01.csv", header = TRUE)
~~~
{: .r}

rather than:


~~~
dat <- read.csv(file = "/Users/Karthik/Documents/sannic-project/files/dataset-2013-01.csv", header = TRUE)
~~~
{: .r}

10. R can run into memory issues. It is a common problem to run out of memory after running R scripts for a long time. To inspect the objects in your current R  environment, you can list the objects, search current packages, and remove objects that are currently not in use. A good practice when running long lines of computationally intensive  code is to remove temporary objects after they have served their purpose. However, sometimes, R will not clean up unused memory for a while after you delete objects. You can force R to tidy up its memory by using `gc()`.


~~~
interim_object <- data.frame(rep(1:100,10),rep(101:200,10),rep(201:300,10)) # Sample dataset of 1000 rows
object.size(interim_object) # Reports the memory size allocated to the object
rm(interim_object) # Removes only the object itself and not necessarily the memory allotted to it
gc() # Force R to release memory it is no longer using
ls()  # Lists all the objects in your current workspace
rm(list = ls()) # If you want to delete all the objects in the workspace and start with a clean slate
~~~
{: .r}

11. Don't save a session history (the default option in R, when it asks if you want an `RData` file). Instead, start in a clean environment so that older objects don't remain in your environment any longer than they need to. If that happens, it can lead to unexpected results.

12. Wherever possible, keep track of `sessionInfo()` somewhere in your project folder. Session information is invaluable because it captures all of the packages used in the current project. If a newer version of a package changes the way a function behaves, you can always go back and reinstall the version that worked (Note: At least on CRAN, all older versions of packages are permanently archived).

13. Collaborate. Grab a buddy and practice "code review". Review is used for preparing experiments and manuscripts; why not use it for code as well? Our code is also a major scientific achievement and the product of lots of hard work!

14. Develop your code using version control and frequent updates!

> ## Best Practice
>
> 1. What other suggestions do you have for coding best practices?
> 2. What are some specific ways we could restructure the code we worked on today to make it easier for a new user to read? Discuss with your neighbor.
> 3. Make two new R scripts called `inflammation.R` and `inflammation_fxns.R`.
>    Copy and paste code into each script so that `inflammation.R` "does stuff" and `inflammation_fxns.R` holds all of your functions.
>    __Hint__: you will need to add `source` to one of the files.
{: .challenge}
