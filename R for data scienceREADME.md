# Learningblog_R_datascience
My learning notes of R(data science)
R for Data Science

Data visualisation with ggplot2:

The layered grammar of graphics: 

ggplot(<DATA>, aes(<MAPPING>)) +
<GEOM_FUNCTION>(
stat = <STAT>
position = <POSITION>
) +
<COORDINARE_FUNCTION> +
<FACET_FUNCTION>
￼
1. DATA
    * prerequisite:
		library(tidyverse)
		ggplot2:: data

2. AESTHETIC MAPPING
    * A graphing template 
		ggplot(<DATA>, aes(<MAPPING>))

    * Map/ add variables to aesthetics 
		size: control the size of the points based on specific value
		shape:  control the shapes of the points (max=6) baed on specific value
		color:  control the outline color of the points baed on specific value
		fill:  control the inside color of the points baed on specific value
		alpha:  control the transparency of the points baed on specific value
		linetype: control different livetype of the line baed on specific value
		group: group aesthetic to a categorial variable to draw multiple objects.
		ggplot(data, aes(x-axis, y-axis, size/ shape/ color/ fill = xx ))
		Example:
		mpg <- data
		ggplot(mpg, aes(displ, hwy, color= class)) + 
		geom_point()

    * To set aesthetic manually, for example, if we want to make all of the points in our plot the same color
		# Here color doesn’t convey information about a variable, but only change the appearance of the plot
		ggplot(data, aes(x, y))+
		geom_point(color = “blue”)
		
3. GEOMETRIC OBJECTS
# Geometrical objects used to represent data called geom. For example, 
geom_bar(): bar charts use bar geoms, 
geom_smooth() : line charts use line geoms, 
geom_boxplot(): boxplots use boxplot geoms, 
geom_point(): Scatterplots break the trend; they use the point geom. 
Different geoms can be used to plot the same data. 

    * A graphing template 
		ggplot(<DATA>, aes(<MAPPING>)) + 
		<GEOM_FUNCTION>()
		Example:
		mpg <- data
		ggplot(mpg, aes(displ, hwy, color= arv)) + 
		geom_point()+
		geom_smooth(aes(linetype=drv))

4. STATISTICAL TRANSFORMATIONS(STAT)
# The algorithm used to calculate new values for a graph is called a stat, short for statistical transformation. For example:
ggplot(diamonds, aes(cut)) +
geom_bar()
On the x-axis, the chart displays cut, a ‘variable from diamonds. On the y-axis, it displays count, but count is not a variable in diamonds! Where does count come from? Many graphs, like scatterplots, plot the raw values of your dataset. Other graphs, like bar charts, calculate new values to plot:
* Bar charts, histograms, and frequency polygons bin your data and then plot bin counts, the number of points that fall in each bin.
* Smoothers fit a model to your data and then plot predictions from the model.
* Boxplots compute a robust summary of the distribution and display a specially formatted box.

    * A graphing template 
		ggplot(<DATA>, aes(<MAPPING>)) +
		<GEOM_FUNCTION>(
		stat = <STAT>
		)

    * stat= “count”; stat=“identity”	
	# Every geom has a default stat, and every stat has a default geom
		geom_bar() uses stat_count() by default: it counts the number of cases at each x position. 
		geom_col() uses stat_identity(): it leaves the data as is.
	Example: 
		ggplot(data, aes(x, y ))+
		geom_bar(stat = “identity”)

    * A bar chart of proportion 
		# you might want to display a bar chart of proportion, rather than count:
		ggplot(diamonds, aes(cut, y=..prop.., group=1))+
		geom_bar()

    * stat_summary() summarize the y values for each unique x value
		diamonds%>%
		  ggplot(aes(cut, depth))+
 		 stat_summary(
   		 fun.min = min,
   		 fun.max = max,
   		 fun = median
  		)


