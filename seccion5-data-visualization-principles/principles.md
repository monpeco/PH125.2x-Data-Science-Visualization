### Section 5 Overview

Section 5 covers some general principles that can serve as guides for effective data visualization.

After completing Section 5, you will:

* understand basic principles of effective data visualization.
* understand the importance of keeping your goal in mind when deciding on a visualization approach.
* understand principles for encoding data, including position, aligned lengths, angles, area, brightness, and color hue.
* know when to include the number zero in visualizations.
* be able to use techniques to ease comparisons, such as using common axes, putting visual cues to be compared adjacent to one another, and using color effectively.

There are 3 assignments that use the DataCamp platform for you to practice your coding skills.

We encourage you to use R to interactively test out your answers and further your learning.

---

#### Section 5: Data Visualization Principles   5.1 Data Visualization Principles, Part 1   Introduction to Data Visualization Principles

We have already provided some rules
to follow as we created plots for our examples.
Here we aim to provide some general principles
we can use as guidelines for effective data visualization.
Much of this part of the course is based on a talk
by **Karl Broman** entitled 

    "Creating Effective Figures and Tables"

and from class notes from **Peter Aldhous** titled

    "Introduction to Data Visualization."

In many of our examples, we follow Karl's approach.
We show some examples of plot styles we should
avoid, explain how to improve them, and then use these
as motivation for a list of principles.
We compare and contrast plots that follow these principles to those that
don't.
The principles are mostly based on research
related to how humans detect patterns and make visual comparisons.
The preferred approaches are those that best
fit the way our brain processes visual information.
When deciding on a visualization approach
it is also important to keep our goal in mind.
We may be comparing a viewable number of quantities,
describing distributions for categories or numeric values,
comparing the data from two groups, or describing
the relationship between two variables.
As a final note, we also know that for a data scientist
it is important to adapt and optimize graphs to the audience.
For example, an exploratory plot made for ourselves
will be different than a chart intended to communicate
a finding to a general audience.


---

#### Section 5: Data Visualization Principles   5.1 Data Visualization Principles, Part 1   Encoding Data Using Visual Cues

We start by describing some principles for encoding data.
There are several approaches to our disposal,
including 

* position, 
* aligned lengths, 
* angles, 
* area, 
* brightness,
* and color hue.

In our first example, to illustrate how some of these strategies compare,
let's suppose we want to report the results from two
hypothetical polls, asking what browser you use,
what's your browser preference.
And the polls were taken in 2000, and then in 2015.
Here, for each year, we are simply comparing
four quantities, four percentages.

A widely used graphical representation of percentages, popularized
by Microsoft Excel, is the pie chart.
Here's a pie chart for our data.
There's two pie charts, one for 2000, one for 2015.
Here, we're representing quantities with both areas and angles,
since both the angle and area of each pie slice
is proportional to the quantity it represents.
This turns out to be a suboptimal choice,
since, as demonstrated by perception studies,
humans are not good at precisely quantifying angles,
and are even worse when only area is available.
This makes the donut chart, which only uses area, even worse
than the pie chart.
To see how hard it is to quantify angles and area,
note that the rankings and all the percentages in the plots
that we just saw changed from 2000 to 2015.
Can you determine the actual percentages and rank the browser's popularity?
Can you see how the percentages changed from 2000 to 2015?
It's not easy to tell from the plot.

In this case, simply showing the numbers is not only clear,
but it would save us on print costs, if making a paper version of our results.
If we write out the percentages, we quickly
see which browser is more popular, and how they changed from 2000 to 2015.
If we insist on a plot, the preferred way to plot these quantities
is to use length and positions, since humans are
much better at judging linear measures.
The bar plot uses this approach by using bars of length proportional
to the quantity of interest.
By adding a horizontal line at strategically chosen values,
in this case at every multiple of 10, we ease the quantifying
through the position of the top of the bars.
Compare these two plots.

Notice how much easier it is to see the differences in the bar plot.
In fact, we can now determine the actual percentages
by following a horizontal line to the y-axis.
If for some reason you need to make a pie chart,
do include the percentages as numbers to avoid having
to infer them from the angles or area.

