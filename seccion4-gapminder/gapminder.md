### Section 4 Overview

In Section 4, you will look at a case study involving data from the Gapminder Foundation about trends in world health and economics. 

After completing Section 4, you will:

* understand how Hans Rosling and the Gapminder Foundation use effective data visualization to convey data-based trends.
* be able to apply the ggplot2 techniques from the previous section to answer questions using data.
* understand how fixed scales across plots can ease comparisons.
* be able to modify graphs to improve data visualization.

There are 2 assignments that use the DataCamp platform for you to practice your coding skills.

We encourage you to use R to interactively test out your answers and further your learning.


---

#### Section 4: Gapminder   4.1 Introduction to Gapminder   Case Study: Trends in World Health and Economics
In this section, we will
demonstrate how relatively simple ggplot and dplyr
code can create insightful and aesthetically pleasing
plots that help us better understand trends in world health and economics.
We will use many of the techniques we have
learned about data visualization, exploratory data analysis,
and summarization.
We later augment the code somewhat to perfect the plots,
and describe some general principles to guide data visualization.
We're going to be using data from Gapminder.

    gapminder.org

Hans Rosling was the co-founder of the Gapminder Foundation, an organization
dedicated to educating the public by using data
to dispel common myths about the so-called developing world.
The organization uses data to show how actual trends in health and economics
contradict the narratives that emanate from sensationalist media
coverage of catastrophes, tragedies, and other unfortunate events.


As stated in the Gapminder Foundation's website,

> Journalists and lobbyists tell dramatic stories.
That's their job.
They tell stories about extraordinary events and unusual people.
The piles of dramatic stories pile up in people's minds
into an overdramatic worldview and strong negative stress feelings.


> The world is getting worse.

> It's we versus them.

> Other people are strange.

> The population just keeps growing.

> And nobody cares.


Hans Rosling conveyed actual database trends, in a dramatic way of his own,
using effective data visualization.
This section is based on these talks that
exemplify this approach to education.
"New Insights on Poverty" and "The Best Stats You've Ever Seen"
are the title of these talks.

Specifically, in this section, we set out
to answer the following two questions.

* First, is it a fair characterization of today's world
to say that it is divided into a Western rich nations,
and the developing world in Africa, Asia, and Latin America?

* Second, has income inequality across countries
worsened during the last 40 years?


We're going to use data and our code to answer these questions.


---

#### Section 4: Gapminder   4.1 Introduction to Gapminder   Gapminder Dataset

To learn about world health and economics,
we will be using the Gapminder data set provided in the DS Labs library.
This dataset was put together for you, and it
was created using a number of spreadsheets
available from the Gapminder Foundation.
You can access the table using this code.

> library(dslabs)
> data(gapminder)
> head(gapminder)
              country year infant_mortality life_expectancy fertility
1             Albania 1960           115.40           62.87      6.19
2             Algeria 1960           148.20           47.50      7.65
3              Angola 1960           208.00           35.98      7.32
4 Antigua and Barbuda 1960               NA           62.97      4.43
5           Argentina 1960            59.87           65.39      3.11
6             Armenia 1960               NA           66.86      4.55
  population          gdp continent          region
1    1636054           NA    Europe Southern Europe
2   11124892  13828152297    Africa Northern Africa
3    5270844           NA    Africa   Middle Africa
4      54681           NA  Americas       Caribbean
5   20619075 108322326649  Americas   South America
6    1867396           NA      Asia    Western Asia



and we can see that the data includes country, year, and several health
outcomes and economics outcomes.

As done in the New Insights on Poverty video,
we start by testing our knowledge regarding differences in child
mortality across different countries.
To get us started, we're going to take a quiz created
by Hans Rosling in his video New Insights on Poverty,
and we're going to start by testing our knowledge regarding differences
in child mortality across different countries.
So here's a quiz.

* For each of the pairs of countries here, which country
do you think had the highest child mortality in 2015?

* And also, which pairs do you think are most similar?
```
1. Sri Lanka* or Turkey
2. Poland* or South Korea
3. Malaysia or Russia*
4. Pakistan or Vietman*
5. Thailand* of South Africa
```

When answering these questions without data,
the non-European countries are typically picked
as having higher mortality rates, Sri Lanka over Turkey,
South Korea over Poland, and Malaysia over Russia.
It is also common to assume that countries considered
to be part of the developing world, Pakistan, Vietnam, Thailand, and South
Africa, have similarly high mortality rates.

Now let's answer these questions with data.
For example, for this first comparison, we
can write this simple dplyr code to see that Turkey has a higher mortality
rate than Sri Lanka.

    > gapminder %>% 
    +     filter(year == 2015 & country %in% c("Sri Lanka", "Turkey")) %>%
    +     select(country, infant_mortality)
        country infant_mortality
    1 Sri Lanka              8.4
    2    Turkey             11.6

We can use the same code to answer each of the five questions,
and we see that Sri Lanka has a lower mortality rate than Turkey,
South Korea has a lower mortality rate than Poland,
Malaysia has a lower mortality rate than Russia,
and Pakistan is very different from Vietnam,
and South Africa is very different from Thailand.
From here, we see that these comparisons, the European countries
have higher rates.
We also see that the countries from the developing world
can have very different rates.
It turns out that most people do worse than if they were just
guessing, which implies that we're more than ignorant, we're misinformed.



---

####  Section 4: Gapminder   4.1 Introduction to Gapminder   Life Expectancy and Fertility Rates

 Our misconceptions stem from the preconceived notion
