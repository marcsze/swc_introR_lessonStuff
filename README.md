# Intro to R Lesson Plan
**Originally designed for a SWC session from August 31st - September 1st**

### Preamble

This repository contains all the material needed as well as a description and walkthrough of all the different topics that will be covered during the session.

### Obtaining Example Data Set

The inflammation data set that will be used during this lesson can be downloaded [here](http://swcarpentry.github.io/r-novice-inflammation/setup/).  After downloading the data create a new folder called `r-novice-inflammation`, unzip the file and move it into this newly created folder.  

*Alternatively, if you have `git` already installed and have a user profile you can also clone this repo and get the data set and this walkthrough with the following commands on the command line.*

`cd ~/Desktop`
<br>
`git clone https://github.com/marcsze/swc_introR_lessonStuff.git`

### Intro to RStudio

When we open up RStudio there are three panels the immediately appear
* The Console
* Environment/History (where you can see the data you load)
* Help/Directory/Vis

You can run commands directly on the console like you would in the `bash` command line.

```R
print("Hello World")
```
Alternatively, we can also run commands using a script.  To open a new script file in RStudio select the `File` tab and then hover over the `New File` and then select `R Script`.  This will open a fourth panel.  We can then write our commands in the script file and then have it run via the console.  There are a few different ways in which we can run commands from the script file.

1. ![Using the run button to execute a command in the script file](RRunPic2.png) Use the run button to execute command on the same line as the cursor

2. ![Using the run button to execute a command in the script file](RRunPic3.png) Use the source button to run the command (this will run all commands in your script)
  2. Add extra lines of code to show the difference between source and run  

3. You can use `ctrl` + `enter` to run the command on the line of the cursor as well.

4. You can also quickly source all of your code with `ctrl` + `shift` + `s` 


A few extra short cuts that could be helpful for switching between the console and script windows
* `ctrl` + `1` will move you from the console to the script
* `ctrl` + `2` will move you from the script to the console

Another important thing that we can do in the script file that is not easy to do in the console is to comment our code using the `#` button.  This tells the computer to ignore everything on this line.

What happens when you send incomplete code to the console to run?
```R
print("Hello World"
```
Try it out.  You should see a `+` sign appear.  R is smart enough to know that you are missing something in your code and is waiting for you to add it.  Cool right?  You can escape the `+` sign by pressing `Esc`.  Now that we've gotten a good feel for the RStudio program lets get started with some commands now...


### Analyzing Patient Data

I'm going to use the script panel for most of the rest of the lesson (need to make sure to enlarge the view with `ctrl` + `shift` + `+`).

Main parts covered during this component include:
* Loading Data
* Variable assignment
    * can use `Alt` + `-` to quickly bring up arrow used for assignment
*  Commenting examples
*  Basic data manipulation
*  How to bring up help for each function
*  Basic apply usage
    * alternative include `rowMeans` and `colMeans`
* Basic Plotting

#### Loading Data

We can get an idea of our current working directory by typing in 

```R
getwd()
```
Since we downloaded and unzipped our file into a folder on the Desktop to make things easier on us this morning we will set the directory to which RStudio will initially be in with:

```R
setwd("~/Desktop/r-novice-inflammation/")
```
We can then read a csv file using the following command:

```R
read.csv(file = "data/inflammation-01.csv", header = FALSE)
```

Segway class into two observations that happened from running this command.
  * First bring up the Need to go over the options for `read.csv` (header and other types of decimal formats)
      * initial intro to the the help functionality of R
  * Second bring up that everything was read to the console (not very useful)

  
#### Variable assignment

Variable can be thought of as name for a value.  The neat thing is that it can be stored for future use (assign a variable and then show that it will appear in the Environment pane).

* Cover assignment (kilogram weight)
* Cover arithmetic (kg to pound with 2.2 multiplication)
* If not already mentioned highlight utility of the `up` arrow

#### Commenting examples

* Go over how to use comments to make code easier to understand
    * Change the kg weight variable to new number
    * Add a new variable for weight in pounds
    * Change the kg weight to highlight how variables store the info
    * highlight use of parenthesis
    * Use the `read.csv` function along with variable assignment to store data as `dat`
        * can use the command `head` to view data in console (ask how you would bring up the data then bring up `head`)

Create a `mass` and `age` variable and get class to explain what happens when the arguments
* `mass <- mass * 2.0` 
* `age <- age - 20`


#### Manipulating Data

Basic data table parsing
* Go over the `class` function (type of data table being used)
* `dim` to get total rows and columns
* how to get a specific value in a data table with row and column number
    * Try a view differnt examples
* How to bring up a specific range of rows and columns
    * have them try to get a different range (e.g. rows 5-10 and columns 4-7)
* show the use of `c()` to get non-continuous rows and columns from data table
    * Get them to try commands without filling in the row or column e.g. `data[, 5]` 
      * Ask them what they think will happen

Doing some calculations
* assign variable to a whole row
* use the `max()` function to get the highest value
    * Important to note that can use `as.numeric` and `class` to view how data is stored
* show the same thing but without variable storage
* use `min()`, `mean()`, `median()`, `sd()`
  * mention that this is good for one at a time but really want to apply to all the data table
* Re-Introduce the help function by using it on `apply()`
* Use `apply()` to get values by row (1) or column(2)

```R
# get average by row
avg_patient_inflammation <- apply(dat, 1, mean)
avg_day_inflammation <- apply(dat, 2, mean)
```

Finally, explain how R is filled with functions that have more efficient alternatives e.g. `colMeans` and `rowMeans`.

Questions
* Get class to create a variable called animal (go over how RStudio automatically completes quotes)
```R
animal <- c("m", "o", "n", "k", "e", "y")
```
    * remind about selecting specific parts of the vector 
    * how to get reverse order (`animal[4:1]`)
    * what does `animal[-1]` or `animal[-4]` do?
    * spell new word "eon" (`animal[c(5,2,3)]`)

* How would you get the max inflammation from patient 5 for days 3-7 (`max(dat[5, 3:7])`)

* Using the inflammation data frame dat from above: Let’s pretend there was something wrong with the instrument on the first five days for every second patient (#2, 4, 6, etc.), which resulted in the measurements being twice as large as they should be
  * Write a vector with each affected patient (hint use `seq(from, to, by)`)
  * Create new data from in which you halve the 1st five values in only those patients
  * print new data table to check
  * Mention in solution how R is vectorized so it will do the division for all values

*One possible solution*
```R
whichPatients <- seq(2, 40, 2)
whichDays <- c(1:5)
dat2 <- dat
dat2[whichPatients, whichDays] <- dat2[whichPatients, whichDays]/2
(dat2)
```

* Use the apply function to 
  * get mean over 40 days for patient 1-5 (`apply(dat[1:5, ], 1, mean)`)
  * mean for days 1 - 10 for all patients (`apply(dat[, 1:10], 2, mean)`)
  * mean for every second day for all patients (`apply(dat[, seq(1,40,2)], 2, mean)`)
  * Are there any alternative methods the class can think of?


#### Plotting

* Go over base `plot()` function with `avg_day_inflammation` 
* Go over plotting only the `max()` day inflammation 
* Go over plotting only the `min()` day inflammation (ask class how they would do this)

Questions
* How would you plot the SD for the inflammation data for each day across all patients (`plot(apply(dat, 2, sd))`)


### Creating Functions

#### Defining a Function

Now we are going to learn how to create a function.  Some functions that we have already use include `apply`, `sd`, `mean`, etc.   We are going to create fahrenheit to kelvin conversion function.

```R
fahr_to_kelvin <- function(temp){
  kelvin <- ((temp - 32) * (5 / 9)) + 273.15
  return(kelvin)
}
```
* Demonstrate a few examples
* Mention that R has an automatic reutrn but we will specify the return for our lesson for clarity
* Try examples without the return

#### Composing Functions

* Get class to create a kelvin to celsius function

*Solution*
```R
kelvin_to_celsius <- function(temp){
  celsius <- temp - 273.15
  return(celsius)
}
```
* Try this new function on 0 should return -273.15 if it works properly.
* Show how to create fahrenheit to celsius conversion using the two functions already made

```R
fahr_to_celsius <- function(temp){
  temp_k <- fahr_to_kelvin(temp)
  result <- kelvin_to_celsius(temp_k)
  return(result)
}
```
* Try this on a fahrenheit reading of 32 the result should return 0 if working properly.
* Can also achieve this result without creating a new function by nesting/chaining functions
  * `kelvin_to_celsius(fahr_to_kelvin(32.0))`

Question
* Create a wrapper function that puts a variable at the beginning and end of vector
    * we want a vector with `*` at the beginning and the end of an inputed vector
    * `vec1 <- c("write", "programs", "for", "people", "not", "computers")`
    * `wrapperVar <- "*"`

*Solution*
```R
fence <- function(vec1, wrapperVar){
  newVector <- c(wrapperVar, vec1, wrapperVar)
  return(newVector)
}
```
* Create a fucntion that obtains the first and last component in a vector (hint `length()`)

*Solution*
```R
outside <- function(data){
  newData <- c(data[1], data[length(data)])
  return(newData)
}
```

#### Testing and Documenting

First create a function called center (get class to explain what the function is doing)

```R
center <- function(data, desired){
  new_data <- (data - mean(data)) + desired
  return(new_data)
}
```

* First try this on a vector of all zeros `z <- c(0,0,0,0)` and move everything to 3
* Try this on the inflammation data set for day 4
* compare `min`, `mean`, `max` between original and new centered data
* Can compare `sd` of both to make sure the data structure didn't change
* can check for difference by subtraction (`sd(dat[,4]) - sd(centered)`)
* can also use `all.equal` on the standard deviations to check similarity of the two data sets
* add documentation to function (get class to think about what they would put in the documentation)

Questions
* create a function that uses the inflammation data and plots the mean, max, and min for each day

*Solution*
```R
analyze <- function(filename) {
  # Plots the average, min, and max inflammation over time.
  # Input is character string of a csv file.
  dat <- read.csv(file = filename, header = FALSE)
  avg_day_inflammation <- apply(dat, 2, mean)
  plot(avg_day_inflammation)
  max_day_inflammation <- apply(dat, 2, max)
  plot(max_day_inflammation)
  min_day_inflammation <- apply(dat, 2, min)
  plot(min_day_inflammation)
}
```
* Create a function that rescales your data to be between 0 and 1 (Use comments to document where you are)

*Solution*
```R
rescale_0_to_1 <- function(v) {
  # Rescales a vector, v, to lie in the range 0 to 1.
  L <- min(v)
  H <- max(v)
  result <- (v - L) / (H - L)
  return(result)
}
```

#### Defining Defaults

* Demonstrate how we can pass arguments without specifying name (e.g. "file =" for `red.csv`)
  * Stipulate that position matters if they are not named (use a demonstration)
  * change the previous center function to include "desired = 0"
  * create a new function to highlight how R does this in more detail
    * use (), (55), (55,66), (55,66,77), (c=77)

```R
display <- function(a = 1, b = 2, c = 3) {
  result <- c(a, b, c)
  names(result) <- c("a", "b", "c")  # This names each element of the vector
  return(result)
}
```

* Go over the `read.csv` command using the `?read.csv` to see what is going on with defaults

Question
* Rewrite the `rescale_0_to_1` function so it scales between 0 -1 by default but will allow for the user to change these bounds.

*One possible Solution*
```R
rescale_0_to_1 <- function(v, lower = 0, upper = 1) {
  # Rescales a vector, v, to lie in the range lower to upper.
  L <- min(v)
  H <- max(v)
  result <- (v - L) / (H - L) * (upper - lower) + lower
  return(result)
}
```

### Analyzing Multiple Data Sets

#### Preamble

The main objective of this lesson will be the introduction of for loops in R.

Remind the class of the previous funtion called `analyze()` that creates a graph for the minimum, average, and maximum inflammation by day.

```R
analyze <- function(filename) {
  # Plots the average, min, and max inflammation over time.
  # Input is character string of a csv file.
  dat <- read.csv(file = filename, header = FALSE)
  avg_day_inflammation <- apply(dat, 2, mean)
  plot(avg_day_inflammation)
  max_day_inflammation <- apply(dat, 2, max)
  plot(max_day_inflammation)
  min_day_inflammation <- apply(dat, 2, min)
  plot(min_day_inflammation)
}

analyze("data/inflammation-01.csv")
```

Can use this on other similarily formatted data sets (e.g. "inflammation-02.csv").  So this is okay with a couple but can get really burdensome if you have many similar data sets in which you want to analyze.

#### For Loops

* Let's take the given function:

```R
best_practice <- c("Let", "the", "computer", "do", "the", "work")
print_words <- function(sentence){
	print(sentence[1])
	print(sentence[2])
	print(sentence[3])
	print(sentence[4])
	print(sentence[5])
	print(sentence[6])
}
```

* Ask the class what might be some of the limitations of this sort of approach?
	* Doesn't scale well
	* It will only print the first 6 even of longer vectors

* Good alternative is to use a for loop

```R
print_words <- function(sentence){
	for(word in sentence){
		print(word)
	}
}
```
* Talk about the general format of the for loop in R

* Look through the next example and explain what R is doing
```R
len <- 0
vowels <- c("a", "e", "i", "o", "u")
for(v in vowels){
	len <- len + 1
}
len
```
* Ask the class if they can remember an alternative way of doing this (use `length`)
* Change `len <- len + 1` to `print(v)` to highlight how the loop ends up on the last character

Questions

* Using `seq()`, write a function that prints the first N natural numbers, one per line

*Solution*
```R
print_N <- function(N) {
  nseq <- seq(N)
  for (num in nseq) {
    print(num)
  }
}
```

* Create a function that adds a each number in a vector together (do not use `sum()`)

* One Possible Solution*
```R
total <- function(vec) {
  #calculates the sum of the values in a vector
  vec_sum <- 0
  for (num in vec) {
    vec_sum <- vec_sum + num
  }
  return(vec_sum)
}
```

* There is a built in exponent function in R (`2^4`).  Create one yourself.

*One Possible Solution*
```R
expo <- function(base, power) {
  result <- 1
  for (i in seq(power)) {
    result <- result * base
  }
  return(result)
}
```

#### Processing Multiple Files

Now we almost have everything we need to complete our original task of graphing a large number of similar data tables.  The last thing we need is to take advantage of a built in R function called `list.files()`.

* Show what `list.files()` does with `?list.files()`
* Show two ways of running the search with either "csv" or "inflammation" as the pattern
	* Quick plug on puttind different parts of the analysis into different directories
*  Show what happens with the "full.names = TRUE" option in `list.files()`
* Create a workflow that utilizes this and a for loop

```R
filenames <- list.files(path = "data", pattern = "inflammation.*csv", full.names = TRUE)
filenames <- filenames[1:3]
for (f in filenames) {
  print(f)
  analyze(f)
}
```

Questions

* Write a fucntion called "analyze_all" that takes a filename pattern as its sole argument and runs analyze for each file whose name matches the pattern.

*Solution*
```R
analyze_all <- function(pattern) {
  # Runs the function analyze for each file in the current working directory
  # that contains the given pattern.
  filenames <- list.files(path = "data", pattern = pattern, full.names = TRUE)
  for (f in filenames) {
    analyze(f)
  }
}
```

### Making Choices

#### Saving Plots to a File

We have now built two important functions "analyze" and "analyze_all".  Now how do we take this and save as a `.pdf`? So that we can share our findings with collaborators, PIs.  A simple way of doing this would be to use

```R
pdf("inflammation-01.pdf")
analyze("data/inflammation-01.csv")
dev.off()
```

* Need to have `dev.off()` to close connections and avoid overwriting files 

However, we may be able to make the analyze function have the option to save as a pdf based on a user specified input by using conditionals (if/else statements).

#### Conditionals

R uses conditional statements to allow for it to decide between different options.  How does this look?

```R
num <- 37
if (num > 100) {
  print("greater")
} else {
  print("not greater")
}
print("done")
```

* See if the class can figure out what is going on with the statement.
* Cover how conditional statements don't need an `else{}` statement.

```R
num <- 53
if (num > 100) {
  print("num is greater than 100")
}
```

* Mention how in this case R will simply do nothing if the condition is not met.
* Show how we can use multiple statements together

```R
sign <- function(num) {
  if (num > 0) {
    return(1)
  } else if (num == 0) {
    return(0)
  } else {
    return(-1)
  }
}
```

* Try various different numbers to highlight the functions functionality
* Go over other statements `>=`, `<=`, and `!=`
* Go over `&`

```R
if (1 > 0 & -1 > 0) {
    print("both parts are true")
} else {
  print("at least one part is not true")
}
```

* Modify the above code
	* Replace `&` with `|`
	* Replace "both parts are true" with "at least one part is true"
	* Replace "at least one part is true" with "neither part is true"


Questions

* Write a function plot_dist that plots a boxplot if the length of the vector is greater than a specified threshold and a stripchart otherwise. To do this you’ll use the R functions `boxplot()` and `stripchart()`.
	* Can use `dat[, 10]` and `dat[1:5, 10]` to help test the function out.
	* Need to use a threshold value of 10

*Solution*
```R
plot_dist <- function(x, threshold) {
  if (length(x) > threshold) {
    boxplot(x)
  } else {
    stripchart(x)
  }
}
```

* One of your collaborators prefers to see the distributions of the larger vectors as a histogram instead of as a boxplot. In order to choose between a histogram and a boxplot we will edit the function plot_dist and add an additional argument use_boxplot. By default we will set use_boxplot to TRUE which will create a boxplot when the vector is longer than threshold. When use_boxplot is set to FALSE, plot_dist will instead plot a histogram for the larger vectors. As before, if the length of the vector is shorter than threshold, plot_dist will create a stripchart. A histogram is made with the `hist()` command in R
	* Need to have an option for boxplot to be true or false if not below a threshold
	* Can use the same vectors as the previous question to test your function

*Solution*
```R
plot_dist <- function(x, threshold, use_boxplot = TRUE) {
   if (length(x) > threshold & use_boxplot) {
       boxplot(x)
   } else if (length(x) > threshold & !use_boxplot) {
       hist(x)
   } else {
       stripchart(x)
   }
}
```

* Find the file containing the patient with the highest average inflammation score. Print the file name, the patient number (row number) and the value of the maximum average inflammation score.
	* Use variables to store the maximum average and update it as you go through files and patients.
	* Use variables to store the maximum average and update it as you go through files and patients.

Add your code:
```R
filenames <- list.files(path = "data", pattern = "inflammation.*csv", full.names = TRUE)
filename_max <- "" # filename where the maximum average inflammation patient is found
patient_max <- 0 # index (row number) for this patient in this file
average_inf_max <- 0 # value of the average inflammation score for this patient
for (f in filenames) {
  dat <- read.csv(file = f, header = FALSE)
  dat.means = apply(dat, 1, mean)
  for (patient_index in length(dat.means)){
    patient_average_inf = dat.means[patient_index]
    # Add your code here ...
  }
}
print(filename_max)
print(patient_max)
print(average_inf_max)
```

*Solution*
```R
# Add your code here ...
if (patient_average_inf > average_inf_max) {
  average_inf_max = patient_average_inf
  filename_max <- f
  patient_max <- patient_index
}
```

#### Saving Automatically Generated Figures

Now we can update the analyze function so we can control which figures are made into a `.pdf`.

```R
analyze <- function(filename, output = NULL) {
  # Plots the average, min, and max inflammation over time.
  # Input:
  #    filename: character string of a csv file
  #    output: character string of pdf file for saving
  if (!is.null(output)) {
    pdf(output)
  }
  dat <- read.csv(file = filename, header = FALSE)
  avg_day_inflammation <- apply(dat, 2, mean)
  plot(avg_day_inflammation)
  max_day_inflammation <- apply(dat, 2, max)
  plot(max_day_inflammation)
  min_day_inflammation <- apply(dat, 2, min)
  plot(min_day_inflammation)
  if (!is.null(output)) {
    dev.off()
  }
}
```

* Demonstrate how the `is.null()` function works by looking at `output <- NULL` with it
* Demonstrate the different options with output
	* Null
	* "inflammation-01.pdf"
* Remind class of best practices of saving results in a different directory
	* use `dir.create("results")` to make a results directory
* Now re-run `analyze()` but route output to the results directory
* Use the `sub()` and 'file.path()` command and a modification of the `analyze_all()` function to create pdfs of all data

```R
f <- "inflammation-01.csv"
sub("csv", "pdf", f)
file.path("results", sub("csv", "pdf", f))

analyze_all <- function(pattern) {
  # Directory name containing the data
  data_dir <- "data"
  # Directory name for results
  results_dir <- "results"
  # Runs the function analyze for each file in the current working directory
  # that contains the given pattern.
  filenames <- list.files(path = data_dir, pattern = pattern)
  for (f in filenames) {
    pdf_name <- file.path(results_dir, sub("csv", "pdf", f))
    analyze(file.path(data_dir, f), output = pdf_name)
  }
}

analyze_all("inflammation*.csv")
```

Questions

* One of your collaborators asks if you can recreate the figures with lines instead of points. Find the relevant argument to plot by reading the documentation (?plot), update analyze, and then recreate all the figures with analyze_all.


*Solution*
```R
analyze <- function(filename, output = NULL) {
  # Plots the average, min, and max inflammation over time.
  # Input:
  #    filename: character string of a csv file
  #    output: character string of pdf file for saving
  if (!is.null(output)) {
    pdf(output)
  }
  dat <- read.csv(file = filename, header = FALSE)
  avg_day_inflammation <- apply(dat, 2, mean)
  plot(avg_day_inflammation, type = "l")
  max_day_inflammation <- apply(dat, 2, max)
  plot(max_day_inflammation, type = "l")
  min_day_inflammation <- apply(dat, 2, min)
  plot(min_day_inflammation, type = "l")
  if (!is.null(output)) {
    dev.off()
  }
}
```