5. POSITION ADJUSTMENT
# If we apply 
ggplot(diamonds, aes(cut, fill=clarity)) +
geom_bar()
we will find that the bars are automatically stacked. 
The stacking is performed automatically by the position adjustment specified by the position argument. If you don’t want a stacked bar chart, you can use one of three other options: "identity", "dodge" or "fill":

    * A graphing template 
		ggplot(<DATA>, aes(<MAPPING>)) +
		<GEOM_FUNCTION>(
		stat = <STAT>
		position = <POSITION>
		)

    * position = "identity" 
	# position = "identity" will place each object exactly where it falls in the context of the graph. “This is not very useful for bars, because it overlaps them, but is more useful for 2D geoms, like points, where it is the default.
Example: 
ggplot(diamonds, aes(cut, color=clarity)) +
geom_bar(position = “identity”, fill=NA)
 
    * position = "fill" 
	# position = "fill"  works like stacking, but makes each set of stacked bars the same height. This makes it easier to compare proportions across groups:
	Example: 
	ggplot(diamonds, aes(cut, fill=clarity))+
	geom_bar(position = “fill”)

    * position = "dodge" 
# position = "dodge" places overlapping objects directly beside one another. This makes it easier to compare individual values:
	Example: 
	ggplot(diamonds, aes(cut, fill=clarity))+
	geom_bar(position = “dodge”)

    * position = “jitter” 
	# position = “jitter” is not useful for bar chart but can be very useful for scatterplots, especially the over 		plotting problems. “position = "jitter" adds a small amount of random noise to each point. This spreads 		the  points out because no two points are likely to receive the same amount of random noise:
	Example 
	ggplot(mpg, aes(displ, hwy))+
	geom_point(position = “jitter”)
	# Adding randomness seems like a strange way to improve your plot, but while it makes your graph less 		accurate at small scales, it makes your graph more revealing at large scales.

6. COORDINATE SYSTEM
# The default coordinate system is the Cartesian coordinate system where the x and y position act independently to find the location of each point. 

    * A graphing template 
		ggplot(<DATA>, aes(<MAPPING>)) +
		<GEOM_FUNCTION>(
		stat = <STAT>
		position = <POSITION>) +
		<COORDINARE_FUNCTION>

    * coord_flip() 
# coord_flip() switches the x- and y-axes. This is useful (for example) if you want horizontal boxplots. It’s also useful for long labels—it’s hard to get them to fit without overlapping on the x-axis:
	Example: 
	ggplot(mpg, aes(class, hwy))+
	geom_boxplot()+
	coord_flip()

    * coord_quickmap()
	# coord_quickmap() sets the aspect ratio correctly for maps. This is very important if you’re plotting 			spatial data with ggplot2
	Example: 
	ggplot(map_data, aes(long, lat, group=group))+
	geom_polygon(fill = “white”, color = “black”)+
	coord_quickmap()

    * coord_polar()
	# coord_polar() uses polar coordinates. Polar coordinates reveal an interesting connection between a bar 	chart and a Coxcomb chart:
	Example: 
	bar <- ggplot(diamonds, aes(cut, fill = cut)) +
			geom_bar(show.legend= FALSE, width = 1) +
			theme(aspect.ratio=1)+
			labs(x=NULL, y=NULL)
	bar + coord_polar()
	bar + coord_flip()

7. FACETS
# Apart from aesthetics, <FACET_FUNCTION> is another way to add additional variables, particularly useful for categorical variables, by spliting your plot into facets, subplots that each display one subset of the data.

    * A graphing template 
		ggplot(<DATA>, aes(<MAPPING>)) +
		<GEOM_FUNCTION>(
		stat = <STAT>
		position = <POSITION>) +
		<COORDINARE_FUNCTION> +
		<FACET_FUNCTION>

    * To facet plot by a single variable: facet_wrap() 
# The first argument of facet_wrap() should be a formula, which you create with ~ followed by a variable name (here “formula” is the name of a data structure in R, not a synonym for “equation”)
		ggplot(mpg, aes(displ, hwy)) +
		geom_point() +
		facet_wrap(~ class, nrow=2)