that the world is divided into two groups, the Western World, composed
of Western Europe and North America, which
is characterized by long lifespans and small families versus the developing
world, Africa, Asia, and Latin America, characterized
by short lifespans and large families.
But does the data support this dichotomous view of the world?

The necessary data to answer this question
is also available in our gapminder table.
Using our newly-learned data visualization skills,
we will be able to answer this question.
The first plot we make to see what data have to say about this worldview
is a scatterplot of life expectancy versus fertility rates.
Fertility rates are defined as the average number of children per woman.
We will start by looking at data from about 50 years
ago when, perhaps, this worldview was cemented in our minds.
We just type the simple code and we see this plot.

    ds_theme_set()
    filter(gapminder, year == 1962) %>% ggplot(aes(fertility, life_expectancy)) + geom_point()

Note that most points do, in fact, fall into two distinct categories, one
with life expectancies around 70 years and three or less children per family
and the other with life expectancies lower than 65 years
and with more than five children per family.

Now, to confirm that indeed these countries are from the regions
we expect, we can use color to represent continent.
So we change the code slightly by adding the color argument,
assigning continent to it

    filter(gapminder, year == 1962) %>% ggplot(aes(fertility, life_expectancy, color = continent)) + geom_point()

Because continent is a character, it will automatically
assign color to each continent.
Here's the plot.
So indeed, in 1962, the West versus developing worldview
was grounded in some reality, but is this still the case 50 years later?
To answer to this question, we're going to learn about faceting.

---

#### Section 4: Gapminder   4.2 Using the Gapminder Dataset   Faceting

We could easily applaud the 2012
data in the same way we did for 1962.
But for comparison, side by side plots are preferable.
In ggplot, we can achieve this by faceting variables.
We stratify the data by some variable and make the same plot for each strata.

Here we are faceting by the year.
To achieve this, we use a function 

    facet_grid

This is added as a layer which automatically separates the plots.
The function lets you facet by up to two variables
using columns to represent one variable and rows to represent the other.
The function expects the rows and column variables separated by a tilde.


We're going to facet by continent and year.
So continent will be in the rows, and year will be in the columns.

> filter(gapminder, year %in% c(1962, 2012)) %>% ggplot(aes(fertility, life_expectancy, col=continent)) + geom_point() + facet_grid( continent ~ year )

We can see how the data has been stratified.
We have 1962 on the left, 2012 on the right,
and the 5 continents in each row.

However, this is just an example and more than what we want,
which is simply to compare in 1962 and 2012.
In this case, there's just one variable.
So what we do is we use the dot to let the facet function
know that we're not using two variables but just one.
The code looks like this.

> filter(gapminder, year %in% c(1962, 2012)) %>% ggplot(aes(fertility, life_expectancy, col=continent)) + geom_point() + facet_grid( . ~ year )

We simply type facet_grid dot-- meaning we're not using a variable
for the rows--
tilde year which now tells it make two columns--
1962 and 2012.
And here is the plot.
After we split the plot like this, it clearly
shows that the majority of countries have moved from the developing world
cluster to the Western world one.
They went from having large families and short lifespans
to having smaller families and longer lifespans.
In 2012, the Western versus developing world view no longer makes sense.
This is particularly clear when we compare Europe to Asia.

Asia includes several countries that have made great improvements
in the last 40 to 50 years.

To explore how this transformation happened through the years,
we can make the plot for several years.
For example, we can add 1970, 1980, 1990, and 2000 to the plot.
Now, if we do this, we will not want all the plots on the same row.
This is the default behavior of 

    facet_grid()

If we do this, the plots will become too thin,
and we won't be able to see the data.

Instead, we might want to have the plots across different rows and columns.
For this, we can use the 

    facet_wrap()

function which permits us to do this.
It automatically wraps the series of plots
so that most displays has viewable dimensions.

    > years <- c(1962, 1980, 1990, 2000, 2012)
    > continents <- c("Europe", "Asia")
    > gapminder %>% filter(year %in% years & continent %in% continents) %>% ggplot(aes(fertility, life_expectancy, col=continent)) + geom_point() + facet_grid( ~ year )

And then at the end, we `facet_wrap` instead of `facet_grid`.
And now, the plot looks like this.
Now, we're only showing Asia and Europe, but the function clearly
shows us how the Asian countries have made great improvements
throughout the years.

Now, note that the default choice for the range of the axes
is an important one.
When not using facet, this range is determined
by the data shown in the plot.
When using facet, the range is determined
by the data shown in **all plots**.
And therefore, it's kept fixed across the plots.
This makes comparisons across plots much easier.

For example, in the plot we just saw, the life expectancy has increased,
and the fertility has decreased across most countries.
We see this because the cloud of points moves up and to the left.
This is not the case if we adjust the scales to each year separately.

In this case, we have to pay special attention
to the range to notice that the plot on the right has larger life expectancy.

Therefore, by keeping the scales the same,
we were able to quickly see how many of the countries
outside of the Western world have improved
during the last 40 to 50 years.

---

#### Section 4: Gapminder   4.2 Using the Gapminder Dataset   Time Series Plots

The visualizations we have just seen
effectively illustrate that data no longer
supports the Western versus worldview.
But once we see these plots, new questions emerge.
For example, which countries are improving more?
Which ones are improving less?
Was the improvement constant during the last 50 years,
or was there more of an acceleration during a specific certain period?
For a closer look that may help answer these questions,
we introduce time series plots.

Time series plots have time in the x-axis, and an outcome, or measurement
of interest, on the y-axis.
For example, here's a trend plot for the United States fertility rate.
We can get this plot by simply using the geom points layer.

