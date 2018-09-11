The Chapter Wrangling data teaches how to transform and analyze data.
We’ll be using the dplyr and tidyr packages.
The first class is about using dplyr library.
There’re 5 basic verbs, filter(),
select().
arrange()
mutate()
summarize() group_by()

We use source(“import_murders.R") to run the whole R script file. 
Use nrow() to see how big the data frame is.
command + enter to run 
Use glimpse() to let console give you a list of all the variables of the data set and the data type. 
Filter works by extracting rows that meet a criteria we set.
The function is filter(), the first argument we pass to is the name of the data frame, after that we can pass one or more logical tests, like
df1 <- filter(murders, Relationship_label=="Husband", VicAge > 60, Year==2016)
It means we create a new data frame called df1, and we filter by relationship label, looking for husbands, and then victim age is older than 60, comma means and, and year is 2016.

pic1

We can also replace the comma with &.
We can use | (or) command, or we can use %in% to show group membership.
df3 <- filter(murders, Relationship_label %in% c("Husband", "Boyfriend") | Circumstance_label=="Lovers triangle”)
This matches the relationship to victims is either husband or boyfriend, or with lovers triangle. 

extract variables or columns with select().
We need to list the column names we’d like to pull from. And avoid the space between column names.
df1_narrow <- select(df1, State, Agency, Solved_label, Year)
We can use a colon between variables if we want all the columns between.
We can also use a - to remove a column.

pic2

arrange()is like sorting, we put the name of the data set in it, followed by the column names to sort by.
age_df2 <- arrange(murders, VicAge, OffAge)

We can create new columns with mutate().
We give the name of a new variable or function to specific argument we want to use. We can do math in the mutate, +-*/^ 
murders_ver2 <- mutate(murders,
                       age_difference=OffAge-VicAge)
We can use case_when() in mutate() to create new values, like if_else. 

pic3

We use rename() to rename columns, use = to replace the names. select() also can be used to rename.

summarize() is like creating a pivot table in excel.
We can use group_by() to aggregate data by groups.

pic4


pipe %>%
Data analyses will most often involve more than one step. We can pass this argument to do the extract rows, group, count and arrange by nesting functions.
dc_annual_murders <- arrange(summarize(group_by(filter(murders, State=="District of Columbia"), Year), total=n()), desc(total))

We use pipe operator %>% because in dplyr the first argument is always the data frame. So in order to avoid much typing, we can put data frame name right before %>%, similar to “ and then”.
murders %>% filter(OffAge==2) do the same thing as filter(murders, OffAge==2).

pic5

There’re some summary functions. mean() for average, median() for median, sd() for standard deviation, range(), min() for minimum, max() for maximum, abs() for absolute value.


Class 2 teaches to use tidyr to tidy and join data.
We use DT library to help us show the data.

pic6  pic7

We use spread() in tidyr library to turn tall data into wide data with spread. The argument is like data frame %>% spread (key = data, value= amount). spread() can can only turn one tall column wide at a time.We need to drop the cases_unsolved column in order for this to transpose correctly.



pic 9

Joining combines two data sets by adding their columns together. It uses dplyr package. There’s left_join(), right_join(), full_join() (any rows from the second data frame that doesn’t match the target data frame are kept), and inner_join() (any rows that don’t match are dropped completely from both data sets.)


Case Study

pic10 pic11

Handling Strings

We need stringr package to manipulate text. Each function starts with str_
We use str_length() to figure out length of string; str_c() to combine strings; str_sub() to substitute string, str_detect() to detect string in string, str_match() to do string match, str_count() to count string, str_split() to split string, str_to_upper()/str_to_lower()/str_to_title() to convert string to certain case, str_trim() to eliminate trailing white space. 

pic12 pic13


Dealing with dates
If we need to convert characters dates into a date variable, we’ll use lubridate package.
mdy()stands for month-date-year. And year, day, month for ydm(), hour, minute for hm(), hour minute, second for hms(), year, month, day, hour, minute, second for ymd_hms().

pic14

ggplot2
This chapter is about applying the grammar of graphics to data visualizations
First we need to use library readr, ggplot2.
ggplot(data=ages) +
  geom_point(mapping=aes(x=actor_age, y=actress_age)) + 
  expand_limits(x = 0, y = 0) +
  geom_abline(intercept=0, col="light gray") 
The aesthetics (aes()) is the visual characteristics that represent your data.
And geom_point() is one type out of dozens of possible geom_functions, like geom_bar() or geom_boxplot().
expand_limits() is passed to force the x- and y-axis to start at 0.
pic15

ggplot(data=ages,
  aes(x=actor, fill=Genre)) +
  geom_bar()

pic16

ggplot(ages, aes(x=actor, y=actress_age)) +
  geom_boxplot()

pic17,18
box plot/ violin plot


kernel density plot
geom_density()

pic19

line plot pic20

scatterplot pic21

facets pic22

customizing charts
We use forcats library to transform data. And pass the function fct_reorder(factor, variable, fun=mean, …, desc=FALSE)
pic23

We use geom_segment() to create lollipop plot.
pic24
