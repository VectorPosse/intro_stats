# Hypothesis testing with randomization, Part 1 {#hypothesis1}  

<!-- Please don't mess with the next few lines! -->
<style>h5{font-size:2em;color:#0000FF}h6{font-size:1.5em;color:#0000FF}div.answer{margin-left:5%;border:1px solid #0000FF;border-left-width:10px;padding:25px} div.summary{background-color:rgba(30,144,255,0.1);border:3px double #0000FF;padding:25px}</style><p style="color:#ffffff">2.0</p>
<!-- Please don't mess with the previous few lines! -->

::: {.summary}

### Functions introduced in this chapter {-}

`drop_na`, `pull`

:::


## Introduction {#hypothesis1-intro}

Using a sample to deduce something about a population is called "statistical inference". In this chapter, we'll learn about one form of statistical inference called "hypothesis testing". The focus will be on walking through the example from Part 2 of "Introduction to randomization" and recasting it here as a formal hypothesis test.

There are no new R commands here, but there are many new ideas that will require careful reading. You are not expected to be an expert on hypothesis testing after this one chapter. However, within the next few chapters, as we learn more about hypothesis testing and work through many more examples, the hope is that you will begin to assimilate and internalize the logic of inference and the steps of a hypothesis test.

### Install new packages {#hypothesis1-install}

There are no new packages used in this chapter.

### Download the R notebook file {#hypothesis1-download}

Check the upper-right corner in RStudio to make sure you're in your `intro_stats` project. Then click on the following link to download this chapter as an R notebook file (`.Rmd`).

<a href = "https://vectorposse.github.io/intro_stats/chapter_downloads/10-hypothesis_testing_with_randomization_1.Rmd" download>https://vectorposse.github.io/intro_stats/chapter_downloads/10-hypothesis_testing_with_randomization_1.Rmd</a>

Once the file is downloaded, move it to your project folder in RStudio and open it there.

### Restart R and run all chunks {#hypothesis1-restart}

In RStudio, select "Restart R and Run All Chunks" from the "Run" menu.

## Load packages {#hypothesis1-load}

We load `tidyverse` and `janitor`. We'll continue to explore the `infer` package for investigating statistical claims. We load the `openintro` package to access the `sex_discrimination` data (the one with the male bank managers promoting male files versus female files).


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


## Our research question {#hypothesis1-question}

We return to the sex discrimination experiment from the last chapter. We are interested in finding out if there is an association between the recommendation to promote a candidate for branch manager and the gender listed on the file being evaluated by the male bank manager.


## Hypothesis testing {#hypothesis1-hypothesis-testing}

The approach we used in Part 2 of "Introduction to randomization" was to assume that the two variables `decision` and `sex` were independent. From that assumption, we were able to compare the observed difference in promotion percentages between males and females from the actual data to the distribution of random values obtained by randomization. When the observed difference was far enough away from zero, we concluded that the assumption of independence was probably false, giving us evidence that the two variables were associated after all.

This logic is formalized into a sequence of steps known as a *hypothesis test*. In this section, we will introduce a rubric for conducting a full and complete hypothesis test for the sex discrimination example. (This rubric also appears in the [Appendix](#appendix-rubric). If you need the rubric as a file, you can also download copies either as an `.Rmd` file [here](https://vectorposse.github.io/intro_stats/chapter_downloads/rubric_for_inference.Rmd) or as an `.nb.html` file [here](https://vectorposse.github.io/intro_stats/chapter_downloads/rubric_for_inference.nb.html).)

A hypothesis test can be organized into five parts:

1. Exploratory data analysis
2. Hypotheses
3. Model
4. Mechanics
5. Conclusion

Below, I'll address each of these steps.

### Exploratory data analysis {#hypothesis1-eda}

Before we can answer questions using data, we need to understand our data.

Most data sets come with some information about the provenance and structure of the data. (Often this is called "metadata".) Data provenance is the story of how the data was collected and for what purpose. Together with some information about the types of variables recorded, this is the who, what, when, where, why, and how. Without context, data is just a bunch of letters and numbers. You must understand the nature of the data in order to use the data. Information about the structure of the data is often recorded in a "code book".

For data that you collect yourself, you'll already know all about it, although should probably write that stuff down in case other people want to use your data (or in case "future you" wants to use the data). For other data sets, you hope that other people have recorded information about how the data was collected and what is described in the data. When working with data sets in R as we do for these chapters, we've already seen that there are help files---sometimes more or less helpful. In some cases, you'll need to go beyond the brief explanations in the help file to investigate the data provenance. And for files we download from other places on the internet, we may have a lot of work to do.

##### Exercise 1 {-}

What are some ethical issues you might want to consider when looking into the provenance of data? Have a discussion with a classmate and/or do some internet sleuthing to see if you can identify one or two key issues that should be considered before you access or analyze data.

::: {.answer}

Please write up your answer here.

:::


For exploring the raw data in front of us, we can use commands like `View` from the Console to see the data in spreadsheet form, although if we're using R Notebooks, we can just type the name of the data frame in a code chunk and run it to print the data in a form we can navigate and explore. There is also `glimpse` to explore the structure of the data (the variables and how they're coded), as well as other summary functions to get a quick sense of the variables.

Sometimes you have to prepare your data for analysis. A common example is converting categorical variables that should be coded as factor variables, but often are coded as character vectors, or are coded numerically (like "1" and "0" instead of "Yes" and "No"). Sometimes missing data is coded unusually (like "999") and that has to be fixed before trying to calculate statistics. "Cleaning" data is often a task that takes more time than analyzing it!

Finally, once the data is in a suitably tidy form, we can use visualizations like tables, graphs, and charts to understand the data better. Often, there are conditions about the shape of our data that have to be met before inference is appropriate, and this step can help diagnose problems that could arise in the inferential procedure. This is a good time to look for outliers, for example.

### Hypotheses {#hypothesis1-hypotheses}

We are trying to ask some question about a population of interest. However, all we have in our data is a sample of that population. The word inference comes from the verb "infer": we are trying to infer what might be true of a population just from examining a sample. It's also possible that our question involves comparing two or more populations to each other. In this case, we'll have multiple samples, one from each of our populations. For example, in our sex discrimination example, we are comparing two populations: male bank managers who consider male files for promotion, and male bank managers who consider female files for promotion. Our data gives us two samples who form only a part of the larger populations of interest. 

To convince our audience that our analysis is correct, it makes sense to take a skeptical position. If we are trying to prove that there is an association between promotion and sex, we don't just declare it to be so. We start with a "null hypothesis", or an expression of the belief that there is no association. A null hypothesis always represents the "default" position that a skeptic might take. It codifies the idea that "there's nothing to see here."

Our job is to gather evidence to show that there is something interesting going on. The statement of interest to us is called the "alternative hypothesis". This is usually the thing we're trying to prove related to our research question.

We can perform *one-sided* tests or *two-sided* tests. A one-sided test is when we have a specific direction in mind for the effect. For example, if we are trying to prove that male files are *more* likely to be promoted than female files, then we would perform a one-sided test. On the other hand, if we only care about proving an association, then male files could be either more likely or less likely to be promoted than female files. (This is contrasted to the null that states that male files are *equally* likely to be promoted as female files.) If it seems weird to run a two-sided test, keep in mind that we want to give our statistical analysis a chance to prove an association regardless of the direction of the association. Wouldn't you be interested to know if it turned out that male files are, in fact, *less* likely to be promoted?

You can't cheat and look at the data first. In a normal research study out there in the real world, you develop hypotheses long before you collect data. So you have to decide to do a one-sided or two-sided test before you have the luxury of seeing your data pointing in one direction or the other.

Running a two-sided test is often a good default option. Again, this is because our analysis will allow us to show interesting effects in any direction.

We typically express hypotheses in two ways. First, we write down full sentences that express in the context of the problem what our null and alternative hypotheses are stating. Then, we express the same ideas as mathematical statements. This translation from words to math is important as it gives us the connection to the quantitative statistical analysis we need to perform. The null hypothesis will always be that some quantity is equal to (=) the null value. The alternative hypothesis depends on whether we are conducting a one-sided test or a two-sided test. A one-sided test is mathematically saying that the quantity of interest is either greater than (>) or less than (<) the null value. A two-sided test always states that the quantity of interest is not equal to ($\neq$) the null value. (Notice the math symbol enclosed in dollar signs in the previous sentence. In the HTML file, these symbols will appear correctly. In the R Notebook, you can hover the cursor anywhere between the dollar signs and the math symbol will show up. Alternatively, you can click somewhere between the dollar signs and hit Ctrl-Enter or Cmd-Enter, just like with inline R code.)

The most important thing to know is that the entire hypothesis test up until you reach the conclusion is conducted **under the assumption that the null hypothesis is true**. In other words, we pretend the whole time that our alternative hypothesis is false, and we carry out our analysis working under that assumption. This may seem odd, but it makes sense when you remember that the goal of inference is to try to convince a skeptic. Others will only believe your claim after you present evidence that suggests that the data is inconsistent with the claims made in the null.

### Model {#hypothesis1-model}

A model is an approximation---usually a simplification---of reality. In a hypothesis test, when we say "model" we are talking specifically about the "null model". In other words, what is true about the population under the assumption of the null? If we sample from the population repeatedly, we find that there is some kind of distribution of values that can occur by pure chance alone. This is called the *sampling distribution model*. We have been learning about how to use randomization to understand the sampling distribution and how much sampling variability to expect, even when the null hypothesis is true.

Building a model is contingent upon certain assumptions being true. We cannot usually demonstrate directly that these assumptions are conclusively met; however, there are often conditions that can be checked with our data that can give us some confidence in saying that the assumptions are probably met. For example, there is no hope that we can infer anything from our sample unless that sample is close to a random sample of the population. There is rarely any direct evidence of having a properly random sample, and often, random samples are too much to ask for. There is almost never such a thing as a truly random sample of the population. Nevertheless, it is up to us to make the case that our sample is as representative of the population as possible. Additionally, we have to know that our sample comprises less than 10% of the size of the population. The reasons for this are somewhat technical and the 10% figure is just a rough guideline, but we should think carefully about this whenever we want our inference to be correct.

Those are just two examples. For the randomization tests we are running, those are the only two conditions we need to check. For other hypothesis tests in the future that use different types of models, we will need to check more conditions that correspond to the modeling assumptions we will need to make.

### Mechanics {#hypothesis1-mechanics}

This is the nitty-gritty, nuts-and-bolts part of a hypothesis test. Once we have a model that tells us how data should behave under the assumption of the null hypothesis, we need to check how our data actually behaved. The measure of where our data is relative to the null model is called the *test statistic*. For example, if the null hypothesis states that there should be a difference of zero between promotion rates for males and females, then the test statistic would be the actual observed difference in our data between males and females.

Once we have a test statistic, we can plot it in the same graph as the null model. This gives us a visual sense of how rare or unusual our observed data is. The further our test statistic is from the center of the null model, the more evidence we have that our data would be very unusual if the null model were true. And that, in turn, gives us a reason not to believe the null model. When conducting a two-sided test, we will actually graph locations on both side of the null value: the test statistic on one side of the null value and a point the same distance on the other side of the null value. This will acknowledge that we're interested in evidence of an effect in either direction. 

Finally, we convert the visual evidence explained in the previous paragraph to a number called a *P-value*. This measures how likely it is to see our observed data---or data even more extreme---under the assumption of the null. A small P-value, then, means that if the null were really true, we wouldn't be very likely at all to see data like ours. That leaves us with little confidence that the null model is really true. (After all, we *did* see the data we gathered!) If the P-value is large---in other words, if the test statistic is closer to the middle of the null distribution---then our data is perfectly consistent with the null hypothesis. That doesn't mean the null is true, but it certainly does not give us evidence against the null.

A one-sided test will give us a P-value that only counts data more extreme than the observed data in the direction that we explicitly hypothesized. For example, if our alternative hypothesis was that male files are more likely to be promoted, then we would only look at the part of the model that showed differences with as many or more male promotions as our data showed. A two-sided P-value, by contrast, will count data that is extreme in either direction. This will include values on both sides of the distribution, which is why it's called a two-sided test. Computationally, it is usually easiest to calculate the one-sided P-value and just double it.^[This is not technically the most mathematically appropriate thing to do, but it's a reasonable approximation in many common situations.]

Remember the statement made earlier that throughout the hypothesis testing process, **we work under the assumption that the null hypothesis is true**. The P-value is no exception. It tells us **under the assumption of the null** how likely we are to to see data at least as extreme (if not even more extreme) as the data we actually saw.

### Conclusion {#hypothesis1-ht-conclusion}

The P-value we calculate in the Mechanics section allows us to determine what our decision will be relative to the null hypothesis. As explained above, when the P-value is small, that means we had data that would be very unlikely had the null been true. The sensible conclusion is then to "reject the null hypothesis." On the other hand, if the data is consistent with the null hypothesis, then we "fail to reject the null hypothesis."

How small does the P-value need to be before we are willing to reject the null hypothesis? That is a decision we have to make based on how much we are willing to risk an incorrect conclusion. A value that is widely used is 0.05; in other words, if $P < 0.05$ we reject the null, and if $P > 0.05$, we fail to reject the null. However, for situations where we want to be conservative, we could choose this threshold to be much smaller. If we insist that the P-value be less than 0.01, for example, then we will only reject the null when we have a lot more evidence. The threshold we choose is called the "significance level", denoted by the Greek letter alpha: $\alpha$. The value of $\alpha$ must be chosen long before we compute our P-value so that we're not tempted to cheat and change the value of $\alpha$ to suit our P-value (and by doing so, quite literally, move the goalposts).

**Note that we never accept the null hypothesis.** The hypothesis testing procedure gives us no evidence in favor of the null. All we can say is that the evidence is either strong enough to warrant rejection of the null, or else it isn't, in which case we can conclude nothing. If we can't prove the null false, we are left not knowing much of anything at all.

The phrases "reject the null" or "fail to reject the null" are very statsy. Your audience may not be statistically trained. Besides, the *real* conclusion you care about concerns the research question of interest you posed at the beginning of this process, and that is built into the alternative hypothesis, not the null. Therefore, we need some statement that addresses the alternative hypothesis in words that a general audience will understand. I recommend the following templates:

- When you reject the null, you can safely say, "We have sufficient evidence that [restate the alternative hypothesis]."
- When you fail to reject the null, you can safely say, "We have insufficient evidence that [restate the alternative hypothesis]."

The last part of your conclusion should be an acknowledgement of the uncertainty in this process. Statistics tries to tame randomness, but in the end, randomness is always somewhat unpredictable. It is possible that we came to the wrong conclusion, not because we made mistakes in our computation, but because statistics just can't be right 100% of the time when randomness is involved. Therefore, we need to explain to our audience that we may have made an error.

A *Type I* error is what happens when the null hypothesis is actually true, but our procedure rejects it anyway. This happens when we get an unrepresentative extreme sample for some reason. For example, perhaps there really is no association between promotion and sex. Even if that were true, we could accidentally survey a group of bank managers who---by pure chance alone---happen to recommend promotion more often for the male files. Our test statistic will be "accidentally" far from the null value, and we will mistakenly reject the null. Whenever we reject the null, we are at risk of making a Type I error. Given that we are conclusively stating a statistically significant finding, if that finding is wrong, this is a *false positive*, a term that is synonymous with a Type I error. The significance level $\alpha$ discussed above is, in fact, the probability of making a Type I error. (If the null is true, we will still reject the null if our P-value happens to be less than $\alpha$.)

On the other hand, the null may actually be false, and yet, we may not manage to gather enough evidence to disprove it. This can also happen due to an unusual sample---a sample that doesn't conform to the "truth". But there are other ways this can happen as well, most commonly when you have a small sample size (which doesn't allow you to prove much of anything at all) or when the effect you're trying to measure exists, but is so small that it is hard to distinguish from no effect at all (which is what the null postulates). In these cases, we are at risk of making a *Type II* error. Anytime we say that we fail to reject the null, we have to worry about the possibility of making a Type II error, also called a *false negative*.