# nrow, ncol represent the number of the rows and columns 

    * To facet plot by a combination of two variables: facet_grid()
		ggplot(mpg, aes(displ, hwy)) +
		geom_point() +
		facet_grid(dry~ cly)
	# If you prefer to not facet in the rows or columns dimension, use a . instead of a variable name, 
		ggplot(mpg, aes(displ, hwy)) +
		geom_point() +
		facet_grid(. ~ cly)


Data Transformation with dpltr 

Prerequisites: 
dplyr package is another core member of the tidyverse. 
library(nycflights13)
library(tidyverse)
nycflights13:flights
# to see the whole dataset of flights, we can run view()

Abbreviation: 
* int stands for integers.
* dbl stands for doubles, or real numbers.
* chr stands for character vectors, or strings.
* dttm stands for date-times (a date + a time).
* lgl stands for logical, vectors that contain only TRUE or FALSE.
* fctr stands for factors, which R uses to represent categorical variables with fixed possible values.
* date stands for dates.

Five key dplyr functions: 
* Pick observations by their values (filter()).
* Reorder the rows (arrange()).
* Pick variables by their names (select()).
* Create new variables with functions of existing variables (mutate()).
* Collapse many values down to a single summary (summarize()).
# Above five can all be used in conjunction with group_by(), which changes the scope of each function from operating on the entire dataset to operating on it group-by-group. 

# These six functions provide the verbs for a language of data manipulation, which works similarly:
1. The first argument is a data frame.
2. The subsequent arguments describe what to do with the data frame, using the variable names (without quotes).
3. The result is a new data frame.
# Together these properties make it easy to chain together multiple simple steps to achieve a complex result.
Example: 	flights%>%
			group_by(year, month, day)%>%
			filter()%>%
			summarize()%>%
			select()%>%
			mutate()%>%
			arrange()%>%
			ggplot()

Filter rows with filter():

1. Basics: 
filter() allows you to subset observations based on their values.
# Example: to select all flights on 1 Jan:
jan_1 <- flights%>%
filter(month == 1, day ==1)
# R either prints out the results, or saves them to a variable. If you want to do both, you can wrap the assignment in parentheses:
(jan_1 <- flights%>%
filter(month == 1, day ==1))

2. Comparison: 
R provides the standard suite: >, >=, <, <=, != (not equal), and == (equal).
Computer use finite precision arithmetic (they can not store an infinite number of digits), thus every number we see is an approximation. Thus using ==: 
sqrt(2)^2 == 2 
# > [1] FALSE
Whereas, using near(): 
near(sqrt(2)^2, 2)
# >[1] TRUE

3. Logical operation:
Boolean operators : & is “and,” | is “or,” and ! is “not.”
 Complete set of Boolean operation:
￼

Two methods to find all flights that departed in Nov or Dec:
First one: 
flights%>%
filter(month == 11| month == 12)
Second one: 
flights%>%
filter(month %in% c(11, 12))

De Morgan’s law: !(x & y) is the same as !x | !y, and !(x | y) is the same as !x & !y.
Example: if you wanted to find flights that weren’t delayed (on arrival or departure) by more than two hours, you could use either of the following two filters:

flights%>%
filter(arr_delay <120, dep_delay <120)

flights%>%
filter(!(arr_delay>120 | dep_arrive >120))

4. Missing value:
NA represents an unknown value, missing values are “contagious” and almost any operation involving an unknown value will also be unknown. 

is.na() is used to determine whether a value is missing.
Example: is.na(x)

filter() only includes rows where the condition is TRUE; it excludes both FALSE and NA values. If you want to preserve missing values, ask for them explicitly:
df<- tibble(x= c(1, NA, 3))
df%>%
filter(x>1 | is.na(x))

5.  between()
for x >= left & x <= right
Usage: 
between(x, left, right)
Arguments
x - A numeric vector of values
left, right - Boundary values (must be scalars).
Example: 
filter(starwars, between(height, 100, 150))

Arrange rows with arrange()

