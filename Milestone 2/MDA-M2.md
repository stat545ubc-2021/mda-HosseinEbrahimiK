Begin by loading your data and the tidyverse package below:

``` r
suppressMessages(library(datateachr)) # <- might contain the data you picked!
suppressMessages(library(tidyverse))
```

# Task 1: Process and summarize your data (15 points)

### 1.1 (2.5 points)

First, write out the 4 research questions you defined in milestone 1
were. This will guide your work through milestone 2:

<!-------------------------- Start your work below ---------------------------->

**Answer:**

1.  Which types of trees have been planted the most in the past five
    years in each neighborhood? In other words, is there a trend in
    cultivated tree species in each area?
2.  Is there any kind of relation between the height of trees (with the
    same type) and the neighborhood they have been planted in?
3.  Within each neighborhood, do trees with a curb around them tend to
    have smaller diameters (in terms of growth)?
4.  Could we validate within the dataset that the age of a tree directly
    relates to its diameter? Is it the same rate among all tree species,
    or it differs from one to another?
    <!----------------------------------------------------------------------------->

### 1.2 (10 points)

Now, for each of your four research questions, choose one task from
options 1-4 (summarizing), and one other task from 4-8 (graphing). You
should have 2 tasks done for each research question (8 total). Make sure
it makes sense to do them! (e.g. don’t use a numerical variables for a
task that needs a categorical variable.). Comment on why each task helps
(or doesn’t!) answer the corresponding research question.

Ensure that the output of each operation is printed!

**Summarizing:**

1.  Compute the *range*, *mean*, and *two other summary statistics* of
    **one numerical variable** across the groups of **one categorical
    variable** from your data.
2.  Compute the number of observations for at least one of your
    categorical variables. Do not use the function `table()`!
3.  Create a categorical variable with 3 or more groups from an existing
    numerical variable. You can use this new variable in the other
    tasks! *An example: age in years into “child, teen, adult, senior”.*
4.  Based on two categorical variables, calculate two summary statistics
    of your choosing.

**Graphing:**

1.  Create a graph out of summarized variables that has at least two
    geom layers.
2.  Create a graph of your choosing, make one of the axes logarithmic,
    and format the axes labels so that they are “pretty” or easier to
    read.
3.  Make a graph where it makes sense to customize the alpha
    transparency.
4.  Create 3 histograms out of summarized variables, with each histogram
    having different sized bins. Pick the “best” one and explain why it
    is the best.

Make sure it’s clear what research question you are doing each operation
for!

**Answer:**

<!------------------------- Start your work below ----------------------------->

#### Question 1: any trend in cultivated tree species in each area in past 5 years?

For this question, I choose **computing number of observations (option
2)** for the summarizing task. If we are going to compare between each
area that which tree species is most popular, it would be suitable to
know exactly how many trees were planted that year in neighborhoods. For
example, if we’re saying 80% percent of planted trees were from ABIES in
Kits and 70% in Downtown, the number of total planted trees would be of
interest to be more accurate. Also, it is useful in comparison between
tree species in different years.

``` r
num_tr_nghbr <- vancouver_trees %>%
  # creating a variable year when trees are planted
  mutate(year_planted = as.character(substring(date_planted, 1, 4))) %>%
  # grouping trees by their neighborhood & year
  group_by(neighbourhood_name, year_planted) %>%
  # trees has planted in the pas 10 years
  filter(as.numeric(substring(Sys.Date(), 1, 4)) - 
          as.numeric(year_planted) <= 5) %>%
  # counting number of trees per year in each neighborhood
  tally()

num_tr_nghbr
```

    ## # A tibble: 88 × 3
    ## # Groups:   neighbourhood_name [22]
    ##    neighbourhood_name year_planted     n
    ##    <chr>              <chr>        <int>
    ##  1 ARBUTUS-RIDGE      2016            51
    ##  2 ARBUTUS-RIDGE      2017            58
    ##  3 ARBUTUS-RIDGE      2018            57
    ##  4 ARBUTUS-RIDGE      2019            43
    ##  5 DOWNTOWN           2016            29
    ##  6 DOWNTOWN           2017            42
    ##  7 DOWNTOWN           2018             1
    ##  8 DOWNTOWN           2019            53
    ##  9 DUNBAR-SOUTHLANDS  2016            78
    ## 10 DUNBAR-SOUTHLANDS  2017            54
    ## # … with 78 more rows

