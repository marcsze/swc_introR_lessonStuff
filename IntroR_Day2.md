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

   * `ggplot(data = gapminder, aes(x = gdpPercap, fill=continent)) + geom_density(alpha=0.6) + facet_wrap( ~ year) + scale_x_log10()`




### Split-Apply-Combine




### Dataframe Manipulation with dplyr



### Dataframe Manipulation with tidyr