When we look at this plot, we immediately
see that the trend is not linear at all.
Instead, we see a sharp drop during the 60s and 70s to below 2.
Then, the trend comes back up to 2, and stabilizes there in the 1990s.

When the points are regularly spaced and densely packed as they are here,
we can create curves by joining points with lines.
This conveys that these data are from a single country.
To do this, we use the 
    
    geom_line() 

function instead of `geom_point`.

We write the code like this, and now the curve looks like this.

    gapminder %>% filter(country == "United States") %>% ggplot(aes(year, fertility)) + geom_line()

This is particularly helpful when we look at two or more countries.
Let's look at an example.
Let's subset the data to include two countries.
Let's look at one from Europe and one from Asia.

    > countries <- c("South Korea", "Germany")
    gapminder %>% filter(country == countries) %>% ggplot(aes(year, fertility)) + geom_line()

So we copy the code above--
here it is-- and we get this plot.
But note that this is not what we want.
Rather than a line for each country, this code
has produced a line that goes through the points for both countries-- they're
both joined.

This is actually expected, since we have not told ggplot anything
about wanting two separate lines.

To let ggplot know that there are two curves that need to be made separately,
we assign each point to a group, one for each country.
We do this through the mapping.
We assign country to the group argument.
The plot now looks like this.

    gapminder %>% filter(country == countries) %>% ggplot(aes(year, fertility, group = country)) + geom_line()

We can see the two lines, one for each country.

However, we don't know which line goes with which country.
To see this, we can use color for example.
We can use color to distinguish the two countries.

A useful side effect of using color to assign different colors to each country
is that ggplot automatically groups the data by the color value.
So the code is very simple, it looks like this.

    gapminder %>% filter(country == countries) %>% ggplot(aes(year, fertility, col = country)) + geom_line()


And once we type this, then we get two lines, each with a color.
And a legend has been added by default. Note that this plot clearly
shows how South Korea's fertility rate dropped drastically
during the 60s and 70s.
And by 1990, it had a similar fertility rate to Germany.

For time series plots, we actually recommend labeling the curves
rather than using legends as we did in the previous plot.
This suggestion actually applies to most plots.
Labeling is usually preferred over legends.
However, legends are easier to make and appear
by default in many of ggplot's functions.

We are going to show an example of how to add labels to a time series plot.
We demonstrate how we can do this using the life expectancy data.
We define a data table with the label locations.
And then we use a second mapping just for the labels.
The code looks like this.

    > labels <- data.frame(country = countries, x = c(1975, 1965), y = c(60, 72))
    > gapminder %>% filter(country == countries) %>% 
        ggplot(aes(year, life_expectancy, col = country)) + 
        geom_line() + 
        geom_text(data = labels, aes(x, y, label = country), size = 5) + 
        theme(legend.position = "none")


Notice that we define a data frame with the locations
of where we want the labels.
We pick these by eye.
And then you can see in the geom_text, we
are using the labels data frame as the data,
so that those labels are put in those positions.
Then we have to tell the plot not to add a legend through the theme function.
And now the plot looks like this.

This is the life expectancy plot.
And we can see how the plot shows how an improvement in life expectancy
followed the drops in fertility rates.
While in 1960, Germans lived more than 15 years more on average
than South Koreans, by 2010 the gap is completely closed.

Another commonly held notion is that wealth distribution across the world
has become worse during the last decades.
When general audiences are asked if poor countries have become poorer
and rich countries have become richer, the majority answer yes.

By using histograms, smooth densities, and box plots,
will be able to understand if this is in fact the case.


---

#### Section 4: Gapminder   4.2 Using the Gapminder Dataset   Transformations
 In this video, we cover transformations.
Transformations can be very useful to better understand distributions.
As an example, in this video, we look at income.
The Gapminder data table includes a column
with the country's gross domestic product, the GDP.
GDP measures the market value of goods and services
produced by a country in a given year.

    > head(gapminder)
                  country year infant_mortality life_expectancy fertility population          gdp
    1             Albania 1960           115.40           62.87      6.19    1636054           NA
    2             Algeria 1960           148.20           47.50      7.65   11124892  13828152297
    3              Angola 1960           208.00           35.98      7.32    5270844           NA
    4 Antigua and Barbuda 1960               NA           62.97      4.43      54681           NA
    5           Argentina 1960            59.87           65.39      3.11   20619075 108322326649
    6             Armenia 1960               NA           66.86      4.55    1867396           NA
      continent          region
    1    Europe Southern Europe
    2    Africa Northern Africa
    3    Africa   Middle Africa
    4  Americas       Caribbean
    5  Americas   South America
    6      Asia    Western Asia

The GDP per person is often used as a rough summary of how rich a country is.
Here we divide this quantity by 365 to obtain the more interpretable measure
dollars per day.

Using current US dollars as a unit, a person surviving on an income
of less than $2 a day, for example, is defined
to be living in absolute poverty.
So we're going to add this variable to our data table.
It's the dollars per day variable.

    > gapminder <- gapminder %>% mutate(dollars_per_day = gdp/population/365)



GDP divided by population divided by 365.
Before we continue, note that GDP values is in our data table
are adjusted for inflation and represent current US dollars.
So these values are meant to be comparable across the years.
Also note that these are country averages and that within each country,
there's much variability.

OK, so let's move on to examining distributions.
Here's a histogram of per day incomes from 1970.
You can obtain it with a simple code which you've already learned.

    > past_year <- 1970
    > gapminder %>% filter(year == past_year & !is.na(gdp)) %>% ggplot(aes(dollars_per_day)) + geom_histogram(binwitdth = 1, color = "black")

