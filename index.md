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

![Image1](https://github.com/ptsang1/r_ggplot2_library.io/blob/master/image_installed.package.png)
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

** boxplot **
- The histogram showed us that the income distribution values show a dichotomy. However, the histogram does not show us if the two groups of countries are west versus the developing world.

- To see distributions by geographical region, we first stratify the data into regions, and then examine the distribution for each. 

- Because the number of regions is large. If we look at at histograms or smooth densities for each will not be useful. Instead, we can stack box plots next to each other. To do this, we simply write this code:

```
# add dollars per day variable
gapminder <- gapminder %>%
    mutate(dollars_per_day = gdp/population/365)
    
# number of regions
length(levels(gapminder$region))

# boxplot of GDP by region in 1970
past_year <- 1970
p <- gapminder %>%
    filter(year == past_year & !is.na(gdp)) %>%
    ggplot(aes(region, dollars_per_day))
p + geom_boxplot()
```
- When we do this, we get this plot:

![Image1](https://github.com/ptsang1/r_ggplot2_library.io/blob/master/image.png)

- You are going to see that we can't read the region names because the default ggplot
behavior is to write the labels horizontally and here we run out of room.

- We can easily fix this by rotating the labels by adding the layer like this:

```
theme(axis.text.x = element_text(angle = 90, hjust = 1))

# add data points
p + scale_y_continuous(trans = "log2") + geom_point(show.legend = FALSE)
```

```
# add dollars per day variable and define past year
gapminder <- gapminder %>%
    mutate(dollars_per_day = gdp/population/365)
past_year <- 1970

# define Western countries
west <- c("Western Europe", "Northern Europe", "Southern Europe", "Northern America", "Australia and New Zealand")

# facet by West vs devloping
gapminder %>%
    filter(year == past_year & !is.na(gdp)) %>%
    mutate(group = ifelse(region %in% west, "West", "Developing")) %>%
    ggplot(aes(dollars_per_day)) +
    geom_histogram(binwidth = 1, color = "black") +
    scale_x_continuous(trans = "log2") +
    facet_grid(. ~ group)

# facet by West/developing and year
present_year <- 2010
gapminder %>%
    filter(year %in% c(past_year, present_year) & !is.na(gdp)) %>%
    mutate(group = ifelse(region %in% west, "West", "Developing")) %>%
    ggplot(aes(dollars_per_day)) +
    geom_histogram(binwidth = 1, color = "black") +
    scale_x_continuous(trans = "log2") +
    facet_grid(year ~ group)

# define countries that have data available in both years
country_list_1 <- gapminder %>%
    filter(year == past_year & !is.na(dollars_per_day)) %>% .$country
    country_list_2 <- gapminder %>%
    filter(year == present_year & !is.na(dollars_per_day)) %>% .$country
    country_list <- intersect(country_list_1, country_list_2)

# make histogram including only countries with data available in both years
gapminder %>%
    filter(year %in% c(past_year, present_year) & country %in% country_list) %>%    # keep only selected countries
    mutate(group = ifelse(region %in% west, "West", "Developing")) %>%
    ggplot(aes(dollars_per_day)) +
    geom_histogram(binwidth = 1, color = "black") +
    scale_x_continuous(trans = "log2") +
    facet_grid(year ~ group)
    
p <- gapminder %>%
    filter(year %in% c(past_year, present_year) & country %in% country_list) %>%
    mutate(region = reorder(region, dollars_per_day, FUN = median)) %>%
    ggplot() +
    theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
    xlab("") + scale_y_continuous(trans = "log2")
    
 p + geom_boxplot(aes(region, dollars_per_day, fill = continent)) +
     facet_grid(year ~ .)
 
 # arrange matching boxplots next to each other, colored by year
 p + geom_boxplot(aes(region, dollars_per_day, fill = factor(year)))
```

```
# see the code below the previous video for variable definitions

# smooth density plots - area under each curve adds to 1
gapminder %>%
    filter(year == past_year & country %in% country_list) %>%
    mutate(group = ifelse(region %in% west, "West", "Developing")) %>% group_by(group) %>%
    summarize(n = n()) %>% knitr::kable()

# smooth density plots - variable counts on y-axis
p <- gapminder %>%
    filter(year == past_year & country %in% country_list) %>%
    mutate(group = ifelse(region %in% west, "West", "Developing")) %>%
    ggplot(aes(dollars_per_day, y = ..count.., fill = group)) +
    scale_x_continuous(trans = "log2")
p + geom_density(alpha = 0.2, bw = 0.75) + facet_grid(year ~ .)

# add group as a factor, grouping regions
gapminder <- gapminder %>%
    mutate(group = case_when(
            .$region %in% west ~ "West",
            .$region %in% c("Eastern Asia", "South-Eastern Asia") ~ "East Asia",
            .$region %in% c("Caribbean", "Central America", "South America") ~ "Latin America",
            .$continent == "Africa" & .$region != "Northern Africa" ~ "Sub-Saharan Africa",
            TRUE ~ "Others"))

# reorder factor levels
gapminder <- gapminder %>%
    mutate(group = factor(group, levels = c("Others", "Latin America", "East Asia", "Sub-Saharan Africa", "West")))
    
# note you must redefine p with the new gapminder object first
p <- gapminder %>%
  filter(year %in% c(past_year, present_year) & country %in% country_list) %>%
    ggplot(aes(dollars_per_day, fill = group)) +
    scale_x_continuous(trans = "log2")

# stacked density plot
p + geom_density(alpha = 0.2, bw = 0.75, position = "stack") +
    facet_grid(year ~ .)
    
# weighted stacked density plot
gapminder %>%
    filter(year %in% c(past_year, present_year) & country %in% country_list) %>%
    group_by(year) %>%
    mutate(weight = population/sum(population*2)) %>%
    ungroup() %>%
    ggplot(aes(dollars_per_day, fill = group, weight = weight)) +
    scale_x_continuous(trans = "log2") +
    geom_density(alpha = 0.2, bw = 0.75, position = "stack") + facet_grid(year ~ .)
```

```
# define gapminder
library(tidyverse)
library(dslabs)
data(gapminder)

# add additional cases
gapminder <- gapminder %>%
    mutate(group = case_when(
        .$region %in% west ~ "The West",
        .$region %in% "Northern Africa" ~ "Northern Africa",
        .$region %in% c("Eastern Asia", "South-Eastern Asia") ~ "East Asia",
        .$region == "Southern Asia" ~ "Southern Asia",
        .$region %in% c("Central America", "South America", "Caribbean") ~ "Latin America",
        .$continent == "Africa" & .$region != "Northern Africa" ~ "Sub-Saharan Africa",
        .$region %in% c("Melanesia", "Micronesia", "Polynesia") ~ "Pacific Islands"))

# define a data frame with group average income and average infant survival rate
surv_income <- gapminder %>%
    filter(year %in% present_year & !is.na(gdp) & !is.na(infant_mortality) & !is.na(group)) %>%
    group_by(group) %>%
    summarize(income = sum(gdp)/sum(population)/365,
                        infant_survival_rate = 1 - sum(infant_mortality/1000*population)/sum(population))
surv_income %>% arrange(income)

# plot infant survival versus income, with transformed axes
surv_income %>% ggplot(aes(income, infant_survival_rate, label = group, color = group)) +
    scale_x_continuous(trans = "log2", limit = c(0.25, 150)) +
    scale_y_continuous(trans = "logit", limit = c(0.875, .9981),
                                       breaks = c(.85, .90, .95, .99, .995, .998)) +
    geom_label(size = 3, show.legend = FALSE) 
```




