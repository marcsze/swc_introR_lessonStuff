# R for Reproducible Data Science Lesson Plan Day 2
**Originally designed for a SWC session from October 27th - October 28th**

* First we need to install and load the necessary packages for the days lesson
  * Gapminder data set
    * `install.packages("gapminder")` then `library(gapminder)` then `gap_data <- gapminder`
  * Install ggplot2, plyr, tidyr, and dplyr
    * `install.packages(c("ggplot2", "plyr", "tidyr", "dplyr"))`
 
 
### Creating Publication-Quality Graphics

* load our plotting package `library("ggplot2")`
* ggplot2
  * very effective packge for creating publication quality graphics
  * built on Hadley Wickham's grammar of graphics
    * breaks down graphing into three main components
      * a data set
      * a coordinate system
      * a set of "geoms" or the visual representation of data points
  * need to think of a figure in layers
    * similar in idea as how programs like photoshope, Illustrator, or Inkscape approach the problem
  
* Let's get right into it and try graphing data from the gapminder data
  * `ggplot(data = gap_data, aes(x = gdpPercap, y = lifeExp)) + geom_point()`
    * need to break down this command to highlight the component parts of the function
      * data set, coordinate system, "geom"
   * `ggplot` function lets R know that we’re creating a new plot
      * all arguments are global options and are applied to every layer
   * Two arguments have been passed
     * what data we want
     * aesthetic properties (x and y) 
       * didn't need to call something like `x = gap_data[, "gdpPercap"])` smart enough to look for the data frame
  * what happens if we only use the `ggplot` command without the corresponding `geom_point()` call?
    * `ggplot(data = gapminder, aes(x = gdpPercap, y = lifeExp))`
    * So this is interesting. Need to tell ggplot how to visualize the data 
      * `geom_point()` in our case represents scatterplot of points
  * plot `ggplot(data = gap_data, aes(x = gdpPercap, y = lifeExp)) + geom_point()` on more time to bring it all together
  
**Challenge 1**

Modify the previous example that we used so that the new plot shows how life expectancy has changed over time.
 * modify `ggplot(data = gap_data, aes(x = gdpPercap, y = lifeExp)) + geom_point()`
 * to `ggplot(data = gap_data, aes(x = year, y = lifeExp)) + geom_point()`
 
**Challenge 2**

In the previous examples and challenge we’ve used the aes function to tell the scatterplot geom about the x and y locations of each point. Another aesthetic property we can modify is the point color. Modify the code from the previous challenge to color the points by the “continent” column. 

 * First, see if the learners can attempt on their own
 * Second, if they have troubles provide hint
    * `ggplot(data = gap_data, aes(x = year, y = lifeExp, color=_________)) + geom_point()`
 
    
What trends do you see in the data? Are they what you expected?

**Layers**

Scatterplot may not be the best way to visualize change over time
 * Let's try a line plot instead
   * `ggplot(data = gap_data, aes(x=year, y=lifeExp, by=country, color=continent)) + geom_line()`

 * Notice how we have changed the `geom_point` to a `geom_line` 
   * tells ggplot to draw a line for each country instead of a point

 * What would we do if we wanted to have both lines and points on the plot?
   * Ask class for response before showing the code
   * `ggplot(data = gap_data, aes(x=year, y=lifeExp, by=country, color=continent)) + geom_line() + geom_point()`