## Example {#hypothesis1-example}

Below, we'll model the process of walking through a complete hypothesis test, showing how we would address each step. Then, you'll have a turn at doing the same thing for a different question. Unless otherwise stated, we will always assume a significance level of $\alpha = 0.05$. (In other words, we will reject the null if our computed P-value is less than 0.05, and we will fail to reject the null if our P-value is greater than or equal to 0.05.)

Note that there is some mathematical formatting. As mentioned before, this is done by enclosing such math in dollar signs. Don't worry too much about the syntax; just mimic what you see in the example.


## Exploratory data analysis {#hypothesis1-ex-eda}

### Use data documentation (help files, code books, Google, etc.) to determine as much as possible about the data provenance and structure. {#hypothesis1-ex-documentation}

You can look at the help file by typing `?sex_discrimination` at the Console. (However, do not put that command here in a code chunk. The R Notebook has no way of displaying a help file when it's processed.) You can also type that into the Help tab in the lower-right panel in RStudio.

The help file doesn't say too much, but there is a "Source" at the bottom. We can do an internet search for "Rosen Jerdee Influence of sex role stereotypes on personnel decisions". As many academics articles on the internet are, this one is pay-walled, so we can't read it for free. If you go to school or work for an institution with a library, though, you may be able to access articles through your library services. Talk to a librarian if you'd like to access research articles. As long as you have the citation details, librarians can often track down articles, and many are already accessible through library databases.

In this case, we can read the abstract for free. This tells us that the data we have is only one part of a larger set of experiments done.

This is also the place to comment on any ethical concerns you may have. For example, how was the data collected? Did the researchers follow ethical guidelines in the treatment of their subjects, like obtaining consent? Without accessing the full article, it's hard to know in this case. But do your best in each data analysis task you have to try to find out as much as possible about the data.

In this section, we'll also print the data set and use `glimpse` to summarize the variables.


```r
sex_discrimination
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["sex"],"name":[1],"type":["fct"],"align":["left"]},{"label":["decision"],"name":[2],"type":["fct"],"align":["left"]}],"data":[{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"not promoted"},{"1":"male","2":"not promoted"},{"1":"male","2":"not promoted"},{"1":"female","2":"promoted"},{"1":"female","2":"promoted"},{"1":"female","2":"promoted"},{"1":"female","2":"promoted"},{"1":"female","2":"promoted"},{"1":"female","2":"promoted"},{"1":"female","2":"promoted"},{"1":"female","2":"promoted"},{"1":"female","2":"promoted"},{"1":"female","2":"promoted"},{"1":"female","2":"promoted"},{"1":"female","2":"promoted"},{"1":"female","2":"promoted"},{"1":"female","2":"promoted"},{"1":"female","2":"not promoted"},{"1":"female","2":"not promoted"},{"1":"female","2":"not promoted"},{"1":"female","2":"not promoted"},{"1":"female","2":"not promoted"},{"1":"female","2":"not promoted"},{"1":"female","2":"not promoted"},{"1":"female","2":"not promoted"},{"1":"female","2":"not promoted"},{"1":"female","2":"not promoted"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


```r
glimpse(sex_discrimination)
```

```
## Rows: 48
## Columns: 2
## $ sex      <fct> male, male, male, male, male, male, male, male, male, male, m…
## $ decision <fct> promoted, promoted, promoted, promoted, promoted, promoted, p…
```

### Prepare the data for analysis. {#hypothesis1-ex-prepare}

In this section, we do any tasks required to clean the data. This will often involve using `mutate`, either to convert other variable types to factors, or compute additional variables using existing columns. It may involve using `filter` to analyze only one part of the data we care about.

If there is missing data, this is the place to identify it and decide if you need to address it before starting your analysis. It's always important to check for missing data. It's not always necessary to address it now as many of the R functions we use will ignore rows with missing data.

The easiest way to detect missing data is to try deleting rows that are missing some data with `drop_na` and see if the number of rows changes:


```r
sex_discrimination %>%
  drop_na()
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["sex"],"name":[1],"type":["fct"],"align":["left"]},{"label":["decision"],"name":[2],"type":["fct"],"align":["left"]}],"data":[{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"promoted"},{"1":"male","2":"not promoted"},{"1":"male","2":"not promoted"},{"1":"male","2":"not promoted"},{"1":"female","2":"promoted"},{"1":"female","2":"promoted"},{"1":"female","2":"promoted"},{"1":"female","2":"promoted"},{"1":"female","2":"promoted"},{"1":"female","2":"promoted"},{"1":"female","2":"promoted"},{"1":"female","2":"promoted"},{"1":"female","2":"promoted"},{"1":"female","2":"promoted"},{"1":"female","2":"promoted"},{"1":"female","2":"promoted"},{"1":"female","2":"promoted"},{"1":"female","2":"promoted"},{"1":"female","2":"not promoted"},{"1":"female","2":"not promoted"},{"1":"female","2":"not promoted"},{"1":"female","2":"not promoted"},{"1":"female","2":"not promoted"},{"1":"female","2":"not promoted"},{"1":"female","2":"not promoted"},{"1":"female","2":"not promoted"},{"1":"female","2":"not promoted"},{"1":"female","2":"not promoted"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

Since the result still has 48 rows, there are no missing values.

The `sex_discimination` data is already squeaky clean, so we don't need to do anything here.

### Make tables or plots to explore the data visually. {#hypothesis1-ex-plots}

As we have two categorical variables, a contingency table is a good way of visualizing the distribution of both variables together. (Don't forget to include the marginal distribution and create two tables: one with counts and one with percentages!)


```r
tabyl(sex_discrimination, decision, sex) %>%
  adorn_totals()
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["decision"],"name":[1],"type":["fct"],"align":["left"]},{"label":["male"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["female"],"name":[3],"type":["dbl"],"align":["right"]}],"data":[{"1":"promoted","2":"21","3":"14","_rn_":"1"},{"1":"not promoted","2":"3","3":"10","_rn_":"2"},{"1":"Total","2":"24","3":"24","_rn_":"3"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


```r
tabyl(sex_discrimination, decision, sex) %>%
  adorn_totals() %>%
  adorn_percentages("col") %>%
  adorn_pct_formatting()
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["decision"],"name":[1],"type":["fct"],"align":["left"]},{"label":["male"],"name":[2],"type":["chr"],"align":["left"]},{"label":["female"],"name":[3],"type":["chr"],"align":["left"]}],"data":[{"1":"promoted","2":"87.5%","3":"58.3%","_rn_":"1"},{"1":"not promoted","2":"12.5%","3":"41.7%","_rn_":"2"},{"1":"Total","2":"100.0%","3":"100.0%","_rn_":"3"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


## Hypotheses {#hypothesis1-ex-hypotheses}

### Identify the sample (or samples) and a reasonable population (or populations) of interest. {#hypothesis1-ex-sample-pop}

There are technically two samples of interest here. All the data comes from a group of 48 bank managers recruited for the study, but one group of interest are bank managers who are evaluating male files, and the other group of interest are bank managers who are evaluating female files.

One of the contingency tables above shows the sample sizes for each group in the marginal distribution along the bottom of the table (i.e., the column sums). There are 24 mangers with male files and 24 managers with female files.

The populations of interest are probably all bank managers evaluating male candidates and all bank managers evaluating female candidates, probably only in in the U.S. (where the two researchers were based) and only during the 1970s.

### Express the null and alternative hypotheses as contextually meaningful full sentences. {#hypothesis1-ex-express-words}

(Note: The null hypothesis is indicated by the symbol $H_{0}$, often pronounced "H naught" or "H sub zero." The alternative hypothesis is indicated by $H_{A}$, pronounced "H sub A.")

$H_{0}:$ There is no association between decision and sex in hiring branch managers for banks in the 1970s.

$H_{A}:$ There is an association between decision and sex in hiring branch managers for banks in the 1970s.

### Express the null and alternative hypotheses in symbols (when possible). {#hypothesis1-ex-express-math}

$H_{0}: p_{promoted, male} - p_{promoted, female} = 0$

$H_{A}: p_{promoted, male} - p_{promoted, female} \neq 0$

Note: First, pay attention to the "success" condition (in this case, "promoted"). We could choose to measure either those promoted or those not promoted. The difference will be positive for one and negative for the other, so it really doesn't matter which one we choose. Just make a choice and be consistent. Also pay close attention here to the order of the subtraction. Again, while it doesn't matter conceptually, we need to make sure that the code we include later agrees with this order. 


## Model {#hypothesis1-ex-model}

### Identify the sampling distribution model. {#hypothesis1-ex-sampling-dist-model}

We will randomize to simulate the sampling distribution.

### Check the relevant conditions to ensure that model assumptions are met. {#hypothesis1-ex-ht-conditions}

- Random (for both groups)
  - We have no evidence that these are random samples of bank managers. We hope that they are representative. If the populations of interest are all bank managers in the U.S. evaluating either male candidates or female candidates, then we have some doubts as to how representative these samples are. It is likely that the bank managers were recruited from limited geographic areas based on the location of the researchers, and we know that geography could easily be a confounder for sex discrimination (because some areas of the country might be more prone to it than others). Despite our misgivings, we will proceed on with the analysis, but we will temper our expectations for grand, sweeping conclusions.

- 10% (for both groups)
  - Regardless of the intended populations, 24 bank managers evaluating male files and 24 bank managers evaluating female files are surely less than 10% of all bank managers under consideration.


## Mechanics {#hypothesis1-ex-mechanics}

### Compute the test statistic. {#hypothesis1-ex-compute-test-stat}

We let `infer` do the work here:


```r
obs_diff <- sex_discrimination %>%
    observe(decision ~ sex, success = "promoted",
            stat = "diff in props", order = c("male", "female"))
obs_diff
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["stat"],"name":[1],"type":["dbl"],"align":["right"]}],"data":[{"1":"0.2916667"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


Note: `obs_diff` is a tibble, albeit a small one, having only one column and one row. That tibble is what we need to feed into the visualization later. However, for reporting the value by itself, we have to pull it out of the tibble. We will do this below using the `pull` function. See the inline code in the next subsection.

### Report the test statistic in context (when possible). {#hypothesis1-ex-report-test-stat}

The observed difference in the proportion of promotion recommendations for male files versus female files is 0.2916667 (subtracting males minus females). Or, another way to say this: there is a 29.1666667% difference in the promotion rates between male files and female files.

### Plot the null distribution. {#hypothesis1-ex-plot-null}

Note: In this section, we will use the series of verbs from `infer` to generate all the information we need about the hypothesis test. We call that output `decision_sex_test` here, but you'll want to change it to another name for a different test. The recommended pattern is `response_predictor_test`.

Don't forget to set the seed. We are using randomization to permute the values of the predictor variable in order to break any association that might exist in the data. This will allow us to explore the sampling distribution created under the assumption of the null hypothesis.

When you get to the `visualize` step, leave the number of bins out. (Just type `visualize()` with empty parentheses.) If you determine that the default binning is not optimal, you can add back `bins` and experiment with the number. We know from the previous chapter that 9 bins is good here.


```r
set.seed(9999)
decision_sex_test <- sex_discrimination %>%
    specify(decision ~ sex, success = "promoted") %>%
    hypothesize(null = "independence") %>%
    generate(reps = 1000, type = "permute") %>%
    calculate(stat = "diff in props", order = c("male", "female"))
decision_sex_test
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["replicate"],"name":[1],"type":["int"],"align":["right"]},{"label":["stat"],"name":[2],"type":["dbl"],"align":["right"]}],"data":[{"1":"1","2":"-0.04166667"},{"1":"2","2":"0.20833333"},{"1":"3","2":"0.04166667"},{"1":"4","2":"-0.12500000"},{"1":"5","2":"-0.04166667"},{"1":"6","2":"-0.20833333"},{"1":"7","2":"-0.20833333"},{"1":"8","2":"0.04166667"},{"1":"9","2":"-0.29166667"},{"1":"10","2":"0.12500000"},{"1":"11","2":"0.12500000"},{"1":"12","2":"0.29166667"},{"1":"13","2":"0.04166667"},{"1":"14","2":"-0.04166667"},{"1":"15","2":"-0.04166667"},{"1":"16","2":"0.04166667"},{"1":"17","2":"0.12500000"},{"1":"18","2":"0.04166667"},{"1":"19","2":"-0.04166667"},{"1":"20","2":"-0.04166667"},{"1":"21","2":"-0.12500000"},{"1":"22","2":"-0.04166667"},{"1":"23","2":"0.04166667"},{"1":"24","2":"-0.12500000"},{"1":"25","2":"0.12500000"},{"1":"26","2":"0.29166667"},{"1":"27","2":"-0.20833333"},{"1":"28","2":"0.12500000"},{"1":"29","2":"0.04166667"},{"1":"30","2":"0.04166667"},{"1":"31","2":"0.04166667"},{"1":"32","2":"0.12500000"},{"1":"33","2":"0.04166667"},{"1":"34","2":"0.04166667"},{"1":"35","2":"-0.12500000"},{"1":"36","2":"0.04166667"},{"1":"37","2":"0.04166667"},{"1":"38","2":"-0.04166667"},{"1":"39","2":"-0.12500000"},{"1":"40","2":"-0.12500000"},{"1":"41","2":"-0.04166667"},{"1":"42","2":"-0.04166667"},{"1":"43","2":"0.04166667"},{"1":"44","2":"-0.04166667"},{"1":"45","2":"-0.04166667"},{"1":"46","2":"-0.29166667"},{"1":"47","2":"0.12500000"},{"1":"48","2":"-0.04166667"},{"1":"49","2":"-0.12500000"},{"1":"50","2":"0.12500000"},{"1":"51","2":"-0.04166667"},{"1":"52","2":"-0.12500000"},{"1":"53","2":"0.20833333"},{"1":"54","2":"0.29166667"},{"1":"55","2":"-0.04166667"},{"1":"56","2":"0.04166667"},{"1":"57","2":"0.20833333"},{"1":"58","2":"0.12500000"},{"1":"59","2":"-0.12500000"},{"1":"60","2":"0.04166667"},{"1":"61","2":"0.04166667"},{"1":"62","2":"0.04166667"},{"1":"63","2":"-0.20833333"},{"1":"64","2":"-0.04166667"},{"1":"65","2":"0.12500000"},{"1":"66","2":"0.29166667"},{"1":"67","2":"-0.12500000"},{"1":"68","2":"-0.20833333"},{"1":"69","2":"0.04166667"},{"1":"70","2":"0.12500000"},{"1":"71","2":"-0.12500000"},{"1":"72","2":"0.12500000"},{"1":"73","2":"0.12500000"},{"1":"74","2":"0.20833333"},{"1":"75","2":"0.20833333"},{"1":"76","2":"0.04166667"},{"1":"77","2":"0.12500000"},{"1":"78","2":"0.04166667"},{"1":"79","2":"0.20833333"},{"1":"80","2":"-0.04166667"},{"1":"81","2":"0.12500000"},{"1":"82","2":"0.12500000"},{"1":"83","2":"0.29166667"},{"1":"84","2":"0.12500000"},{"1":"85","2":"-0.12500000"},{"1":"86","2":"0.04166667"},{"1":"87","2":"0.12500000"},{"1":"88","2":"-0.04166667"},{"1":"89","2":"-0.12500000"},{"1":"90","2":"-0.04166667"},{"1":"91","2":"-0.04166667"},{"1":"92","2":"-0.04166667"},{"1":"93","2":"-0.04166667"},{"1":"94","2":"-0.20833333"},{"1":"95","2":"0.04166667"},{"1":"96","2":"-0.04166667"},{"1":"97","2":"0.12500000"},{"1":"98","2":"0.12500000"},{"1":"99","2":"0.04166667"},{"1":"100","2":"-0.04166667"},{"1":"101","2":"0.04166667"},{"1":"102","2":"0.12500000"},{"1":"103","2":"-0.04166667"},{"1":"104","2":"-0.12500000"},{"1":"105","2":"0.20833333"},{"1":"106","2":"0.04166667"},{"1":"107","2":"0.12500000"},{"1":"108","2":"0.04166667"},{"1":"109","2":"-0.12500000"},{"1":"110","2":"-0.04166667"},{"1":"111","2":"0.04166667"},{"1":"112","2":"0.20833333"},{"1":"113","2":"0.04166667"},{"1":"114","2":"0.12500000"},{"1":"115","2":"0.12500000"},{"1":"116","2":"0.12500000"},{"1":"117","2":"0.12500000"},{"1":"118","2":"0.12500000"},{"1":"119","2":"-0.04166667"},{"1":"120","2":"-0.04166667"},{"1":"121","2":"0.12500000"},{"1":"122","2":"0.04166667"},{"1":"123","2":"0.12500000"},{"1":"124","2":"-0.12500000"},{"1":"125","2":"0.04166667"},{"1":"126","2":"-0.29166667"},{"1":"127","2":"0.12500000"},{"1":"128","2":"-0.04166667"},{"1":"129","2":"0.20833333"},{"1":"130","2":"-0.04166667"},{"1":"131","2":"0.29166667"},{"1":"132","2":"-0.04166667"},{"1":"133","2":"-0.12500000"},{"1":"134","2":"0.04166667"},{"1":"135","2":"-0.04166667"},{"1":"136","2":"-0.20833333"},{"1":"137","2":"0.04166667"},{"1":"138","2":"0.12500000"},{"1":"139","2":"0.20833333"},{"1":"140","2":"0.20833333"},{"1":"141","2":"-0.12500000"},{"1":"142","2":"0.12500000"},{"1":"143","2":"0.20833333"},{"1":"144","2":"-0.04166667"},{"1":"145","2":"0.12500000"},{"1":"146","2":"0.12500000"},{"1":"147","2":"-0.04166667"},{"1":"148","2":"0.12500000"},{"1":"149","2":"0.12500000"},{"1":"150","2":"-0.20833333"},{"1":"151","2":"0.12500000"},{"1":"152","2":"-0.04166667"},{"1":"153","2":"-0.04166667"},{"1":"154","2":"-0.12500000"},{"1":"155","2":"-0.04166667"},{"1":"156","2":"-0.04166667"},{"1":"157","2":"-0.20833333"},{"1":"158","2":"0.12500000"},{"1":"159","2":"0.12500000"},{"1":"160","2":"-0.12500000"},{"1":"161","2":"0.12500000"},{"1":"162","2":"0.04166667"},{"1":"163","2":"-0.04166667"},{"1":"164","2":"0.12500000"},{"1":"165","2":"0.04166667"},{"1":"166","2":"0.04166667"},{"1":"167","2":"0.12500000"},{"1":"168","2":"0.04166667"},{"1":"169","2":"-0.20833333"},{"1":"170","2":"0.12500000"},{"1":"171","2":"-0.12500000"},{"1":"172","2":"0.04166667"},{"1":"173","2":"0.20833333"},{"1":"174","2":"-0.20833333"},{"1":"175","2":"-0.20833333"},{"1":"176","2":"0.04166667"},{"1":"177","2":"-0.12500000"},{"1":"178","2":"-0.12500000"},{"1":"179","2":"0.12500000"},{"1":"180","2":"0.12500000"},{"1":"181","2":"-0.04166667"},{"1":"182","2":"0.12500000"},{"1":"183","2":"-0.04166667"},{"1":"184","2":"-0.04166667"},{"1":"185","2":"-0.04166667"},{"1":"186","2":"-0.04166667"},{"1":"187","2":"-0.04166667"},{"1":"188","2":"0.04166667"},{"1":"189","2":"-0.04166667"},{"1":"190","2":"0.04166667"},{"1":"191","2":"-0.12500000"},{"1":"192","2":"-0.04166667"},{"1":"193","2":"0.04166667"},{"1":"194","2":"-0.04166667"},{"1":"195","2":"-0.12500000"},{"1":"196","2":"0.04166667"},{"1":"197","2":"-0.12500000"},{"1":"198","2":"-0.12500000"},{"1":"199","2":"0.04166667"},{"1":"200","2":"0.04166667"},{"1":"201","2":"-0.04166667"},{"1":"202","2":"0.04166667"},{"1":"203","2":"-0.12500000"},{"1":"204","2":"-0.04166667"},{"1":"205","2":"0.12500000"},{"1":"206","2":"-0.12500000"},{"1":"207","2":"-0.12500000"},{"1":"208","2":"0.04166667"},{"1":"209","2":"-0.04166667"},{"1":"210","2":"0.20833333"},{"1":"211","2":"-0.04166667"},{"1":"212","2":"0.04166667"},{"1":"213","2":"0.12500000"},{"1":"214","2":"-0.04166667"},{"1":"215","2":"-0.12500000"},{"1":"216","2":"-0.12500000"},{"1":"217","2":"0.04166667"},{"1":"218","2":"-0.04166667"},{"1":"219","2":"0.04166667"},{"1":"220","2":"-0.12500000"},{"1":"221","2":"-0.04166667"},{"1":"222","2":"-0.04166667"},{"1":"223","2":"0.12500000"},{"1":"224","2":"-0.04166667"},{"1":"225","2":"0.12500000"},{"1":"226","2":"-0.04166667"},{"1":"227","2":"-0.12500000"},{"1":"228","2":"-0.04166667"},{"1":"229","2":"0.12500000"},{"1":"230","2":"0.20833333"},{"1":"231","2":"0.29166667"},{"1":"232","2":"0.12500000"},{"1":"233","2":"0.04166667"},{"1":"234","2":"-0.04166667"},{"1":"235","2":"-0.04166667"},{"1":"236","2":"0.12500000"},{"1":"237","2":"0.04166667"},{"1":"238","2":"0.12500000"},{"1":"239","2":"0.04166667"},{"1":"240","2":"-0.04166667"},{"1":"241","2":"-0.12500000"},{"1":"242","2":"-0.04166667"},{"1":"243","2":"0.04166667"},{"1":"244","2":"-0.04166667"},{"1":"245","2":"-0.12500000"},{"1":"246","2":"0.12500000"},{"1":"247","2":"-0.12500000"},{"1":"248","2":"0.04166667"},{"1":"249","2":"0.04166667"},{"1":"250","2":"-0.04166667"},{"1":"251","2":"0.04166667"},{"1":"252","2":"0.29166667"},{"1":"253","2":"0.04166667"},{"1":"254","2":"0.04166667"},{"1":"255","2":"-0.04166667"},{"1":"256","2":"0.12500000"},{"1":"257","2":"0.04166667"},{"1":"258","2":"0.12500000"},{"1":"259","2":"0.20833333"},{"1":"260","2":"-0.12500000"},{"1":"261","2":"0.12500000"},{"1":"262","2":"-0.04166667"},{"1":"263","2":"-0.04166667"},{"1":"264","2":"0.12500000"},{"1":"265","2":"0.04166667"},{"1":"266","2":"-0.20833333"},{"1":"267","2":"-0.12500000"},{"1":"268","2":"-0.04166667"},{"1":"269","2":"0.12500000"},{"1":"270","2":"-0.20833333"},{"1":"271","2":"0.29166667"},{"1":"272","2":"-0.04166667"},{"1":"273","2":"-0.04166667"},{"1":"274","2":"0.12500000"},{"1":"275","2":"0.04166667"},{"1":"276","2":"-0.20833333"},{"1":"277","2":"-0.04166667"},{"1":"278","2":"-0.12500000"},{"1":"279","2":"0.04166667"},{"1":"280","2":"-0.12500000"},{"1":"281","2":"0.04166667"},{"1":"282","2":"-0.29166667"},{"1":"283","2":"-0.04166667"},{"1":"284","2":"-0.12500000"},{"1":"285","2":"0.04166667"},{"1":"286","2":"0.04166667"},{"1":"287","2":"-0.04166667"},{"1":"288","2":"0.20833333"},{"1":"289","2":"-0.12500000"},{"1":"290","2":"0.04166667"},{"1":"291","2":"0.12500000"},{"1":"292","2":"0.29166667"},{"1":"293","2":"-0.04166667"},{"1":"294","2":"0.04166667"},{"1":"295","2":"0.04166667"},{"1":"296","2":"0.12500000"},{"1":"297","2":"0.04166667"},{"1":"298","2":"-0.04166667"},{"1":"299","2":"-0.04166667"},{"1":"300","2":"-0.04166667"},{"1":"301","2":"0.20833333"},{"1":"302","2":"0.04166667"},{"1":"303","2":"0.29166667"},{"1":"304","2":"0.12500000"},{"1":"305","2":"0.12500000"},{"1":"306","2":"-0.12500000"},{"1":"307","2":"-0.04166667"},{"1":"308","2":"-0.12500000"},{"1":"309","2":"0.12500000"},{"1":"310","2":"-0.04166667"},{"1":"311","2":"-0.12500000"},{"1":"312","2":"-0.04166667"},{"1":"313","2":"-0.04166667"},{"1":"314","2":"0.12500000"},{"1":"315","2":"0.12500000"},{"1":"316","2":"-0.04166667"},{"1":"317","2":"-0.04166667"},{"1":"318","2":"0.04166667"},{"1":"319","2":"-0.20833333"},{"1":"320","2":"-0.20833333"},{"1":"321","2":"-0.12500000"},{"1":"322","2":"0.04166667"},{"1":"323","2":"0.04166667"},{"1":"324","2":"0.20833333"},{"1":"325","2":"-0.12500000"},{"1":"326","2":"0.04166667"},{"1":"327","2":"0.12500000"},{"1":"328","2":"-0.20833333"},{"1":"329","2":"-0.20833333"},{"1":"330","2":"-0.12500000"},{"1":"331","2":"0.12500000"},{"1":"332","2":"-0.12500000"},{"1":"333","2":"0.04166667"},{"1":"334","2":"0.20833333"},{"1":"335","2":"-0.04166667"},{"1":"336","2":"-0.29166667"},{"1":"337","2":"0.12500000"},{"1":"338","2":"-0.04166667"},{"1":"339","2":"0.04166667"},{"1":"340","2":"0.12500000"},{"1":"341","2":"-0.04166667"},{"1":"342","2":"-0.12500000"},{"1":"343","2":"-0.29166667"},{"1":"344","2":"0.04166667"},{"1":"345","2":"-0.04166667"},{"1":"346","2":"0.04166667"},{"1":"347","2":"0.04166667"},{"1":"348","2":"-0.04166667"},{"1":"349","2":"0.12500000"},{"1":"350","2":"0.04166667"},{"1":"351","2":"0.04166667"},{"1":"352","2":"0.29166667"},{"1":"353","2":"-0.04166667"},{"1":"354","2":"0.04166667"},{"1":"355","2":"0.12500000"},{"1":"356","2":"0.12500000"},{"1":"357","2":"0.12500000"},{"1":"358","2":"-0.04166667"},{"1":"359","2":"-0.12500000"},{"1":"360","2":"-0.29166667"},{"1":"361","2":"0.20833333"},{"1":"362","2":"0.04166667"},{"1":"363","2":"0.20833333"},{"1":"364","2":"-0.29166667"},{"1":"365","2":"-0.12500000"},{"1":"366","2":"-0.04166667"},{"1":"367","2":"-0.12500000"},{"1":"368","2":"0.04166667"},{"1":"369","2":"0.04166667"},{"1":"370","2":"0.04166667"},{"1":"371","2":"-0.20833333"},{"1":"372","2":"0.20833333"},{"1":"373","2":"0.04166667"},{"1":"374","2":"-0.04166667"},{"1":"375","2":"0.04166667"},{"1":"376","2":"0.04166667"},{"1":"377","2":"0.04166667"},{"1":"378","2":"-0.04166667"},{"1":"379","2":"0.12500000"},{"1":"380","2":"0.12500000"},{"1":"381","2":"-0.20833333"},{"1":"382","2":"0.12500000"},{"1":"383","2":"-0.20833333"},{"1":"384","2":"-0.12500000"},{"1":"385","2":"-0.04166667"},{"1":"386","2":"-0.04166667"},{"1":"387","2":"-0.12500000"},{"1":"388","2":"-0.12500000"},{"1":"389","2":"0.20833333"},{"1":"390","2":"-0.20833333"},{"1":"391","2":"-0.04166667"},{"1":"392","2":"0.20833333"},{"1":"393","2":"-0.04166667"},{"1":"394","2":"0.04166667"},{"1":"395","2":"-0.04166667"},{"1":"396","2":"0.20833333"},{"1":"397","2":"-0.20833333"},{"1":"398","2":"-0.12500000"},{"1":"399","2":"0.20833333"},{"1":"400","2":"0.04166667"},{"1":"401","2":"-0.04166667"},{"1":"402","2":"-0.37500000"},{"1":"403","2":"0.20833333"},{"1":"404","2":"0.12500000"},{"1":"405","2":"0.20833333"},{"1":"406","2":"-0.04166667"},{"1":"407","2":"-0.04166667"},{"1":"408","2":"0.04166667"},{"1":"409","2":"-0.04166667"},{"1":"410","2":"0.04166667"},{"1":"411","2":"-0.04166667"},{"1":"412","2":"-0.12500000"},{"1":"413","2":"0.04166667"},{"1":"414","2":"0.12500000"},{"1":"415","2":"-0.12500000"},{"1":"416","2":"0.12500000"},{"1":"417","2":"0.04166667"},{"1":"418","2":"0.20833333"},{"1":"419","2":"0.12500000"},{"1":"420","2":"0.04166667"},{"1":"421","2":"0.04166667"},{"1":"422","2":"-0.20833333"},{"1":"423","2":"0.29166667"},{"1":"424","2":"-0.12500000"},{"1":"425","2":"0.12500000"},{"1":"426","2":"0.04166667"},{"1":"427","2":"-0.04166667"},{"1":"428","2":"0.04166667"},{"1":"429","2":"0.04166667"},{"1":"430","2":"-0.04166667"},{"1":"431","2":"-0.04166667"},{"1":"432","2":"0.20833333"},{"1":"433","2":"-0.12500000"},{"1":"434","2":"-0.12500000"},{"1":"435","2":"-0.04166667"},{"1":"436","2":"0.04166667"},{"1":"437","2":"0.04166667"},{"1":"438","2":"-0.20833333"},{"1":"439","2":"-0.12500000"},{"1":"440","2":"0.04166667"},{"1":"441","2":"0.12500000"},{"1":"442","2":"0.12500000"},{"1":"443","2":"-0.12500000"},{"1":"444","2":"0.04166667"},{"1":"445","2":"-0.04166667"},{"1":"446","2":"-0.12500000"},{"1":"447","2":"0.04166667"},{"1":"448","2":"-0.04166667"},{"1":"449","2":"-0.04166667"},{"1":"450","2":"0.04166667"},{"1":"451","2":"0.04166667"},{"1":"452","2":"-0.04166667"},{"1":"453","2":"0.20833333"},{"1":"454","2":"0.04166667"},{"1":"455","2":"0.04166667"},{"1":"456","2":"-0.12500000"},{"1":"457","2":"-0.04166667"},{"1":"458","2":"-0.12500000"},{"1":"459","2":"0.20833333"},{"1":"460","2":"0.20833333"},{"1":"461","2":"-0.04166667"},{"1":"462","2":"-0.12500000"},{"1":"463","2":"0.12500000"},{"1":"464","2":"-0.12500000"},{"1":"465","2":"0.12500000"},{"1":"466","2":"0.04166667"},{"1":"467","2":"-0.12500000"},{"1":"468","2":"-0.04166667"},{"1":"469","2":"0.04166667"},{"1":"470","2":"0.04166667"},{"1":"471","2":"-0.12500000"},{"1":"472","2":"-0.12500000"},{"1":"473","2":"0.12500000"},{"1":"474","2":"-0.12500000"},{"1":"475","2":"0.04166667"},{"1":"476","2":"0.12500000"},{"1":"477","2":"-0.29166667"},{"1":"478","2":"-0.04166667"},{"1":"479","2":"0.04166667"},{"1":"480","2":"-0.04166667"},{"1":"481","2":"-0.12500000"},{"1":"482","2":"0.04166667"},{"1":"483","2":"-0.12500000"},{"1":"484","2":"0.04166667"},{"1":"485","2":"-0.04166667"},{"1":"486","2":"-0.12500000"},{"1":"487","2":"0.20833333"},{"1":"488","2":"0.04166667"},{"1":"489","2":"0.04166667"},{"1":"490","2":"-0.04166667"},{"1":"491","2":"-0.04166667"},{"1":"492","2":"0.04166667"},{"1":"493","2":"-0.04166667"},{"1":"494","2":"0.20833333"},{"1":"495","2":"-0.04166667"},{"1":"496","2":"-0.04166667"},{"1":"497","2":"-0.20833333"},{"1":"498","2":"-0.12500000"},{"1":"499","2":"0.04166667"},{"1":"500","2":"0.04166667"},{"1":"501","2":"-0.04166667"},{"1":"502","2":"-0.12500000"},{"1":"503","2":"-0.04166667"},{"1":"504","2":"-0.20833333"},{"1":"505","2":"-0.04166667"},{"1":"506","2":"-0.12500000"},{"1":"507","2":"-0.04166667"},{"1":"508","2":"-0.04166667"},{"1":"509","2":"0.20833333"},{"1":"510","2":"-0.04166667"},{"1":"511","2":"0.37500000"},{"1":"512","2":"-0.04166667"},{"1":"513","2":"0.04166667"},{"1":"514","2":"0.12500000"},{"1":"515","2":"0.12500000"},{"1":"516","2":"0.04166667"},{"1":"517","2":"0.20833333"},{"1":"518","2":"0.20833333"},{"1":"519","2":"-0.20833333"},{"1":"520","2":"-0.04166667"},{"1":"521","2":"0.04166667"},{"1":"522","2":"-0.29166667"},{"1":"523","2":"-0.04166667"},{"1":"524","2":"-0.04166667"},{"1":"525","2":"0.04166667"},{"1":"526","2":"0.04166667"},{"1":"527","2":"0.20833333"},{"1":"528","2":"0.04166667"},{"1":"529","2":"0.12500000"},{"1":"530","2":"0.04166667"},{"1":"531","2":"-0.12500000"},{"1":"532","2":"-0.20833333"},{"1":"533","2":"0.12500000"},{"1":"534","2":"0.12500000"},{"1":"535","2":"0.12500000"},{"1":"536","2":"-0.04166667"},{"1":"537","2":"0.04166667"},{"1":"538","2":"0.04166667"},{"1":"539","2":"0.20833333"},{"1":"540","2":"-0.04166667"},{"1":"541","2":"-0.04166667"},{"1":"542","2":"-0.12500000"},{"1":"543","2":"0.04166667"},{"1":"544","2":"-0.04166667"},{"1":"545","2":"0.20833333"},{"1":"546","2":"0.29166667"},{"1":"547","2":"0.04166667"},{"1":"548","2":"0.04166667"},{"1":"549","2":"-0.04166667"},{"1":"550","2":"0.04166667"},{"1":"551","2":"0.20833333"},{"1":"552","2":"-0.04166667"},{"1":"553","2":"-0.04166667"},{"1":"554","2":"0.12500000"},{"1":"555","2":"0.04166667"},{"1":"556","2":"0.20833333"},{"1":"557","2":"0.12500000"},{"1":"558","2":"0.04166667"},{"1":"559","2":"-0.04166667"},{"1":"560","2":"-0.12500000"},{"1":"561","2":"-0.04166667"},{"1":"562","2":"-0.20833333"},{"1":"563","2":"-0.04166667"},{"1":"564","2":"-0.04166667"},{"1":"565","2":"0.12500000"},{"1":"566","2":"0.12500000"},{"1":"567","2":"-0.04166667"},{"1":"568","2":"-0.12500000"},{"1":"569","2":"0.20833333"},{"1":"570","2":"-0.20833333"},{"1":"571","2":"-0.20833333"},{"1":"572","2":"0.20833333"},{"1":"573","2":"-0.04166667"},{"1":"574","2":"0.04166667"},{"1":"575","2":"0.04166667"},{"1":"576","2":"-0.12500000"},{"1":"577","2":"-0.12500000"},{"1":"578","2":"0.04166667"},{"1":"579","2":"0.12500000"},{"1":"580","2":"0.12500000"},{"1":"581","2":"-0.04166667"},{"1":"582","2":"0.04166667"},{"1":"583","2":"-0.20833333"},{"1":"584","2":"0.12500000"},{"1":"585","2":"-0.29166667"},{"1":"586","2":"0.12500000"},{"1":"587","2":"-0.12500000"},{"1":"588","2":"0.04166667"},{"1":"589","2":"0.20833333"},{"1":"590","2":"0.04166667"},{"1":"591","2":"-0.12500000"},{"1":"592","2":"0.04166667"},{"1":"593","2":"-0.04166667"},{"1":"594","2":"0.12500000"},{"1":"595","2":"0.04166667"},{"1":"596","2":"-0.20833333"},{"1":"597","2":"-0.12500000"},{"1":"598","2":"0.04166667"},{"1":"599","2":"0.04166667"},{"1":"600","2":"-0.12500000"},{"1":"601","2":"-0.04166667"},{"1":"602","2":"-0.04166667"},{"1":"603","2":"0.04166667"},{"1":"604","2":"0.04166667"},{"1":"605","2":"0.04166667"},{"1":"606","2":"0.12500000"},{"1":"607","2":"0.04166667"},{"1":"608","2":"-0.04166667"},{"1":"609","2":"0.04166667"},{"1":"610","2":"-0.12500000"},{"1":"611","2":"-0.04166667"},{"1":"612","2":"-0.12500000"},{"1":"613","2":"-0.20833333"},{"1":"614","2":"0.12500000"},{"1":"615","2":"0.12500000"},{"1":"616","2":"-0.12500000"},{"1":"617","2":"-0.04166667"},{"1":"618","2":"0.12500000"},{"1":"619","2":"0.04166667"},{"1":"620","2":"-0.04166667"},{"1":"621","2":"-0.20833333"},{"1":"622","2":"-0.04166667"},{"1":"623","2":"0.04166667"},{"1":"624","2":"0.04166667"},{"1":"625","2":"0.04166667"},{"1":"626","2":"0.20833333"},{"1":"627","2":"-0.04166667"},{"1":"628","2":"0.12500000"},{"1":"629","2":"0.12500000"},{"1":"630","2":"0.12500000"},{"1":"631","2":"-0.04166667"},{"1":"632","2":"-0.04166667"},{"1":"633","2":"0.04166667"},{"1":"634","2":"-0.04166667"},{"1":"635","2":"0.29166667"},{"1":"636","2":"0.12500000"},{"1":"637","2":"-0.12500000"},{"1":"638","2":"0.04166667"},{"1":"639","2":"-0.12500000"},{"1":"640","2":"-0.04166667"},{"1":"641","2":"-0.20833333"},{"1":"642","2":"-0.04166667"},{"1":"643","2":"-0.04166667"},{"1":"644","2":"0.12500000"},{"1":"645","2":"-0.12500000"},{"1":"646","2":"-0.12500000"},{"1":"647","2":"0.12500000"},{"1":"648","2":"-0.12500000"},{"1":"649","2":"-0.20833333"},{"1":"650","2":"-0.04166667"},{"1":"651","2":"0.12500000"},{"1":"652","2":"0.20833333"},{"1":"653","2":"0.04166667"},{"1":"654","2":"0.04166667"},{"1":"655","2":"-0.12500000"},{"1":"656","2":"-0.12500000"},{"1":"657","2":"0.04166667"},{"1":"658","2":"0.12500000"},{"1":"659","2":"-0.04166667"},{"1":"660","2":"-0.12500000"},{"1":"661","2":"-0.04166667"},{"1":"662","2":"0.12500000"},{"1":"663","2":"-0.12500000"},{"1":"664","2":"0.20833333"},{"1":"665","2":"0.04166667"},{"1":"666","2":"0.12500000"},{"1":"667","2":"0.12500000"},{"1":"668","2":"-0.12500000"},{"1":"669","2":"0.20833333"},{"1":"670","2":"-0.12500000"},{"1":"671","2":"0.04166667"},{"1":"672","2":"-0.12500000"},{"1":"673","2":"-0.04166667"},{"1":"674","2":"0.04166667"},{"1":"675","2":"-0.29166667"},{"1":"676","2":"-0.12500000"},{"1":"677","2":"-0.04166667"},{"1":"678","2":"0.04166667"},{"1":"679","2":"-0.12500000"},{"1":"680","2":"-0.12500000"},{"1":"681","2":"-0.04166667"},{"1":"682","2":"0.04166667"},{"1":"683","2":"-0.04166667"},{"1":"684","2":"0.04166667"},{"1":"685","2":"-0.20833333"},{"1":"686","2":"-0.20833333"},{"1":"687","2":"0.12500000"},{"1":"688","2":"0.12500000"},{"1":"689","2":"0.12500000"},{"1":"690","2":"-0.12500000"},{"1":"691","2":"0.04166667"},{"1":"692","2":"0.04166667"},{"1":"693","2":"0.12500000"},{"1":"694","2":"-0.12500000"},{"1":"695","2":"-0.04166667"},{"1":"696","2":"-0.12500000"},{"1":"697","2":"-0.04166667"},{"1":"698","2":"0.04166667"},{"1":"699","2":"-0.12500000"},{"1":"700","2":"0.04166667"},{"1":"701","2":"0.29166667"},{"1":"702","2":"0.12500000"},{"1":"703","2":"0.04166667"},{"1":"704","2":"-0.04166667"},{"1":"705","2":"0.04166667"},{"1":"706","2":"0.04166667"},{"1":"707","2":"0.12500000"},{"1":"708","2":"0.12500000"},{"1":"709","2":"-0.04166667"},{"1":"710","2":"-0.20833333"},{"1":"711","2":"-0.04166667"},{"1":"712","2":"0.04166667"},{"1":"713","2":"0.04166667"},{"1":"714","2":"0.12500000"},{"1":"715","2":"0.04166667"},{"1":"716","2":"0.29166667"},{"1":"717","2":"0.12500000"},{"1":"718","2":"0.20833333"},{"1":"719","2":"0.12500000"},{"1":"720","2":"-0.20833333"},{"1":"721","2":"-0.04166667"},{"1":"722","2":"0.12500000"},{"1":"723","2":"0.04166667"},{"1":"724","2":"-0.20833333"},{"1":"725","2":"-0.12500000"},{"1":"726","2":"0.20833333"},{"1":"727","2":"-0.29166667"},{"1":"728","2":"0.04166667"},{"1":"729","2":"0.04166667"},{"1":"730","2":"0.04166667"},{"1":"731","2":"0.12500000"},{"1":"732","2":"-0.12500000"},{"1":"733","2":"0.04166667"},{"1":"734","2":"0.04166667"},{"1":"735","2":"-0.04166667"},{"1":"736","2":"-0.12500000"},{"1":"737","2":"-0.20833333"},{"1":"738","2":"-0.12500000"},{"1":"739","2":"0.04166667"},{"1":"740","2":"0.04166667"},{"1":"741","2":"0.04166667"},{"1":"742","2":"-0.12500000"},{"1":"743","2":"-0.04166667"},{"1":"744","2":"-0.12500000"},{"1":"745","2":"-0.20833333"},{"1":"746","2":"0.04166667"},{"1":"747","2":"0.04166667"},{"1":"748","2":"-0.04166667"},{"1":"749","2":"-0.04166667"},{"1":"750","2":"-0.12500000"},{"1":"751","2":"0.04166667"},{"1":"752","2":"0.04166667"},{"1":"753","2":"0.04166667"},{"1":"754","2":"-0.04166667"},{"1":"755","2":"0.12500000"},{"1":"756","2":"-0.12500000"},{"1":"757","2":"-0.20833333"},{"1":"758","2":"0.04166667"},{"1":"759","2":"0.12500000"},{"1":"760","2":"0.12500000"},{"1":"761","2":"-0.12500000"},{"1":"762","2":"-0.12500000"},{"1":"763","2":"-0.04166667"},{"1":"764","2":"0.29166667"},{"1":"765","2":"-0.20833333"},{"1":"766","2":"-0.12500000"},{"1":"767","2":"0.29166667"},{"1":"768","2":"0.04166667"},{"1":"769","2":"0.12500000"},{"1":"770","2":"0.12500000"},{"1":"771","2":"0.20833333"},{"1":"772","2":"0.04166667"},{"1":"773","2":"-0.12500000"},{"1":"774","2":"-0.04166667"},{"1":"775","2":"0.04166667"},{"1":"776","2":"0.20833333"},{"1":"777","2":"0.29166667"},{"1":"778","2":"-0.12500000"},{"1":"779","2":"-0.12500000"},{"1":"780","2":"0.12500000"},{"1":"781","2":"-0.04166667"},{"1":"782","2":"0.04166667"},{"1":"783","2":"-0.04166667"},{"1":"784","2":"-0.04166667"},{"1":"785","2":"-0.04166667"},{"1":"786","2":"0.04166667"},{"1":"787","2":"0.12500000"},{"1":"788","2":"-0.37500000"},{"1":"789","2":"0.20833333"},{"1":"790","2":"-0.20833333"},{"1":"791","2":"-0.04166667"},{"1":"792","2":"0.29166667"},{"1":"793","2":"-0.04166667"},{"1":"794","2":"0.04166667"},{"1":"795","2":"-0.04166667"},{"1":"796","2":"-0.20833333"},{"1":"797","2":"-0.20833333"},{"1":"798","2":"-0.12500000"},{"1":"799","2":"0.04166667"},{"1":"800","2":"0.12500000"},{"1":"801","2":"-0.12500000"},{"1":"802","2":"0.12500000"},{"1":"803","2":"-0.04166667"},{"1":"804","2":"-0.12500000"},{"1":"805","2":"-0.04166667"},{"1":"806","2":"0.04166667"},{"1":"807","2":"0.04166667"},{"1":"808","2":"0.20833333"},{"1":"809","2":"0.12500000"},{"1":"810","2":"0.04166667"},{"1":"811","2":"-0.12500000"},{"1":"812","2":"-0.12500000"},{"1":"813","2":"-0.12500000"},{"1":"814","2":"-0.20833333"},{"1":"815","2":"-0.20833333"},{"1":"816","2":"-0.04166667"},{"1":"817","2":"0.04166667"},{"1":"818","2":"-0.12500000"},{"1":"819","2":"-0.12500000"},{"1":"820","2":"-0.12500000"},{"1":"821","2":"0.12500000"},{"1":"822","2":"0.12500000"},{"1":"823","2":"-0.12500000"},{"1":"824","2":"-0.04166667"},{"1":"825","2":"-0.04166667"},{"1":"826","2":"0.04166667"},{"1":"827","2":"-0.12500000"},{"1":"828","2":"-0.04166667"},{"1":"829","2":"0.04166667"},{"1":"830","2":"-0.04166667"},{"1":"831","2":"-0.04166667"},{"1":"832","2":"-0.04166667"},{"1":"833","2":"-0.12500000"},{"1":"834","2":"-0.20833333"},{"1":"835","2":"0.20833333"},{"1":"836","2":"0.04166667"},{"1":"837","2":"-0.12500000"},{"1":"838","2":"0.12500000"},{"1":"839","2":"0.04166667"},{"1":"840","2":"-0.04166667"},{"1":"841","2":"0.04166667"},{"1":"842","2":"-0.04166667"},{"1":"843","2":"0.04166667"},{"1":"844","2":"0.04166667"},{"1":"845","2":"0.04166667"},{"1":"846","2":"-0.20833333"},{"1":"847","2":"-0.04166667"},{"1":"848","2":"-0.04166667"},{"1":"849","2":"0.12500000"},{"1":"850","2":"0.12500000"},{"1":"851","2":"-0.12500000"},{"1":"852","2":"-0.04166667"},{"1":"853","2":"0.04166667"},{"1":"854","2":"-0.04166667"},{"1":"855","2":"-0.04166667"},{"1":"856","2":"-0.20833333"},{"1":"857","2":"0.04166667"},{"1":"858","2":"0.12500000"},{"1":"859","2":"-0.12500000"},{"1":"860","2":"0.04166667"},{"1":"861","2":"0.04166667"},{"1":"862","2":"0.20833333"},{"1":"863","2":"-0.04166667"},{"1":"864","2":"0.12500000"},{"1":"865","2":"0.04166667"},{"1":"866","2":"-0.20833333"},{"1":"867","2":"0.04166667"},{"1":"868","2":"0.04166667"},{"1":"869","2":"-0.12500000"},{"1":"870","2":"0.04166667"},{"1":"871","2":"0.12500000"},{"1":"872","2":"-0.12500000"},{"1":"873","2":"0.12500000"},{"1":"874","2":"0.12500000"},{"1":"875","2":"-0.04166667"},{"1":"876","2":"-0.12500000"},{"1":"877","2":"-0.04166667"},{"1":"878","2":"0.04166667"},{"1":"879","2":"-0.04166667"},{"1":"880","2":"-0.12500000"},{"1":"881","2":"0.12500000"},{"1":"882","2":"0.04166667"},{"1":"883","2":"-0.04166667"},{"1":"884","2":"-0.04166667"},{"1":"885","2":"0.12500000"},{"1":"886","2":"-0.20833333"},{"1":"887","2":"-0.12500000"},{"1":"888","2":"0.12500000"},{"1":"889","2":"0.20833333"},{"1":"890","2":"0.20833333"},{"1":"891","2":"0.20833333"},{"1":"892","2":"0.04166667"},{"1":"893","2":"0.12500000"},{"1":"894","2":"0.04166667"},{"1":"895","2":"0.04166667"},{"1":"896","2":"-0.12500000"},{"1":"897","2":"-0.04166667"},{"1":"898","2":"0.12500000"},{"1":"899","2":"-0.04166667"},{"1":"900","2":"-0.12500000"},{"1":"901","2":"0.04166667"},{"1":"902","2":"0.12500000"},{"1":"903","2":"0.20833333"},{"1":"904","2":"0.04166667"},{"1":"905","2":"0.29166667"},{"1":"906","2":"-0.04166667"},{"1":"907","2":"-0.04166667"},{"1":"908","2":"0.04166667"},{"1":"909","2":"-0.20833333"},{"1":"910","2":"-0.12500000"},{"1":"911","2":"0.04166667"},{"1":"912","2":"0.04166667"},{"1":"913","2":"0.12500000"},{"1":"914","2":"-0.12500000"},{"1":"915","2":"0.12500000"},{"1":"916","2":"-0.12500000"},{"1":"917","2":"0.04166667"},{"1":"918","2":"-0.12500000"},{"1":"919","2":"-0.04166667"},{"1":"920","2":"-0.12500000"},{"1":"921","2":"-0.12500000"},{"1":"922","2":"-0.04166667"},{"1":"923","2":"0.12500000"},{"1":"924","2":"0.12500000"},{"1":"925","2":"0.12500000"},{"1":"926","2":"-0.04166667"},{"1":"927","2":"-0.12500000"},{"1":"928","2":"-0.12500000"},{"1":"929","2":"0.04166667"},{"1":"930","2":"0.12500000"},{"1":"931","2":"-0.04166667"},{"1":"932","2":"-0.04166667"},{"1":"933","2":"0.12500000"},{"1":"934","2":"-0.04166667"},{"1":"935","2":"-0.20833333"},{"1":"936","2":"-0.04166667"},{"1":"937","2":"-0.04166667"},{"1":"938","2":"0.12500000"},{"1":"939","2":"0.20833333"},{"1":"940","2":"0.04166667"},{"1":"941","2":"-0.12500000"},{"1":"942","2":"0.04166667"},{"1":"943","2":"-0.12500000"},{"1":"944","2":"-0.20833333"},{"1":"945","2":"-0.04166667"},{"1":"946","2":"-0.04166667"},{"1":"947","2":"0.04166667"},{"1":"948","2":"-0.12500000"},{"1":"949","2":"-0.04166667"},{"1":"950","2":"-0.04166667"},{"1":"951","2":"-0.12500000"},{"1":"952","2":"0.12500000"},{"1":"953","2":"-0.12500000"},{"1":"954","2":"-0.04166667"},{"1":"955","2":"-0.12500000"},{"1":"956","2":"0.04166667"},{"1":"957","2":"-0.20833333"},{"1":"958","2":"-0.04166667"},{"1":"959","2":"-0.04166667"},{"1":"960","2":"-0.04166667"},{"1":"961","2":"0.12500000"},{"1":"962","2":"-0.12500000"},{"1":"963","2":"0.04166667"},{"1":"964","2":"0.12500000"},{"1":"965","2":"0.04166667"},{"1":"966","2":"-0.04166667"},{"1":"967","2":"0.12500000"},{"1":"968","2":"0.37500000"},{"1":"969","2":"-0.12500000"},{"1":"970","2":"-0.12500000"},{"1":"971","2":"0.04166667"},{"1":"972","2":"-0.04166667"},{"1":"973","2":"-0.12500000"},{"1":"974","2":"-0.04166667"},{"1":"975","2":"0.12500000"},{"1":"976","2":"0.04166667"},{"1":"977","2":"0.04166667"},{"1":"978","2":"-0.12500000"},{"1":"979","2":"0.12500000"},{"1":"980","2":"-0.04166667"},{"1":"981","2":"-0.04166667"},{"1":"982","2":"0.04166667"},{"1":"983","2":"0.12500000"},{"1":"984","2":"0.04166667"},{"1":"985","2":"0.12500000"},{"1":"986","2":"-0.12500000"},{"1":"987","2":"0.04166667"},{"1":"988","2":"-0.12500000"},{"1":"989","2":"0.12500000"},{"1":"990","2":"0.04166667"},{"1":"991","2":"-0.12500000"},{"1":"992","2":"0.12500000"},{"1":"993","2":"0.12500000"},{"1":"994","2":"0.04166667"},{"1":"995","2":"0.04166667"},{"1":"996","2":"0.04166667"},{"1":"997","2":"-0.12500000"},{"1":"998","2":"-0.04166667"},{"1":"999","2":"-0.04166667"},{"1":"1000","2":"0.12500000"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


```r
decision_sex_test %>%
    visualize(bins = 9) +
    shade_p_value(obs_stat = obs_diff, direction = "two-sided")
```

<img src="10-hypothesis_testing_with_randomization_1-web_files/figure-html/unnamed-chunk-9-1.png" width="672" />

(You'll note that there is light gray shading in *both* tails above. This is because we are conducting a two-sided test, which means that we're interested in values that are more extreme than our observed difference in *both* directions.)

### Calculate the P-value. {#hypothesis1-ex-calculate-p}


```r
P <- decision_sex_test %>%
  get_p_value(obs_stat = obs_diff, direction = "two-sided")
P
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["p_value"],"name":[1],"type":["dbl"],"align":["right"]}],"data":[{"1":"0.048"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

Note: as with the test statistic above, the P-value appears above in a 1x1 tibble. That's fine for this step, but in the inline code below, we will need to use `pull` again to extract the value.

### Interpret the P-value as a probability given the null. {#hypothesis1-ex-interpret-p}

The P-value is 0.048. If there were no association between decision and sex, there would be a 4.8% chance of seeing data at least as extreme as we saw.

Some important things here:

1. We include an interpretation for our P-value. Remember that the P-value is the probability---**under the assumption of the null hypothesis**---of seeing results as extreme or even more extreme than the data we saw.

2. The P-value is less than 0.05 (just barely). Remember that as we talk about the conclusion in the next section of the rubric.


## Conclusion {#hypothesis1-ex-ht-conclusion}

### State the statistical conclusion. {#hypothesis1-ex-stat-conclusion}

We reject the null hypothesis.

### State (but do not overstate) a contextually meaningful conclusion. {#hypothesis1-ex-context-conclusion}

There is sufficient evidence to suggest that there is an an association between decision and sex in hiring branch managers for banks in the 1970s.

Note: the easiest thing to do here is just restate the alternative hypothesis. If we reject the null, then we have *sufficient* evidence for the alternative hypothesis. If we fail to reject the null, we have *insufficient* evidence for the alternative hypothesis. Either way, though, this contextually meaningful conclusion is all about the alternative hypothesis.

### Express reservations or uncertainty about the generalizability of the conclusion. {#hypothesis1-ex-reservations}

We have some reservations about how generalizable this conclusion is due to the fact that we are lacking information about how representative our samples of bank managers were. We also point out that this experiment was conducted in the 1970s, so its conclusions are not valid for today.

Note: This would also be the place to point out any possible sources of bias or confounding that might be present, especially for observational studies.


### Identify the possibility of either a Type I or Type II error and state what making such an error means in the context of the hypotheses. {#hypothesis1-ex-errors}

As we rejected the null, we run the risk of committing a Type I error. It is possible that there is no association between decision and sex, but we've come across a sample in which male files were somehow more likely to be recommended for promotion.

*****

After writing up your conclusions and acknowledging the possibility of a Type I or Type II error, the hypothesis test is complete. (At least for now. In the future, we will add one more step of computing a confidence interval.)


## More on one-sided and two-sided tests {#hypothesis1-one-sided-two-sided}

I want to emphasize again the difference between conducting a one-sided versus a two-sided test. You may recall that in "Introduction to simulation, Part 2", we calculated this:


```r
set.seed(9999)
sex_discrimination %>%
    specify(decision ~ sex, success = "promoted") %>%
    hypothesize(null = "independence") %>%
    generate(reps = 1000, type = "permute") %>%
    calculate(stat = "diff in props", order = c("male", "female")) %>%
    get_p_value(obs_stat = obs_diff, direction = "greater")
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["p_value"],"name":[1],"type":["dbl"],"align":["right"]}],"data":[{"1":"0.024"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

The justification was that, back then, we already suspected that male files were more likely to be promoted, and it appears that our evidence (the test statistic, or our observed difference) was pretty far in that direction. (Actually, we may get a slightly different number each time. Remember that we are randomizing. Therefore, we won't expect to get the exact same numbers each time.)

By way of contrast, in this chapter we computed the two-sided P-value:


```r
set.seed(9999)
sex_discrimination %>%
    specify(decision ~ sex, success = "promoted") %>%
    hypothesize(null = "independence") %>%
    generate(reps = 1000, type = "permute") %>%
    calculate(stat = "diff in props", order = c("male", "female")) %>%
    get_p_value(obs_stat = obs_diff, direction = "two-sided")
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["p_value"],"name":[1],"type":["dbl"],"align":["right"]}],"data":[{"1":"0.048"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

The only change to the code is the word "two-sided" (versus "greater") in the last line.

Our P-value in this chapter is twice as large as it could have been if we had run a one-sided test.

Doubling the P-value might mean that it no longer falls under the significance threshold $\alpha = 0.05$ (although in this case, we still came in under 0.05). This raises an obvious question: why use two-sided tests at all? If the P-values are higher, that makes it less likely that we will reject the null, which means we won't be able to prove our alternative hypothesis. Isn't that a bad thing?

As a matter of fact, there are many researchers in the world who do think it's a bad thing, and routinely do things like use one-sided tests to give them a better chance of getting small P-values. But this is not ethical. The point of research is to do good science, not prove your pet theories correct. There are many incentives in the world for a researcher to prove their theories correct (money, awards, career advancement, fame and recognition, legacy, etc.), but these should be secondary to the ultimate purpose of advancing knowledge. Sadly, many researchers out there have these priorities reversed. I do not claim that researchers set out to cheat; I suspect that the vast majority of researchers act in good faith. Nevertheless, the rewards associated with "successful" research cause cognitive biases that are hard to overcome. And "success" is often very narrowly defined as research that produces small P-values.

A better approach is to be conservative. For example, a two-sided test is not only more conservative because it produces higher P-values, but also because it answers a more general question. That is, it is scientifically interesting when an association goes in either direction (e.g. more male promotions, but also possibly more female promotions). This is why we recommended above using two-sided tests by default, and only using a one-sided test when there is a very strong research hypothesis that justifies it.


## A reminder about failing to reject the null {#hypothesis1-fail-to-reject}

It's also important to remember that when we fail to reject the null hypothesis, we are not saying that the null hypothesis is true. Neither are we saying it's false. Failure to reject the null is really a failure to conclude anything at all. But rather than looking at it as a failure, a more productive viewpoint is to see it as an opportunity for more research, possibly with larger sample sizes.

Even when we do reject the null, it is important not to see that as the end of the conversation. Too many times, a researcher publishes a "statistically significant" finding in a peer-reviewed journal, and then that result is taken as "Truth". We should, instead, view statistical inference as incremental knowledge that works slowly to refine our state of scientific knowledge, as opposed to a collection of "facts" and "non-facts".


## Your turn {#hypothesis1-your-turn}

Now it's your turn to run a complete hypothesis test. Determine if males were admitted to the top six UC Berkeley grad programs at a higher rate than females. For purposes of this exercise, we will not take into account the `Dept` variable as we did in the last chapter when we discussed Simpson's Paradox. But as that is a potential source of confounding, be sure to mention it in the part of the rubric where you discuss reservations about your conclusion.

As always, use a significance level of $\alpha = 0.05$.

Here is the data import:


```r
ucb_admit <- read_csv("https://vectorposse.github.io/intro_stats/data/ucb_admit.csv",
                      col_types = list(
                          Admit = col_factor(),
                          Gender = col_factor(),
                          Dept = col_factor()))
```

I have copied the template below. You need to fill in each step. Some of the steps will be the same or similar to steps in the example above. It is perfectly okay to copy and paste R code, making the necessary changes. It is **not** okay to copy and paste text. You need to put everything into your own words. Also, don't copy and paste the parts that are labeled as "Notes". That is information to help you understand each step, but it's not part of the statistical analysis itself.

The template below is exactly the same as in the [Appendix](#appendix-rubric) up to the part about confidence intervals which we haven't learned yet.


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
set.seed(9999)
# Add code here to simulate the null distribution.
# Run 1000 reps like in the earlier example.
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


## Conclusion {#hypothesis1-conclusion}

A hypothesis test is a formal set of steps---a procedure, if you will---for implementing the logic of inference. We take a skeptical position and assume a null hypothesis in contrast to the question of interest, the alternative hypothesis. We build a model under the assumption of the null hypothesis to see if our data is consistent with the null (in which case we fail to reject the null) or unusual/rare relative to the null (in which case we reject the null). We always work under the assumption of the null so that we can convince a skeptical audience using evidence. We also take care to acknowledge that statistical procedures can be wrong, and not to put too much credence in the results of any single set of data or single hypothesis test.

### Preparing and submitting your assignment {#hypothesis1-prep}

1. From the "Run" menu, select "Restart R and Run All Chunks".
2. Deal with any code errors that crop up. Repeat steps 1–-2 until there are no more code errors.
3. Spell check your document by clicking the icon with "ABC" and a check mark.
4. Hit the "Preview" button one last time to generate the final draft of the `.nb.html` file.
5. Proofread the HTML file carefully. If there are errors, go back and fix them, then repeat steps 1--5 again.

If you have completed this chapter as part of a statistics course, follow the directions you receive from your professor to submit your assignment.


