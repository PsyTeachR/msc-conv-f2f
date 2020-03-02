
# Supplementary Analyses {#refsup}

**WARNING** This chapter is still under construction.

![](http://www.reactiongifs.com/wp-content/uploads/2013/07/see.gif)

This chapter will contain a number of supplementary analyses. These are beyond the core content of the MSc Conversion course. Some of them are taken from the undergraduate course where there is more time for statistics, and some of them are just for fun. You may find some of the function useful for your dissertation or if you want to stretch yourself with the mini-project so it's worth having a browse.WW

## Data transformation

This section has been adapted from materials made available by [Greg Anderson at Bates College](http://abacus.bates.edu/~ganderso/biology/bio270/homework_files/Data_Transformation.pdf). 

The following brief overview of Data Transformation is compiled from Howell (pp. 318-324,
2007) and Tabachnick and Fidell (pp. 86-89, 2007). See the references at the end of this handout for a more complete discussion of data transformation. Most people find it difficult to accept the idea of transforming data. Tukey (1977) probably had the right idea when he called data transformation calculations “reexpressions” rather than “transformations.” A researcher is merely reexpressing what the data have to say in other terms. However, it is important to recognize that conclusions that you draw on transformed data do not always transfer neatly to the original measurements. Grissom (2000) reports that the means of transformed variables can occasionally reverse the difference of means of the original variables. While this is disturbing, and it is important to think about the meaning of what you are doing, but it is not, in itself, a reason to rule out the use of transformations as a viable option.

If you are willing to accept that is it permissible to transform one set of measures into another, then many possibilities become available for modifying the data to fit more closely the underlying assumptions of statistical tests. An added benefit about most of the transformations is that when we transform the data to meet one assumption, we often come closer to meeting other assumptions as well. For example, a square-root transformation may help equate group variances, and because it compresses the upper end of a distribution more than it compresses the lower end, it may also have the effect of making positively skewed distributions more nearly normal in shape. If you decide to transform, it is important to check that the variable is normally or nearly normally distributed after transformation. That is, make sure it worked.

When it comes to reporting our data… although it is legitimate and proper to run a statistical test, such as the one-way analysis of variance, on the transformed values, we often report means in the unit of the untransformed scale. This is especially true when the original units are intrinsically meaningful. Howell (2007) urges researchers to look at both the converted (transformed) and unconverted (original) means and make sure that they are telling the same basic story. Do not convert standard deviations – you will do serious injustice if you try that. And be sure to indicate to your readers what you have done. It is not uncommon to see both the converted and unconverted values reported. Tabachnick and Fidell (2007) point out that, although data transformations are recommended as a remedy for outliers and for failures of normality, linearity, and homoscedasticity, they are not universally recommended. The reason is that an analysis is interpreted from the variables that are in it, and transformed variables are sometimes harder to interpret. 

You should not get the impression that data transformations should be applied routinely to all your data. As a rule of thumb, “If it’s not broken, don’t fix it.” If your data are reasonably distributed (i.e., are more or less symmetrical and have few, if any, outliers) and if your variances are reasonably homogeneous, there is probably nothing to be gained by applying a transformation. If you have markedly skewed data or heterogeneous variances, however, some form of data transformation may be useful. Furthermore, it is perfectly legitimate to shop around for a transformation that makes the necessary changes to the variance and shape. The only thing you should not do it to try out every transformation, looking for one that gives you a significant result. You are trying to optimize the data, not the resulting F.

As suggested by Tabachnick and Fidell (2007) and Howell (2007), the following guidelines
should be used when transforming data.



Problem                                              Transformation         R function       
---------------------------------------------------  ---------------------  -----------------
Moderately positive skewness                         Square-root            `sqrt(var)`      
Substantially positive skewness                      Logarithmic (Log 10)   `log10(var)`     
Substantially positive skewness (with zero values)   Logarithmic (Log 10)   `log10(var + C)` 
Moderately negative skewness                         Square-Root            `sqrt(var + K)`  
Substantially negative skewness                      Logarithmic (Log 10)   `log10(K - var)` 

`C` = a constant added to each score so that the smallest score is 1.  
`K` = a constant from which each score is subtracted so that the smallest score is 1; usually equal to the largest score + 1.

**References**  

Howell, D. C. (2007). Statistical methods for psychology (6th ed.). Belmont, CA: Thomson
Wadsworth.  
Grissom, R. J. (2000). Heterogeneity of variance in clinical data. *Journal of Consulting and Clinical Psychology, 68*, 155-165.  
Tabachnick, B. G., & Fidell, L. S. (2007). *Using multivariate statistics (5th ed.)*. Boston: Allyn and Bacon.  
Tukey, J. W. (1977). *Exploratory data analysis*. Reading, MA: Addison-Wesley.
  
Also see:  
  
Hoaglin, D. C., Mosteller, F., & Tukey, J. W. (1983). *Understanding robust and exploratory data analysis*. New York: Wiley.

## Permutation tests

This section has been adapated from the Level 2 class on permutation tests written by Dr. Phil McAleer. The original [can be viewed here](https://psyteachr.github.io/ug2-practical/permutation-tests-a-skill-set.html).

### Overview

In this week's lab you will perform your first hypothesis test using a procedure known as a **permutation test**. We will help you learn how to do this through building and running data simulation procedures. In order to complete this lab you will require the following skills which we will teach you today:

  + Skill 1: Generating random numbers with `base::rnorm()` 
  + Skill 2: Permuting values with `base::sample()`  
  + Skill 3: Creating a "tibble" (a type of data table) using `tibble::tibble()`
  + Skill 4: Computing and extracting a difference in group means using `dplyr::pull()` and `purrr::pluck()`
  + Skill 5: Creating your own custom functions using `base::function()`
  + Skill 6: Repeating operations using `base::replicate()`
  
To many, a lot of statistics must seem a bit like blind faith as it deals with estimating quantities we haven't observed (or can't observe), e.g. the mean of a whole population. As such we have to know if we can trust our procedures for making estimations and inferences because we rarely get a chance to compare the estimated values to the true values to see if they match up. One way to test a procedure, and in turn learn about statistics, is through data simulation. In simulations **we create** a population and then draw samples and run tests on the data, i.e. on this **known** population. By running lots of simulations we can test our procedures and make sure they are acting as we expect them to. This approach is known as a **Monte Carlo simulation**, named after the city famous for the many games of chance that are played there. 

<div class="info">
<p>You can go read up on the Monte Carlo approach if you like. It can however get quite indepth, as having a brief glance at the wikipedia entry on it highlights. The main thing to keep in mind is that the method involves creating a population and continually taking samples from that population in order to make an inference. This is what we will show you in the lab. Data simulation and “creating” your own datasets, to see how tests work, is a great way to understand statistics. When doing this lab, keep in mind how easy it really is to find a significant result if even randomly created data can give a significant result. This may help dispell any notion that there is something inherently important about a significant result, in itself.</p>
</div>

We will now take each skill in turn. Be sure to try them all out. It looks a lot of reading but it is mainly just showing you the output of the functions so you can see you are doing it correctly. The key thing is to try them yourselves and don't be scared to change things to see what might happen if you do it slightly differently. We will also ask a couple of questions along the way to make sure you understand the skills.

### Skill 1: Generating Random Numbers

The `base::rnorm()` function generates values from a normal distribution and takes the following arguments:

  + `n`: the number of observations to generate
  + `mean`: the mean of the distribution (default 0)
  + `sd` : the standard deviation of the distribution (default 1)

To generate 10 or even 50 random numbers from a standard normal distribution (M = 0, SD = 1), you would use `rnorm(10)` or `rnorm(50)` respectively.  

* Type `rnorm(50)` into your console and see what happens. Use the below example for `rnorm(10)` to help you.  
* Try increasing `n` to 1000.  


```r
rnorm(10)
```

```
##  [1]  0.09595122 -1.85860834  1.27363960  1.01998709  0.45498293
##  [6]  1.64545920  0.01630733 -0.85848768  0.63645798 -0.60722779
```
<br>
<span style="font-size: 22px; font-weight: bold; color: var(--green);">Quickfire Questions</span>  

If you enter `rnorm(50)` again you will get different numbers. Why? <select class='solveme' data-answer='["The numbers are random"]'> <option></option> <option>I have made a mistake</option> <option>The numbers are random</option> <option>R has made a mistake</option> <option>Phil has made a mistake</option></select>

If you want to change the mean or sd, you would need to pass additional arguments to the function as shown below.  


```r
rnorm(n = 10, mean = 1, sd = .5)
```

* Try changing the mean and sd values a couple of times and see what happens. You get different numbers again that will be around the mean you set! Set a mean of 10, then a mean of 100, to test this.

Finally, for this Skill, you can concatenate (i.e. link) numbers together into a single vector using the `c()` function from base R.  For instance, say you wanted to create a vector with two sets of 50 random numbers from two separate samples: one set of 50 with a mean of 75 and the other with a mean of 90, you would use:


```r
random_numbers <- c(rnorm(50, 75),
                    rnorm(50, 90))
```

<span style="font-size: 22px; font-weight: bold; color: var(--green);">Quickfire Questions</span>  

In the above example code, what is the standard deviation of the two samples you have created? <select class='solveme' data-answer='["1"]'> <option></option> <option>50</option> <option>75</option> <option>90</option> <option>1</option></select>


<div class='solution'><button>Explain This - I don't get this answer!</button>

<div class="info">
<p>What is the <strong>default</strong> sd of the function?</p>
<p>Both populations would have an sd of 1, because that is the default, although you could easily change that. Try it out!</p>
</div>

</div>
 
<br>
It is always good to check that your new vector has the right number of data points in it - i.e. the total of the two samples; a sanity check if you will. The new vector `random_numbers` should have 100 elements. You could verify this using the `length()` function:


```r
length(random_numbers)
```

```
## [1] 100
```
<br>

### Skill 2: Permuting Values

Another thing that is useful to be able to do is to generate **permutations** of values.  


<div class='solution'><button>Portfolio Point - What are Permutations?</button>

<div class="info">
<p>A <strong>permutation</strong> is just a different ordering of the same values. For example, the numbers 1, 2, 3 can be permuted into the following 6 sequences:</p>
<ul>
<li>1, 2, 3</li>
<li>1, 3, 2</li>
<li>2, 1, 3</li>
<li>2, 3, 1</li>
<li>3, 1, 2</li>
<li>3, 2, 1</li>
</ul>
<p>The more values you have, the more permutations of the order you have. The number of permutations can be calculated by, for example, <code>3*2*1</code>, where 3 is the number of values you have. Or through code: <code>factorial(3) = 6</code>. This assumes that each value is used once in the sequence and that each value never changes, i.e. 1234 cannot suddenly become 1235.</p>
</div>

</div>


We can create random permutations of a vector using the `sample()` function. Let's use one of R's built in vectors: `letters`. 

* Type `letters` into the console, as below, and press RETURN/ENTER. You will see it contains all the lowercase letters of the English alphabet. Now, I bet you are wondering what `LETTERS` does, right?


```r
letters
```

```
##  [1] "a" "b" "c" "d" "e" "f" "g" "h" "i" "j" "k" "l" "m" "n" "o" "p" "q"
## [18] "r" "s" "t" "u" "v" "w" "x" "y" "z"
```

We can combine `base::sample()` with `letters` to put the letters into a random order:

* Run the below line. Run it again. And again. What do you notice? And why is our output different from yours? (The answer is below)


```r
sample(letters)
```

```
##  [1] "s" "k" "h" "l" "x" "i" "f" "v" "y" "q" "t" "w" "g" "n" "o" "u" "r"
## [18] "m" "j" "b" "a" "d" "c" "z" "p" "e"
```

<span style="font-size: 22px; font-weight: bold; color: var(--green);">Quickfire Questions</span>  

If `month.name` contains the names of the twelve months of the year, how many possible permutations are there of `sample(month.name)`? <select class='solveme' data-answer='["479001600"]'> <option></option> <option>1</option> <option>12</option> <option>144</option> <option>479001600</option></select>


<div class='solution'><button>Portfolio Point - Different samples with sample()</button>

<div class="info">
<p>Each time you run <code>sample(letters)</code> it will give you another random permutation of the sequence. That is what <code>sample()</code> does - creates a random permutation of the values you give it. Try repeating this command many times in the console. Because there are so many possible sequences, it is very unlikely that you will ever see the same sequence twice!</p>
<p>An interesting thing about <code>sample()</code> is that <code>sample(c(1,2,3,4))</code> is the same as <code>sample(4)</code>. And to recap, there would be 24 different permutations based on <code>factorial(4)</code>, meaning that each time you type <code>sample(4)</code> you are getting one of those 24 different orders. So what would factorial(12) be?</p>
<p>Top Tip: Remember that you can scroll up through your command history in the console using the up arrow on your keyboard; this way, you don’t ever have to retype a command you’ve already entered.</p>
</div>

</div>

<br>

### Skill 3: Creating Tibbles

Tables are important because most of the data we want to analyze comes in a table, i.e. tabular form. There are different ways to get tabular data into R for analysis.  One common way is to load existing data in from a data file (for example, using `readr::read_csv()` which you have seen before).  But other times you might want to just type in data directly.  You can do this using the `tibble::tibble()` function. Being able to create a tibble is a useful data analysis skill because sometimes you will want to create some data on the fly just to try certain codes or functions. 

#### Entering Data into a Tibble

The `tibble()` function takes named arguments - this means that the name you give each argument within the tibble function, e.g. `Y = rnorm(10)` will be the name of the column that appears in the table, i.e. `Y`.  It's best to see how it works through an example.


```r
tibble(Y = rnorm(10))
```

```
## # A tibble: 10 x 1
##          Y
##      <dbl>
##  1 -0.0733
##  2  1.19  
##  3  1.10  
##  4  0.575 
##  5  0.163 
##  6  0.0573
##  7  0.639 
##  8 -0.535 
##  9  0.0549
## 10  1.63
```

The above command creates a new table with one column named `Y`, and the values in that column are the result of a call to `rnorm(10)`: 10 randomly sampled values from a standard normal distribution (mean = 0, sd = 1) - See Skill 1.

If however we wanted to sample from two different populations for `Y`, we could combine two calls to `rnorm()` within the `c()` function. Again this was in Skill 1, here we are now just storing it in a tibble. See below:


```r
tibble(Y = c(rnorm(5, mean = -10), 
             rnorm(5, mean =  20)))
```

```
## # A tibble: 10 x 1
##         Y
##     <dbl>
##  1 -10.0 
##  2  -8.39
##  3 -11.1 
##  4 -10.0 
##  5 -10.4 
##  6  21.6 
##  7  20.6 
##  8  20.4 
##  9  19.4 
## 10  19.3
```

Now we have sampled a total of 10 observations - the first 5 come from a group with a mean of -10, and the second 5 come from a group with a mean of 20. Try changing the values in the above example to get an idea of how this works. Maybe even add a third group!

But, of course, it would be good to know which population each data point refers to and so we should add some group names. We can do this with some additional trickery using the `rep()` function.

#### Repeating Values to Save Typing

Before finalising our table let's learn a little about the base R function, `rep()`.  This is most useful for automatically repeating values in order to save typing.  For instance, if we wanted 20 letter "A"s in a row, we would type:


```r
rep("A", 20)
```

```
##  [1] "A" "A" "A" "A" "A" "A" "A" "A" "A" "A" "A" "A" "A" "A" "A" "A" "A"
## [18] "A" "A" "A"
```

The first argument to `rep()` is the vector containing the information you want repeated, **A**, and the second argument, `times`, is the number of times to repeat it; in this case **20**.

If you wanted to add more information, e.g. if the first argument has more than one element, say "A" and "B", it will repeat the entire vector that number of times; A B, A B, A B, ... .  Note that we enclose "A" and "B" in the `c()` function so that it is seen as a single argument.


```r
rep(c("A", "B"), 20)
```

```
##  [1] "A" "B" "A" "B" "A" "B" "A" "B" "A" "B" "A" "B" "A" "B" "A" "B" "A"
## [18] "B" "A" "B" "A" "B" "A" "B" "A" "B" "A" "B" "A" "B" "A" "B" "A" "B"
## [35] "A" "B" "A" "B" "A" "B"
```

But sometimes we want a specific number of As followed by a specific number of Bs; A A A B B B.  If the `times` argument has the same number of elements as the vector given for the first argument, it will repeat each element of the first vector as many times as given by the corresponding element in the `times` vector. In other words, for example, `times = c(2, 4)` for vector `c("A", "B")` will give you 2 As followed by 4 Bs.


```r
rep(c("A", "B"), c(2, 4))
```

```
## [1] "A" "A" "B" "B" "B" "B"
```

The best way to learn about this function is to play around with it in the console and see what happens. From the dropdown menus, the correct output of the following function would be:

1. `rep(c("A", "B", "C"),(2, 3, 1))`  - <select class='solveme' data-answer='["A A B B B C"]'> <option></option> <option>A A A B B C</option> <option>A A B B B C</option> <option>A A B B C C</option> <option>A B C A B C</option></select>

2. `rep(1:5, 5:1)` - <select class='solveme' data-answer='["1 1 1 1 1 2 2 2 2 3 3 3 4 4 5"]'> <option></option> <option>1 2 3 4 5 1 2 3 4 5 1 2 3 4 5</option> <option>5 5 5 5 5 4 4 4 4 3 3 3 2 2 1</option> <option>1 1 1 1 1 2 2 2 2 3 3 3 4 4 5</option> <option>1 1 1 1 1 1 1 1 5 5 5 5 5 5 5</option></select>  
 
#### Bringing it Together in a Tibble

Now we know `rep()`, we can complete our table of simulated data by combining what we've learned about generating random numbers and repeating values. We want our table to look like this:


```
## # A tibble: 10 x 2
##    group      Y
##    <chr>  <dbl>
##  1 A     -12.4 
##  2 A     -10.0 
##  3 A     -10.7 
##  4 A      -9.59
##  5 A     -10.2 
##  6 B      18.9 
##  7 B      20.4 
##  8 B      18.7 
##  9 B      18.7 
## 10 B      19.6
```

You now know how to create this table. Have a look at the code below and make sure you understand it. We have one column called `group` where we create **A**s and **B**s through `rep()`, and one column called **Y**, our data, all in our `tibble()`:


```r
tibble(group = rep(c("A", "B"), c(5, 5)),
       Y = c(rnorm(5, mean = -10), rnorm(5, mean =  20)))
```

Be sure to play around with the code chunk to get used to it. Try adding a third group or even a third column? Perhaps you want to give every participant a random age with a mean of 18, and a sd of 1; or even a participant number.


<div class='solution'><button>Helpful Hint</button>

<div class="info">
<p>Try <code>row_number()</code> to create participant numbers.</p>
</div>

</div>
 
<br>
Don't forget, if you wanted to store your tibble, you would just assign it to a name, such as `my_data`:


```r
my_data <- tibble(ID = row_number(1:10), 
                    group = rep(c("A", "B"), c(5, 5)),
                    Y = c(rnorm(5, mean = -10), rnorm(5, mean =  20)),
                    Age = rnorm(10, 18, 1))
```
<br>
<span style="font-size: 18px; font-weight: bold; color: var(--blue);">Skill 3 out of 6 Complete!</span>

### Skill 4: Computing Differences in Group Means

You have already learned how to calculate group means using `group_by()` and `summarise()`. For example, you might want to calculate sample means for a randomly generated dataset like so:


```r
my_data <- tibble(group = rep(c("A", "B"), c(20, 20)),
                  Y = c(rnorm(20,  20, 5), rnorm(20, -20, 5)))

my_data_means <- my_data %>%
  group_by(group) %>%
  summarise(m = mean(Y))

my_data_means
```

```
## # A tibble: 2 x 2
##   group     m
##   <chr> <dbl>
## 1 A      20.4
## 2 B     -17.7
```

Sometimes what we want though is to calculate **the differences between means** rather than just the means; so we'd like to subtract the second group mean -17.7 from the first group mean of 20.4, to get a single value, the difference: 38.

We can do this using the `dplyr::pull()` and `purrr::pluck()` functions.  `pull()` will extract a single column from a dataframe and turn it into a vector.  `pluck()` then allows you to pull out an element (i.e. a value or values) from within that vector.


```r
vec <- my_data_means %>%
  pull(m)

vec
```

```
## [1]  20.35569 -17.67668
```

We have now created `vec` which is a vector containing only the group means; the rest of the information in the table has been discarded.  Now that we have `vec`, we can calculate the mean difference as below, where `vec` is our vector of the two means and `[1]` and `[2]` refer to the two means:


```r
vec[1] - vec[2]
```

```
## [1] 38.03237
```

But `pluck()` is also useful, and can be written as so: 


```r
pluck(vec, 1) - pluck(vec, 2)
```

```
## [1] 38.03237
```

It can also be incorporated into a pipeline as below where we still `pull()` the means column, `m`, and then `pluck()` each value in turn and subtract them from each other.


```r
## whole thing in a pipeline
my_data_means %>% pull(m) %>% pluck(1) -
  my_data_means %>% pull(m) %>% pluck(2)
```

```
## [1] 38.03237
```

However, there is an alternative way to extract the difference between means which may make more intuitive sense.  You already know how to calculate a difference between values in the same row of a table using `dplyr::mutate()`, e.g. `mutate(new_column = column1 minus column2)`.  So if you can get the observations in `my_data_means` into the same row, different columns, you could then use `mutate()` to calculate the difference.  Previously you learned `gather()` to bring columns together. Well the opposite of gather is the `tidyr::spread()` function to split columns apart - as below.


```r
my_data_means %>%
  spread(group, m)
```

```
## # A tibble: 1 x 2
##       A     B
##   <dbl> <dbl>
## 1  20.4 -17.7
```

The spread function (`?spread`) splits the data in column `m` by the information, i.e. labels, in column `group` and puts the data into separate columns.  A call to `spread()` followed by a `mutate()` can be used to calculate the difference in means - see below:


```r
my_data_means %>%
  spread(group, m) %>%
  mutate(diff = A - B) 
```

```
## # A tibble: 1 x 3
##       A     B  diff
##   <dbl> <dbl> <dbl>
## 1  20.4 -17.7  38.0
```

* What is the name of the column containing the differences between the means of A and B? <select class='solveme' data-answer='["diff"]'> <option></option> <option>means</option> <option>group</option> <option>m</option> <option>diff</option></select>

Finally, if you then wanted to just get `diff` and throw away everything else in the table:


```r
my_data_means %>%
  spread(group, m) %>%
  mutate(diff = A - B) %>%
  pull(diff)
```

```
## [1] 38.03237
```


<div class='solution'><button>Portfolio Point - Reading pipes and verbalising tasks</button>

<div class="info">
<p>Keep in mind that a very useful technique for establishing what you want to do to a dataframe is to verbalise what you need, or to write it down in words, or to say it out loud. Take this last code chunk. What we wanted to do was to <code>spread()</code> the data in <code>m</code> into the groups A and B. Then we wanted to <code>mutate()</code> a new column that is the difference, <code>diff</code>, of A minus B. And finally we wanted to <code>pull()</code> out the value in <code>diff</code>.</p>
<p>Often step 1 of writing code or understanding code is knowing what it is you want to do in the first place. After that you just need the correct functions. Fortunately for us a lot of the <code>tidyverse</code> names its functions based on what they specifically do!</p>
</div>

</div>

<br>

### Skill 5: Creating Your Own Functions

In Skills 1 to 4, we have looked at creating and sampling data, storing it in a tibble, and extracting information from that tibble. Now say we wanted to do this over and over again. For instance, we might want to generate 100 random datasets just like the one in Skill 4. It would be a pain to have to type out the `tibble()` function 100 times or even to copy and paste it 100 times.  We'd likely make an error somewhere and it would be hard to read.  To help us, we can create a custom function that performs the action you want; in our case, creating a tibble of random data.  

Remember, a function is just a procedure that takes an input and gives you the same output each time - like a toaster! A function has the following format:


```r
name_of_function <- function(arg1, arg2, arg3) {
  ## body of function goes between these curly brackets; i.e. what the function does for you.
  ## Note that the last value calculated will be returned if you call the function.
}
```

First you define your own function name (e.g. `name_of_function`) and define the names of the arguments it will take (`arg1`, `arg2`, ...) - an argument is the information that you feed into your function, e.g. data. Finally, you state the calculations or actions of the function in the body of the function (the portion that appears between the curly braces). 

One of the most basic possible functions is one that takes no arguments and just prints a message. Here is an example:


```r
hello <- function() {
  print("Hello World!")
}
```

So this function is called `hello`. It can be run by typing `hello()` in your console and it will give the output of `Hello World!` every single time you run it; it has no other actions or information. Test this in the console now by typing:


```r
hello()
```

```
## [1] "Hello World!"
```

Awesome right? Ok, so not very exciting.  Let's make it better by adding an argument, `name`, and have it say Hello to `name`.


```r
hello <- function(name = "World!") {
  paste("Hello", name)
}
```

This new function is again called `hello()` and replaces the one you previously created. This time however you are supplying what is called a default argument, `name = "World!", but it still has the same action as the previous function of putting "Hello" and "World!" together. So if you run it you get "Hello World!". Try it yourself!


```r
hello()
```

```
## [1] "Hello World!"
```

The difference this time however is that because you have added an argument to the input, you can change the information you give the argument and therefore change the output of the function. More flexible. More exciting.

<span style="font-size: 22px; font-weight: bold; color: var(--green);">Quickfire Questions</span>

Test your understanding by answering these questions:

* Typing `hello("Phil")` in the console with this new function will give: <select class='solveme' data-answer='["Hello Phil"]'> <option></option> <option>Hello Heather</option> <option>Hello Phil</option> <option>Hello Niamh</option> <option>Hello Kevin</option></select>

* Typing the argument as `"is it me you are looking for"` will give: <select class='solveme' data-answer='["Hello is it me you are looking for"]'> <option></option> <option>Hello is it me you are looking for</option> <option>I just called to say Hello</option> <option>You had me at Hello</option> <option>Hello seems to be the hardest word</option></select>

* What argument would you type to get "Hello Dolly!" as the output: <select class='solveme' data-answer='["Dolly!"]'> <option></option> <option>Dolly</option> <option>Molly</option> <option>Holly</option> <option>Dolly!</option></select>

Most of the time however we want to create a function that computes a value or constructs a table.  For instance, let's create a function that returns randomly generated data from two samples, as we learned in the previous skills - see below. All we are doing is taking the tibble we created in Skill 4 and putting it in the body (between the curly brackets) of the function.


```r
gen_data <- function() {
  tibble(group = rep(c("A", "B"), c(20, 20)),
                  Y = c(rnorm(20,  20, 5), rnorm(20, -20, 5)))
}
```

This function is called `gen_data()` and when we run it we get a randomly generated table of two groups, each with 20 people, one with M = 20, SD = 5, the other with M = -20, sd = 5. Try running this `gen_data()` function in the console a few times; remember that as the data is random, the numbers will be different each time you run it.  

But say we want to modify the function to allow us to get a table with smaller or larger numbers of observations per group.  We can add an argument `n` and modify the code as follows. Create this function and run it a couple of times through `gen_data()`. The way to think about this is that every place that `n` appears in the body of the function (between the curly brackets) it will have the value of whatever you gave it in the arguments, i.e. in this case, 20.


```r
gen_data <- function(n = 20) {
  tibble(group = rep(c("A", "B"), c(n, n)),
                  Y = c(rnorm(n,  20, 5), rnorm(n, -20, 5)))
}
```

* How many total participants would there be if you ran `gen_data(2)`? <select class='solveme' data-answer='["4"]'> <option></option> <option>2</option> <option>4</option> <option>20</option> <option>40</option></select>

* What would you type to get 100 participants per group? <select class='solveme' data-answer='["gen_data(100)"]'> <option></option> <option>gen_data(50)</option> <option>gen_data(10)</option> <option>gen_dota(100)</option> <option>gen_data(100)</option></select>

**Challenge Question:**

Keeping in mind that functions can take numerous arguments, and that each group in your function have separate means, can you modify the function `gen_data` to allow the user to change the means for the two calls to `rnorm`? Have a try before revealing the solution below.


<div class='solution'><button>Solution To Challenge Question</button>


```r
gen_data <- function(n = 20, m1 = 20, m2 = -20) {
  tibble(group = rep(c("A", "B"), c(n, n)),
                  Y = c(rnorm(n,  m1, 5), rnorm(n, m2, 5)))
}

# m1 is the mean of group A, 
# m2 is mean of group B and would look like:

# The function would be called by: gen_data(20, 20, -20)
# Giving 20 participants in each group, 
# The first group having a mean of 20, 
# The second group having a mean of -20. 

# Likewise, a call of: gen_data(4, 10, 5)
# Would give two groups of 4, 
# The first having a mean of 10, 
# The second having a mean of 5.
```

</div>



<div class='solution'><button>Portfolio Point - Two important facts about functions</button>

<div class="info">
<p>Here are two important things to understand about functions.</p>
<ol style="list-style-type: decimal">
<li><p><strong>Functions obey lexical scoping.</strong> What does this mean? It’s like what they say about Las Vegas: what happens in the function, stays in the function. Any variables created inside of a function will be discarded after the function executes and will not be accessible to the outside calling process. So if you have a line, say a variable <code>my_var &lt;- 17</code> inside of a function, and try to print <code>my_var</code> from outside of the function, you will get an error: <code>object 'my_var' not found</code>. Although the function can ‘read’ variables from the environment that are not passed to it through an argument, it cannot change them. So you can only write a function to return a value, not change a value.</p></li>
<li><p><strong>Functions return the last value that was computed.</strong> You can compute many things inside of a function but only the last thing that was computed will be returned as part of the calling process. If you want to return <code>my_var</code>, which you computed earlier but not as the final computation, you can do so explicitly using <code>return(my_var)</code> at the end of the function (before the second curly bracket).</p></li>
</ol>
</div>

</div>

<br>

### Skill 6: Replicating Operations

The last skill you will need for the upcoming lab is knowing how to repeat an action (or expression) multiple times. You saw this in Lab 4 so we will only briefly recap here. Here, we use the  base function `replicate()`.  For instance, say you wanted to calculate the mean from `rnorm(100)` ten times, you could write it like this:


```r
## bad way
rnorm(100) %>% mean()
rnorm(100) %>% mean()
rnorm(100) %>% mean()
rnorm(100) %>% mean()
rnorm(100) %>% mean()
rnorm(100) %>% mean()
rnorm(100) %>% mean()
rnorm(100) %>% mean()
rnorm(100) %>% mean()
```

But it's far easier and more readable to wrap the expression in `replicate()` function where the first argument is the number of times you want to repeat the expression stated as the second argument, i.e. `replicate(times, expression)`. Here below we replicate the mean of 100 randomly generated numbers from the normal distribution, and we do this 10 times:


```r
replicate(10, rnorm(100) %>% mean())
```

Also you'll probably want to store the results in a variable, for example, `ten_samples`:


```r
ten_samples <- replicate(10, rnorm(100) %>% mean())
ten_samples
```

```
##  [1] -0.014831815  0.039219782 -0.036140814  0.116590880 -0.009711783
##  [6]  0.148121753  0.044451063  0.067318641 -0.066107079 -0.044085145
```

Each element (value) of the vector within `ten_samples` is the result of a single call to `rnorm(100) %>% mean()`.

* Assuming that your `hello()` function from Skill 5 still exists, and it takes the argument `name = Goodbye`, what would happen in the console if you wrote, `replicate(1000, hello("Goodbye"))`? <select class='solveme' data-answer='["Hello Goodbye would appear a thousand times"]'> <option></option> <option>Hello World would appear a thousand times</option> <option>hello Goodbye would appear a thousand times</option> <option>Hello Goodbye would appear a thousand times</option></select> - Try it and see if it works!


<div class='solution'><button>Solution To Quickfire Question</button>


```r
# the function would be:
hello <- function(name = "World!"){
  paste("Hello", name)
}

# and would be called by:
replicate(1000, hello("Goodbye"))
```

</div>

<br>

#### Finished!

To recap, we have shown you the following six skills:

  + Skill 1: Generating random numbers with `base::rnorm()` 
  + Skill 2: Permuting values with `base::sample()`  
  + Skill 3: Creating a "tibble" (a type of data table) using `tibble::tibble()`
  + Skill 4: Computing and extracting a difference in group means using `dplyr::pull()` and `purrr::pluck()`
  + Skill 5: Creating your own custom functions using `base::function()`
  + Skill 6: Repeating operations using `base::replicate()`

You will need these skills in the coming lab to help you perform a real **permutation test**. Through these skills and the permutation test you will learn about null hypothesis significance testing. 

### Permutation Tests of Hypotheses

A common statistical question when comparing two groups might be, "**Is there a real difference between the group means?**" From this we can establish two contrasting hypotheses: 

1. The **null hypothesis** which states that the group means **are equivalent** and is written as: $H_0: \mu_1 = \mu_2$ 
    - where $\mu_1$ is the population mean of group 1 
    - and $\mu_2$ is the population mean of group 2  
2. Or the **alternative hypothesis** which states the groups means **are not equivalent** and is written as: $H_1: \mu_1 \ne \mu_2$.

Using the techniques you read about earlier and in previous labs, today you will learn how to test the null hypothesis of no difference between two independent groups. We will first do this using a **permutation test** before looking at other tests in later labs. 

A permutation test is a basic inferential procedure that involves a reshuffling of group labels or values to create new possible outcomes of the data you collected to see how your original mean difference compares to all possible outcomes. The test can in fact be applied in many situations, this is just one, and it provides a good starting place for understanding hypothesis testing. The steps for the exercise below, and really the logic of a permutation test for two independent groups, are:

1. Calculate the real difference $D_{orig}$ between the means of two groups (e.g. Mean of A minus Mean of B).
2. Randomly shuffle the group labels (i.e. which group each participant belonged to - A or B) and re-calculate the difference, $D'$.
3. Repeat step 2 $N_{r}$ times, where $N_r$ is a large number (typically greater than 1000), storing each $D_i'$ value to form a null hypothesis distribution.
4. Locate the difference you observed in step 1 (the real difference) on the null hypothesis distribution of all possible differences. 
5. Decide whether the original difference is sufficiently extreme to reject the null hypothesis of no difference ($H_0$).

This logic works because if the null hypothesis is true (there is no difference between the groups) then the labeling of the observations/participants into groups is arbitrary, and we can rearrange the labels in order to estimate the likelihood of our original difference under the $H_0$. In other words if you know the original value of the difference between two groups (or the true difference) falls in the middle of your permuted distribution then there is no significant difference between the two groups. If however the original difference falls in the tail of the permuted distribution then there might be a significant difference depending on how far into the tail it falls.

Let's get started!

### Step 1: Load in Add-on Packages and Data {#Ch5InClassQueT1}

1.1.  Open a new script and call the `tidyverse` into your library.  

1.2.  Now type the statement `set.seed(1011)` at the top of your script after your library call and run it.  (This 'seeds' the random number generator so that you will get the same results as everyone else. The number 1011 is a bit random but if everyone uses it then we all get the same outcome. Different seeds give different outcomes)  

1.3.  <a href="perm_data.csv" download>Download the data file here.</a> and read the data in `perm_data.csv` into a variable called `dat`.  

1.4.  Let's give every participant a participant number as well by adding a new column to `dat`. Something like this would work: `mutate(subj_id = row_number())`


<div class='solution'><button>Helpful Hint</button>

<div class="info">
<ol style="list-style-type: decimal">
<li><p>Something to do with <code>library()</code></p></li>
<li><p><code>set.seed(1011)</code></p></li>
<li><p>Something to do with <code>read_csv()</code></p></li>
<li><p>pipe <code>%&gt;%</code> on the mutate line shown</p></li>
</ol>
</div>

</div>



<div class='solution'><button>Portfolio Point - Different uses of row_number</button>

<div class="info">
<p>You will see that in the example here to put a row number for each of the participants we do not have to state the number of participants we have. In the Preclass however we did. What is the difference? Well, in the Preclass we were making a tibble and trying to create a column in that tibble using <code>row_numbers</code>. If you want to do that you have to state the number of rows, e.g. <code>1:20</code>. However, in this example in the lab today the tibble already exists, we are just adding to it. If that is the case then you can just mutate on a column of row numbers without stating the number of participants. In summary:</p>
<ul>
<li>When creating the tibble, state the number of participants in <code>row_numbers()</code>.</li>
<li>If tibble already exists, just mutate on <code>row_numbers()</code>. No need for specific numbers.</li>
</ul>
</div>

</div>


Have a look at the resulting tibble, `dat`.  

* The column `Y` is your dependent variable (DV) 
* The column `group` is your independent variable (IV).
* The columns `subj_id` is the participant number.

### Step 2: Calculate the Original Mean Difference - $D_{orig}$ {#Ch5InClassQueT2}

We now need to write a pipeline of five functions that calculates the mean difference between the groups in `dat`, Group A minus Group B. Broken down into steps this would be: 

2.1.1. Use a pipe of two `dplyr` one-table verbs (e.g. Lab 2) to create a tibble where each row contains the mean of one of the groups. Name the column storing the means as `m`. 

2.1.2. Continue the pipe to `spread()` your data from long to wide format, based on the columns `group` and `m`.

2.1.3. Now add a pipe that **creates** a new column in this wide dataset called `diff` which is the value of group A's mean minus group B's mean.   

2.1.4.  Pull out the value in `diff` (the mean of group A minus the mean of group B) to finish the pipe.  


<div class='solution'><button>Helpful Hint</button>

<div class="info">
<p><code>dat %&gt;%</code></p>
<p><code>group_by(?) %&gt;%</code></p>
<p><code>summarise(m = ?) %&gt;%</code></p>
<p><code>spread(group, m) %&gt;%</code></p>
<p><code>mutate(diff = ? - ?) %&gt;%</code></p>
<p><code>pull(?)</code></p>
</div>

</div>


* Check that your value for `d_orig` is correct, without using the solution, by typing your `d_orig` value to two decimal places in the box. Include the sign, e.g. -1.23. The box will go green if you are correct. <input class='solveme nospaces' size='10' data-answer='["-7.39"]'/>

The above steps have created a pipeline of five functions to get one value. Nice! We now need to turn this into a function because we are going to be permuting the data set (specifically the grouping labels) and re-calculating the difference many, many times.

2.2.  Wrap your pipeline in a function called `calc_diff` but swap `dat` for `x`. This function will take a single argument named `x`, where `x` is the tibble that you want to calculate group means from. As in the previous step, the function will return a single value which is the difference between the group means. The start will look like this below:  


```r
calc_diff <- function(x){
  x %>%.....
}
```


<div class='solution'><button>Helpful Hint</button>


```r
calc_diff <- function(x) {
  x %>%
    group_by(group) %>%
    the_rest_of_your_pipe...
}
```

</div>


2.3.  Now call your new function where `x` = `dat` as the argument and store the result in a new variable called `d_orig`.  Make sure that your function returns the same value as you got above and that your function returns a single value rather than a tibble. You can test this: `is.tibble(d_orig)` should give you `FALSE` and `is.numeric(d_orig)` should give you `TRUE`.


<div class='solution'><button>Helpful Hint</button>


```r
d_orig <- function_name(x = data_name) 
# or
d_orig <- function_name(data_name)

# Then type the following in the Console and look at answer:

is.tibble(d_orig)
# True (is a tibble) or False (is not a tibble)

is.numeric(d_orig)
# True (is numeric) or False (is not numeric; it is a character or integer instead.)
```

</div>


So we now have the original difference between the groups stored in `d_orig`. Next we need to create a distribution of all possible differences to see where our original difference lies in this distribution. But first we need to shuffle the `group` letters (A or B) in our dataset and find the difference...a few hundred times!

### Step 3: Permute the Group Labels {#Ch5InClassQueT3}

3.1.  Create a new function called `permute()` that takes as input a dataset `x` and returns the same dataset transformed such that the group labels (the values in the column `group`) are shuffled: started below for you.  This will require using the `sample()` function within a `mutate()`. You have used `mutate()` twice already today and you saw how to `sample()` **letters** in the PreClass.


```r
permute <- function(x){
  x %>%.....
}
```


<div class='solution'><button>Helpful Hint</button>

<div class="info">
<p>Might be easier to think of these steps in reverse.</p>
<ol style="list-style-type: decimal">
<li><p>Start with a <code>mutate()</code> function that rewrites the column <code>group</code> every time you run it, e.g. <code>dat %&gt;% mutate(variable = sample(variable))</code></p></li>
<li><p>Now put that into your <code>permute()</code> function making the necessary adjustments to the code so it starts <code>x %&gt;%...</code>. Again <code>x</code> should be in the function and not <code>dat</code>. ")</p></li>
</ol>
</div>

</div>


3.2.  Try out your new `permute()` function by calling it on `dat` (i.e. `x = dat`) a few times.  You should see the group labels in the `group` column changing randomly. The most common mistake is that people mutate a new column by mispelling `group`. You want to overwrite/change the information in the `group` column not make a new one, so be careful with the spelling.

Now would be an excellent time to spend five minutes as a group recapping what you are doing. 

* You have the original difference between groups. 
* You have a function that calculates and stores this difference.
* You have a function that reshuffles the labels of the group. 

Do you understand why? If not, go back to the principles of the permutation test at the start of the lab then read on...

### Step 4: Create the Null-Hypothesis Distribution (NHD) for the Difference {#Ch5InClassQueT4}

Now that we have the original difference and our two functions, one to shuffle group labels and one to calculate the difference between two groups, we need to actually create the distribution of possible differences and see where the original difference lies in it.

4.1.1.  Write a **a single pipeline** that takes `dat` as the input, permutes the group labels with a call to your function `permute()`, and then calculates the difference in means between these new groups with a call to your function `calc_diff()`.  

4.1.2. Run this line manually a few times and watch the resulting value change as the labels get permuted.


<div class='solution'><button>Helpful Hint</button>

<div class="info">
<p>Think about verbalising your pipelines. In a single pipeline:</p>
<ol style="list-style-type: decimal">
<li>I want to permute the data into two new groups.</li>
<li>Then I want to calculate the difference between these two new groups.</li>
</ol>
<p>The functions you have created do these steps. You just have to put them in order and pipe the data through it.</p>
</div>

</div>


4.2.  Now take your pipeline of functions and repeat it 1000 times using the `replicate()` function. Store the output in a variable called `nhd`. `nhd` will contain 1000 values where each value is the mean difference of each of the 1000 random permutations of the data. (**Warning:** This will probably take a while to run, perhaps 10 seconds.)


<div class='solution'><button>Helpful Hint</button>


```r
# replace expression with the pipeline you created in 4.1
nhd <- replicate(times, expression)
```

</div>


You now have 1000 possible values of the difference between the permuted groups A and B - your permuted distribution. 

4.3 Let's visualise this distribution through a frequency histogram of the values in `nhd`.  This shows us the likelihood of various mean differences under $H_0$. One thing to note however is that `nhd` is not a `tibble` and `ggplot` needs it to be a `tibble`. You need to convert it. You might start by do something like:


```r
ggplot(data = tibble(x = NULL), aes(x)) + NULL
```


<div class='solution'><button>Helpful Hint</button>

<div class="info">
<p>Remember that <code>ggplot</code> works as: <code>ggplot(data, aes(x)) + geom...</code>. Here you need to convert <code>nhd</code> into a tibble and put that in as your data. Look at the example above and keep in mind that, in this case, the first NULL could be replaced with the data in <code>nhd</code>.</p>
</div>

</div>


* Looking at the histogram, visually locate where your original value would sit on this distribution. Would it be extreme, in the tail, or does it look rather common, in the middle? <select class='solveme' data-answer='["is in the tail so looks extreme"]'> <option></option> <option>is in the middle so looks common</option> <option>is in the tail so looks extreme</option></select>

Before moving on stop to think about what this means - that the difference between the two original groups is rather uncommon in this permuted distribution, i.e. is in the tails! Again, if unsure, go back to the principles of NHST or discuss it with your tutor!

### Step 5: Compare the Observed Mean Difference to the NHD {#Ch5InClassQueT5}

If the null hypothesis is false, and there is a real difference between the groups, then the difference in means we observed for the original data (`d_orig`) should be somewhere in either tail of the null-hypothesis distribution we just estimated; it should be an "extreme" value.  How can we test this beyond a visual inspection?

First we have to decide on a false positive (Type I error) rate which is the rate at which we will falsely reject $H_0$ when it is true.  This rate is referred to by the Greek letter $\alpha$ ("alpha").  Let's just use the conventional level used in Psychology: $\alpha = .05$.

So the question we must ask is, if the null hypothesis was  true, what would be the probability of getting a difference in means as extreme as the one we observed in the original data? We will label this probability `p`.  

Take a few moments to see if you can figure out how you might compute `p` from the data before we show you how. We will then show you the process in the next few, final, steps.

5.1.  Replace the NULLS in the code below to create a logical vector which states TRUE for all values of `nhd` greater than or equal to `d_orig` regardless of sign. **Note:** A logical vector is one that returns TRUE when the expression is true and FALSE when the expression is false. 


```r
lvec <- abs(NULL) >= abs(NULL)
```


<div class='solution'><button>Portfolio Point - abs and the case of one or two tails</button>

<div class="info">
<p>In the code above, the function <code>abs()</code> says to ignore the sign and use the absolute value. For instance, if <code>d_orig = -7</code>, then <code>abs(d_orig) = 7</code>. Why do we do this here? Can you think why you want to know how extreme your value is in this distribution regardless of whether the value is positive or negative?</p>
<p>The answer relates to whether you are testing in one or two tails of your distribution; the positive side, the negative side, or both. You will have heard in your lectures of one or two-tailed tests. Most people would say to run two-tailed tests. This means looking at the negative and positive tails of the distribution to see if our original value is extreme, and the simplest way to do this is to ignore the sign of the values and treat both sides equally. If you wanted to only test one-tail, say that your value is extreme to the negative side of the tail, then you would not use the <code>abs()</code> and set the expression to make sure you only find values less than your original value. To test only on the positive side of the distribution, make sure you only get values higher than the original. But for now we will mostly look at two-tailed tests.</p>
</div>

</div>


5.2.  Replace the NULL in the code below to `sum()` the `lvec` vector to get the total number of values equal to or greater than our original difference, `d_orig`. Fortunately R is fine with summing TRUEs and FALSEs so you do not have to convert the data at all.


```r
n_exceeding_orig <- NULL
```

5.3.  Replace the NULL in the code below to calculate the probability of finding a value of `d_orig` in our `nhd` distribution by dividing `n_exceeding_orig`, the number of values greater than or equal to your original value, by the `length()` of your whole distribution `nhd`. **Note: the length of `nhd` is the same as the number of replications we ran. Using code reduces the chance of human error**


```r
p <- NULL
```

5.4.  Finally, complete the sentence below determining if the original value was extreme or not in regards to the distribution. Use inline coding, shown in Lab 1, to replace the `XXX`s. For example, when formatted without the space before the first r, ` r length(nhd)` would present as 1000.

**" The difference between Group A and Group B (M = `XXX`) was found to be have a probability of p = `XXX`. This means that the original mean difference was ...... and the null hypothesis is ....." **

#### Finished!

Well done in completing this lab. Let's recap before finishing. We had two groups, A and B, that we had tested in an experiment. We calculated the mean difference between A and B and wanted to know if this was a significant difference. To test this we created a distribution of all possible differences between A and B using the premise of permutation tests and then found the probability of our original value in that permuted distribution. The more extreme the value in a distribution the more likely that the difference is significant. And that is exactly what we found; an $\alpha < .05$. Next time we will look at using functions and inferential tests to perform this analysis but by understanding the above you now know how probability is determined.


## Non-parametric tests

## Simulation

## rtweet

### Packages

In order to run these analyses you will need `tidyverse` for data wrangling,`rtweet` for getting the twitter data, `tidytext` for working with text, `knitr` for tidy tables, and `igraph` and `ggraph` for making pretty network plots.

In order to get data from Twitter you will need to have a twitter account and gain access to Twitter's API. There are instructions [for doing so here](https://rtweet.info/).


```r
library(tidyverse)
library(knitr)
library(tidytext)
library(rtweet)
library(igraph)
library(ggraph)
```

### Hashtag search

We can use rtweet to search for all tweets containing particular words or hashtags. It will return the last 18,000 tweets and only from the last 6-9 days. Be careful, there is a 15 minute time-out which means you can only retrieve 18,000 tweets every 15 minutes (there are various limits on what Twitter allows you to do with the data).

First, let's search for all tweets that contain #GoT and #ForTheThrone.

There are various arguments to the `search_tweets` function, you can specify how many tweets you want to retrive with `n`, whether retweets should be included in the search results with `include_rts` and you can also specify the language of the user's account with `lang`. Note that this doesn't tell you what language the tweets are written in, only that the account language is set to e.g., English, so it's not perfect. For additional options, see the help documentation.



```r
tweets <- search_tweets(q = "#GoT OR #ForTheThrone", n = 18000, include_rts = FALSE, lang = "en")
```



### Data wrangling

The output of `search_tweets()` is a list which makes it difficult to share. We need to do a little bit of tidying to turn it into a tibble, and additionally there's a lot of data and we don't need it all. I'm also going to add a tweet counter which will help when we move between wide and long-form. `rtweet` provides a huge amount of data, more than we're going to use in this example so have a look through to see what you have access to - there are some great examples of how this data can be used [here](https://mkearney.github.io/nicar_tworkshop/) and [here](https://rud.is/books/21-recipes/index.html) .

In the following code, I have added an identifier column `tweet_number` and then selected `text` (the actual text of the tweets), `created_at` (the timestamp of the tweet), `source` (iphone/android etc), `followers_count` (how many followers the accounts the tweets came from have), and `country` (like `lang`, the is the country specified on the account, not necessarily the country the tweeter is in).


```r
dat <- tweets %>%
  mutate(tweet_number = row_number())%>%
  select(tweet_number, text, created_at, source, followers_count, country)%>%
  as_tibble()
```

You can <a href="all_data.zip" download>download this file here</a> so that you can reproduce my exact analyses (or you can use your own, but the results will look a bit different).

The first thing we need to do is tidy up the text by getting rid of punctuation, numbers, and links that aren't of any interest to us. We can also remove the hashtags because we don't want those to be included in any analysis. This code uses **regular expressions** which quite frankly make very little sense to me, I have copied and pasted this code from the Tidy Text book.


```r
dat <- dat %>%
  mutate(text = str_replace_all(text, "[^\x01-\x7F]", ""),
         text = str_replace_all(text, "#GoT", ""),
         text = str_replace_all(text, "#ForTheThrone", ""),
         text = str_replace_all(text, "\\.|[[:digit:]]+", ""),
         text = str_replace_all(text, "https|amp|t.co", ""))
```

### Time series

We can plot when the tweets were sent. This is somewhat uninteresting because it's no longer airing, but it's worth highlighting this as a feature. If you were watching live, you could use this to see the spikes in tweets when people are watching each episode live (different timezones will muddle this a little, you could filter by `country` perhaps).



```r
ts_plot(tweets, by = "1 hours")
```

<div class="figure" style="text-align: center">
<img src="11-supplementary_files/figure-html/unnamed-chunk-22-1.png" alt="Time series plot by hour" width="100%" />
<p class="caption">(\#fig:unnamed-chunk-22)Time series plot by hour</p>
</div>

You can change the time interval with the `by` argument and you can also change the time zone. `ts_plot` creates a `ggplot` object so you can also add the usual ggplot layers to customise apperance. 


```r
ts_plot(tweets, by = "10 mins", tz = "GMT") +
  theme_minimal()+
  scale_y_continuous(name = "Number of tweets")
```

<div class="figure" style="text-align: center">
<img src="11-supplementary_files/figure-html/unnamed-chunk-23-1.png" alt="Time series plot by 10 minute intervals" width="100%" />
<p class="caption">(\#fig:unnamed-chunk-23)Time series plot by 10 minute intervals</p>
</div>

### Tidy text and word frequencies

First, we can produce frequency plots for words used in all tweets to see which words are used most often in #GoT and #ForTheThrone tweets. To do this, we have to create a tidy dataset, just like we do when working with numerical data. We're going to use the `unnest_tokens` function from `tidytext` which will separate each word on to a new line, something similar to like using `gather` (or `pivot_longer` as it will soon be known). Helpfully, this function will also convert all of our words to lower case which makes them a bit easier to work with.

The second part of the code removes all the stop words. Stop words are words that are commonly used but are of not real interest, for example function words like "the", "a", "it". You can make your own list but `tidytext` helpfully comes with several databases. Look at the help documentation if you want to know more about these or change the defaults.



```r
# create tidy text
dat_token <- dat %>%  
  unnest_tokens(word, text) %>%
  anti_join(stop_words, by = "word")
```

We can plot the 20 most frequent words used in all the tweets.


```r
dat_token%>%
  na.omit()%>%
  count(word, sort = TRUE)%>%
  head(20) %>%
  mutate(word = reorder(word, n))%>%
  ggplot(aes(x = word, y = n))+
  geom_col()+
  coord_flip()
```

<div class="figure" style="text-align: center">
<img src="11-supplementary_files/figure-html/unnamed-chunk-25-1.png" alt="Most frequent words" width="100%" />
<p class="caption">(\#fig:unnamed-chunk-25)Most frequent words</p>
</div>

There's quite a few words here that aren't that helpful to us so it might be best to get rid of them (essentially we're building our own list of stop words).


```r
custom_stop <- c("gameofthrones", "hbo", "season", "game", "thrones", "lol", "tco", "https", "watch", "watching", "im", "amp")

dat_token <- dat_token %>%
  filter(!word %in% custom_stop)
```

Now we can try plotting the words again and make them pretty. 


```r
dat_token%>%
  na.omit()%>%
  count(word, sort = TRUE)%>%
  head(20) %>%
  mutate(word = reorder(word, n))%>%
  ggplot(aes(x = word, y = n, fill = word))+
  geom_col(show.legend = FALSE)+
  coord_flip()+
  scale_fill_viridis(discrete = TRUE)
```

<div class="figure" style="text-align: center">
<img src="11-supplementary_files/figure-html/unnamed-chunk-27-1.png" alt="Most frequent words (edited)" width="100%" />
<p class="caption">(\#fig:unnamed-chunk-27)Most frequent words (edited)</p>
</div>

To be honest, this isn't that interesting because it's so general, it might be more interesting to see how often each of the main characters are being mentioned. 

One problem is that people on the internet are terrible at spelling and we need to have the exact spellings which means that for Daenerys, Jon, and Jaime, the chances that people will have spelled their names wrong is quite high (as my level 2 students who watched me live code the first version of this will attest) so first we're going to correct those. 


```r
dat_token2 <- dat_token %>%
  mutate(word = recode(word, "khaleesi" = "daenerys",
                       "dany" = "daenerys",
                       "jamie" = "jaime",
                       "john" = "jon"))

characters <- c("jon", "daenerys", "bran", "arya", "sansa", "tyrion", "cersei", "jaime")
```

Now we can plot a count of how many times each name has been mentioned.


```r
dat_token2 %>%
  filter(word %in% characters)%>%
  count(word, sort = TRUE) %>%
  mutate(word = reorder(word, n))%>%
  ggplot(aes(x = word, y = n, fill = word)) +
  geom_col(show.legend = FALSE) +
  coord_flip()+
  scale_y_continuous(name = "Number of mentions")+
  scale_x_discrete(name = "Character")+
  scale_fill_viridis(discrete = TRUE)
```

<div class="figure" style="text-align: center">
<img src="11-supplementary_files/figure-html/unnamed-chunk-29-1.png" alt="Frequecy of mentions for each character" width="100%" />
<p class="caption">(\#fig:unnamed-chunk-29)Frequecy of mentions for each character</p>
</div>

### Bigram analysis

Rather than looking at individual words we can look at what words tend to co-occur. We want to use the data set where we've corrected the spelling so this is going to require us to transform from long to wide and then back to long because the night is dark and full of terror. DID YOU SEE WHAT I DID THERE.


```r
dat_bigram <- dat_token2 %>%
  group_by(tweet_number) %>%  summarise(text = str_c(word, collapse = " "))%>% # this puts it back into wide-form
  unnest_tokens(bigram, text, token = "ngrams", n = 2, collapse = FALSE)%>% # and then this turns it into bigrams in a tidy format
  na.omit()

dat_bigram %>%
  count(bigram, sort = TRUE)%>%
  head(10)%>%
  kable(align = "c")
```

      bigram          n  
------------------  -----
   freefolk gt       165 
 togive freefolk     164 
    se tvtime        103 
 watched episode     88  
    episode se       86  
     ice fire        85  
     song ice        81  
 jonsnow daenerys    73  
  bracelets fef      71  
 connected matter    71  

Again there's a bit of nonsense here and it's a bit uninteresting but it's worth highlighting this is something you can do. Now that we've got our bigrams we can plot these to see the connections between the different words. First, we're going to use `separate` to put the two words into different columns, then we'll count them up and plot them. If you want more information about this see the [tidytext book online](https://www.tidytextmining.com/ngrams.html) as I am entirely cribbing this from that book. The plot requires the packages `igraph` and `ggraph`.


```r
bigrams_separated <- dat_bigram %>%
  separate(bigram, c("word1", "word2"), sep = " ")

# new bigram counts:
bigram_counts <- bigrams_separated %>% 
  count(word1, word2, sort = TRUE)

bigram_counts

# filter for only relatively common combinations (more than 20 occurances)
bigram_graph <- bigram_counts %>%
  filter(n > 20) %>%
  graph_from_data_frame()

# make a pretty network plot

ggraph(bigram_graph, layout = "fr") +
  geom_edge_link() +
  geom_node_point() +
  geom_node_text(aes(label = name), vjust = 1, hjust = 1)+
  theme_void()
```

<div class="figure" style="text-align: center">
<img src="11-supplementary_files/figure-html/unnamed-chunk-31-1.png" alt="Network graph of bigrams" width="100%" />
<p class="caption">(\#fig:unnamed-chunk-31)Network graph of bigrams</p>
</div>

```
## # A tibble: 30,134 x 3
##    word1     word2        n
##    <chr>     <chr>    <int>
##  1 freefolk  gt         165
##  2 togive    freefolk   164
##  3 se        tvtime     103
##  4 watched   episode     88
##  5 episode   se          86
##  6 ice       fire        85
##  7 song      ice         81
##  8 jonsnow   daenerys    73
##  9 bracelets fef         71
## 10 connected matter      71
## # ... with 30,124 more rows
```

I am still figuring out how to customise the aesthetics of `ggraph`. 

### Sentiment analysis

Sentiment analyses look at whether the expressed opinion in a bit of text is positive, negative, or neutral, using information from databases about the valance of different words. We can perform a sentiment analysis on the tweets that contain each character's name to see whether e.g., Jon is mentioned in tweets that are largely positive or if Jaime is mentioned in tweets that are largely negative. 

To do this, we first need to do a bit of wrangling. We're going to transform it back to wide-form, then we're going to add in in a column that says whether each character was mentioned in the tweet, then we're going to transform it back to long-form.


```r
dat_mentions <- dat_token2 %>%
  group_by(tweet_number) %>%
  summarise(text = str_c(word, collapse = " "))%>%
  mutate(jon = case_when(str_detect(text, ".jon") ~ TRUE, TRUE ~ FALSE),
         daenerys = case_when(str_detect(text, ".daenerys") ~ TRUE, TRUE ~ FALSE),
         bran = case_when(str_detect(text, ".bran") ~ TRUE, TRUE ~ FALSE),
         arya = case_when(str_detect(text, ".arya") ~ TRUE, TRUE ~ FALSE),
         sansa = case_when(str_detect(text, ".sansa") ~ TRUE, TRUE ~ FALSE),
         tyrion = case_when(str_detect(text, ".tyrion") ~ TRUE, TRUE ~ FALSE),
         cersei = case_when(str_detect(text, ".cersei") ~ TRUE, TRUE ~ FALSE),
         jaime = case_when(str_detect(text, ".jaime") ~ TRUE, TRUE ~ FALSE))%>%
  unnest_tokens(word, text) %>%
  anti_join(stop_words, by = "word")
```

Once we've done this, we can then run a sentiment analysis on the tweets for each character. I still haven't quite cracked iteration so this code is a bit repetitive, if you can give me the better way of doing this that's less prone to copy and paste errors, please do.


```r
jon <- dat_mentions %>%
  filter(jon)%>%
  inner_join(get_sentiments("bing"))%>%
  count(index = tweet_number, sentiment) %>%
  spread(sentiment, n, fill = 0) %>%
  mutate(sentiment = positive - negative)%>%
  mutate(character = "jon")

daenerys <- dat_mentions %>%
  filter(daenerys == "TRUE")%>%
  inner_join(get_sentiments("bing"))%>%
  count(index = tweet_number, sentiment) %>%
  spread(sentiment, n, fill = 0) %>%
  mutate(sentiment = positive - negative)%>%
  mutate(character = "daenerys")


bran <- dat_mentions %>%
  filter(bran == "TRUE")%>%
  inner_join(get_sentiments("bing"))%>%
  count(index = tweet_number, sentiment) %>%
  spread(sentiment, n, fill = 0) %>%
  mutate(sentiment = positive - negative)%>%
  mutate(character = "bran")

arya <- dat_mentions %>%
  filter(arya == "TRUE")%>%
  inner_join(get_sentiments("bing"))%>%
  count(index = tweet_number, sentiment) %>%
  spread(sentiment, n, fill = 0) %>%
  mutate(sentiment = positive - negative)%>%
  mutate(character = "arya")

sansa <- dat_mentions %>%
  filter(sansa == "TRUE")%>%
  inner_join(get_sentiments("bing"))%>%
  count(index = tweet_number, sentiment) %>%
  spread(sentiment, n, fill = 0) %>%
  mutate(sentiment = positive - negative)%>%
  mutate(character = "sansa")

tyrion <- dat_mentions %>%
  filter(tyrion == "TRUE")%>%
  inner_join(get_sentiments("bing"))%>%
  count(index = tweet_number, sentiment) %>%
  spread(sentiment, n, fill = 0) %>%
  mutate(sentiment = positive - negative)%>%
  mutate(character = "tyrion")

cersei <- dat_mentions %>%
  filter(cersei == "TRUE")%>%
  inner_join(get_sentiments("bing"))%>%
  count(index = tweet_number, sentiment) %>%
  spread(sentiment, n, fill = 0) %>%
  mutate(sentiment = positive - negative)%>%
  mutate(character = "cersei")

jaime <- dat_mentions %>%
  filter(jaime == "TRUE")%>%
  inner_join(get_sentiments("bing"))%>%
  count(index = tweet_number, sentiment) %>%
  spread(sentiment, n, fill = 0) %>%
  mutate(sentiment = positive - negative)%>%
  mutate(character = "jaime")


dat_sentiment <- bind_rows(jon,daenerys,bran,arya,sansa,tyrion,cersei,jaime) %>%
  group_by(character) %>%
  summarise(positive = sum(positive),
            negative = sum(negative),
            overall = sum(sentiment))%>%
  gather(positive:overall, key = type, value = score)%>%
  mutate(type = factor(type, levels = c("positive", "negative", "overall")))
```

Now that we've done all that we can display a table of positive, negative and overall sentiment scores. Bear in mind that not all words have an associated sentiment score, particularly if they're a non-standard usage of English (as an aside, this makes RuPaul's Drag Race very difficult to analyse because tidytext will think a sickening death drop is a bad thing).


```r
dat_sentiment %>%
  spread(type, score)%>%
  arrange(desc(overall))%>%
  kable(align = "c")
```



 character    positive    negative    overall 
-----------  ----------  ----------  ---------
   jaime         16          29         -13   
  tyrion         15          30         -15   
   sansa         26          44         -18   
  cersei         28          55         -27   
   arya          26          58         -32   
   bran          32          66         -34   
 daenerys        62         155         -93   
    jon          90         185         -95   

Because there's diferent numbers of tweets for each character, it might be more helpful to convert it to percentages to make it easier to compare.


```r
dat_sentiment %>%
  spread(type, score)%>%
  group_by(character)%>%
  mutate(total = positive + negative)%>%
  mutate(positive_percent = positive/total*100,
         negative_percent = negative/total*100,
         sentiment = positive_percent - negative_percent)%>%
  select(character, positive_percent, negative_percent, sentiment)%>%
  arrange(desc(sentiment))%>%
  kable(align = "c")
```



 character    positive_percent    negative_percent    sentiment 
-----------  ------------------  ------------------  -----------
   sansa          37.14286            62.85714        -25.71429 
   jaime          35.55556            64.44444        -28.88889 
  cersei          33.73494            66.26506        -32.53012 
  tyrion          33.33333            66.66667        -33.33333 
    jon           32.72727            67.27273        -34.54545 
   bran           32.65306            67.34694        -34.69388 
   arya           30.95238            69.04762        -38.09524 
 daenerys         28.57143            71.42857        -42.85714 

They're all quite negative because there's no pleasing some people but there's some face validity to the analysis given the order of the rankings. If you'd been watching this live you could have repeated this each episode to see how the reactions to the characters changes.

Let's make that into a graph cause graphs are great.


```r
dat_sentiment %>%
  spread(type, score)%>%
  group_by(character)%>%
  mutate(total = positive + negative)%>%
  mutate(positive_percent = positive/total*100,
         negative_percent = negative/total*100,
         sentiment = positive_percent - negative_percent)%>%
  select(character, positive_percent, negative_percent, sentiment)%>%
  arrange(desc(sentiment))%>%
  ggplot(aes(x = reorder(character,sentiment), y = sentiment)) +
  geom_col(show.legend = FALSE) +
  scale_y_continuous(name = "Overall sentiment (percent)")+
  scale_x_discrete(name = "Character")
```

<div class="figure" style="text-align: center">
<img src="11-supplementary_files/figure-html/unnamed-chunk-36-1.png" alt="Sentiment scores for each character" width="100%" />
<p class="caption">(\#fig:unnamed-chunk-36)Sentiment scores for each character</p>
</div>

`rtweet` is such a cool package and I've found that the limits of what you can do with it are much more about one's imagination. There's much more you could do with this package but when I first ran these analyses I found that tracking RuPaul's Drag Race was a fun way to learn a new package as it did give an insight into the fan reactions of one of my favourite shows. I also use this package to look at swearing on Twitter (replace the hashtags with swear words). The best way to learn what `rtweet` and `tidytext` can do for you is to find a topic you care about and explore the options it gives you. If you have any feedback on this tutorial you can find me on twitter: [@emilynordmann](https://twitter.com/emilynordmann).