We see that for the majority of countries,
averages are below $10 a day.
However, the majority of the x-axis is dedicated to the 35 countries
with averages above 10.
It might be more informative to quickly be
able to see how many countries make on average about $1 a day,
extremely poor--
$2 a day, very poor--
$4 a day, poor--
$8 a day, which is about middle--
$16 a day which is a well-off country--
$32 is rich, and $64 which is very rich.
These changes are multiplicative.

And here we introduce **log transformations**.
Log transformations change multiplicative changes
into additive ones.
Using base 2 for, example, means that every time a value doubles,
the log transformation increases by one.
So to get the distribution of the log base 2
transform values, we simply transform the data and use the same code.
And now we obtain this histogram.

    > gapminder %>% filter(year == past_year & !is.na(gdp)) %>% ggplot(aes(log2(dollars_per_day))) + geom_histogram(binwidth = 1, color = "black")

In this plot, we see something new.
We see two clear bumps.

Before we continue interpreting the data,
let's introduce some commonly used statistical language.
In statistics, these bumps are sometimes referred to as **modes**.
The mode of a distribution is the value with the highest frequency.
The mode of a normal distribution is the average.
But if the mode is a value with the highest frequency, how can
we have more than one?

When a distribution like the one we just saw
doesn't monotonically decrease from the mode,
we call the location where it goes up and down again as **local modes**.
And we say that the distribution has **multiple modes**.

The histogram we just saw suggests that in 1970, country income distribution
have two modes.
One at about $2 per day, one in the log2 scale, and another at about $32
per day--
5 in the log2 scale.

This bimodality is consistent with the dichotomous world
made up of countries with average incomes less than $8 per day,
3 on the log scale.
And countries above that we see two modes in the histogram.

Now before we continue interpreting the data,
we need to make another pause to explain how we choose the base.
And the histogram we just saw we chose base 2.
Other common choices are the natural log and base 10.
In general, we do not recommend using the natural log
for data exploration in visualization.
Why is this?
It's because while we know what 2 to the 2 is--
2 to the 3--
2 to do the 4, we can quickly compute that in our mind.
10 to the 1, 10 to the 2, 10 to the 3-- also very easy to compute.
It's not easy to compute E to the 2, E to the 3, etcetera.
So we don't recommend using the natural log for data exploration.

In the dollar per day example, we use base 2 instead of base 10
because the resulting range is easier to interpret.
The range of the values being plotted started from about 0.3
and ended around 50.
In base 10, this turns to a range that includes very few integers, just 0
and 1.
With base 2, our range includes negative 2, negative 1, 0, 1, 2, 3, 4, and 5.
Note that it is easier to compute 2 to the x and 10 to the x
when x is an integer.
So we prefer to have more integers in the transform scale.

Another consequence of a limited range is that choosing the bin width
is more challenging.
With log base 2, we know that a bin width of 1
will translate to bins with range x to 2 to the x.

As an example in which base 10 makes more sense than base 2,
consider population size.
Using log base 10 makes more sense here since the range for these data
goes from 45,000 to about 800 million.

Here's a histogram if we transform the values with the log base 10.
Looking at the scale knowing that we're in base 10,
we can quickly determine that country population ranges from about 40,000
to about a billion.

Now let's talk about log transformations and how we use them in the plots.
There are two ways we can use long transformation in plots:

* We can log the values before plotting them,
* or we can use log scales in the axis.

Both approaches are useful and have different strengths.

If we log the data, we can more easily interpret intermediate values
in the scale.
For example, if we use a scale that looks
like this that has been log transformed, we know that x is 1.5.
If the scales are logged and we have that x in between 1 and 10,
then we don't know immediately what the x
is because it's 10 to the 1.5, not an easy thing to compute in our heads.
However, the advantage of using log scales
is that we see the original values on the axis.
So this has an advantage because we see the original values displayed
in the plot which makes it very easy to quickly see what
numbers we're actually dealing with.
For example, when we see $32 a day, instead of 5 log base $2 a day.

Now let's review how we make plots where the scales have been log transformed.
We already learned this.
We learned the `scale_x_continuous` function.
So we want to remake the histograms that we already made
but now using scales that have been transformed.
We simply add a layer using the scale underscore
x underscore continuous function.
And we no longer transform the data before plotting it.
So the code would look like this, and the plot would look like this.

    > gapminder %>% filter(year == past_year & !is.na(gdp)) %>% ggplot(aes(dollars_per_day)) + geom_histogram(binwidth = 1, color = "black") + scale_x_continuous(trans = "log2")

Notice that the histogram looks exactly the same.
The difference is that in the scales in the x-axis,
instead of seeing the log values, we see the original values in a log scale.
So we see 1, 8, and 64.
And we can very quickly interpret what that means in terms of dollars per day.


---

#### Section 4: Gapminder   4.2 Using the Gapminder Dataset   Stratify and Boxplot
 
 The histogram showed us that the income distribution
values show a dichotomy.
However, the histogram does not show us if the two groups of countries
are west versus the developing world.
To see distributions by geographical region,
we first stratify the data into regions, and then examine
the distribution for each.
Now, because a number of regions is large in this case,
it's 22, as we can see, by just typing this command in r.

    > length(levels(gapminder$region))
    [1] 22

Looking at histograms or smooth densities for each will not be useful.
Instead, we can stack box plots next to each other.
To do this, we simply write this code.
We've learned how to use geom_boxplot before so we write this.

    p <- gapminder %>% filter(year == past_year & !is.na(gdp)) %>% ggplot(aes(region, dollars_per_day)) 
    p + geom_boxplot()