1. Basic 
arrange() changes the order of rows, which requires a data frame and a set of column names. 
Example: 
flights%>%
arrange(year, month, day)

To record a column in descending order, desc() is used. 
Example: 
flights%>%
arrange(desc(arr_delay))

Missing value are always sorted at the end.

Select columns with select()
select() allows you to rapidly zoom in on a useful subset using operations based on the names of the variables.
Example:
# select columns by name:
flights%>%
select(year, month, day)
# select all columns between year and day(inclusive)
flights%>%
select(year:day)
# select all columns except those from year to day(inclusive)
flights%>%
select(-(year:day))

There are a number of helper functions you can use within select():
* starts_with("abc") matches names that begin with “abc”.
* ends_with("xyz") matches names that end with “xyz”.
* contains("ijk") matches names that contain “ijk”.
* matches("(.)\\1") selects variables that match a regular expression. This one matches any variables that contain repeated characters. 
* num_range("x", 1:3) matches x1, x2, and x3.”

rename(), a variant of select() that keeps all the variables that aren’t explicitly mentioned:
Example: 
flights%>%
rename(tail_num= tailnum) # changed name=unchanged name

everything() is helpful if you have a handful of variables that you want to move to the start of the data frame:
Example: 
flights%>%
select(time_hour, air_time, everything()) # everything must be used within a “selecting” function


Add new variables/ columns with mutate()
mutate() add new columns that are functions of existing columns at the end of dataset.
Example: 
flights()%>%
mutate(gain= arr_delay- dep_delay, speed= distance/ air_time *60)

If you only want to keep the new variables, use transmute():
Example: 
flights%>%
transmute(gain= arr_delay- dep_delay, speed= distance/ air_time *60)


Useful Creation Function:

1. Arithmetic operators: +, -, *, /, ^
2. Modular arithmetic: 
    1. %/% : integer division
    2. %% : remainder
	Example: flights%>%
			   transmute(dep_time, hour= dep_time %/% 100, minute = dep_time %% 100)	
3. Logs : log(), log2(), log10()
4. Offset:  lag(), lead()
	Find the "previous" (lag()) or "next" (lead()) values in a vector. Useful for comparing values behind of or 		ahead of the current values.
5. Cumulative and rolling aggregates: cumsum(), cumprod(), cummin(), cummax(), cummean()
6. Logical comparison: <, <=, >, >=, !=
7. Ranking: min_rank(), min_rank(desc()), row_number(), dense_rank(), percent_rank(), cume_dist()


Grouped summaries with summarise():

summarise() collapse a data frame to a single row:
Example: 
flights%>%
summarise(delay= mean(dep_delay, na.rm=TRUE))

summarize() is not terribly useful unless we pair it with group_by(). This changes the unit of analysis from the complete dataset to individual groups. 
Example: 
flights%>%
group_by(year, month, day)%>%
summarize(delay= mean(dep_delay, na.rm=TRUE))


Combining multiple operations with the Pipe:
Imagine that we want to explore the relationship between the distance and average delay for each location. 

flights%>%
group(dest)%>%
summarize(n=n(), dist=mean(distance, na.rm=TRUE), delay=(arr_delay, na.rm=TRUE))%>%
filter(n>20, dest != “HNL”)%>%
ggplot(aes(dist, delay))+
geom_point(aes(size=n), alpha=1/3)+
geom_smooth(se=FALSE)


Missing Values:
If there’s any missing value in the input, the output will be a missing value. Fortunately, all aggregation functions have an na.rm argument, which removes the missing values prior to computation:
Example: 
flights%>%
group_by(year, month, day)%>%
summarize(mean= mean(dep_delay, na.rm=TRUE))

we could also tackle the problem by first removing the missing values.
no_miss<- flights%>%
filter(!is.na(dep_delay), !is.na(arr_delay))%>%
group_by(year, month, day)%>%
summarize(mean=mean(dep_delay))


