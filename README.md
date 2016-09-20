# SearchTree
## Reactive shiny filters through collapsible d3js trees
### Overview
D3js is a great tool to visualize complex data in a dynamic way. But how can the visualization be part of the natural workflow? 

Through Shiny we let the server observe the d3 [collapsible tree library](https://bl.ocks.org/mbostock/4339083) and its real-time layout. The data transferred  back to Shiny can be mapped to a series of logial expressions to create reactive filters. This allows for complex data structures, such as heirarchal simulations, complex design of clinical trials and results from ploycompartmental structural models to be visually represented and *filtered in a reactive manner* through an intuitive and simple tool.

### Examples

####Running the App through Github

```r
#check to see if libraries need to be installed
libs=c("rstan","shiny","shinyAce","reshape2","stringr","DT","plyr","dplyr")
x=sapply(libs,function(x)if(!require(x,character.only = T)) install.packages(x));rm(x,libs)

#run App
shiny::runGitHub("yonicd/SearchTree")
```

#### Titanic
Let's start with a popular data.frame, the Titanic data

#### STAN
After getting the hang of how the tool works lets test it out on a real problem. For those of us who are familiar with MCMC simulators (such as BUGS, WinBUGS, JAGS and STAN) we know that simulation results can scale up in a hurry. For each simulation there are chains, burn ins, priors, posteriors etc. Comparing between different simulations is a task that becomes a labor intensive excercise. A great example of an online source for different model examples is the [STAN github example repository](https://github.com/stan-dev/example-models), in it there are full examples coded with all the data files needed to run it locally on your own station all you need to do is fork it and go at it. We will focus on the book by Gelman and Hill [Data Analysis Using Regression Analysis and Multilevel/Hierarchical Models](http://www.stat.columbia.edu/~gelman/arm/) which has a vast amount of ARM models coded in STAN and R. 

Well a few things a new users to the site ask themselves is

  - How is it organized? 
  - What examples are in this book?
  - How do I get to certain models across chapters?
  - Do I need to fork the whole repo to run a few models instead of copy/paste?
  
For the first three questions there is a great readme file for the repo that you can click through but that gets confusing after 5 or 6 clicks (for me). How about if we leverage the information in all the readme files for each chapter and create a tree structure. Change the hierarchy order as we want to answer our specific searches in real time and let the tree filter out the chosen examples for us. This can let us grow branches in different chapters by model type and combine simulations that fit our needs. 

So we have a tool that can filter for us...ok...now what? We still need to run the code. Do we need to fork the whole repo to combine it to the tree? No!

##### setwd() for github url paths
So as we all know ussually the code in your repo is built to be reproducible so you have in it the r files, data files (csv,xl,tab,sas, etc) and in our case the stan files. What if you could just read the lines of code from the internet and set the working directory to the repo http path. This is what [RunStanGit.r](https://github.com/yonicd/SearchTree/blob/master/RunStanGit.r) does. It downloads the lines of code adds prefixes to the relevant read commands, comments out any plots and console print outs, and returns the output objects from the simulations. It is built to run nested calls that arise from source commands and fixes partial file paths to full url addresses. So given **properly coded** files in the repo you can run script without forking it. 

##### Shiny implementation
We used this function to create the shiny app that holds no actual data in it but can simulate any example in the STAN ARM repository.

Once the user chooses the simulations they want to run on the tree the simulate button is pressed and after all simulations are run the outputs are placed in a list object to continue anaylsis. This can through [ShinyStan](http://mc-stan.org/interfaces/shinystan) or any personal code you have written yourself.


