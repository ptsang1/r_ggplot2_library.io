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
#### 1. Install
- The first thing you need to guarantee that you did install it.
- To install the ggplot2 library in your R. We just type the code in your R console:
```
install.package(ggplot2)
```
- How can we check it installed? We have 2 ways to check it:

**a. Check from installed list:**

```
installed.packages()
```

**b. Try loading it (prefer):**

```
library(ggplot2)
```

- Why did I prefer the 2nd way? Because of using the 1st way, you are going to recieve the very long list like this:

![Image1](https://github.com/ptsang1/r_ggplot2_library.io/blob/master/image.png)
It's really hard to check.

#### 2. Load
- To load the package we just type the code:
```
library(ggplot2)
```

#### 3. How to visualize data with ggplot2
**a. We need to determine the components in your graph:**
- Data: The US murders data table is being summarized. We refer to this as the data component.
- Geometry: The type of your graph. The possible geometries are scatterplot, barplot, histogram, smooth densities, qqplot, and boxplot.
- Aesthetic mapping: This is the part in which we tell ggplot2 how to visualize our data. Such as what are x-axis and y-axis?, what is color used?,...

**b. Define the ggplot object:**
```
# load data
library(dslabs)
data(murders)

# load ggplot library
library(ggplot2)

# define ggplot2 object
graph = murders %>% ggplot(aes(population/10^6, total))
# or
graph = ggplot(murders, aes(population/10^6, total))
```
- We have just defined the _graph_ ggplot object. But _graph_ will not show anything if we print it. Because we haven't determined a geometry component yet.

**c. Visualization:**

- In ggplot2 we create graphs by adding layers. Layers can define geometries, compute summary statistics, define what scales to use, or even change styles. To add layers, we use the symbol +. In general, a line of code will look like this:

```
_ggplot_object_ + LAYER1 + LAYER2 + ...
```

Usually, the first added layer defines the geometry. We want to make a scatterplot. What geometry do we use?

Taking a quick look at the cheat sheet. We are going to use the functions whose name starts with geom_ to visualize data.

![Image2](https://rafalab.github.io/dsbook/R/img/ggplot2-cheatsheeta.png)

- Now, let's get started with each functions.

**- geom_point and geom_text**
```
# facet by continent and year
filter(gapminder, year %in% c(1962, 2012)) %>%
    ggplot(aes(fertility, life_expectancy, col = continent)) +
    geom_point() +
    facet_grid(continent ~ year)

# facet by year only
filter(gapminder, year %in% c(1962, 2012)) %>%
    ggplot(aes(fertility, life_expectancy, col = continent)) +
    geom_point() +
    facet_grid(. ~ year)

# facet by year, plots wrapped onto multiple rows
years <- c(1962, 1980, 1990, 2000, 2012)
continents <- c("Europe", "Asia")
gapminder %>%
    filter(year %in% years & continent %in% continents) %>%
    ggplot(aes(fertility, life_expectancy, col = continent)) +
    geom_point() +
    facet_wrap(~year)
```
