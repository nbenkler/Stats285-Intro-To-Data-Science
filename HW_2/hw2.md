Homework 2
================
N. Benkler
Due by 1:50 pm, Wed. 1/23

### To Do: Complete before Wednesday's class (1/23)

Use complete sentences to answer the questions below if you are asked for an *answer* or *explanation*.

Three data sets have been added to the data folder in this repository. To load one of these files, please use **relative file paths**. For example, to load the `fisheries.csv` data set, use the following command:

``` r
fisheries <- read_csv("data/fisheries.csv")
```

Push your knitted homework assignment (.Rmd and .md files) to GitHub by the given deadline.

Also let me know:

**Who you worked with:** I worked by myself.

### Problem 1

Explain why the following command does not color the data points blue, then write down the command that will turn the points blue.

The folowing command does not color the data points blue because the color command is in the aesthetics section of your command, meaning you are telling r to diffirentiate which of the data points by their different values for variable "blue", rather than to color the data points blue. Therefore as all the data points have the same value, they are all colored the same color (which happens to be red).

``` r
library(ggplot2)
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy, color = "blue"))
```

![](hw2_files/figure-markdown_github/unnamed-chunk-2-1.png)

``` r
#to color the points blue color must be outside of aes()
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy),color = "blue")
```

![](hw2_files/figure-markdown_github/unnamed-chunk-2-2.png)

### Problem 2

Revisit the `storms` data used in homework 1.

1.  Create a stacked bar chart that shows the proportion of storm `type`s that occur each `year`. Use `scale_fill_brewer` or `scale_fill_manual` to change the default coloring of the bars.

``` r
library(nasaweather)
```

    ## Warning: package 'nasaweather' was built under R version 3.5.2

    ## 
    ## Attaching package: 'nasaweather'

    ## The following object is masked from 'package:dplyr':
    ## 
    ##     storms

``` r
stormdata <- nasaweather::storms
  
ggplot(data = stormdata,
       aes(x= year, fill = type)) + 
  geom_bar(position = "fill", aes(y = ..prop..)) +
  scale_fill_brewer(palette="Dark2") +
  theme(panel.background = element_rect(fill = "gray70"), 
        legend.background = element_rect(fill = "gray80"),
        axis.title = element_blank()) +
  labs(title = "Proportion of Storm Types Occuring Each Year (1995-2000)")
```

![](hw2_files/figure-markdown_github/unnamed-chunk-3-1.png)

1.  Explain the perceptual difficulty with stacked bar charts. (Hint: think about the categories in the interior of the chart.) The perceptual difficulty with stacked bar charts is it is difficult to discern relative quantities and proportins of the internal quantities from year to year (or whatever your independant variable happens to be). Stacked bar plots require a signficant amout of subtraction and tracking where one variable catagory ends and the other begins in order to identify actual change in variable quantities/ratios.

2.  Create another graphic that you feel better communicates the story the above stacked bar chart is attempting to tell. Either scattering the columns or facet wrapping by type would be better ways to communicate this information

``` r
ggplot(data = stormdata,
       aes(x= year, 
           fill = type)) + 
  geom_bar(position = "dodge", aes(y = ..prop..)) +
  scale_fill_brewer(palette="Dark2") + 
  labs(title = "Proportion of Storm Types Occuring Each Year (1995-2000)") +
  theme(panel.background = element_rect(fill = "gray70"), 
        legend.background = element_rect(fill = "gray80"),
        axis.title = element_blank()) +
  scale_x_continuous("year")
```

![](hw2_files/figure-markdown_github/unnamed-chunk-4-1.png)

``` r
ggplot(data = stormdata,
       aes(x= year, 
           fill = type)) + 
  geom_bar(position = "dodge", aes(y = ..prop..)) +
  scale_fill_brewer(palette="Dark2") + 
  facet_wrap(~type)
```

![](hw2_files/figure-markdown_github/unnamed-chunk-4-2.png)

``` r
  labs(title = "Proportion of Storm Types Occuring Each Year (1995-2000)") +
  theme(panel.background = element_rect(fill = "gray70"), 
        legend.background = element_rect(fill = "gray80"),
        axis.title = element_blank()) +
  scale_x_continuous("year")
```

    ## NULL

