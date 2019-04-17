Team Assignment 3: Classification
================
Math 285, Winter 2019

Due date
--------

-   Push your test set predictions to GitHub by

-   Push your report.Rmd and report.md files to GitHub by 11:59 p.m. on Wednesday, March 6.

Description
-----------

Use the training data below to predict the county-level `winner16` of the 2016 presidential election, classified as Democratic (Clinton) or Republican (Trump):

``` r
train <- read_csv("https://aloy.rbind.io/data/train.csv")
```

The first 51 variables in this data frame contain county-level demographic and economic data. The table at the end of this document shows the dictionary that describes these variables.

``` r
key <- read.csv("https://aloy.rbind.io/data/county_facts_dictionary.csv")
```

### Your task

Use the training data to create classification model for `winner16`. I'll be judging your predictions based on your accuracy.

-   Use validation or cross-validation with the training data to avoid overfitting your model to the (entire) training data set which could produce a poor fit to the new test data.
-   Be sure to tune your model.
-   You don't need to include all 51 predictors in your model/method, so be sure to explore and bring in background knowledge.
-   There is variation on when the county-level variables were measured. Ideally the time frame of these would all in 2016 but this type of info is not always available!

### Deliverables

1.  **Predictions** After class on Monday (3/4) I will provide a test data set without the `winner` column.

    ``` r
    testNoY <- read_csv("https://aloy.rbind.io/data/testNoY.csv")
    ```

    Once you find a suitable classifier using the training data, use your classifier to predict responses for the test data. Add these predicted `pred_winner` values to the 52nd column. **Use this exact column name!** Save this modified test data file as a .csv with your team's last names added to the file name:

    ``` r
    write_csv(testNoY, "testNoY_LastNames.csv")
    ```

    Push this file to your GitHub repo. I will compute your accuracy and share the "leaderboard." *Make sure that you don't rearrange the order of the rows because there is not a row id given in the data to match with the actual response file!*

