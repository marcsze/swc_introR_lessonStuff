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

 