![graph](http://swcarpentry.github.io/r-novice-gapminder/fig/rmd-08-lifeExp-line-point-1.png)

 * go back to how each layer (lines and points) are drawn on top of the previous layer
    * in this example points on top of lines
    * can highlight this with 
       * `ggplot(data = gap_data, aes(x=year, y=lifeExp, by=country)) + geom_line(aes(color=continent)) + geom_point()`
       
![graph2](http://swcarpentry.github.io/r-novice-gapminder/fig/rmd-08-lifeExp-layer-example-1-1.png)
   
 * on top of highlighting how layers are done the above code also highlights another important point
    * color has been moved from global plot options so that it is now only applied to lines
    * since the plain `geom_point()` draws from global we no longer have colour defined and defaults to black
    
 
**Challenge 3**
Switch the order of the point and line layers from the previous example. What happened?
  * `ggplot(data = gap_data, aes(x=year, y=lifeExp, by=country)) + geom_point() + geom_line(aes(color=continent))`

**Transformations and Statistics**

 * Another nice thing about ggplot2 is that it makes it relatively easy to overaly statistical models
 * first example
   * `ggplot(data = gap_data, aes(x = gdpPercap, y = lifeExp, color=continent)) + geom_point()`

 * Relatively hard to see the relationship due to outliers
   * We can change the scale on the x axis using the `scale` function
   * It is also possible to change the transparency of the points (which can be helpful when we have a lot of data)
      * `ggplot(data = gap_data, aes(x = gdpPercap, y = lifeExp)) + geom_point(alpha = 0.5) + scale_x_log10()`
          * So what happened here?
              * the log 10 function applied a transformation to the gdpPercap values before displaying on the plot
              * so each multiple of 10 now corresponds to an increase of 1 on the new scale
                  * 10,000 is now 4 while 1,000 is now 3
   
* We can also now fit a simple relationship to the data by adding another layer `geom_smooth`
   * `ggplot(data = gap_data, aes(x = gdpPercap, y = lifeExp)) + geom_point() + scale_x_log10() + geom_smooth(method="lm")`

* We can make the line thicker by adding specifics to the `geom_smooth()` 
  * `ggplot(data = gap_data, aes(x = gdpPercap, y = lifeExp)) + geom_point() + scale_x_log10() + geom_smooth(method="lm", size=1.5)`
  * ask what would happen if we added the size argument to the `geom_point()` instead
  * note that there are 2 ways an aesthetic can be specified 
     * within `aes` as we have done previously
     * as an argument passed to `geom_smooth`
  

**Challenge 4a**
Modify the color and size of the points on the point layer in the previous example.
*Hint: do **not** use the aes function.*
  * `ggplot(data = gap_data, aes(x = gdpPercap, y = lifeExp)) + geom_point(size=3, color="orange") + scale_x_log10() + 
 geom_smooth(method="lm", size=1.5)`
 
 * The colour is not important and need to remind class that in R and ggplot2 different ways to specify colour
    * can use actual names "blue", "green", etc. R and ggplot2 are smart enough to figure out what you mean
    * can use specific colour codes e.g. "#FF0000FF"
    
**Challenge 4b**
Modify your solution to Challenge 4a so that the points are now a different shape and are colored by continent with new trendlines. *Hint: The color argument **can be used** inside the aesthetic.*
    
* `ggplot(data = gap_data, aes(x = gdpPercap, y = lifeExp, color = continent)) + geom_point(size=3, pch=17) + scale_x_log10() + geom_smooth(method="lm", size=1.5)`

  * Need to go over the `pch` (plotting character) argument and that it can control shapes
  
  ![graph2](http://www.statmethods.net/advgraphs/images/points.png)


**Multi-panel figures**

* We can split our life expectancy by country as either one plot or as multiple ones
* This is done by adding a facet panel
* This example will specifically focus on countries that start with the letter "A" or "Z"

* Here we are going to introduce the `substr` function 
* This will pull out a character string (go over a string if we haven't talked about it already)
   * It will look at characters that occur within the given start and stop position
      * so a `start = 1` and a `stop =1` would limit everything to only the first character in the string
      * the special operator `%in%` allows us to make multiple comparisons instead of doing long subsetting conditions
        * e.g. `starts.with %in% c("A", "Z")` versus `starts.with == "A" | starts.with == "Z"`

* This is a multi step problem
  * First we need to pull out the first character from every country
       * `starts.with <- substr(gap_data$country, start = 1, stop = 1)`
  * Second we then need to subset the gapminder data based on this information
       * `az.countries <- gap_data[starts.with %in% c("A", "Z"), ]`
  * Third we then can graph the subsetted data and seperate it out by country
       * `ggplot(data = az.countries, aes(x = year, y = lifeExp, color=continent)) + geom_line() + facet_wrap( ~ country)`

* `facet_wrap` takes a “formula” as its argument (~) 
   * This tells R to draw a panel for each unique value in the country column of the gapminder dataset
   
** Modifyiing Text**

* We can make modifications to the text to make the figure more publication ready
  * Some changes we are going to make include
      * remove clutter of x-axis
      * change the y-axis title
      * Capitilize the legend title
      
   * To make these changes we are going to use `theme`, `scale`, and `lab` arguments

   * `ggplot(data = az.countries, aes(x = year, y = lifeExp, color=continent)) +
  geom_line() + facet_wrap( ~ country) +
  xlab("Year") + ylab("Life expectancy") + ggtitle("Figure 1") +
  scale_colour_discrete(name="Continent") +
  theme(axis.text.x=element_blank(), axis.ticks.x=element_blank())`

     * Need to walk through point by point exactly what `xlab`, `ylab`, `ggtitle`, `scale_colour_discrete`, and `theme` do
         * need to specifically cover `axis.text.x`, `axis.ticks.x`, and what `elecment_blank()` does

* Final Preamble from SWC
This is a taste of what you can do with ggplot2. RStudio provides a really useful cheat sheet of the different layers available, and more extensive documentation is available on the ggplot2 website. Finally, if you have no idea how to change something, a quick Google search will usually send you to a relevant question and answer on Stack Overflow with reusable code to modify!

**Challenge 5**
Create a density plot of GDP per capita, filled by continent.
*Advanced: - Transform the x axis to better visualise the data spread. - **Add a facet layer to panel the density plots by year.***

   * `ggplot(data = gap_data, aes(x = gdpPercap, fill=continent)) + geom_density(alpha=0.6) + facet_wrap( ~ year) + scale_x_log10()`




### Split-Apply-Combine

* Go over an example function that multiplies the population and GDP per capita column

`# Takes a dataset and multiplies the population column
# with the GDP per capita column.
calcGDP <- function(dat, year=NULL, country=NULL) {
  if(!is.null(year)) {
    dat <- dat[dat$year %in% year, ]
  }
  if (!is.null(country)) {
    dat <- dat[dat$country %in% country,]
  }
  gdp <- dat$pop * dat$gdpPercap

  new <- cbind(dat, gdp=gdp)
  return(new)
}`

* A common problem you will run into is that you'll want to run calculations on different groups within the data
* This function can be applied to all the data but what if we want mean GDP per continent?

* One possible solution is to take the mean of each continent (this is really efficient though)

  * `withGDP <- calcGDP(gapminder) 
  mean(withGDP[withGDP$continent == "Africa", "gdp"])`

  * `mean(withGDP[withGDP$continent == "Americas", "gdp"])`
  * `mean(withGDP[withGDP$continent == "Asia", "gdp"])`
  
  * We always want to try and reduce the amount of repetition 
     * Saves time
     * Can avoid bugs (e.g. typos, etc.)
  * Could right a new function, but this can take time
  * The problem that we are grappling with is what this lesson is called
     * "split-apply-combine"
     
 ![split_apply_graph](http://swcarpentry.github.io/r-novice-gapminder/fig/splitapply.png)
 
 * We want to split our data into groups, in this case continents, apply some calculations on that group, then optionally combine the results together afterwards
 
    * Need to bring up that we have seen some of this when we covered the `apply` function
    
**The plyr package**

* One way of solving this problem is with the base function `apply` introduced earlier.

* Another way of solving this problem is with the `plyr` package
   * In general these functions are more user friendly but the choice on which to use is ultimately up to you.
   
   * `library(plyr)`
   * Can operate on lists, data frames, and arrays.  Each of these performs
      * A splitting operation
      * Apply a function on each split in turn
      * Recombine output data as a single data object
   * Function names are based on the data structure expected as input and the data structure returned as output
      * [a]rray, [l]ist, [d]ata.frame
          * the first letter corresponds to the input data structure
          * the second letter to the output data structure
          * rest of function is named "ply"
          
      * In total 9 core function for \*\*ply
      * There are an additional three functions which will only perform the split and apply steps, and not any combine step
         * They’re named by their input data type and represent null output by a _ (see table)
      * An array in ply can include a vector or matrix
      
 ![table_ply](http://swcarpentry.github.io/r-novice-gapminder/fig/full_apply_suite.png)
 
 
 * Each of the `ply` functions (`daply`, `ddply`, `llply`, `laply`) have the same structure and has 4 key features and structures
     * `xxply(.data, .variables, .fun)`
        * The first letter of the function name give the input type and the second gives the output type
        * `.data` gives the data object to be processed
        * `.variable` identifies the splitting variables
        * `.fun` gives the function to be called on each piece
        
 * This allows us to quickly calculate the mean GDP per continent
    * `ddply(
    .data = calcGDP(gap_data), 
    .variables = "continent", 
    .fun = function(x) mean(x$gdp))`
          
    * If we take a look at the previous code and walk through line by line
        * The ddply function feeds in a data.frame (function starts with d) and returns another data.frame (2nd letter is a d)
        * the first argument we gave was the data.frame we wanted to operate on: in this case the gapminder data 
             * We called calcGDP on it first so that it would have the additional gdp column added to it
        * The second argument indicated our split criteria: in this case the “continent” column. 
             * Note that we gave the name of the column, not the values of the column like we had done previously with subsetting. 
             * Plyr takes care of these implementation details for you
        * The third argument is the function we want to apply to each grouping of the data. 
             * We had to define our own short function here: each subset of the data gets stored in x, 
                  * the first argument of our function. 
                  * This is an anonymous function: 
                      * we haven’t defined it elsewhere, and it has no name. It only exists in the scope of our call to ddply
                      
* We can also change the output structure (e.g. a list)

  * `dlply(
 .data = calcGDP(gap_data),
 .variables = "continent",
 .fun = function(x) mean(x$gdp))`
 
 
 * plyr is also able to group by multiple columns (this is pretty cool!)
 
   * `ddply(
 .data = calcGDP(gap_data),
 .variables = c("continent", "year"),
 .fun = function(x) mean(x$gdp))`
             
* Like the previous example we can also change the output (input data.frame and output and array)

  * `daply(
 .data = calcGDP(gap_data),
 .variables = c("continent", "year"),
 .fun = function(x) mean(x$gdp))`
 
* One final cool thing is that these functions can take the place of `for` loops. Why do this?
   * They are faster than `for` loops
   * To make the replacement simply put the code that was in the body of the for loop inside an anonymous function
   
      * `d_ply(
  .data=gap_data,
  .variables = "continent",
  .fun = function(x) {
    meanGDPperCap <- mean(x$gdpPercap)
    print(paste(
      "The mean GDP per capita for", unique(x$continent),
      "is", format(meanGDPperCap, big.mark=",")))
    }
      )`
 
      * Need to go over each part of this code step by step and break it down for the learners so that they understand it
      * Mention the `format` function as a good way of making numeric values look "pretty"


**Warm Up Challenge**
Without running them, which of the following will calculate the average life expectancy per continent:

1)
`ddply(
  .data = gap_data,
  .variables = gapminder$continent,
  .fun = function(dataGroup) {
     mean(dataGroup$lifeExp)
  }
)`

2)
`ddply(
  .data = gap_data,
  .variables = "continent",
  .fun = mean(dataGroup$lifeExp)
)`

3)
`ddply(
  .data = gap_data,
  .variables = "continent",
  .fun = function(dataGroup) {
     mean(dataGroup$lifeExp)
  }
)`

4)
`adply(
  .data = gap_data,
  .variables = "continent",
  .fun = function(dataGroup) {
     mean(dataGroup$lifeExp)
  }
)`


**Challenge 1**
Calculate the average life expectancy per continent. Which has the longest? Which had the shortest?
`
ddply(
 .data = gap_data, 
 .variables = "continent", 
 .fun = function(x) mean(x$lifeExp))`

**Challenge 2**
Calculate the average life expectancy per continent and year. Which had the longest and shortest in 2007? Which had the greatest change in between 1952 and 2007?

`test <- ddply(
 .data = gap_data, 
 .variables = c("continent", "year"), 
 .fun = function(x) mean(x$lifeExp)
)`

* Answer longest and shortest
  * `test[test$year == 2007, ]`

* Answer greatest change (Need to see how class handles this one and let them know it is okay not to use `plyr` to answer)

1) 
`cbind(levels(test$continent), test$V1[test$year == 2007] - test$V1[test$year == 1952])`

2)
`ddply(
  .data = test, 
  .variables = c("continent"), 
  .fun = function(x) x$V1[x$year == 2007] - x$V1[x$year == 1952]
)`
 