### Problem 3

Given below are two data visualizations that violate many data visualization best practices. Improve these visualizations using R and the tips for effective visualizations that we introduced in class. You should produce one visualization per data set. Your visualizaiton should be accompanied by a *brief* paragraph describing the choices you made in your improvement, specifically discussing what you didn't like in the original plots and why, and how you addressed them in the visualization you created.

#### (a) **Fisheries**

Fisheries and Aquaculture Department of the Food and Agriculture Organization of the United Nations collects data on fisheries production of countries. [This Wikipedia page](https://en.wikipedia.org/wiki/Fishing_industry_by_country) lists fishery production of countries for 2005. For each country tonnage from capture and aquaculture are listed. Note that countries which harvested less than 100,000 tons are not included in the data. The source data can be found in the `fisheries.csv` data set (in the data folder of this assignment). The following plots were produced based off the data given on the Wikipedia page.

![fisheries-plot](img/fisheries.png)

``` r
fisheries <- read_csv("data/fisheries.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   country = col_character(),
    ##   capture = col_double(),
    ##   aquaculture = col_double()
    ## )

``` r
ggplot(data = fisheries, 
       aes(x = country)) +
  geom_bar(aes(y = aquaculture),
    stat = "identity", 
    fill = "blue",
           alpha = 0.5) + 
  geom_bar(aes(y = capture), 
           stat = "identity", 
           fill = "red",
           alpha = 0.5) + 
  scale_y_log10() +
  theme(axis.text.x = element_text(angle = 85, hjust = 1, size = 5),
        axis.title.x = element_blank(),
        panel.grid.major.x = element_blank(),
        panel.grid.minor.x = element_blank(),
        panel.grid.major.y = element_line(color = "black", linetype = 3),
        panel.grid.minor.y = element_line(color = "black", linetype = 3),
        panel.background = element_rect("white"),
        plot.subtitle = element_text(size = 8))+
  annotate("text", 
           label = "Capture",
           x = 56,
           y = 7000000,
           size = 4, 
           color = "red",
           alpha = 0.5) +
  annotate("text", 
           label = "Aquaculture", 
           x = 54, 
           y = 30000000, 
           size = 4, 
           color = "blue", 
           alpha = 0.5) +
  labs(title = "Tonnage of Fishery Production From Capture and Aquaculture by Country",
       y = "Tonnage of Fish Produced",
       subtitle = "Tonnage displayed in Log Scale")
```

    ## Warning: Removed 6 rows containing missing values (position_stack).

![](hw2_files/figure-markdown_github/unnamed-chunk-5-1.png)

The original figures describing fishery production in different countries were horrendous. The plot on the left made the mistake of viewing the fisheries data as a continuous variable rather than a discrete measure for each country. Moreover, the scale made it difficult to discern actual values of fishery production for countries other than Papua New Guinea, and the alpha value of 1 made it impossible to see the capture data for countries where fish capture tonnage that was lower than aquaculture tonnage, such as in Ecuador and Peru. The two pie charts are just monsterous, while they give an acceptable impression of relative size of fishery production in different continents they fail to show what colors correspond to most continents, and they completely fail to provide the viewer with a sense of the actual magnitude of the fishery production in any of the countries. In an attempt to fix these problems I created a bar plot with discrete values of capture and aquaculture data per country overlayed on a plot with a log-scaled y-axis. This makes it much easier to tell which countries have higher production of capture and/or aquaculture than other countries, and simultaniously get a general sense of the total fishery production of each country.

#### (b) **Instructional staff employee trends**

The American Association of University Professors (AAUP) is a nonprofit membership association of faculty and other academic professionals. [This report](https://www.aaup.org/sites/default/files/files/AAUP_Report_InstrStaff-75-11_apr2013.pdf) compiled by the AAUP shows trends in instructional staff employees between 1975 and 2011.

The following plot was presented in that report.

![instructors-plot](img/inst_staff.png)

The source data can be found in the \[`instructors.csv`\] data set in the data folder of this assignment.

``` r
instructors <- read_csv("data/instructors.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   job = col_character(),
    ##   year = col_double(),
    ##   staff.pct = col_double()
    ## )

``` r
ggplot(data = instructors, 
       aes(x=year, 
           y=staff.pct,
           color = job)) + 
  geom_point() + 
  geom_smooth(se = FALSE) +
  scale_color_brewer(NULL, palette = "Dark2") +
  labs(title = "Trends in Instructional Staff Employment Status, 1975-2011",
    subtitle = "All Institutions, National Totals",
    x = "Year",
    y = "Percent of Total Instructional Staff",
    color = "Staff Employment Status") +
  theme(panel.grid.minor.x = element_blank(),
        panel.grid.minor.y =  element_line(linetype = 5, color = "black"),
        panel.grid.major.x = element_blank(),
        panel.grid.major.y = element_line(linetype = 5, color = "black"),
        plot.background = element_rect(fill = "gray90"),
        panel.background = element_rect(fill = "white"))
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](hw2_files/figure-markdown_github/unnamed-chunk-6-1.png)

The central problem facing the origional plot of trends in instructional staff employment status was that it attempted to show trends over time using employment status as the independant variable. This makes comparing the different levels of employment status in years other than 1975 and 2011 (where the levels are printed in text) difficult and unituitive. In order to address this I chose to put time on the x-axis and map trends in employment status over time differentiating between employment stati with color and adding trendlines. This way discerning relative percent of total instructional staff occupied by different employment stati over time is much easier. Moreover, this allows for easier prediction of percent of total instructional staff occupied by each employment status in interveining years not mapped out in the original figure (such as 1980).

### Problem 4

(An adaptation of exercise 3.7)

Using the data set `Top25CommonFemaleNames.csv` (in the data folder of this assignment), recreate the "Median Names for Females with the 25 Most Common Names" graphic from FiveThirtyEight ([link to graphic](https://fivethirtyeight.com/wp-content/uploads/2014/05/silver-feature-most-common-women-names3.png?w=1150); [link to full article](https://fivethirtyeight.com/features/how-to-tell-someones-age-when-all-you-know-is-her-name/)).

``` r
FemNames <- read.csv("data/Top25CommonFemaleNames.csv")
ggplot(data=FemNames, 
       aes(x = median_age, 
           y = reorder(name, -median_age, FUN = median))) +
  geom_errorbarh(x = FemNames$median_age,
                 xmin=FemNames$q1_age, 
                 xmax=FemNames$q3_age,
                 size = 4.5, 
                 color = "goldenrod1", 
                 alpha = 0.5) +
  geom_point(x=FemNames$median_age,
             size = 2.5,
             fill = "firebrick1",
             color = "white", 
             pch = 21) +
  expand_limits(x = c(12,72)) +
  scale_x_continuous(position = "top", 
                     breaks = seq(5,85, by = 10)) +
  labs(title = "Median Ages for Females With the 25 Most Common Names",
       subtitle = "Among Americans estimated to be alive as of Jan. 1, 2014") +
  theme(axis.title.x = element_blank(), 
        axis.title.y = element_blank(),
        plot.background = element_rect(fill = "white"),
        panel.background = element_rect(fill = "white"),
        plot.title = element_text(face = "bold"),
        panel.grid.minor = element_blank(),
        panel.grid.major.x = element_line(linetype = 3, color = "black"),
        panel.grid.major.y = element_blank(),
        axis.ticks.x = element_blank(),
        axis.ticks.y = element_blank()) + 
  annotate(geom="text", 
           x=25, 
           y=16.1, 
           label="< 25th", 
           color="black", 
           size = 3.5) +
  annotate(geom = "text",
           x=52, 
           y=16.1, 
           label="75th percentile >",
           color="black", 
           size = 3.5) + 
  annotate(geom = "point",
           x=60, 
           y=22,
           fill = "firebrick1", 
           size = 3.5,
           pch = 21) + 
  annotate(geom = "text",
           x=63.4, 
           y=22.2,
           color="black",
           label = "median",
           size = 3.5,
           pch = 21)
```

    ## Warning: Ignoring unknown parameters: x

    ## Warning: Ignoring unknown parameters: shape

![](hw2_files/figure-markdown_github/unnamed-chunk-7-1.png)