When we do this, we get this plot.

Now, note that we can't read the region names because the default ggplot
behavior is to write the labels horizontally
and here we run out of room.
We can easily fix this by rotating the labels.
Consulting the documentation, we find that we
can rotate the names by changing the theme through element underscore text.
The h just equals 1 argument justifies this text
so that it's next to the axis.
So now what we do is we add to our graph,
using layers, the following line.

    p + geom_boxplot() + theme(axis.text.x = element_text(angle = 90, hjust = 1))

We can already see that there is indeed a west versus the rest dichotomy.
If you look closely at the box plots that are high,
we see that they're North America, northern Europe, Australia,
New Zealand, and Western Europe.

There are a few more adjustments we can make
to this plot to help uncover this reality to help relay this message.
First, it helps to order the regions in some other order that
is not alphabetical.
Ordering alphabetically is completely arbitrary.

The function that's going to help us achieve this is the reorder function.

    reorder()

This function lets us change the order of the levels of a factor variable
based on a summary computed on a numeric vector.


Before we continue with our example, let's
understand how the reorder function works using a simpler one.
Let's define a factor.
Based on the vector with 5 entries, Asia, Asia, west, west, west.
If we turn this vector into factor, the levels of this factor
are ordered alphabetically.
This is the default in r.

    > fac <- factor(c("Asia", "Asia", "West", "West", "West"))
    > levels(fac)
    [1] "Asia" "West"

So Asia is the first level.
West is the second level.
But suppose that each of these elements of the original vector
are associated with a value.
Here we're just defining one arbitrarily.

    value <- c(10,11,12,6,4)

Lets suppose that we want to order the levels based
on the mean value of these numbers.
In this case, the west has a lower mean.
It's the mean of 12, 6, 4.
Compared to the mean of Asia, which is the mean of 10 and 11.
So if we use a function the order like this-- reorder fac--

    > fac <- reorder(fac, value, FUN = mean)
    > levels(fac)
    [1] "West" "Asia"

that's our factor.
Value-- those are five values.
And then using the function mean to summarize the values,
we can see that the new factor that's created has levels ordered differently.
Now west is the first one.
Why?
Because it has a smaller mean value of the value vector.

Let's get back to our example.
In our example, we have regions.
These are the different parts of a continent.
We also have continents.
And then we have divided the world into West versus the rest.
So we have three different ways of dividing the data.
The first thing we're going to do to improve our plot,
is to simply reorder the regions by their median income level.
To achieve this, we write the same code as
before but we add to mutate that changes region to a new factor
where the levels are reordered.
In this line here.
If we do this, we get the following plot.

    p <- gapminder %>% filter(year == past_year & !is.na(gdp)) %>% mutate(region = reorder(region, dollars_per_day, FUN = median)) %>% ggplot(aes(region, dollars_per_day, fill = continent)) + geom_boxplot() + theme(axis.text.x = element_text(angle = , hjust = )) + xlab("")

Now we can see that the box plots are ordered by their medium value.
And we very quickly see that there's four box
plots that stand out at the end.
The four highest ones.
These are Western Europe, Australia and New Zealand, northern Europe,
and North America.

This is what we define as the West.

Now there's another change we made to the plot to help convey this message,
and that's that we use color to show another variable.
We use color to show continent.
Remember, regions are parts of continents.
To add color to define the different continents,
we use the fill argument in the aesthetic mappings of ggplot.
We simply say fill equals continent.
And now each continent gets its color.

Its own color.
Now, we can see what this does is it helps
us see that, for example, the blue box blots are towards the right
because these are the European countries.
We also see that the red countries, the red box blots, are to the left.
These are the countries in the African continent.
The last change we can make to this plot to help
us see the data little bit better, is to change the scale through the log scale.


    p + scale_y_continuous(trans = "log2")


We want to change it to log2 scale in this case,
so we add the layer scale underscore y underscore continuous,
and we use the log2 transformation.
And now what this does, is it helps us see
the differences between the countries with the lower income.
For example, we see a difference now between the African continent, which
is in red, and Asia, which is in green.


The last change we can make to this plot to make
it tell the story a little better to give us even more information,
is to show the data.
In many cases, we don't show the data, the actual individual points,
because it adds too much clutter to the plot and it obfuscates the message.
But in this particular example, we don't have that many points.


    p + scale_y_continuous(trans = "log2") + geom_point(show.legend = FALSE) 

So we can add a layer of points by simply adding the geom point layer.
It's very simple.
We just add that layer and now we get this plot.
And we can see the individual points.
You can decide if you show this or not.
But now we can see exactly where every single country lies.

---


#### Section 4: Gapminder   4.2 Using the Gapminder Dataset   Comparing Distributions

The exploratory data analysis we have conducted
has revealed two characteristics about average income distributions in 1970.

Using a histogram, we found a bimodal distribution
with the most relating to poor and rich countries.
Then by stratifying by region and examining box plots,
we found that the rich countries were mostly in Europe and Northern America,
along with Australia, New Zealand, and then the poor countries were mostly
in the rest of the world.

So we are going to define a vector that defines the regions in the West.
They're just simply defining a vector like this.

    west <- c("Western Europe", "Northern Europe", "Southern Europe", "Northern America", "Australia and New Zealand")


