# Welcome to my blog
date: 03/06/2020 (mm/dd/yyyy)

author: Phung Thanh Sang (hwngs)

## Introduction
- This blog is going to talk about ggplot2 library in R.
- All things below here which I was taught from the visualization course of edX.
- [Here is link of the course](https://www.edx.org/course/data-science-visualization)
- Now, let's get started.

## Content
### What is the ggplot2 library?
- The ggplot2 library is one of the greatest tools in R where is going to support you in visualizing data.

### How can we use it?
#### Install
- The first thing you need to guarantee that you did install it.
- To install the ggplot2 library in your R. We just type the code in your R console:
```
install.package(ggplot2)
```
- How can we check it installed? We have 2 ways to check it:

**1. Check from installed list:**\

```
installed.packages()
```

**2. Load it (prefer):**

```
library(ggplot2)
```

- Why did I prefer the 2nd way? Because of using the 1st way, you are going to recieve the very long list like this:
#### Load
- To load the package we just type the code:
```
library(ggplot2)
```
#### How to visualize data with ggplot2
**1. We need to determine the components in your graph:**
- Data: The US murders data table is being summarized. We refer to this as the data component.
- Geometry: The type of your graph. The possible geometries are scatterplot, barplot, histogram, smooth densities, qqplot, and boxplot.
- Aesthetic mapping: This is the part in which we tell ggplot2 how to visualize our data. Such as which x-axis, y-axis is?, what is color used?,...

![Image](https://github.com/ptsang1/r_ggplot2_library.io/blob/master/image.png)

