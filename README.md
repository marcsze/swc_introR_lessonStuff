# Intro to R Lesson Plan
**Originally designed for a SWC session from August 31st - September 1st**

### Preamble

This repository contains all the material needed as well as a description and walkthrough of all the different topics that will be covered during the session.

### Obtaining Example Data Set

The inflammation data set that will be used during this lesson can be downloaded [here](http://swcarpentry.github.io/r-novice-inflammation/setup/).  After downloading the data move it onto your Desktop and unzip the file.  

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