In summary, position and length are the preferred way
to display quantities over angles, which are preferred over area.
Brightness and color are even harder to quantify than angles and area.
But, as we will see later, they are sometimes
useful when more than two dimensions are being displayed.


---

#### Section 5: Data Visualization Principles   5.1 Data Visualization Principles, Part 1   Know When to Include Zero

When using bar plots, it is dishonest
not to start the bars at 0.
This is because by using a bar plot, we are
implying the length is proportional to the quantities being displayed.
By avoiding 0, relatively small differences
can be made to look much bigger than they actually are.

This approach is often used by politicians or media organizations
trying to exaggerate the difference.
Here is an illustrative example.
This is a bar plot made by Fox News showing southwest border
apprehensions in 2011, 2012, and 2013.
Look how much bigger the 2013 bar looks compared to the 2011.
From this plot, it appears that apprehensions have almost
tripled, when in fact, if you look at the numbers.
They have only increased by about 16%.
Starting the graph at 0 illustrates this clearly.
This is what it looks like if the plot includes 0.

Here's another example, again from Fox News it's showing us
what would happen if Bush tax cuts expires for the top tax
rate it's comparing January 1, 2013, to the time in which this was broadcast.
We look at the bar plots, it looks like the January 1, 2013
is about 5 times bigger than the now bar plot.
Here's what it looks like with the appropriate plot, a much different
story.

When using position rather than length, then it's not necessary to include 0.
This is particularly the case when we want
to compare differences between groups relative to the variability seen
within the groups.
Here's an illustrative example showing country average life expectancies,
stratified into continents, in 2012.
In the plot on the left, which includes 0, the space between 0 and 43
adds no information, and makes it harder to appreciate
the between and within variability.
For this reason, on the plot on the right,
we restrict the range to include the points.

---

#### Section 5: Data Visualization Principles   5.1 Data Visualization Principles, Part 1   Do Not Distort Quantities

Our next principle is do not distort quantities.
Here's an example.
During President Barack Obama's 2011 State Of The Union address,
the following chart was used to compare the US GDP
to the GDP of four competing nations.
Note that judging by the area of the circles,
the US appears to have an economy over 5 times larger than China,
and over 30 times larger than France.

However, when looking at the actual numbers,
one sees that this is not the case.
The actual ratios are 2.6, and 5.8 times bigger than China and France
respectively.

The reason for this distortion is that the radius, rather than the area,
was made to be proportional to the quantity, which
implies that the proportions between the areas is squared.
So 2.6 turns into 6.5, and 5.8, turns into 34.1.
Here's a comparison of the circles we get
if we make the values proportional to the radius, that's on the left, and so
the area, that's on the right.

Not surprisingly, ggplot defaults to using area rather than the radius.
Of course, in this case, we really should not be using area at all,
since we can use position and length.
Here's the bar plot comparing the GDPs.

---

#### Section 5: Data Visualization Principles   5.1 Data Visualization Principles, Part 1   Order by a Meaningful Value

Our next principle is order by a meaningful value.
When one of the axes is used to show categories, as done bar plots,
the default ggplot behavior is to order the categories alphabetically
when they are defined by character strings.

If they are defined by factors, they are ordered by the factor levels.
But remember, factor levels default to ordering by alphabetical order.
We rarely want to use alphabetical order.
It's arbitrary.
Instead, we should order by a meaningful quantity.

In all the cases discussed, the bar plots
were ordered by the values being displayed.
The exception was the graph showing bar plots comparing browsers.
In this case, we kept the order the same across the bar plots
to ease the comparison.
Instead, we ordered by the average value of 2000 and 2015.
We previously learned how to use the reorder function, which
helps achieve this goal.

To appreciate how the right order to help convey a message,
suppose we want to create a plot to compare the murder rates across states.
We're particularly interested in the most dangerous and the safest states.
Note the difference when we order alphabetically, the default,
versus when we order by the actual rate.

The information we want is much easier to extract from the plot on the right.
Note that the reorder function lets us reorder groups as well.
In an earlier video, we saw an example related to income distribution
across regions.
Here are those two plots again.
In the first one, we simply order alphabetically.
In the second one, we order by the median value of each group.
So, we've seen a few examples of how ordering by meaningful values
make much better graphs.

---

