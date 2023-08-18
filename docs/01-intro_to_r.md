# Introduction to R {#intror}

<style>div.summary{background-color:rgba(30,144,255,0.1);border:3px double #0000FF;padding:25px}</style>

::: {.summary}

### Functions introduced in this chapter: {-}
`<-`, `c`, `sum`, `mean`, `library`, `?`, `??`, `View`, `head`, `tail`, `str`, `NROW`,  `NCOL`, `summary`, `$`

:::


## Introduction {#intror-intro}

Welcome to R! This chapter will walk you through everything you need to know to get started using R.

As you go through this chapter (and all future chapters), please read slowly and carefully, and pay attention to detail. Many steps depend on the correct execution of all previous steps, so reading quickly and casually might come back to bite you later.


## What is R? {#intror-whatisr}

R is a programming language specifically designed for doing statistics. Don't be intimidated by the word "programming" though. The goal of this course is not to make you a computer programmer. To use R to do statistics, you don't need know anything about programming at all. Every chapter throughout the whole course will give you examples of the commands you need to use. All you have to do is use those example commands as templates and make the necessary changes to adapt them to the data you're trying to analyze.

The greatest thing about R is that it is free and open source. This means that you can download it and use it for free, and also that you can inspect and modify the source code for all R functions. This kind of transparency does not exist in commercial software. The net result is a robust, secure, widely-used language with literally tens of thousands of contributions from R users all over the world.

R has also become a standard tool for statistical analysis, from academia to industry to government. Although some commercial packages are still widely used, many practitioners are switching to R due to its cost (free!) and relative ease of use. After this course, you will be able to list some R experience on your r&eacute;sum&eacute; and your future employer will value this. It might even help get you a job!


## RStudio {#intror-rstudio}

RStudio is an "Integrated Development Environment," or IDE for short. An IDE is a tool for working with a programming language that is fancier than just a simple text editor. Most IDEs give you shortcuts, menus, debugging facilities, syntax highlighting, and other things to make your life as easy as possible.

Open RStudio so we can explore some of the areas you'll be using in the future.

On the left side of your screen, you should see a big pane called the "Console". There will be some startup text there, and below that, you should see a "command prompt": the symbol ">" followed by a blinking cursor. (If the cursor is not blinking, that means that the focus is in another pane. Click anywhere in the Console and the cursor should start blinking again.)

A command prompt can be one of the more intimidating things about starting to use R. It's just sitting there waiting for you to do something. Unlike other programs where you run commands from menus, R requires you to know what you need to type to make it work.

We'll return to the Console in a moment.

Next, look at the upper-right corner of the screen. There are at least three tabs in this pane starting with "Environment", "History", and "Connections". The "Environment" (also called the "Global Environment") keeps track of things you define while working with R. There's nothing to see there yet because we haven't defined anything! The "History" tab will likewise be empty; again, we haven't done anything yet. We won't use the "Connections" tab in this course. (Depending on the version of RStudio you are using and its configuration, you may see additional tabs, but we won't need them for this course.)

Now look at the lower-right corner of the screen. There are likely five tabs here: "Files", "Plots", "Packages", "Help", and "Viewer". The "Files" tab will eventually contain the files you upload or create. "Plots" will show you the result of commands that produce graphs and charts. "Packages" will be explained later. "Help" is precisely what it sounds like; this will be a very useful place for you to get to know. We will never use the "Viewer" tab, so don't worry about it.


## Try something! {#intror-trysomething}

So let's do something in R! Go back to the Console and at the command prompt (the ">" symbol with the blinking cursor), type


```r
1+1
```

and hit Enter.

Congratulations! You just ran your first command in R. It's all downhill from here. R really is nothing more than a glorified calculator.

Okay, let's do something slightly more sophisticated. It's important to note that R is case-sensitive, which means that lowercase letters and uppercase letters are treated differently. Type the following, making sure you use a lowercase `c`, and hit Enter:


```r
x <- c(1, 3, 4, 7, 9)
```

You have just created a "vector". When we use the letter `c` and enclose a list of things in parentheses, we tell R to "combine" those elements. So, a vector is just a collection of data. The little arrow `<-` says to take what's on the right and assign it to the symbol on the left. The vector `x` is now saved in memory. As long as you don't terminate your current R session, this vector is available to you.

Check out the "Environment" pane now. You should see the vector `x` that you just created, along with some information about it. Next to `x`, it says `num`, which means your vector has numerical data. Then it says `[1:5]` which indicates that there are five elements in the vector `x`.

At the command prompt in the Console, type


```r
x
```

and hit Enter. Yup, `x` is there. R knows what it is. You may be wondering about the `[1]` that appears at the beginning of the line. To see what that means, try typing this (and hit Enter---at some point here I'm going to stop reminding you to hit Enter after everything you type):


```r
y <- letters
```

R is clever, so the alphabet is built in under the name `letters`.

Type


```r
y
```

Now can you see what the `[1]` meant above? Assuming the letters spilled onto more than one line of the Console, you should see a number in brackets at the beginning of each line telling you the numerical position of the first entry in each new line.

Since we've done a few things, check out the "Global Environment" in the upper-right corner. You should see the two objects we've defined thus far, `x` and `y`. Now click on the "History" tab. Here you have all the commands you have run so far. This can be handy if you need to go back and re-run an earlier command, or if you want to modify an earlier command and it's easier to edit it slightly than type it all over again. To get an older command back into the Console, either double-click on it, or select it and click the "To Console" button at the top of the pane.

When we want to re-use an old command, it has usually not been that long since we last used it. In this case, there is an even more handy trick. Click in the Console so that the cursor is blinking at the blank command prompt. Now hit the up arrow on your keyboard. Do it again. Now hit the down arrow once or twice. This is a great way to access the most recently used commands from your command history.

Let's do something with `x`. Type


```r
sum(x)
```

I bet you figured out what just happened.

Now try


```r
mean(x)
```

What if we wanted to save the mean of those five numbers for use later? We can assign the result to another variable! Type the following and observe the effect in the Environment.


```r
m <- mean(x)
```

It makes no difference what letter or combination of letters we use to name our variables. For example,


```r
mean_x <- mean(x)
```

just saves the mean to a differently named variable. In general, variable names can be any combination of characters that are letters, numbers, underscore symbols (`_`), and dots (`.`). (In this course, we will prefer underscores over dots.) You cannot use spaces or any other special character in the names of variables.^[The official spec says that a valid variable name "consists of letters, numbers and the dot or underline characters and starts with a letter or the dot not followed by a number."] You should avoid variable names that are the same words as predefined R functions; for example, we should not type `mean <- mean(x)`.


## Load packages {#intror-loadpackages}

Packages are collections of commands, functions, and sometimes data that people all over the world write and maintain. These packages extend the capabilities of R and add useful tools. For example, we would like to use the `palmerpenguins` package because it includes an interesting data set on penguins.

If you have installed R and RStudio on your own machine instead of accessing RStudio Workbench through a browser, you'll need to type `install.packages("palmerpenguins")` if you've never used the `palmerpenguins` package before. If you are using RStudio Workbench through a browser, you may not be able to install packages because you may not have admin privileges. If you need a package that is not installed, contact the person who administers your server.

The data set is called `penguins`. Let's see what happens when we try to access this data set without loading the package that contains it. Try typing this:


```r
penguins
```

You should have received an error. That makes sense because R doesn't know anything about a data set called `penguins`.

Now---assuming you have the `palmerpenguins` package installed---type this at the command prompt:


```r
library(palmerpenguins)
```

It didn't look like anything happened. However, in the background, all the stuff in the `palmerpenguins` package became available to use.

Let's test that claim. Hit the up arrow twice and get back to where you see this at the Console (or you can manually re-type it, but that's no fun!):


```r
penguins
```

Now R knows about the `penguins` data, so the last command printed some of it to the Console.

Go look at the "Packages" tab in the pane in the lower-right corner of the screen. Scroll down a little until you get to the "P"s. You should be able to find the `palmerpenguins` package. You'll also notice a check mark by it, indicating that this package is loaded into your current R session.

You must use the `library` command in every new R session in which you want to use a package.^[If you have installed R and RStudio on your own machine instead of accessing RStudio Workbench through a browser, you'll want to know that `install.packages` only has to be run once, the first time you want to install a package. If you're using RStudio Workbench, you don't even need to type that because your server admin will have already done it for you.] If you terminate your R session, R forgets about the package. If you are ever in a situation where you are trying to use a command and you know you're typing it correctly, but you're still getting an error, check to see if the package containing that command has been loaded with `library`.  (Many R commands are "base R" commands, meaning they come with R and no special package is required to access them. The set of `letters` you used above is one such example.)


## Getting help {#intror-gettinghelp}

There are four important ways to get help with R. The first is the obvious "Help" tab in the lower-right pane on your screen. Click on that tab now. In the search bar at the right, type `penguins` and hit Enter. Take a few minutes to read the help file.

Help files are only as good as their authors. Fortunately, most package developers are conscientious enough to write decent help files. But don't be surprised if the help file doesn't quite tell you what you want to know. And for highly technical R functions, sometimes the help files are downright inscrutable. Try looking at the help file for the `grep` function. Can you honestly say you have any idea what this command does or how you might use it? Over time, as you become more knowledgeable about how R works, these help files get less mysterious.

The second way of getting help is from the Console. Go to the Console and type


```r
?letters
```

The question mark tells R you need help with the R command `letters`. This will bring up the help file in the same Help pane you were looking at before.

Sometimes, you don't know exactly what the name of the command is. For example, suppose we misremembered the name and thought it was `letter` instead of `letters`. Try typing this:


```r
?letter
```

You should have received an error because there is no command called `letter`. Try this instead:


```r
??letter
```

and scroll down a bit in the Help pane. Two question marks tell R not to be too picky about the spelling. This will bring up a whole bunch of possibilities in the Help pane, representing R's best guess as to what you might be searching for. (In this case, it's not easy to find. You'd have to know that the help file for `letters` appeared on a help page called `base::Constants`.)

The fourth way to get help---and often the most useful way---is to use your best friend Google. You don't want to just search for "R". (That's the downside of using a single letter of the alphabet for the name of a programming language.) However, if you type "R __________" where you fill in the blank with the topic of interest, Google usually does a pretty good job sending you to relevant pages. Within the first few hits, in fact, you'll often see an online copy of the same help file you see in R. Frequently, the next few hits lead to [StackOverflow](https://stackoverflow.com) where very knowledgeable people post very helpful responses to common questions.

Use Google to find out how to take the square root of a number in R. Test out your newly-discovered function on a few numbers to make sure it works.


## Understanding the data {#intror-understandingdata}

Let's go back to the penguins data contained in the `penguins` data set from the `palmerpenguins` package.

The first thing we do to understand a data set is to read the help file on it. (We've already done this for the `penguins` data.) Of course, this only works for data files that come with R or with a package that can be loaded into R. If you are using R to analyze your own data, presumably you don't need a help file. And if you're analyzing data from another source, you'll have to go to that source to find out about the data.

When you read the help file for `penguins`, you may have noticed that it described the "Format" as being "A tibble with 344 rows and 8 variables." What is a "tibble"?

The word "tibble" is an R-specific term that describes data organized in a specific way. A more common term is "data frame" (or sometimes "data table"). The idea is that in a data frame, the rows and the columns have very specific interpretations.

Each row of a data frame represents a single object or observation. So in the `penguins` data, each row represents a penguin. If you have survey data, each row will usually represent a single person. But an "object" can be anything about which we collect data. State-level data might have 50 rows and each row represents an entire state.

Each column of a data frame represents a *variable*, which is a property, attribute, or measurement made about the objects in the data. For example, the help file mentions that various pieces of information are recorded about each penguin, like species, bill length, flipper length, boy mass, sex, and so on. These are examples of variables. In a survey, for example, the variables will likely be the responses to individual questions.

We will use the terms tibble and data frame interchangeably in this course. They are not quite synonyms: tibbles are R-specific implementations of data frames, the latter being a more general term that applies in all statistical contexts. Nevertheless, there are no situations (at least not encountered in this course) where it makes any difference if a data set is called a tibble or a data frame.

We can also look at the data frame in "spreadsheet" form. Type


```r
View(penguins)
```

(Be sure you're using an upper-case "V" in `View`.) A new pane should open up in the upper-left corner of the screen. In that pane, the penguins data appears in a grid format, like a spreadsheet. The observations (individual penguins) are the rows and the variables (attributes and measurements about the penguins) are the columns. This will also let you sort each column by clicking on the arrows next to the variable name across the top.

Sometimes, we just need a little peek at the data. Try this to print just a few rows of data to the Console:


```r
head(penguins)
```

We can customize this by specifying the number of rows to print. (Don't forget about the up arrow trick!)


```r
head(penguins, n = 10)
```

The `tail` command does something similar.


```r
tail(penguins)
```

When we're working with HTML documents like this one, it's usually not necessary to use `View`, `head`, or `tail` because the HTML format will print the data frame a lot more neatly than it did in the Console. You do not need to type the following code; just look below it for the table that appears.


```
## Warning: package 'palmerpenguins' was built under R version 4.3.1
```


```r
penguins
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["species"],"name":[1],"type":["fct"],"align":["left"]},{"label":["island"],"name":[2],"type":["fct"],"align":["left"]},{"label":["bill_length_mm"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["bill_depth_mm"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["flipper_length_mm"],"name":[5],"type":["int"],"align":["right"]},{"label":["body_mass_g"],"name":[6],"type":["int"],"align":["right"]},{"label":["sex"],"name":[7],"type":["fct"],"align":["left"]},{"label":["year"],"name":[8],"type":["int"],"align":["right"]}],"data":[{"1":"Adelie","2":"Torgersen","3":"39.1","4":"18.7","5":"181","6":"3750","7":"male","8":"2007"},{"1":"Adelie","2":"Torgersen","3":"39.5","4":"17.4","5":"186","6":"3800","7":"female","8":"2007"},{"1":"Adelie","2":"Torgersen","3":"40.3","4":"18.0","5":"195","6":"3250","7":"female","8":"2007"},{"1":"Adelie","2":"Torgersen","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"2007"},{"1":"Adelie","2":"Torgersen","3":"36.7","4":"19.3","5":"193","6":"3450","7":"female","8":"2007"},{"1":"Adelie","2":"Torgersen","3":"39.3","4":"20.6","5":"190","6":"3650","7":"male","8":"2007"},{"1":"Adelie","2":"Torgersen","3":"38.9","4":"17.8","5":"181","6":"3625","7":"female","8":"2007"},{"1":"Adelie","2":"Torgersen","3":"39.2","4":"19.6","5":"195","6":"4675","7":"male","8":"2007"},{"1":"Adelie","2":"Torgersen","3":"34.1","4":"18.1","5":"193","6":"3475","7":"NA","8":"2007"},{"1":"Adelie","2":"Torgersen","3":"42.0","4":"20.2","5":"190","6":"4250","7":"NA","8":"2007"},{"1":"Adelie","2":"Torgersen","3":"37.8","4":"17.1","5":"186","6":"3300","7":"NA","8":"2007"},{"1":"Adelie","2":"Torgersen","3":"37.8","4":"17.3","5":"180","6":"3700","7":"NA","8":"2007"},{"1":"Adelie","2":"Torgersen","3":"41.1","4":"17.6","5":"182","6":"3200","7":"female","8":"2007"},{"1":"Adelie","2":"Torgersen","3":"38.6","4":"21.2","5":"191","6":"3800","7":"male","8":"2007"},{"1":"Adelie","2":"Torgersen","3":"34.6","4":"21.1","5":"198","6":"4400","7":"male","8":"2007"},{"1":"Adelie","2":"Torgersen","3":"36.6","4":"17.8","5":"185","6":"3700","7":"female","8":"2007"},{"1":"Adelie","2":"Torgersen","3":"38.7","4":"19.0","5":"195","6":"3450","7":"female","8":"2007"},{"1":"Adelie","2":"Torgersen","3":"42.5","4":"20.7","5":"197","6":"4500","7":"male","8":"2007"},{"1":"Adelie","2":"Torgersen","3":"34.4","4":"18.4","5":"184","6":"3325","7":"female","8":"2007"},{"1":"Adelie","2":"Torgersen","3":"46.0","4":"21.5","5":"194","6":"4200","7":"male","8":"2007"},{"1":"Adelie","2":"Biscoe","3":"37.8","4":"18.3","5":"174","6":"3400","7":"female","8":"2007"},{"1":"Adelie","2":"Biscoe","3":"37.7","4":"18.7","5":"180","6":"3600","7":"male","8":"2007"},{"1":"Adelie","2":"Biscoe","3":"35.9","4":"19.2","5":"189","6":"3800","7":"female","8":"2007"},{"1":"Adelie","2":"Biscoe","3":"38.2","4":"18.1","5":"185","6":"3950","7":"male","8":"2007"},{"1":"Adelie","2":"Biscoe","3":"38.8","4":"17.2","5":"180","6":"3800","7":"male","8":"2007"},{"1":"Adelie","2":"Biscoe","3":"35.3","4":"18.9","5":"187","6":"3800","7":"female","8":"2007"},{"1":"Adelie","2":"Biscoe","3":"40.6","4":"18.6","5":"183","6":"3550","7":"male","8":"2007"},{"1":"Adelie","2":"Biscoe","3":"40.5","4":"17.9","5":"187","6":"3200","7":"female","8":"2007"},{"1":"Adelie","2":"Biscoe","3":"37.9","4":"18.6","5":"172","6":"3150","7":"female","8":"2007"},{"1":"Adelie","2":"Biscoe","3":"40.5","4":"18.9","5":"180","6":"3950","7":"male","8":"2007"},{"1":"Adelie","2":"Dream","3":"39.5","4":"16.7","5":"178","6":"3250","7":"female","8":"2007"},{"1":"Adelie","2":"Dream","3":"37.2","4":"18.1","5":"178","6":"3900","7":"male","8":"2007"},{"1":"Adelie","2":"Dream","3":"39.5","4":"17.8","5":"188","6":"3300","7":"female","8":"2007"},{"1":"Adelie","2":"Dream","3":"40.9","4":"18.9","5":"184","6":"3900","7":"male","8":"2007"},{"1":"Adelie","2":"Dream","3":"36.4","4":"17.0","5":"195","6":"3325","7":"female","8":"2007"},{"1":"Adelie","2":"Dream","3":"39.2","4":"21.1","5":"196","6":"4150","7":"male","8":"2007"},{"1":"Adelie","2":"Dream","3":"38.8","4":"20.0","5":"190","6":"3950","7":"male","8":"2007"},{"1":"Adelie","2":"Dream","3":"42.2","4":"18.5","5":"180","6":"3550","7":"female","8":"2007"},{"1":"Adelie","2":"Dream","3":"37.6","4":"19.3","5":"181","6":"3300","7":"female","8":"2007"},{"1":"Adelie","2":"Dream","3":"39.8","4":"19.1","5":"184","6":"4650","7":"male","8":"2007"},{"1":"Adelie","2":"Dream","3":"36.5","4":"18.0","5":"182","6":"3150","7":"female","8":"2007"},{"1":"Adelie","2":"Dream","3":"40.8","4":"18.4","5":"195","6":"3900","7":"male","8":"2007"},{"1":"Adelie","2":"Dream","3":"36.0","4":"18.5","5":"186","6":"3100","7":"female","8":"2007"},{"1":"Adelie","2":"Dream","3":"44.1","4":"19.7","5":"196","6":"4400","7":"male","8":"2007"},{"1":"Adelie","2":"Dream","3":"37.0","4":"16.9","5":"185","6":"3000","7":"female","8":"2007"},{"1":"Adelie","2":"Dream","3":"39.6","4":"18.8","5":"190","6":"4600","7":"male","8":"2007"},{"1":"Adelie","2":"Dream","3":"41.1","4":"19.0","5":"182","6":"3425","7":"male","8":"2007"},{"1":"Adelie","2":"Dream","3":"37.5","4":"18.9","5":"179","6":"2975","7":"NA","8":"2007"},{"1":"Adelie","2":"Dream","3":"36.0","4":"17.9","5":"190","6":"3450","7":"female","8":"2007"},{"1":"Adelie","2":"Dream","3":"42.3","4":"21.2","5":"191","6":"4150","7":"male","8":"2007"},{"1":"Adelie","2":"Biscoe","3":"39.6","4":"17.7","5":"186","6":"3500","7":"female","8":"2008"},{"1":"Adelie","2":"Biscoe","3":"40.1","4":"18.9","5":"188","6":"4300","7":"male","8":"2008"},{"1":"Adelie","2":"Biscoe","3":"35.0","4":"17.9","5":"190","6":"3450","7":"female","8":"2008"},{"1":"Adelie","2":"Biscoe","3":"42.0","4":"19.5","5":"200","6":"4050","7":"male","8":"2008"},{"1":"Adelie","2":"Biscoe","3":"34.5","4":"18.1","5":"187","6":"2900","7":"female","8":"2008"},{"1":"Adelie","2":"Biscoe","3":"41.4","4":"18.6","5":"191","6":"3700","7":"male","8":"2008"},{"1":"Adelie","2":"Biscoe","3":"39.0","4":"17.5","5":"186","6":"3550","7":"female","8":"2008"},{"1":"Adelie","2":"Biscoe","3":"40.6","4":"18.8","5":"193","6":"3800","7":"male","8":"2008"},{"1":"Adelie","2":"Biscoe","3":"36.5","4":"16.6","5":"181","6":"2850","7":"female","8":"2008"},{"1":"Adelie","2":"Biscoe","3":"37.6","4":"19.1","5":"194","6":"3750","7":"male","8":"2008"},{"1":"Adelie","2":"Biscoe","3":"35.7","4":"16.9","5":"185","6":"3150","7":"female","8":"2008"},{"1":"Adelie","2":"Biscoe","3":"41.3","4":"21.1","5":"195","6":"4400","7":"male","8":"2008"},{"1":"Adelie","2":"Biscoe","3":"37.6","4":"17.0","5":"185","6":"3600","7":"female","8":"2008"},{"1":"Adelie","2":"Biscoe","3":"41.1","4":"18.2","5":"192","6":"4050","7":"male","8":"2008"},{"1":"Adelie","2":"Biscoe","3":"36.4","4":"17.1","5":"184","6":"2850","7":"female","8":"2008"},{"1":"Adelie","2":"Biscoe","3":"41.6","4":"18.0","5":"192","6":"3950","7":"male","8":"2008"},{"1":"Adelie","2":"Biscoe","3":"35.5","4":"16.2","5":"195","6":"3350","7":"female","8":"2008"},{"1":"Adelie","2":"Biscoe","3":"41.1","4":"19.1","5":"188","6":"4100","7":"male","8":"2008"},{"1":"Adelie","2":"Torgersen","3":"35.9","4":"16.6","5":"190","6":"3050","7":"female","8":"2008"},{"1":"Adelie","2":"Torgersen","3":"41.8","4":"19.4","5":"198","6":"4450","7":"male","8":"2008"},{"1":"Adelie","2":"Torgersen","3":"33.5","4":"19.0","5":"190","6":"3600","7":"female","8":"2008"},{"1":"Adelie","2":"Torgersen","3":"39.7","4":"18.4","5":"190","6":"3900","7":"male","8":"2008"},{"1":"Adelie","2":"Torgersen","3":"39.6","4":"17.2","5":"196","6":"3550","7":"female","8":"2008"},{"1":"Adelie","2":"Torgersen","3":"45.8","4":"18.9","5":"197","6":"4150","7":"male","8":"2008"},{"1":"Adelie","2":"Torgersen","3":"35.5","4":"17.5","5":"190","6":"3700","7":"female","8":"2008"},{"1":"Adelie","2":"Torgersen","3":"42.8","4":"18.5","5":"195","6":"4250","7":"male","8":"2008"},{"1":"Adelie","2":"Torgersen","3":"40.9","4":"16.8","5":"191","6":"3700","7":"female","8":"2008"},{"1":"Adelie","2":"Torgersen","3":"37.2","4":"19.4","5":"184","6":"3900","7":"male","8":"2008"},{"1":"Adelie","2":"Torgersen","3":"36.2","4":"16.1","5":"187","6":"3550","7":"female","8":"2008"},{"1":"Adelie","2":"Torgersen","3":"42.1","4":"19.1","5":"195","6":"4000","7":"male","8":"2008"},{"1":"Adelie","2":"Torgersen","3":"34.6","4":"17.2","5":"189","6":"3200","7":"female","8":"2008"},{"1":"Adelie","2":"Torgersen","3":"42.9","4":"17.6","5":"196","6":"4700","7":"male","8":"2008"},{"1":"Adelie","2":"Torgersen","3":"36.7","4":"18.8","5":"187","6":"3800","7":"female","8":"2008"},{"1":"Adelie","2":"Torgersen","3":"35.1","4":"19.4","5":"193","6":"4200","7":"male","8":"2008"},{"1":"Adelie","2":"Dream","3":"37.3","4":"17.8","5":"191","6":"3350","7":"female","8":"2008"},{"1":"Adelie","2":"Dream","3":"41.3","4":"20.3","5":"194","6":"3550","7":"male","8":"2008"},{"1":"Adelie","2":"Dream","3":"36.3","4":"19.5","5":"190","6":"3800","7":"male","8":"2008"},{"1":"Adelie","2":"Dream","3":"36.9","4":"18.6","5":"189","6":"3500","7":"female","8":"2008"},{"1":"Adelie","2":"Dream","3":"38.3","4":"19.2","5":"189","6":"3950","7":"male","8":"2008"},{"1":"Adelie","2":"Dream","3":"38.9","4":"18.8","5":"190","6":"3600","7":"female","8":"2008"},{"1":"Adelie","2":"Dream","3":"35.7","4":"18.0","5":"202","6":"3550","7":"female","8":"2008"},{"1":"Adelie","2":"Dream","3":"41.1","4":"18.1","5":"205","6":"4300","7":"male","8":"2008"},{"1":"Adelie","2":"Dream","3":"34.0","4":"17.1","5":"185","6":"3400","7":"female","8":"2008"},{"1":"Adelie","2":"Dream","3":"39.6","4":"18.1","5":"186","6":"4450","7":"male","8":"2008"},{"1":"Adelie","2":"Dream","3":"36.2","4":"17.3","5":"187","6":"3300","7":"female","8":"2008"},{"1":"Adelie","2":"Dream","3":"40.8","4":"18.9","5":"208","6":"4300","7":"male","8":"2008"},{"1":"Adelie","2":"Dream","3":"38.1","4":"18.6","5":"190","6":"3700","7":"female","8":"2008"},{"1":"Adelie","2":"Dream","3":"40.3","4":"18.5","5":"196","6":"4350","7":"male","8":"2008"},{"1":"Adelie","2":"Dream","3":"33.1","4":"16.1","5":"178","6":"2900","7":"female","8":"2008"},{"1":"Adelie","2":"Dream","3":"43.2","4":"18.5","5":"192","6":"4100","7":"male","8":"2008"},{"1":"Adelie","2":"Biscoe","3":"35.0","4":"17.9","5":"192","6":"3725","7":"female","8":"2009"},{"1":"Adelie","2":"Biscoe","3":"41.0","4":"20.0","5":"203","6":"4725","7":"male","8":"2009"},{"1":"Adelie","2":"Biscoe","3":"37.7","4":"16.0","5":"183","6":"3075","7":"female","8":"2009"},{"1":"Adelie","2":"Biscoe","3":"37.8","4":"20.0","5":"190","6":"4250","7":"male","8":"2009"},{"1":"Adelie","2":"Biscoe","3":"37.9","4":"18.6","5":"193","6":"2925","7":"female","8":"2009"},{"1":"Adelie","2":"Biscoe","3":"39.7","4":"18.9","5":"184","6":"3550","7":"male","8":"2009"},{"1":"Adelie","2":"Biscoe","3":"38.6","4":"17.2","5":"199","6":"3750","7":"female","8":"2009"},{"1":"Adelie","2":"Biscoe","3":"38.2","4":"20.0","5":"190","6":"3900","7":"male","8":"2009"},{"1":"Adelie","2":"Biscoe","3":"38.1","4":"17.0","5":"181","6":"3175","7":"female","8":"2009"},{"1":"Adelie","2":"Biscoe","3":"43.2","4":"19.0","5":"197","6":"4775","7":"male","8":"2009"},{"1":"Adelie","2":"Biscoe","3":"38.1","4":"16.5","5":"198","6":"3825","7":"female","8":"2009"},{"1":"Adelie","2":"Biscoe","3":"45.6","4":"20.3","5":"191","6":"4600","7":"male","8":"2009"},{"1":"Adelie","2":"Biscoe","3":"39.7","4":"17.7","5":"193","6":"3200","7":"female","8":"2009"},{"1":"Adelie","2":"Biscoe","3":"42.2","4":"19.5","5":"197","6":"4275","7":"male","8":"2009"},{"1":"Adelie","2":"Biscoe","3":"39.6","4":"20.7","5":"191","6":"3900","7":"female","8":"2009"},{"1":"Adelie","2":"Biscoe","3":"42.7","4":"18.3","5":"196","6":"4075","7":"male","8":"2009"},{"1":"Adelie","2":"Torgersen","3":"38.6","4":"17.0","5":"188","6":"2900","7":"female","8":"2009"},{"1":"Adelie","2":"Torgersen","3":"37.3","4":"20.5","5":"199","6":"3775","7":"male","8":"2009"},{"1":"Adelie","2":"Torgersen","3":"35.7","4":"17.0","5":"189","6":"3350","7":"female","8":"2009"},{"1":"Adelie","2":"Torgersen","3":"41.1","4":"18.6","5":"189","6":"3325","7":"male","8":"2009"},{"1":"Adelie","2":"Torgersen","3":"36.2","4":"17.2","5":"187","6":"3150","7":"female","8":"2009"},{"1":"Adelie","2":"Torgersen","3":"37.7","4":"19.8","5":"198","6":"3500","7":"male","8":"2009"},{"1":"Adelie","2":"Torgersen","3":"40.2","4":"17.0","5":"176","6":"3450","7":"female","8":"2009"},{"1":"Adelie","2":"Torgersen","3":"41.4","4":"18.5","5":"202","6":"3875","7":"male","8":"2009"},{"1":"Adelie","2":"Torgersen","3":"35.2","4":"15.9","5":"186","6":"3050","7":"female","8":"2009"},{"1":"Adelie","2":"Torgersen","3":"40.6","4":"19.0","5":"199","6":"4000","7":"male","8":"2009"},{"1":"Adelie","2":"Torgersen","3":"38.8","4":"17.6","5":"191","6":"3275","7":"female","8":"2009"},{"1":"Adelie","2":"Torgersen","3":"41.5","4":"18.3","5":"195","6":"4300","7":"male","8":"2009"},{"1":"Adelie","2":"Torgersen","3":"39.0","4":"17.1","5":"191","6":"3050","7":"female","8":"2009"},{"1":"Adelie","2":"Torgersen","3":"44.1","4":"18.0","5":"210","6":"4000","7":"male","8":"2009"},{"1":"Adelie","2":"Torgersen","3":"38.5","4":"17.9","5":"190","6":"3325","7":"female","8":"2009"},{"1":"Adelie","2":"Torgersen","3":"43.1","4":"19.2","5":"197","6":"3500","7":"male","8":"2009"},{"1":"Adelie","2":"Dream","3":"36.8","4":"18.5","5":"193","6":"3500","7":"female","8":"2009"},{"1":"Adelie","2":"Dream","3":"37.5","4":"18.5","5":"199","6":"4475","7":"male","8":"2009"},{"1":"Adelie","2":"Dream","3":"38.1","4":"17.6","5":"187","6":"3425","7":"female","8":"2009"},{"1":"Adelie","2":"Dream","3":"41.1","4":"17.5","5":"190","6":"3900","7":"male","8":"2009"},{"1":"Adelie","2":"Dream","3":"35.6","4":"17.5","5":"191","6":"3175","7":"female","8":"2009"},{"1":"Adelie","2":"Dream","3":"40.2","4":"20.1","5":"200","6":"3975","7":"male","8":"2009"},{"1":"Adelie","2":"Dream","3":"37.0","4":"16.5","5":"185","6":"3400","7":"female","8":"2009"},{"1":"Adelie","2":"Dream","3":"39.7","4":"17.9","5":"193","6":"4250","7":"male","8":"2009"},{"1":"Adelie","2":"Dream","3":"40.2","4":"17.1","5":"193","6":"3400","7":"female","8":"2009"},{"1":"Adelie","2":"Dream","3":"40.6","4":"17.2","5":"187","6":"3475","7":"male","8":"2009"},{"1":"Adelie","2":"Dream","3":"32.1","4":"15.5","5":"188","6":"3050","7":"female","8":"2009"},{"1":"Adelie","2":"Dream","3":"40.7","4":"17.0","5":"190","6":"3725","7":"male","8":"2009"},{"1":"Adelie","2":"Dream","3":"37.3","4":"16.8","5":"192","6":"3000","7":"female","8":"2009"},{"1":"Adelie","2":"Dream","3":"39.0","4":"18.7","5":"185","6":"3650","7":"male","8":"2009"},{"1":"Adelie","2":"Dream","3":"39.2","4":"18.6","5":"190","6":"4250","7":"male","8":"2009"},{"1":"Adelie","2":"Dream","3":"36.6","4":"18.4","5":"184","6":"3475","7":"female","8":"2009"},{"1":"Adelie","2":"Dream","3":"36.0","4":"17.8","5":"195","6":"3450","7":"female","8":"2009"},{"1":"Adelie","2":"Dream","3":"37.8","4":"18.1","5":"193","6":"3750","7":"male","8":"2009"},{"1":"Adelie","2":"Dream","3":"36.0","4":"17.1","5":"187","6":"3700","7":"female","8":"2009"},{"1":"Adelie","2":"Dream","3":"41.5","4":"18.5","5":"201","6":"4000","7":"male","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"46.1","4":"13.2","5":"211","6":"4500","7":"female","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"50.0","4":"16.3","5":"230","6":"5700","7":"male","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"48.7","4":"14.1","5":"210","6":"4450","7":"female","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"50.0","4":"15.2","5":"218","6":"5700","7":"male","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"47.6","4":"14.5","5":"215","6":"5400","7":"male","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"46.5","4":"13.5","5":"210","6":"4550","7":"female","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"45.4","4":"14.6","5":"211","6":"4800","7":"female","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"46.7","4":"15.3","5":"219","6":"5200","7":"male","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"43.3","4":"13.4","5":"209","6":"4400","7":"female","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"46.8","4":"15.4","5":"215","6":"5150","7":"male","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"40.9","4":"13.7","5":"214","6":"4650","7":"female","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"49.0","4":"16.1","5":"216","6":"5550","7":"male","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"45.5","4":"13.7","5":"214","6":"4650","7":"female","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"48.4","4":"14.6","5":"213","6":"5850","7":"male","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"45.8","4":"14.6","5":"210","6":"4200","7":"female","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"49.3","4":"15.7","5":"217","6":"5850","7":"male","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"42.0","4":"13.5","5":"210","6":"4150","7":"female","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"49.2","4":"15.2","5":"221","6":"6300","7":"male","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"46.2","4":"14.5","5":"209","6":"4800","7":"female","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"48.7","4":"15.1","5":"222","6":"5350","7":"male","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"50.2","4":"14.3","5":"218","6":"5700","7":"male","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"45.1","4":"14.5","5":"215","6":"5000","7":"female","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"46.5","4":"14.5","5":"213","6":"4400","7":"female","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"46.3","4":"15.8","5":"215","6":"5050","7":"male","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"42.9","4":"13.1","5":"215","6":"5000","7":"female","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"46.1","4":"15.1","5":"215","6":"5100","7":"male","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"44.5","4":"14.3","5":"216","6":"4100","7":"NA","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"47.8","4":"15.0","5":"215","6":"5650","7":"male","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"48.2","4":"14.3","5":"210","6":"4600","7":"female","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"50.0","4":"15.3","5":"220","6":"5550","7":"male","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"47.3","4":"15.3","5":"222","6":"5250","7":"male","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"42.8","4":"14.2","5":"209","6":"4700","7":"female","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"45.1","4":"14.5","5":"207","6":"5050","7":"female","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"59.6","4":"17.0","5":"230","6":"6050","7":"male","8":"2007"},{"1":"Gentoo","2":"Biscoe","3":"49.1","4":"14.8","5":"220","6":"5150","7":"female","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"48.4","4":"16.3","5":"220","6":"5400","7":"male","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"42.6","4":"13.7","5":"213","6":"4950","7":"female","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"44.4","4":"17.3","5":"219","6":"5250","7":"male","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"44.0","4":"13.6","5":"208","6":"4350","7":"female","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"48.7","4":"15.7","5":"208","6":"5350","7":"male","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"42.7","4":"13.7","5":"208","6":"3950","7":"female","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"49.6","4":"16.0","5":"225","6":"5700","7":"male","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"45.3","4":"13.7","5":"210","6":"4300","7":"female","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"49.6","4":"15.0","5":"216","6":"4750","7":"male","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"50.5","4":"15.9","5":"222","6":"5550","7":"male","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"43.6","4":"13.9","5":"217","6":"4900","7":"female","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"45.5","4":"13.9","5":"210","6":"4200","7":"female","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"50.5","4":"15.9","5":"225","6":"5400","7":"male","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"44.9","4":"13.3","5":"213","6":"5100","7":"female","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"45.2","4":"15.8","5":"215","6":"5300","7":"male","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"46.6","4":"14.2","5":"210","6":"4850","7":"female","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"48.5","4":"14.1","5":"220","6":"5300","7":"male","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"45.1","4":"14.4","5":"210","6":"4400","7":"female","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"50.1","4":"15.0","5":"225","6":"5000","7":"male","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"46.5","4":"14.4","5":"217","6":"4900","7":"female","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"45.0","4":"15.4","5":"220","6":"5050","7":"male","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"43.8","4":"13.9","5":"208","6":"4300","7":"female","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"45.5","4":"15.0","5":"220","6":"5000","7":"male","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"43.2","4":"14.5","5":"208","6":"4450","7":"female","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"50.4","4":"15.3","5":"224","6":"5550","7":"male","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"45.3","4":"13.8","5":"208","6":"4200","7":"female","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"46.2","4":"14.9","5":"221","6":"5300","7":"male","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"45.7","4":"13.9","5":"214","6":"4400","7":"female","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"54.3","4":"15.7","5":"231","6":"5650","7":"male","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"45.8","4":"14.2","5":"219","6":"4700","7":"female","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"49.8","4":"16.8","5":"230","6":"5700","7":"male","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"46.2","4":"14.4","5":"214","6":"4650","7":"NA","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"49.5","4":"16.2","5":"229","6":"5800","7":"male","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"43.5","4":"14.2","5":"220","6":"4700","7":"female","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"50.7","4":"15.0","5":"223","6":"5550","7":"male","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"47.7","4":"15.0","5":"216","6":"4750","7":"female","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"46.4","4":"15.6","5":"221","6":"5000","7":"male","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"48.2","4":"15.6","5":"221","6":"5100","7":"male","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"46.5","4":"14.8","5":"217","6":"5200","7":"female","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"46.4","4":"15.0","5":"216","6":"4700","7":"female","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"48.6","4":"16.0","5":"230","6":"5800","7":"male","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"47.5","4":"14.2","5":"209","6":"4600","7":"female","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"51.1","4":"16.3","5":"220","6":"6000","7":"male","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"45.2","4":"13.8","5":"215","6":"4750","7":"female","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"45.2","4":"16.4","5":"223","6":"5950","7":"male","8":"2008"},{"1":"Gentoo","2":"Biscoe","3":"49.1","4":"14.5","5":"212","6":"4625","7":"female","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"52.5","4":"15.6","5":"221","6":"5450","7":"male","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"47.4","4":"14.6","5":"212","6":"4725","7":"female","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"50.0","4":"15.9","5":"224","6":"5350","7":"male","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"44.9","4":"13.8","5":"212","6":"4750","7":"female","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"50.8","4":"17.3","5":"228","6":"5600","7":"male","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"43.4","4":"14.4","5":"218","6":"4600","7":"female","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"51.3","4":"14.2","5":"218","6":"5300","7":"male","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"47.5","4":"14.0","5":"212","6":"4875","7":"female","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"52.1","4":"17.0","5":"230","6":"5550","7":"male","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"47.5","4":"15.0","5":"218","6":"4950","7":"female","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"52.2","4":"17.1","5":"228","6":"5400","7":"male","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"45.5","4":"14.5","5":"212","6":"4750","7":"female","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"49.5","4":"16.1","5":"224","6":"5650","7":"male","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"44.5","4":"14.7","5":"214","6":"4850","7":"female","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"50.8","4":"15.7","5":"226","6":"5200","7":"male","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"49.4","4":"15.8","5":"216","6":"4925","7":"male","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"46.9","4":"14.6","5":"222","6":"4875","7":"female","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"48.4","4":"14.4","5":"203","6":"4625","7":"female","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"51.1","4":"16.5","5":"225","6":"5250","7":"male","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"48.5","4":"15.0","5":"219","6":"4850","7":"female","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"55.9","4":"17.0","5":"228","6":"5600","7":"male","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"47.2","4":"15.5","5":"215","6":"4975","7":"female","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"49.1","4":"15.0","5":"228","6":"5500","7":"male","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"47.3","4":"13.8","5":"216","6":"4725","7":"NA","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"46.8","4":"16.1","5":"215","6":"5500","7":"male","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"41.7","4":"14.7","5":"210","6":"4700","7":"female","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"53.4","4":"15.8","5":"219","6":"5500","7":"male","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"43.3","4":"14.0","5":"208","6":"4575","7":"female","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"48.1","4":"15.1","5":"209","6":"5500","7":"male","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"50.5","4":"15.2","5":"216","6":"5000","7":"female","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"49.8","4":"15.9","5":"229","6":"5950","7":"male","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"43.5","4":"15.2","5":"213","6":"4650","7":"female","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"51.5","4":"16.3","5":"230","6":"5500","7":"male","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"46.2","4":"14.1","5":"217","6":"4375","7":"female","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"55.1","4":"16.0","5":"230","6":"5850","7":"male","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"44.5","4":"15.7","5":"217","6":"4875","7":"NA","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"48.8","4":"16.2","5":"222","6":"6000","7":"male","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"47.2","4":"13.7","5":"214","6":"4925","7":"female","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"46.8","4":"14.3","5":"215","6":"4850","7":"female","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"50.4","4":"15.7","5":"222","6":"5750","7":"male","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"45.2","4":"14.8","5":"212","6":"5200","7":"female","8":"2009"},{"1":"Gentoo","2":"Biscoe","3":"49.9","4":"16.1","5":"213","6":"5400","7":"male","8":"2009"},{"1":"Chinstrap","2":"Dream","3":"46.5","4":"17.9","5":"192","6":"3500","7":"female","8":"2007"},{"1":"Chinstrap","2":"Dream","3":"50.0","4":"19.5","5":"196","6":"3900","7":"male","8":"2007"},{"1":"Chinstrap","2":"Dream","3":"51.3","4":"19.2","5":"193","6":"3650","7":"male","8":"2007"},{"1":"Chinstrap","2":"Dream","3":"45.4","4":"18.7","5":"188","6":"3525","7":"female","8":"2007"},{"1":"Chinstrap","2":"Dream","3":"52.7","4":"19.8","5":"197","6":"3725","7":"male","8":"2007"},{"1":"Chinstrap","2":"Dream","3":"45.2","4":"17.8","5":"198","6":"3950","7":"female","8":"2007"},{"1":"Chinstrap","2":"Dream","3":"46.1","4":"18.2","5":"178","6":"3250","7":"female","8":"2007"},{"1":"Chinstrap","2":"Dream","3":"51.3","4":"18.2","5":"197","6":"3750","7":"male","8":"2007"},{"1":"Chinstrap","2":"Dream","3":"46.0","4":"18.9","5":"195","6":"4150","7":"female","8":"2007"},{"1":"Chinstrap","2":"Dream","3":"51.3","4":"19.9","5":"198","6":"3700","7":"male","8":"2007"},{"1":"Chinstrap","2":"Dream","3":"46.6","4":"17.8","5":"193","6":"3800","7":"female","8":"2007"},{"1":"Chinstrap","2":"Dream","3":"51.7","4":"20.3","5":"194","6":"3775","7":"male","8":"2007"},{"1":"Chinstrap","2":"Dream","3":"47.0","4":"17.3","5":"185","6":"3700","7":"female","8":"2007"},{"1":"Chinstrap","2":"Dream","3":"52.0","4":"18.1","5":"201","6":"4050","7":"male","8":"2007"},{"1":"Chinstrap","2":"Dream","3":"45.9","4":"17.1","5":"190","6":"3575","7":"female","8":"2007"},{"1":"Chinstrap","2":"Dream","3":"50.5","4":"19.6","5":"201","6":"4050","7":"male","8":"2007"},{"1":"Chinstrap","2":"Dream","3":"50.3","4":"20.0","5":"197","6":"3300","7":"male","8":"2007"},{"1":"Chinstrap","2":"Dream","3":"58.0","4":"17.8","5":"181","6":"3700","7":"female","8":"2007"},{"1":"Chinstrap","2":"Dream","3":"46.4","4":"18.6","5":"190","6":"3450","7":"female","8":"2007"},{"1":"Chinstrap","2":"Dream","3":"49.2","4":"18.2","5":"195","6":"4400","7":"male","8":"2007"},{"1":"Chinstrap","2":"Dream","3":"42.4","4":"17.3","5":"181","6":"3600","7":"female","8":"2007"},{"1":"Chinstrap","2":"Dream","3":"48.5","4":"17.5","5":"191","6":"3400","7":"male","8":"2007"},{"1":"Chinstrap","2":"Dream","3":"43.2","4":"16.6","5":"187","6":"2900","7":"female","8":"2007"},{"1":"Chinstrap","2":"Dream","3":"50.6","4":"19.4","5":"193","6":"3800","7":"male","8":"2007"},{"1":"Chinstrap","2":"Dream","3":"46.7","4":"17.9","5":"195","6":"3300","7":"female","8":"2007"},{"1":"Chinstrap","2":"Dream","3":"52.0","4":"19.0","5":"197","6":"4150","7":"male","8":"2007"},{"1":"Chinstrap","2":"Dream","3":"50.5","4":"18.4","5":"200","6":"3400","7":"female","8":"2008"},{"1":"Chinstrap","2":"Dream","3":"49.5","4":"19.0","5":"200","6":"3800","7":"male","8":"2008"},{"1":"Chinstrap","2":"Dream","3":"46.4","4":"17.8","5":"191","6":"3700","7":"female","8":"2008"},{"1":"Chinstrap","2":"Dream","3":"52.8","4":"20.0","5":"205","6":"4550","7":"male","8":"2008"},{"1":"Chinstrap","2":"Dream","3":"40.9","4":"16.6","5":"187","6":"3200","7":"female","8":"2008"},{"1":"Chinstrap","2":"Dream","3":"54.2","4":"20.8","5":"201","6":"4300","7":"male","8":"2008"},{"1":"Chinstrap","2":"Dream","3":"42.5","4":"16.7","5":"187","6":"3350","7":"female","8":"2008"},{"1":"Chinstrap","2":"Dream","3":"51.0","4":"18.8","5":"203","6":"4100","7":"male","8":"2008"},{"1":"Chinstrap","2":"Dream","3":"49.7","4":"18.6","5":"195","6":"3600","7":"male","8":"2008"},{"1":"Chinstrap","2":"Dream","3":"47.5","4":"16.8","5":"199","6":"3900","7":"female","8":"2008"},{"1":"Chinstrap","2":"Dream","3":"47.6","4":"18.3","5":"195","6":"3850","7":"female","8":"2008"},{"1":"Chinstrap","2":"Dream","3":"52.0","4":"20.7","5":"210","6":"4800","7":"male","8":"2008"},{"1":"Chinstrap","2":"Dream","3":"46.9","4":"16.6","5":"192","6":"2700","7":"female","8":"2008"},{"1":"Chinstrap","2":"Dream","3":"53.5","4":"19.9","5":"205","6":"4500","7":"male","8":"2008"},{"1":"Chinstrap","2":"Dream","3":"49.0","4":"19.5","5":"210","6":"3950","7":"male","8":"2008"},{"1":"Chinstrap","2":"Dream","3":"46.2","4":"17.5","5":"187","6":"3650","7":"female","8":"2008"},{"1":"Chinstrap","2":"Dream","3":"50.9","4":"19.1","5":"196","6":"3550","7":"male","8":"2008"},{"1":"Chinstrap","2":"Dream","3":"45.5","4":"17.0","5":"196","6":"3500","7":"female","8":"2008"},{"1":"Chinstrap","2":"Dream","3":"50.9","4":"17.9","5":"196","6":"3675","7":"female","8":"2009"},{"1":"Chinstrap","2":"Dream","3":"50.8","4":"18.5","5":"201","6":"4450","7":"male","8":"2009"},{"1":"Chinstrap","2":"Dream","3":"50.1","4":"17.9","5":"190","6":"3400","7":"female","8":"2009"},{"1":"Chinstrap","2":"Dream","3":"49.0","4":"19.6","5":"212","6":"4300","7":"male","8":"2009"},{"1":"Chinstrap","2":"Dream","3":"51.5","4":"18.7","5":"187","6":"3250","7":"male","8":"2009"},{"1":"Chinstrap","2":"Dream","3":"49.8","4":"17.3","5":"198","6":"3675","7":"female","8":"2009"},{"1":"Chinstrap","2":"Dream","3":"48.1","4":"16.4","5":"199","6":"3325","7":"female","8":"2009"},{"1":"Chinstrap","2":"Dream","3":"51.4","4":"19.0","5":"201","6":"3950","7":"male","8":"2009"},{"1":"Chinstrap","2":"Dream","3":"45.7","4":"17.3","5":"193","6":"3600","7":"female","8":"2009"},{"1":"Chinstrap","2":"Dream","3":"50.7","4":"19.7","5":"203","6":"4050","7":"male","8":"2009"},{"1":"Chinstrap","2":"Dream","3":"42.5","4":"17.3","5":"187","6":"3350","7":"female","8":"2009"},{"1":"Chinstrap","2":"Dream","3":"52.2","4":"18.8","5":"197","6":"3450","7":"male","8":"2009"},{"1":"Chinstrap","2":"Dream","3":"45.2","4":"16.6","5":"191","6":"3250","7":"female","8":"2009"},{"1":"Chinstrap","2":"Dream","3":"49.3","4":"19.9","5":"203","6":"4050","7":"male","8":"2009"},{"1":"Chinstrap","2":"Dream","3":"50.2","4":"18.8","5":"202","6":"3800","7":"male","8":"2009"},{"1":"Chinstrap","2":"Dream","3":"45.6","4":"19.4","5":"194","6":"3525","7":"female","8":"2009"},{"1":"Chinstrap","2":"Dream","3":"51.9","4":"19.5","5":"206","6":"3950","7":"male","8":"2009"},{"1":"Chinstrap","2":"Dream","3":"46.8","4":"16.5","5":"189","6":"3650","7":"female","8":"2009"},{"1":"Chinstrap","2":"Dream","3":"45.7","4":"17.0","5":"195","6":"3650","7":"female","8":"2009"},{"1":"Chinstrap","2":"Dream","3":"55.8","4":"19.8","5":"207","6":"4000","7":"male","8":"2009"},{"1":"Chinstrap","2":"Dream","3":"43.5","4":"18.1","5":"202","6":"3400","7":"female","8":"2009"},{"1":"Chinstrap","2":"Dream","3":"49.6","4":"18.2","5":"193","6":"3775","7":"male","8":"2009"},{"1":"Chinstrap","2":"Dream","3":"50.8","4":"19.0","5":"210","6":"4100","7":"male","8":"2009"},{"1":"Chinstrap","2":"Dream","3":"50.2","4":"18.7","5":"198","6":"3775","7":"female","8":"2009"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

You can scroll through the rows by using the numbers at the bottom or the "Next" button. You can scroll through the variables by clicked the little black arrow pointed to the right in the upper-right corner. The only thing you can't do here that you can do with `View` is sort the columns.

We want to understand the "structure" of our data. For this, we use the `str` command. Try it:


```r
str(penguins)
```

This tells us several important things. First it says that we are looking at a tibble with 344 observations of 8 variables. We can isolate those pieces of information separately as well, if needed:


```r
NROW(penguins)
```


```r
NCOL(penguins)
```

These give you the number of rows and columns, respectively.

The `str` command also tells us about each of the variables in our data set. We'll talk about these later.

We need to be able to summarize variables in the data set. The `summary` command is one way to do it:


```r
summary(penguins)
```

You may not recognize terms like "Median" or "1st Qu." or "3rd Qu." yet. Nevertheless, you can see why this summary could come in handy.


## Understanding the variables {#intror-understandingvariables}

When we want to look at only one variable at a time, we use the dollar sign to grab it. Try this:


```r
penguins$body_mass_g
```

This will list the entire `body_mass_g` column, in other words, the body masses (in grams) of all the penguins in this particular study. If we only want to see the first few, we can use `head` like before.


```r
head(penguins$body_mass_g)
```

If we want the structure of the variable `body_mass_g`, we do this:


```r
str(penguins$body_mass_g)
```

Notice the letters `int` at the beginning of the line. That stands for "integer" which is another word for whole number. In other words, the penguins' body masses all appear in this data set as whole numbers. There are other data types you'll see in the future:

* `num`: This is for general numerical data (which can be integers as well as having decimal parts).
* `chr`: This means "character", used for character strings, which can be any sequence of letters or numbers. For example, if the researcher recorded some notes for each penguin, these notes would be recorded in a character variable.
* `factor`:  This is for categorical data, which is data that groups observations together into categories. For example, `species` is categorical. These are generally recorded like character strings, but factor variables have more structure because they take on a limited number of possible values corresponding to a generally small number of categories. We'll learn a lot more about factor variables in future chapters.

There are other data types, but the ones above are by far the most common that you'll encounter on a regular basis.

If we want to summarize only the variable `body_mass_g`, we can do this:


```r
summary(penguins$body_mass_g)
```

While executing the commands above, you may have noticed entries listed as `NA`. These are "missing" values. It is worth paying attention to missing values and thinking carefully about why they might be missing. For now, just make a mental note that `NA` is the code R uses for data that is missing. (This would be the same as a blank cell in a spreadsheet.)


## Projects {#intror-projects}

Using files in R requires you to be organized. R uses what's called a "working directory" to find the files it needs. Therefore, you can't just put files any old place and expect R to be able to find them.

One way of ensuring that files are all located where R can find them is to organize your work into projects. Look in the far upper-right corner of the RStudio screen. You should see some text that says `Project: (None)`. This means we are not currently in a project. We're going to create a new project in preparation for the next chapter on using R Markdown.

Open the drop-down menu here and select `New Project`. When the dialog box opens, select `New Directory`, then `New Project`.

You'll need to give your project a name. In general, this should be a descriptive name---one that could still remind you in several years what the project was about. The only thing to remember is that project names and file names should not have any spaces in them. In fact, you should avoid other kinds of special characters as well, like commas, number signs, etc. Stick to letters and numerals and you should be just fine. If you want a multi-word project name or file name, I recommend using underscores. R will allow you to name projects with spaces and modern operating systems are set up to handle file names with spaces, but there are certain things that either don't work at all or require awkward workarounds when file names have spaces. In this case, let's type `intro_stats` for the "Directory name". Leave everything else alone and click `Create Project`.

You will see the screen refresh and R will restart.

You will see a new file called `intro_stats.Rproj` in the Files pane, but **you should never touch that file**. It's just for RStudio to keep track of your project details.

If everything works the way it should, creating a new project will create a new folder, put you in that folder, and automatically make it your working directory.

Any additional files you need for your project should be placed in this directory. In all future chapters, the first thing you will do is download the chapter file from the book website and place it here in your project folder. If you have installed R and RStudio on your own machine, you'll need to navigate your system to find the downloaded file and move or copy it to your project working directory. (This is done most easily using File Explorer in Windows and the Finder in MacOS.) If you are using RStudio Workbench through a web browser, you'll need to upload it to your project folder using the "Upload" button in the Files tab.


## Conclusion {#intror-conclusion}

It is often said that there is a steep learning curve when learning R. This is true to some extent. R is harder to use at first than other types of software. Nevertheless, in this course, we will work hard to ease you over that first hurdle and get you moving relatively quickly. Don't get frustrated and don't give up! Learning R is worth the effort you put in. Eventually, you'll grow to appreciate the power and flexibility of R for accomplishing a huge variety of statistical tasks.

Onward and upward!