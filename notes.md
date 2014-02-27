#Maps and Charts in R: Real newsroom examples#

##Goals##

This class will use R to: 

1. Grab data and visualize it as a reporting tool.
2. Create broad descriptive charts.
3. Create thousands of small charts idea for reporting questions.
4. Create maps to add a geospatial dimension to analysis.
5. Demystify R. 

The data used for this class are [hosted on GitHub](https://github.com/mattwaite/MapsAndChartsInR). Not a GitHub user or git whiz? Follow the link and look on the right side of the page for a button that says Download Zip. Click that and rest easy.

##Data visualization as a reporting tool##

Often we find that the trend is not immediately apparent from a set of data until we visualize it in some way. R is a fantastic tool for data analysis AND data visualization. But it does take some getting used to.

What makes R so great?

1. Power. R can do so many things, and people are writing new libraries for it all the time. You won't threaten learning all there is to know about R anytime soon.
2. Integration. You have a way to analyze AND visualize in one place.
3. Repetition. You can repeat the things you've done, which is particularly useful if you have a regularly updating dataset.
4. Iteration. When you learn enough R, you can try, tweak, try, tweak, try very easily.

Let's first take care of some housekeeping.

First, these materials are on GitHub. You are free to download them and use them as you wish. 

Second, we need to install some packages. Here's what you'll need: 

`install.packages("ggplot2")`

`install.packages("maps")`

`install.packages("maptools")`

Third, we need to get some data. Go here: 

url

##Simple charts##

Often, when we get a dataset, simple or huge, we want to get some idea of what we've got. The first step in any R analysis is to run some summary statistics on the data. In R, that's really simple.

First, let's load some data into our environment. 

`enrollment <- read.csv("~/Dropbox/Presentations/MapsAndChartsInR/data/enrollment.csv")`

This creates a variable, called a data frame, called enrollment. It's the enrollment at the University of Nebraska-Lincoln going back to 1967. Our chancellor has pledged to increase enrollment to 30,000. A reasonable question is what is the history here? 

So the first thing we can do is view the data:

`View(enrollment)`

A second thing we can do is run some summary statistics on it:

`summary(enrollment)`

Why do this? It's going to help you find out if your data is flawed. Are there numbers far outside of what would be normal? Are the measures of central tendency what you expect? If they aren't there's some red flags that you've got bad data.

But does this tell us anything about the history of the university? Sort of. That max figure is interesting. The average number of students is interesting (more so when you see the graph). But, speaking of graphs, lets look at one.

R comes with built-in chart libraries, but a better alternative is the ggplot2 library.

Let's make a basic bar chart. You do that like this:

`ggplot(data=enrollment, aes(x=Year, y=Students)) + geom_bar(stat="identity")`

So, let's break this down. First, we call the ggplot library. Next we tell it what data we're using. Next we define our axes. After that, we tell it we want to use the geometric shape of a bar chart. Inside that, we say we want to use some value as the height of the bar. We do this by saying the stat is identity, not bin, which is just a count of a thing. Bar charts in R are usually used for histograms that count a thing. We're using it for something else.

We can play with this a little. Disturbed by it being in black and white? 

`ggplot(data=enrollment, aes(x=Year, y=Students, fill="red")) + geom_bar(stat="identity")`

We add fill="red" and boom. Color. We're now USAToday in the 80s. 

Not sophisticated enough for your data visualization tastes? Try this: 

`ggplot(data=enrollment, aes(x=Year, y=Students, fill=Year)) + geom_bar(stat="identity")`

Ooh, gradients. 

What does the chart tell us about enrollment at UNL? 

What if we wanted to tell this as a line chart? 

`ggplot(data=enrollment, aes(x=Year, y=Students)) + geom_line()`

See that? We really just change the geom to geom_line. That's it. But is that chart accurate? What's wrong with it?

How do we fix this?

`ggplot(data=enrollment, aes(x=Year, y=Students)) + geom_line() + ylim(0, max(enrollment$Students))`

What that says is we limit the y axis, from 0 to the max of a given variable, in our case the max enrollment. 

##Lattice charts##

Sometimes, we need to see a little chart for a lot of things to see where the trends we want to see are changing the most. R has fantastic tools for this. The first I saw were called Lattice Charts. ggplot has something called facet wrapping. 

Let's grab a new dataset. Lets import a file called big schools. It's a file of the graduation rates of every university in the US with more than 20000 students going back to 2001. 

`bigschools <- read.csv("~/Dropbox/Presentations/MapsAndChartsInR/data/bigschools.csv")`

And view it:

`View(bigschools)`

What we'd like to do is see all of the graduation rate trends together -- one graph for each school. It's actually really simple with ggplot

`ggplot(data=bigschools, aes(x=academicyear, y=grad_rate_150_p)) + geom_bar(stat="identity") + facet_wrap(~ instname)`

See the facet_wrap parts?

Are bars the clearest way to do this? How about a line?

`ggplot(data=bigschools, aes(x=academicyear, y=grad_rate_150_p)) + geom_line() + facet_wrap(~ instname)`

##Using R as a GIS##

Another form of data visualization is a map. 