I choose **option 6 (logarithmic scale and formatting the axes labels)**
for this question. The plot is trying to convey the summarized variable
in a way that is easy to compare the number of planted trees across
years and neighborhoods.

``` r
hist_tr_nghbr <- num_tr_nghbr %>%
  ggplot(aes(neighbourhood_name, n)) +
  geom_col(aes(fill=year_planted), position = "dodge") +
  scale_y_log10() +
  xlab("Neighbourhood") +
  ylab("Number of trees (log)") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90))

print(hist_tr_nghbr)
```

![](MDA-M2_files/figure-markdown_github/unnamed-chunk-3-1.png)

#### Question 2: Any relation between the height of trees (with the same type) and the neighborhood?

I choose **option 1: summary statistics** for this question. I
calculated mean, min, max, median, and standard deviation of height
across neighborhoods and tree species. It would be useful to know the
mean and other statistics on the height when we’re comparing it between
two neighborhoods to detect differences.

``` r
height_nghbr <- vancouver_trees %>%
  # putting together species and neighborhood 
  group_by(neighbourhood_name, genus_name) %>%
  summarise(mean_height= mean(height_range_id),  # mean
            min_height = min(height_range_id), # min
            max_height =  max(height_range_id), # max
            median_height = median(height_range_id), # median
            std_height = sd(height_range_id), .groups = 'drop')

  #filter(genus_name == "X") ** by adding this line you can see the differences in height statistics for a species across neighborhoods **

height_nghbr
```

    ## # A tibble: 1,303 × 7
    ##    neighbourhood_name genus_name  mean_height min_height max_height median_height
    ##    <chr>              <chr>             <dbl>      <dbl>      <dbl>         <dbl>
    ##  1 ARBUTUS-RIDGE      ABIES              3.11          2          7             2
    ##  2 ARBUTUS-RIDGE      ACER               2.69          1          9             2
    ##  3 ARBUTUS-RIDGE      AESCULUS           4.67          1          8             5
    ##  4 ARBUTUS-RIDGE      AMELANCHIER        1.95          1          4             2
    ##  5 ARBUTUS-RIDGE      BETULA             5.04          1          7             5
    ##  6 ARBUTUS-RIDGE      CALOCEDRUS         1             1          1             1
    ##  7 ARBUTUS-RIDGE      CARPINUS           2.02          1          6             2
    ##  8 ARBUTUS-RIDGE      CATALPA            2.8           2          6             2
    ##  9 ARBUTUS-RIDGE      CEDRUS             4.73          2          9             5
    ## 10 ARBUTUS-RIDGE      CELTIS             1             1          1             1
    ## # … with 1,293 more rows, and 1 more variable: std_height <dbl>

Since we have many types of tree species in the data, I focused only on
the family of trees instead of species and considered the top 3 that are
most populated in the dataset to investigate the question. I plotted the
density distribution of the height of the tree families across
neighborhoods to explore whether there are any differences in the
distributions among neighborhoods or not.

``` r
top_genus <- vancouver_trees %>%
  group_by(genus_name) %>%
  tally() %>%
  arrange(desc(n)) %>%
  top_n(3)
```

    ## Selecting by n

``` r
height_dist_nghbr <- vancouver_trees %>%
  filter(genus_name %in% top_genus$genus_name) %>%
  group_by(genus_name) %>%
  ggplot(aes(height_range_id)) +
  geom_density(aes(fill=genus_name), alpha = 0.4) +
  facet_wrap(~ neighbourhood_name) + 
  xlab("Height")
  
height_dist_nghbr
```

![](MDA-M2_files/figure-markdown_github/unnamed-chunk-5-1.png)

It seems distributions do not vary a lot for each tree family across
neighborhoods!

#### Question 3: curb affects diameters?

For this research question, It would be good to know how many trees have
curbs around them and how many do not. Because it would not be a fair
comparison if we have a lot of observations from one population and few
samples from another one. It turns out to be a case in our question! We
do have fewer samples for `curb=NO` against `curb=YES` in each
neighborhood.

``` r
count_curbed <- vancouver_trees %>%
  group_by(neighbourhood_name, curb) %>%
  tally() # counting  numbers of trees with and without curb in each neighborhood

count_curbed
```

    ## # A tibble: 44 × 3
    ## # Groups:   neighbourhood_name [22]
    ##    neighbourhood_name curb      n
    ##    <chr>              <chr> <int>
    ##  1 ARBUTUS-RIDGE      N       500
    ##  2 ARBUTUS-RIDGE      Y      4669
    ##  3 DOWNTOWN           N        69
    ##  4 DOWNTOWN           Y      5090
    ##  5 DUNBAR-SOUTHLANDS  N      1797
    ##  6 DUNBAR-SOUTHLANDS  Y      7618
    ##  7 FAIRVIEW           N       203
    ##  8 FAIRVIEW           Y      3799
    ##  9 GRANDVIEW-WOODLAND N       612
    ## 10 GRANDVIEW-WOODLAND Y      6091
    ## # … with 34 more rows