#### Section 5: Data Visualization Principles   5.1 Data Visualization Principles, Part 1   Assessment: Data Visualization Principles, Part 1

In this video, we describe yet another principal,
show the data.
We have focused on displaying single quantities across categories.
We now shift our attention to this plane data with a focus on comparing groups.
To motivate this principle, we go back to our official example
describing heights to an extraterrestrial, ET.
This time, let's assume ET is interested in the difference
in heights between males and females.

A commonly seen plot used for comparison between groups,
popularized by software such as Microsoft Excel,
shows the average and the standard error.
Now keep in mind, standard errors, which we define later,
are not the same as standard deviations.

Here's what the plot looks like.
The average of each group is represented by the top of each bar,
and the antenna that we see that expands out
is the average plus two standard errors.
If all ET receives is this plot, he will have
little information on what to expect if he meets
a group of humans, males and females.
Note that the bars go to 0.
Does this mean there are tiny humans measuring less than one foot?
Are all males taller than the tallest female?
Is there a range of heights?
ET can't answer these questions since we have provided almost no information
on the height distribution.

This brings us back to our principle, show the data.
The following simple ggplot code already generates a more informative plot
than the bar plot by simply showing all the points.
Just this little line of code shows you the points, the heights for females
and the height for males.
However, this plot has limitations as well.
Since we can't really see all the 216 and 708 points
plotted for females and males, respectively.
And many points are plotted above each other
so we don't know how many there are.
As we have described, visualizing the distribution is much more informative.
But before doing this, we point out two ways
we can improve a plot showing all the points.

The first is to add **jitter**.
Jitter is adding a small random shift to each point.
In this case, adding horizontal jitter does not
alter the interpretation since the height of the points doesn't change.
But we minimize the number of points that fall on top of each other
and, therefore, get a better sense of how many points there are
and how the data is distributed.

A second improvement comes from using **alpha blending**,
making the point somewhat transparent.
Without alpha blending, the more points fall
on top of each other, the darker the plot gets
in that region, which also helps us get a sense of how
the points are distributed.
Here's the same plot with jitter and alpha blending.
It just requires us to change a couple of arguments,
and immediately we get a much better sense of what
the distribution of the data is.
Now since there are so many points, it is
more effective to show distribution rather than show individual points.
In the next video, we'll show distributions,
and we'll learn some principles of how to best compare these distributions.


---
#### Section 5: Data Visualization Principles   5.2 Data Visualization Principles, Part 2   Ease Comparisons: Use Common Axes

Earlier, we saw this plot used
to compare male and female heights.
We criticized it for not showing all the data.
Now in this case, showing all the data is not
as effective as showing distributions.
We can add the points, but we want to show the distributions.
So let's start by creating histograms for each group.
They look like this.

However, from this plot it's not immediately obvious
that males are, on average, taller than females.
We have to look carefully to notice that the x-axis has a higher
range of values in the male histogram.
This brings us to another important principle,
and it's to keep the axes the same when comparing data across plots.
Know how the comparison becomes easier when we keep the axes the same.
Now we do see that the male histogram is shifted to the right,
compared to the female histogram.

Now, there's another principle that we need
to follow here to make it even easier to compare,
and it's to **align plots vertical to see horizontal changes**,
and **horizontally to see vertical changes**.

In these histograms, the visual cue related to decrease
or increasing height, are shifts to the left, or right, respectively.
Horizontal change, aligning in the plots vertically,
helps us see the change when the axes are fixed.
Look at how much easier it is to see it now.
This one clearly shows that the male heights are shifted to the right.
If instead of histograms, we want the more compact summary provided
by box plots, then we align horizontally,
since by default box plots move up and down with changes in height,
vertically.

Here are the two box blocks plotted next to each other, horizontally
next to each other.
We can appreciate the vertical changes.
In this case, we also add all the data, all the points,
using jitter and alpha blending.
Now contrast and compare these three plots, based on exactly the same data.
Note how much more we learn from the two plots on the right.
Bar plots are useful for showing one number,
but not very useful when wanting to describe distributions.


---

#### Section 5: Data Visualization Principles   5.2 Data Visualization Principles, Part 2   Consider Transformations

Another important principle
is to consider transformations.
We have motivated the use of the log transformation
in cases where the changes are multiplicative.
Population size was an example in which we found a log transformation
to yield a more informative plot.

