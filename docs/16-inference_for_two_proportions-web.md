# Inference for two proportions {#two-prop}

<!-- Please don't mess with the next few lines! -->
<style>h5{font-size:2em;color:#0000FF}h6{font-size:1.5em;color:#0000FF}div.answer{margin-left:5%;border:1px solid #0000FF;border-left-width:10px;padding:25px} div.summary{background-color:rgba(30,144,255,0.1);border:3px double #0000FF;padding:25px}</style><p style="color:#ffffff">2.0</p>
<!-- Please don't mess with the previous few lines! -->


::: {.summary}

### Functions introduced in this chapter {-}

No new R functions are introduced here.

:::


## Introduction

In this chapter, we revisit the idea of inference for two proportions, but this time using a normal model as the sampling distribution model.

### Install new packages

There are no new packages used in this chapter.

### Download the R notebook file

Check the upper-right corner in RStudio to make sure you're in your `intro_stats` project. Then click on the following link to download this chapter as an R notebook file (`.Rmd`).

<a href = "https://vectorposse.github.io/intro_stats/chapter_downloads/16-inference_for_two_proportions.Rmd" download>https://vectorposse.github.io/intro_stats/chapter_downloads/16-inference_for_two_proportions.Rmd</a>

Once the file is downloaded, move it to your project folder in RStudio and open it there.

### Restart R and run all chunks

In RStudio, select "Restart R and Run All Chunks" from the "Run" menu.


## Load packages

We load the standard `tidyverse`, `janitor` and `infer` packages as well as the `MASS` package for the `Melanoma` data.


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
library(MASS)
```

```
## 
## Attaching package: 'MASS'
## 
## The following object is masked from 'package:dplyr':
## 
##     select
```


## Research question

In an earlier chapter, we used the data set `Melanoma` from the `MASS` package to explore the possibility of a sex bias among patients with melanoma. A related question is whether male or females are more likely to die from melanoma. In this case, we are thinking of `status` as the response variable and `sex` as the predictor variable.


## The sampling distribution model for two proportions

When we simulated a sampling distribution using randomization (shuffling the values of the predictor variable), it looked like the simulated sampling distribution was roughly normal. Therefore, we should be able to use a normal model in place of randomization when we want to perform statistical inference.

The question is, "Which normal model?" In other words, what is the mean and standard deviation we should use?

Since we have two groups, let's call the true proportion of success $p_{1}$ for group 1 and $p_{2}$ for group 2. Therefore, the true difference between groups 1 and 2 in the population is $p_{1} - p_{2}$. If we sample repeatedly from groups 1 and 2 and form many sample differences $\hat{p}_{1} - \hat{p}_{2}$, we should expect most of the values $\hat{p}_{1} - \hat{p}_{2}$ to be close to the true difference $p_{1} - p_{2}$. In other words, the sampling distribution is centered at a mean of $p_{1} - p_{2}$.

What about the standard error? This is much more technical and complicated. Here is the formula, whose derivation is outside the scope of the course:

$$
\sqrt{\frac{p_{1} (1 - p_{1})}{n_{1}} + \frac{p_{2} (1 - p_{2})}{n_{2}}}.
$$

So the somewhat complicated normal model is

$$
N\left( p_{1} - p_{2}, \sqrt{\frac{p_{1} (1 - p_{1})}{n_{1}} + \frac{p_{2} (1 - p_{2})}{n_{2}}} \right).
$$

When we ran hypothesis tests for one proportion, the true proportion $p$ was assumed to be known, set equal to some null value. Therefore, we could calculate the standard error $\sqrt{\frac{p(1 - p)}{n}}$ under the assumption of the null.

We also have a null hypothesis for two proportions. When comparing two groups, the default assumption is that the two groups are the same. This translates into the mathematical statement $p_{1} - p_{2} = 0$ (i.e., there is no difference between $p_{1}$ and $p_{2}$).

But there is a problem here. Although we are assuming something about the difference $p_{1} - p_{2}$, we are not assuming anything about the actual values of $p_{1}$ and $p_{2}$. For example, both groups could be 0.3, or 0.6, or 0.92, or whatever, and the difference between the groups would still be zero.

Without values of $p_{1}$ and $p_{2}$, we cannot plug anything into the standard error formula above. One easy "cheat" is to just use the sample values $\hat{p}_{1}$ and $\hat{p}_{2}$:

$$
SE = \sqrt{\frac{\hat{p}_{1} (1 - \hat{p}_{1})}{n_{1}} + \frac{\hat{p}_{2} (1 - \hat{p}_{2})}{n_{2}}}.
$$

There is a more sophisticated way to address this called "pooling". This more advanced concept is covered in an optional appendix to this chapter.


## Inference for two proportions

Below is a fully-worked example of inference (hypothesis test and confidence interval) for two proportions. When you work your own example, you can thoughtfully copy and paste the R code, making changes as necessary.

The example below will pause frequently for commentary on the steps, especially where their execution will be different from what you've seen before when you used randomization. When it's your turn to work through another example on your own, you should follow the outline of the rubric, but you should **not** copy and paste the commentary that accompanies it.


## Exploratory data analysis

### Use data documentation (help files, code books, Google, etc.) to determine as much as possible about the data provenance and structure.

Type `?Melanoma` at the Console to read the help file. We discussed this data back in Chapter 11 and determined that it was difficult, if not impossible, to discover anything useful about the true provenance of the data. We can, at least, print the data out and examine the variables 


```r
Melanoma
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["time"],"name":[1],"type":["int"],"align":["right"]},{"label":["status"],"name":[2],"type":["int"],"align":["right"]},{"label":["sex"],"name":[3],"type":["int"],"align":["right"]},{"label":["age"],"name":[4],"type":["int"],"align":["right"]},{"label":["year"],"name":[5],"type":["int"],"align":["right"]},{"label":["thickness"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["ulcer"],"name":[7],"type":["int"],"align":["right"]}],"data":[{"1":"10","2":"3","3":"1","4":"76","5":"1972","6":"6.76","7":"1","_rn_":"1"},{"1":"30","2":"3","3":"1","4":"56","5":"1968","6":"0.65","7":"0","_rn_":"2"},{"1":"35","2":"2","3":"1","4":"41","5":"1977","6":"1.34","7":"0","_rn_":"3"},{"1":"99","2":"3","3":"0","4":"71","5":"1968","6":"2.90","7":"0","_rn_":"4"},{"1":"185","2":"1","3":"1","4":"52","5":"1965","6":"12.08","7":"1","_rn_":"5"},{"1":"204","2":"1","3":"1","4":"28","5":"1971","6":"4.84","7":"1","_rn_":"6"},{"1":"210","2":"1","3":"1","4":"77","5":"1972","6":"5.16","7":"1","_rn_":"7"},{"1":"232","2":"3","3":"0","4":"60","5":"1974","6":"3.22","7":"1","_rn_":"8"},{"1":"232","2":"1","3":"1","4":"49","5":"1968","6":"12.88","7":"1","_rn_":"9"},{"1":"279","2":"1","3":"0","4":"68","5":"1971","6":"7.41","7":"1","_rn_":"10"},{"1":"295","2":"1","3":"0","4":"53","5":"1969","6":"4.19","7":"1","_rn_":"11"},{"1":"355","2":"3","3":"0","4":"64","5":"1972","6":"0.16","7":"1","_rn_":"12"},{"1":"386","2":"1","3":"0","4":"68","5":"1965","6":"3.87","7":"1","_rn_":"13"},{"1":"426","2":"1","3":"1","4":"63","5":"1970","6":"4.84","7":"1","_rn_":"14"},{"1":"469","2":"1","3":"0","4":"14","5":"1969","6":"2.42","7":"1","_rn_":"15"},{"1":"493","2":"3","3":"1","4":"72","5":"1971","6":"12.56","7":"1","_rn_":"16"},{"1":"529","2":"1","3":"1","4":"46","5":"1971","6":"5.80","7":"1","_rn_":"17"},{"1":"621","2":"1","3":"1","4":"72","5":"1972","6":"7.06","7":"1","_rn_":"18"},{"1":"629","2":"1","3":"1","4":"95","5":"1968","6":"5.48","7":"1","_rn_":"19"},{"1":"659","2":"1","3":"1","4":"54","5":"1972","6":"7.73","7":"1","_rn_":"20"},{"1":"667","2":"1","3":"0","4":"89","5":"1968","6":"13.85","7":"1","_rn_":"21"},{"1":"718","2":"1","3":"1","4":"25","5":"1967","6":"2.34","7":"1","_rn_":"22"},{"1":"752","2":"1","3":"1","4":"37","5":"1973","6":"4.19","7":"1","_rn_":"23"},{"1":"779","2":"1","3":"1","4":"43","5":"1967","6":"4.04","7":"1","_rn_":"24"},{"1":"793","2":"1","3":"1","4":"68","5":"1970","6":"4.84","7":"1","_rn_":"25"},{"1":"817","2":"1","3":"0","4":"67","5":"1966","6":"0.32","7":"0","_rn_":"26"},{"1":"826","2":"3","3":"0","4":"86","5":"1965","6":"8.54","7":"1","_rn_":"27"},{"1":"833","2":"1","3":"0","4":"56","5":"1971","6":"2.58","7":"1","_rn_":"28"},{"1":"858","2":"1","3":"0","4":"16","5":"1967","6":"3.56","7":"0","_rn_":"29"},{"1":"869","2":"1","3":"0","4":"42","5":"1965","6":"3.54","7":"0","_rn_":"30"},{"1":"872","2":"1","3":"0","4":"65","5":"1968","6":"0.97","7":"0","_rn_":"31"},{"1":"967","2":"1","3":"1","4":"52","5":"1970","6":"4.83","7":"1","_rn_":"32"},{"1":"977","2":"1","3":"1","4":"58","5":"1967","6":"1.62","7":"1","_rn_":"33"},{"1":"982","2":"1","3":"0","4":"60","5":"1970","6":"6.44","7":"1","_rn_":"34"},{"1":"1041","2":"1","3":"1","4":"68","5":"1967","6":"14.66","7":"0","_rn_":"35"},{"1":"1055","2":"1","3":"0","4":"75","5":"1967","6":"2.58","7":"1","_rn_":"36"},{"1":"1062","2":"1","3":"1","4":"19","5":"1966","6":"3.87","7":"1","_rn_":"37"},{"1":"1075","2":"1","3":"1","4":"66","5":"1971","6":"3.54","7":"1","_rn_":"38"},{"1":"1156","2":"1","3":"0","4":"56","5":"1970","6":"1.34","7":"1","_rn_":"39"},{"1":"1228","2":"1","3":"1","4":"46","5":"1973","6":"2.24","7":"1","_rn_":"40"},{"1":"1252","2":"1","3":"0","4":"58","5":"1971","6":"3.87","7":"1","_rn_":"41"},{"1":"1271","2":"1","3":"0","4":"74","5":"1971","6":"3.54","7":"1","_rn_":"42"},{"1":"1312","2":"1","3":"0","4":"65","5":"1970","6":"17.42","7":"1","_rn_":"43"},{"1":"1427","2":"3","3":"1","4":"64","5":"1972","6":"1.29","7":"0","_rn_":"44"},{"1":"1435","2":"1","3":"1","4":"27","5":"1969","6":"3.22","7":"0","_rn_":"45"},{"1":"1499","2":"2","3":"1","4":"73","5":"1973","6":"1.29","7":"0","_rn_":"46"},{"1":"1506","2":"1","3":"1","4":"56","5":"1970","6":"4.51","7":"1","_rn_":"47"},{"1":"1508","2":"2","3":"1","4":"63","5":"1973","6":"8.38","7":"1","_rn_":"48"},{"1":"1510","2":"2","3":"0","4":"69","5":"1973","6":"1.94","7":"0","_rn_":"49"},{"1":"1512","2":"2","3":"0","4":"77","5":"1973","6":"0.16","7":"0","_rn_":"50"},{"1":"1516","2":"1","3":"1","4":"80","5":"1968","6":"2.58","7":"1","_rn_":"51"},{"1":"1525","2":"3","3":"0","4":"76","5":"1970","6":"1.29","7":"1","_rn_":"52"},{"1":"1542","2":"2","3":"0","4":"65","5":"1973","6":"0.16","7":"0","_rn_":"53"},{"1":"1548","2":"1","3":"0","4":"61","5":"1972","6":"1.62","7":"0","_rn_":"54"},{"1":"1557","2":"2","3":"0","4":"26","5":"1973","6":"1.29","7":"0","_rn_":"55"},{"1":"1560","2":"1","3":"0","4":"57","5":"1973","6":"2.10","7":"0","_rn_":"56"},{"1":"1563","2":"2","3":"0","4":"45","5":"1973","6":"0.32","7":"0","_rn_":"57"},{"1":"1584","2":"1","3":"1","4":"31","5":"1970","6":"0.81","7":"0","_rn_":"58"},{"1":"1605","2":"2","3":"0","4":"36","5":"1973","6":"1.13","7":"0","_rn_":"59"},{"1":"1621","2":"1","3":"0","4":"46","5":"1972","6":"5.16","7":"1","_rn_":"60"},{"1":"1627","2":"2","3":"0","4":"43","5":"1973","6":"1.62","7":"0","_rn_":"61"},{"1":"1634","2":"2","3":"0","4":"68","5":"1973","6":"1.37","7":"0","_rn_":"62"},{"1":"1641","2":"2","3":"1","4":"57","5":"1973","6":"0.24","7":"0","_rn_":"63"},{"1":"1641","2":"2","3":"0","4":"57","5":"1973","6":"0.81","7":"0","_rn_":"64"},{"1":"1648","2":"2","3":"0","4":"55","5":"1973","6":"1.29","7":"0","_rn_":"65"},{"1":"1652","2":"2","3":"0","4":"58","5":"1973","6":"1.29","7":"0","_rn_":"66"},{"1":"1654","2":"2","3":"1","4":"20","5":"1973","6":"0.97","7":"0","_rn_":"67"},{"1":"1654","2":"2","3":"0","4":"67","5":"1973","6":"1.13","7":"0","_rn_":"68"},{"1":"1667","2":"1","3":"0","4":"44","5":"1971","6":"5.80","7":"1","_rn_":"69"},{"1":"1678","2":"2","3":"0","4":"59","5":"1973","6":"1.29","7":"0","_rn_":"70"},{"1":"1685","2":"2","3":"0","4":"32","5":"1973","6":"0.48","7":"0","_rn_":"71"},{"1":"1690","2":"1","3":"1","4":"83","5":"1971","6":"1.62","7":"0","_rn_":"72"},{"1":"1710","2":"2","3":"0","4":"55","5":"1973","6":"2.26","7":"0","_rn_":"73"},{"1":"1710","2":"2","3":"1","4":"15","5":"1973","6":"0.58","7":"0","_rn_":"74"},{"1":"1726","2":"1","3":"0","4":"58","5":"1970","6":"0.97","7":"1","_rn_":"75"},{"1":"1745","2":"2","3":"0","4":"47","5":"1973","6":"2.58","7":"1","_rn_":"76"},{"1":"1762","2":"2","3":"0","4":"54","5":"1973","6":"0.81","7":"0","_rn_":"77"},{"1":"1779","2":"2","3":"1","4":"55","5":"1973","6":"3.54","7":"1","_rn_":"78"},{"1":"1787","2":"2","3":"1","4":"38","5":"1973","6":"0.97","7":"0","_rn_":"79"},{"1":"1787","2":"2","3":"0","4":"41","5":"1973","6":"1.78","7":"1","_rn_":"80"},{"1":"1793","2":"2","3":"0","4":"56","5":"1973","6":"1.94","7":"0","_rn_":"81"},{"1":"1804","2":"2","3":"0","4":"48","5":"1973","6":"1.29","7":"0","_rn_":"82"},{"1":"1812","2":"2","3":"1","4":"44","5":"1973","6":"3.22","7":"1","_rn_":"83"},{"1":"1836","2":"2","3":"0","4":"70","5":"1972","6":"1.53","7":"0","_rn_":"84"},{"1":"1839","2":"2","3":"0","4":"40","5":"1972","6":"1.29","7":"0","_rn_":"85"},{"1":"1839","2":"2","3":"1","4":"53","5":"1972","6":"1.62","7":"1","_rn_":"86"},{"1":"1854","2":"2","3":"0","4":"65","5":"1972","6":"1.62","7":"1","_rn_":"87"},{"1":"1856","2":"2","3":"1","4":"54","5":"1972","6":"0.32","7":"0","_rn_":"88"},{"1":"1860","2":"3","3":"1","4":"71","5":"1969","6":"4.84","7":"1","_rn_":"89"},{"1":"1864","2":"2","3":"0","4":"49","5":"1972","6":"1.29","7":"0","_rn_":"90"},{"1":"1899","2":"2","3":"0","4":"55","5":"1972","6":"0.97","7":"0","_rn_":"91"},{"1":"1914","2":"2","3":"0","4":"69","5":"1972","6":"3.06","7":"0","_rn_":"92"},{"1":"1919","2":"2","3":"1","4":"83","5":"1972","6":"3.54","7":"0","_rn_":"93"},{"1":"1920","2":"2","3":"1","4":"60","5":"1972","6":"1.62","7":"1","_rn_":"94"},{"1":"1927","2":"2","3":"1","4":"40","5":"1972","6":"2.58","7":"1","_rn_":"95"},{"1":"1933","2":"1","3":"0","4":"77","5":"1972","6":"1.94","7":"0","_rn_":"96"},{"1":"1942","2":"2","3":"0","4":"35","5":"1972","6":"0.81","7":"0","_rn_":"97"},{"1":"1955","2":"2","3":"0","4":"46","5":"1972","6":"7.73","7":"1","_rn_":"98"},{"1":"1956","2":"2","3":"0","4":"34","5":"1972","6":"0.97","7":"0","_rn_":"99"},{"1":"1958","2":"2","3":"0","4":"69","5":"1972","6":"12.88","7":"0","_rn_":"100"},{"1":"1963","2":"2","3":"0","4":"60","5":"1972","6":"2.58","7":"0","_rn_":"101"},{"1":"1970","2":"2","3":"1","4":"84","5":"1972","6":"4.09","7":"1","_rn_":"102"},{"1":"2005","2":"2","3":"0","4":"66","5":"1972","6":"0.64","7":"0","_rn_":"103"},{"1":"2007","2":"2","3":"1","4":"56","5":"1972","6":"0.97","7":"0","_rn_":"104"},{"1":"2011","2":"2","3":"0","4":"75","5":"1972","6":"3.22","7":"1","_rn_":"105"},{"1":"2024","2":"2","3":"0","4":"36","5":"1972","6":"1.62","7":"0","_rn_":"106"},{"1":"2028","2":"2","3":"1","4":"52","5":"1972","6":"3.87","7":"1","_rn_":"107"},{"1":"2038","2":"2","3":"0","4":"58","5":"1972","6":"0.32","7":"1","_rn_":"108"},{"1":"2056","2":"2","3":"0","4":"39","5":"1972","6":"0.32","7":"0","_rn_":"109"},{"1":"2059","2":"2","3":"1","4":"68","5":"1972","6":"3.22","7":"1","_rn_":"110"},{"1":"2061","2":"1","3":"1","4":"71","5":"1968","6":"2.26","7":"0","_rn_":"111"},{"1":"2062","2":"1","3":"0","4":"52","5":"1965","6":"3.06","7":"0","_rn_":"112"},{"1":"2075","2":"2","3":"1","4":"55","5":"1972","6":"2.58","7":"1","_rn_":"113"},{"1":"2085","2":"3","3":"0","4":"66","5":"1970","6":"0.65","7":"0","_rn_":"114"},{"1":"2102","2":"2","3":"1","4":"35","5":"1972","6":"1.13","7":"0","_rn_":"115"},{"1":"2103","2":"1","3":"1","4":"44","5":"1966","6":"0.81","7":"0","_rn_":"116"},{"1":"2104","2":"2","3":"0","4":"72","5":"1972","6":"0.97","7":"0","_rn_":"117"},{"1":"2108","2":"1","3":"0","4":"58","5":"1969","6":"1.76","7":"1","_rn_":"118"},{"1":"2112","2":"2","3":"0","4":"54","5":"1972","6":"1.94","7":"1","_rn_":"119"},{"1":"2150","2":"2","3":"0","4":"33","5":"1972","6":"0.65","7":"0","_rn_":"120"},{"1":"2156","2":"2","3":"0","4":"45","5":"1972","6":"0.97","7":"0","_rn_":"121"},{"1":"2165","2":"2","3":"1","4":"62","5":"1972","6":"5.64","7":"0","_rn_":"122"},{"1":"2209","2":"2","3":"0","4":"72","5":"1971","6":"9.66","7":"0","_rn_":"123"},{"1":"2227","2":"2","3":"0","4":"51","5":"1971","6":"0.10","7":"0","_rn_":"124"},{"1":"2227","2":"2","3":"1","4":"77","5":"1971","6":"5.48","7":"1","_rn_":"125"},{"1":"2256","2":"1","3":"0","4":"43","5":"1971","6":"2.26","7":"1","_rn_":"126"},{"1":"2264","2":"2","3":"0","4":"65","5":"1971","6":"4.83","7":"1","_rn_":"127"},{"1":"2339","2":"2","3":"0","4":"63","5":"1971","6":"0.97","7":"0","_rn_":"128"},{"1":"2361","2":"2","3":"1","4":"60","5":"1971","6":"0.97","7":"0","_rn_":"129"},{"1":"2387","2":"2","3":"0","4":"50","5":"1971","6":"5.16","7":"1","_rn_":"130"},{"1":"2388","2":"1","3":"1","4":"40","5":"1966","6":"0.81","7":"0","_rn_":"131"},{"1":"2403","2":"2","3":"0","4":"67","5":"1971","6":"2.90","7":"1","_rn_":"132"},{"1":"2426","2":"2","3":"0","4":"69","5":"1971","6":"3.87","7":"0","_rn_":"133"},{"1":"2426","2":"2","3":"0","4":"74","5":"1971","6":"1.94","7":"1","_rn_":"134"},{"1":"2431","2":"2","3":"0","4":"49","5":"1971","6":"0.16","7":"0","_rn_":"135"},{"1":"2460","2":"2","3":"0","4":"47","5":"1971","6":"0.64","7":"0","_rn_":"136"},{"1":"2467","2":"1","3":"0","4":"42","5":"1965","6":"2.26","7":"1","_rn_":"137"},{"1":"2492","2":"2","3":"0","4":"54","5":"1971","6":"1.45","7":"0","_rn_":"138"},{"1":"2493","2":"2","3":"1","4":"72","5":"1971","6":"4.82","7":"1","_rn_":"139"},{"1":"2521","2":"2","3":"0","4":"45","5":"1971","6":"1.29","7":"1","_rn_":"140"},{"1":"2542","2":"2","3":"1","4":"67","5":"1971","6":"7.89","7":"1","_rn_":"141"},{"1":"2559","2":"2","3":"0","4":"48","5":"1970","6":"0.81","7":"1","_rn_":"142"},{"1":"2565","2":"1","3":"1","4":"34","5":"1970","6":"3.54","7":"1","_rn_":"143"},{"1":"2570","2":"2","3":"0","4":"44","5":"1970","6":"1.29","7":"0","_rn_":"144"},{"1":"2660","2":"2","3":"0","4":"31","5":"1970","6":"0.64","7":"0","_rn_":"145"},{"1":"2666","2":"2","3":"0","4":"42","5":"1970","6":"3.22","7":"1","_rn_":"146"},{"1":"2676","2":"2","3":"0","4":"24","5":"1970","6":"1.45","7":"1","_rn_":"147"},{"1":"2738","2":"2","3":"0","4":"58","5":"1970","6":"0.48","7":"0","_rn_":"148"},{"1":"2782","2":"1","3":"1","4":"78","5":"1969","6":"1.94","7":"0","_rn_":"149"},{"1":"2787","2":"2","3":"1","4":"62","5":"1970","6":"0.16","7":"0","_rn_":"150"},{"1":"2984","2":"2","3":"1","4":"70","5":"1969","6":"0.16","7":"0","_rn_":"151"},{"1":"3032","2":"2","3":"0","4":"35","5":"1969","6":"1.29","7":"0","_rn_":"152"},{"1":"3040","2":"2","3":"0","4":"61","5":"1969","6":"1.94","7":"0","_rn_":"153"},{"1":"3042","2":"1","3":"0","4":"54","5":"1967","6":"3.54","7":"1","_rn_":"154"},{"1":"3067","2":"2","3":"0","4":"29","5":"1969","6":"0.81","7":"0","_rn_":"155"},{"1":"3079","2":"2","3":"1","4":"64","5":"1969","6":"0.65","7":"0","_rn_":"156"},{"1":"3101","2":"2","3":"1","4":"47","5":"1969","6":"7.09","7":"0","_rn_":"157"},{"1":"3144","2":"2","3":"1","4":"62","5":"1969","6":"0.16","7":"0","_rn_":"158"},{"1":"3152","2":"2","3":"0","4":"32","5":"1969","6":"1.62","7":"0","_rn_":"159"},{"1":"3154","2":"3","3":"1","4":"49","5":"1969","6":"1.62","7":"0","_rn_":"160"},{"1":"3180","2":"2","3":"0","4":"25","5":"1969","6":"1.29","7":"0","_rn_":"161"},{"1":"3182","2":"3","3":"1","4":"49","5":"1966","6":"6.12","7":"0","_rn_":"162"},{"1":"3185","2":"2","3":"0","4":"64","5":"1969","6":"0.48","7":"0","_rn_":"163"},{"1":"3199","2":"2","3":"0","4":"36","5":"1969","6":"0.64","7":"0","_rn_":"164"},{"1":"3228","2":"2","3":"0","4":"58","5":"1969","6":"3.22","7":"1","_rn_":"165"},{"1":"3229","2":"2","3":"0","4":"37","5":"1969","6":"1.94","7":"0","_rn_":"166"},{"1":"3278","2":"2","3":"1","4":"54","5":"1969","6":"2.58","7":"0","_rn_":"167"},{"1":"3297","2":"2","3":"0","4":"61","5":"1968","6":"2.58","7":"1","_rn_":"168"},{"1":"3328","2":"2","3":"1","4":"31","5":"1968","6":"0.81","7":"0","_rn_":"169"},{"1":"3330","2":"2","3":"1","4":"61","5":"1968","6":"0.81","7":"1","_rn_":"170"},{"1":"3338","2":"1","3":"0","4":"60","5":"1967","6":"3.22","7":"1","_rn_":"171"},{"1":"3383","2":"2","3":"0","4":"43","5":"1968","6":"0.32","7":"0","_rn_":"172"},{"1":"3384","2":"2","3":"0","4":"68","5":"1968","6":"3.22","7":"1","_rn_":"173"},{"1":"3385","2":"2","3":"0","4":"4","5":"1968","6":"2.74","7":"0","_rn_":"174"},{"1":"3388","2":"2","3":"1","4":"60","5":"1968","6":"4.84","7":"1","_rn_":"175"},{"1":"3402","2":"2","3":"1","4":"50","5":"1968","6":"1.62","7":"0","_rn_":"176"},{"1":"3441","2":"2","3":"0","4":"20","5":"1968","6":"0.65","7":"0","_rn_":"177"},{"1":"3458","2":"3","3":"0","4":"54","5":"1967","6":"1.45","7":"0","_rn_":"178"},{"1":"3459","2":"2","3":"0","4":"29","5":"1968","6":"0.65","7":"0","_rn_":"179"},{"1":"3459","2":"2","3":"1","4":"56","5":"1968","6":"1.29","7":"1","_rn_":"180"},{"1":"3476","2":"2","3":"0","4":"60","5":"1968","6":"1.62","7":"0","_rn_":"181"},{"1":"3523","2":"2","3":"0","4":"46","5":"1968","6":"3.54","7":"0","_rn_":"182"},{"1":"3667","2":"2","3":"0","4":"42","5":"1967","6":"3.22","7":"0","_rn_":"183"},{"1":"3695","2":"2","3":"0","4":"34","5":"1967","6":"0.65","7":"0","_rn_":"184"},{"1":"3695","2":"2","3":"0","4":"56","5":"1967","6":"1.03","7":"0","_rn_":"185"},{"1":"3776","2":"2","3":"1","4":"12","5":"1967","6":"7.09","7":"1","_rn_":"186"},{"1":"3776","2":"2","3":"0","4":"21","5":"1967","6":"1.29","7":"1","_rn_":"187"},{"1":"3830","2":"2","3":"1","4":"46","5":"1967","6":"0.65","7":"0","_rn_":"188"},{"1":"3856","2":"2","3":"0","4":"49","5":"1967","6":"1.78","7":"0","_rn_":"189"},{"1":"3872","2":"2","3":"0","4":"35","5":"1967","6":"12.24","7":"1","_rn_":"190"},{"1":"3909","2":"2","3":"1","4":"42","5":"1967","6":"8.06","7":"1","_rn_":"191"},{"1":"3968","2":"2","3":"0","4":"47","5":"1967","6":"0.81","7":"0","_rn_":"192"},{"1":"4001","2":"2","3":"0","4":"69","5":"1967","6":"2.10","7":"0","_rn_":"193"},{"1":"4103","2":"2","3":"0","4":"52","5":"1966","6":"3.87","7":"0","_rn_":"194"},{"1":"4119","2":"2","3":"1","4":"52","5":"1966","6":"0.65","7":"0","_rn_":"195"},{"1":"4124","2":"2","3":"0","4":"30","5":"1966","6":"1.94","7":"1","_rn_":"196"},{"1":"4207","2":"2","3":"1","4":"22","5":"1966","6":"0.65","7":"0","_rn_":"197"},{"1":"4310","2":"2","3":"1","4":"55","5":"1966","6":"2.10","7":"0","_rn_":"198"},{"1":"4390","2":"2","3":"0","4":"26","5":"1965","6":"1.94","7":"1","_rn_":"199"},{"1":"4479","2":"2","3":"0","4":"19","5":"1965","6":"1.13","7":"1","_rn_":"200"},{"1":"4492","2":"2","3":"1","4":"29","5":"1965","6":"7.06","7":"1","_rn_":"201"},{"1":"4668","2":"2","3":"0","4":"40","5":"1965","6":"6.12","7":"0","_rn_":"202"},{"1":"4688","2":"2","3":"0","4":"42","5":"1965","6":"0.48","7":"0","_rn_":"203"},{"1":"4926","2":"2","3":"0","4":"50","5":"1964","6":"2.26","7":"0","_rn_":"204"},{"1":"5565","2":"2","3":"0","4":"41","5":"1962","6":"2.90","7":"0","_rn_":"205"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>



```r
glimpse(Melanoma)
```

```
## Rows: 205
## Columns: 7
## $ time      <int> 10, 30, 35, 99, 185, 204, 210, 232, 232, 279, 295, 355, 386,…
## $ status    <int> 3, 3, 2, 3, 1, 1, 1, 3, 1, 1, 1, 3, 1, 1, 1, 3, 1, 1, 1, 1, …
## $ sex       <int> 1, 1, 1, 0, 1, 1, 1, 0, 1, 0, 0, 0, 0, 1, 0, 1, 1, 1, 1, 1, …
## $ age       <int> 76, 56, 41, 71, 52, 28, 77, 60, 49, 68, 53, 64, 68, 63, 14, …
## $ year      <int> 1972, 1968, 1977, 1968, 1965, 1971, 1972, 1974, 1968, 1971, …
## $ thickness <dbl> 6.76, 0.65, 1.34, 2.90, 12.08, 4.84, 5.16, 3.22, 12.88, 7.41…
## $ ulcer     <int> 1, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, …
```

### Prepare the data for analysis.

The two variables of interest are `status` and `sex`. We are considering them as categorical variables, but they are recorded numerically in the data frame. We convert them to proper factor variables and put them in their own data frame using the help file to identify the levels and labels we need.

There is a minor hitch with `status`. The help file shows three categories: 1. died from melanoma, 2. alive, 3. dead from other causes. For two-proportion inference, it would be better to have two categories only, a success category and a failure category. Since our research question asks about deaths due to melanoma, the "success" condition is the one numbered 1 in the help file, "died from melanoma". That means we need to combine the other two categories into a single failure category. Perhaps we should call it "other". You can accomplish this by simply repeating the "other" label more than once in the `factor` command:


```r
Melanoma <- Melanoma %>%
    mutate(sex_fct = factor(sex,
                            levels = c(0, 1),
                            labels = c("female", "male")),
           status_fct = factor(status,
                               levels = c(1, 2, 3),
                               labels = c("died from melanoma", "other", "other")))
glimpse(Melanoma)
```

```
## Rows: 205
## Columns: 9
## $ time       <int> 10, 30, 35, 99, 185, 204, 210, 232, 232, 279, 295, 355, 386…
## $ status     <int> 3, 3, 2, 3, 1, 1, 1, 3, 1, 1, 1, 3, 1, 1, 1, 3, 1, 1, 1, 1,…
## $ sex        <int> 1, 1, 1, 0, 1, 1, 1, 0, 1, 0, 0, 0, 0, 1, 0, 1, 1, 1, 1, 1,…
## $ age        <int> 76, 56, 41, 71, 52, 28, 77, 60, 49, 68, 53, 64, 68, 63, 14,…
## $ year       <int> 1972, 1968, 1977, 1968, 1965, 1971, 1972, 1974, 1968, 1971,…
## $ thickness  <dbl> 6.76, 0.65, 1.34, 2.90, 12.08, 4.84, 5.16, 3.22, 12.88, 7.4…
## $ ulcer      <int> 1, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,…
## $ sex_fct    <fct> male, male, male, female, male, male, male, female, male, f…
## $ status_fct <fct> other, other, other, other, died from melanoma, died from m…
```

##### Exercise 1 {-}

Observe the new variables `sex_fct` and `status_fct` in the `glimpse` output above. How can we check that the categories got assigned correctly and match the original `sex` and `status` variables?

::: {.answer}

Please write up your answer here.

:::


### Make tables or plots to explore the data visually.

As these are two categorical variables, we should look at contingency tables (both counts and percentages). The variable `status` is the response and `sex` is the predictor.


```r
tabyl(Melanoma, status_fct, sex_fct) %>%
    adorn_totals()
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["status_fct"],"name":[1],"type":["fct"],"align":["left"]},{"label":["female"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["male"],"name":[3],"type":["dbl"],"align":["right"]}],"data":[{"1":"died from melanoma","2":"28","3":"29","_rn_":"1"},{"1":"other","2":"98","3":"50","_rn_":"2"},{"1":"Total","2":"126","3":"79","_rn_":"3"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


```r
tabyl(Melanoma, status_fct, sex_fct) %>%
    adorn_totals() %>%
    adorn_percentages("col") %>%
    adorn_pct_formatting()
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["status_fct"],"name":[1],"type":["fct"],"align":["left"]},{"label":["female"],"name":[2],"type":["chr"],"align":["left"]},{"label":["male"],"name":[3],"type":["chr"],"align":["left"]}],"data":[{"1":"died from melanoma","2":"22.2%","3":"36.7%","_rn_":"1"},{"1":"other","2":"77.8%","3":"63.3%","_rn_":"2"},{"1":"Total","2":"100.0%","3":"100.0%","_rn_":"3"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

Commentary: You can see why column percentages are necessary in a contingency table. There are 28 females and 29 males who died from melanoma, almost a tie. However, there are more females (126) than there are males (79) who have melanoma in this data set. So the *proportion* of males who died from melanoma is quite a bit larger.

    
## Hypotheses

### Identify the sample (or samples) and a reasonable population (or populations) of interest.

There are two samples: 126 female patients and 79 male patients in Denmark with malignant melanoma. In order for these samples to be representative of their respective populations, we should probably restrict our conclusions to the population of all females and males in Denmark with malignant melanoma, although we might be able to make the case that these females and males could be representative of people in other countries who have malignant melanoma.

### Express the null and alternative hypotheses as contextually meaningful full sentences.

$H_{0}:$ There is no difference between the rate at which women and men in Denmark die from malignant melanoma.

$H_{A}:$ There is a difference between the rate at which women and men in Denmark die from malignant melanoma.

OR

$H_{0}:$ In Denmark, death from malignant melanoma is independent of sex.

$H_{A}:$ In Denmark, death from malignant melanoma is associated with sex.

Commentary: Either of these forms is correct. The former makes it a little easier to figure out how to express the hypotheses mathematically in the next step. The latter reminds us that the `hypothesize` step of the `infer` pipeline will require a null of `independence`.

### Express the null and alternative hypotheses in symbols (when possible).

$H_{0}: p_{died, F} - p_{died, M} = 0$

$H_{A}: p_{died, F} - p_{died, M} \neq 0$

Commentary: The order in which you subtract is irrelevant to the inferential process. However, you should be sure that any future steps respect the order you choose here. To be on the safe side, it's always best to subtract in the order in which the factor was created. So in the contingency tables above, females are listed first, and that's because "female" was the first label we used when we created the `sex_fct` variable. So we'll subtract females minus males throughout the remaining steps.


## Model

### Identify the sampling distribution model.

We will use a normal model.

### Check the relevant conditions to ensure that model assumptions are met.

* Random
    - As observed in a previous chapter when we used this data set before, We have no information about how these samples were obtained. We hope the 126 female patients and 79 male patients are representative of other Danish patients with malignant melanoma.

* 10%
    - We don't know exactly how many people in Denmark suffer from malignant melanoma, but we could imagine over time it's more than 1260 females and 790 males.

* Success/Failure
    - Checking the contingency table above (the one with counts), we see the numbers 28 and 98 (the successes and failures among females), and 29 and 50 (the successes and failures among males). These are all larger than 10.

Commentary: Ideally, for the success/failure condition we would like to check $n_{1} p_{1}$, $n_{1} (1 - p_{1})$, $n_{2} p_{2}$, and $n_{2} (1 - p_{2})$; however, the null makes no claim about the values of $p_{1}$ and $p_{2}$. We do the next best thing and estimate these by substituting the sample proportions $\hat{p}_{1}$ and $\hat{p}_{2}$. But $n_{1} \hat{p}_{1}$ and $n_{2} \hat{p}_{2}$ are just the raw counts of successes in each group. Likewise, $n_{1} (1 - \hat{p}_{1})$ and $n_{2} (1 - \hat{p}_{2})$ are just the raw counts of failures in each group. That's why we can just read them off the contingency table.

For a more sophisticated approach, one could also use "pooled proportions". See the optional appendix to this chapter for more information.


## Mechanics

### Compute the test statistic.


```r
obs_diff <- Melanoma %>%
    observe(status_fct ~ sex_fct, success = "died from melanoma",
            stat = "diff in props", order = c("female", "male"))
obs_diff
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["stat"],"name":[1],"type":["dbl"],"align":["right"]}],"data":[{"1":"-0.1448664"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

The test statistic is the difference of proportions in the sample, $\hat{p}_{died, F} - \hat{p}_{died, M}$:

$$
\hat{p}_{died, F} - \hat{p}_{died, M} = 0.222 - 0.367 = -0.145
$$

As a z-score:


```r
obs_diff_z <- Melanoma %>%
    observe(status_fct ~ sex_fct, success = "died from melanoma",
            stat = "z", order = c("female", "male"))
obs_diff_z
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["stat"],"name":[1],"type":["dbl"],"align":["right"]}],"data":[{"1":"-2.253072"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


Commentary: We can confirm the value of the z-score manually just to make sure we understand where it comes from.

The standard error looks like the following:

$$
SE = \sqrt{\frac{\hat{p}_{died, F} (1 - \hat{p}_{died, F})}{n_{F}} + \frac{\hat{p}_{died, M} (1 - \hat{p}_{died, M})}{n_M}}
$$

Plugging in the numbers from the exploratory data analysis output:

$$
SE = \sqrt{\frac{0.222 (1 - 0.222)}{126} + \frac{0.367 (1 - 0.367)}{79}}
$$

In R,


```r
sqrt(0.222 * (1 - 0.222) / 126 + 0.367 * (1 - 0.367) / 79)
```

```
## [1] 0.06566131
```

Now our z-score formula is

$$
z = \frac{(\hat{p}_{died, F} - \hat{p}_{died, M}) - (p_{died, F} - p_{died, M})}{SE}
$$

The first term in the numerator $(\hat{p}_{died, F} - \hat{p}_{died, M})$ is our test statistic, -0.145. The second term in the numerator $(p_{died, F} - p_{died, M})$  is zero according to the null hypothesis.  Plugging all that in, along with the value of SE, gives

$$
z = \frac{-0.145 - 0}{0.066} \approx -2.2
$$

Other than a little rounding error (since we rounded everything in sight to three decimal places instead of keeping more precision), this is what the `infer` output also reported.

### Report the test statistic in context (when possible).

In our sample, there is a -14.4866385% difference between the rate at which women and men in Denmark die from malignant melanoma (meaning that males died at a higher rate).

The test statistic has a z score of -2.2530721. The difference in proportions between the rate at which women and men in Denmark die from malignant melanoma lies a bit more than 2 standard errors to the left of the null value.

Commentary: Note the phrase "meaning that males died at a higher rate". If you are looking at a difference, you must indicate the direction of the difference. Without that, we would know that there was a difference, but we would have no idea whether women or men die more from malignant melanoma. Once we know that we are subtracting female minus male, then given the values are negative, we can infer that males die from malignant melanoma more often than females in these samples.

### Plot the null distribution.


```r
status_sex_test <- Melanoma %>%
    specify(status_fct ~ sex_fct, success = "died from melanoma") %>%
    hypothesize(null = "independence") %>%
    assume(distribution = "z")
status_sex_test
```

```
## A Z distribution.
```


```r
status_sex_test %>%
    visualize() +
    shade_p_value(obs_stat = obs_diff_z, direction = "two-sided")
```

<img src="16-inference_for_two_proportions-web_files/figure-html/unnamed-chunk-11-1.png" width="672" />

Commentary: Remember that this is a two-sided test.The red line above is the location of the test statistic, but both tails are shaded and count toward the P-value.

### Calculate the P-value.


```r
status_sex_test_p <- status_sex_test %>%
    get_p_value(obs_stat = obs_diff_z, direction = "two-sided")
status_sex_test_p
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["p_value"],"name":[1],"type":["dbl"],"align":["right"]}],"data":[{"1":"0.0242546"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

### Interpret the P-value as a probability given the null.

The P-value is 0.0242546. If there were truly no difference between the rate at which women and men in Denmark die from malignant melanoma, there is only a 2.4254604% chance of seeing a difference in our data at least as extreme as what we saw.


## Conclusion

### State the statistical conclusion.

We reject the null hypothesis.

### State (but do not overstate) a contextually meaningful conclusion.

We have sufficient evidence to suggest that there is a difference between the rate at which women and men in Denmark die from malignant melanoma.

### Express reservations or uncertainty about the generalizability of the conclusion.

We echo the same concerns we had back in Chapter 11 when we first saw this data. We have no idea how these patients were sampled. Are these all the patients in Denmark with malignant melanoma over a certain period of time? Were they part of a convenience sample? As a result of our uncertainly about the sampling process, we can't be sure if the results generalize to a larger population, either in Denmark or especially outside of Denmark.

### Identify the possibility of either a Type I or Type II error and state what making such an error means in the context of the hypotheses.

If we have made a Type I error, then there would actually be no difference between the rate at which women and men in Denmark die from malignant melanoma, but our samples showed a significant difference.


## Confidence interval

### Check the relevant conditions to ensure that model assumptions are met.

None of the conditions have changed, so they don't need to be rechecked.

### Calculate and graph the confidence interval.


```r
status_sex_ci <- status_sex_test %>%
    get_confidence_interval(point_estimate = obs_diff, level = 0.95)
status_sex_ci
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["lower_ci"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["upper_ci"],"name":[2],"type":["dbl"],"align":["right"]}],"data":[{"1":"-0.2735793","2":"-0.01615351"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


```r
status_sex_test %>%
    visualize() +
    shade_confidence_interval(endpoints = status_sex_ci)
```

<img src="16-inference_for_two_proportions-web_files/figure-html/unnamed-chunk-14-1.png" width="672" />

### State (but do not overstate) a contextually meaningful interpretation.

We are 95% confident that the true difference between the rate at which women and men die from malignant melanoma is captured in the interval (-27.3579265%, -1.6153506%). (This difference is measured by calculating female minus male.)

Commentary: Note the addition of that last sentence. As we mentioned before, if you are looking at a difference, you must indicate the direction of the difference. We know that we are subtracting female minus male, So given that the values are negative, we can infer that males die from malignant melanoma more often than females---at least according to this confidence interval.

### If running a two-sided test, explain how the confidence interval reinforces the conclusion of the hypothesis test.

The confidence interval does not contain the null value of zero. Since zero is not a plausible value for the true difference between the rate at which women and men die from malignant melanoma, it makes sense that we rejected the null hypothesis.

### When comparing two groups, comment on the effect size and the practical significance of the result.

At the most extreme end of the confidence interval, -27.3579265% is a very large difference between females and males. If this outer value is close to the truth, males are at much more risk of melanoma than females (at least in Denmark at the time of the study). The other end of the confidence interval, -1.6153506%, is a negligible difference. If that number were close to the truth, it's not clear that the true difference would have practical significance in the real world.

Commentary: The P-value for the hypothesis test indicated that the results are *statistically significant*. But what does that really mean? It means that if the null were true, the probability of getting samples of females and males whose melanoma rates differed by -14.4866385%---or something more extreme in either direction---would be quite small. Our conclusion to reject the null follows as a logical consequence.

So we can be somewhat confident that there is a difference between females and males. But how much of a difference? A small difference can be statistically significant, and yet be completely irrelevant in the real world. A 1% difference in melanoma rates might not be enough to enact extra preventative measures for men, for example. On the other hand, a 27% difference is huge, and might result in a campaign targeted at men specifically due to the extra risk.

In other words, we cannot just rest on a conclusion of statistical significance. A difference might exist, but so what? We also need to know if that difference is *practically significant*? Are there any practical, real-world consequences due to the magnitude of the difference? There is no cutoff for practical significance. This is determined in the context of the problem, preferably using expert guidance. There are policy considerations, cost-benefit analyses, risk assessments, and a host of other considerations that are made when determining if a result is practically significant. 

A big part of this process that is often neglected is the role of uncertainty. Our point estimate was -14.4866385%. But that number, by itself, is not that meaningful. That is but one estimate coming from one set of samples. The range of plausible values, according to the confidence interval, is -27.3579265% to -1.6153506% . This is a huge range, and there are very different consequences to society is the difference is -27.3579265% versus -1.6153506%.


## Your turn

Go through the rubric to determine if females and males in Denmark who are diagnosed with malignant melanoma suffer from ulcerated tumors at different rates.

The rubric outline is reproduced below. You may refer to the worked example above and modify it accordingly. Remember to strip out all the commentary. That is just exposition for your benefit in understanding the steps, but is not meant to form part of the formal inference process.

Another word of warning: the copy/paste process is not a substitute for your brain. You will often need to modify more than just the names of the data frames and variables to adapt the worked examples to your own work. Do not blindly copy and paste code without understanding what it does. And you should **never** copy and paste text. All the sentences and paragraphs you write are expressions of your own analysis. They must reflect your own understanding of the inferential process.

**Also, so that your answers here don't mess up the code chunks above, use new variable names everywhere. In particular, you should use `ulcer_sex`  everywhere instead of `status_sex`**

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

Just like with one proportion, when certain conditions are met, the difference between two proportions follow a normal model. Rather than simulating a bunch of different sample differences under the assumption of independent variables, we can just replace all that with a relatively simple normal model with mean zero and a standard error based on the sample proportions of successes and failures in the two samples. From that normal model, we obtain P-values and confidence intervals as before.

### Preparing and submitting your assignment

1. From the "Run" menu, select "Restart R and Run All Chunks".
2. Deal with any code errors that crop up. Repeat steps 1–-2 until there are no more code errors.
3. Spell check your document by clicking the icon with "ABC" and a check mark.
4. Hit the "Preview" button one last time to generate the final draft of the `.nb.html` file.
5. Proofread the HTML file carefully. If there are errors, go back and fix them, then repeat steps 1--5 again.

If you have completed this chapter as part of a statistics course, follow the directions you receive from your professor to submit your assignment.


## Optional appendix: Pooling {#pooling}

Earlier, we mentioned that that we cannot calculate the "true" standard error directly because the null hypothesis does not give us $p_{1}$ and $p_{2}$. (The null only addresses the value of the difference $p_{1} - p_{2}$.) We dealt with this by simply substituting $\hat{p}_{1}$ for $p_{1}$ and $\hat{p}_{2}$ for $p_{2}$.

There is, however, one assumption from the null we can still salvage that will improve our test. Since the null hypothesis assumes that the two groups are the same, let's compute a single overall success rate for both samples together. In other words, if the two groups aren't different, let's just pool them into one single group and calculate the successes for the whole group.

This is called a *pooled proportion*. It's straightforward to compute: just take the total number of successes in both groups and divide by the total size of both groups. Here is the formula:

$$\hat{p}_{pooled} = \frac{successes_{1} + successes_{2}}{n_{1} + n_{2}}.$$

Occasionally, we are not given the raw number of successes in each group, but rather, the proportion of successes in each group, $\hat{p}_{1}$ and $\hat{p}_{2}$. The simple fix is to recompute the raw count of successes as $n_{1} \hat{p}_{1}$ and $n_{2} \hat{p}_{2}$. Here is what it looks like in the formula:

$$\hat{p}_{pooled} = \frac{n_{1} \hat{p}_{1} + n_{2} \hat{p}_{2}}{n_{1} + n_{2}}.$$
The normal model can still have a mean of $p_{1} - p_{2}$. (We usually assume this is 0 in the null hypothesis.) But its standard error will use the pooled proportion:

$$N\left( p_{1} - p_{2}, \sqrt{\frac{\hat{p}_{pooled} (1 - \hat{p}_{pooled})}{n_{1}} + \frac{\hat{p}_{pooled} (1 - \hat{p}_{pooled})}{n_{2}}} \right).$$

Not only can we use the pooled proportion in the standard error, but in fact we can use it anywhere we assume the null. For example, the success/failure condition is also subject to the assumption of the null, so we could use the pooled proportion there too.

For a confidence interval, things are different. There is no null hypothesis in effect while computing a confidence interval, so there is no assumption that would justify pooling.

The standard error in the one-proportion interval is

$$
\sqrt{\frac{\hat{p}(1 - \hat{p})}{n}}
$$

which just substitutes $\hat{p}$ for $p$. We do the same for the standard error in the two-proportion case:

$$
SE = \sqrt{\frac{\hat{p}_{1} (1 - \hat{p}_{1})}{n_{1}} + \frac{\hat{p}_{2} (1 - \hat{p}_{2})}{n_{2}}}.
$$