Now we want to focus on comparing the differences in distribution
across time.
We start by confirming that the bi-modality observed in 1970
is explained by a west versus developing world economy.
We do this by creating a histogram for the groups previously defined.

    p <- gapminder %>% filter(year == past_year & !is.na(gdp)) %>% mutate(group = ifelse(region%in%west, "West", "Developing")) %>% ggplot(aes(dollars_per_day)) + geom_histogram(binwidth = 1, color = "black") + scale_x_continuous(trans = "log2") + facet_grid(. ~ group)

Note that we create the two groups with an if else inside a mutate.
And that if we then use facet grid to make histograms for each group using
this code, we see this histogram.
And we immediately see that the countries in the West
have higher incomes.
The histogram is shifted to the right.
Countries in the developing world are shifted towards the left.

Now we're ready to see if the separation is worse today
than it was 40 years ago.
We do this by now faceting by both region and year.
So it's the same code, but now we're looking at two years, 1970 and 2010.
And we end the code with a facet grid by year and group.

    past_year <- 1970
    present_year <- 2010
    p <- gapminder %>% filter(year %in% c(past_year, present_year) & !is.na(gdp)) %>% mutate(group = ifelse(region%in%west, "West", "Developing")) %>% ggplot(aes(dollars_per_day)) + geom_histogram(binwidth = 1, color = "black") + scale_x_continuous(trans = "log2") + facet_grid(year ~ group)

Now we can see the histogram again for the four different groups.
When we look at this figure, we can see that the developing world has shifted
to the right more than the West.
Meaning that it has gotten closer.
The income distribution of the developing countries
has gotten closer to those from the west.

Before we interpret the findings of this plot further.
We note that there are more countries represented in the 2010 histograms
than in the 1970s ones.
The total accounts are larger.

One reason for this is that several countries were founded after 1970.
For example, the Soviet Union turned into several countries,
including Russia and Ukraine there in the '90s.
Another reason is that data is available for more countries in 2010
compared to 1970.

So we're going to remake the plots, but using only countries
with data available for both years.
We're going to use this very simple code.

    country_list_1 <- gapminder %>% filter(year == past_year & !is.na(dollars_per_day)) %>% .$country
    country_list_2 <- gapminder %>% filter(year == present_year & !is.na(dollars_per_day)) %>% .$country
    country_list <- intersect(country_list_1, country_list_2)



We're going to define a vector with a list for 2010,
a vector with a list for 1970.
Here notice we use the dot that we explained earlier,
to get this character vector out of this the deplyer command.
And then we're going to take the intersection using
the intersect function.
There's actually a better way of doing this using the tidyverse tools,
but we haven't learned those yet.
So we use this simple piece of code.

So now there's 108 countries in this list.
It accounts for 86% of the total population.
So this subset should be representative of the entire world.

Let's make the plot again, but this time using
only the subset of countries that are present for which
data is present in 1970 and 2010.
We're going to use the country in country list argument
to do this in a filter function.

    gapminder %>% filter(year %in% c(past_year, present_year) & country %in% country_list) %>% mutate(group = ifelse(region%in%west, "West", "Developing")) %>% ggplot(aes(dollars_per_day)) + geom_histogram(binwidth = 1, color = "black") + scale_x_continuous(trans = "log2") + facet_grid(year ~ group)

Now we get this plot.
We now see that while the rich countries have
become a bit richer percentage wise, the poorer countries
appear to have improved more.
The histogram has shifted more to the right than for the rich countries.
In particular, we see that the proportion
of developing countries earning more than $16 a day increases substantially.

To see which specific regions improve the most,
we can remake the box plots that we made earlier, but now adding 2010.
Here it is.
We use the same code.

    p <- gapminder %>% filter(year %in% c(past_year, present_year) & country %in% country_list) %>% mutate(region = reorder(region, dollars_per_day, FUN = median)) %>% ggplot() + theme(axis.text.x = element_text(angle = 90, hjust = 1)) + xlab("") + scale_y_continuous(trans = "log2")

    p + geom_boxplot(aes(region, dollars_per_day, fill = continent)) + facet_grid(year ~ .)

We use facet grid to divide into 2010 and 1970.
And we can see which countries have gone up more.
Now this box plots, it's a little bit hard to compare,
because we're trying to compare box plots that are on top of each other.
It's helpful to put them next to each other.
So we're going to learn to **ease the comparisons**.

To do this we're going to pause to introduce another powerful ggplot
feature.
Because we want to compare each region before and after,
it would be convenient to have the 1970 box plot next to the 2010 box plot.
In general, comparisons are easier when data are plotted next to each other.
So instead of faceting, we keep the data from each year together.
But ask ggplot plot to color or fill the box block depending on the year.
ggplot automatically separates them and puts the two box plots
next to each other.
This is very convenient.
Because year is a number, we turn it into a factor
so that each is a category.
This is because ggplot automatically assigns a color
to each level of a factor if we assign that factor to the color argument.

    p + geom_boxplot(aes(region, dollars_per_day, fill = factor(year)))

So if we type this command now, we add fill equals factor year,
we get this plot.
And we can see which countries have improved the most.
Look at Eastern Asia, for example, how it went from way down around 8
all the way up almost to 64.

And finally we point out that if what we are most interested in
is in comparing before and after values, it
might make more sense to plot the ratios, or differences in the log
scale.
We're still not ready to learn the code that achieves this,
but here's what the plot would look like.
This is actually showing a box plot of the log ratios from 2010
compared to 1970 for each country.
And we can see again, eastern Asia has the big if improvement.


---

#### Section 4: Gapminder   4.2 Using the Gapminder Dataset   Density Plots

 We have used data exploration