The combination of incorrectly using bar plots,
when a log transformation is merited, can be particularly distorting.
As an example, consider this bar plot showing the average population sizes
for each continent in 2015.

From this plot, one would conclude that countries in Asia
are much more populous than other continents.
Following the show-the-data principle, we quickly
notice that this is due to two very large countries, which
we assume are India and China.
You can see those two points way up there.
Those are those two countries.

Here, using a log transformation provides a much more informative plot.
We compare the original bar plot to a box plot
using the log-scale transformation for the y-axis.
Note how much more informative that box plot is over the bar plot.
In fact, we see that Africa has a higher median population size than Asia.
Other transformations you should consider are the logistic
transformation-- useful to better see full changes in odds--
and the square root transformation, useful for count data.

---

#### Section 5: Data Visualization Principles   5.2 Data Visualization Principles, Part 2   Ease Comparisons: Compared Visual Cues Should Be Adjacent

In this video we introduce yet another principle.
It helps us ease comparison.
And the principle is that visual cues to be compared should be accurate.
When comparing income data between 1970 and 2010, across regions,
we made a figure similar to this one.
A difference is that here, we look at continents, instead of regions.
But this is not relevant to the point we're trying to make.
Note that for each continent, we want to compare
the distribution from 1970 to 2010.

The default in ggplot is to order alphabetically.
So the labels with 1970 come before the labels
with 2010, making that comparison challenging.
Know how much easier it is to make the comparison when the box blocks that we
want to compare are next to each other.

Here, we have reordered them to do just that.
The comparison becomes even easier if we use color to distinguish 1970 to 2010.
Using color is another way to ease comparisons.
When picking colors, keep in mind that about 10% of the population
is color blind.

Unfortunately, the default colors used in ggplot
are not optimal for this group.
However, ggplot does it make it easy to change
the color palette used in the plots.

Here's an example of how we can use a color
blind friendly palette using ggplot.

    color_blind_friendly_colors <- c("#999999","#E69F00","#5EB4E9","#009E73","#F0E442","#0072B2","#D55E00","#CC79A7")
    
    p1 <- data.frame(x=1:8, y=1:8, col = as.character(1:8)) %>% ggplot(aes(x,y,color=col)) + geom_point(size=5)
    
    p1 + scale_color_manual(values = color_blind_friendly_colors)


You can see that by adding some layers, and picking the right colors,
we can, in fact, make the use of color blind friendly colors.

---

#### Section 5: Data Visualization Principles   5.2 Data Visualization Principles, Part 2   Assessment: Data Visualization Principles, Part 2


---

#### Section 5: Data Visualization Principles   5.3 Data Visualization Principles, Part 3   Slope Charts

 In every single instance
in which we have examined the relationship between two variables,
total murders versus population size, life expectancy versus fertility rates,
and child mortality versus income, we have used scatterplots.
This is the plot we generally recommend.
One exception where another type of plot may be more informative
is when you are comparing variables of the same type
but at different time points and for a relatively small number of comparison.
For example, comparing life expectancy between 2010 and 2015.

In this case, we might consider a slope chart.
There's no geometry for slope charts in ggplot2,
but we can construct one using geom_lines.
We need to do some tinkering to add labels and some other changes.
The code looks something like this.
This piece of code produces the following slope chart.

An advantage of the slope chart is that it
permits us to quickly get an idea of changes
based on the slope of the lines.
Note, that we're using angle as a visual cue,
but we also have position to determine the exact values.
Comparing the improvement is a bit harder when we use the scatterplot.
Note that in the scatterplot, we have followed
the principle use common axes since we are comparing values before and after.
Now, note that when we have many points, the slope charge
stops being useful because it becomes too cluttered, and in this case,
we would use a scatterplot.

Finally, we're going to describe the **Bland-Altman plot**.
Since what we're interested in is in differences,
it makes sense to dedicate one of our axes to differences.
The Bland-Altman plot, also known as the **Two Key Mean Different plot**, and also
the **MA plot**, shows the difference versus the average.
Here's an example.
Here we quickly see which countries have improved the most
as it's represented in the y-axis.
We also get an idea of the overall value from the x-axis.