I plotted the distribution of trees diameter with and without curbs in
each neighborhood. So, it can help us identify does having a curb
affects the tree’s diameter. As shown in the plot, there is no
significant difference between the distributions and their means in most
neighborhoods. Also, it is worth mentioning that numbers of positive
(with a curb) and negative are not the same! If we had more negative
samples, we could answer the question better.

``` r
means <- vancouver_trees %>%
  filter(0 < diameter & diameter < 50) %>%
  group_by(neighbourhood_name, curb) %>%
  # calculating mean of diameter of trees in each neighborhood based on curb
  summarise(mean_dmtr = mean(diameter), .groups = 'drop')

dist_dmtr_nghbr <- vancouver_trees %>%
  filter(0 < diameter & diameter < 50) %>%
  group_by(neighbourhood_name) %>%
  ggplot(aes(diameter)) + 
  geom_density(aes(fill=curb), alpha=0.5) +
  geom_vline(data=means, aes(xintercept=means$mean_dmtr, color=curb), 
             linetype="dashed") +
  facet_wrap(~ neighbourhood_name)

dist_dmtr_nghbr
```

    ## Warning: Use of `means$mean_dmtr` is discouraged. Use `mean_dmtr` instead.

![](MDA-M2_files/figure-markdown_github/unnamed-chunk-7-1.png)

#### Question 4: Age Vs diameter across tree species?

I created a categorical variable named age_cat, based on the age of
trees. This new variable has five categories:
`Youth, Mature, Middle Age, Senior, Ancient`. The reason for creating
this was to first, have better visualization of age against diameter as
scatter plot for this question was not that informative, and second,
investigate how diameter is distributed in each period of age.

``` r
van_tree_catAge <- vancouver_trees %>%
  # calculating age of trees with Sys.Date() and date_planted variable
  mutate(age = as.numeric(substring(Sys.Date(), 1, 4))-
                 as.numeric(substring(date_planted, 1, 4))) %>%
  # creating categorical variable based on age!
  mutate(age_cat = cut(age, 
                breaks = c(0, 10, 20, 30, 40, Inf), 
                labels = c("Youth","Mature","Middle Age","Senior","Ancient"))) %>%
  select(tree_id, age, age_cat, everything())

head(van_tree_catAge)
```

    ## # A tibble: 6 × 22
    ##   tree_id   age age_cat    civic_number std_street genus_name species_name
    ##     <dbl> <dbl> <fct>             <dbl> <chr>      <chr>      <chr>       
    ## 1  149556    22 Middle Age          494 W 58TH AV  ULMUS      AMERICANA   
    ## 2  149563    25 Middle Age          450 W 58TH AV  ZELKOVA    SERRATA     
    ## 3  149579    28 Middle Age         4994 WINDSOR ST STYRAX     JAPONICA    
    ## 4  149590    25 Middle Age          858 E 39TH AV  FRAXINUS   AMERICANA   
    ## 5  149604    28 Middle Age         5032 WINDSOR ST ACER       CAMPESTRE   
    ## 6  149616    NA <NA>                585 W 61ST AV  PYRUS      CALLERYANA  
    ## # … with 15 more variables: cultivar_name <chr>, common_name <chr>,
    ## #   assigned <chr>, root_barrier <chr>, plant_area <chr>,
    ## #   on_street_block <dbl>, on_street <chr>, neighbourhood_name <chr>,
    ## #   street_side_name <chr>, height_range_id <dbl>, diameter <dbl>, curb <chr>,
    ## #   date_planted <date>, longitude <dbl>, latitude <dbl>

For the top 3 tree species that we have the most data on, I tried to
explore how statistics of diameters change among the period of ages. I
plotted a box plot of diameters for each age group and compared them. As
you can see, for each species, the median, upper and lower quantile
grows as we go forward in age periods, meaning that there is a positive
correlation between diameter and age of trees! Since we did not have any
data in senior ages for FRAXINUS, its box plot looks like this.

