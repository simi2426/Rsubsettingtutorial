# Rsubsettingtutorial
How to subset datasets in R!

Subsetting data in R allows you to select specific variables in a dataset. We can subset data based on variable, column, and many other things.

## Import Dataset
For this example, I am going to use the collegedata.csv, which you can download with this Dropbox link: https://www.dropbox.com/s/vqu6djotrt2q7ur/collegedata.csv?dl=0

``` r
college = read.csv("~/downloads/collegedata.csv")
```

## Subsetting by a Factor
Say we want to subset this dataset by creating different variables that only include datapoints from students in the first college tier, the second college tier, etc. To do that, we use the `subset()` function and assign it a name

```r
# lets subset all datapoints from the first college tier
tierone = subset(college, collegetier == "1")

# lets subset all datapoints from the second college tier
tiertwo = subset(college, collegetier == "2")

# view new dataframes in sheet format
View(tierone)
View(tiertwo)
```
Here, we have created two variables that only include datapoints that have a college tier one or two, respectively. This can be useful in trying to dial in more specifically within a variable.

`View(tiertwo)` this allows us to see the new dataset we have created in a sheet format, similar to an Excel file. R is case sensitive, so make sure you capitalize `View()` or else it will return a different, unrelated argument.

## Assigning multiple rows to a singular subset argument

We want to subset the data by college tier so that instead of using multiple variables, therefore multiple functions,
we can subset the college tiers into bigger groups and use those variables.

We can group the tiers from 1-3, 4-6, 7-9, and 14

The collegetier column is already sorted numerically, so we can just choose the columns
we want by pulling those and assigning them a name

``` r
one = college[c(2:103), c(1:7)]
two = college[c(104:1133), c(1:7)]
three = college[c(1134:2004), c(1:7)]
four = college[c(2005:2203), c(1:7)]

```


ARGUMENT MEANINGS:

`one` `two` `three` `four` = the names we have assigned the variables

`college` the dataset we are pulling the numbers from

`c(2:103), c(2:7)` = We want all numbers from 1-3, so we can scroll through
the Excel sheet to find when those numbers end and begin. We want to contain the
data from columns 1 - 7 into the variable, so we keep that in the new variable we make.

If your data is not sorted, you can sort it in Excel, or we can use another way:

``` r
tierone = subset(college, collegetier <= 3 | kmean <= 90000)

```
Now, `tierone` contains all colleges in tiers from 1-3, and from those rows, 
only contains observations where the mean income of the child is equal to 
or less than $90,0000. We can use `<=` `==` `>=` for other functions as well. There are many
ways to subset data, and even packages, such as the `dplyr` package, which allows for more
intuitive ways to subset rather than what is included in the base R package.

## METHOD 2
There is another method to doing this, which is useful if we are only drawing a singular factor out of the dataset, such as only wanting to pull rows that include a college tier of 2, or 4.

```r
only2 = college[college$collegetier == 2, ]
view(only2)

# draw data with college tier of 4
only4 = college[college$collegetier == 4, ]
view(only4)
head(only4)
```

Here, we have created two new subsets that only include data from college tiers 2 and 4. 

## < or >
Say we want to extract all datapoints that contain parents whose mean income is greater than $450,000. To do that, we use the `subset()` function:

```r
greaterthan = subset(college, pmean > 450000)
view(greaterthan)
```
Now, we have a variable that contains only datapoints where the mean parental income is greater than $450,000.

What if we want to know where the parental mean income is greater than $450,000, but we also want to know where the child's income mean is greater than $20,000? We can simply add `&` into the function to do this:

```r
write = subset(college, pmean >= 450000 & kmean > 20000)
```

Now, `write` will only include datapoints where the mean parental income is greater than $450,000 and the child's mean income is greater than $20,000.

## Random Sampling
Say we want to take n random samples from this large dataset. To do that, we use the `sample_n()` function. Let's take 10 random rows from the `college` dataset:

``` r
tenrandom = college %>% sample_n(10, replace = FALSE)
view(tenrandom)
```

`tenrandom` is the name we have assigned our subset of ten random rows.

`replace = FALSE` ensures that there is no replacement in our sample

`view(random)` allows us to view our new subset like an Excel file, in sheet form.

## Subsetting Columns
Say we want to create a dataset without some of the columns included in the main dataset. For this example, let's say we only wanted to include the rows that include the college tier and quintiles. To do that, we use brackets. First, we want to find the numbers that correspond to our row names:

```r
> names(college)
[1] "collegetier" "pmean"       "kmean"      
[4] "kq1"         "kq2"         "kq3"        
[7] "kq4"         "kq5" 
```

Now, we can see the numbers that correspond to our row names. For this example, we want the college tier and quintiles, so we need to extract 1, 4, 5, 6, 7, 8. We can do this in two ways, by naming each number, or using `:`:

```r
onlyquintiles = college[, c(1, 4, 5, 6, 7, 8)]
view(onlyquintiles)
# or
quintiles = college[, c(1, 4:8)]
view(quintiles)
```

Both of these options do the same thing, select specific columns from the dataset.



CITATIONS:

Much of the info in this tutorial was taken from UCLA Statistical Consulting, found at https://stats.idre.ucla.edu/r/modules/subsetting-data/






