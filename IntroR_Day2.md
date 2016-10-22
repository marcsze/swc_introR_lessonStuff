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




### Split-Apply-Combine




### Dataframe Manipulation with dplyr



### Dataframe Manipulation with tidyr