---

####  Section 5: Data Visualization Principles   5.3 Data Visualization Principles, Part 3   Encoding a Third Variable

 We previously showed a scatterplot
showing the relationship between infant survival rates and average income.
Here's a version of this plot where we encode
three more variables, OPEC membership, region, and population size.
Note that we encode categorical variables with color hue and shape.
These shapes can be controlled with a shape argument.
Here are the shapes available for use in R. Note that for the last five,
the color goes inside.
For continuous variables, we can use color, intensity, or size.
In the next video, we're going to show a case
study that demonstrates how to do this.


---

#### Section 5: Data Visualization Principles   5.3 Data Visualization Principles, Part 3   Case Study: Vaccines

Vaccines have helped save millions of lives.
In the 19th century, before herd immunization
was achieved through vaccination programs,
deaths from infectious diseases, like smallpox and polio, were common.
However, today, despite all the scientific evidence
for their importance, vaccination programs
have become somewhat controversial.
The controversy started with a paper published in 1988
and led by Andrew Wakefield claiming there
was a link between the administration of the measles, mumps, and rubella MMR
vaccine, and the appearance of autism and bowel disease.
Despite much scientific evidence contradicting this finding,
sensationalist media reports and fear mongering from conspiracy theories
lead parts of the public to believe that vaccines were harmful.
Some parents even stopped vaccinating their children.

This dangerous practice can be potentially disastrous,
given that the Center for Disease Control, CDC,
estimates that vaccination will prevent more than 21
million hospitalizations and 732,000 deaths
among children born in the last 20 years.

The 1988 paper has since been retracted, and Andrew Wakefield
was eventually struck off the UK medical register with a statement
and identifying deliberate falsification in the research published
in The Lancet, and was thereby barred from practicing medicine in the UK.
Yet misconceptions persist, in part, due to self-proclaimed activists
that continue to dispel misinformation about vaccines.
Effective communication of data is a strong antidote
to misinformation and fear mongering.

Earlier we showed an example provided by the Wall Street Journal
showing data related to the impact of vaccines
on battling infectious diseases.
Here we reconstruct that example.
The data used in these plots were collected, organized,
and distributed by the Tycho project.
They include weekly reported counts data for 7 diseases from 1928 to 2011
from all 50 states.
We include the yearly totals in the DS labs package.
You can get it like this and look at the structure using this command.

    data(us_contagious_diseases)
    str(us_contagious_diseases)

For the plot there we're going to make in this video,
we create a temporary object called dat that stores all the measles data.
It includes a per 100,000 rate, orders states by average value of disease,
and removes Alaska and Hawaii, since they only
became states in the late 50s.
    
    the_disease <- "Measles"
    dat <- us_contagious_diseases %>% 
           filter(!state %in% c("Hawaii", "Alaska") & disease == the_disease) %>%
           mutate(rate = count / population * 10000) %>%
           mutate(state = reorder(state, rate))

Here's the code where we define that.
We can now easily plot disease rates for per year.
Here are the measles data for California.

    dat %>% filter(state == "California") %>%
            ggplot(aes(year, rate)) + 
            geom_line() + ylab("Cases per 100,000") +
            geom_vline(xintercept = 1963, col = "blue")


We can use this simple code to show it.
We add a vertical line at 1963, since this
is when the vaccine was introduced.
Now can we show data for all states in one plot?
We have three variables to show, year, state, and rate.
In the Wall Street Journal figure, they use the x-axis
for year, the y-axis for state, and color hue represent rates.
We're using color to represent a continuous variable.

However, the color scale they use, which goes
from yellow to blue to green to orange to red can be improved.
When choosing colors to quantify a numeric variable,
we choose between two options, `sequential` and `diverging`.

Sequential palettes are suited for data that goes from high to low.
High values are clearly distinguished from the low values.
Here are some examples offered by the package R color Brewer.
    
    library(RColorBrewer)
    display.brewer.all(type = "seq")

On the other hand, diverging colors are used
to represent values that verge from a center.
We put equal emphasis on both ends of the data range, higher than the center
and lower than the center.
An example of when we would use a divergent pattern
would be if we were to show heights and standard deviations away
from the average.

    > display.brewer.all(type = "div")