``` r
age_diamtr_rel <- van_tree_catAge %>%
  # filtering top 3 families of trees
  filter(genus_name %in% top_genus$genus_name) %>%
  # from the previous milestone, we have very few data points with diameter > 75
  filter(0 < diameter & diameter < 75) %>%
  # removing NA values
  filter(!is.na(diameter) & !is.na(age)) %>%
  ggplot(aes(age_cat,  diameter)) +
  geom_boxplot(aes(fill=age_cat)) +
  theme(axis.text.x = element_text(color = "black", size=11, angle=30, vjust=.8, hjust=0.8)) +
  scale_y_log10() +
  xlab("Age Categories") +
  ylab("Diameter (log)") +
  facet_wrap(~ genus_name)

age_diamtr_rel
```

![](MDA-M2_files/figure-markdown_github/unnamed-chunk-9-1.png)

<!----------------------------------------------------------------------------->

### 1.3 (2.5 points)

Based on the operations that you’ve completed, how much closer are you
to answering your research questions? Think about what aspects of your
research questions remain unclear. Can your research questions be
refined, now that you’ve investigated your data a bit more? Which
research questions are yielding interesting results?

**Answer:**

<!------------------------- Write your answer here ---------------------------->

-   I think for question 1, so far, I’ve got the background knowledge
    for it, and I need to involve tree species information to get closer
    to the answer. I think adding one layer of tree species to the
    provided plot would reveal much information about the question. But,
    it needs additional work in terms of plotting to convey the results
    better.

-   With the operations we did (statistics of height and distributions),
    we can say there are no specific differences for some types of trees
    like the PRUNUS among neighborhoods (3 modalities). But, I’d like to
    explore in more depth species like ACER. Another thing to consider
    is the amount of data that we have for each species in each
    neighborhood. For example, if only we had a handful of trees in an
    area, it would not be statistically significant to draw a conclusion
    based on those numbers of trees. That is why I tried to narrow my
    focus to tree species that we have more data on (ACER, PRUNUS, and
    Fraxinus).

-   For question 3, we have an unbalanced number of observations for
    each curb class, which makes it hard to have accurate analysis.
    There are differences in diameter distributions and their means for
    each type of curb in each neighborhood, but they are not
    significant. Also, I think we should consider the age of trees and
    only compare trees within the same age in further analysis. Because
    older trees tend to have bigger diameters and this fact has made our
    analysis biased.

-   Based on the provided box plot, we’re confident to say that there is
    a positive correlation between the age of trees and their diameters
    for the explored tree species. But what I am curious about is to fit
    a linear model on age versus diameter scatter-plot and see how
    coefficients differ from one to another for tree species and which
    one’s diameter grows more when it gets older.

<!----------------------------------------------------------------------------->

# Task 2: Tidy your data (12.5 points)

In this task, we will do several exercises to reshape our data. The goal
here is to understand how to do this reshaping with the `tidyr` package.

A reminder of the definition of *tidy* data:

-   Each row is an **observation**
-   Each column is a **variable**
-   Each cell is a **value**

*Tidy’ing* data is sometimes necessary because it can simplify
computation. Other times it can be nice to organize data so that it can
be easier to understand when read manually.

### 2.1 (2.5 points)

Based on the definition above, can you identify if your data is tidy or
untidy? Go through all your columns, or if you have \>8 variables, just
pick 8, and explain whether the data is untidy or tidy.

<!--------------------------- Start your work below --------------------------->

**Answer:**

Let’s start with looking at the data again and see if it is satisfied
with our criteria of tidy data.