to discover that income gap between rich and poor countries
has closed considerably during the last four years.
We use a series of histograms and box plots to see this.
Here, we suggest a succinct way to convey this message with just one plot.
We will use smooth density plots.

Let's start by noting that the density plot for income distribution in 1970
and 2010 deliver the message that the gap is closing.
In the 1970s plot, we see two clear modes, poor and rich.
In 2010, it appears that some of the poorer countries
have shifted towards the right, closing the gap.

The next message we need to convey is that the reason
for this change in distribution is that poor countries became
richer rather than some rich countries becoming poorer.
To do this, all we need to do is assign a color
to the groups we identified during the data exploration.
However, before we can do this, we need to learn
how to make these smooth densities in a way that preserves information of how
many countries are in each group.
To understand why we need to do this, note the discrepancy
in the size of each group.
If we divide the world into developing and West,
we have 87 developing countries and 21 Western countries.

    > gapminder %>% filter(year == past_year & country %in% country_list) %>% mutate(group = ifelse(region %in% west, "West", "Developing")) %>% group_by(group) %>% summarize(n=n()) %>% knitr::kable()
    |group      |  n|
    |:----------|--:|
    |Developing | 87|
    |West       | 21|

   

If we overlay the two densities, the default
is to have the area represented by each distribution add up to 1
regardless of the size of each group.
This makes it seem like there's the same number of countries
in each group, which is incorrect.

To change this, we'll need to learn to access computed variables
with the `geom_density` function.

To have the areas of the densities be proportional to the size of the groups,
we can simply multiply the y-axis values by the size of the group.
From the geom_density help file, we see that the function
computes a variable called count that does exactly this.

We want this variable to be on the y-axis rather than the density value.
In gg plot, we can access these variables
by surrounding their names with dot dot.
So we will use the following mapping.
We type aes x = dollars_per_day and y = dot dot count dot dot.

    > aes(x = dollars_per_day, y = ..count..)


This will put count on the y-axis.
We can now create the desired plot by simply changing the mapping
in the previous code chunk.

    p <- gapminder %>% filter(year %in% c(past_year, present_year) & country %in% country_list) %>% mutate(group = ifelse(region%in%west, "West", "Developing")) %>% ggplot(aes(dollars_per_day, y = ..count.., fill = group))  + scale_x_continuous(trans = "log2") 
    p + geom_density(alpha = 0.2) + facet_grid(year ~ .)


It would look like this.
And it produces a plot like this.
Notice that now we can clearly see that the developing
world has more countries.

If you want the densities to be smoother,
because we can see in the Western countries,
there was a lot of unsmoothness, we can change the bw argument,
as we learned earlier.
We tried a few and decided on 0.75.
You can try a few yourself.

    p + geom_density(alpha = 0.2, bw = 0.75) + facet_grid(year ~ .)

Here's what it looks like with 0.75.


This plot now shows what is happening very clearly.
The developing world distribution is changing.
A third mode appears consisting of the countries that most closed the gap.

We can actually make this figure somewhat more informative.
From the exploratory data analysis, we noticed
that many of the countries that most improved were from Asia.
We can easily alter the plot to show key regions separately.

To do this, we introduced a new function called 
    
    case_when

It's useful for defining groups.
It currently does not have a data argument.
This might change.
But because it doesn't, we need to access the components of our data
using the dot placeholder.
So the code looks like this.

```R
gapminder <- gapminder %>% 
  mutate(group = case_when(
    .$region %in% west ~ "West",
    .$region %in% c("Eastern Asia", "South-Eastern Asia") ~ "East Asia",
    .$region %in% c("Caribbean", "Central America", "South America") ~ "Latin America",
    .$continent == "Africa" & .$region != "Northern Africa" ~ "Sub-Saharan Africa",
    TRUE ~ "Others")) 
```

Look at what we're doing.
We're assigning groups depending on the region.
If the region's in the West, we call the West.
If the region is in Eastern Asia, Southern Asia, we call it East Asia.
If the region is in the Caribbean, Central America, South America,
we call it Latin America.
If the continent is Africa and the region is not Northern Africa,
we're going to call it Sub-Saharan Africa.

And then the rest we're just going to call others.
Now we turn this group variable into a factor
to control the order of the levels.
We do it like this.

    gapminder <- gapminder %>% mutate(group = factor(group, levels = c("Others", "Latin America", "East Asia", "Sub-Saharan Africa", "West")))

We picked this particular order for a reason that becomes clearer
later when we make the plots.

Now we can easily plot the density for each one.
We use color and size to clearly see the top.
Here's what the two look like in 1970 and 2010.
The plot is a little bit cluttered and is hard to read,
and we're going to use a stacking approach to make the picture clear.
Here's how we do it.

    p + geom_density(alpha = 0.2, bw = 0.75, position = "stack") + facet_grid(year ~ .)

We use this argument, position = "stack".
And now what happens is that the histograms or density
plots are stacked on top of each other.
Here we can see clearly that the distribution from East Asia and Latin
America and others shift markedly to the right
while Sub-Saharan Africa remain stagnant.
Note that we order the levels of the groups
so that the West density was plotted first, and then Sub-Saharan Africa.
This helps us see this pattern.
As a final point, we note that these distributions
weigh every country the same.
So if most of the population is improving
but living in a very large country such as China, we might not appreciate this.
We can actually weigh the smooth densities using the weight mapping
argument.

    gapminder %>% filter(year %in% c(past_year, present_year) & country %in% country_list) %>% group_by(year) %>% mutate(weight = population/sum(population)*2) %>% ungroup() %>% ggplot(aes(dollars_per_day, fill = group, weight = weight)) + scale_x_continuous(trans = "log2") + geom_density(alpha = 0.2, bw = 0.75, position = "stack") + facet_grid(year ~ .)