2.  **Write-up** Produce a detailed (2-3 pages) write-up of your classification method. Describe any data cleanup (preprocessing) that was needed and describe how you arrived at your final classifier, including any error (or other evaluation metrics) measured during training. I should be able to read your writeup and come away with a sound understanding of how to reproduce your test set predictions. If you use any classification methods not covered in class, you should explain how the methods work. (You are not expected to do this, but you can if you're interested.)

    I don't want to see R code in your write up, but it should be included either at the end of the write up document or in a separate .Rmd. If your code is slow to run, please submit separate writeup .Rmd and analysis code .Rmd files. Either way, make sure your code is concise and well commented. I should be able to run your code and reproduce your test set predictions.

Team assignments
----------------

[Link to assignment](https://classroom.github.com/g/Wn3XRSCF)

The first group member to accept the assignment should create a team named "team-hw3-X" where X denotes your team number.

|  Team| Name                    | Partner                  |
|-----:|:------------------------|:-------------------------|
|     5| Alyssa Akiyama          | Zhao, Megan              |
|     6| Noam Benkler            | White, Lewis             |
|    13| Nupur Bindal            | Meza-Bigornia, Jez       |
|    15| Joey Caradimitropoulo   | Chen, Serafina           |
|    15| Serafina Chen           | Caradimitropoulo, Joey   |
|    16| Dean Gladish            | Smith, Liralyn           |
|     8| Phuoc Huynh             | Isbell, Nate             |
|     8| Nate Isbell             | Huynh, Phuoc             |
|     1| Dallas Keate            | Roy, Andrew              |
|    12| Andrew Lin              | Rye, Dylan               |
|    14| Yanhan Lyu              | Miao, Kitty              |
|     4| Nathan Mannes           | Mehta, Tanvi             |
|     2| Daniel Matsuda          | Perl, David              |
|    17| Natalie Maurice         | Mayville, Quinn          |
|    17| Quinn Mayville          | Maurice, Natalie         |
|     4| Tanvi Mehta             | Mannes, Nathan           |
|    13| Jez Meza-Bigornia       | Bindal, Nupur            |
|    14| Kitty Miao              | Lyu, Yanhan              |
|    10| Jay Na                  | Yu, Kavie                |
|     2| David Perl              | Matsuda, Daniel          |
|    11| Elliot Pickens          | Schoch, Tim              |
|     3| Aaron Prentice          | Ruan, Chunjin            |
|     1| Andrew Roy              | Keate, Dallas            |
|     3| Chunjin Ruan            | Prentice, Aaron          |
|    12| Dylan Rye               | Lin, Andrew              |
|    11| Tim Schoch              | Pickens, Elliot          |
|     7| Ben Schwartz            | Zaytoun, Christian       |
|    16| Liralyn Smith           | Gladish, Dean            |
|     6| Lewis White             | Benkler, Noam            |
|     9| Siang Wongrattanapiboon | Zhang, Arthur            |
|    10| Kavie Yu                | Na, Jay                  |
|     7| Christian Zaytoun       | Schwartz, Ben            |
|     9| Arthur Zhang            | Wongrattanapiboon, Siang |
|     5| Megan Zhao              | Akiyama, Alyssa          |

Data Dictionary
---------------

| column\_name | description                                                            |
|:-------------|:-----------------------------------------------------------------------|
| PST045214    | Population, 2014 estimate                                              |
| PST040210    | Population, 2010 (April 1) estimates base                              |
| PST120214    | Population, percent change - April 1, 2010 to July 1, 2014             |
| POP010210    | Population, 2010                                                       |
| AGE135214    | Persons under 5 years, percent, 2014                                   |
| AGE295214    | Persons under 18 years, percent, 2014                                  |
| AGE775214    | Persons 65 years and over, percent, 2014                               |
| SEX255214    | Female persons, percent, 2014                                          |
| RHI125214    | White alone, percent, 2014                                             |
| RHI225214    | Black or African American alone, percent, 2014                         |
| RHI325214    | American Indian and Alaska Native alone, percent, 2014                 |
| RHI425214    | Asian alone, percent, 2014                                             |
| RHI525214    | Native Hawaiian and Other Pacific Islander alone, percent, 2014        |
| RHI625214    | Two or More Races, percent, 2014                                       |
| RHI725214    | Hispanic or Latino, percent, 2014                                      |
| RHI825214    | White alone, not Hispanic or Latino, percent, 2014                     |
| POP715213    | Living in same house 1 year & over, percent, 2009-2013                 |
| POP645213    | Foreign born persons, percent, 2009-2013                               |
| POP815213    | Language other than English spoken at home, pct age 5+, 2009-2013      |
| EDU635213    | High school graduate or higher, percent of persons age 25+, 2009-2013  |
| EDU685213    | Bachelor's degree or higher, percent of persons age 25+, 2009-2013     |
| VET605213    | Veterans, 2009-2013                                                    |
| LFE305213    | Mean travel time to work (minutes), workers age 16+, 2009-2013         |
| HSG010214    | Housing units, 2014                                                    |
| HSG445213    | Homeownership rate, 2009-2013                                          |
| HSG096213    | Housing units in multi-unit structures, percent, 2009-2013             |
| HSG495213    | Median value of owner-occupied housing units, 2009-2013                |
| HSD410213    | Households, 2009-2013                                                  |
| HSD310213    | Persons per household, 2009-2013                                       |
| INC910213    | Per capita money income in past 12 months (2013 dollars), 2009-2013    |
| INC110213    | Median household income, 2009-2013                                     |
| PVY020213    | Persons below poverty level, percent, 2009-2013                        |
| BZA010213    | Private nonfarm establishments, 2013                                   |
| BZA110213    | Private nonfarm employment, 2013                                       |
| BZA115213    | Private nonfarm employment, percent change, 2012-2013                  |
| NES010213    | Nonemployer establishments, 2013                                       |
| SBO001207    | Total number of firms, 2007                                            |
| SBO315207    | Black-owned firms, percent, 2007                                       |
| SBO115207    | American Indian- and Alaska Native-owned firms, percent, 2007          |
| SBO215207    | Asian-owned firms, percent, 2007                                       |
| SBO515207    | Native Hawaiian- and Other Pacific Islander-owned firms, percent, 2007 |
| SBO415207    | Hispanic-owned firms, percent, 2007                                    |
| SBO015207    | Women-owned firms, percent, 2007                                       |
| MAN450207    | Manufacturers shipments, 2007 ($1,000)                                 |
| WTN220207    | Merchant wholesaler sales, 2007 ($1,000)                               |
| RTN130207    | Retail sales, 2007 ($1,000)                                            |
| RTN131207    | Retail sales per capita, 2007                                          |
| AFN120207    | Accommodation and food services sales, 2007 ($1,000)                   |
| BPS030214    | Building permits, 2014                                                 |
| LND110210    | Land area in square miles, 2010                                        |
| POP060210    | Population per square mile, 2010                                       |