``` r
glimpse(vancouver_trees)
```

    ## Rows: 146,611
    ## Columns: 20
    ## $ tree_id            <dbl> 149556, 149563, 149579, 149590, 149604, 149616, 149…
    ## $ civic_number       <dbl> 494, 450, 4994, 858, 5032, 585, 4909, 4925, 4969, 7…
    ## $ std_street         <chr> "W 58TH AV", "W 58TH AV", "WINDSOR ST", "E 39TH AV"…
    ## $ genus_name         <chr> "ULMUS", "ZELKOVA", "STYRAX", "FRAXINUS", "ACER", "…
    ## $ species_name       <chr> "AMERICANA", "SERRATA", "JAPONICA", "AMERICANA", "C…
    ## $ cultivar_name      <chr> "BRANDON", NA, NA, "AUTUMN APPLAUSE", NA, "CHANTICL…
    ## $ common_name        <chr> "BRANDON ELM", "JAPANESE ZELKOVA", "JAPANESE SNOWBE…
    ## $ assigned           <chr> "N", "N", "N", "Y", "N", "N", "N", "N", "N", "N", "…
    ## $ root_barrier       <chr> "N", "N", "N", "N", "N", "N", "N", "N", "N", "N", "…
    ## $ plant_area         <chr> "N", "N", "4", "4", "4", "B", "6", "6", "3", "3", "…
    ## $ on_street_block    <dbl> 400, 400, 4900, 800, 5000, 500, 4900, 4900, 4900, 7…
    ## $ on_street          <chr> "W 58TH AV", "W 58TH AV", "WINDSOR ST", "E 39TH AV"…
    ## $ neighbourhood_name <chr> "MARPOLE", "MARPOLE", "KENSINGTON-CEDAR COTTAGE", "…
    ## $ street_side_name   <chr> "EVEN", "EVEN", "EVEN", "EVEN", "EVEN", "ODD", "ODD…
    ## $ height_range_id    <dbl> 2, 4, 3, 4, 2, 2, 3, 3, 2, 2, 2, 5, 3, 2, 2, 2, 2, …
    ## $ diameter           <dbl> 10.00, 10.00, 4.00, 18.00, 9.00, 5.00, 15.00, 14.00…
    ## $ curb               <chr> "N", "N", "Y", "Y", "Y", "Y", "Y", "Y", "Y", "Y", "…
    ## $ date_planted       <date> 1999-01-13, 1996-05-31, 1993-11-22, 1996-04-29, 19…
    ## $ longitude          <dbl> -123.1161, -123.1147, -123.0846, -123.0870, -123.08…
    ## $ latitude           <dbl> 49.21776, 49.21776, 49.23938, 49.23469, 49.23894, 4…

As we can see, each row is an observation unit of trees, and each
variable is a record from them. As far as my research questions were, we
didn’t need any re-formatting (wide to long and vice versa) to make our
analysis more straightforward and more efficient since we could put the
names of our variables to the axis of plots. Another criteria that the
dataset meets is that each cell represents a value. So, our data is
indeed tidy.

<!----------------------------------------------------------------------------->

### 2.2 (5 points)

Now, if your data is tidy, untidy it! Then, tidy it back to it’s
original state.

If your data is untidy, then tidy it! Then, untidy it back to it’s
original state.

Be sure to explain your reasoning for this task. Show us the “before”
and “after”.

<!--------------------------- Start your work below --------------------------->

**Answer:**

First, start with untidy-ing data. For this purpose, I combine
`diameter` and `height_range_id` variables as `dimension_name` and their
values to `dimension_value` to make a long data version. As shown below,
now the dimension_name variable is no longer a record of our data and
doesn’t represent a value, making it untidy! With the below dataset,
answering questions 3 and 4 would be hard.

``` r
untidey_data <- vancouver_trees %>%
  pivot_longer(
    cols = c(diameter, height_range_id),
    names_to = "dimension_name",
    values_to = "dimension_value"
  ) %>%
  select(tree_id, dimension_name, dimension_value, everything())

head(untidey_data)
```

    ## # A tibble: 6 × 20
    ##   tree_id dimension_name  dimension_value civic_number std_street genus_name
    ##     <dbl> <chr>                     <dbl>        <dbl> <chr>      <chr>     
    ## 1  149556 diameter                     10          494 W 58TH AV  ULMUS     
    ## 2  149556 height_range_id               2          494 W 58TH AV  ULMUS     
    ## 3  149563 diameter                     10          450 W 58TH AV  ZELKOVA   
    ## 4  149563 height_range_id               4          450 W 58TH AV  ZELKOVA   
    ## 5  149579 diameter                      4         4994 WINDSOR ST STYRAX    
    ## 6  149579 height_range_id               3         4994 WINDSOR ST STYRAX    
    ## # … with 14 more variables: species_name <chr>, cultivar_name <chr>,
    ## #   common_name <chr>, assigned <chr>, root_barrier <chr>, plant_area <chr>,
    ## #   on_street_block <dbl>, on_street <chr>, neighbourhood_name <chr>,
    ## #   street_side_name <chr>, curb <chr>, date_planted <date>, longitude <dbl>,
    ## #   latitude <dbl>

Now let’s try to make the data back to tidy again!

