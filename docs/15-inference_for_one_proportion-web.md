# Inference for one proportion {#one-prop}

<!-- Please don't mess with the next few lines! -->
<style>h5{font-size:2em;color:#0000FF}h6{font-size:1.5em;color:#0000FF}div.answer{margin-left:5%;border:1px solid #0000FF;border-left-width:10px;padding:25px} div.summary{background-color:rgba(30,144,255,0.1);border:3px double #0000FF;padding:25px}</style><p style="color:#ffffff">2.0</p>
<!-- Please don't mess with the previous few lines! -->


::: {.summary}

### Functions introduced in this chapter {-}

No new R functions are introduced here.

:::


## Introduction {#one-prop-intro}

Our earlier work with simulations showed us that when the number of successes and failures is large enough, we can use a normal model as our sampling distribution model.

We revisit hypothesis tests for a single proportion, but now, instead of running a simulation to compute the P-value, we take the shortcut of computing the P-value directly from a normal model.

There are no new concepts here. All we are doing is revisiting the rubric for inference and making the necessary changes.

### Install new packages {#one-prop-install}

There are no new packages used in this chapter.

### Download the R notebook file {#one-prop-download}

Check the upper-right corner in RStudio to make sure you're in your `intro_stats` project. Then click on the following link to download this chapter as an R notebook file (`.Rmd`).

<a href = "https://vectorposse.github.io/intro_stats/chapter_downloads/15-inference_for_one_proportion.Rmd" download>https://vectorposse.github.io/intro_stats/chapter_downloads/15-inference_for_one_proportion.Rmd</a>

Once the file is downloaded, move it to your project folder in RStudio and open it there.

### Restart R and run all chunks {#one-prop-restart}

In RStudio, select "Restart R and Run All Chunks" from the "Run" menu.


## Load packages {#one-prop-load}

We load the standard `tidyverse`, `janitor` and `infer` packages as well as the `openintro` package to access data on heart transplant candidates. We'll include `mosaic` for one spot below when we compare the results of `infer` to the results of graphing a normal distribution using `qdist`.


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
library(janitor)
```

```
## 
## Attaching package: 'janitor'
## 
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
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

```r
library(mosaic)
```

```
## Warning: package 'mosaic' was built under R version 4.3.1
```

```
## Registered S3 method overwritten by 'mosaic':
##   method                           from   
##   fortify.SpatialPolygonsDataFrame ggplot2
## 
## The 'mosaic' package masks several functions from core packages in order to add 
## additional features.  The original behavior of these functions should not be affected by this.
## 
## Attaching package: 'mosaic'
## 
## The following object is masked from 'package:Matrix':
## 
##     mean
## 
## The following object is masked from 'package:openintro':
## 
##     dotPlot
## 
## The following objects are masked from 'package:infer':
## 
##     prop_test, t_test
## 
## The following objects are masked from 'package:dplyr':
## 
##     count, do, tally
## 
## The following object is masked from 'package:purrr':
## 
##     cross
## 
## The following object is masked from 'package:ggplot2':
## 
##     stat
## 
## The following objects are masked from 'package:stats':
## 
##     binom.test, cor, cor.test, cov, fivenum, IQR, median, prop.test,
##     quantile, sd, t.test, var
## 
## The following objects are masked from 'package:base':
## 
##     max, mean, min, prod, range, sample, sum
```


## Revisiting the rubric for inference {#one-prop-rubric}

Instead of running a simulation, we are going to assume that the sampling distribution can be modeled with a normal model as long as the conditions for using a normal model are met.

Although the rubric has not changed, the use of a normal model changes quite a bit about the way we go through the other steps. For example, we won't have simulated values to give us a histogram of the null model. Instead, we'll go straight to graphing a normal model. We won't compute the percent of our simulated samples that are at least as extreme as our test statistic to get the P-value. The P-value from a normal model is found directly from shading the model.

What follows is a fully-worked example of inference for one proportion. After the hypothesis test (sometimes called a one-proportion z-test for reasons that will become clear), we also follow up by computing a confidence interval. **From now on, we will consider inference to consist of a hypothesis test and a confidence interval.** Whenever you're asked a question that requires statistical inference, you should follow both the rubric steps for a hypothesis test and for a confidence interval.

The example below will pause frequently for commentary on the steps, especially where their execution will be different from what you've seen before when you used simulation. When it's your turn to work through another example on your own, you should follow the outline of the rubric, but you should **not** copy and paste the commentary that accompanies it.


## Research question {#one-prop-question}

Data from the Stanford University Heart Transplant Study is located in the `openintro` package in a data frame called `heart_transplant`. From the help file we learn, "Each patient entering the program was designated officially a heart transplant candidate, meaning that he was gravely ill and would most likely benefit from a new heart." Survival rates are not good for this population, although they are better for those who receive a heart transplant. Do heart transplant recipients still have less than a 50% chance of survival?


## Exploratory data analysis {#one-prop-ex-eda}

### Use data documentation (help files, code books, Google, etc.) to determine as much as possible about the data provenance and structure. {#one-prop-ex-documentation}

Start by typing `?heart_transplant` at the Console or searching for `heart_translplant` in the Help tab to read the help file.

##### Exercise 1 {-}

Click on the link under "Source" in the help file. Why is this not helpful for determining the provenance of the data?

Now try to do an internet search to find the original research article from 1974. Why is this search process also not likely to help you determine the provenance of the data?

::: {.answer}

Please write up your answer here.

:::

*****

Now that we have learned everything we can reasonably learn about the data, we print it out and look at the variables.


```r
heart_transplant
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["id"],"name":[1],"type":["int"],"align":["right"]},{"label":["acceptyear"],"name":[2],"type":["int"],"align":["right"]},{"label":["age"],"name":[3],"type":["int"],"align":["right"]},{"label":["survived"],"name":[4],"type":["fct"],"align":["left"]},{"label":["survtime"],"name":[5],"type":["int"],"align":["right"]},{"label":["prior"],"name":[6],"type":["fct"],"align":["left"]},{"label":["transplant"],"name":[7],"type":["fct"],"align":["left"]},{"label":["wait"],"name":[8],"type":["int"],"align":["right"]}],"data":[{"1":"15","2":"68","3":"53","4":"dead","5":"1","6":"no","7":"control","8":"NA"},{"1":"43","2":"70","3":"43","4":"dead","5":"2","6":"no","7":"control","8":"NA"},{"1":"61","2":"71","3":"52","4":"dead","5":"2","6":"no","7":"control","8":"NA"},{"1":"75","2":"72","3":"52","4":"dead","5":"2","6":"no","7":"control","8":"NA"},{"1":"6","2":"68","3":"54","4":"dead","5":"3","6":"no","7":"control","8":"NA"},{"1":"42","2":"70","3":"36","4":"dead","5":"3","6":"no","7":"control","8":"NA"},{"1":"54","2":"71","3":"47","4":"dead","5":"3","6":"no","7":"control","8":"NA"},{"1":"38","2":"70","3":"41","4":"dead","5":"5","6":"no","7":"treatment","8":"5"},{"1":"85","2":"73","3":"47","4":"dead","5":"5","6":"no","7":"control","8":"NA"},{"1":"2","2":"68","3":"51","4":"dead","5":"6","6":"no","7":"control","8":"NA"},{"1":"103","2":"67","3":"39","4":"dead","5":"6","6":"no","7":"control","8":"NA"},{"1":"12","2":"68","3":"53","4":"dead","5":"8","6":"no","7":"control","8":"NA"},{"1":"48","2":"71","3":"56","4":"dead","5":"9","6":"no","7":"control","8":"NA"},{"1":"102","2":"74","3":"40","4":"alive","5":"11","6":"no","7":"control","8":"NA"},{"1":"35","2":"70","3":"43","4":"dead","5":"12","6":"no","7":"control","8":"NA"},{"1":"95","2":"73","3":"40","4":"dead","5":"16","6":"no","7":"treatment","8":"2"},{"1":"31","2":"69","3":"54","4":"dead","5":"16","6":"no","7":"control","8":"NA"},{"1":"3","2":"68","3":"54","4":"dead","5":"16","6":"no","7":"treatment","8":"1"},{"1":"74","2":"72","3":"29","4":"dead","5":"17","6":"no","7":"treatment","8":"5"},{"1":"5","2":"68","3":"20","4":"dead","5":"18","6":"no","7":"control","8":"NA"},{"1":"77","2":"72","3":"41","4":"dead","5":"21","6":"no","7":"control","8":"NA"},{"1":"99","2":"73","3":"49","4":"dead","5":"21","6":"no","7":"control","8":"NA"},{"1":"20","2":"69","3":"55","4":"dead","5":"28","6":"no","7":"treatment","8":"1"},{"1":"70","2":"72","3":"52","4":"dead","5":"30","6":"no","7":"treatment","8":"5"},{"1":"101","2":"74","3":"49","4":"alive","5":"31","6":"no","7":"control","8":"NA"},{"1":"66","2":"72","3":"53","4":"dead","5":"32","6":"no","7":"control","8":"NA"},{"1":"29","2":"69","3":"50","4":"dead","5":"35","6":"no","7":"control","8":"NA"},{"1":"17","2":"68","3":"20","4":"dead","5":"36","6":"no","7":"control","8":"NA"},{"1":"19","2":"68","3":"59","4":"dead","5":"37","6":"no","7":"control","8":"NA"},{"1":"4","2":"68","3":"40","4":"dead","5":"39","6":"no","7":"treatment","8":"36"},{"1":"100","2":"74","3":"35","4":"alive","5":"39","6":"yes","7":"treatment","8":"38"},{"1":"8","2":"68","3":"45","4":"dead","5":"40","6":"no","7":"control","8":"NA"},{"1":"44","2":"70","3":"42","4":"dead","5":"40","6":"no","7":"control","8":"NA"},{"1":"16","2":"68","3":"56","4":"dead","5":"43","6":"no","7":"treatment","8":"20"},{"1":"45","2":"71","3":"36","4":"dead","5":"45","6":"no","7":"treatment","8":"1"},{"1":"1","2":"67","3":"30","4":"dead","5":"50","6":"no","7":"control","8":"NA"},{"1":"22","2":"69","3":"42","4":"dead","5":"51","6":"no","7":"treatment","8":"12"},{"1":"39","2":"70","3":"50","4":"dead","5":"53","6":"no","7":"treatment","8":"2"},{"1":"10","2":"68","3":"42","4":"dead","5":"58","6":"no","7":"treatment","8":"12"},{"1":"35","2":"71","3":"52","4":"dead","5":"61","6":"no","7":"treatment","8":"10"},{"1":"37","2":"70","3":"61","4":"dead","5":"66","6":"no","7":"treatment","8":"19"},{"1":"68","2":"72","3":"45","4":"dead","5":"68","6":"no","7":"treatment","8":"3"},{"1":"60","2":"71","3":"49","4":"dead","5":"68","6":"no","7":"treatment","8":"3"},{"1":"62","2":"71","3":"39","4":"dead","5":"69","6":"no","7":"control","8":"NA"},{"1":"28","2":"69","3":"53","4":"dead","5":"72","6":"no","7":"treatment","8":"71"},{"1":"47","2":"71","3":"47","4":"dead","5":"72","6":"no","7":"treatment","8":"21"},{"1":"32","2":"69","3":"64","4":"dead","5":"77","6":"no","7":"treatment","8":"17"},{"1":"65","2":"72","3":"51","4":"dead","5":"78","6":"no","7":"treatment","8":"12"},{"1":"83","2":"73","3":"53","4":"dead","5":"80","6":"no","7":"treatment","8":"32"},{"1":"13","2":"68","3":"54","4":"dead","5":"81","6":"no","7":"treatment","8":"17"},{"1":"9","2":"68","3":"47","4":"dead","5":"85","6":"no","7":"control","8":"NA"},{"1":"73","2":"72","3":"56","4":"dead","5":"90","6":"no","7":"treatment","8":"27"},{"1":"79","2":"72","3":"53","4":"dead","5":"96","6":"no","7":"treatment","8":"67"},{"1":"36","2":"70","3":"48","4":"dead","5":"100","6":"no","7":"treatment","8":"46"},{"1":"32","2":"71","3":"41","4":"dead","5":"102","6":"no","7":"control","8":"NA"},{"1":"98","2":"73","3":"28","4":"alive","5":"109","6":"no","7":"treatment","8":"96"},{"1":"87","2":"73","3":"46","4":"dead","5":"110","6":"no","7":"treatment","8":"60"},{"1":"97","2":"73","3":"23","4":"alive","5":"131","6":"no","7":"treatment","8":"21"},{"1":"37","2":"71","3":"41","4":"dead","5":"149","6":"no","7":"control","8":"NA"},{"1":"11","2":"68","3":"47","4":"dead","5":"153","6":"no","7":"treatment","8":"26"},{"1":"94","2":"73","3":"43","4":"dead","5":"165","6":"yes","7":"treatment","8":"4"},{"1":"96","2":"73","3":"26","4":"alive","5":"180","6":"no","7":"treatment","8":"13"},{"1":"90","2":"73","3":"52","4":"dead","5":"186","6":"yes","7":"treatment","8":"160"},{"1":"53","2":"71","3":"47","4":"dead","5":"188","6":"no","7":"treatment","8":"41"},{"1":"89","2":"73","3":"51","4":"dead","5":"207","6":"no","7":"treatment","8":"139"},{"1":"24","2":"69","3":"51","4":"dead","5":"219","6":"no","7":"treatment","8":"83"},{"1":"27","2":"69","3":"8","4":"dead","5":"263","6":"no","7":"control","8":"NA"},{"1":"93","2":"73","3":"47","4":"alive","5":"265","6":"no","7":"treatment","8":"28"},{"1":"51","2":"71","3":"48","4":"dead","5":"285","6":"no","7":"treatment","8":"32"},{"1":"67","2":"73","3":"19","4":"dead","5":"285","6":"no","7":"treatment","8":"57"},{"1":"16","2":"68","3":"49","4":"dead","5":"308","6":"no","7":"treatment","8":"28"},{"1":"84","2":"73","3":"42","4":"dead","5":"334","6":"no","7":"treatment","8":"37"},{"1":"91","2":"73","3":"47","4":"dead","5":"340","6":"no","7":"control","8":"NA"},{"1":"92","2":"73","3":"44","4":"alive","5":"340","6":"no","7":"treatment","8":"310"},{"1":"58","2":"71","3":"47","4":"dead","5":"342","6":"yes","7":"treatment","8":"21"},{"1":"88","2":"73","3":"54","4":"alive","5":"370","6":"no","7":"treatment","8":"31"},{"1":"86","2":"73","3":"48","4":"alive","5":"397","6":"no","7":"treatment","8":"8"},{"1":"82","2":"71","3":"29","4":"alive","5":"427","6":"no","7":"control","8":"NA"},{"1":"81","2":"73","3":"52","4":"alive","5":"445","6":"no","7":"treatment","8":"6"},{"1":"80","2":"72","3":"46","4":"alive","5":"482","6":"yes","7":"treatment","8":"26"},{"1":"78","2":"72","3":"48","4":"alive","5":"515","6":"no","7":"treatment","8":"210"},{"1":"76","2":"72","3":"52","4":"alive","5":"545","6":"yes","7":"treatment","8":"46"},{"1":"64","2":"72","3":"48","4":"dead","5":"583","6":"yes","7":"treatment","8":"32"},{"1":"72","2":"72","3":"26","4":"alive","5":"596","6":"no","7":"treatment","8":"4"},{"1":"71","2":"72","3":"47","4":"alive","5":"630","6":"no","7":"treatment","8":"31"},{"1":"69","2":"72","3":"47","4":"alive","5":"670","6":"no","7":"treatment","8":"10"},{"1":"7","2":"68","3":"50","4":"dead","5":"675","6":"no","7":"treatment","8":"51"},{"1":"23","2":"69","3":"58","4":"dead","5":"733","6":"no","7":"treatment","8":"3"},{"1":"63","2":"71","3":"32","4":"alive","5":"841","6":"no","7":"treatment","8":"27"},{"1":"30","2":"69","3":"44","4":"dead","5":"852","6":"no","7":"treatment","8":"16"},{"1":"59","2":"71","3":"41","4":"alive","5":"915","6":"no","7":"treatment","8":"78"},{"1":"56","2":"71","3":"38","4":"alive","5":"941","6":"no","7":"treatment","8":"67"},{"1":"50","2":"71","3":"45","4":"dead","5":"979","6":"yes","7":"treatment","8":"83"},{"1":"46","2":"71","3":"48","4":"dead","5":"995","6":"yes","7":"treatment","8":"2"},{"1":"21","2":"69","3":"43","4":"dead","5":"1032","6":"no","7":"treatment","8":"8"},{"1":"49","2":"71","3":"36","4":"alive","5":"1141","6":"yes","7":"treatment","8":"36"},{"1":"41","2":"70","3":"45","4":"alive","5":"1321","6":"yes","7":"treatment","8":"58"},{"1":"14","2":"68","3":"53","4":"dead","5":"1386","6":"no","7":"treatment","8":"37"},{"1":"26","2":"69","3":"30","4":"alive","5":"1400","6":"no","7":"control","8":"NA"},{"1":"40","2":"70","3":"48","4":"alive","5":"1407","6":"yes","7":"treatment","8":"41"},{"1":"34","2":"69","3":"40","4":"alive","5":"1571","6":"no","7":"treatment","8":"23"},{"1":"33","2":"69","3":"48","4":"alive","5":"1586","6":"no","7":"treatment","8":"51"},{"1":"25","2":"69","3":"33","4":"alive","5":"1799","6":"no","7":"treatment","8":"25"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


```r
glimpse(heart_transplant)
```

```
## Rows: 103
## Columns: 8
## $ id         <int> 15, 43, 61, 75, 6, 42, 54, 38, 85, 2, 103, 12, 48, 102, 35,…
## $ acceptyear <int> 68, 70, 71, 72, 68, 70, 71, 70, 73, 68, 67, 68, 71, 74, 70,…
## $ age        <int> 53, 43, 52, 52, 54, 36, 47, 41, 47, 51, 39, 53, 56, 40, 43,…
## $ survived   <fct> dead, dead, dead, dead, dead, dead, dead, dead, dead, dead,…
## $ survtime   <int> 1, 2, 2, 2, 3, 3, 3, 5, 5, 6, 6, 8, 9, 11, 12, 16, 16, 16, …
## $ prior      <fct> no, no, no, no, no, no, no, no, no, no, no, no, no, no, no,…
## $ transplant <fct> control, control, control, control, control, control, contr…
## $ wait       <int> NA, NA, NA, NA, NA, NA, NA, 5, NA, NA, NA, NA, NA, NA, NA, …
```

Commentary: The variable of interest is `survived`, which is coded as a factor variable with two categories, "alive" and "dead". Keep in mind that because we are interested in survival rates, the "alive" condition will be considered the "success" condition.

There are 103 patients, but we are not considering all these patients. Our sample should consist of only those patients who actually received the transplant. The following table shows that only 69 patients were in the "treatment" group (meaning that they received a heart transplant).


```r
tabyl(heart_transplant, transplant) %>%
    adorn_totals()
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["transplant"],"name":[1],"type":["fct"],"align":["left"]},{"label":["n"],"name":[2],"type":["int"],"align":["right"]},{"label":["percent"],"name":[3],"type":["dbl"],"align":["right"]}],"data":[{"1":"control","2":"34","3":"0.3300971","_rn_":"1"},{"1":"treatment","2":"69","3":"0.6699029","_rn_":"2"},{"1":"Total","2":"103","3":"1.0000000","_rn_":"3"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

### Prepare the data for analysis. {#one-prop-ex-prepare}

**CAUTION: If you are copying and pasting from this example to use for another research question, the following code chunk is specific to this research question and not applicable in other contexts.**

We need to use `filter` so we get only the patients who actually received the heart transplant.


```r
# Do not copy and paste this code for future work
heart_transplant2 <- heart_transplant %>%
    filter(transplant == "treatment")
heart_transplant2
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["id"],"name":[1],"type":["int"],"align":["right"]},{"label":["acceptyear"],"name":[2],"type":["int"],"align":["right"]},{"label":["age"],"name":[3],"type":["int"],"align":["right"]},{"label":["survived"],"name":[4],"type":["fct"],"align":["left"]},{"label":["survtime"],"name":[5],"type":["int"],"align":["right"]},{"label":["prior"],"name":[6],"type":["fct"],"align":["left"]},{"label":["transplant"],"name":[7],"type":["fct"],"align":["left"]},{"label":["wait"],"name":[8],"type":["int"],"align":["right"]}],"data":[{"1":"38","2":"70","3":"41","4":"dead","5":"5","6":"no","7":"treatment","8":"5"},{"1":"95","2":"73","3":"40","4":"dead","5":"16","6":"no","7":"treatment","8":"2"},{"1":"3","2":"68","3":"54","4":"dead","5":"16","6":"no","7":"treatment","8":"1"},{"1":"74","2":"72","3":"29","4":"dead","5":"17","6":"no","7":"treatment","8":"5"},{"1":"20","2":"69","3":"55","4":"dead","5":"28","6":"no","7":"treatment","8":"1"},{"1":"70","2":"72","3":"52","4":"dead","5":"30","6":"no","7":"treatment","8":"5"},{"1":"4","2":"68","3":"40","4":"dead","5":"39","6":"no","7":"treatment","8":"36"},{"1":"100","2":"74","3":"35","4":"alive","5":"39","6":"yes","7":"treatment","8":"38"},{"1":"16","2":"68","3":"56","4":"dead","5":"43","6":"no","7":"treatment","8":"20"},{"1":"45","2":"71","3":"36","4":"dead","5":"45","6":"no","7":"treatment","8":"1"},{"1":"22","2":"69","3":"42","4":"dead","5":"51","6":"no","7":"treatment","8":"12"},{"1":"39","2":"70","3":"50","4":"dead","5":"53","6":"no","7":"treatment","8":"2"},{"1":"10","2":"68","3":"42","4":"dead","5":"58","6":"no","7":"treatment","8":"12"},{"1":"35","2":"71","3":"52","4":"dead","5":"61","6":"no","7":"treatment","8":"10"},{"1":"37","2":"70","3":"61","4":"dead","5":"66","6":"no","7":"treatment","8":"19"},{"1":"68","2":"72","3":"45","4":"dead","5":"68","6":"no","7":"treatment","8":"3"},{"1":"60","2":"71","3":"49","4":"dead","5":"68","6":"no","7":"treatment","8":"3"},{"1":"28","2":"69","3":"53","4":"dead","5":"72","6":"no","7":"treatment","8":"71"},{"1":"47","2":"71","3":"47","4":"dead","5":"72","6":"no","7":"treatment","8":"21"},{"1":"32","2":"69","3":"64","4":"dead","5":"77","6":"no","7":"treatment","8":"17"},{"1":"65","2":"72","3":"51","4":"dead","5":"78","6":"no","7":"treatment","8":"12"},{"1":"83","2":"73","3":"53","4":"dead","5":"80","6":"no","7":"treatment","8":"32"},{"1":"13","2":"68","3":"54","4":"dead","5":"81","6":"no","7":"treatment","8":"17"},{"1":"73","2":"72","3":"56","4":"dead","5":"90","6":"no","7":"treatment","8":"27"},{"1":"79","2":"72","3":"53","4":"dead","5":"96","6":"no","7":"treatment","8":"67"},{"1":"36","2":"70","3":"48","4":"dead","5":"100","6":"no","7":"treatment","8":"46"},{"1":"98","2":"73","3":"28","4":"alive","5":"109","6":"no","7":"treatment","8":"96"},{"1":"87","2":"73","3":"46","4":"dead","5":"110","6":"no","7":"treatment","8":"60"},{"1":"97","2":"73","3":"23","4":"alive","5":"131","6":"no","7":"treatment","8":"21"},{"1":"11","2":"68","3":"47","4":"dead","5":"153","6":"no","7":"treatment","8":"26"},{"1":"94","2":"73","3":"43","4":"dead","5":"165","6":"yes","7":"treatment","8":"4"},{"1":"96","2":"73","3":"26","4":"alive","5":"180","6":"no","7":"treatment","8":"13"},{"1":"90","2":"73","3":"52","4":"dead","5":"186","6":"yes","7":"treatment","8":"160"},{"1":"53","2":"71","3":"47","4":"dead","5":"188","6":"no","7":"treatment","8":"41"},{"1":"89","2":"73","3":"51","4":"dead","5":"207","6":"no","7":"treatment","8":"139"},{"1":"24","2":"69","3":"51","4":"dead","5":"219","6":"no","7":"treatment","8":"83"},{"1":"93","2":"73","3":"47","4":"alive","5":"265","6":"no","7":"treatment","8":"28"},{"1":"51","2":"71","3":"48","4":"dead","5":"285","6":"no","7":"treatment","8":"32"},{"1":"67","2":"73","3":"19","4":"dead","5":"285","6":"no","7":"treatment","8":"57"},{"1":"16","2":"68","3":"49","4":"dead","5":"308","6":"no","7":"treatment","8":"28"},{"1":"84","2":"73","3":"42","4":"dead","5":"334","6":"no","7":"treatment","8":"37"},{"1":"92","2":"73","3":"44","4":"alive","5":"340","6":"no","7":"treatment","8":"310"},{"1":"58","2":"71","3":"47","4":"dead","5":"342","6":"yes","7":"treatment","8":"21"},{"1":"88","2":"73","3":"54","4":"alive","5":"370","6":"no","7":"treatment","8":"31"},{"1":"86","2":"73","3":"48","4":"alive","5":"397","6":"no","7":"treatment","8":"8"},{"1":"81","2":"73","3":"52","4":"alive","5":"445","6":"no","7":"treatment","8":"6"},{"1":"80","2":"72","3":"46","4":"alive","5":"482","6":"yes","7":"treatment","8":"26"},{"1":"78","2":"72","3":"48","4":"alive","5":"515","6":"no","7":"treatment","8":"210"},{"1":"76","2":"72","3":"52","4":"alive","5":"545","6":"yes","7":"treatment","8":"46"},{"1":"64","2":"72","3":"48","4":"dead","5":"583","6":"yes","7":"treatment","8":"32"},{"1":"72","2":"72","3":"26","4":"alive","5":"596","6":"no","7":"treatment","8":"4"},{"1":"71","2":"72","3":"47","4":"alive","5":"630","6":"no","7":"treatment","8":"31"},{"1":"69","2":"72","3":"47","4":"alive","5":"670","6":"no","7":"treatment","8":"10"},{"1":"7","2":"68","3":"50","4":"dead","5":"675","6":"no","7":"treatment","8":"51"},{"1":"23","2":"69","3":"58","4":"dead","5":"733","6":"no","7":"treatment","8":"3"},{"1":"63","2":"71","3":"32","4":"alive","5":"841","6":"no","7":"treatment","8":"27"},{"1":"30","2":"69","3":"44","4":"dead","5":"852","6":"no","7":"treatment","8":"16"},{"1":"59","2":"71","3":"41","4":"alive","5":"915","6":"no","7":"treatment","8":"78"},{"1":"56","2":"71","3":"38","4":"alive","5":"941","6":"no","7":"treatment","8":"67"},{"1":"50","2":"71","3":"45","4":"dead","5":"979","6":"yes","7":"treatment","8":"83"},{"1":"46","2":"71","3":"48","4":"dead","5":"995","6":"yes","7":"treatment","8":"2"},{"1":"21","2":"69","3":"43","4":"dead","5":"1032","6":"no","7":"treatment","8":"8"},{"1":"49","2":"71","3":"36","4":"alive","5":"1141","6":"yes","7":"treatment","8":"36"},{"1":"41","2":"70","3":"45","4":"alive","5":"1321","6":"yes","7":"treatment","8":"58"},{"1":"14","2":"68","3":"53","4":"dead","5":"1386","6":"no","7":"treatment","8":"37"},{"1":"40","2":"70","3":"48","4":"alive","5":"1407","6":"yes","7":"treatment","8":"41"},{"1":"34","2":"69","3":"40","4":"alive","5":"1571","6":"no","7":"treatment","8":"23"},{"1":"33","2":"69","3":"48","4":"alive","5":"1586","6":"no","7":"treatment","8":"51"},{"1":"25","2":"69","3":"33","4":"alive","5":"1799","6":"no","7":"treatment","8":"25"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

Commentary: don't forget the double equal sign (`==`) that checks whether the `treatment` variable is equal to the value "treatment". (See the Chapter 5 if you've forgotten how to use `filter`.)

Again, this step isn't something you need to do for other research questions. This question is peculiar because it asks only about patients who received a heart transplant, and that only involves a subset of the data we have in the `heart_transplant` data frame.

### Make tables or plots to explore the data visually. {#one-prop-ex-plots}

Making sure that we refer from now on to the `heart_transplant2` data frame and not the original `heart_transplant` data frame:


```r
tabyl(heart_transplant2, survived) %>%
    adorn_totals()
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["survived"],"name":[1],"type":["fct"],"align":["left"]},{"label":["n"],"name":[2],"type":["int"],"align":["right"]},{"label":["percent"],"name":[3],"type":["dbl"],"align":["right"]}],"data":[{"1":"alive","2":"24","3":"0.3478261","_rn_":"1"},{"1":"dead","2":"45","3":"0.6521739","_rn_":"2"},{"1":"Total","2":"69","3":"1.0000000","_rn_":"3"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


## Hypotheses {#one-prop-ex-hypotheses}

### Identify the sample (or samples) and a reasonable population (or populations) of interest. {#one-prop-ex-sample-pop}

The sample consists of 69 heart transplant recipients in a study at Stanford University. The population of interest is presumably all heart transplants recipients.

### Express the null and alternative hypotheses as contextually meaningful full sentences. {#one-prop-ex-express-words}

$H_{0}:$ Heart transplant recipients have a 50% chance of survival.

$H_{A}:$ Heart transplant recipients have less than a 50% chance of survival.

Commentary: It is slightly unusual that we are conducting a one-sided test. The standard default is typically a two-sided test. However, it is not for us to choose: the proposed research question is unequivocal in hypothesizing "less than 50%" survival.

### Express the null and alternative hypotheses in symbols (when possible). {#one-prop-ex-express-math}

$H_{0}: p_{alive} = 0.5$

$H_{A}: p_{alive} < 0.5$


## Model {#one-prop-ex-model}

### Identify the sampling distribution model. {#one-prop-ex-sampling-dist-model}

We will use a normal model.

Commentary: In past chapters, we have simulated the sampling distribution or applied some kind of randomization to simulate the effect of the null hypothesis. The point of this chapter is that we can---when the conditions are met---substitute a normal model to replace the unimodal and symmetric histogram that resulted from randomization and simulation.

### Check the relevant conditions to ensure that model assumptions are met. {#one-prop-ex-ht-conditions}

* Random
    - Since the 69 patients are from a study at Stanford, we do not have a random sample of all heart transplant recipients. We hope that the patients recruited to this study were physiologically similar to other heart patients so that they are a representative sample. Without more information, we have no real way of knowing.

* 10%
    - 69 patients are definitely less than 10% of all heart transplant recipients.

* Success/failure

$$
np_{alive} = 69(0.5) = 34.5 \geq 10
$$

$$
n(1 - p_{alive}) = 69(0.5) = 34.5 \geq 10
$$

Commentary: Notice something interesting here. Why did we not use the 24 patients who survived and the 45 who died as the successes and failures? In other words, why did we use $np_{alive}$ and $n(1 - p_{alive})$ instead of $n \hat{p}_{alive}$ and $n(1 - \hat{p}_{alive})$?

Remember the logic of inference and the philosophy of the null hypothesis. To convince the skeptics, we must assume the null hypothesis throughout the process. It's only after we present sufficient evidence that can we reject the null and fall back on the alternative hypothesis that encapsulates our research question.

Therefore, under the assumption of the null, the sampling distribution is the *null distribution*, meaning that it's centered at 0.5. All work we do with the normal model, including checking conditions, must use the null model with $p_{alive}= 0.5$.

That's also why the numbers don't have to be whole numbers. If the null states that of the 69 patients, 50% are expected to survive, then we expect 50% of 69, or 34.5, to survive. Of course, you can't have half of a survivor. But these are not *actual* survivors. Rather, they are the expected number of survivors in a group of 69 patients *on average* under the assumption of the null.


## Mechanics {#one-prop-ex-mechanics}

### Compute the test statistic. {#one-prop-ex-compute-test-stat}


```r
alive_prop <- heart_transplant2 %>%
    specify(response = survived, succes = "alive") %>%
    calculate(stat = "prop")
alive_prop
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["stat"],"name":[1],"type":["dbl"],"align":["right"]}],"data":[{"1":"0.3478261"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

We'll also compute the corresponding z score.


```r
alive_z <- heart_transplant2 %>%
    specify(response = survived, success = "alive") %>%
    hypothesize(null = "point", p = 0.5) %>%
    calculate(stat = "z")
alive_z
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["stat"],"name":[1],"type":["dbl"],"align":["right"]}],"data":[{"1":"-2.528103"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


Commentary: The sample proportion code is straightforward and we've seen it before. To get the z score, we also have to tell `infer` what the null hypothesis is so that it knows where the center of our normal distribution will be. In the `hypothesize` function, we tell `infer` to use a "point" null hypothesis with `p = 0.5`. All this means is that the null is a specific point: 0.5. (Contrast this to hypothesis tests with two variables when we had `null = "independence"`.)

We can confirm the calculation of the z score manually. It's easiest to compute the standard error first. Recall that the standard error is

$$
SE = \sqrt{\frac{p_{alive}(1 - p_{alive})}{n}} = \sqrt{\frac{0.5(1 - 0.5)}{69}}
$$

**Remember that are working under the assumption of the null hypothesis.** This means that we use $p_{alive} = 0.5$ everywhere in the formula for the standard error. 

We can do the math in R and store our result as `SE`.


```r
SE <- sqrt(0.5*(1 - 0.5)/69)
SE
```

```
## [1] 0.06019293
```
Then our z score is

$$
z = \frac{(\hat{p}_{alive} - p_{alive})}{SE} =  \frac{(\hat{p}_{alive} - p_{alive})}{\sqrt{\frac{p_{alive} (1 - p_{alive})}{n}}} = \frac{(0.348 - 0.5)}{\sqrt{\frac{0.5 (1 - 0.5)}{69}}} =  -2.53.
$$

Using the values of `alive_prop` and `SE`:


```r
z <- (alive_prop - 0.5)/SE
z
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["stat"],"name":[1],"type":["dbl"],"align":["right"]}],"data":[{"1":"-2.528103"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

Both the sample proportion $\hat{p}_{alive}$ (stored above as `alive_prop`) and the corresponding z-score can be considered the "test statistic". If we use $\hat{p}_{alive}$ as the test statistic, then we're considering the null model to be

$$
N\left(0.5, \sqrt{\frac{0.5 (1 - 0.5)}{69}}\right).
$$

If we use z as the test statistic, then we're considering the null model to be the *standard normal model*:

$$
N(0, 1).
$$

The standard normal model is more intuitive and easier to work with, both conceptually and in R. Generally, then, we will consider z as the test statistic so that we can consider our null model to be the standard normal model. For example, knowing that our test statistic is two and a half standard deviations to the left of the null value already tells us a lot. We can anticipate a small P-value leading to rejection of the null. Nevertheless, for this type of hypothesis test, we'll compute both in this section of the rubric.

### Report the test statistic in context (when possible). {#one-prop-ex-report-test-stat}

The test statistic is 0.3478261. In other words, 34.7826087% of heart transplant recipients were alive at the end of the study.

The z score is -2.5281029. The proportion of survivors is about 2.5 standard errors below the null value.


### Plot the null distribution. {#one-prop-ex-plot_null}


```r
alive_test <- heart_transplant2 %>%
    specify(response = survived, success = "alive") %>%
    hypothesize(null = "point", p = 0.5) %>%
    assume(distribution = "z")
alive_test
```

```
## A Z distribution.
```


```r
alive_test %>%
    visualize() +
    shade_p_value(obs_stat = alive_z, direction = "less")
```

<img src="15-inference_for_one_proportion-web_files/figure-html/unnamed-chunk-12-1.png" width="672" />

Commentary: In past chapters, we have used the `generate` verb to get many repetitions (usually 1000) of some kind of random process to simulate the sampling distribution model. In this chapter, we have used the verb `assume` instead to assume that the sampling distribution is a normal model. As long as the conditions hold, this is a reasonable assumption. This also means that we don't have to use `set.seed` as there is no random process to reproduce.

Compare the graph above to what we would see if we simulated the sampling distribution. (Now we do need `set.seed`!)


```r
set.seed(6789)
alive_test_draw <- heart_transplant2 %>%
    specify(response = survived, success = "alive") %>%
    hypothesize(null = "point", p = 0.5) %>%
    generate(reps = 1000, type = "draw") %>%
    calculate(stat = "prop")
alive_test_draw
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["replicate"],"name":[1],"type":["fct"],"align":["left"]},{"label":["stat"],"name":[2],"type":["dbl"],"align":["right"]}],"data":[{"1":"1","2":"0.4927536"},{"1":"2","2":"0.4057971"},{"1":"3","2":"0.4347826"},{"1":"4","2":"0.5797101"},{"1":"5","2":"0.5217391"},{"1":"6","2":"0.5072464"},{"1":"7","2":"0.5797101"},{"1":"8","2":"0.4347826"},{"1":"9","2":"0.5507246"},{"1":"10","2":"0.4347826"},{"1":"11","2":"0.4927536"},{"1":"12","2":"0.6086957"},{"1":"13","2":"0.5797101"},{"1":"14","2":"0.5797101"},{"1":"15","2":"0.4927536"},{"1":"16","2":"0.4927536"},{"1":"17","2":"0.5942029"},{"1":"18","2":"0.4057971"},{"1":"19","2":"0.5217391"},{"1":"20","2":"0.5942029"},{"1":"21","2":"0.4492754"},{"1":"22","2":"0.4202899"},{"1":"23","2":"0.5217391"},{"1":"24","2":"0.6376812"},{"1":"25","2":"0.3913043"},{"1":"26","2":"0.5507246"},{"1":"27","2":"0.3768116"},{"1":"28","2":"0.5652174"},{"1":"29","2":"0.4927536"},{"1":"30","2":"0.5217391"},{"1":"31","2":"0.5362319"},{"1":"32","2":"0.4782609"},{"1":"33","2":"0.4782609"},{"1":"34","2":"0.5217391"},{"1":"35","2":"0.3623188"},{"1":"36","2":"0.5507246"},{"1":"37","2":"0.4782609"},{"1":"38","2":"0.5362319"},{"1":"39","2":"0.4347826"},{"1":"40","2":"0.4492754"},{"1":"41","2":"0.3623188"},{"1":"42","2":"0.4637681"},{"1":"43","2":"0.4637681"},{"1":"44","2":"0.4927536"},{"1":"45","2":"0.5072464"},{"1":"46","2":"0.5652174"},{"1":"47","2":"0.5362319"},{"1":"48","2":"0.5362319"},{"1":"49","2":"0.5217391"},{"1":"50","2":"0.4782609"},{"1":"51","2":"0.4927536"},{"1":"52","2":"0.4347826"},{"1":"53","2":"0.4637681"},{"1":"54","2":"0.4927536"},{"1":"55","2":"0.4347826"},{"1":"56","2":"0.4347826"},{"1":"57","2":"0.4927536"},{"1":"58","2":"0.4782609"},{"1":"59","2":"0.5797101"},{"1":"60","2":"0.3768116"},{"1":"61","2":"0.5362319"},{"1":"62","2":"0.4782609"},{"1":"63","2":"0.3768116"},{"1":"64","2":"0.4927536"},{"1":"65","2":"0.4637681"},{"1":"66","2":"0.4492754"},{"1":"67","2":"0.4637681"},{"1":"68","2":"0.5072464"},{"1":"69","2":"0.6376812"},{"1":"70","2":"0.5942029"},{"1":"71","2":"0.4492754"},{"1":"72","2":"0.5362319"},{"1":"73","2":"0.4492754"},{"1":"74","2":"0.5942029"},{"1":"75","2":"0.5072464"},{"1":"76","2":"0.4927536"},{"1":"77","2":"0.5217391"},{"1":"78","2":"0.4782609"},{"1":"79","2":"0.6086957"},{"1":"80","2":"0.5217391"},{"1":"81","2":"0.4637681"},{"1":"82","2":"0.3768116"},{"1":"83","2":"0.5507246"},{"1":"84","2":"0.4492754"},{"1":"85","2":"0.5942029"},{"1":"86","2":"0.4202899"},{"1":"87","2":"0.4782609"},{"1":"88","2":"0.5652174"},{"1":"89","2":"0.4492754"},{"1":"90","2":"0.6086957"},{"1":"91","2":"0.4492754"},{"1":"92","2":"0.4782609"},{"1":"93","2":"0.5362319"},{"1":"94","2":"0.3768116"},{"1":"95","2":"0.5072464"},{"1":"96","2":"0.5072464"},{"1":"97","2":"0.4782609"},{"1":"98","2":"0.5942029"},{"1":"99","2":"0.5072464"},{"1":"100","2":"0.4637681"},{"1":"101","2":"0.4202899"},{"1":"102","2":"0.5362319"},{"1":"103","2":"0.3913043"},{"1":"104","2":"0.5072464"},{"1":"105","2":"0.5942029"},{"1":"106","2":"0.4637681"},{"1":"107","2":"0.5942029"},{"1":"108","2":"0.4492754"},{"1":"109","2":"0.4637681"},{"1":"110","2":"0.4492754"},{"1":"111","2":"0.4782609"},{"1":"112","2":"0.4202899"},{"1":"113","2":"0.4202899"},{"1":"114","2":"0.5652174"},{"1":"115","2":"0.4347826"},{"1":"116","2":"0.3913043"},{"1":"117","2":"0.5072464"},{"1":"118","2":"0.3913043"},{"1":"119","2":"0.5507246"},{"1":"120","2":"0.4492754"},{"1":"121","2":"0.4057971"},{"1":"122","2":"0.5507246"},{"1":"123","2":"0.4927536"},{"1":"124","2":"0.5072464"},{"1":"125","2":"0.5072464"},{"1":"126","2":"0.4637681"},{"1":"127","2":"0.4782609"},{"1":"128","2":"0.4637681"},{"1":"129","2":"0.5072464"},{"1":"130","2":"0.5652174"},{"1":"131","2":"0.3768116"},{"1":"132","2":"0.5072464"},{"1":"133","2":"0.4927536"},{"1":"134","2":"0.5942029"},{"1":"135","2":"0.4927536"},{"1":"136","2":"0.3768116"},{"1":"137","2":"0.5362319"},{"1":"138","2":"0.4347826"},{"1":"139","2":"0.5507246"},{"1":"140","2":"0.5072464"},{"1":"141","2":"0.5507246"},{"1":"142","2":"0.3333333"},{"1":"143","2":"0.5217391"},{"1":"144","2":"0.5362319"},{"1":"145","2":"0.5217391"},{"1":"146","2":"0.4927536"},{"1":"147","2":"0.4782609"},{"1":"148","2":"0.5652174"},{"1":"149","2":"0.5942029"},{"1":"150","2":"0.4202899"},{"1":"151","2":"0.5072464"},{"1":"152","2":"0.5072464"},{"1":"153","2":"0.5362319"},{"1":"154","2":"0.3913043"},{"1":"155","2":"0.5362319"},{"1":"156","2":"0.4927536"},{"1":"157","2":"0.4347826"},{"1":"158","2":"0.4637681"},{"1":"159","2":"0.4202899"},{"1":"160","2":"0.4927536"},{"1":"161","2":"0.5072464"},{"1":"162","2":"0.5362319"},{"1":"163","2":"0.5362319"},{"1":"164","2":"0.4492754"},{"1":"165","2":"0.4927536"},{"1":"166","2":"0.4927536"},{"1":"167","2":"0.4782609"},{"1":"168","2":"0.6086957"},{"1":"169","2":"0.5507246"},{"1":"170","2":"0.6231884"},{"1":"171","2":"0.5217391"},{"1":"172","2":"0.4782609"},{"1":"173","2":"0.5362319"},{"1":"174","2":"0.4637681"},{"1":"175","2":"0.5217391"},{"1":"176","2":"0.6086957"},{"1":"177","2":"0.5217391"},{"1":"178","2":"0.4637681"},{"1":"179","2":"0.4637681"},{"1":"180","2":"0.5507246"},{"1":"181","2":"0.4637681"},{"1":"182","2":"0.5797101"},{"1":"183","2":"0.4927536"},{"1":"184","2":"0.4057971"},{"1":"185","2":"0.4782609"},{"1":"186","2":"0.4637681"},{"1":"187","2":"0.5942029"},{"1":"188","2":"0.4492754"},{"1":"189","2":"0.5652174"},{"1":"190","2":"0.4782609"},{"1":"191","2":"0.6231884"},{"1":"192","2":"0.5362319"},{"1":"193","2":"0.4202899"},{"1":"194","2":"0.4347826"},{"1":"195","2":"0.5072464"},{"1":"196","2":"0.6376812"},{"1":"197","2":"0.4347826"},{"1":"198","2":"0.4202899"},{"1":"199","2":"0.5507246"},{"1":"200","2":"0.4927536"},{"1":"201","2":"0.5797101"},{"1":"202","2":"0.5072464"},{"1":"203","2":"0.5797101"},{"1":"204","2":"0.4202899"},{"1":"205","2":"0.5797101"},{"1":"206","2":"0.4927536"},{"1":"207","2":"0.5072464"},{"1":"208","2":"0.4492754"},{"1":"209","2":"0.6086957"},{"1":"210","2":"0.5652174"},{"1":"211","2":"0.4782609"},{"1":"212","2":"0.4637681"},{"1":"213","2":"0.5652174"},{"1":"214","2":"0.5652174"},{"1":"215","2":"0.5797101"},{"1":"216","2":"0.5507246"},{"1":"217","2":"0.4927536"},{"1":"218","2":"0.4637681"},{"1":"219","2":"0.5942029"},{"1":"220","2":"0.4347826"},{"1":"221","2":"0.4637681"},{"1":"222","2":"0.5652174"},{"1":"223","2":"0.5217391"},{"1":"224","2":"0.5217391"},{"1":"225","2":"0.5652174"},{"1":"226","2":"0.4492754"},{"1":"227","2":"0.5797101"},{"1":"228","2":"0.3913043"},{"1":"229","2":"0.5362319"},{"1":"230","2":"0.3623188"},{"1":"231","2":"0.5652174"},{"1":"232","2":"0.5362319"},{"1":"233","2":"0.5217391"},{"1":"234","2":"0.4782609"},{"1":"235","2":"0.3623188"},{"1":"236","2":"0.4637681"},{"1":"237","2":"0.5507246"},{"1":"238","2":"0.4492754"},{"1":"239","2":"0.3913043"},{"1":"240","2":"0.4927536"},{"1":"241","2":"0.5072464"},{"1":"242","2":"0.4347826"},{"1":"243","2":"0.4637681"},{"1":"244","2":"0.4347826"},{"1":"245","2":"0.5362319"},{"1":"246","2":"0.4347826"},{"1":"247","2":"0.5797101"},{"1":"248","2":"0.5072464"},{"1":"249","2":"0.5652174"},{"1":"250","2":"0.4492754"},{"1":"251","2":"0.4492754"},{"1":"252","2":"0.4057971"},{"1":"253","2":"0.4202899"},{"1":"254","2":"0.5507246"},{"1":"255","2":"0.5217391"},{"1":"256","2":"0.5217391"},{"1":"257","2":"0.5362319"},{"1":"258","2":"0.4202899"},{"1":"259","2":"0.4492754"},{"1":"260","2":"0.4782609"},{"1":"261","2":"0.5942029"},{"1":"262","2":"0.6086957"},{"1":"263","2":"0.5942029"},{"1":"264","2":"0.4202899"},{"1":"265","2":"0.4927536"},{"1":"266","2":"0.4637681"},{"1":"267","2":"0.5362319"},{"1":"268","2":"0.6231884"},{"1":"269","2":"0.5072464"},{"1":"270","2":"0.4782609"},{"1":"271","2":"0.5942029"},{"1":"272","2":"0.5362319"},{"1":"273","2":"0.5072464"},{"1":"274","2":"0.4927536"},{"1":"275","2":"0.5217391"},{"1":"276","2":"0.3333333"},{"1":"277","2":"0.5507246"},{"1":"278","2":"0.6231884"},{"1":"279","2":"0.5362319"},{"1":"280","2":"0.4927536"},{"1":"281","2":"0.4927536"},{"1":"282","2":"0.3913043"},{"1":"283","2":"0.5072464"},{"1":"284","2":"0.4637681"},{"1":"285","2":"0.4637681"},{"1":"286","2":"0.5362319"},{"1":"287","2":"0.5217391"},{"1":"288","2":"0.5362319"},{"1":"289","2":"0.5217391"},{"1":"290","2":"0.4057971"},{"1":"291","2":"0.5652174"},{"1":"292","2":"0.5362319"},{"1":"293","2":"0.4782609"},{"1":"294","2":"0.4782609"},{"1":"295","2":"0.5072464"},{"1":"296","2":"0.5797101"},{"1":"297","2":"0.5507246"},{"1":"298","2":"0.5362319"},{"1":"299","2":"0.5072464"},{"1":"300","2":"0.5652174"},{"1":"301","2":"0.5362319"},{"1":"302","2":"0.5072464"},{"1":"303","2":"0.5072464"},{"1":"304","2":"0.5072464"},{"1":"305","2":"0.4492754"},{"1":"306","2":"0.3623188"},{"1":"307","2":"0.6086957"},{"1":"308","2":"0.4782609"},{"1":"309","2":"0.4927536"},{"1":"310","2":"0.4202899"},{"1":"311","2":"0.4347826"},{"1":"312","2":"0.5652174"},{"1":"313","2":"0.4057971"},{"1":"314","2":"0.3478261"},{"1":"315","2":"0.5072464"},{"1":"316","2":"0.4637681"},{"1":"317","2":"0.4057971"},{"1":"318","2":"0.3768116"},{"1":"319","2":"0.5072464"},{"1":"320","2":"0.4202899"},{"1":"321","2":"0.4927536"},{"1":"322","2":"0.5217391"},{"1":"323","2":"0.5797101"},{"1":"324","2":"0.5072464"},{"1":"325","2":"0.5362319"},{"1":"326","2":"0.5652174"},{"1":"327","2":"0.4492754"},{"1":"328","2":"0.5072464"},{"1":"329","2":"0.4637681"},{"1":"330","2":"0.4782609"},{"1":"331","2":"0.4347826"},{"1":"332","2":"0.4637681"},{"1":"333","2":"0.5507246"},{"1":"334","2":"0.4057971"},{"1":"335","2":"0.5072464"},{"1":"336","2":"0.4347826"},{"1":"337","2":"0.4927536"},{"1":"338","2":"0.5507246"},{"1":"339","2":"0.4637681"},{"1":"340","2":"0.4782609"},{"1":"341","2":"0.4927536"},{"1":"342","2":"0.5072464"},{"1":"343","2":"0.4637681"},{"1":"344","2":"0.5217391"},{"1":"345","2":"0.5652174"},{"1":"346","2":"0.5072464"},{"1":"347","2":"0.5507246"},{"1":"348","2":"0.5217391"},{"1":"349","2":"0.4637681"},{"1":"350","2":"0.5507246"},{"1":"351","2":"0.5942029"},{"1":"352","2":"0.5072464"},{"1":"353","2":"0.5652174"},{"1":"354","2":"0.5072464"},{"1":"355","2":"0.4782609"},{"1":"356","2":"0.4927536"},{"1":"357","2":"0.4637681"},{"1":"358","2":"0.5217391"},{"1":"359","2":"0.5362319"},{"1":"360","2":"0.4492754"},{"1":"361","2":"0.6376812"},{"1":"362","2":"0.4927536"},{"1":"363","2":"0.4927536"},{"1":"364","2":"0.4782609"},{"1":"365","2":"0.4927536"},{"1":"366","2":"0.4927536"},{"1":"367","2":"0.4057971"},{"1":"368","2":"0.4782609"},{"1":"369","2":"0.5942029"},{"1":"370","2":"0.4927536"},{"1":"371","2":"0.4782609"},{"1":"372","2":"0.4927536"},{"1":"373","2":"0.5507246"},{"1":"374","2":"0.5217391"},{"1":"375","2":"0.6086957"},{"1":"376","2":"0.5217391"},{"1":"377","2":"0.5217391"},{"1":"378","2":"0.5217391"},{"1":"379","2":"0.4927536"},{"1":"380","2":"0.5797101"},{"1":"381","2":"0.5217391"},{"1":"382","2":"0.4782609"},{"1":"383","2":"0.5362319"},{"1":"384","2":"0.5942029"},{"1":"385","2":"0.4782609"},{"1":"386","2":"0.4927536"},{"1":"387","2":"0.5507246"},{"1":"388","2":"0.4927536"},{"1":"389","2":"0.4637681"},{"1":"390","2":"0.3768116"},{"1":"391","2":"0.5072464"},{"1":"392","2":"0.4637681"},{"1":"393","2":"0.4202899"},{"1":"394","2":"0.5217391"},{"1":"395","2":"0.5942029"},{"1":"396","2":"0.5072464"},{"1":"397","2":"0.4782609"},{"1":"398","2":"0.5217391"},{"1":"399","2":"0.4347826"},{"1":"400","2":"0.5797101"},{"1":"401","2":"0.5507246"},{"1":"402","2":"0.5507246"},{"1":"403","2":"0.4782609"},{"1":"404","2":"0.5507246"},{"1":"405","2":"0.4927536"},{"1":"406","2":"0.4347826"},{"1":"407","2":"0.5362319"},{"1":"408","2":"0.6086957"},{"1":"409","2":"0.4637681"},{"1":"410","2":"0.4637681"},{"1":"411","2":"0.4347826"},{"1":"412","2":"0.4927536"},{"1":"413","2":"0.4782609"},{"1":"414","2":"0.5072464"},{"1":"415","2":"0.5072464"},{"1":"416","2":"0.3913043"},{"1":"417","2":"0.3768116"},{"1":"418","2":"0.5797101"},{"1":"419","2":"0.5507246"},{"1":"420","2":"0.4202899"},{"1":"421","2":"0.4492754"},{"1":"422","2":"0.5217391"},{"1":"423","2":"0.4637681"},{"1":"424","2":"0.3623188"},{"1":"425","2":"0.5507246"},{"1":"426","2":"0.4927536"},{"1":"427","2":"0.5362319"},{"1":"428","2":"0.5942029"},{"1":"429","2":"0.4927536"},{"1":"430","2":"0.5072464"},{"1":"431","2":"0.4347826"},{"1":"432","2":"0.4492754"},{"1":"433","2":"0.4782609"},{"1":"434","2":"0.4927536"},{"1":"435","2":"0.4637681"},{"1":"436","2":"0.5507246"},{"1":"437","2":"0.5942029"},{"1":"438","2":"0.4927536"},{"1":"439","2":"0.4782609"},{"1":"440","2":"0.3913043"},{"1":"441","2":"0.5652174"},{"1":"442","2":"0.3913043"},{"1":"443","2":"0.5362319"},{"1":"444","2":"0.4492754"},{"1":"445","2":"0.4202899"},{"1":"446","2":"0.5652174"},{"1":"447","2":"0.3623188"},{"1":"448","2":"0.5072464"},{"1":"449","2":"0.4782609"},{"1":"450","2":"0.5652174"},{"1":"451","2":"0.4927536"},{"1":"452","2":"0.4927536"},{"1":"453","2":"0.5217391"},{"1":"454","2":"0.4782609"},{"1":"455","2":"0.4927536"},{"1":"456","2":"0.5072464"},{"1":"457","2":"0.5362319"},{"1":"458","2":"0.5072464"},{"1":"459","2":"0.4637681"},{"1":"460","2":"0.4202899"},{"1":"461","2":"0.4782609"},{"1":"462","2":"0.4347826"},{"1":"463","2":"0.4782609"},{"1":"464","2":"0.5217391"},{"1":"465","2":"0.5217391"},{"1":"466","2":"0.4782609"},{"1":"467","2":"0.4927536"},{"1":"468","2":"0.5362319"},{"1":"469","2":"0.3913043"},{"1":"470","2":"0.4927536"},{"1":"471","2":"0.4927536"},{"1":"472","2":"0.5362319"},{"1":"473","2":"0.4927536"},{"1":"474","2":"0.4782609"},{"1":"475","2":"0.4927536"},{"1":"476","2":"0.4927536"},{"1":"477","2":"0.4782609"},{"1":"478","2":"0.4782609"},{"1":"479","2":"0.4782609"},{"1":"480","2":"0.4782609"},{"1":"481","2":"0.4202899"},{"1":"482","2":"0.4927536"},{"1":"483","2":"0.4782609"},{"1":"484","2":"0.5652174"},{"1":"485","2":"0.4637681"},{"1":"486","2":"0.5797101"},{"1":"487","2":"0.5217391"},{"1":"488","2":"0.4202899"},{"1":"489","2":"0.4637681"},{"1":"490","2":"0.5072464"},{"1":"491","2":"0.5072464"},{"1":"492","2":"0.5072464"},{"1":"493","2":"0.4202899"},{"1":"494","2":"0.5507246"},{"1":"495","2":"0.5072464"},{"1":"496","2":"0.5217391"},{"1":"497","2":"0.4492754"},{"1":"498","2":"0.4347826"},{"1":"499","2":"0.4782609"},{"1":"500","2":"0.4637681"},{"1":"501","2":"0.5217391"},{"1":"502","2":"0.5217391"},{"1":"503","2":"0.5072464"},{"1":"504","2":"0.4637681"},{"1":"505","2":"0.4782609"},{"1":"506","2":"0.4927536"},{"1":"507","2":"0.4927536"},{"1":"508","2":"0.4492754"},{"1":"509","2":"0.4782609"},{"1":"510","2":"0.5362319"},{"1":"511","2":"0.4927536"},{"1":"512","2":"0.5507246"},{"1":"513","2":"0.5362319"},{"1":"514","2":"0.4492754"},{"1":"515","2":"0.5362319"},{"1":"516","2":"0.6086957"},{"1":"517","2":"0.4057971"},{"1":"518","2":"0.5217391"},{"1":"519","2":"0.3913043"},{"1":"520","2":"0.5942029"},{"1":"521","2":"0.5362319"},{"1":"522","2":"0.5652174"},{"1":"523","2":"0.5507246"},{"1":"524","2":"0.4927536"},{"1":"525","2":"0.5362319"},{"1":"526","2":"0.5072464"},{"1":"527","2":"0.5362319"},{"1":"528","2":"0.6086957"},{"1":"529","2":"0.5507246"},{"1":"530","2":"0.4492754"},{"1":"531","2":"0.5217391"},{"1":"532","2":"0.5797101"},{"1":"533","2":"0.4637681"},{"1":"534","2":"0.5942029"},{"1":"535","2":"0.4637681"},{"1":"536","2":"0.4637681"},{"1":"537","2":"0.5217391"},{"1":"538","2":"0.4057971"},{"1":"539","2":"0.5652174"},{"1":"540","2":"0.5217391"},{"1":"541","2":"0.6231884"},{"1":"542","2":"0.4782609"},{"1":"543","2":"0.4347826"},{"1":"544","2":"0.5217391"},{"1":"545","2":"0.5652174"},{"1":"546","2":"0.5217391"},{"1":"547","2":"0.4637681"},{"1":"548","2":"0.5507246"},{"1":"549","2":"0.4637681"},{"1":"550","2":"0.4202899"},{"1":"551","2":"0.3913043"},{"1":"552","2":"0.5072464"},{"1":"553","2":"0.4782609"},{"1":"554","2":"0.5217391"},{"1":"555","2":"0.5797101"},{"1":"556","2":"0.4637681"},{"1":"557","2":"0.4782609"},{"1":"558","2":"0.6521739"},{"1":"559","2":"0.5072464"},{"1":"560","2":"0.5072464"},{"1":"561","2":"0.4347826"},{"1":"562","2":"0.4637681"},{"1":"563","2":"0.5072464"},{"1":"564","2":"0.5217391"},{"1":"565","2":"0.5072464"},{"1":"566","2":"0.4057971"},{"1":"567","2":"0.4347826"},{"1":"568","2":"0.4782609"},{"1":"569","2":"0.4637681"},{"1":"570","2":"0.5362319"},{"1":"571","2":"0.4057971"},{"1":"572","2":"0.4492754"},{"1":"573","2":"0.4492754"},{"1":"574","2":"0.5797101"},{"1":"575","2":"0.5797101"},{"1":"576","2":"0.4782609"},{"1":"577","2":"0.5507246"},{"1":"578","2":"0.4492754"},{"1":"579","2":"0.4782609"},{"1":"580","2":"0.5362319"},{"1":"581","2":"0.5072464"},{"1":"582","2":"0.5217391"},{"1":"583","2":"0.4927536"},{"1":"584","2":"0.4492754"},{"1":"585","2":"0.4492754"},{"1":"586","2":"0.4637681"},{"1":"587","2":"0.5217391"},{"1":"588","2":"0.5072464"},{"1":"589","2":"0.5217391"},{"1":"590","2":"0.4782609"},{"1":"591","2":"0.4927536"},{"1":"592","2":"0.5652174"},{"1":"593","2":"0.5072464"},{"1":"594","2":"0.4347826"},{"1":"595","2":"0.4347826"},{"1":"596","2":"0.4347826"},{"1":"597","2":"0.5072464"},{"1":"598","2":"0.4202899"},{"1":"599","2":"0.4057971"},{"1":"600","2":"0.4492754"},{"1":"601","2":"0.5072464"},{"1":"602","2":"0.4927536"},{"1":"603","2":"0.5217391"},{"1":"604","2":"0.4637681"},{"1":"605","2":"0.4347826"},{"1":"606","2":"0.5507246"},{"1":"607","2":"0.4782609"},{"1":"608","2":"0.4927536"},{"1":"609","2":"0.5362319"},{"1":"610","2":"0.4782609"},{"1":"611","2":"0.4202899"},{"1":"612","2":"0.4057971"},{"1":"613","2":"0.5362319"},{"1":"614","2":"0.5217391"},{"1":"615","2":"0.5072464"},{"1":"616","2":"0.4782609"},{"1":"617","2":"0.4927536"},{"1":"618","2":"0.4927536"},{"1":"619","2":"0.5217391"},{"1":"620","2":"0.5217391"},{"1":"621","2":"0.3913043"},{"1":"622","2":"0.3478261"},{"1":"623","2":"0.3913043"},{"1":"624","2":"0.4927536"},{"1":"625","2":"0.5072464"},{"1":"626","2":"0.5507246"},{"1":"627","2":"0.5072464"},{"1":"628","2":"0.4927536"},{"1":"629","2":"0.4927536"},{"1":"630","2":"0.4782609"},{"1":"631","2":"0.4782609"},{"1":"632","2":"0.4927536"},{"1":"633","2":"0.5507246"},{"1":"634","2":"0.4927536"},{"1":"635","2":"0.5362319"},{"1":"636","2":"0.5507246"},{"1":"637","2":"0.5072464"},{"1":"638","2":"0.4637681"},{"1":"639","2":"0.4927536"},{"1":"640","2":"0.3478261"},{"1":"641","2":"0.4492754"},{"1":"642","2":"0.3768116"},{"1":"643","2":"0.5652174"},{"1":"644","2":"0.5652174"},{"1":"645","2":"0.5507246"},{"1":"646","2":"0.4637681"},{"1":"647","2":"0.4202899"},{"1":"648","2":"0.4347826"},{"1":"649","2":"0.5072464"},{"1":"650","2":"0.5072464"},{"1":"651","2":"0.5362319"},{"1":"652","2":"0.3913043"},{"1":"653","2":"0.5217391"},{"1":"654","2":"0.4637681"},{"1":"655","2":"0.4202899"},{"1":"656","2":"0.5507246"},{"1":"657","2":"0.4782609"},{"1":"658","2":"0.6086957"},{"1":"659","2":"0.5072464"},{"1":"660","2":"0.4637681"},{"1":"661","2":"0.4347826"},{"1":"662","2":"0.4637681"},{"1":"663","2":"0.3913043"},{"1":"664","2":"0.5217391"},{"1":"665","2":"0.5072464"},{"1":"666","2":"0.4492754"},{"1":"667","2":"0.5072464"},{"1":"668","2":"0.4637681"},{"1":"669","2":"0.4347826"},{"1":"670","2":"0.5072464"},{"1":"671","2":"0.4782609"},{"1":"672","2":"0.5072464"},{"1":"673","2":"0.5797101"},{"1":"674","2":"0.5507246"},{"1":"675","2":"0.4782609"},{"1":"676","2":"0.4782609"},{"1":"677","2":"0.4347826"},{"1":"678","2":"0.4637681"},{"1":"679","2":"0.5072464"},{"1":"680","2":"0.4492754"},{"1":"681","2":"0.5507246"},{"1":"682","2":"0.5362319"},{"1":"683","2":"0.6086957"},{"1":"684","2":"0.5072464"},{"1":"685","2":"0.5072464"},{"1":"686","2":"0.5362319"},{"1":"687","2":"0.5362319"},{"1":"688","2":"0.4927536"},{"1":"689","2":"0.5652174"},{"1":"690","2":"0.4927536"},{"1":"691","2":"0.5652174"},{"1":"692","2":"0.4782609"},{"1":"693","2":"0.3913043"},{"1":"694","2":"0.4927536"},{"1":"695","2":"0.5942029"},{"1":"696","2":"0.4202899"},{"1":"697","2":"0.5797101"},{"1":"698","2":"0.4347826"},{"1":"699","2":"0.4202899"},{"1":"700","2":"0.5507246"},{"1":"701","2":"0.4637681"},{"1":"702","2":"0.5072464"},{"1":"703","2":"0.5797101"},{"1":"704","2":"0.5217391"},{"1":"705","2":"0.5217391"},{"1":"706","2":"0.6521739"},{"1":"707","2":"0.4492754"},{"1":"708","2":"0.5217391"},{"1":"709","2":"0.4782609"},{"1":"710","2":"0.4492754"},{"1":"711","2":"0.3623188"},{"1":"712","2":"0.5072464"},{"1":"713","2":"0.4637681"},{"1":"714","2":"0.5217391"},{"1":"715","2":"0.6521739"},{"1":"716","2":"0.4782609"},{"1":"717","2":"0.6231884"},{"1":"718","2":"0.4202899"},{"1":"719","2":"0.4202899"},{"1":"720","2":"0.4927536"},{"1":"721","2":"0.5072464"},{"1":"722","2":"0.5217391"},{"1":"723","2":"0.5507246"},{"1":"724","2":"0.5362319"},{"1":"725","2":"0.4782609"},{"1":"726","2":"0.5362319"},{"1":"727","2":"0.4782609"},{"1":"728","2":"0.5942029"},{"1":"729","2":"0.4927536"},{"1":"730","2":"0.4782609"},{"1":"731","2":"0.4347826"},{"1":"732","2":"0.5072464"},{"1":"733","2":"0.6231884"},{"1":"734","2":"0.5072464"},{"1":"735","2":"0.5507246"},{"1":"736","2":"0.6811594"},{"1":"737","2":"0.5652174"},{"1":"738","2":"0.5072464"},{"1":"739","2":"0.5217391"},{"1":"740","2":"0.4637681"},{"1":"741","2":"0.4782609"},{"1":"742","2":"0.5507246"},{"1":"743","2":"0.6086957"},{"1":"744","2":"0.5072464"},{"1":"745","2":"0.5797101"},{"1":"746","2":"0.5797101"},{"1":"747","2":"0.6231884"},{"1":"748","2":"0.5217391"},{"1":"749","2":"0.5072464"},{"1":"750","2":"0.4782609"},{"1":"751","2":"0.5362319"},{"1":"752","2":"0.4492754"},{"1":"753","2":"0.5652174"},{"1":"754","2":"0.3623188"},{"1":"755","2":"0.4202899"},{"1":"756","2":"0.5797101"},{"1":"757","2":"0.5217391"},{"1":"758","2":"0.4927536"},{"1":"759","2":"0.4347826"},{"1":"760","2":"0.4492754"},{"1":"761","2":"0.4782609"},{"1":"762","2":"0.4927536"},{"1":"763","2":"0.4637681"},{"1":"764","2":"0.5072464"},{"1":"765","2":"0.5942029"},{"1":"766","2":"0.5217391"},{"1":"767","2":"0.4347826"},{"1":"768","2":"0.5072464"},{"1":"769","2":"0.4492754"},{"1":"770","2":"0.4057971"},{"1":"771","2":"0.4492754"},{"1":"772","2":"0.5362319"},{"1":"773","2":"0.4637681"},{"1":"774","2":"0.4782609"},{"1":"775","2":"0.3768116"},{"1":"776","2":"0.5362319"},{"1":"777","2":"0.4347826"},{"1":"778","2":"0.5797101"},{"1":"779","2":"0.5362319"},{"1":"780","2":"0.5362319"},{"1":"781","2":"0.5507246"},{"1":"782","2":"0.5362319"},{"1":"783","2":"0.4492754"},{"1":"784","2":"0.5797101"},{"1":"785","2":"0.4927536"},{"1":"786","2":"0.4637681"},{"1":"787","2":"0.3623188"},{"1":"788","2":"0.4347826"},{"1":"789","2":"0.5217391"},{"1":"790","2":"0.4057971"},{"1":"791","2":"0.4782609"},{"1":"792","2":"0.5217391"},{"1":"793","2":"0.4637681"},{"1":"794","2":"0.5072464"},{"1":"795","2":"0.5362319"},{"1":"796","2":"0.4637681"},{"1":"797","2":"0.5217391"},{"1":"798","2":"0.4782609"},{"1":"799","2":"0.5217391"},{"1":"800","2":"0.4782609"},{"1":"801","2":"0.5217391"},{"1":"802","2":"0.4057971"},{"1":"803","2":"0.6231884"},{"1":"804","2":"0.4202899"},{"1":"805","2":"0.4927536"},{"1":"806","2":"0.5217391"},{"1":"807","2":"0.5072464"},{"1":"808","2":"0.5072464"},{"1":"809","2":"0.5217391"},{"1":"810","2":"0.5652174"},{"1":"811","2":"0.4782609"},{"1":"812","2":"0.4347826"},{"1":"813","2":"0.6521739"},{"1":"814","2":"0.4927536"},{"1":"815","2":"0.6231884"},{"1":"816","2":"0.5217391"},{"1":"817","2":"0.5942029"},{"1":"818","2":"0.5797101"},{"1":"819","2":"0.5362319"},{"1":"820","2":"0.3913043"},{"1":"821","2":"0.5362319"},{"1":"822","2":"0.4782609"},{"1":"823","2":"0.4927536"},{"1":"824","2":"0.5797101"},{"1":"825","2":"0.5507246"},{"1":"826","2":"0.5507246"},{"1":"827","2":"0.4202899"},{"1":"828","2":"0.4057971"},{"1":"829","2":"0.5362319"},{"1":"830","2":"0.4057971"},{"1":"831","2":"0.4782609"},{"1":"832","2":"0.5942029"},{"1":"833","2":"0.5362319"},{"1":"834","2":"0.4492754"},{"1":"835","2":"0.4927536"},{"1":"836","2":"0.3768116"},{"1":"837","2":"0.5507246"},{"1":"838","2":"0.5507246"},{"1":"839","2":"0.5072464"},{"1":"840","2":"0.5797101"},{"1":"841","2":"0.5217391"},{"1":"842","2":"0.5797101"},{"1":"843","2":"0.4782609"},{"1":"844","2":"0.5797101"},{"1":"845","2":"0.4492754"},{"1":"846","2":"0.5072464"},{"1":"847","2":"0.4202899"},{"1":"848","2":"0.5797101"},{"1":"849","2":"0.5217391"},{"1":"850","2":"0.5217391"},{"1":"851","2":"0.4782609"},{"1":"852","2":"0.5217391"},{"1":"853","2":"0.4202899"},{"1":"854","2":"0.6086957"},{"1":"855","2":"0.4782609"},{"1":"856","2":"0.4202899"},{"1":"857","2":"0.5217391"},{"1":"858","2":"0.5507246"},{"1":"859","2":"0.3768116"},{"1":"860","2":"0.6086957"},{"1":"861","2":"0.3623188"},{"1":"862","2":"0.5797101"},{"1":"863","2":"0.5507246"},{"1":"864","2":"0.5797101"},{"1":"865","2":"0.4347826"},{"1":"866","2":"0.4637681"},{"1":"867","2":"0.5652174"},{"1":"868","2":"0.4637681"},{"1":"869","2":"0.5072464"},{"1":"870","2":"0.4492754"},{"1":"871","2":"0.4057971"},{"1":"872","2":"0.4492754"},{"1":"873","2":"0.5217391"},{"1":"874","2":"0.5362319"},{"1":"875","2":"0.5217391"},{"1":"876","2":"0.4927536"},{"1":"877","2":"0.4492754"},{"1":"878","2":"0.5652174"},{"1":"879","2":"0.5652174"},{"1":"880","2":"0.5072464"},{"1":"881","2":"0.5797101"},{"1":"882","2":"0.5217391"},{"1":"883","2":"0.4927536"},{"1":"884","2":"0.4782609"},{"1":"885","2":"0.4492754"},{"1":"886","2":"0.6086957"},{"1":"887","2":"0.4927536"},{"1":"888","2":"0.4347826"},{"1":"889","2":"0.5507246"},{"1":"890","2":"0.4202899"},{"1":"891","2":"0.4202899"},{"1":"892","2":"0.5507246"},{"1":"893","2":"0.4927536"},{"1":"894","2":"0.5072464"},{"1":"895","2":"0.5072464"},{"1":"896","2":"0.4057971"},{"1":"897","2":"0.5942029"},{"1":"898","2":"0.4782609"},{"1":"899","2":"0.5362319"},{"1":"900","2":"0.4492754"},{"1":"901","2":"0.3768116"},{"1":"902","2":"0.5217391"},{"1":"903","2":"0.5797101"},{"1":"904","2":"0.4492754"},{"1":"905","2":"0.3913043"},{"1":"906","2":"0.4927536"},{"1":"907","2":"0.4927536"},{"1":"908","2":"0.5072464"},{"1":"909","2":"0.3913043"},{"1":"910","2":"0.4637681"},{"1":"911","2":"0.4202899"},{"1":"912","2":"0.4492754"},{"1":"913","2":"0.4927536"},{"1":"914","2":"0.6231884"},{"1":"915","2":"0.5652174"},{"1":"916","2":"0.4492754"},{"1":"917","2":"0.4492754"},{"1":"918","2":"0.5217391"},{"1":"919","2":"0.5362319"},{"1":"920","2":"0.5652174"},{"1":"921","2":"0.5072464"},{"1":"922","2":"0.4347826"},{"1":"923","2":"0.4492754"},{"1":"924","2":"0.4057971"},{"1":"925","2":"0.5072464"},{"1":"926","2":"0.5217391"},{"1":"927","2":"0.4782609"},{"1":"928","2":"0.4492754"},{"1":"929","2":"0.4782609"},{"1":"930","2":"0.4782609"},{"1":"931","2":"0.5652174"},{"1":"932","2":"0.4637681"},{"1":"933","2":"0.3913043"},{"1":"934","2":"0.4782609"},{"1":"935","2":"0.4492754"},{"1":"936","2":"0.4927536"},{"1":"937","2":"0.4927536"},{"1":"938","2":"0.4057971"},{"1":"939","2":"0.5362319"},{"1":"940","2":"0.4637681"},{"1":"941","2":"0.4347826"},{"1":"942","2":"0.6376812"},{"1":"943","2":"0.3768116"},{"1":"944","2":"0.4347826"},{"1":"945","2":"0.4637681"},{"1":"946","2":"0.4782609"},{"1":"947","2":"0.4782609"},{"1":"948","2":"0.4347826"},{"1":"949","2":"0.4782609"},{"1":"950","2":"0.5507246"},{"1":"951","2":"0.3478261"},{"1":"952","2":"0.5652174"},{"1":"953","2":"0.4637681"},{"1":"954","2":"0.4492754"},{"1":"955","2":"0.4782609"},{"1":"956","2":"0.3478261"},{"1":"957","2":"0.3623188"},{"1":"958","2":"0.5507246"},{"1":"959","2":"0.5942029"},{"1":"960","2":"0.3623188"},{"1":"961","2":"0.5652174"},{"1":"962","2":"0.5362319"},{"1":"963","2":"0.5507246"},{"1":"964","2":"0.4347826"},{"1":"965","2":"0.5072464"},{"1":"966","2":"0.5362319"},{"1":"967","2":"0.4347826"},{"1":"968","2":"0.5217391"},{"1":"969","2":"0.5652174"},{"1":"970","2":"0.4927536"},{"1":"971","2":"0.4492754"},{"1":"972","2":"0.5362319"},{"1":"973","2":"0.5217391"},{"1":"974","2":"0.4492754"},{"1":"975","2":"0.4202899"},{"1":"976","2":"0.5507246"},{"1":"977","2":"0.5217391"},{"1":"978","2":"0.4927536"},{"1":"979","2":"0.4057971"},{"1":"980","2":"0.4782609"},{"1":"981","2":"0.5217391"},{"1":"982","2":"0.5507246"},{"1":"983","2":"0.5217391"},{"1":"984","2":"0.4347826"},{"1":"985","2":"0.6231884"},{"1":"986","2":"0.6521739"},{"1":"987","2":"0.5652174"},{"1":"988","2":"0.5217391"},{"1":"989","2":"0.4492754"},{"1":"990","2":"0.4782609"},{"1":"991","2":"0.5217391"},{"1":"992","2":"0.5797101"},{"1":"993","2":"0.4057971"},{"1":"994","2":"0.4347826"},{"1":"995","2":"0.5072464"},{"1":"996","2":"0.5652174"},{"1":"997","2":"0.4637681"},{"1":"998","2":"0.4202899"},{"1":"999","2":"0.5507246"},{"1":"1000","2":"0.4492754"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


```r
alive_test_draw %>%
    visualize() +
    shade_p_value(obs_stat = alive_prop, direction = "less")
```

<img src="15-inference_for_one_proportion-web_files/figure-html/unnamed-chunk-14-1.png" width="672" />

This is essentially the same picture, although the model above is centered on the null value 0.5 instead of the z score of 0. This also means that the `obs_stat` had to be the sample proportion `alive_prop` and not the z score `alive_z`.


### Calculate the P-value. {#one-prop-ex-calculate-p}


```r
alive_test_p <- alive_test %>%
    get_p_value(obs_stat = alive_z, direction = "less")
alive_test_p
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["p_value"],"name":[1],"type":["dbl"],"align":["right"]}],"data":[{"1":"0.005734037"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

Commentary: compare this to the P-value we get from simulating random draws:


```r
alive_test_draw %>%
    get_p_value(obs_stat = alive_prop, direction = "less")
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["p_value"],"name":[1],"type":["dbl"],"align":["right"]}],"data":[{"1":"0.007"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

The values are not exactly the same. And a new simulation with a different seed would likely give another slightly different P-value. The takeaway here is that the P-value itself has some uncertainty, so you should never take the value too seriously.

### Interpret the P-value as a probability given the null. {#one-prop-ex-interpret-p}

The P-value is 0.005734. If there were truly a 50% chance of survival among heart transplant patients, there would only be a 0.5734037% chance of seeing data at least as extreme as we saw.


## Conclusion {#one-prop-ex-ht-conclusion}

### State the statistical conclusion. {#one-prop-ex-stat-conclusion}

We reject the null hypothesis.

### State (but do not overstate) a contextually meaningful conclusion. {#one-prop-ex-context-conclusion}

We have sufficient evidence that heart transplant recipients have less than a 50% chance of survival.

### Express reservations or uncertainty about the generalizability of the conclusion. {#one-prop-ex-reservations}

Because we know nearly nothing about the provenance of the data, it's hard to generalize the conclusion. We know the data is from 1974, so it's also very likely that survival rates for heart transplant patients then are not the same as they are today. The most we could hope for is that the Stanford data was representative for heart transplant patients in 1974. Our sample size (69) is also quite small.

### Identify the possibility of either a Type I or Type II error and state what making such an error means in the context of the hypotheses. {#one-prop-ex-errors}

As we rejected the null, we run the risk of making a Type I error. It is possible that the null is true and that there is a 50% chance of survival for these patients, but we got an unusual sample that appears to have a much smaller chance of survival.


## Confidence interval {#one-prop-ex-ci}

### Check the relevant conditions to ensure that model assumptions are met. {#one-prop-ex-ci-conditions}

- Random
    - Same as above.
- 10%
    - Same as above.
- Success/failure
    - There were 24 patients who survived and 45 who died in our sample. Both are larger than 10.

Commentary: In the "Confidence interval" section of the rubric, there is no need to recheck conditions that have already been checked. The sample has not changed; if it met the "Random" and "10%" conditions before, it will meet them now.

So why recheck the success/failure condition?

Keep in mind that in a hypothesis test, we temporarily assume the null is true. The null states that $p = 0.5$ and the resulting null distribution is, therefore, centered at $p = 0.5$. The success/failure condition is a condition that applies to the normal model we're using, and for a hypothesis test, that's the null model.

By contrast, a confidence interval is making no assumption about the "true" value of $p$. The inferential goal of a confidence interval is to try to capture the true value of $p$, so we certainly cannot make any assumptions about it. Therefore, we go back to the original way we learned about the success/failure condition. That is, we check the actual number of successes and failures.

### Calculate and graph the confidence interval. {#one-prop-ex-ci-calculate}


```r
alive_ci <- alive_test %>%
    get_confidence_interval(point_estimate = alive_prop, level = 0.95)
alive_ci
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["lower_ci"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["upper_ci"],"name":[2],"type":["dbl"],"align":["right"]}],"data":[{"1":"0.2354468","2":"0.4602054"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


```r
alive_test %>%
    visualize() +
    shade_confidence_interval(endpoints = alive_ci)
```

<img src="15-inference_for_one_proportion-web_files/figure-html/unnamed-chunk-18-1.png" width="672" />

Commentary: when we use a theoretical normal distribution, we have to compute the confidence interval a different way.

When we bootstrapped, we had many repetitions of a process that resulted in a sampling distribution. From all those, we could find the 2.5th percentile and the 97.5th percentile. Although we let the computer do it for us, the process is straightforward enough that we could do it by hand if we needed to. Just put all 1000 bootstrapped values in order, then go to the 25th and 975th position in the list.

We don't have a list of 1000 values when we use an abstract curve to represent our sampling distribution. Nevertheless, we can find the 2.5th percentile and the 97.5th percentile using the area under the normal curve as we saw in the last two chapters. We can do this "manually" with the `qdist` command, but we need the standard error first.

Didn't we calculate this earlier?

$$
SE = \sqrt{\frac{p_{alive}(1 - p_{alive})}{n}} = \sqrt{\frac{0.5(1 - 0.5)}{69}}
$$

Well...sort of. The value of $p_{alive}$ here is the value of the null hypothesis from the hypothesis test above. *However*, the hypothesis test is done. For a confidence interval, we have no information about any "null" value. There is no null anymore. It's irrelevant.

So what is the standard error for a confidence interval? Since we don't have $p_{alive}$, the best we can do is replace it with $\hat{p}_{alive}$:

$$
SE = \sqrt{\frac{\hat{p}_{alive} (1 - \hat{p}_{alive})}{n}} = \sqrt{\frac{0.3478261 (1 - 0.3478261 )}{69}}.
$$

We can let R do the heavy lifting here:


```r
SE2 <- sqrt(alive_prop * (1 - alive_prop) / 69)
SE2
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["stat"],"name":[1],"type":["dbl"],"align":["right"]}],"data":[{"1":"0.05733743"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

And now this number can go into `qdist` as our standard deviation:


```r
qdist("norm", p = c(0.025, 0.975), mean = 0.3478261, sd = 0.05733743, plot = FALSE)
```

```
## [1] 0.2354468 0.4602054
```

The numbers above are identical to the ones computed by the `infer` commands.

### State (but do not overstate) a contextually meaningful interpretation. {#one-prop-ex-ci-interpret}

We are 95% confident that the true percentage of heart transplant recipients who survive is captured in the interval (23.5446784%, 46.020539%).

Commentary: Note that when we state our contextually meaningful conclusion, we also convert the decimal proportions to percentages. Humans like percentages a lot better.

### If running a two-sided test, explain how the confidence interval reinforces the conclusion of the hypothesis test. {#one-prop-ex-two-sided}

We are not running a two-sided test, so this step is not applicable.

### When comparing two groups, comment on the effect size and the practical significance of the result. {#one-prop-ex-effect}

This is not applicable here because we are not comparing two groups. We are looking at the survival percentage in only one group of patients, those who had a heart transplant.


## Your turn {#one-prop-your-turn}

Follow the rubric to answer the following research question:

Some heart transplant candidates have already had a prior surgery. Use the variable `prior` in the `heart_transplant` data set to determine if fewer than 50% of patients have had a prior surgery. (To be clear, you are being asked to perform a one-sided test again.) **Be sure to use the full `heart_transplant` data, not the modified `heart_transplant2` from the previous example.**

The rubric outline is reproduced below. You may refer to the worked example above and modify it accordingly. Remember to strip out all the commentary. That is just exposition for your benefit in understanding the steps, but is not meant to form part of the formal inference process.

Another word of warning: the copy/paste process is not a substitute for your brain. You will often need to modify more than just the names of the tibbles and variables to adapt the worked examples to your own work. For example, if you run a two-sided test instead of a one-sided test, there are a few places that have to be adjusted accordingly. Understanding the sampling distribution model and the computation of the P-value goes a long way toward understanding the changes that must be made. Do not blindly copy and paste code without understanding what it does. And you should **never** copy and paste text. All the sentences and paragraphs you write are expressions of your own analysis. They must reflect your own understanding of the inferential process.

**Also, so that your answers here don't mess up the code chunks above, use new variable names everywhere. In particular, you should use `prior_test`(instead of `alive_test`) to store the results of your hypothesis test. Make other corresponding changes as necessary, like `prior_test_p` instead of `alive_test_p`, for example.**

##### Exploratory data analysis {-}

###### Use data documentation (help files, code books, Google, etc.) to determine as much as possible about the data provenance and structure. {-}

::: {.answer}

Please write up your answer here.


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

[Remember that you are using the full `heart_transplant` data, so your sample size should be larger here than in the example above.]

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

[Remember that you are using the full `heart_transplant` data, so the number of successes and failures will be different here than in the example above.]

::: {.answer}

Please write up your answer here. (Some conditions may require R code as well.)

:::


##### Mechanics {-}

[Be sure to use `heart_transplant` everywhere and not `heart_transplant2`!]

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

###### Calculate the confidence interval. {-}

::: {.answer}


```r
# Add code here to calculate the confidence interval.
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


## Conclusion {#one-prop-conclusion}

When certain conditions are met, we can use a theoretical normal model---a perfectly symmetric bell curve---as a sampling distribution model in hypothesis testing. Because this does not require drawing many samples, it is faster and cleaner than simulation. Of course, on modern computing devices, drawing even thousands of simulated samples is not very time consuming, and the code we write doesn't really change much. Given the additional success/failure condition that has to met, it's worth considering the pros and cons of using a normal model instead of simulating the sampling distribution. Similarly, confidence intervals can be obtained directly from the percentiles of the normal model without the need to obtain bootstrapped samples.

### Preparing and submitting your assignment {#one-prop-prep}

1. From the "Run" menu, select "Restart R and Run All Chunks".
2. Deal with any code errors that crop up. Repeat steps 1–-2 until there are no more code errors.
3. Spell check your document by clicking the icon with "ABC" and a check mark.
4. Hit the "Preview" button one last time to generate the final draft of the `.nb.html` file.
5. Proofread the HTML file carefully. If there are errors, go back and fix them, then repeat steps 1--5 again.

If you have completed this chapter as part of a statistics course, follow the directions you receive from your professor to submit your assignment.