**Advanced Challenge**
Calculate the difference in mean life expectancy between the years 1952 and 2007 from the output of challenge 2 using one of the plyr functions.

`ddply(
  .data = test, 
  .variables = c("continent"), 
  .fun = function(x) x$V1[x$year == 2007] - x$V1[x$year == 1952]
)`


### Dataframe Manipulation with dplyr

We often select observations or variables, group data by certain criteria or even calculate summary statistics.  Similar to the case with plyr many functions can be performed using base R operations.

`mean(gap_data[gap_data$continent == "Africa", "gdpPercap"])`

`mean(gap_data[gap_data$continent == "Americas", "gdpPercap")`

`mean(gap_data[gap_data$continent == "Asia", "gdpPercap"])`

Similar to the last situation this isn't great because there is a lot of repetition.  It can cost you time now and later and produce unnecessary errors.

**The dplyr package**

* Fortunately the `dplyr` package provides some useful functions that are very good and manipulating dataframes.
   * It does it in a way that limits repetition and reduces the probability of making errors
   * It might also save you time on some typing and it might also be easier to understand

* We are going to cover 6 of the more commonly used functions 
   * `select()`
   * `filter()`
   * `group_by()`
   * `summarize()`
   * `mutate()`
   
* Load the package
  * `library*dply)`
  