Counts:
Include either a count (n()), or a count of nonmissing values (sum(!is.na(x))) is a good habit which can check that you’re not drawing conclusions based on very small amounts of data. To count the number of distinct (unique) values, use n_distinct(x):
Example: 
Delay<- no_miss%>%
group_by(tailnum)%>%
summarize(delay= mean(arr_delay, na.rm=TRUE), n=n())%>%
filter(n>25)%>%
ggplot(aes(n, delay))+
geom_point(alpha=1/10)

no_miss%>%
group_by(dest)%>%
summarize(carriers= n_distinct(carrier))%>%
arrange(desc(carriers))

Counts are so useful that dplyr provides a simple helper if all you want is a count:
not_cancelled %>%
  count(dest)

You can optionally provide a weight variable. For example, you could use this to “count” (sum) the total number of miles a plane flew:
not_cancelled %>%
  count(tailnum, wt = distance)


Useful summary functions:
mean(x): the sum divided by the length;
median(x): a value where 50% of x is above it, and 50% is below it.
example:
no_miss%>%
group_by(year, month, day)%>%
summarize(n=n(), avg_delay1=mean(arr_delay[arr_delay>0]), avg_delay2=mean(arr_delay[arr_delay<0]))

sd(x): squared deviation
IQR(x): The interquartile range 
mad(x): median absolute deviation mad(x) which may be more useful if you have outliers

min(x)
max(x)
quantile(x, 0.25): Quantiles are a generalization of the median. For example, quantile(x, 0.25) will find a value of x that is greater than 25% of the values, and less than the remaining 75%
example: 
no_miss%>%
group_by(year, month, day)%>%
summarize(n=n(), first=min(dep_time), last=max(dep_time))

Measures of position first(x), nth(x, 2), last(x)
These work similarly to x[1], x[2], and x[length(x)] but let you set a default value if that position does not exist (i.e., you’re trying to get the third element from a group that only has two elements)
example:
no_miss%>%
group_by(year, month, day)%>%
summarize(n=n(), first_dep=first(dep_time), last_dep=last(dep_time))

These functions are complementary to filtering on ranks. Filtering gives you all variables, with each observation in a separate row:
not_cancelled %>%
  group_by(year, month, day) %>%
  mutate(r = min_rank(desc(dep_time))) %>%
  filter(r %in% range(r))

When used with numeric functions, TRUE is converted to 1 and FALSE to 0. This makes sum() and mean() very useful: sum(x) gives the number of TRUEs in x, and mean(x) gives the proportion:”
# How many flights left before 5am? (these usually indicate delayed flights from the previous day)
not_cancelled %>%
  group_by(year, month, day) %>%
  summarize(n_early = sum(dep_time < 500))

# What proportion of flights are delayed by more than an hour?
not_cancelled %>%
  group_by(year, month, day) %>%
  summarize(hour_perc = mean(arr_delay > 60))


Grouping by multiple variables:
When you group by multiple variables, each summary peels off one level of the grouping. 
Example: 
per_day<-flights%>%
group_by(year, month, day)%>%
summatize(flights=n())