Here is an example of divergent patterns available from R Color Brewer.
In our example, we want to use a sequential palette
since there is no meaningful center, just low and high rates.
We use the geometry 

    geom_tile()

to tile the region
with colors representing disease rates.
We use square root transformation to avoid
having the really high counts dominate the plot.
Here's the code that generates a very nice and impactful plot.

    dat %>% ggplot(aes(year, state, fill = rate)) + 
            geom_tile(color="grey50") + 
            scale_x_continuous(expand=c(0,0)) +
            scale_fill_gradientn(colors = brewer.pal(9, "Reds"), trans = "sqrt") + 
            geom_vline(xintercept=1963, col="blue") + 
            theme_minimal() + theme(panel.grid = element_blank()) +
            ggtitle(the_disease) +
            ylab("") +
            xlab("")

This plot makes a very striking argument for the contribution of vaccines.
However, one limitation of this plot is that it uses color
to represent quantity, which we earlier explained makes it a bit harder to know
exactly how high it is going.

Position and lens are better cues.
If we are willing to lose data information,
we can make a version of the plot that shows the values with position.
We can also show the average for the US, which we compute like this.
    
    avg <- us_contagious_diseases %>% 
           filter(disease == the_disease) %>% group_by(year) %>%
           summarize(us_rate = sum(count, na.rm=TRUE) / sum(population, na.rm=TRUE) * 10000)

Now to make the plot, we simply use the geom_line geometry.
We are going to make every state the same color.
This is because it's harder to choose 50 distinct colors.
However, the plot is very impactful.
It shows very clearly how after the vaccine was introduced
the rates went down across all states.
It shows the same information as our previous plot,
but now we can actually see what the values are.


--- 

#### Section 5: Data Visualization Principles   5.3 Data Visualization Principles, Part 3   Avoid Pseudo and Gratuitous 3D Plots

Here we describe an important data visualization principle.
Avoid pseudo three dimensional plots.
The figure we show here was taken from the scientific literature.
It shows three variables, dose, drug type, and survival.
Although when you look at a plot, you're almost always
looking at a screen or a book page, which
are both flat and two dimensional, this plot tries to imitate three-dimension
and assigns a dimension to each variable.

Humans are not good at seeing in three dimensions.
Think about how hard it is to parallel park.
And our limitation is even worse when it's pseudo three-dimensional,
as it is when you put it on a page or a web page.
To see this, try to determine the value of the survival variable in the plot.
Can you tell when the purple ribbon intersects the red one?

This is an example in which it's easy to use color
to represent the categorical variable.
We can make the plot like this.
This plot demonstrate that using color is more than
enough to distinguish the three lines.
Pseudo 3D is somewhat used completely gratuitously.
Plots are made to look 3D, even when the third dimension does not
represent any quantity.

This only adds confusion and makes it harder to relay your message.
Here is an example.
This is a three-dimensional bar plot.
The third dimension adds nothing, only confusion.
So in general, avoid pseudo 3D plots, and even more avoid
gratuitous 3D plots.


---

#### Section 5: Data Visualization Principles   5.3 Data Visualization Principles, Part 3   Avoid Too Many Significant Digits

 By default, statistical software like R
returns many significant digits.
The principle we're about to discuss relates to tables, not graphs,
and it's to avoid too many statistical digits.

The default behavior in R is to show seven significant digits.
So many digits often adds no information,
and the visual clutter makes it hard for the consumer of your table
to understand the message.

As an example, here the per 10,000 disease
rates for California across five decades.
We are reporting positions of up to 0.00001 cases per 10,000,
a very small value in the context of the changes that are occurring across
the dates.
In this case, two significant figures is more than enough and makes the point
that the rates are decreasing.

Useful functions in R to change the number of significant digits
or to round numbers are 

    signif

and
    round

You can define the number of significant digits
to use globally by setting an option.
You can do it like this.

    options(digits=n)

Another principle related to displaying tables
is the place values being compared on columns rather than rows.
Here's what the table would look like if we placed the numbers being
compared horizontally.
It's a little bit harder to make the comparison.

---

#### Section 5: Data Visualization Principles   5.3 Data Visualization Principles, Part 3   Assessment: Data Visualization Principles, Part 3