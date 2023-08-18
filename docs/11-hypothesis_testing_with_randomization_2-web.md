# Hypothesis testing with randomization, Part 2 {#hypothesis2}

<!-- Please don't mess with the next few lines! -->
<style>h5{font-size:2em;color:#0000FF}h6{font-size:1.5em;color:#0000FF}div.answer{margin-left:5%;border:1px solid #0000FF;border-left-width:10px;padding:25px} div.summary{background-color:rgba(30,144,255,0.1);border:3px double #0000FF;padding:25px}</style><p style="color:#ffffff">2.0</p>
<!-- Please don't mess with the previous few lines! -->

::: {.summary}

### Functions introduced in this chapter {-}

`factor`

:::


## Introduction {#hypothesis2-intro}

Now that we have learned about hypothesis testing, we'll explore a different example. Although the rubric for performing the hypothesis test will not change, the individual steps will be implemented in a different way due to the research question we're asking and the type of data used to answer it.

### Install new packages {#hypothesis2-install}

If you are using RStudio Workbench, you do not need to install any packages. (Any packages you need should already be installed by the server administrators.)

If you are using R and RStudio on your own machine instead of accessing RStudio Workbench through a browser, you'll need to type the following command at the Console:

```
install.packages("MASS")
```

### Download the R notebook file {#hypothesis2-download}

Check the upper-right corner in RStudio to make sure you're in your `intro_stats` project. Then click on the following link to download this chapter as an R notebook file (`.Rmd`).

<a href = "https://vectorposse.github.io/intro_stats/chapter_downloads/11-hypothesis_testing_with_randomization_2.Rmd" download>https://vectorposse.github.io/intro_stats/chapter_downloads/11-hypothesis_testing_with_randomization_2.Rmd</a>

Once the file is downloaded, move it to your project folder in RStudio and open it there.

### Restart R and run all chunks {#hypothesis2-restart}

In RStudio, select "Restart R and Run All Chunks" from the "Run" menu.

## Load packages {#hypothesis2-load}

In additional to `tidyverse` and `janitor`, we load the `MASS` package to access the `Melanoma` data on patients in Denmark with malignant melanoma, and the `infer` package for inference tools.


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

```r
library(infer)
```


## Our research question {#hypothesis2-question}

We know that certain types of cancer are more common among females or males. Is there a sex bias among patients with malignant melanoma?

Let's jump into the "Exploratory data analysis" part of the rubric first.


## Exploratory data analysis {#hypothesis2ex-eda}

### Use data documentation (help files, code books, Google, etc.) to determine as much as possible about the data provenance and structure. {#hypothesis2-ex-documentation}

You can look at the help file by typing `?Melanoma` at the Console. However, do not put that command here in a code chunk. The R Notebook has no way of displaying a help file when it's processed. Be careful: there's another data set called `melanoma` with a lower-case "m". Make sure you are using an uppercase "M".

There is a reference at the bottom of the help file.

##### Exercise 1 {-}

Using the reference in the help file, do an internet search to find the source of this data. How can you tell that this reference is not, in fact, a reference to a study of cancer patients in Denmark?

::: {.answer}

Please write up your answer here.

:::

*****

From the exercise above, we can see that it will be very difficult, if not impossible, to discover anything useful about the true provenance of the data (unless you happen to have a copy of that textbook, which in theory provided another more primary source). We will not know, for example, how the data was collected and if the patients consented to having their data shared publicly. The data is suitably anonymized, though, so we don't have any serious concerns about the privacy of the data. Having said that, if a condition is rare enough, a dedicated research can often "de-anonymize" data by cross-referencing information in the data to other kinds of public records. But melanoma is not particularly rare. At any rate, all we can do is assume that the textbook authors obtained the data from a source that used proper procedures for collecting and handling the data.

We print the data frame:


```r
Melanoma
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["time"],"name":[1],"type":["int"],"align":["right"]},{"label":["status"],"name":[2],"type":["int"],"align":["right"]},{"label":["sex"],"name":[3],"type":["int"],"align":["right"]},{"label":["age"],"name":[4],"type":["int"],"align":["right"]},{"label":["year"],"name":[5],"type":["int"],"align":["right"]},{"label":["thickness"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["ulcer"],"name":[7],"type":["int"],"align":["right"]}],"data":[{"1":"10","2":"3","3":"1","4":"76","5":"1972","6":"6.76","7":"1","_rn_":"1"},{"1":"30","2":"3","3":"1","4":"56","5":"1968","6":"0.65","7":"0","_rn_":"2"},{"1":"35","2":"2","3":"1","4":"41","5":"1977","6":"1.34","7":"0","_rn_":"3"},{"1":"99","2":"3","3":"0","4":"71","5":"1968","6":"2.90","7":"0","_rn_":"4"},{"1":"185","2":"1","3":"1","4":"52","5":"1965","6":"12.08","7":"1","_rn_":"5"},{"1":"204","2":"1","3":"1","4":"28","5":"1971","6":"4.84","7":"1","_rn_":"6"},{"1":"210","2":"1","3":"1","4":"77","5":"1972","6":"5.16","7":"1","_rn_":"7"},{"1":"232","2":"3","3":"0","4":"60","5":"1974","6":"3.22","7":"1","_rn_":"8"},{"1":"232","2":"1","3":"1","4":"49","5":"1968","6":"12.88","7":"1","_rn_":"9"},{"1":"279","2":"1","3":"0","4":"68","5":"1971","6":"7.41","7":"1","_rn_":"10"},{"1":"295","2":"1","3":"0","4":"53","5":"1969","6":"4.19","7":"1","_rn_":"11"},{"1":"355","2":"3","3":"0","4":"64","5":"1972","6":"0.16","7":"1","_rn_":"12"},{"1":"386","2":"1","3":"0","4":"68","5":"1965","6":"3.87","7":"1","_rn_":"13"},{"1":"426","2":"1","3":"1","4":"63","5":"1970","6":"4.84","7":"1","_rn_":"14"},{"1":"469","2":"1","3":"0","4":"14","5":"1969","6":"2.42","7":"1","_rn_":"15"},{"1":"493","2":"3","3":"1","4":"72","5":"1971","6":"12.56","7":"1","_rn_":"16"},{"1":"529","2":"1","3":"1","4":"46","5":"1971","6":"5.80","7":"1","_rn_":"17"},{"1":"621","2":"1","3":"1","4":"72","5":"1972","6":"7.06","7":"1","_rn_":"18"},{"1":"629","2":"1","3":"1","4":"95","5":"1968","6":"5.48","7":"1","_rn_":"19"},{"1":"659","2":"1","3":"1","4":"54","5":"1972","6":"7.73","7":"1","_rn_":"20"},{"1":"667","2":"1","3":"0","4":"89","5":"1968","6":"13.85","7":"1","_rn_":"21"},{"1":"718","2":"1","3":"1","4":"25","5":"1967","6":"2.34","7":"1","_rn_":"22"},{"1":"752","2":"1","3":"1","4":"37","5":"1973","6":"4.19","7":"1","_rn_":"23"},{"1":"779","2":"1","3":"1","4":"43","5":"1967","6":"4.04","7":"1","_rn_":"24"},{"1":"793","2":"1","3":"1","4":"68","5":"1970","6":"4.84","7":"1","_rn_":"25"},{"1":"817","2":"1","3":"0","4":"67","5":"1966","6":"0.32","7":"0","_rn_":"26"},{"1":"826","2":"3","3":"0","4":"86","5":"1965","6":"8.54","7":"1","_rn_":"27"},{"1":"833","2":"1","3":"0","4":"56","5":"1971","6":"2.58","7":"1","_rn_":"28"},{"1":"858","2":"1","3":"0","4":"16","5":"1967","6":"3.56","7":"0","_rn_":"29"},{"1":"869","2":"1","3":"0","4":"42","5":"1965","6":"3.54","7":"0","_rn_":"30"},{"1":"872","2":"1","3":"0","4":"65","5":"1968","6":"0.97","7":"0","_rn_":"31"},{"1":"967","2":"1","3":"1","4":"52","5":"1970","6":"4.83","7":"1","_rn_":"32"},{"1":"977","2":"1","3":"1","4":"58","5":"1967","6":"1.62","7":"1","_rn_":"33"},{"1":"982","2":"1","3":"0","4":"60","5":"1970","6":"6.44","7":"1","_rn_":"34"},{"1":"1041","2":"1","3":"1","4":"68","5":"1967","6":"14.66","7":"0","_rn_":"35"},{"1":"1055","2":"1","3":"0","4":"75","5":"1967","6":"2.58","7":"1","_rn_":"36"},{"1":"1062","2":"1","3":"1","4":"19","5":"1966","6":"3.87","7":"1","_rn_":"37"},{"1":"1075","2":"1","3":"1","4":"66","5":"1971","6":"3.54","7":"1","_rn_":"38"},{"1":"1156","2":"1","3":"0","4":"56","5":"1970","6":"1.34","7":"1","_rn_":"39"},{"1":"1228","2":"1","3":"1","4":"46","5":"1973","6":"2.24","7":"1","_rn_":"40"},{"1":"1252","2":"1","3":"0","4":"58","5":"1971","6":"3.87","7":"1","_rn_":"41"},{"1":"1271","2":"1","3":"0","4":"74","5":"1971","6":"3.54","7":"1","_rn_":"42"},{"1":"1312","2":"1","3":"0","4":"65","5":"1970","6":"17.42","7":"1","_rn_":"43"},{"1":"1427","2":"3","3":"1","4":"64","5":"1972","6":"1.29","7":"0","_rn_":"44"},{"1":"1435","2":"1","3":"1","4":"27","5":"1969","6":"3.22","7":"0","_rn_":"45"},{"1":"1499","2":"2","3":"1","4":"73","5":"1973","6":"1.29","7":"0","_rn_":"46"},{"1":"1506","2":"1","3":"1","4":"56","5":"1970","6":"4.51","7":"1","_rn_":"47"},{"1":"1508","2":"2","3":"1","4":"63","5":"1973","6":"8.38","7":"1","_rn_":"48"},{"1":"1510","2":"2","3":"0","4":"69","5":"1973","6":"1.94","7":"0","_rn_":"49"},{"1":"1512","2":"2","3":"0","4":"77","5":"1973","6":"0.16","7":"0","_rn_":"50"},{"1":"1516","2":"1","3":"1","4":"80","5":"1968","6":"2.58","7":"1","_rn_":"51"},{"1":"1525","2":"3","3":"0","4":"76","5":"1970","6":"1.29","7":"1","_rn_":"52"},{"1":"1542","2":"2","3":"0","4":"65","5":"1973","6":"0.16","7":"0","_rn_":"53"},{"1":"1548","2":"1","3":"0","4":"61","5":"1972","6":"1.62","7":"0","_rn_":"54"},{"1":"1557","2":"2","3":"0","4":"26","5":"1973","6":"1.29","7":"0","_rn_":"55"},{"1":"1560","2":"1","3":"0","4":"57","5":"1973","6":"2.10","7":"0","_rn_":"56"},{"1":"1563","2":"2","3":"0","4":"45","5":"1973","6":"0.32","7":"0","_rn_":"57"},{"1":"1584","2":"1","3":"1","4":"31","5":"1970","6":"0.81","7":"0","_rn_":"58"},{"1":"1605","2":"2","3":"0","4":"36","5":"1973","6":"1.13","7":"0","_rn_":"59"},{"1":"1621","2":"1","3":"0","4":"46","5":"1972","6":"5.16","7":"1","_rn_":"60"},{"1":"1627","2":"2","3":"0","4":"43","5":"1973","6":"1.62","7":"0","_rn_":"61"},{"1":"1634","2":"2","3":"0","4":"68","5":"1973","6":"1.37","7":"0","_rn_":"62"},{"1":"1641","2":"2","3":"1","4":"57","5":"1973","6":"0.24","7":"0","_rn_":"63"},{"1":"1641","2":"2","3":"0","4":"57","5":"1973","6":"0.81","7":"0","_rn_":"64"},{"1":"1648","2":"2","3":"0","4":"55","5":"1973","6":"1.29","7":"0","_rn_":"65"},{"1":"1652","2":"2","3":"0","4":"58","5":"1973","6":"1.29","7":"0","_rn_":"66"},{"1":"1654","2":"2","3":"1","4":"20","5":"1973","6":"0.97","7":"0","_rn_":"67"},{"1":"1654","2":"2","3":"0","4":"67","5":"1973","6":"1.13","7":"0","_rn_":"68"},{"1":"1667","2":"1","3":"0","4":"44","5":"1971","6":"5.80","7":"1","_rn_":"69"},{"1":"1678","2":"2","3":"0","4":"59","5":"1973","6":"1.29","7":"0","_rn_":"70"},{"1":"1685","2":"2","3":"0","4":"32","5":"1973","6":"0.48","7":"0","_rn_":"71"},{"1":"1690","2":"1","3":"1","4":"83","5":"1971","6":"1.62","7":"0","_rn_":"72"},{"1":"1710","2":"2","3":"0","4":"55","5":"1973","6":"2.26","7":"0","_rn_":"73"},{"1":"1710","2":"2","3":"1","4":"15","5":"1973","6":"0.58","7":"0","_rn_":"74"},{"1":"1726","2":"1","3":"0","4":"58","5":"1970","6":"0.97","7":"1","_rn_":"75"},{"1":"1745","2":"2","3":"0","4":"47","5":"1973","6":"2.58","7":"1","_rn_":"76"},{"1":"1762","2":"2","3":"0","4":"54","5":"1973","6":"0.81","7":"0","_rn_":"77"},{"1":"1779","2":"2","3":"1","4":"55","5":"1973","6":"3.54","7":"1","_rn_":"78"},{"1":"1787","2":"2","3":"1","4":"38","5":"1973","6":"0.97","7":"0","_rn_":"79"},{"1":"1787","2":"2","3":"0","4":"41","5":"1973","6":"1.78","7":"1","_rn_":"80"},{"1":"1793","2":"2","3":"0","4":"56","5":"1973","6":"1.94","7":"0","_rn_":"81"},{"1":"1804","2":"2","3":"0","4":"48","5":"1973","6":"1.29","7":"0","_rn_":"82"},{"1":"1812","2":"2","3":"1","4":"44","5":"1973","6":"3.22","7":"1","_rn_":"83"},{"1":"1836","2":"2","3":"0","4":"70","5":"1972","6":"1.53","7":"0","_rn_":"84"},{"1":"1839","2":"2","3":"0","4":"40","5":"1972","6":"1.29","7":"0","_rn_":"85"},{"1":"1839","2":"2","3":"1","4":"53","5":"1972","6":"1.62","7":"1","_rn_":"86"},{"1":"1854","2":"2","3":"0","4":"65","5":"1972","6":"1.62","7":"1","_rn_":"87"},{"1":"1856","2":"2","3":"1","4":"54","5":"1972","6":"0.32","7":"0","_rn_":"88"},{"1":"1860","2":"3","3":"1","4":"71","5":"1969","6":"4.84","7":"1","_rn_":"89"},{"1":"1864","2":"2","3":"0","4":"49","5":"1972","6":"1.29","7":"0","_rn_":"90"},{"1":"1899","2":"2","3":"0","4":"55","5":"1972","6":"0.97","7":"0","_rn_":"91"},{"1":"1914","2":"2","3":"0","4":"69","5":"1972","6":"3.06","7":"0","_rn_":"92"},{"1":"1919","2":"2","3":"1","4":"83","5":"1972","6":"3.54","7":"0","_rn_":"93"},{"1":"1920","2":"2","3":"1","4":"60","5":"1972","6":"1.62","7":"1","_rn_":"94"},{"1":"1927","2":"2","3":"1","4":"40","5":"1972","6":"2.58","7":"1","_rn_":"95"},{"1":"1933","2":"1","3":"0","4":"77","5":"1972","6":"1.94","7":"0","_rn_":"96"},{"1":"1942","2":"2","3":"0","4":"35","5":"1972","6":"0.81","7":"0","_rn_":"97"},{"1":"1955","2":"2","3":"0","4":"46","5":"1972","6":"7.73","7":"1","_rn_":"98"},{"1":"1956","2":"2","3":"0","4":"34","5":"1972","6":"0.97","7":"0","_rn_":"99"},{"1":"1958","2":"2","3":"0","4":"69","5":"1972","6":"12.88","7":"0","_rn_":"100"},{"1":"1963","2":"2","3":"0","4":"60","5":"1972","6":"2.58","7":"0","_rn_":"101"},{"1":"1970","2":"2","3":"1","4":"84","5":"1972","6":"4.09","7":"1","_rn_":"102"},{"1":"2005","2":"2","3":"0","4":"66","5":"1972","6":"0.64","7":"0","_rn_":"103"},{"1":"2007","2":"2","3":"1","4":"56","5":"1972","6":"0.97","7":"0","_rn_":"104"},{"1":"2011","2":"2","3":"0","4":"75","5":"1972","6":"3.22","7":"1","_rn_":"105"},{"1":"2024","2":"2","3":"0","4":"36","5":"1972","6":"1.62","7":"0","_rn_":"106"},{"1":"2028","2":"2","3":"1","4":"52","5":"1972","6":"3.87","7":"1","_rn_":"107"},{"1":"2038","2":"2","3":"0","4":"58","5":"1972","6":"0.32","7":"1","_rn_":"108"},{"1":"2056","2":"2","3":"0","4":"39","5":"1972","6":"0.32","7":"0","_rn_":"109"},{"1":"2059","2":"2","3":"1","4":"68","5":"1972","6":"3.22","7":"1","_rn_":"110"},{"1":"2061","2":"1","3":"1","4":"71","5":"1968","6":"2.26","7":"0","_rn_":"111"},{"1":"2062","2":"1","3":"0","4":"52","5":"1965","6":"3.06","7":"0","_rn_":"112"},{"1":"2075","2":"2","3":"1","4":"55","5":"1972","6":"2.58","7":"1","_rn_":"113"},{"1":"2085","2":"3","3":"0","4":"66","5":"1970","6":"0.65","7":"0","_rn_":"114"},{"1":"2102","2":"2","3":"1","4":"35","5":"1972","6":"1.13","7":"0","_rn_":"115"},{"1":"2103","2":"1","3":"1","4":"44","5":"1966","6":"0.81","7":"0","_rn_":"116"},{"1":"2104","2":"2","3":"0","4":"72","5":"1972","6":"0.97","7":"0","_rn_":"117"},{"1":"2108","2":"1","3":"0","4":"58","5":"1969","6":"1.76","7":"1","_rn_":"118"},{"1":"2112","2":"2","3":"0","4":"54","5":"1972","6":"1.94","7":"1","_rn_":"119"},{"1":"2150","2":"2","3":"0","4":"33","5":"1972","6":"0.65","7":"0","_rn_":"120"},{"1":"2156","2":"2","3":"0","4":"45","5":"1972","6":"0.97","7":"0","_rn_":"121"},{"1":"2165","2":"2","3":"1","4":"62","5":"1972","6":"5.64","7":"0","_rn_":"122"},{"1":"2209","2":"2","3":"0","4":"72","5":"1971","6":"9.66","7":"0","_rn_":"123"},{"1":"2227","2":"2","3":"0","4":"51","5":"1971","6":"0.10","7":"0","_rn_":"124"},{"1":"2227","2":"2","3":"1","4":"77","5":"1971","6":"5.48","7":"1","_rn_":"125"},{"1":"2256","2":"1","3":"0","4":"43","5":"1971","6":"2.26","7":"1","_rn_":"126"},{"1":"2264","2":"2","3":"0","4":"65","5":"1971","6":"4.83","7":"1","_rn_":"127"},{"1":"2339","2":"2","3":"0","4":"63","5":"1971","6":"0.97","7":"0","_rn_":"128"},{"1":"2361","2":"2","3":"1","4":"60","5":"1971","6":"0.97","7":"0","_rn_":"129"},{"1":"2387","2":"2","3":"0","4":"50","5":"1971","6":"5.16","7":"1","_rn_":"130"},{"1":"2388","2":"1","3":"1","4":"40","5":"1966","6":"0.81","7":"0","_rn_":"131"},{"1":"2403","2":"2","3":"0","4":"67","5":"1971","6":"2.90","7":"1","_rn_":"132"},{"1":"2426","2":"2","3":"0","4":"69","5":"1971","6":"3.87","7":"0","_rn_":"133"},{"1":"2426","2":"2","3":"0","4":"74","5":"1971","6":"1.94","7":"1","_rn_":"134"},{"1":"2431","2":"2","3":"0","4":"49","5":"1971","6":"0.16","7":"0","_rn_":"135"},{"1":"2460","2":"2","3":"0","4":"47","5":"1971","6":"0.64","7":"0","_rn_":"136"},{"1":"2467","2":"1","3":"0","4":"42","5":"1965","6":"2.26","7":"1","_rn_":"137"},{"1":"2492","2":"2","3":"0","4":"54","5":"1971","6":"1.45","7":"0","_rn_":"138"},{"1":"2493","2":"2","3":"1","4":"72","5":"1971","6":"4.82","7":"1","_rn_":"139"},{"1":"2521","2":"2","3":"0","4":"45","5":"1971","6":"1.29","7":"1","_rn_":"140"},{"1":"2542","2":"2","3":"1","4":"67","5":"1971","6":"7.89","7":"1","_rn_":"141"},{"1":"2559","2":"2","3":"0","4":"48","5":"1970","6":"0.81","7":"1","_rn_":"142"},{"1":"2565","2":"1","3":"1","4":"34","5":"1970","6":"3.54","7":"1","_rn_":"143"},{"1":"2570","2":"2","3":"0","4":"44","5":"1970","6":"1.29","7":"0","_rn_":"144"},{"1":"2660","2":"2","3":"0","4":"31","5":"1970","6":"0.64","7":"0","_rn_":"145"},{"1":"2666","2":"2","3":"0","4":"42","5":"1970","6":"3.22","7":"1","_rn_":"146"},{"1":"2676","2":"2","3":"0","4":"24","5":"1970","6":"1.45","7":"1","_rn_":"147"},{"1":"2738","2":"2","3":"0","4":"58","5":"1970","6":"0.48","7":"0","_rn_":"148"},{"1":"2782","2":"1","3":"1","4":"78","5":"1969","6":"1.94","7":"0","_rn_":"149"},{"1":"2787","2":"2","3":"1","4":"62","5":"1970","6":"0.16","7":"0","_rn_":"150"},{"1":"2984","2":"2","3":"1","4":"70","5":"1969","6":"0.16","7":"0","_rn_":"151"},{"1":"3032","2":"2","3":"0","4":"35","5":"1969","6":"1.29","7":"0","_rn_":"152"},{"1":"3040","2":"2","3":"0","4":"61","5":"1969","6":"1.94","7":"0","_rn_":"153"},{"1":"3042","2":"1","3":"0","4":"54","5":"1967","6":"3.54","7":"1","_rn_":"154"},{"1":"3067","2":"2","3":"0","4":"29","5":"1969","6":"0.81","7":"0","_rn_":"155"},{"1":"3079","2":"2","3":"1","4":"64","5":"1969","6":"0.65","7":"0","_rn_":"156"},{"1":"3101","2":"2","3":"1","4":"47","5":"1969","6":"7.09","7":"0","_rn_":"157"},{"1":"3144","2":"2","3":"1","4":"62","5":"1969","6":"0.16","7":"0","_rn_":"158"},{"1":"3152","2":"2","3":"0","4":"32","5":"1969","6":"1.62","7":"0","_rn_":"159"},{"1":"3154","2":"3","3":"1","4":"49","5":"1969","6":"1.62","7":"0","_rn_":"160"},{"1":"3180","2":"2","3":"0","4":"25","5":"1969","6":"1.29","7":"0","_rn_":"161"},{"1":"3182","2":"3","3":"1","4":"49","5":"1966","6":"6.12","7":"0","_rn_":"162"},{"1":"3185","2":"2","3":"0","4":"64","5":"1969","6":"0.48","7":"0","_rn_":"163"},{"1":"3199","2":"2","3":"0","4":"36","5":"1969","6":"0.64","7":"0","_rn_":"164"},{"1":"3228","2":"2","3":"0","4":"58","5":"1969","6":"3.22","7":"1","_rn_":"165"},{"1":"3229","2":"2","3":"0","4":"37","5":"1969","6":"1.94","7":"0","_rn_":"166"},{"1":"3278","2":"2","3":"1","4":"54","5":"1969","6":"2.58","7":"0","_rn_":"167"},{"1":"3297","2":"2","3":"0","4":"61","5":"1968","6":"2.58","7":"1","_rn_":"168"},{"1":"3328","2":"2","3":"1","4":"31","5":"1968","6":"0.81","7":"0","_rn_":"169"},{"1":"3330","2":"2","3":"1","4":"61","5":"1968","6":"0.81","7":"1","_rn_":"170"},{"1":"3338","2":"1","3":"0","4":"60","5":"1967","6":"3.22","7":"1","_rn_":"171"},{"1":"3383","2":"2","3":"0","4":"43","5":"1968","6":"0.32","7":"0","_rn_":"172"},{"1":"3384","2":"2","3":"0","4":"68","5":"1968","6":"3.22","7":"1","_rn_":"173"},{"1":"3385","2":"2","3":"0","4":"4","5":"1968","6":"2.74","7":"0","_rn_":"174"},{"1":"3388","2":"2","3":"1","4":"60","5":"1968","6":"4.84","7":"1","_rn_":"175"},{"1":"3402","2":"2","3":"1","4":"50","5":"1968","6":"1.62","7":"0","_rn_":"176"},{"1":"3441","2":"2","3":"0","4":"20","5":"1968","6":"0.65","7":"0","_rn_":"177"},{"1":"3458","2":"3","3":"0","4":"54","5":"1967","6":"1.45","7":"0","_rn_":"178"},{"1":"3459","2":"2","3":"0","4":"29","5":"1968","6":"0.65","7":"0","_rn_":"179"},{"1":"3459","2":"2","3":"1","4":"56","5":"1968","6":"1.29","7":"1","_rn_":"180"},{"1":"3476","2":"2","3":"0","4":"60","5":"1968","6":"1.62","7":"0","_rn_":"181"},{"1":"3523","2":"2","3":"0","4":"46","5":"1968","6":"3.54","7":"0","_rn_":"182"},{"1":"3667","2":"2","3":"0","4":"42","5":"1967","6":"3.22","7":"0","_rn_":"183"},{"1":"3695","2":"2","3":"0","4":"34","5":"1967","6":"0.65","7":"0","_rn_":"184"},{"1":"3695","2":"2","3":"0","4":"56","5":"1967","6":"1.03","7":"0","_rn_":"185"},{"1":"3776","2":"2","3":"1","4":"12","5":"1967","6":"7.09","7":"1","_rn_":"186"},{"1":"3776","2":"2","3":"0","4":"21","5":"1967","6":"1.29","7":"1","_rn_":"187"},{"1":"3830","2":"2","3":"1","4":"46","5":"1967","6":"0.65","7":"0","_rn_":"188"},{"1":"3856","2":"2","3":"0","4":"49","5":"1967","6":"1.78","7":"0","_rn_":"189"},{"1":"3872","2":"2","3":"0","4":"35","5":"1967","6":"12.24","7":"1","_rn_":"190"},{"1":"3909","2":"2","3":"1","4":"42","5":"1967","6":"8.06","7":"1","_rn_":"191"},{"1":"3968","2":"2","3":"0","4":"47","5":"1967","6":"0.81","7":"0","_rn_":"192"},{"1":"4001","2":"2","3":"0","4":"69","5":"1967","6":"2.10","7":"0","_rn_":"193"},{"1":"4103","2":"2","3":"0","4":"52","5":"1966","6":"3.87","7":"0","_rn_":"194"},{"1":"4119","2":"2","3":"1","4":"52","5":"1966","6":"0.65","7":"0","_rn_":"195"},{"1":"4124","2":"2","3":"0","4":"30","5":"1966","6":"1.94","7":"1","_rn_":"196"},{"1":"4207","2":"2","3":"1","4":"22","5":"1966","6":"0.65","7":"0","_rn_":"197"},{"1":"4310","2":"2","3":"1","4":"55","5":"1966","6":"2.10","7":"0","_rn_":"198"},{"1":"4390","2":"2","3":"0","4":"26","5":"1965","6":"1.94","7":"1","_rn_":"199"},{"1":"4479","2":"2","3":"0","4":"19","5":"1965","6":"1.13","7":"1","_rn_":"200"},{"1":"4492","2":"2","3":"1","4":"29","5":"1965","6":"7.06","7":"1","_rn_":"201"},{"1":"4668","2":"2","3":"0","4":"40","5":"1965","6":"6.12","7":"0","_rn_":"202"},{"1":"4688","2":"2","3":"0","4":"42","5":"1965","6":"0.48","7":"0","_rn_":"203"},{"1":"4926","2":"2","3":"0","4":"50","5":"1964","6":"2.26","7":"0","_rn_":"204"},{"1":"5565","2":"2","3":"0","4":"41","5":"1962","6":"2.90","7":"0","_rn_":"205"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

Use `glimpse` to examine the structure of the data:


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

### Prepare the data for analysis. {#hypothesis2-ex-prepare}

It appears that `sex` is coded as an integer. You will recall that we need to convert it to a factor variable since it is categorical, not numerical.

##### Exercise 2 {-}

According to the help file, which number corresponds to which sex?

::: {.answer}

Please write up your answer here.

:::

*****

We can convert a numerical variable a couple of different ways. In Chapter 3, we used the `as_factor` command. That command works fine, but it doesn't give you a way to change the levels of the variable. In other words, if we used `as_factor` here, we would get a factor variable that still contained zeroes and ones. 

Instead, we will use the `factor` command. It allows us to manually relabel the levels. The `levels` argument requires a vector (with `c`) of the current levels, and the `labels` argument requires a vector listing the new names you want to assign, as follows:


```r
Melanoma <- Melanoma %>%
    mutate(sex_fct = factor(sex, levels = c(0, 1), labels = c("female", "male")))
glimpse(Melanoma)
```

```
## Rows: 205
## Columns: 8
## $ time      <int> 10, 30, 35, 99, 185, 204, 210, 232, 232, 279, 295, 355, 386,…
## $ status    <int> 3, 3, 2, 3, 1, 1, 1, 3, 1, 1, 1, 3, 1, 1, 1, 3, 1, 1, 1, 1, …
## $ sex       <int> 1, 1, 1, 0, 1, 1, 1, 0, 1, 0, 0, 0, 0, 1, 0, 1, 1, 1, 1, 1, …
## $ age       <int> 76, 56, 41, 71, 52, 28, 77, 60, 49, 68, 53, 64, 68, 63, 14, …
## $ year      <int> 1972, 1968, 1977, 1968, 1965, 1971, 1972, 1974, 1968, 1971, …
## $ thickness <dbl> 6.76, 0.65, 1.34, 2.90, 12.08, 4.84, 5.16, 3.22, 12.88, 7.41…
## $ ulcer     <int> 1, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, …
## $ sex_fct   <fct> male, male, male, female, male, male, male, female, male, fe…
```

You should check to make sure the first few entries of `sex_fct` agree with the numbers in the `sex` variable according to the labels explained in the help file. (If not, it means that you put the `levels` in one order and the `labels` in a different order.)

### Make tables or plots to explore the data visually. {#hypothesis2-ex-plots}

We only have one categorical variable, so we only need a frequency table. Since we are concerned with proportions, we'll also look at a relative frequency table which the `tabyl` command provides for free.


```r
tabyl(Melanoma, sex_fct) %>%
    adorn_totals()
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["sex_fct"],"name":[1],"type":["fct"],"align":["left"]},{"label":["n"],"name":[2],"type":["int"],"align":["right"]},{"label":["percent"],"name":[3],"type":["dbl"],"align":["right"]}],"data":[{"1":"female","2":"126","3":"0.6146341","_rn_":"1"},{"1":"male","2":"79","3":"0.3853659","_rn_":"2"},{"1":"Total","2":"205","3":"1.0000000","_rn_":"3"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


## The logic of inference and randomization {#hypothesis2-logic}

This is a good place to pause and remember why statistical inference is important. There are certainly more females than males in this data set. So why don't we just show the table above, declare females are more likely to have malignant melanoma, and then go home?

Think back to coin flips. Even though there was a 50% chance of seeing heads, did that mean that exactly half of our flips came up heads? No. We have to acknowledge *sampling variability*: even if the truth were 50%, when w sample, we could accidentally get more or less than 50%, just by pure chance alone. Perhaps these 205 patients just happen to have more females than average.

The key, then, is to figure out if 61.5% is *significantly* larger than 50%, or if a number like 61.5% (or one even more extreme) could easily come about from random chance.

As we know from the last chapter, we can run a formal hypothesis test to find out. As we do so, make note of the things that are the same and the things that have changed from the last hypothesis tests you ran. For example, we are not comparing two groups anymore. We have one group of patients, and all we're doing is measuring the percentage of this group that is female. It's tempting to think that we're comparing males and females, but that's not the case. We are not using sex to divide our data into two groups for the purpose of exploring whether some other variable differs between men and women. We just have one sample. "Female" and "Male" are simply categories in a single categorical variable. Also, because we are only asking about one variable (`sex_fct`), the mathematical form of the hypotheses will look a little different.

Because this is no longer a question about two variables being independent or associated, the "permuting" idea we've been using no longer makes sense. So what does make sense?

It helps to start by figuring out what our null hypothesis is. Remember, our question of interest is whether there is a sex bias in malignant melanoma. In other words, are there more or fewer females than males with malignant melanoma? As this is our research question, it will be the alternative hypothesis. So what is the null? What is the "default" situation in which nothing interesting is going on? Well, there would be no sex bias. In other words, there would be the same number of females and males with malignant melanoma. Or another way of saying that---with respect to the "success" condition of being female that we discussed earlier---is that females comprise 50% of all patients with malignant melanoma.

Okay, given our philosophy about the null hypothesis, let's take the skeptical position and assume that, indeed, 50% of all malignant melanoma patients in our population are female. Then let's take a sample of 205 patients. We can't get exactly 50% females from a sample of 205 (that would be 102.5 females!), so what numbers can we get?

Randomization will tell us. What kind of randomization? As we come across each patient in our sample, there is a 50% chance of them being female. So instead of sampling real patients, what if we just flipped a coin? A simulated coin flip will come up heads just as often as our patients will be female under the assumption of the null.

This brings us full circle, back to the first randomization idea we explored. We can simulate coin flips, graph our results, and calculate a P-value. More specifically, we'll flip a coin 205 times to represent sampling 205 patients. Then we'll repeat this procedure a bunch of times and establish a range of plausible percentages that can come about by chance from this procedure. Instead of doing coin flips with the `rflip` command as we did then, however, we'll use our new favorite friend, the `infer` package.

Let's dive back into the remaining steps of the formal hypothesis test.


## Hypotheses {#hypothesis2-ex-hypotheses}

### Identify the sample (or samples) and a reasonable population (or populations) of interest. {#hypothesis2-ex-sample-pop}

The sample consists of 205 patients from Denmark with malignant melanoma. Our population is presumably all patients with malignant melanoma, although in checking conditions below, we'll take care to discuss whether patients in Denmark are representative of patients elsewhere.

### Express the null and alternative hypotheses as contextually meaningful full sentences. {#hypothesis2-ex-express-words}

$H_{0}:$ Half of malignant melanoma patients are female.

$H_{A}:$ There is a sex bias among patients with malignant melanoma (meaning that females are either over-represented or under-represented).

### Express the null and alternative hypotheses in symbols (when possible). {#hypothesis2-ex-express-math}

$H_{0}: p_{female} = 0.5$

$H_{A}: p_{female} \neq 0.5$


## Model {#hypothesis2-ex-model}

### Identify the sampling distribution model. {#hypothesis2-ex-sampling-dist-model}

We will randomize to simulate the sampling distribution.

### Check the relevant conditions to ensure that model assumptions are met. {#hypothesis2-ex-ht-conditions}

- Random
  - As mentioned above, these 205 patients are not a random sample of all people with malignant melanoma. We don't even have any evidence that they are a random sample of melanoma patients in Denmark. Without such evidence, we have to hope that these 205 patients are representative of all patients who have malignant melanoma. Unless there's something special about Danes in terms of their genetics or diet or something like that, one could imagine that their physiology makes them just as susceptible to melanoma as anyone else. More specifically, though, our question is about females and males getting malignant melanoma. Perhaps there are more female sunbathers in Denmark than in other countries. That might make Danes unrepresentative in terms of the gender balance among melanoma patients. We should be cautious in interpreting any conclusion we might reach in light of these doubts.

- 10%
  - Whether in Denmark or not, given that melanoma is a fairly common form of cancer, I assume 205 is less than 10% of all patients with malignant melanoma.


## Mechanics {#hypothesis2-ex-mechanics}

### Compute the test statistic. {#hypothesis2-ex-compute-test-stat}


```r
female_prop <- Melanoma %>%
    observe(response = sex_fct, success = "female",
            stat = "prop")
female_prop
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["stat"],"name":[1],"type":["dbl"],"align":["right"]}],"data":[{"1":"0.6146341"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

Note: Pay close attention to the difference in the `observe` command above. Unlike in the last chapter, we don't have any tildes. That's because there are not two variables involved. There is only one variable, which `observe` needs to see as the "response" variable. (Don't forget to use the factor version `sex_fct` and not `sex`!) We still have to specify a "success" condition. Since the hypotheses are about measuring females, we have to tell `observe` to calculate the proportion of females. Finally, the `stat` is no longer "diff in props" There are not two proportions with which to find a difference. There is just one proportion, hence, "prop".

### Report the test statistic in context (when possible). {#hypothesis2-ex-report-test-stat}

The observed percentage of females with melanoma in our sample is 61.4634146%.

Note: As explained in the last chapter, we have to use `pull` to pull out the number from the `female_prop` tibble.

### Plot the null distribution. {#hypothesis2-ex-plot-null}

Since this is the first step for which we need the simulated values, it will be convenient to run the simulation here. We'll need to set the seed as well.


```r
set.seed(42)
melanoma_test <- Melanoma %>%
    specify(response = sex_fct, success = "female") %>%
    hypothesize(null = "point", p = 0.5) %>%
    generate(reps = 1000, type = "draw") %>%
    calculate(stat = "prop")
melanoma_test
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["replicate"],"name":[1],"type":["fct"],"align":["left"]},{"label":["stat"],"name":[2],"type":["dbl"],"align":["right"]}],"data":[{"1":"1","2":"0.4439024"},{"1":"2","2":"0.5853659"},{"1":"3","2":"0.5512195"},{"1":"4","2":"0.5024390"},{"1":"5","2":"0.5609756"},{"1":"6","2":"0.4926829"},{"1":"7","2":"0.5268293"},{"1":"8","2":"0.4878049"},{"1":"9","2":"0.5121951"},{"1":"10","2":"0.4536585"},{"1":"11","2":"0.4975610"},{"1":"12","2":"0.4195122"},{"1":"13","2":"0.5073171"},{"1":"14","2":"0.5268293"},{"1":"15","2":"0.4975610"},{"1":"16","2":"0.5121951"},{"1":"17","2":"0.4682927"},{"1":"18","2":"0.4829268"},{"1":"19","2":"0.4975610"},{"1":"20","2":"0.4634146"},{"1":"21","2":"0.4878049"},{"1":"22","2":"0.4536585"},{"1":"23","2":"0.4585366"},{"1":"24","2":"0.4780488"},{"1":"25","2":"0.4878049"},{"1":"26","2":"0.4926829"},{"1":"27","2":"0.4585366"},{"1":"28","2":"0.5170732"},{"1":"29","2":"0.5219512"},{"1":"30","2":"0.5073171"},{"1":"31","2":"0.5121951"},{"1":"32","2":"0.5463415"},{"1":"33","2":"0.5219512"},{"1":"34","2":"0.4487805"},{"1":"35","2":"0.4682927"},{"1":"36","2":"0.5268293"},{"1":"37","2":"0.5268293"},{"1":"38","2":"0.5317073"},{"1":"39","2":"0.5268293"},{"1":"40","2":"0.5463415"},{"1":"41","2":"0.5317073"},{"1":"42","2":"0.5219512"},{"1":"43","2":"0.5121951"},{"1":"44","2":"0.4975610"},{"1":"45","2":"0.4536585"},{"1":"46","2":"0.5121951"},{"1":"47","2":"0.5414634"},{"1":"48","2":"0.4731707"},{"1":"49","2":"0.4536585"},{"1":"50","2":"0.4926829"},{"1":"51","2":"0.4243902"},{"1":"52","2":"0.5024390"},{"1":"53","2":"0.5219512"},{"1":"54","2":"0.4536585"},{"1":"55","2":"0.5121951"},{"1":"56","2":"0.4731707"},{"1":"57","2":"0.4829268"},{"1":"58","2":"0.5414634"},{"1":"59","2":"0.4780488"},{"1":"60","2":"0.4682927"},{"1":"61","2":"0.5073171"},{"1":"62","2":"0.5073171"},{"1":"63","2":"0.4341463"},{"1":"64","2":"0.5170732"},{"1":"65","2":"0.5024390"},{"1":"66","2":"0.5219512"},{"1":"67","2":"0.4634146"},{"1":"68","2":"0.4682927"},{"1":"69","2":"0.4878049"},{"1":"70","2":"0.5121951"},{"1":"71","2":"0.5024390"},{"1":"72","2":"0.5121951"},{"1":"73","2":"0.5414634"},{"1":"74","2":"0.5073171"},{"1":"75","2":"0.5804878"},{"1":"76","2":"0.5609756"},{"1":"77","2":"0.5609756"},{"1":"78","2":"0.5365854"},{"1":"79","2":"0.5024390"},{"1":"80","2":"0.4585366"},{"1":"81","2":"0.4682927"},{"1":"82","2":"0.5121951"},{"1":"83","2":"0.4292683"},{"1":"84","2":"0.4878049"},{"1":"85","2":"0.5170732"},{"1":"86","2":"0.4731707"},{"1":"87","2":"0.5317073"},{"1":"88","2":"0.4975610"},{"1":"89","2":"0.5365854"},{"1":"90","2":"0.5073171"},{"1":"91","2":"0.4878049"},{"1":"92","2":"0.4926829"},{"1":"93","2":"0.5170732"},{"1":"94","2":"0.4585366"},{"1":"95","2":"0.5121951"},{"1":"96","2":"0.5121951"},{"1":"97","2":"0.4829268"},{"1":"98","2":"0.5268293"},{"1":"99","2":"0.4780488"},{"1":"100","2":"0.4926829"},{"1":"101","2":"0.4634146"},{"1":"102","2":"0.4780488"},{"1":"103","2":"0.4926829"},{"1":"104","2":"0.5073171"},{"1":"105","2":"0.5268293"},{"1":"106","2":"0.5365854"},{"1":"107","2":"0.5121951"},{"1":"108","2":"0.4829268"},{"1":"109","2":"0.4731707"},{"1":"110","2":"0.5219512"},{"1":"111","2":"0.5121951"},{"1":"112","2":"0.5073171"},{"1":"113","2":"0.5365854"},{"1":"114","2":"0.5073171"},{"1":"115","2":"0.5268293"},{"1":"116","2":"0.5463415"},{"1":"117","2":"0.4731707"},{"1":"118","2":"0.5268293"},{"1":"119","2":"0.5268293"},{"1":"120","2":"0.5073171"},{"1":"121","2":"0.4878049"},{"1":"122","2":"0.4780488"},{"1":"123","2":"0.4634146"},{"1":"124","2":"0.4878049"},{"1":"125","2":"0.5365854"},{"1":"126","2":"0.5317073"},{"1":"127","2":"0.4975610"},{"1":"128","2":"0.5121951"},{"1":"129","2":"0.5317073"},{"1":"130","2":"0.5170732"},{"1":"131","2":"0.4634146"},{"1":"132","2":"0.5463415"},{"1":"133","2":"0.4731707"},{"1":"134","2":"0.5463415"},{"1":"135","2":"0.4341463"},{"1":"136","2":"0.4926829"},{"1":"137","2":"0.5121951"},{"1":"138","2":"0.5463415"},{"1":"139","2":"0.5560976"},{"1":"140","2":"0.4731707"},{"1":"141","2":"0.5170732"},{"1":"142","2":"0.5024390"},{"1":"143","2":"0.4926829"},{"1":"144","2":"0.5268293"},{"1":"145","2":"0.5414634"},{"1":"146","2":"0.4487805"},{"1":"147","2":"0.4487805"},{"1":"148","2":"0.4878049"},{"1":"149","2":"0.5121951"},{"1":"150","2":"0.4390244"},{"1":"151","2":"0.4487805"},{"1":"152","2":"0.4536585"},{"1":"153","2":"0.5365854"},{"1":"154","2":"0.4634146"},{"1":"155","2":"0.5219512"},{"1":"156","2":"0.5024390"},{"1":"157","2":"0.5268293"},{"1":"158","2":"0.5073171"},{"1":"159","2":"0.4780488"},{"1":"160","2":"0.4926829"},{"1":"161","2":"0.4975610"},{"1":"162","2":"0.5658537"},{"1":"163","2":"0.4682927"},{"1":"164","2":"0.5512195"},{"1":"165","2":"0.5317073"},{"1":"166","2":"0.4585366"},{"1":"167","2":"0.5365854"},{"1":"168","2":"0.5170732"},{"1":"169","2":"0.5219512"},{"1":"170","2":"0.4829268"},{"1":"171","2":"0.4585366"},{"1":"172","2":"0.5219512"},{"1":"173","2":"0.5073171"},{"1":"174","2":"0.4487805"},{"1":"175","2":"0.5268293"},{"1":"176","2":"0.4341463"},{"1":"177","2":"0.4536585"},{"1":"178","2":"0.5219512"},{"1":"179","2":"0.5463415"},{"1":"180","2":"0.5170732"},{"1":"181","2":"0.4926829"},{"1":"182","2":"0.5073171"},{"1":"183","2":"0.4780488"},{"1":"184","2":"0.4829268"},{"1":"185","2":"0.4975610"},{"1":"186","2":"0.5365854"},{"1":"187","2":"0.5365854"},{"1":"188","2":"0.4439024"},{"1":"189","2":"0.4731707"},{"1":"190","2":"0.4878049"},{"1":"191","2":"0.4780488"},{"1":"192","2":"0.5658537"},{"1":"193","2":"0.4975610"},{"1":"194","2":"0.4926829"},{"1":"195","2":"0.4634146"},{"1":"196","2":"0.5170732"},{"1":"197","2":"0.4731707"},{"1":"198","2":"0.5463415"},{"1":"199","2":"0.5219512"},{"1":"200","2":"0.5024390"},{"1":"201","2":"0.5073171"},{"1":"202","2":"0.5609756"},{"1":"203","2":"0.5268293"},{"1":"204","2":"0.4292683"},{"1":"205","2":"0.5365854"},{"1":"206","2":"0.5414634"},{"1":"207","2":"0.5317073"},{"1":"208","2":"0.4926829"},{"1":"209","2":"0.5170732"},{"1":"210","2":"0.4292683"},{"1":"211","2":"0.5414634"},{"1":"212","2":"0.4585366"},{"1":"213","2":"0.4634146"},{"1":"214","2":"0.5463415"},{"1":"215","2":"0.4926829"},{"1":"216","2":"0.4390244"},{"1":"217","2":"0.5073171"},{"1":"218","2":"0.4487805"},{"1":"219","2":"0.4390244"},{"1":"220","2":"0.5024390"},{"1":"221","2":"0.4439024"},{"1":"222","2":"0.4634146"},{"1":"223","2":"0.5658537"},{"1":"224","2":"0.4634146"},{"1":"225","2":"0.4682927"},{"1":"226","2":"0.5365854"},{"1":"227","2":"0.4585366"},{"1":"228","2":"0.4487805"},{"1":"229","2":"0.5365854"},{"1":"230","2":"0.4780488"},{"1":"231","2":"0.4975610"},{"1":"232","2":"0.4975610"},{"1":"233","2":"0.4878049"},{"1":"234","2":"0.5317073"},{"1":"235","2":"0.5073171"},{"1":"236","2":"0.4975610"},{"1":"237","2":"0.5073171"},{"1":"238","2":"0.5365854"},{"1":"239","2":"0.4195122"},{"1":"240","2":"0.4585366"},{"1":"241","2":"0.5560976"},{"1":"242","2":"0.4926829"},{"1":"243","2":"0.4292683"},{"1":"244","2":"0.4487805"},{"1":"245","2":"0.5317073"},{"1":"246","2":"0.4439024"},{"1":"247","2":"0.4585366"},{"1":"248","2":"0.4682927"},{"1":"249","2":"0.5219512"},{"1":"250","2":"0.4536585"},{"1":"251","2":"0.4682927"},{"1":"252","2":"0.5414634"},{"1":"253","2":"0.5219512"},{"1":"254","2":"0.5414634"},{"1":"255","2":"0.4829268"},{"1":"256","2":"0.4585366"},{"1":"257","2":"0.5121951"},{"1":"258","2":"0.4878049"},{"1":"259","2":"0.5268293"},{"1":"260","2":"0.4878049"},{"1":"261","2":"0.5024390"},{"1":"262","2":"0.4439024"},{"1":"263","2":"0.5121951"},{"1":"264","2":"0.5219512"},{"1":"265","2":"0.5268293"},{"1":"266","2":"0.4975610"},{"1":"267","2":"0.4780488"},{"1":"268","2":"0.4487805"},{"1":"269","2":"0.5073171"},{"1":"270","2":"0.5024390"},{"1":"271","2":"0.5073171"},{"1":"272","2":"0.4975610"},{"1":"273","2":"0.5024390"},{"1":"274","2":"0.4926829"},{"1":"275","2":"0.4878049"},{"1":"276","2":"0.4634146"},{"1":"277","2":"0.4390244"},{"1":"278","2":"0.5170732"},{"1":"279","2":"0.4682927"},{"1":"280","2":"0.4926829"},{"1":"281","2":"0.5268293"},{"1":"282","2":"0.5317073"},{"1":"283","2":"0.4878049"},{"1":"284","2":"0.5414634"},{"1":"285","2":"0.4780488"},{"1":"286","2":"0.5463415"},{"1":"287","2":"0.4780488"},{"1":"288","2":"0.5073171"},{"1":"289","2":"0.4926829"},{"1":"290","2":"0.4731707"},{"1":"291","2":"0.5609756"},{"1":"292","2":"0.4975610"},{"1":"293","2":"0.5024390"},{"1":"294","2":"0.5121951"},{"1":"295","2":"0.4585366"},{"1":"296","2":"0.5073171"},{"1":"297","2":"0.4682927"},{"1":"298","2":"0.4634146"},{"1":"299","2":"0.4878049"},{"1":"300","2":"0.4585366"},{"1":"301","2":"0.4682927"},{"1":"302","2":"0.5073171"},{"1":"303","2":"0.5170732"},{"1":"304","2":"0.5219512"},{"1":"305","2":"0.5463415"},{"1":"306","2":"0.4585366"},{"1":"307","2":"0.4829268"},{"1":"308","2":"0.4878049"},{"1":"309","2":"0.5317073"},{"1":"310","2":"0.5121951"},{"1":"311","2":"0.4975610"},{"1":"312","2":"0.5121951"},{"1":"313","2":"0.5121951"},{"1":"314","2":"0.5463415"},{"1":"315","2":"0.5073171"},{"1":"316","2":"0.5024390"},{"1":"317","2":"0.4780488"},{"1":"318","2":"0.4536585"},{"1":"319","2":"0.5219512"},{"1":"320","2":"0.4926829"},{"1":"321","2":"0.4975610"},{"1":"322","2":"0.5268293"},{"1":"323","2":"0.4682927"},{"1":"324","2":"0.5073171"},{"1":"325","2":"0.4780488"},{"1":"326","2":"0.4634146"},{"1":"327","2":"0.5268293"},{"1":"328","2":"0.4829268"},{"1":"329","2":"0.4926829"},{"1":"330","2":"0.5024390"},{"1":"331","2":"0.5170732"},{"1":"332","2":"0.4634146"},{"1":"333","2":"0.4926829"},{"1":"334","2":"0.4439024"},{"1":"335","2":"0.4634146"},{"1":"336","2":"0.5268293"},{"1":"337","2":"0.5121951"},{"1":"338","2":"0.5073171"},{"1":"339","2":"0.5024390"},{"1":"340","2":"0.5756098"},{"1":"341","2":"0.5756098"},{"1":"342","2":"0.4878049"},{"1":"343","2":"0.4536585"},{"1":"344","2":"0.4536585"},{"1":"345","2":"0.4585366"},{"1":"346","2":"0.4731707"},{"1":"347","2":"0.4878049"},{"1":"348","2":"0.4878049"},{"1":"349","2":"0.5512195"},{"1":"350","2":"0.5170732"},{"1":"351","2":"0.4926829"},{"1":"352","2":"0.4780488"},{"1":"353","2":"0.4926829"},{"1":"354","2":"0.4926829"},{"1":"355","2":"0.4926829"},{"1":"356","2":"0.4975610"},{"1":"357","2":"0.4829268"},{"1":"358","2":"0.4829268"},{"1":"359","2":"0.5219512"},{"1":"360","2":"0.5219512"},{"1":"361","2":"0.4829268"},{"1":"362","2":"0.4682927"},{"1":"363","2":"0.5073171"},{"1":"364","2":"0.4829268"},{"1":"365","2":"0.5365854"},{"1":"366","2":"0.4585366"},{"1":"367","2":"0.5219512"},{"1":"368","2":"0.4829268"},{"1":"369","2":"0.4146341"},{"1":"370","2":"0.5121951"},{"1":"371","2":"0.4829268"},{"1":"372","2":"0.5073171"},{"1":"373","2":"0.5365854"},{"1":"374","2":"0.4731707"},{"1":"375","2":"0.4975610"},{"1":"376","2":"0.5219512"},{"1":"377","2":"0.5317073"},{"1":"378","2":"0.4097561"},{"1":"379","2":"0.4731707"},{"1":"380","2":"0.5024390"},{"1":"381","2":"0.4829268"},{"1":"382","2":"0.5365854"},{"1":"383","2":"0.4682927"},{"1":"384","2":"0.4585366"},{"1":"385","2":"0.5219512"},{"1":"386","2":"0.5219512"},{"1":"387","2":"0.4926829"},{"1":"388","2":"0.5853659"},{"1":"389","2":"0.4975610"},{"1":"390","2":"0.5365854"},{"1":"391","2":"0.4926829"},{"1":"392","2":"0.5170732"},{"1":"393","2":"0.5219512"},{"1":"394","2":"0.5268293"},{"1":"395","2":"0.4634146"},{"1":"396","2":"0.5073171"},{"1":"397","2":"0.4878049"},{"1":"398","2":"0.5170732"},{"1":"399","2":"0.5609756"},{"1":"400","2":"0.4780488"},{"1":"401","2":"0.5121951"},{"1":"402","2":"0.4975610"},{"1":"403","2":"0.5414634"},{"1":"404","2":"0.4829268"},{"1":"405","2":"0.5121951"},{"1":"406","2":"0.4487805"},{"1":"407","2":"0.5365854"},{"1":"408","2":"0.4878049"},{"1":"409","2":"0.5073171"},{"1":"410","2":"0.5317073"},{"1":"411","2":"0.5463415"},{"1":"412","2":"0.4536585"},{"1":"413","2":"0.4780488"},{"1":"414","2":"0.5512195"},{"1":"415","2":"0.5073171"},{"1":"416","2":"0.4975610"},{"1":"417","2":"0.5609756"},{"1":"418","2":"0.4390244"},{"1":"419","2":"0.5024390"},{"1":"420","2":"0.4780488"},{"1":"421","2":"0.4634146"},{"1":"422","2":"0.5073171"},{"1":"423","2":"0.5317073"},{"1":"424","2":"0.4926829"},{"1":"425","2":"0.5317073"},{"1":"426","2":"0.4829268"},{"1":"427","2":"0.4975610"},{"1":"428","2":"0.4731707"},{"1":"429","2":"0.4975610"},{"1":"430","2":"0.4487805"},{"1":"431","2":"0.5121951"},{"1":"432","2":"0.5707317"},{"1":"433","2":"0.5219512"},{"1":"434","2":"0.4731707"},{"1":"435","2":"0.5073171"},{"1":"436","2":"0.5121951"},{"1":"437","2":"0.4780488"},{"1":"438","2":"0.5658537"},{"1":"439","2":"0.4878049"},{"1":"440","2":"0.5170732"},{"1":"441","2":"0.4634146"},{"1":"442","2":"0.4975610"},{"1":"443","2":"0.4731707"},{"1":"444","2":"0.5268293"},{"1":"445","2":"0.5024390"},{"1":"446","2":"0.4878049"},{"1":"447","2":"0.5317073"},{"1":"448","2":"0.4585366"},{"1":"449","2":"0.5414634"},{"1":"450","2":"0.5902439"},{"1":"451","2":"0.4829268"},{"1":"452","2":"0.5268293"},{"1":"453","2":"0.5073171"},{"1":"454","2":"0.5756098"},{"1":"455","2":"0.4536585"},{"1":"456","2":"0.4682927"},{"1":"457","2":"0.5170732"},{"1":"458","2":"0.4829268"},{"1":"459","2":"0.4780488"},{"1":"460","2":"0.4829268"},{"1":"461","2":"0.4536585"},{"1":"462","2":"0.5463415"},{"1":"463","2":"0.4731707"},{"1":"464","2":"0.5317073"},{"1":"465","2":"0.4878049"},{"1":"466","2":"0.4682927"},{"1":"467","2":"0.4878049"},{"1":"468","2":"0.4487805"},{"1":"469","2":"0.4731707"},{"1":"470","2":"0.4634146"},{"1":"471","2":"0.5560976"},{"1":"472","2":"0.5317073"},{"1":"473","2":"0.4341463"},{"1":"474","2":"0.5024390"},{"1":"475","2":"0.5512195"},{"1":"476","2":"0.5317073"},{"1":"477","2":"0.4341463"},{"1":"478","2":"0.4829268"},{"1":"479","2":"0.5609756"},{"1":"480","2":"0.4195122"},{"1":"481","2":"0.5365854"},{"1":"482","2":"0.5317073"},{"1":"483","2":"0.5121951"},{"1":"484","2":"0.4146341"},{"1":"485","2":"0.4731707"},{"1":"486","2":"0.4975610"},{"1":"487","2":"0.4926829"},{"1":"488","2":"0.5073171"},{"1":"489","2":"0.5170732"},{"1":"490","2":"0.4926829"},{"1":"491","2":"0.4634146"},{"1":"492","2":"0.5414634"},{"1":"493","2":"0.4439024"},{"1":"494","2":"0.5365854"},{"1":"495","2":"0.5024390"},{"1":"496","2":"0.5219512"},{"1":"497","2":"0.5463415"},{"1":"498","2":"0.5512195"},{"1":"499","2":"0.4585366"},{"1":"500","2":"0.5121951"},{"1":"501","2":"0.4390244"},{"1":"502","2":"0.4975610"},{"1":"503","2":"0.4975610"},{"1":"504","2":"0.4878049"},{"1":"505","2":"0.4878049"},{"1":"506","2":"0.5121951"},{"1":"507","2":"0.5219512"},{"1":"508","2":"0.4682927"},{"1":"509","2":"0.5024390"},{"1":"510","2":"0.4536585"},{"1":"511","2":"0.4975610"},{"1":"512","2":"0.5268293"},{"1":"513","2":"0.4682927"},{"1":"514","2":"0.4975610"},{"1":"515","2":"0.5365854"},{"1":"516","2":"0.5024390"},{"1":"517","2":"0.5512195"},{"1":"518","2":"0.5024390"},{"1":"519","2":"0.4926829"},{"1":"520","2":"0.5317073"},{"1":"521","2":"0.4878049"},{"1":"522","2":"0.5024390"},{"1":"523","2":"0.4195122"},{"1":"524","2":"0.5073171"},{"1":"525","2":"0.5365854"},{"1":"526","2":"0.5073171"},{"1":"527","2":"0.5560976"},{"1":"528","2":"0.5121951"},{"1":"529","2":"0.4536585"},{"1":"530","2":"0.4878049"},{"1":"531","2":"0.5609756"},{"1":"532","2":"0.4585366"},{"1":"533","2":"0.4243902"},{"1":"534","2":"0.4878049"},{"1":"535","2":"0.4634146"},{"1":"536","2":"0.4878049"},{"1":"537","2":"0.5902439"},{"1":"538","2":"0.5024390"},{"1":"539","2":"0.5463415"},{"1":"540","2":"0.4829268"},{"1":"541","2":"0.4682927"},{"1":"542","2":"0.4780488"},{"1":"543","2":"0.4682927"},{"1":"544","2":"0.4536585"},{"1":"545","2":"0.4682927"},{"1":"546","2":"0.5121951"},{"1":"547","2":"0.4682927"},{"1":"548","2":"0.4926829"},{"1":"549","2":"0.5219512"},{"1":"550","2":"0.4585366"},{"1":"551","2":"0.4585366"},{"1":"552","2":"0.5024390"},{"1":"553","2":"0.4634146"},{"1":"554","2":"0.4341463"},{"1":"555","2":"0.4439024"},{"1":"556","2":"0.5121951"},{"1":"557","2":"0.5365854"},{"1":"558","2":"0.5024390"},{"1":"559","2":"0.4975610"},{"1":"560","2":"0.5512195"},{"1":"561","2":"0.4780488"},{"1":"562","2":"0.5414634"},{"1":"563","2":"0.4926829"},{"1":"564","2":"0.5219512"},{"1":"565","2":"0.5463415"},{"1":"566","2":"0.4926829"},{"1":"567","2":"0.5219512"},{"1":"568","2":"0.5414634"},{"1":"569","2":"0.5560976"},{"1":"570","2":"0.4975610"},{"1":"571","2":"0.5073171"},{"1":"572","2":"0.5317073"},{"1":"573","2":"0.5121951"},{"1":"574","2":"0.5219512"},{"1":"575","2":"0.4975610"},{"1":"576","2":"0.4975610"},{"1":"577","2":"0.4731707"},{"1":"578","2":"0.4829268"},{"1":"579","2":"0.5024390"},{"1":"580","2":"0.4878049"},{"1":"581","2":"0.5170732"},{"1":"582","2":"0.4780488"},{"1":"583","2":"0.4878049"},{"1":"584","2":"0.4390244"},{"1":"585","2":"0.5756098"},{"1":"586","2":"0.5170732"},{"1":"587","2":"0.5121951"},{"1":"588","2":"0.4634146"},{"1":"589","2":"0.4341463"},{"1":"590","2":"0.4341463"},{"1":"591","2":"0.5073171"},{"1":"592","2":"0.5170732"},{"1":"593","2":"0.5170732"},{"1":"594","2":"0.4585366"},{"1":"595","2":"0.4926829"},{"1":"596","2":"0.5268293"},{"1":"597","2":"0.5317073"},{"1":"598","2":"0.4731707"},{"1":"599","2":"0.4731707"},{"1":"600","2":"0.4829268"},{"1":"601","2":"0.4878049"},{"1":"602","2":"0.4341463"},{"1":"603","2":"0.5073171"},{"1":"604","2":"0.5170732"},{"1":"605","2":"0.5121951"},{"1":"606","2":"0.4292683"},{"1":"607","2":"0.5268293"},{"1":"608","2":"0.4829268"},{"1":"609","2":"0.5268293"},{"1":"610","2":"0.5463415"},{"1":"611","2":"0.5804878"},{"1":"612","2":"0.4585366"},{"1":"613","2":"0.4926829"},{"1":"614","2":"0.4975610"},{"1":"615","2":"0.4682927"},{"1":"616","2":"0.5268293"},{"1":"617","2":"0.5268293"},{"1":"618","2":"0.4975610"},{"1":"619","2":"0.4829268"},{"1":"620","2":"0.5170732"},{"1":"621","2":"0.5121951"},{"1":"622","2":"0.4731707"},{"1":"623","2":"0.4878049"},{"1":"624","2":"0.5512195"},{"1":"625","2":"0.4829268"},{"1":"626","2":"0.5024390"},{"1":"627","2":"0.4731707"},{"1":"628","2":"0.5073171"},{"1":"629","2":"0.4634146"},{"1":"630","2":"0.4926829"},{"1":"631","2":"0.4487805"},{"1":"632","2":"0.5560976"},{"1":"633","2":"0.4878049"},{"1":"634","2":"0.4731707"},{"1":"635","2":"0.4926829"},{"1":"636","2":"0.4731707"},{"1":"637","2":"0.5073171"},{"1":"638","2":"0.5268293"},{"1":"639","2":"0.5268293"},{"1":"640","2":"0.5073171"},{"1":"641","2":"0.5512195"},{"1":"642","2":"0.4829268"},{"1":"643","2":"0.4926829"},{"1":"644","2":"0.4731707"},{"1":"645","2":"0.5219512"},{"1":"646","2":"0.5073171"},{"1":"647","2":"0.5219512"},{"1":"648","2":"0.4878049"},{"1":"649","2":"0.5170732"},{"1":"650","2":"0.5317073"},{"1":"651","2":"0.4829268"},{"1":"652","2":"0.5463415"},{"1":"653","2":"0.5219512"},{"1":"654","2":"0.5707317"},{"1":"655","2":"0.5121951"},{"1":"656","2":"0.5268293"},{"1":"657","2":"0.5463415"},{"1":"658","2":"0.5756098"},{"1":"659","2":"0.4975610"},{"1":"660","2":"0.4634146"},{"1":"661","2":"0.4634146"},{"1":"662","2":"0.4829268"},{"1":"663","2":"0.4878049"},{"1":"664","2":"0.5073171"},{"1":"665","2":"0.4634146"},{"1":"666","2":"0.4292683"},{"1":"667","2":"0.4634146"},{"1":"668","2":"0.5121951"},{"1":"669","2":"0.5365854"},{"1":"670","2":"0.5658537"},{"1":"671","2":"0.4585366"},{"1":"672","2":"0.4634146"},{"1":"673","2":"0.4487805"},{"1":"674","2":"0.4682927"},{"1":"675","2":"0.5024390"},{"1":"676","2":"0.5121951"},{"1":"677","2":"0.5463415"},{"1":"678","2":"0.5609756"},{"1":"679","2":"0.4682927"},{"1":"680","2":"0.4975610"},{"1":"681","2":"0.5024390"},{"1":"682","2":"0.5317073"},{"1":"683","2":"0.5121951"},{"1":"684","2":"0.5707317"},{"1":"685","2":"0.5414634"},{"1":"686","2":"0.4780488"},{"1":"687","2":"0.5902439"},{"1":"688","2":"0.4975610"},{"1":"689","2":"0.5024390"},{"1":"690","2":"0.4926829"},{"1":"691","2":"0.5121951"},{"1":"692","2":"0.5170732"},{"1":"693","2":"0.5170732"},{"1":"694","2":"0.4390244"},{"1":"695","2":"0.5219512"},{"1":"696","2":"0.5658537"},{"1":"697","2":"0.5024390"},{"1":"698","2":"0.5317073"},{"1":"699","2":"0.5024390"},{"1":"700","2":"0.5512195"},{"1":"701","2":"0.5512195"},{"1":"702","2":"0.5073171"},{"1":"703","2":"0.4975610"},{"1":"704","2":"0.5365854"},{"1":"705","2":"0.4926829"},{"1":"706","2":"0.4487805"},{"1":"707","2":"0.4682927"},{"1":"708","2":"0.5365854"},{"1":"709","2":"0.4682927"},{"1":"710","2":"0.4390244"},{"1":"711","2":"0.4780488"},{"1":"712","2":"0.5560976"},{"1":"713","2":"0.5463415"},{"1":"714","2":"0.4585366"},{"1":"715","2":"0.5463415"},{"1":"716","2":"0.5024390"},{"1":"717","2":"0.4975610"},{"1":"718","2":"0.5073171"},{"1":"719","2":"0.5219512"},{"1":"720","2":"0.4975610"},{"1":"721","2":"0.4829268"},{"1":"722","2":"0.4487805"},{"1":"723","2":"0.5609756"},{"1":"724","2":"0.5073171"},{"1":"725","2":"0.5804878"},{"1":"726","2":"0.5853659"},{"1":"727","2":"0.5121951"},{"1":"728","2":"0.5268293"},{"1":"729","2":"0.4878049"},{"1":"730","2":"0.5268293"},{"1":"731","2":"0.5121951"},{"1":"732","2":"0.4536585"},{"1":"733","2":"0.5121951"},{"1":"734","2":"0.4975610"},{"1":"735","2":"0.4975610"},{"1":"736","2":"0.4634146"},{"1":"737","2":"0.4487805"},{"1":"738","2":"0.4926829"},{"1":"739","2":"0.5024390"},{"1":"740","2":"0.5317073"},{"1":"741","2":"0.4731707"},{"1":"742","2":"0.5219512"},{"1":"743","2":"0.4780488"},{"1":"744","2":"0.5121951"},{"1":"745","2":"0.4487805"},{"1":"746","2":"0.4780488"},{"1":"747","2":"0.4634146"},{"1":"748","2":"0.4682927"},{"1":"749","2":"0.4926829"},{"1":"750","2":"0.4926829"},{"1":"751","2":"0.5024390"},{"1":"752","2":"0.5317073"},{"1":"753","2":"0.4731707"},{"1":"754","2":"0.4926829"},{"1":"755","2":"0.4829268"},{"1":"756","2":"0.4536585"},{"1":"757","2":"0.5121951"},{"1":"758","2":"0.5317073"},{"1":"759","2":"0.5560976"},{"1":"760","2":"0.4926829"},{"1":"761","2":"0.4780488"},{"1":"762","2":"0.4926829"},{"1":"763","2":"0.5560976"},{"1":"764","2":"0.4585366"},{"1":"765","2":"0.4878049"},{"1":"766","2":"0.4682927"},{"1":"767","2":"0.5024390"},{"1":"768","2":"0.5073171"},{"1":"769","2":"0.4487805"},{"1":"770","2":"0.4926829"},{"1":"771","2":"0.5073171"},{"1":"772","2":"0.5268293"},{"1":"773","2":"0.4731707"},{"1":"774","2":"0.5463415"},{"1":"775","2":"0.5658537"},{"1":"776","2":"0.4634146"},{"1":"777","2":"0.5219512"},{"1":"778","2":"0.4780488"},{"1":"779","2":"0.5268293"},{"1":"780","2":"0.5024390"},{"1":"781","2":"0.4780488"},{"1":"782","2":"0.4926829"},{"1":"783","2":"0.4585366"},{"1":"784","2":"0.5609756"},{"1":"785","2":"0.5073171"},{"1":"786","2":"0.4634146"},{"1":"787","2":"0.4878049"},{"1":"788","2":"0.5317073"},{"1":"789","2":"0.5365854"},{"1":"790","2":"0.5121951"},{"1":"791","2":"0.5463415"},{"1":"792","2":"0.5512195"},{"1":"793","2":"0.4926829"},{"1":"794","2":"0.4195122"},{"1":"795","2":"0.4780488"},{"1":"796","2":"0.5512195"},{"1":"797","2":"0.4829268"},{"1":"798","2":"0.5268293"},{"1":"799","2":"0.5219512"},{"1":"800","2":"0.5170732"},{"1":"801","2":"0.4195122"},{"1":"802","2":"0.5170732"},{"1":"803","2":"0.4780488"},{"1":"804","2":"0.5268293"},{"1":"805","2":"0.5365854"},{"1":"806","2":"0.5658537"},{"1":"807","2":"0.4341463"},{"1":"808","2":"0.5317073"},{"1":"809","2":"0.4975610"},{"1":"810","2":"0.4829268"},{"1":"811","2":"0.5073171"},{"1":"812","2":"0.4682927"},{"1":"813","2":"0.4975610"},{"1":"814","2":"0.5317073"},{"1":"815","2":"0.5024390"},{"1":"816","2":"0.5219512"},{"1":"817","2":"0.5268293"},{"1":"818","2":"0.5463415"},{"1":"819","2":"0.5073171"},{"1":"820","2":"0.5512195"},{"1":"821","2":"0.4926829"},{"1":"822","2":"0.4731707"},{"1":"823","2":"0.4585366"},{"1":"824","2":"0.4682927"},{"1":"825","2":"0.5268293"},{"1":"826","2":"0.5268293"},{"1":"827","2":"0.4926829"},{"1":"828","2":"0.4731707"},{"1":"829","2":"0.5804878"},{"1":"830","2":"0.5512195"},{"1":"831","2":"0.5170732"},{"1":"832","2":"0.5170732"},{"1":"833","2":"0.5073171"},{"1":"834","2":"0.4829268"},{"1":"835","2":"0.4878049"},{"1":"836","2":"0.5073171"},{"1":"837","2":"0.4926829"},{"1":"838","2":"0.5219512"},{"1":"839","2":"0.4975610"},{"1":"840","2":"0.5219512"},{"1":"841","2":"0.4829268"},{"1":"842","2":"0.5170732"},{"1":"843","2":"0.5463415"},{"1":"844","2":"0.5219512"},{"1":"845","2":"0.5073171"},{"1":"846","2":"0.5512195"},{"1":"847","2":"0.4829268"},{"1":"848","2":"0.5609756"},{"1":"849","2":"0.5365854"},{"1":"850","2":"0.4829268"},{"1":"851","2":"0.4731707"},{"1":"852","2":"0.4829268"},{"1":"853","2":"0.4975610"},{"1":"854","2":"0.4878049"},{"1":"855","2":"0.5365854"},{"1":"856","2":"0.5219512"},{"1":"857","2":"0.4780488"},{"1":"858","2":"0.5024390"},{"1":"859","2":"0.4975610"},{"1":"860","2":"0.4439024"},{"1":"861","2":"0.4634146"},{"1":"862","2":"0.6097561"},{"1":"863","2":"0.4926829"},{"1":"864","2":"0.4390244"},{"1":"865","2":"0.5073171"},{"1":"866","2":"0.4536585"},{"1":"867","2":"0.4829268"},{"1":"868","2":"0.5073171"},{"1":"869","2":"0.5073171"},{"1":"870","2":"0.4878049"},{"1":"871","2":"0.5024390"},{"1":"872","2":"0.5073171"},{"1":"873","2":"0.5365854"},{"1":"874","2":"0.5121951"},{"1":"875","2":"0.4682927"},{"1":"876","2":"0.5121951"},{"1":"877","2":"0.5121951"},{"1":"878","2":"0.5121951"},{"1":"879","2":"0.4878049"},{"1":"880","2":"0.4975610"},{"1":"881","2":"0.5658537"},{"1":"882","2":"0.4878049"},{"1":"883","2":"0.5121951"},{"1":"884","2":"0.4634146"},{"1":"885","2":"0.5414634"},{"1":"886","2":"0.4390244"},{"1":"887","2":"0.4585366"},{"1":"888","2":"0.5365854"},{"1":"889","2":"0.5024390"},{"1":"890","2":"0.5121951"},{"1":"891","2":"0.5268293"},{"1":"892","2":"0.5317073"},{"1":"893","2":"0.4780488"},{"1":"894","2":"0.5121951"},{"1":"895","2":"0.5024390"},{"1":"896","2":"0.4585366"},{"1":"897","2":"0.5170732"},{"1":"898","2":"0.4780488"},{"1":"899","2":"0.5121951"},{"1":"900","2":"0.5024390"},{"1":"901","2":"0.5756098"},{"1":"902","2":"0.4878049"},{"1":"903","2":"0.4439024"},{"1":"904","2":"0.5268293"},{"1":"905","2":"0.4975610"},{"1":"906","2":"0.4829268"},{"1":"907","2":"0.5121951"},{"1":"908","2":"0.5512195"},{"1":"909","2":"0.4975610"},{"1":"910","2":"0.4682927"},{"1":"911","2":"0.5219512"},{"1":"912","2":"0.4536585"},{"1":"913","2":"0.5024390"},{"1":"914","2":"0.4878049"},{"1":"915","2":"0.5414634"},{"1":"916","2":"0.5853659"},{"1":"917","2":"0.5024390"},{"1":"918","2":"0.5219512"},{"1":"919","2":"0.5170732"},{"1":"920","2":"0.4829268"},{"1":"921","2":"0.4439024"},{"1":"922","2":"0.5707317"},{"1":"923","2":"0.5317073"},{"1":"924","2":"0.5268293"},{"1":"925","2":"0.5463415"},{"1":"926","2":"0.5317073"},{"1":"927","2":"0.5170732"},{"1":"928","2":"0.5463415"},{"1":"929","2":"0.4341463"},{"1":"930","2":"0.5463415"},{"1":"931","2":"0.5170732"},{"1":"932","2":"0.5219512"},{"1":"933","2":"0.4682927"},{"1":"934","2":"0.5121951"},{"1":"935","2":"0.4975610"},{"1":"936","2":"0.5024390"},{"1":"937","2":"0.4536585"},{"1":"938","2":"0.5317073"},{"1":"939","2":"0.5804878"},{"1":"940","2":"0.5121951"},{"1":"941","2":"0.4536585"},{"1":"942","2":"0.4829268"},{"1":"943","2":"0.5268293"},{"1":"944","2":"0.5121951"},{"1":"945","2":"0.4926829"},{"1":"946","2":"0.4634146"},{"1":"947","2":"0.4926829"},{"1":"948","2":"0.4829268"},{"1":"949","2":"0.4829268"},{"1":"950","2":"0.5073171"},{"1":"951","2":"0.4341463"},{"1":"952","2":"0.5073171"},{"1":"953","2":"0.5268293"},{"1":"954","2":"0.4439024"},{"1":"955","2":"0.4975610"},{"1":"956","2":"0.4926829"},{"1":"957","2":"0.4780488"},{"1":"958","2":"0.5073171"},{"1":"959","2":"0.5560976"},{"1":"960","2":"0.4780488"},{"1":"961","2":"0.4634146"},{"1":"962","2":"0.5024390"},{"1":"963","2":"0.4975610"},{"1":"964","2":"0.5268293"},{"1":"965","2":"0.5365854"},{"1":"966","2":"0.4780488"},{"1":"967","2":"0.5121951"},{"1":"968","2":"0.5073171"},{"1":"969","2":"0.5024390"},{"1":"970","2":"0.4731707"},{"1":"971","2":"0.4780488"},{"1":"972","2":"0.5414634"},{"1":"973","2":"0.5365854"},{"1":"974","2":"0.5512195"},{"1":"975","2":"0.5121951"},{"1":"976","2":"0.5804878"},{"1":"977","2":"0.4682927"},{"1":"978","2":"0.5121951"},{"1":"979","2":"0.5219512"},{"1":"980","2":"0.5170732"},{"1":"981","2":"0.5414634"},{"1":"982","2":"0.5219512"},{"1":"983","2":"0.4780488"},{"1":"984","2":"0.5756098"},{"1":"985","2":"0.3902439"},{"1":"986","2":"0.4682927"},{"1":"987","2":"0.4780488"},{"1":"988","2":"0.4731707"},{"1":"989","2":"0.5024390"},{"1":"990","2":"0.5902439"},{"1":"991","2":"0.5024390"},{"1":"992","2":"0.5609756"},{"1":"993","2":"0.5560976"},{"1":"994","2":"0.5024390"},{"1":"995","2":"0.5609756"},{"1":"996","2":"0.5073171"},{"1":"997","2":"0.5512195"},{"1":"998","2":"0.5463415"},{"1":"999","2":"0.5170732"},{"1":"1000","2":"0.5170732"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

This list of proportions is the sampling distribution. It represents possible sample proportions of females with melanoma **under the assumption that the null is true**. In other words, even if the "true" proportion of female melanoma patients were 0.5, these are all values that can result from random samples.

In the `hypothesize` command, we use "point" to tell `infer` that we want the null to be centered at the point 0.5. In the `generate` command, we need to specify the `type` as "draw" instead of "permute". We are not shuffling any values here; we are "drawing" values from a probability distribution like coin flips. Everything else in the command is pretty self-explanatory.

The value of our test statistic, `female_prop`, is 0.6146341. It appears in the right tail:


```r
melanoma_test %>%
    visualize() +
    shade_p_value(obs_stat = female_prop, direction = "two-sided")
```

<img src="11-hypothesis_testing_with_randomization_2-web_files/figure-html/unnamed-chunk-8-1.png" width="672" />

Although the line only appears on the right, keep in mind that we are conducting a two-sided test, so we are interested in values more extreme than the red line on the right, but also more extreme than a similarly placed line on the left.

##### Exercise 3 {-}

The red line sits at about 0.615. If you were to draw a red line on the above histogram that represented a value equally distant from 0.5, but on the left instead of the right, where would that line be? Do a little arithmetic to figure it out and show your work.

::: {.answer}

Please write up your answer here.

:::

### Calculate the P-value. {#hypothesis2-ex-calculate-p}


```r
melanoma_test %>%
    get_p_value(obs_stat = female_prop, direction = "two-sided")
```

```
## Warning: Please be cautious in reporting a p-value of 0. This result is an
## approximation based on the number of `reps` chosen in the `generate()` step.
## See `?get_p_value()` for more information.
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["p_value"],"name":[1],"type":["dbl"],"align":["right"]}],"data":[{"1":"0"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

The P-value appears to be zero. Indeed, among the 1000 simulated values, we saw none that exceeded 0.615 and none that were less than 0.385. However, a true P-value can never be zero. If you did millions or billions of simulations (please don't try!), surely there would be one or two with even more extreme values. In cases when the P-value is really, really tiny, it is traditional to report $P < 0.001$. It is **incorrect** to say $P = 0$.

### Interpret the P-value as a probability given the null. {#hypothesis2-ex-interpret-p}

$P < 0.001$. If there were no sex bias in malignant melanoma patients, there would be less than a 0.1% chance of seeing a percentage of females at least as extreme as the one we saw in our data.

Note: Don't forget to interpret the P-value in a contextually meaningful way. The P-value is the probability under the assumption of the null hypothesis of seeing data at least as extreme as the data we saw. In this context, that means that if we assume 50% of patients are female, it would be extremely rare to see more than 61.5% or less than 38.5% females in a sample of size 205.


## Conclusion {#hypothesis2-ex-ht-conclusion}

### State the statistical conclusion. {#hypothesis2-ex-stat-conclusion}

We reject the null hypothesis.

### State (but do not overstate) a contextually meaningful conclusion. {#hypothesis2-ex-context-conclusion}

There is sufficient evidence that there is a sex bias in patients who suffer from malignant melanoma.

### Express reservations or uncertainty about the generalizability of the conclusion.  {#hypothesis2-ex-reservations}

We have no idea how these patients were sampled. Are these all the patients in Denmark with malignant melanoma over a certain period of time? Were they part of a convenience sample? As a result of our uncertainly about the sampling process, we can't be sure if the results generalize to a larger population, either in Denmark or especially outside of Denmark.

##### Exercise 4 {-}

Can you find on the internet any evidence that females do indeed suffer from malignant melanoma more often than males (not just in Denmark, but anywhere)?

::: {.answer}

Please write up your answer here.

:::

### Identify the possibility of either a Type I or Type II error and state what making such an error means in the context of the hypotheses. {#hypothesis2-ex-errors}

As we rejected the null, we run the risk of making a Type I error. If we have made such an error, that would mean that patients with malignant melanoma are equally likely to be male or female, but that we got a sample with an unusual number of female patients.


## Your turn {#hypothesis2-your-turn}

Determine if the percentage of patients in Denmark with malignant melanoma who also have an ulcerated tumor (measured with the `ulcer` variable) is significantly different from 50%.

As before, you have the outline of the rubric for inference below. Some of the steps will be the same or similar to steps in the example above. It is perfectly okay to copy and paste R code, making the necessary changes. It is **not** okay to copy and paste text. You need to put everything into your own words.

The template below is exactly the same as in the appendix ([Rubric for inference](#appendix-rubric)) up to the part about confidence intervals which we haven't learned yet.


##### Exploratory data analysis {-}

###### Use data documentation (help files, code books, Google, etc.) to determine as much as possible about the data provenance and structure. {-}

::: {.answer}


```r
# Add code here to understand the data.
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
set.seed(42)
# Add code here to simulate the null distribution.
# Run 1000 simulations like in the earlier example.
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


## Conclusion {#hypothesis2-conclusion}

Now you have seen two fully-worked examples of hypothesis tests using randomization, and you have created two more examples on your own. Hopefully, the logic of inference and the process of running a formal hypothesis test are starting to make sense.

Keep in mind that the outline of steps will not change. However, the way each step is carried out will vary from problem to problem. Not only does the context change (one example involved sex discrimination, the other melanoma patients), but the statistics you compute also change (one example compared proportions from two samples and the other only had one proportion from a single sample). Pay close attention to the research question and the data that will be used to answer that question. That will be the only information you have to help you know which hypothesis test applies.

### Preparing and submitting your assignment {#hypothesis2-prep}

1. From the "Run" menu, select "Restart R and Run All Chunks".
2. Deal with any code errors that crop up. Repeat steps 1–-2 until there are no more code errors.
3. Spell check your document by clicking the icon with "ABC" and a check mark.
4. Hit the "Preview" button one last time to generate the final draft of the `.nb.html` file.
5. Proofread the HTML file carefully. If there are errors, go back and fix them, then repeat steps 1--5 again.

If you have completed this chapter as part of a statistics course, follow the directions you receive from your professor to submit your assignment.