(per_month<-summarize(per_day, flights=sum(flights))


Ungrouping
If you need to remove grouping, and return to operations on ungrouped data, use ungroup():
Example: 
per_day%>%
ungroup()%>%
summarize(flights=n())


Exploratory data analysis (EDA):
1. generate questions about your data
2. Search for answers by visualising, transforming and modling your data
3. use what you learn to refine your questions and/or generate new questions

1. What type of variation occurs within my variables?
2. What type of covariation occurs between my variables? 

Categorical variables:
A variable is categorical if it can only take one of a small set of values. In R, categorical variables are usually saved as factors or character vectors. To examine the distribution of a categorical variable, use a bar chart. 

diamonds %>%
ggplot(aes(cut))+
geom_bar()

Or

diamonds%>%
count(cut)

Continuous variables:
A variable is continuous if it can take any of an infinite set of ordered values. Numbers and date-times are two examples of continuous variables. to examine the distribution of a continuous variable, use a histogram.

diamonds%>%
ggplot(aes(carate))+
geom_histogram(binwidth=0.5)

Or 

diamonds%>%
count(cut_width(carat, 0.5))

If you wish to overlay multiple histograms in the same plot, I recommend using geom_freqpoly() instead of geom_histogram(). geom_freqpoly() performs the same calculation as geom_histogram(), but instead of displaying the counts with bars, uses lines instead. It’s much easier to understand overlapping lines than bars:

diamonds%>%
ggplot(aes(carat, color=cut))+
geom_freqpoly(binwidth=0.1)


Unusual values:
coord_cartesian() can zoom in to small values of the x- and y-axis:

diamonds%>%
ggplot(aes(y), binwidth=0.5)+
coord_cartesian(ylim=c(0,50))


Missing values:
# replacing unusual values with missing values(NA)

diamonds%>%
mutate(y=ifelse(y<3|y>20, NA, y))

ifelse()
# ifelse returns a value with the same shape as test which is filled with elements selected from either yes or no depending on whether the element of test is TRUE or FALSE.
#Usage
ifelse(test, yes, no)
#Arguments
test: an object which can be coerced to logical mode.
yes	: return values for true elements of test.
no: return values for false elements of test.

Covariation 
A categorical and continuous variable
1. geom_freqplot()
diamonds%>%
ggplot(aes(price, color=cut))+
geom_freqplot(binwidth=500)

2. geom_boxplot()
diamonds%>%
ggplot(aes(cut,price))+
geom_boxplot()

If categorical variables don’t have an intrinsic order, you can use reorder() to make a more informative display. If you have long variable names, geom_boxplot() will work better if you flip it 90°. You can do that with coord_flip():

mpg%>%
ggplot(aes(x=reorder(class, hwy, FUN= median), y=hwy))+
geom_boxplot()+
coord_flip()

Two categorical variables:
To visualize the covariation between categorical variables, you’ll need to count the number of observations for each combination. One way to do that is to rely on the built-in geom_count():

diamonds%>%
ggplot(aes(cut, color))+
geom_count()

Another approach is to compare the count with dplyr and then visualise with geom_tile() and fill aesthetic:

diamonds%>%
count(color, cut)%>%
ggplot(aes(color, cut, fill=n))+
geom_tile()

Two continuous variables:
1. Using geom_point()

diamonds%>%
geom_point(aes(carat, price, alpha=1/100))+
geom_point()

2. Using geom_bin2d() and geom_hex(), which divide the coordinate plane into 2D bins and then use a fill color to display how many points fill into each bin. geom_bin2d() creates rectangular bins, while geom_hex() creates hexagonal bins.

diamonds%>%
ggplot(aes(carat, price))+
geom_bin2d()


diamonds%>%
ggplot(aes(carat, price))+
geom_hex()

Another option is to bin one continuous variable so it acts like a categorical variable. Then you can use one of the techniques for visualizing the combination of a categorical and a continuous variable that you learned about. 

diamonds%>%
ggplot(aes(carat, price))+
geom_boxplot(aes(group=cut_width(carat,0.1)))

cut_interval() makes n groups with equal range, cut_number() makes n groups with (approximately) equal numbers of observations; cut_width() makes groups of width width.

Chapter 7: Tiblles with tibble
# coerce a data frame to a tibble
as_tibble(iris)

# creat a new tibble from individual vectors with tibble()
tibble(x=1:5, y=1, z=x^2+y)

# tibble may make column names invalid R variable names such as a space, to 
#refer to these variables, surround them with backticks, ``:
tb<- tibble(
  `:)`="smile",
  ` `="space",
  `2000`="number"
  )
tb

#another way to create a tibble is with tribble(),short for transposed tibble.
#the column headings are defined by formulas(i.e., start with ~), and entries 
# are separated by commas.
df<-tribble(
  ~x, ~y, ~z,
  "a", 2, 3.6,
  "b", 1, 8.5
)

#tibbles are designed to output in limited rows in case to overwhelm your console
#if you want more output, you can explicitly use print() to control the number 
#of rows(n) and columns(width). width=Inf will display all columns:
nycflights13::flights%>%
  print(n=10, width=Inf)

# if you want to pull a single variable from data frames, you need $ or [[]], 
#with the former can only extract by name, while the latter can exact by name 
# and position
df$x
df[["x"]]
df[[1]]
#to use these in a pipe, a special placeholder . should be used.
df%>% .$x
df%>% .[["x"]]
df%>% .[[1]]  

# some older functions don't work with tibbles, if you encounter one of these 
#functions, use as.data.frame() to turn a tibble back to a data.frame:
class(as.data.frame(tb))


Chapter 8: data import with reader
# read_csv() is one of the most common forms of data storage
Heights <- read_csv(“data/heights.csv”)

# you can use read_csv() to create reproducible examples, the first line of datan for the column names
read_csv(“a,b,c
			1, 2,3
			4,5,6”)

# you can use skip=n to skip the first n line or use comment = “#” to drop all lines that start with (e.g.)#:
read_csv(“the first line
			the second line
			x, y, z
			1, 2, 3”, skip=2)
Or
read_csv(“# a comment I want to skip
			x, y, z
			1, ,2, 3 “, comment = “#”)

# the data might not have column name, you can use col_names = FALSE to tell read_csv() not to treat the first row as headings, and instead label them sequentially from x1 to xn:
read_csv(“1, 2, 3 \n4, 5, 6” col_names=FALSE)
# \n is a convenient shortcut for adding a new line.

# you can also pass col_names a character vector which will be used as the coumn names:
read_csv(“1, 2, 3\n4,5, 6”, col_names=c(“x”, “y”, “z”))

# you can use na=“” to represent the missing values in your file:
read_csv(“1,2,3\n4,5,.”, na=“.”)

Parsing a vector
parse_*() functions take a character vector and returen a more specialized vector like a logical, integer, or date:
str(parse_logical(c(“TRUE”, “FALSE”)))
str(parse_integer(c(“1”, “2”, “3”)))
str(parse_date(c(“2010-01-01”, “1979-10-14”)))

# na argument specifies which strings should be treated as missing
parse_integer(c(“1”, “2”, “.”, “456”), na=“.”)

# if parsing fails, you will get a warning and the failures will be missing in the output:
parse_integer(c(“123”, “abc”, “345”, “123.45”))

Number
# when people write numbers differently, for example, some use . in between the integer and fractional parts, while others use , 
parse_double(“1.23”)
parse_double(“1,23”, locale=locale(decimal_mark=“,”))

# parse_number() ignores non-numeric characters before and after numbers.
parse_number(“$100”)
parse_number(“It dose $123.45”)

# for grouping character
parse_number(“$123,456,789”)
parse_number(“123.456.789”, locale=locale(grouping_mark=“.”))
parse_number(“123’456’789”, locale=locale(grouping_mark=“‘”))

Strings
# in R, charToRaw() can provide a underlying representation of a string
charToRaw("Hadley")

x1 <- "El Ni\xf1o was particularly bad this year"
x2 <- "\x82\xb1\x82\xf1\x82\xc9\x82\xbf\x82\xcd"
parse_character(x1, locale = locale(encoding = "Latin1"))
parse_character(x2, locale = locale(encoding = "Shift-JIS"))

#To find correct encoding, guess_encoding() helps.
guess_encoding(charToRaw(x1))
guess_encoding(charToRaw(x2))

Factor:
# R uses factors to represent categorical variables that have a known set of possible values. Give parse_factor() a vector of known levels to generate a warning whenever an unexpected value is present:
fruit<-c("apple", "banana")
parse_factor(c("apple", "banana", "banananan”), levels=fruit)

Dates, date-times, and times
# parse_datetime() expects an ISO8601 Date-time which the components of a date are organised from biggest to smallest: year, month, day, hour, minute, second:
parse_datetime(“2010-10-01T2010”)
# if time is omitted, it will be set to midnight
parse_datetime(“20101010”)

# parse_date() expects a four digit year, - or /, then month, - or /, then day
parse_date(“2010-10-01”)

# parse_time() expects the hour, :, minutes, optionally : and seconds, and optionally a.m./ p/m.
library(hms)
parse_time(“01:10 am”)
parse_time(“20:10:01”)

# parse_date 
> parse_date("01/02/15", "%m/%d/%y")
[1] "2015-01-02"
> parse_date("01/02/15", "%y/%m/%d")
[1] "2001-02-15"
> parse_date("01/02/15", "%d/%m/%y")
[1] "2015-02-01"
> parse_date("1 Janvier 2015", "%d %B %Y", locale = locale("fr"))
[1] "2015-01-01"

Parsing a file
Df <- read_csv(“dataset.csv”)
Or
Df<- tribble(
		~x, ~y,
		“1”, “1.21”,
		“2”, “2.32”,
		“3”, “4.56”)

Writing to a file
write_csv(challenge, “challenge.csv”)
# if you want to export a csv file to excel:
write_excel_csv(challenge, “challenge.csv”)

Chapter 9: tidy data with tidyr
gathering:
#a common problem is a dataset where some of the column names are not names of variable, but values of a variable. To tidy these datasets, we need to gather those columns into a new pair of variable.
# three parameters are needed:1) the set of columns that represent values, not variables; 2)the name of the variable whose values form the column names(key);3)the name of the variable whose values are spread over the cells(value).
Tidy4a<-table4a%>%
gather(“1999”, “2000”, key=“year”, value=“cases”)
Tidy4b<-table4b%>%
gather(“1999”, “2000”, key=“year”, value=“population”)
# to combine the tidied versions of table4a and table4b into a single tribble:
left_join(tidy4a,tidy4b)

spreading: 
# an observation is scattered across multiple rows, to tidy this up, we need two parameters: 1) the column that contains variable names, the key column; 2) the column contains values from multiple variables, the value coulomn.
table2%>%
spread(key=type, value=count)

seperate
Separate() pulls one column into multiple columns by splitting wherever a separator character appears.
table3%>%
separate(rate, into=c(“cases”, “population”), convert=TRUE)
# sep=* can be used to seperate a column by a specific character.
table3%>%
separate(rate, into=c(“cases”, “population”), sep=“/”)
table3%>%
separate(year, into=c(“century”, “”year), sep=2)

Unite
unite() combines multiple columns into a single column.
table5%>%
unite(new, century, year, sep=“”)

Missing values
na.rm=TRUE
complete() ensures the original dataset contains all those values filling in explicait NAs where necessary
fill() table a set of columns where you want missing values to be replaced by the most non missing values
 
Chapter 10: Relational data with dplyr
Mutating joins: add new variables to on data frame from matching observations in another
1. Inner join: matches pairs of observations whenever their keys are equal
x%>%inner_join(y, by=“key”)
2. Outer join:
    1. left_join keeps all observations in x
    2. right_join keeps all observations in y
    3. full_join keeps all observations in x and y
3. Duplicate keys: 
    1. one-to-many relationship
    2. Many-to-many relationship: you get all possible combinations
4. Defining the key columns
    1. The default, by=NULL, uses all variables that appear in both tables.
    2. by=“x”, use only some of the common variables
    3. by=c(“a”=“b”), match variable a in table I x to variable b in table y
        1. flights%>%left_join(airports, c(“dest”=“fat”))

Filtering joins:  filter observations from one data frame based on whether or not they match an observation in the other table
1. semi_join(): keeps all observations in x that have a match in y
    1. flights%>%semi_join(top_dest)
2. anti_join():drops all observation in x that have a match in y
    1. flights%>%anti_join(planes, by=“tailnum”)%>%count(tailnum, sort=TRUE)

Set operations: treat observations as if they were set elements.
1. intersect(x,y): return only observations in both x and y
2. union(x,y): return unique observations in x and y
3. setdiff(x,y):return observations in x but not in y




