``` r
tidy_data <- untidey_data %>%
  pivot_wider(
    id_cols = c(-dimension_name, -dimension_value),
    names_from = dimension_name,
    values_from = dimension_value) %>%
  select(tree_id, diameter, height_range_id, everything())
head(tidy_data)
```

    ## # A tibble: 6 × 20
    ##   tree_id diameter height_range_id civic_number std_street genus_name
    ##     <dbl>    <dbl>           <dbl>        <dbl> <chr>      <chr>     
    ## 1  149556       10               2          494 W 58TH AV  ULMUS     
    ## 2  149563       10               4          450 W 58TH AV  ZELKOVA   
    ## 3  149579        4               3         4994 WINDSOR ST STYRAX    
    ## 4  149590       18               4          858 E 39TH AV  FRAXINUS  
    ## 5  149604        9               2         5032 WINDSOR ST ACER      
    ## 6  149616        5               2          585 W 61ST AV  PYRUS     
    ## # … with 14 more variables: species_name <chr>, cultivar_name <chr>,
    ## #   common_name <chr>, assigned <chr>, root_barrier <chr>, plant_area <chr>,
    ## #   on_street_block <dbl>, on_street <chr>, neighbourhood_name <chr>,
    ## #   street_side_name <chr>, curb <chr>, date_planted <date>, longitude <dbl>,
    ## #   latitude <dbl>

Our data is tidy again!

<!----------------------------------------------------------------------------->

### 2.3 (5 points)

Now, you should be more familiar with your data, and also have made
progress in answering your research questions. Based on your interest,
and your analyses, pick 2 of the 4 research questions to continue your
analysis in milestone 3, and explain your decision.

Try to choose a version of your data that you think will be appropriate
to answer these 2 questions in milestone 3. Use between 4 and 8
functions that we’ve covered so far (i.e. by filtering, cleaning,
tidy’ing, dropping irrelvant columns, etc.).

<!--------------------------- Start your work below --------------------------->

**Answer:**

Based on what I have done in research questions, I would like to
continue my work on question 1 (trends in cultivated tree species in
each neighborhood past five years) and question 4 (Age versus diameter
across tree species). I think these questions are more likely to yield
correct and interesting answers. Unbalanced classes of data in the two
other questions are barriers to producing concrete results. Also, we
somewhat reached the answers we wanted based on the summarized
statistics and plots. Besides, I am excited to explore the relation
between diameter and age for each species in question 4 by fitting
linear models and comparing the coefficients. For question 1, I am
curious about how to plot the information in a way to see trends in each
neighborhood.

``` r
vancouver_trees_v2 <- vancouver_trees %>%
  # Only selecting the variables we need for the next milestone
  select(tree_id, genus_name, neighbourhood_name, diameter, date_planted) %>%
  # Filtering outlier diameters
  filter(diameter > 0 & diameter < 75) %>%
  # Removing NA records
  filter(!is.na(diameter) & !is.na(date_planted) & !is.na(diameter)) %>%
  # Creating year_planted based on date_planted
  mutate(year_planted = as.numeric(substring(date_planted, 1, 4))) %>%
  # Calculating age of each tree
  mutate(age = as.numeric(substring(Sys.Date(), 1, 4)) - year_planted) %>%
  # Removing date_planted variable
  select(tree_id, genus_name, neighbourhood_name, diameter, year_planted, age)%>%
  group_by(neighbourhood_name, year_planted) %>%
  # Counting number of trees in each neighborhood as we need those numbers of question number 1
  mutate(n_nghbr = n()) %>%
  ungroup()
  
  
head(vancouver_trees_v2)
```

    ## # A tibble: 6 × 7
    ##   tree_id genus_name neighbourhood_name       diameter year_planted   age n_nghbr
    ##     <dbl> <chr>      <chr>                       <dbl>        <dbl> <dbl>   <int>
    ## 1  149556 ULMUS      MARPOLE                        10         1999    22     161
    ## 2  149563 ZELKOVA    MARPOLE                        10         1996    25     152
    ## 3  149579 STYRAX     KENSINGTON-CEDAR COTTAGE        4         1993    28     302
    ## 4  149590 FRAXINUS   KENSINGTON-CEDAR COTTAGE       18         1996    25     368
    ## 5  149604 ACER       KENSINGTON-CEDAR COTTAGE        9         1993    28     302
    ## 6  149617 ACER       KENSINGTON-CEDAR COTTAGE       15         1993    28     302

<!----------------------------------------------------------------------------->

*When you are done, knit an `md` file. This is what we will mark! Make
sure to open it and check that everything has knitted correctly before
submitting your tagged release.*

### Attribution

Thanks to Victor Yuan for mostly putting this together.
