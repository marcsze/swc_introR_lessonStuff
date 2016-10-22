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
   
   
   
   
We’ve passed in two arguments to ggplot. First, we tell ggplot what data we want to show on our figure, in this example the gapminder data we read in earlier. For the second argument we passed in the aes function, which tells ggplot how variables in the data map to aesthetic properties of the figure, in this case the x and y locations. Here we told ggplot we want to plot the “gdpPercap” column of the gapminder data frame on the x-axis, and the “lifeExp” column on the y-axis. Notice that we didn’t need to explicitly pass aes these columns (e.g. x = gapminder[, "gdpPercap"]), this is because ggplot is smart enough to know to look in the data for that column!
 



### Split-Apply-Combine




### Dataframe Manipulation with dplyr



### Dataframe Manipulation with tidyr
