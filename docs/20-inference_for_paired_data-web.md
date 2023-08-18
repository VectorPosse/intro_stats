# Inference for paired data

<!-- Please don't mess with the next few lines! -->
<style>h5{font-size:2em;color:#0000FF}h6{font-size:1.5em;color:#0000FF}div.answer{margin-left:5%;border:1px solid #0000FF;border-left-width:10px;padding:25px} div.summary{background-color:rgba(30,144,255,0.1);border:3px double #0000FF;padding:25px}</style><p style="color:#ffffff">2.0</p>
<!-- Please don't mess with the previous few lines! -->


::: {.summary}

### Functions introduced in this chapter {-}

No new R functions are introduced here.

:::


## Introduction

In this chapter we will learn how to run inference for two paired numerical variables.

### Install new packages

There are no new packages used in this chapter.

### Download the R notebook file

Check the upper-right corner in RStudio to make sure you're in your `intro_stats` project. Then click on the following link to download this chapter as an R notebook file (`.Rmd`).

<a href = "https://vectorposse.github.io/intro_stats/chapter_downloads/20-inference_for_paired_data.Rmd" download>https://vectorposse.github.io/intro_stats/chapter_downloads/20-inference_for_paired_data.Rmd</a>

Once the file is downloaded, move it to your project folder in RStudio and open it there.

### Restart R and run all chunks

In RStudio, select "Restart R and Run All Chunks" from the "Run" menu.


## Load packages

We load the standard `tidyverse` and `infer` packages. The `openintro` package will give access to the `textbooks` data and the `hsb2` data.


```r
library(tidyverse)
```

```
## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
## ✔ dplyr     1.1.2     ✔ readr     2.1.4
## ✔ forcats   1.0.0     ✔ stringr   1.5.0
## ✔ ggplot2   3.4.2     ✔ tibble    3.2.1
## ✔ lubridate 1.9.2     ✔ tidyr     1.3.0
## ✔ purrr     1.0.2     
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```

```r
library(infer)
library(openintro)
```

```
## Loading required package: airports
## Loading required package: cherryblossom
## Loading required package: usdata
```


## Paired data

Sometimes data sets have two numerical variables that are related to each other. For example, a diet study might include a pre-weight and a post-weight. The research question is not about either of these variables directly, but rather the difference between the variables, for example how much weight was lost during the diet.

When this is the case, we run inference for paired data. The procedure involves calculating a new variable `d` that represents the difference of the two paired variables. The null hypothesis is almost always that there is no difference between the paired variables, and that translates into the statement that the average value of `d` is zero.


## Research question

The `textbooks` data frame (from the `openintro` package) has data on the price of books at the UCLA bookstore versus Amazon.com. The question of interest here is whether the campus bookstore charges more than Amazon.


## Inference for paired data

The key idea is that we don't actually care about the book prices themselves. All we care about is if there is a difference between the prices for each book. These are not two independent variables because each row represents a single book. Therefore, the two measurements are "paired" and should be treated as a single numerical variable of interest, representing the difference between `ucla_new` and `amaz_new`.

Since we're only interested in analyzing the one numerical variable `d`, this process is nothing more than a one-sample t test. Therefore, there is really nothing new in this chapter.

Let's go through the rubric.


## Exploratory data analysis

### Use data documentation (help files, code books, Google, etc.) to determine as much as possible about the data provenance and structure.

You should type `textbooks` at the Console to read the help file. The data was collected by a person, David Diez. A quick Google search reveals that he is a statistician who graduated from UCLA. We presume he had access to accurate information about the prices of books at the UCLA bookstore and from Amazon.com at the time the data was collected.

Here is the data set:


```r
textbooks
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["dept_abbr"],"name":[1],"type":["fct"],"align":["left"]},{"label":["course"],"name":[2],"type":["fct"],"align":["left"]},{"label":["isbn"],"name":[3],"type":["fct"],"align":["left"]},{"label":["ucla_new"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["amaz_new"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["more"],"name":[6],"type":["fct"],"align":["left"]},{"label":["diff"],"name":[7],"type":["dbl"],"align":["right"]}],"data":[{"1":"Am Ind","2":"C170","3":"978-0803272620","4":"27.67","5":"27.95","6":"Y","7":"-0.28"},{"1":"Anthro","2":"9","3":"978-0030119194","4":"40.59","5":"31.14","6":"Y","7":"9.45"},{"1":"Anthro","2":"135T","3":"978-0300080643","4":"31.68","5":"32.00","6":"Y","7":"-0.32"},{"1":"Anthro","2":"191HB","3":"978-0226206813","4":"16.00","5":"11.52","6":"Y","7":"4.48"},{"1":"Art His","2":"M102K","3":"978-0892365999","4":"18.95","5":"14.21","6":"Y","7":"4.74"},{"1":"Art His","2":"118E","3":"978-0394723693","4":"14.95","5":"10.17","6":"Y","7":"4.78"},{"1":"Asia Am","2":"187B","3":"978-0822338437","4":"24.70","5":"20.06","6":"Y","7":"4.64"},{"1":"Asia Am","2":"191E","3":"978-0816646135","4":"19.50","5":"16.66","6":"N","7":"2.84"},{"1":"Ch Engr","2":"C125","3":"978-0195123401","4":"123.84","5":"106.25","6":"N","7":"17.59"},{"1":"Chicano","2":"M145B","3":"978-0896086265","4":"17.00","5":"13.26","6":"Y","7":"3.74"},{"1":"Chin","2":"174","3":"978-0791420621","4":"31.63","5":"29.95","6":"Y","7":"1.68"},{"1":"Com Sci","2":"180","3":"978-0321295354","4":"116.00","5":"88.09","6":"N","7":"27.91"},{"1":"Comm Lit","2":"290","3":"978-0393329254","4":"27.67","5":"18.45","6":"Y","7":"9.22"},{"1":"Comm St","2":"10","3":"978-0195181234","4":"24.70","5":"16.47","6":"Y","7":"8.23"},{"1":"Comptng","2":"20A","3":"978-0470509487","4":"126.67","5":"97.38","6":"N","7":"29.29"},{"1":"E&S Sci","2":"7","3":"978-0521711128","4":"53.90","5":"47.51","6":"N","7":"6.39"},{"1":"E&S Sci","2":"101","3":"978-0393927634","4":"89.73","5":"74.93","6":"N","7":"14.80"},{"1":"Econ","2":"101","3":"978-0324421620","4":"171.00","5":"132.77","6":"N","7":"38.23"},{"1":"El Engr","2":"2","3":"978-0131497269","4":"152.00","5":"121.50","6":"N","7":"30.50"},{"1":"El Engr","2":"176","3":"978-0471287704","4":"124.80","5":"108.00","6":"N","7":"16.80"},{"1":"Engl","2":"M104C","3":"978-0582276024","4":"16.00","5":"11.67","6":"Y","7":"4.33"},{"1":"Engl","2":"151","3":"978-0199561339","4":"25.95","5":"18.94","6":"Y","7":"7.01"},{"1":"Engl","2":"180","3":"978-0374515362","4":"18.00","5":"12.24","6":"Y","7":"5.76"},{"1":"Engl","2":"M260A","3":"978-0804747295","4":"21.73","5":"21.95","6":"Y","7":"-0.22"},{"1":"Engr","2":"183EW","3":"978-0132306416","4":"40.59","5":"31.14","6":"N","7":"9.45"},{"1":"ESL","2":"38A","3":"978-1577665304","4":"28.95","5":"28.95","6":"Y","7":"0.00"},{"1":"Frnch","2":"120","3":"978-2020525718","4":"19.95","5":"16.15","6":"Y","7":"3.80"},{"1":"Geog","2":"232","3":"978-1405102667","4":"49.45","5":"37.75","6":"Y","7":"11.70"},{"1":"German","2":"57","3":"978-0205668946","4":"41.09","5":"31.78","6":"N","7":"9.31"},{"1":"Greek","2":"130","3":"978-1598561692","4":"50.95","5":"32.97","6":"Y","7":"17.98"},{"1":"Hist","2":"97F","3":"978-1560000259","4":"44.50","5":"30.87","6":"Y","7":"13.63"},{"1":"Hist","2":"201B","3":"978-0198149248","4":"82.45","5":"85.00","6":"Y","7":"-2.55"},{"1":"Hist","2":"201L","3":"978-0742567498","4":"34.60","5":"34.95","6":"Y","7":"-0.35"},{"1":"Hlt Ser","2":"M422","3":"978-0761918479","4":"88.42","5":"97.95","6":"Y","7":"-9.53"},{"1":"Hlt Ser","2":"437","3":"978-0763745554","4":"84.00","5":"81.18","6":"N","7":"2.82"},{"1":"Hnrs","2":"37","3":"978-0140447187","4":"11.25","5":"9.50","6":"Y","7":"1.75"},{"1":"Hnrs","2":"M106","3":"978-1400033416","4":"15.00","5":"10.20","6":"Y","7":"4.80"},{"1":"Lifesci","2":"2","3":"978-0716776710","4":"180.03","5":"134.69","6":"Y","7":"45.34"},{"1":"Mat Sci","2":"104","3":"978-0470419977","4":"174.00","5":"143.75","6":"N","7":"30.25"},{"1":"Math","2":"151A","3":"978-0534407612","4":"188.58","5":"176.00","6":"N","7":"12.58"},{"1":"Mgmt","2":"1A","3":"978-0077251031","4":"146.75","5":"150.63","6":"Y","7":"-3.88"},{"1":"Mgmt","2":"124","3":"978-0078136634","4":"183.75","5":"145.40","6":"N","7":"38.35"},{"1":"Mgmt","2":"127B","3":"978-0324828641","4":"214.50","5":"173.56","6":"N","7":"40.94"},{"1":"Mgmt","2":"228","3":"978-0073379661","4":"197.00","5":"131.00","6":"Y","7":"66.00"},{"1":"Mgmt","2":"231D","3":"978-0131407374","4":"194.00","5":"157.68","6":"Y","7":"36.32"},{"1":"Mgmt","2":"263A","3":"978-0073404769","4":"176.25","5":"137.17","6":"Y","7":"39.08"},{"1":"Mus His","2":"191G","3":"978-0195162578","4":"24.70","5":"24.95","6":"Y","7":"-0.25"},{"1":"Physics","2":"6A","3":"978-0534491437","4":"188.00","5":"159.28","6":"Y","7":"28.72"},{"1":"Pol Sci","2":"10","3":"978-0393090406","4":"29.70","5":"23.07","6":"Y","7":"6.63"},{"1":"Pol Sci","2":"120B","3":"978-1588264169","4":"26.24","5":"21.04","6":"Y","7":"5.20"},{"1":"Pol Sci","2":"M132B","3":"978-0205753383","4":"55.13","5":"42.68","6":"Y","7":"12.45"},{"1":"Pol Sci","2":"171C","3":"978-0393976267","4":"43.56","5":"39.37","6":"Y","7":"4.19"},{"1":"Pol Sci","2":"200C","3":"978-0471663799","4":"129.60","5":"85.20","6":"Y","7":"44.40"},{"1":"Psych","2":"10","3":"978-1429227049","4":"123.84","5":"93.13","6":"N","7":"30.71"},{"1":"Psych","2":"134A","3":"978-0073378541","4":"85.12","5":"67.40","6":"Y","7":"17.72"},{"1":"Psych","2":"263","3":"978-0495099697","4":"152.48","5":"132.79","6":"N","7":"19.69"},{"1":"Russian","2":"90B","3":"978-0674304437","4":"29.70","5":"24.31","6":"Y","7":"5.39"},{"1":"Soc Gen","2":"102W","3":"978-0520265882","4":"24.70","5":"24.95","6":"N","7":"-0.25"},{"1":"Soc Wlf","2":"M104D","3":"978-1412969666","4":"89.71","5":"85.45","6":"N","7":"4.26"},{"1":"Sociol","2":"180A","3":"978-0812968378","4":"10.50","5":"10.08","6":"N","7":"0.42"},{"1":"Span","2":"105","3":"978-0618527748","4":"92.88","5":"65.73","6":"N","7":"27.15"},{"1":"Span","2":"115","3":"978-0471013914","4":"100.88","5":"84.26","6":"N","7":"16.62"},{"1":"Stat","2":"98T","3":"978-0393310726","4":"11.95","5":"8.60","6":"Y","7":"3.35"},{"1":"Stat","2":"C180","3":"978-1584883883","4":"72.47","5":"58.42","6":"N","7":"14.05"},{"1":"Stat","2":"200C","3":"978-0412043710","4":"92.10","5":"80.37","6":"N","7":"11.73"},{"1":"Stat","2":"C216","3":"978-1441931566","4":"78.35","5":"64.09","6":"N","7":"14.26"},{"1":"Urbn Pl","2":"M275","3":"978-1592134328","4":"42.00","5":"37.04","6":"Y","7":"4.96"},{"1":"Wld Art","2":"C139","3":"978-0520224759","4":"18.75","5":"20.21","6":"Y","7":"-1.46"},{"1":"Wld Art","2":"C180","3":"978-1412923484","4":"48.46","5":"39.34","6":"N","7":"9.12"},{"1":"Wld Art","2":"C409A","3":"978-0195390827","4":"39.55","5":"26.37","6":"N","7":"13.18"},{"1":"Wld Art","2":"495","3":"978-0820474151","4":"29.65","5":"24.19","6":"Y","7":"5.46"},{"1":"Wom Std","2":"M144","3":"978-1570755637","4":"23.76","5":"18.72","6":"Y","7":"5.04"},{"1":"Wom Std","2":"285","3":"978-0822341147","4":"27.70","5":"18.22","6":"Y","7":"9.48"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


```r
glimpse(textbooks)
```

```
## Rows: 73
## Columns: 7
## $ dept_abbr <fct> Am Ind, Anthro, Anthro, Anthro, Art His, Art His, Asia Am, A…
## $ course    <fct>  C170, 9, 135T, 191HB, M102K, 118E, 187B, 191E, C125, M145B,…
## $ isbn      <fct> 978-0803272620, 978-0030119194, 978-0300080643, 978-02262068…
## $ ucla_new  <dbl> 27.67, 40.59, 31.68, 16.00, 18.95, 14.95, 24.70, 19.50, 123.…
## $ amaz_new  <dbl> 27.95, 31.14, 32.00, 11.52, 14.21, 10.17, 20.06, 16.66, 106.…
## $ more      <fct> Y, Y, Y, Y, Y, Y, Y, N, N, Y, Y, N, Y, Y, N, N, N, N, N, N, …
## $ diff      <dbl> -0.28, 9.45, -0.32, 4.48, 4.74, 4.78, 4.64, 2.84, 17.59, 3.7…
```
The two paired variables are `ucla_new` and `amaz_new`.

### Prepare the data for analysis.

Generally, we will need to create a new variable `d` that represents the difference between the two paired variables of interest. This uses the `mutate` command that adds an extra column to our data frame. The order of subtraction usually does not matter, but we will want to keep track of that order so that we can interpret our test statistic correctly. In the case of a one-sided test (which this is), it is especially important to keep track of the order of subtraction. Since we suspect the bookstore will charge more than Amazon, let's subtract in that order. Our hunch is that it will be a positive number, on average.


```r
textbooks_d <- textbooks %>%
    mutate(d = ucla_new - amaz_new)
textbooks_d
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["dept_abbr"],"name":[1],"type":["fct"],"align":["left"]},{"label":["course"],"name":[2],"type":["fct"],"align":["left"]},{"label":["isbn"],"name":[3],"type":["fct"],"align":["left"]},{"label":["ucla_new"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["amaz_new"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["more"],"name":[6],"type":["fct"],"align":["left"]},{"label":["diff"],"name":[7],"type":["dbl"],"align":["right"]},{"label":["d"],"name":[8],"type":["dbl"],"align":["right"]}],"data":[{"1":"Am Ind","2":"C170","3":"978-0803272620","4":"27.67","5":"27.95","6":"Y","7":"-0.28","8":"-0.28"},{"1":"Anthro","2":"9","3":"978-0030119194","4":"40.59","5":"31.14","6":"Y","7":"9.45","8":"9.45"},{"1":"Anthro","2":"135T","3":"978-0300080643","4":"31.68","5":"32.00","6":"Y","7":"-0.32","8":"-0.32"},{"1":"Anthro","2":"191HB","3":"978-0226206813","4":"16.00","5":"11.52","6":"Y","7":"4.48","8":"4.48"},{"1":"Art His","2":"M102K","3":"978-0892365999","4":"18.95","5":"14.21","6":"Y","7":"4.74","8":"4.74"},{"1":"Art His","2":"118E","3":"978-0394723693","4":"14.95","5":"10.17","6":"Y","7":"4.78","8":"4.78"},{"1":"Asia Am","2":"187B","3":"978-0822338437","4":"24.70","5":"20.06","6":"Y","7":"4.64","8":"4.64"},{"1":"Asia Am","2":"191E","3":"978-0816646135","4":"19.50","5":"16.66","6":"N","7":"2.84","8":"2.84"},{"1":"Ch Engr","2":"C125","3":"978-0195123401","4":"123.84","5":"106.25","6":"N","7":"17.59","8":"17.59"},{"1":"Chicano","2":"M145B","3":"978-0896086265","4":"17.00","5":"13.26","6":"Y","7":"3.74","8":"3.74"},{"1":"Chin","2":"174","3":"978-0791420621","4":"31.63","5":"29.95","6":"Y","7":"1.68","8":"1.68"},{"1":"Com Sci","2":"180","3":"978-0321295354","4":"116.00","5":"88.09","6":"N","7":"27.91","8":"27.91"},{"1":"Comm Lit","2":"290","3":"978-0393329254","4":"27.67","5":"18.45","6":"Y","7":"9.22","8":"9.22"},{"1":"Comm St","2":"10","3":"978-0195181234","4":"24.70","5":"16.47","6":"Y","7":"8.23","8":"8.23"},{"1":"Comptng","2":"20A","3":"978-0470509487","4":"126.67","5":"97.38","6":"N","7":"29.29","8":"29.29"},{"1":"E&S Sci","2":"7","3":"978-0521711128","4":"53.90","5":"47.51","6":"N","7":"6.39","8":"6.39"},{"1":"E&S Sci","2":"101","3":"978-0393927634","4":"89.73","5":"74.93","6":"N","7":"14.80","8":"14.80"},{"1":"Econ","2":"101","3":"978-0324421620","4":"171.00","5":"132.77","6":"N","7":"38.23","8":"38.23"},{"1":"El Engr","2":"2","3":"978-0131497269","4":"152.00","5":"121.50","6":"N","7":"30.50","8":"30.50"},{"1":"El Engr","2":"176","3":"978-0471287704","4":"124.80","5":"108.00","6":"N","7":"16.80","8":"16.80"},{"1":"Engl","2":"M104C","3":"978-0582276024","4":"16.00","5":"11.67","6":"Y","7":"4.33","8":"4.33"},{"1":"Engl","2":"151","3":"978-0199561339","4":"25.95","5":"18.94","6":"Y","7":"7.01","8":"7.01"},{"1":"Engl","2":"180","3":"978-0374515362","4":"18.00","5":"12.24","6":"Y","7":"5.76","8":"5.76"},{"1":"Engl","2":"M260A","3":"978-0804747295","4":"21.73","5":"21.95","6":"Y","7":"-0.22","8":"-0.22"},{"1":"Engr","2":"183EW","3":"978-0132306416","4":"40.59","5":"31.14","6":"N","7":"9.45","8":"9.45"},{"1":"ESL","2":"38A","3":"978-1577665304","4":"28.95","5":"28.95","6":"Y","7":"0.00","8":"0.00"},{"1":"Frnch","2":"120","3":"978-2020525718","4":"19.95","5":"16.15","6":"Y","7":"3.80","8":"3.80"},{"1":"Geog","2":"232","3":"978-1405102667","4":"49.45","5":"37.75","6":"Y","7":"11.70","8":"11.70"},{"1":"German","2":"57","3":"978-0205668946","4":"41.09","5":"31.78","6":"N","7":"9.31","8":"9.31"},{"1":"Greek","2":"130","3":"978-1598561692","4":"50.95","5":"32.97","6":"Y","7":"17.98","8":"17.98"},{"1":"Hist","2":"97F","3":"978-1560000259","4":"44.50","5":"30.87","6":"Y","7":"13.63","8":"13.63"},{"1":"Hist","2":"201B","3":"978-0198149248","4":"82.45","5":"85.00","6":"Y","7":"-2.55","8":"-2.55"},{"1":"Hist","2":"201L","3":"978-0742567498","4":"34.60","5":"34.95","6":"Y","7":"-0.35","8":"-0.35"},{"1":"Hlt Ser","2":"M422","3":"978-0761918479","4":"88.42","5":"97.95","6":"Y","7":"-9.53","8":"-9.53"},{"1":"Hlt Ser","2":"437","3":"978-0763745554","4":"84.00","5":"81.18","6":"N","7":"2.82","8":"2.82"},{"1":"Hnrs","2":"37","3":"978-0140447187","4":"11.25","5":"9.50","6":"Y","7":"1.75","8":"1.75"},{"1":"Hnrs","2":"M106","3":"978-1400033416","4":"15.00","5":"10.20","6":"Y","7":"4.80","8":"4.80"},{"1":"Lifesci","2":"2","3":"978-0716776710","4":"180.03","5":"134.69","6":"Y","7":"45.34","8":"45.34"},{"1":"Mat Sci","2":"104","3":"978-0470419977","4":"174.00","5":"143.75","6":"N","7":"30.25","8":"30.25"},{"1":"Math","2":"151A","3":"978-0534407612","4":"188.58","5":"176.00","6":"N","7":"12.58","8":"12.58"},{"1":"Mgmt","2":"1A","3":"978-0077251031","4":"146.75","5":"150.63","6":"Y","7":"-3.88","8":"-3.88"},{"1":"Mgmt","2":"124","3":"978-0078136634","4":"183.75","5":"145.40","6":"N","7":"38.35","8":"38.35"},{"1":"Mgmt","2":"127B","3":"978-0324828641","4":"214.50","5":"173.56","6":"N","7":"40.94","8":"40.94"},{"1":"Mgmt","2":"228","3":"978-0073379661","4":"197.00","5":"131.00","6":"Y","7":"66.00","8":"66.00"},{"1":"Mgmt","2":"231D","3":"978-0131407374","4":"194.00","5":"157.68","6":"Y","7":"36.32","8":"36.32"},{"1":"Mgmt","2":"263A","3":"978-0073404769","4":"176.25","5":"137.17","6":"Y","7":"39.08","8":"39.08"},{"1":"Mus His","2":"191G","3":"978-0195162578","4":"24.70","5":"24.95","6":"Y","7":"-0.25","8":"-0.25"},{"1":"Physics","2":"6A","3":"978-0534491437","4":"188.00","5":"159.28","6":"Y","7":"28.72","8":"28.72"},{"1":"Pol Sci","2":"10","3":"978-0393090406","4":"29.70","5":"23.07","6":"Y","7":"6.63","8":"6.63"},{"1":"Pol Sci","2":"120B","3":"978-1588264169","4":"26.24","5":"21.04","6":"Y","7":"5.20","8":"5.20"},{"1":"Pol Sci","2":"M132B","3":"978-0205753383","4":"55.13","5":"42.68","6":"Y","7":"12.45","8":"12.45"},{"1":"Pol Sci","2":"171C","3":"978-0393976267","4":"43.56","5":"39.37","6":"Y","7":"4.19","8":"4.19"},{"1":"Pol Sci","2":"200C","3":"978-0471663799","4":"129.60","5":"85.20","6":"Y","7":"44.40","8":"44.40"},{"1":"Psych","2":"10","3":"978-1429227049","4":"123.84","5":"93.13","6":"N","7":"30.71","8":"30.71"},{"1":"Psych","2":"134A","3":"978-0073378541","4":"85.12","5":"67.40","6":"Y","7":"17.72","8":"17.72"},{"1":"Psych","2":"263","3":"978-0495099697","4":"152.48","5":"132.79","6":"N","7":"19.69","8":"19.69"},{"1":"Russian","2":"90B","3":"978-0674304437","4":"29.70","5":"24.31","6":"Y","7":"5.39","8":"5.39"},{"1":"Soc Gen","2":"102W","3":"978-0520265882","4":"24.70","5":"24.95","6":"N","7":"-0.25","8":"-0.25"},{"1":"Soc Wlf","2":"M104D","3":"978-1412969666","4":"89.71","5":"85.45","6":"N","7":"4.26","8":"4.26"},{"1":"Sociol","2":"180A","3":"978-0812968378","4":"10.50","5":"10.08","6":"N","7":"0.42","8":"0.42"},{"1":"Span","2":"105","3":"978-0618527748","4":"92.88","5":"65.73","6":"N","7":"27.15","8":"27.15"},{"1":"Span","2":"115","3":"978-0471013914","4":"100.88","5":"84.26","6":"N","7":"16.62","8":"16.62"},{"1":"Stat","2":"98T","3":"978-0393310726","4":"11.95","5":"8.60","6":"Y","7":"3.35","8":"3.35"},{"1":"Stat","2":"C180","3":"978-1584883883","4":"72.47","5":"58.42","6":"N","7":"14.05","8":"14.05"},{"1":"Stat","2":"200C","3":"978-0412043710","4":"92.10","5":"80.37","6":"N","7":"11.73","8":"11.73"},{"1":"Stat","2":"C216","3":"978-1441931566","4":"78.35","5":"64.09","6":"N","7":"14.26","8":"14.26"},{"1":"Urbn Pl","2":"M275","3":"978-1592134328","4":"42.00","5":"37.04","6":"Y","7":"4.96","8":"4.96"},{"1":"Wld Art","2":"C139","3":"978-0520224759","4":"18.75","5":"20.21","6":"Y","7":"-1.46","8":"-1.46"},{"1":"Wld Art","2":"C180","3":"978-1412923484","4":"48.46","5":"39.34","6":"N","7":"9.12","8":"9.12"},{"1":"Wld Art","2":"C409A","3":"978-0195390827","4":"39.55","5":"26.37","6":"N","7":"13.18","8":"13.18"},{"1":"Wld Art","2":"495","3":"978-0820474151","4":"29.65","5":"24.19","6":"Y","7":"5.46","8":"5.46"},{"1":"Wom Std","2":"M144","3":"978-1570755637","4":"23.76","5":"18.72","6":"Y","7":"5.04","8":"5.04"},{"1":"Wom Std","2":"285","3":"978-0822341147","4":"27.70","5":"18.22","6":"Y","7":"9.48","8":"9.48"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

If you look closely at the tibble above, you will see that there is a column already in our data called `diff`. It is the same as the column `d` we just created. So in this case, we didn't really need to create a new difference variable. However, since most data sets do not come pre-prepared with such a difference variable, it is good to know how to make one if needed.

### Make tables or plots to explore the data visually.

Here are summary statistics, a histogram, and a QQ plot for `d`.


```r
summary(textbooks_d$d)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   -9.53    3.80    8.23   12.76   17.59   66.00
```


```r
ggplot(textbooks_d, aes(x = d)) +
    geom_histogram(binwidth = 10, boundary = 0)
```

<img src="20-inference_for_paired_data-web_files/figure-html/unnamed-chunk-6-1.png" width="672" />


```r
ggplot(textbooks_d, aes(sample = d)) +
    geom_qq() +
    geom_qq_line()
```

<img src="20-inference_for_paired_data-web_files/figure-html/unnamed-chunk-7-1.png" width="672" />

The data is somewhat skewed to the right with one observation that might be a bit of an outlier. If the sample size were much smaller, we might be concerned about this point However, it's not much higher than other points in that right tail, and it doesn't appear that its inclusion or exclusion will change the overall conclusion much. If you are concerned that the point might alter the conclusion, run the hypothesis test twice, once with and once without the outlier present to see if the main conclusion changes.

## Hypotheses

### Identify the sample (or samples) and a reasonable population (or populations) of interest.

The sample consists of 73 textbooks. The population is all textbooks that might be sold both at the UCLA bookstore and on Amazon.

### Express the null and alternative hypotheses as contextually meaningful full sentences.

$H_{0}:$ There is no difference in textbooks prices between the UCLA bookstore and Amazon.

$H_{A}:$ Textbook prices at the UCLA bookstore are higher on average than on Amazon.

Commentary: Note we are performing a one-sided test. If we are conducting our own research with our own data, we can decide whether we want to run a two-sided or one-sided test. Remember that we only do the latter when we have a strong hypothesis in advance that the difference should be clearly in one direction and not the other. In this case, it's not up to us. We have to respect the research question as it was given to us: "The question of interest here is whether the campus bookstore charges more than Amazon."

##### Exercise 1 {-}

What would the research question say if we were supposed to run a two-sided test instead? In other words, write down a slightly different research question about textbook prices that would prompt us to run a two-sided test.

::: {.answer}

Please write up your answer here.

:::

### Express the null and alternative hypotheses in symbols (when possible).

$H_{0}: \mu_{d} = 0$

$H_{A}: \mu_{d} > 0$

Commentary: Since we're really just doing a one-sample t test, we could just call this parameter $\mu$, but the subscript $d$ is a good reminder that it's the mean of the difference variable we care about (as opposed to the mean price of all the books at the UCLA bookstore or the mean price of all the same books on Amazon).


## Model

### Identify the sampling distribution model.

We use a t model with 72 degrees of freedom.

##### Exercise 2 {-}

Explain how we got 72 degrees of freedom.

::: {.answer}

Please write up your answer here.

:::

### Check the relevant conditions to ensure that model assumptions are met.

* Random
    - We do not know how exactly how David Diez obtained this sample, but the help file claims it is a random sample.
    
* 10%
    - We do not know how many total textbooks were available at the UCLA bookstore at the time the sample was taken, so we do not know if this condition is met. As long as there were at least 730 books, we are okay. We suspect that, based on the size of UCLA and the number of course offerings there, this is a reasonable assumption.
    
* Nearly normal
    - Although the sample distribution is skewed (with a possible mild outlier), the sample size is more than 30.


## Mechanics

### Compute the test statistic.


```r
d_mean <- textbooks_d %>%
  specify(response = d) %>%
  calculate(stat = "mean")
d_mean
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["stat"],"name":[1],"type":["dbl"],"align":["right"]}],"data":[{"1":"12.76164"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


```r
d_t <- textbooks_d %>%
  specify(response = d) %>%
  hypothesize(null = "point", mu = 0) %>%
  calculate(stat = "t")
d_t
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["stat"],"name":[1],"type":["dbl"],"align":["right"]}],"data":[{"1":"7.648771"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

### Report the test statistic in context (when possible).

The mean difference in textbook prices is 12.7616438.

The value of t is 7.6487711. The mean difference in textbook prices is more than 7 standard errors above a difference of zero.


### Plot the null distribution.


```r
price_test <- textbooks_d %>%
  specify(response = d) %>%
  assume("t")
price_test
```

```
## A T distribution with 72 degrees of freedom.
```


```r
price_test %>%
  visualize() +
  shade_p_value(obs_stat = d_t, direction = "greater")
```

<img src="20-inference_for_paired_data-web_files/figure-html/unnamed-chunk-11-1.png" width="672" />

### Calculate the P-value.


```r
price_test_p <- price_test %>%
  get_p_value(obs_stat = d_t, direction = "greater")
price_test_p
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["p_value"],"name":[1],"type":["dbl"],"align":["right"]}],"data":[{"1":"0.00000000003463791"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

### Interpret the P-value as a probability given the null.

$P < 0.001$. If there were no difference in textbook prices between the UCLA bookstore and Amazon, there is only a 0% chance of seeing data at least as extreme as what we saw. (Note that the number is so small that it rounds to zero in the inline code above. That zero is technically incorrect. The P-value is never exactly zero. That's why why also are clear to state $P < 0.001$.)


## Conclusion

### State the statistical conclusion.

We reject the null hypothesis.

### State (but do not overstate) a contextually meaningful conclusion.

We have sufficient evidence that UCLA prices are higher than Amazon prices.

Commentary: Note that because we performed a one-sided test, our conclusion is also one-sided in the hypothesized direction.

### Express reservations or uncertainty about the generalizability of the conclusion.

We can be confident about the validity of this data, and therefore the conclusion drawn. We should be careful to limit our conclusion to the UCLA bookstore (and not extrapolate the findings, say, to other campus bookstores.) Depending on when this data was collected, we may not be able to say anything about current prices at the UCLA bookstore either.

### Identify the possibility of either a Type I or Type II error and state what making such an error means in the context of the hypotheses.

If we made a Type I error, that would mean there was actually no difference in textbook prices, but that we got an unusual sample that detected a difference.


## Confidence interval

### Check the relevant conditions to ensure that model assumptions are met.

All necessary conditions have already been checked.

### Calculate and graph the confidence interval.


```r
price_ci <- price_test %>%
  get_confidence_interval(point_estimate = d_mean, level = 0.95)
price_ci
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["lower_ci"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["upper_ci"],"name":[2],"type":["dbl"],"align":["right"]}],"data":[{"1":"9.435636","2":"16.08765"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


```r
price_test %>%
  visualize() +
  shade_confidence_interval(endpoints = price_ci)
```

<img src="20-inference_for_paired_data-web_files/figure-html/unnamed-chunk-14-1.png" width="672" />

### State (but do not overstate) a contextually meaningful interpretation.

We are 95% confident that the true difference in textbook prices between the UCLA bookstore and Amazon is captured in the interval (9.4356361, 16.0876516). This was obtained by subtracting the Amazon price minus the UCLA bookstore. (In other words, since all differences in the confidence interval are positive, all plausible differences indicate that the UCLA prices are higher than the Amazon prices.)


Commentary: Don't forget that any time we find a number that represents a difference, we have to be clear in the conclusion about the direction of subtraction. Otherwise, we have no idea how to interpret positive and negative values.

### If running a two-sided test, explain how the confidence interval reinforces the conclusion of the hypothesis test.

The confidence interval does not contain zero, which means that zero is not a plausible value for the difference textbook prices.

### When comparing two groups, comment on the effect size and the practical significance of the result.

To think about the practical significance, imagine that you were a student at UCLA and that every textbook you needed was (on average) $10 to $15 more expensive in the bookstore than purchasing on Amazon. Multiplied across the number of textbooks you need, that could amount to a significant increase in expenses. In other words, that dollar figure is not likely a trivial amount of money for many students who require multiple textbooks each semester.


## Your turn

The `hsb2` data set contains data from a random sample of 200 high school seniors from the "High School and Beyond" survey conducted by the National Center of Education Statistics. It contains, among other things, students' scores on standardized tests in math, reading, writing, science, and social studies. We want to know if students do better on the math test or on the reading test.

Run inference to determine if there is a difference between math scores and reading scores.

The rubric outline is reproduced below. You may refer to the worked example above and modify it accordingly. Remember to strip out all the commentary. That is just exposition for your benefit in understanding the steps, but is not meant to form part of the formal inference process.

Another word of warning: the copy/paste process is not a substitute for your brain. You will often need to modify more than just the names of the data frames and variables to adapt the worked examples to your own work. Do not blindly copy and paste code without understanding what it does. And you should **never** copy and paste text. All the sentences and paragraphs you write are expressions of your own analysis. They must reflect your own understanding of the inferential process.

**Also, so that your answers here don't mess up the code chunks above, use new variable names everywhere.**


##### Exploratory data analysis {-}

###### Use data documentation (help files, code books, Google, etc.) to determine as much as possible about the data provenance and structure. {-}

::: {.answer}

Please write up your answer here


```r
# Add code here to print the data
```


```r
# Add code here to glimpse the variables
```

:::

###### Prepare the data for analysis. [Not always necessary.] {-}

::: {.answer}


```r
# Add code here to prepare the data for analysis.
```

:::

###### Make tables or plots to explore the data visually. {-}

::: {.answer}


```r
# Add code here to make tables or plots.
```

:::


##### Hypotheses {-}

###### Identify the sample (or samples) and a reasonable population (or populations) of interest. {-}

::: {.answer}

Please write up your answer here.

:::

###### Express the null and alternative hypotheses as contextually meaningful full sentences. {-}

::: {.answer}

$H_{0}:$ Null hypothesis goes here.

$H_{A}:$ Alternative hypothesis goes here.

:::

###### Express the null and alternative hypotheses in symbols (when possible). {-}

::: {.answer}

$H_{0}: math$

$H_{A}: math$

:::


##### Model {-}

###### Identify the sampling distribution model. {-}

::: {.answer}

Please write up your answer here.

:::

###### Check the relevant conditions to ensure that model assumptions are met. {-}

::: {.answer}

Please write up your answer here. (Some conditions may require R code as well.)

:::


##### Mechanics {-}

###### Compute the test statistic. {-}

::: {.answer}


```r
# Add code here to compute the test statistic.
```

:::

###### Report the test statistic in context (when possible). {-}

::: {.answer}

Please write up your answer here.

:::

###### Plot the null distribution. {-}

::: {.answer}


```r
# IF CONDUCTING A SIMULATION...
set.seed(1)
# Add code here to simulate the null distribution.
```


```r
# Add code here to plot the null distribution.
```

:::

###### Calculate the P-value. {-}

::: {.answer}


```r
# Add code here to calculate the P-value.
```

:::

###### Interpret the P-value as a probability given the null. {-}

::: {.answer}

Please write up your answer here.

:::


##### Conclusion {-}

###### State the statistical conclusion. {-}

::: {.answer}

Please write up your answer here.

:::

###### State (but do not overstate) a contextually meaningful conclusion. {-}

::: {.answer}

Please write up your answer here.

:::

###### Express reservations or uncertainty about the generalizability of the conclusion. {-}

::: {.answer}

Please write up your answer here.

:::

###### Identify the possibility of either a Type I or Type II error and state what making such an error means in the context of the hypotheses. {-}

::: {.answer}

Please write up your answer here.

:::

##### Confidence interval {-}

###### Check the relevant conditions to ensure that model assumptions are met. {-}

::: {.answer}

Please write up your answer here. (Some conditions may require R code as well.)

:::

###### Calculate and graph the confidence interval. {-}

::: {.answer}


```r
# Add code here to calculate the confidence interval.
```


```r
# Add code here to graph the confidence interval.
```

:::

###### State (but do not overstate) a contextually meaningful interpretation. {-}

::: {.answer}

Please write up your answer here.

:::

###### If running a two-sided test, explain how the confidence interval reinforces the conclusion of the hypothesis test. [Not always applicable.] {-}

::: {.answer}

Please write up your answer here.

:::

###### When comparing two groups, comment on the effect size and the practical significance of the result. [Not always applicable.] {-}

::: {.answer}

Please write up your answer here.

:::


## Conclusion

Paired data occurs whenever we have two numerical measurements that are related to each other, whether because they come from the same observational units or from closely related ones. When our data is structured as pairs of measurements in this way, we can subtract the two columns and obtain a difference. That difference variable is the object of our study, and now that it is represented as a single numerical variable, we can apply the one-sample t test from the last chapter.

### Preparing and submitting your assignment

1. From the "Run" menu, select "Restart R and Run All Chunks".
2. Deal with any code errors that crop up. Repeat steps 1–-2 until there are no more code errors.
3. Spell check your document by clicking the icon with "ABC" and a check mark.
4. Hit the "Preview" button one last time to generate the final draft of the `.nb.html` file.
5. Proofread the HTML file carefully. If there are errors, go back and fix them, then repeat steps 1--5 again.

If you have completed this chapter as part of a statistics course, follow the directions you receive from your professor to submit your assignment.
