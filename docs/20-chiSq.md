# Categorical Data and Chi Squared Tests

**Chapter Links**


* Chapter 19-20 Slides: [pdf](http://tysonbarrett.com/EDUC-6600/Slides/u06_Ch19-20_categorical.pdf) or [power point](http://tysonbarrett.com/EDUC-6600/Slides/u06_Ch19-20_categorical.pptx)



**Unit Assignment Links**

* Unit 6 Writen Part: [Skeleton - pdf](https://usu.box.com/s/rk3cojw85xecankrgeo5fx6pvyc7k5tw)

* Unit 6 R Part: [Directions - pdf](https://usu.box.com/s/p2c9052g86ijhgowap2t45nm0ndq1po6) and [Skeleton - Rmd](https://usu.box.com/s/h4kyfov5hjsif7bmqousumfyew6t5sfl)

* Unit 6 Reading to Summarize: [@Marchand2011] [pdf on BOX](https://usu.box.com/s/ab2nobh4u2412eqhdb3llpb17gz5lhk2) or [online ](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3762448/) 

* Inho's Dataset: [Excel](https://usu.box.com/s/9jazgd17mn5bnib4jrdg6zb5jtlze3wi)





-----------------------------------------------------


Required Packages 


```r
library(tidyverse)    # Loads several very helpful 'tidy' packages
library(furniture)    # Nice tables (by our own Tyson Barrett)
library(pander)       # Nice tables in genderal
```



-----------------------------------------------------


## Goodenss of Fit (1-way) 

### Observed Counts vs. Equally Likely Hypothesis

**TEXTBOOK Example:** *Often, especially in an experimental context, the expected frequencies are based on more abstract theoretical considerations. For instance, imagine that a developmental psychologist is studying color preference in toddlers. Each child is told that he or she can take one toy out of four that are offered. All four toys are identical except for color: red, blue, yellow, or green. Forty children are run in the experiment, and their color preferences are as follows: red, 13; blue, 9; yellow, 15; and green, 3. These are the obtained frequencies. The expected frequencies depend on the null hypothesis. If the null hypothesis is that toddlers in general have no preference for color, we would expect the choices of colors to be equally divided among the entire population of toddlers. Hence, the expected frequencies would be 10 for each color.*



Use the `chisq.test()` function to perform a Goodnes-of-Fit or one-way Chi-Squared test to see if the observed counts are significantly different from being equally distributed. 

> **NOTE:** You do not need to declare any options inside the `chisq.test()` function, as the default is to use equally likely probabilities.


```r
# Run the 1-way chi-square test for equally likely
chisq_toy_color <- c(red    = 13, 
                     blue   = 9, 
                     yellow = 15, 
                     green  = 3) %>% 
  chisq.test()                             # defaults to Equally likely
```



The following code chunk shows how to create and display a table of the observed and expected counts for any 1-way Chi-squated test.


```r
# Request the observed and expected counts
rbind(Observed = chisq_toy_color$observed,
      Expected = chisq_toy_color$expected) %>% 
  pander::pander()
```


--------------------------------------------
    &nbsp;      red   blue   yellow   green 
-------------- ----- ------ -------- -------
 **Observed**   13     9       15       3   

 **Expected**   10     10      10      10   
--------------------------------------------



To display the full output, type and run the name the model is save as.


```r
# Diplay the full output
chisq_toy_color
```

```

	Chi-squared test for given probabilities

data:  .
X-squared = 8.4, df = 3, p-value = 0.03843
```



-----------------------------------------------------



### Observed counts vs. Hypothesised Probabilities

**TEXTBOOK Example:** *Imagine that the population of a city is made up of three ethnic groups, which I will label A, B, and C.  The obtained frequencies were 28, 18, and 2. You could test the null hypothesis that sample is representatve of a population proportions which is half group A and a third group B.*



The `chisq.test()` function may also be used to perform a Goodnes-of-Fit or one-way Chi-Squared test to see if the observed counts are significantly different from thoes expected from a set of hypothesised probabilies. 

> **NOTE:** You **DO** need to declare the probabilities, as the default is to use equally likely probabilities.  You may do this by including `p = c(`$p_1$`, `$p_2$`, ..., `$p_k$`)` within the `chisq.test()` function.  The $p_i$'s maybe typed as decimals or fractions, but make suer they add up to exactly $1$!


```r
# Run the 1-way chi-square test for hypothesised probabilityes
chisq_ethnic <- c(A = 28,
                  B = 18,
                  C = 2) %>% 
  chisq.test(p = c(1/2, 1/3, 1/6))      # declare the probabilities
```



Use the same code chunk to display a table of the observed and expected counts for any 1-way Chi-squated test.

> **HINT** You may *copy-and-paste* this code for the rest of the assignment, but make sure to change the name of the model (`chisq_ethnic` appears twice before the \$-sign).


```r
# Request the observed and expected counts
rbind(Observed = chisq_ethnic$observed,
      Expected = chisq_ethnic$expected) %>% 
  pander::pander()
```


----------------------------
    &nbsp;      A    B    C 
-------------- ---- ---- ---
 **Observed**   28   18   2 

 **Expected**   24   16   8 
----------------------------


To display the full output, type and run the name the model is save as.


```r
# Diplay the full output
chisq_ethnic
```

```

	Chi-squared test for given probabilities

data:  .
X-squared = 5.4167, df = 2, p-value = 0.06665
```


-----------------------------------------------------

## Test for Independence (2-way) - vs. Association

**TEXTBOOK Example:** *Suppose that the researcher has interviewed 30 women who have been married: 10 whose parents were divorced and 20 whose parents were married. Half of the 30 women in this hypothetical study have gone through their own divorce; the other half are still married for the first time. To know whether the divorce of a person's parents makes the person more likely to divorce, we need to see the breakdown in each category- that is, how many currently divorced women come from "broken" homes and how many do not, and similarly for those still married. These frequency data are generally presented in a contingency (or cross-classification) table:*


The dataset needs to be declared a table before you can run a Chi-Squared Test


```r
# Store the data as a table
woman_parents <- data.frame(home_broken    = c(7, 3),
                            home_complete  = c(8, 12),
                            row.names = c("self_divorced", "self_married")) %>% 
  as.matrix() %>% 
  as.table() 
```




```r
# Display the observed counts
woman_parents %>% 
  addmargins() %>% 
  pander::pander()
```


-------------------------------------------------------
      &nbsp;         home_broken   home_complete   Sum 
------------------- ------------- --------------- -----
 **self_divorced**        7              8         15  

 **self_married**         3             12         15  

      **Sum**            10             20         30  
-------------------------------------------------------



The `chisq.test()` function may also be used to perform a two-way Chi-Squared test for independence.  In this case, the observed counts are compared to thoes expected if there is no association between the two factors.  


```r
# Run the 2-way chi-square test for independence
chisq_divorces <- woman_parents %>% 
  chisq.test(correct = FALSE)     #IF 2x2, add correct = FALSE
```



To display the counts expected if the variables are independent, start with the model name  and add `$expected` at the end.  Then pipe on both the `addmargins()` and `pander::pander()` functions to print the counts. 


```r
# Request the expected counts based on "no association"
chisq_divorces$expected %>% 
  pander::pander()
```


-------------------------------------------------
      &nbsp;         home_broken   home_complete 
------------------- ------------- ---------------
 **self_divorced**        5             10       

 **self_married**         5             10       
-------------------------------------------------



To display the full output, type and run the name the model is save as.


```r
# Diplay the full output
chisq_divorces
```

```

	Pearson's Chi-squared test

data:  .
X-squared = 2.4, df = 1, p-value = 0.1213
```