* If we only want to use a have a specific portion of our data frame we can use `select()`
   * `year_country_gdp <- select(gap_data, year, country, gdpPercap)`
      * Need to go over each component of the function after running
      
![select_overview](http://swcarpentry.github.io/r-novice-gapminder/fig/13-dplyr-fig1.png)

* We can peek at year_country_gdp and see that it only contains the columns that we chose
   * Although this is useful the true power of `dplyr` comes from being able to combine several functions using pipes
   * `year_country_gdp <- gap_data %>% select(year, country, gdpPercap)`
       * Walk through this step by step
          * First we summon the gapminder dataframe and pass it on, using the pipe symbol `%>%`
          * It is passed on to the next step, which is the select() function
          * Don’t need to specify which data object we use in the select() function since in gets that from the previous pipe

There is a good chance you have encountered pipes before in the shell. In R, a pipe symbol is %>% while in the shell it is | but the concept is the same!
   * Probably also go over why it is `%>%` since `|` represents "or" in R
   
**Using filter()**

* If we want the data frame from above but only European countries
   * combine `select()` and `filter()`
   * `year_country_gdp_euro <- gap_data %>% filter(continent=="Europe") %>% select(year,country,gdpPercap)`
       * Should also walk class through step by step to make sure they understand the concept
       
**Challenge 1**
Write a single command (which can span multiple lines and includes pipes) that will produce a dataframe that has the African values for lifeExp, country and year, but not for other Continents. How many rows does your dataframe have and why?

`year_country_lifeExp_Africa <- gap_data %>% filter(continent=="Africa") %>% select(year,country,lifeExp)`

* Talk about how order of operations is important since if `select` was first there would be no continents to search


**Using group_by() and summarize()**

So how can we reduce the repetitiveness? What we have at the moment may be more understandable but if we wanted every continent we would have to still write out code for each one.
  * `group_by()` can get around this by using every unique criteria available for `filter()`
      * `str(gap_data)` make sure it is a factor
      * `str(gap_data %>% group_by(continent))`
          * Output is a `grouped_df` 
          * A list where each iten in the list is a dataframe which contains only the rows that correspond to the particular value

![group_by_visual](http://swcarpentry.github.io/r-novice-gapminder/fig/13-dplyr-fig2.png)


** Using summarize()**

* The true power of `group_by()` can be realized when grouped with `summarize()`
   * Allow us to create new variable(s) by using functions that repeat for each of the continent-specific data frames
   * using the group_by() function, we split our original dataframe into multiple pieces, 
   * then we can run functions (e.g. `mean()` or `sd()`) within `summarize()`.
   * `gdp_bycontinents <- gap_data %>% group_by(continent) %>% summarize(mean_gdpPercap=mean(gdpPercap))`
   
![summarize_visual](http://swcarpentry.github.io/r-novice-gapminder/fig/13-dplyr-fig3.png)

**Challenge 2**
Calculate the average life expectancy per country. Which had the longest life expectancy and which had the shortest life expectancy?

`lifeExp_bycountry <- gap_data %>%
   group_by(country) %>%
   summarize(mean_lifeExp=mean(lifeExp))`
   
* We can really add to this power
  * Let's also group by both year and continent
  * `gdp_bycontinents_byyear <- gap_data %>% group_by(continent,year) %>% summarize(mean_gdpPercap=mean(gdpPercap))`

* And there is still more power offered
   * We are not limited by only one new variable in `summarize()`
   * `gdp_pop_bycontinents_byyear <- gap_data %>% group_by(continent,year) %>% summarize(mean_gdpPercap=mean(gdpPercap),
        sd_gdpPercap=sd(gdpPercap),
        mean_pop=mean(pop),
        sd_pop=sd(pop))`

**Using mutate()**

* Another neat feature is we can create new variables with `mutate()` before or after summarizing information
   * `gdp_pop_bycontinents_byyear <- gap_data %>% mutate(gdp_billion=gdpPercap*pop/10^9) %>%
    group_by(continent,year) %>% summarize(mean_gdpPercap=mean(gdpPercap),
       sd_gdpPercap=sd(gdpPercap),
       mean_pop=mean(pop),
       sd_pop=sd(pop),
       mean_gdp_billion=mean(gdp_billion),
       sd_gdp_billion=sd(gdp_billion))`

**Advanced Challenge**
Calculate the average life expectancy in 2002 of 2 randomly selected countries for each continent. Then arrange the continent names in reverse order. *Hint: Use the dplyr functions arrange() and sample_n(), they have similar syntax to other dplyr functions.*

`lifeExp_2countries_bycontinents <- gapminder %>%
   filter(year==2002) %>%
   group_by(continent) %>%
   sample_n(2) %>%
   summarize(mean_lifeExp=mean(lifeExp)) %>%
   arrange(desc(mean_lifeExp))`
   
* Might have to do this question with the class and walk through it together


### Dataframe Manipulation with tidyr

Need to get data for this section from the following source:

* `git remote add data https://github.com/resbaz/r-novice-gapminder-files`  
* `git pull data master`

* There are two main formats for data (wide and long)
  * Long format can be thought of as
      * each column is a variable
      * each row is an observation
      * should notice this is how ggplot2 like to have data inputed
  * Wide format can be thought of as
      * each row is a site/subject/patient
      * have multiple observation variables containing the same type of data
          * can be repeated observations over time 
          * or multiple variables (or a mix of both)
   * Although there is not right or wrong answer as to which to use
      * we utilize ggplot2 and other functions are formated to work on the long format
      
* We will work on how to transform your data using tidyr functions regardless of format

![example_wide_long](http://swcarpentry.github.io/r-novice-gapminder/fig/14-tidyr-fig1.png)

* Often time the wide format is easier for us to read
* In contrast the long format is easier for machines to read and aligns more closely with database designs
   * Ask if anyone has experience working with databases.
* ID variables are similar to fields in a database and observed variables are like database values

** Getting started**

* `library(c(dplyr, tidyr))`
* Take a look at the gapminder data set `str(gap_data)`

**Challenge 1**
Is gapminder a purely long, purely wide, or some intermediate format?
  * The original gapminder data.frame is in an intermediate format. 
  * It is not purely long since it had multiple observation variables (pop,lifeExp,gdpPercap)
  
* Comments from SWC

`Sometimes, as with the gapminder dataset, we have multiple types of observed data. It is somewhere in between the purely ‘long’ and ‘wide’ data formats. We have 3 “ID variables” (continent, country, year) and 3 “Observation variables” (pop,lifeExp,gdpPercap). I usually prefer my data in this intermediate format in most cases despite not having ALL observations in 1 column given that all 3 observation variables have different units. There are few operations that would need us to stretch out this dataframe any longer (i.e. 4 ID variables and 1 Observation variable).`

`While using many of the functions in R, which are often vector based, you usually do not want to do mathematical operations on values with different units. For example, using the purely long format, a single mean for all of the values of population, life expectancy, and GDP would not be meaningful since it would return the mean of values with 3 incompatible units. The solution is that we first manipulate the data either by grouping (see the lesson on dplyr), or we change the structure of the dataframe. Note: Some plotting functions in R actually work better in the wide format data.`

**From wide to long format with gather()**

* Data almost never is given to us like in the gapminder example

* We are going to load a wide format version of the data
   * This is an example of how data can sometimes be given to you
   * `gap_wide <- read.csv("data/gap,inder_wide.csv", stringsAsFactors = FALSE)`
   * `str(gap_wide)`
     * Need to go over the argument `stringsAsFactors`
     
![visual_example_gap_wide](http://swcarpentry.github.io/r-novice-gapminder/fig/14-tidyr-fig2.png)     


* First thing that we will do is convert this wide to a long format
   * We will use the `tidyr` function `gather()`
      * We want to 'gather' our observations variables into a single variable.
   * `gap_long <- gap_wide %>% gather(obstype_year, obs_values, starts_with('pop'), 
     starts_with('lifeExp'), starts_with('gdpPercap'))`
   * `str(gap_long)`
   
* Here we have used piping syntax which is similar to what we were doing in the previous lesson with dplyr. 
     * These are compatible and you can use a mix of tidyr and dplyr functions by piping them together

* Let's breakdown the function
  * We first name the new column for the new ID varialbe (obstype_year)
  * We also name the new amagamated observation variable (obs_value)
  * Then we provide the names of the old observation variables
     * It is possible to type out the full observation variables 
        * We can use `starts_with()` to select all varialbes that start with the desired character string
        * `gather()` also allows alternative syntax of "-" symbol to identify which variables are not to be gathered
              * `gap_long <- gap_wide %>% gather(obstype_year, obs_values, -continent, -country)`
              * `str(gap_long)`
              * this can be a huge time saver

![visual_example_gap_long](http://swcarpentry.github.io/r-novice-gapminder/fig/14-tidyr-fig3.png)

* obstype_year contains 2 pieces of information
   * the observation type (pop, lifeExp, or gdpPercap) and the year
   * We can use `separate()` to split the character strings into multiple variables
   * `gap_long <- gap_long %>% separate(obstype_year,into=c('obs_type','year'),sep="_")`
   * `gap_long$year <- as.integer(gap_long$year)`
   
**Challenge 2**
Using gap_long, calculate the mean life expectancy, population, and gdpPercap for each continent. 
*Hint: use the group_by() and summarize() functions we learned in the dplyr lesson*

`gap_long %>% group_by(continent,obs_type) %>%
   summarize(means=mean(obs_values))`
   
  * Probably will have to walk class through this example step by step (perhaps give a few minutes then work out together)

**From long to intermediate format with spread()**

* We can use the `spread()` function to "spread out" our data now back to either a wide or intermediate format
   * `gap_normal <- gap_long %>% spread(obs_type, obs_values)`
   * `dim(gap_normal)` versus `dim(gap_data)`
   * `names(gap_normal)` versus `names(gap_data)`

* So the gap_normal looks similar to the gap_data except that some variables look out of order
   * `gap_normal <- gap_normal[, names(gap_data)]`
   * `all.equal(gap_normal, gap_data)`

* Something is going on here, what could it be?
   * `head(gap_normal)` versus `head(gap_data)`
       * the original was sorted by country, continent, and then year
   * `gap_normal <- gap_normal %>% arrange(country, continent, year)`
   * `all.equal(gap_normal, gap_data)`
   
* Let's now convert our long form back to the wide form
   * We will keep country and continent as ID variables
   * We will spread the observations across 3 metrics (pop, lifeExp, gdpPercap) and time (year)
   * First need to create appropriate labels for new variables (time\*metric combos)
   * Need to unify the ID variables to simplify the process of defining gap_wide

* Let's get started
   * `gap_temp <- gap_long %>% unite(var_ID, continent, country, sep="_")`
   * `str(gap_temp)`

* Getting Closer
  * `gap_temp <- gap_long %>% unite(ID_var, continent, country, sep="_") %>% unite(var_names, obs_type, year, sep="_")`
  * `str(gap_temp)`

* Almost there
  * `gap_wide_new <- gap_long %>% unite(ID_var, continent, country, sep="_") %>% unite(var_names, obs_type, year, sep="_") %>% 
     spread(var_names, obs_values)`
  * `str(gap_wide_new)`
  
**Challenge 3**
Take this 1 step further and create a gap_ludicrously_wide format data by spreading over countries, year and the 3 metrics? 
*Hint this new dataframe should only have 5 rows.*

`gap_ludicrously_wide <- gap_long %>%
   unite(var_names,obs_type,year,country,sep="_") %>%
   spread(var_names,obs_values)`

  
* One last thing
   * `gap_wide_betterID <- separate(gap_wide_new,ID_var,c("continent","country"),sep="_")` **OR**
   * `gap_wide_betterID <- gap_long %>% unite(ID_var, continent,country,sep="_") %>% unite(var_names, obs_type,year,sep="_") %>%
    spread(var_names, obs_values) %>% separate(ID_var, c("continent","country"),sep="_")`
   * `str(gap_wide_betterID)` and `all.equal(gap.wide, gap_wide_betterID)`
   
  
   
   
    

   