And if we do that, the plot now looks like this.
This particular figure shows very clearly how the income distribution
gap is closing with most of the poor countries remaining
in Sub-Saharan Africa.

---

#### Section 4: Gapminder   4.2 Using the Gapminder Dataset   Ecological Fallacy

Throughout this section,
we have been comparing regions of the world.
We have seen that on average some regions do better
than others in health outcomes and economic outcomes.

Here, we focus on the importance of describing
the variability within the groups.
While we do this, we'll also show you some other ggplot functions as well as
a transformation called the **logit transformation**,
which is useful for the data that we'll be looking at.

As an example for this, we will focus on the relationship between **country child
survival rates and average income**.

We start by comparing these quantities across regions.
Before we start, we're going to find a few more
regions using the case one function.
We're going to find the West, Northern Africa, East Asia, Southern Asia, Latin
America, Sub-Saharan Africa, and the Pacific Islands.

```R

gapminder <- gapminder %>%
 
  mutate(group = case_when(

    .$region %in% west ~ "West",
    .$region %in% "Northern Africa" ~ "Northern Africa",

    .$region %in% c("Eastern Asia", "South-Eastern Asia") ~ "East Asia",

    .$region %in% "Southern Asia" ~ "Southern Asia",
    .$region %in% c("Caribbean", "Central America", "South America") ~ "Latin America",

    .$continent == "Africa" & .$region != "Northern Africa" ~ "Sub-Saharan Africa",

    .$region %in% c("Melanesia", "Micronesia", "Polynesia") ~ "Pacific Island",
)) 


```


Once we do this, we can compute the quantities
that we're interested in for each region.
We'll compute the average.

    surv_income <- gapminder %>% filter(year %in% present_year & !is.na(gdp) & !is.na(infant_mortality) & !is.na(group)) %>% group_by(group) %>% summarize(income = sum(gdp) / sum(population)/365, infant_survival_rate = 1 - sum(infant_mortality/1000*population) / sum(population))
    > surv_income %>% arrange(income)
    # A tibble: 7 x 3
      group              income infant_survival_rate
      <chr>               <dbl>                <dbl>
    1 Sub-Saharan Africa   1.76                0.936
    2 Southern Asia        2.07                0.952
    3 Pacific Island       2.70                0.956
    4 Northern Africa      4.94                0.970
    5 Latin America       13.2                 0.983
    6 East Asia           13.4                 0.985
    7 West                77.1                 0.995


This shows a dramatic difference.
While in the West less than 0.5% of children die, in Sub-Saharan Africa,
the rate is higher than 6%.
In fact, the relationship between these two variables
is almost perfectly linear.

In this plot, we introduced the use of the limit argument, which
lets us change the range of the axis.
We would do it like this following this code.

    surv_income %>% ggplot(aes(income, infant_survival_rate, label = group, color = group)) + scale_x_continuous(trans = "log2", limit = C(0.25, 150)) + scale_y_continuous(trans = "logit", limit = c(0.875, .9981), breaks = c(.85, 0.90, .95, .99, .998)) + geom_label(size = 3, show.legend = FALSE)

We are making the range larger than the data needs
because we will later compare this plot we just
saw to one with more variability.
And we want the ranges to be the same.
We also introduced the 

    break

 argument, which lets
us set the location of the axis labels.

Finally, we introduce a new transformation,
the 

    logistic transformation (logit)

The logistic or logit transformation for a proportional rate p
is defined as follows.
f of p equals the log of p divided by 1 minus p.

    f(p) = log(p/(1-p))

When p is a proportion or probability, the quantity that is being logged,
p divided by 1 minus p, is called the **odds**.
And the case p is the proportion of children that survive.
The odds tells us how many more children are expected to survive than to die.
The log transformation makes this quantity symmetric.
If the rates are the same, then the log odds is 0.

Fold increases or decreases turn into positive and negative increments
respectively.

This scale is useful when we want to highlight
differences that are near 0 or near 1.
For survival rates, this is important because a survival rate of 90%
is unacceptable while the survival rate of 99% is relatively good.
We would much prefer a survival rate closer to 99.9%.
We want our scale to highlight these differences and the logit does this.
Note that 99.9 divided by 0.1 is about 10 times larger than 99 divided by 1,
which is about 10 times larger than 90 divided by 10.
By using the log, these fold changes turn into constant increases.

OK, now back to our plot.
Based on the plot we showed earlier, do we
conclude that a country with a low income
is destined to have low survival rate?
Do we conclude that all survival rates in Sub-Saharan Africa
are all lower than in southern Asia, which in turn are lower
than in the Pacific Islands and so on?

Jumping to this conclusion based on the plot
we showed, the plot that shows only the averages is
referred to as the **ecological fallacy**.

The almost perfect relationship between survival rates and income
is only observed for the averages at the regional level.
Once we show the data, we see a somewhat more complicated story.
So here is the plot for the averages.
And look at what happens once we show you every individual country.
Specifically, we see that there is a large amount of variability.
We see that the countries from the same regions can be quite different.
And that countries within the same income
can have different survival rates.

For example, while on average Sub-Saharan Africa
had the worst health and economic outcomes,
there is wide variability within that group.
For example, note that Mauritius and Botswana
are doing much better than Angola and Sierra Leone with Mauritius
comparable to Western countries.

---

