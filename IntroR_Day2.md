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
  
* Challenge 1

Modify the previous example that we used so that the new plot shows how life expectancy has changed over time.
 * modify `ggplot(data = gap_data, aes(x = gdpPercap, y = lifeExp)) + geom_point()`
 * to `ggplot(data = gapminder, aes(x = year, y = lifeExp)) + geom_point()`
 
* Challenge 2

In the previous examples and challenge we’ve used the aes function to tell the scatterplot geom about the x and y locations of each point. Another aesthetic property we can modify is the point color. Modify the code from the previous challenge to color the points by the “continent” column. 

 * First, see if the learners can attempt on their own
 * Second, if they have troubles provide hint
    * `ggplot(data = gapminder, aes(x = year, y = lifeExp, color=_________)) + geom_point()`
 
    
What trends do you see in the data? Are they what you expected?




      
 
   




### Split-Apply-Combine




### Dataframe Manipulation with dplyr



### Dataframe Manipulation with tidyr
