This is a package aimed at converting .agd files from Actigraph into data.frames.

Installation
============

``` r
library("devtools")
install_github("masalmon/convertagd")
```

Converting a single file
========================

`read_agd` is the function for reading a agd file and getting two tables out of it.

``` r
library("convertagd")
```

    ## Loading required package: DBI

    ## Loading required package: PhysicalActivity

    ## Warning: package 'PhysicalActivity' was built under R version 3.2.3

    ## Loading required package: RSQLite

    ## Loading required package: dplyr

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

    ## Loading required package: lubridate

    ## Loading required package: readr

``` r
file <- system.file("extdata", "dummyCHAI.agd", package = "convertagd")
testRes <- read_agd(file, tz = "GMT")
kable(testRes[["settings"]])
```

|  settingID| settingName             | settingValue                                 |
|----------:|:------------------------|:---------------------------------------------|
|          1| softwarename            | ActiLife                                     |
|          2| softwareversion         | 5.10.0                                       |
|          3| osversion               | Microsoft Windows NT 6.1.7601 Service Pack 1 |
|          4| machinename             | THINKCENTRE                                  |
|          5| datetimeformat          | dd-MM-yyyy                                   |
|          6| decimal                 | .                                            |
|          7| grouping                | ,                                            |
|          8| culturename             | English (India)                              |
|          9| finished                | true                                         |
|         10| devicename              | GT3XPlus                                     |
|         11| filter                  | Normal                                       |
|         12| deviceserial            | NEO1D31110533                                |
|         13| deviceversion           | 2.5.0                                        |
|         14| modenumber              | 61                                           |
|         15| epochlength             | 10                                           |
|         16| startdatetime           | 635670972000000000                           |
|         17| stopdatetime            | 635672088000000000                           |
|         18| downloaddatetime        | 635672179164562010                           |
|         19| batteryvoltage          | 4.22                                         |
|         20| original sample rate    | 30                                           |
|         21| subjectname             | 11                                           |
|         22| sex                     | Male                                         |
|         23| race                    | Asian / Pacific Islander                     |
|         24| limb                    | Other                                        |
|         25| epochcount              | 11159                                        |
|         26| sleepscorealgorithmname | Sadeh                                        |
|         27| customsleepparameters   |                                              |
|         28| notes                   |                                              |

``` r
kable(head(testRes[["raw.data"]]))
```

| date                |  axis1|  axis2|  axis3|  steps|  lux|  incline|
|:--------------------|------:|------:|------:|------:|----:|--------:|
| 2015-05-13 07:00:00 |      7|      1|     64|      1|    0|        3|
| 2015-05-13 07:00:10 |      5|      0|     44|      0|    0|        3|
| 2015-05-13 07:00:20 |      0|      0|     17|      0|    0|        3|
| 2015-05-13 07:00:30 |      0|      0|      0|      0|    0|        3|
| 2015-05-13 07:00:40 |      0|      0|      6|      0|    0|        3|
| 2015-05-13 07:00:50 |      0|      0|      1|      0|    0|        3|

Converting all files in a directory
===================================

Suppose you have a lot of agd files in one directory. You can convert all of them at the same time, outside of memory, using the `batch_read_agd` function.

It takes the path to the directory as input and will save the results in the current working directory.

You can either choose to get two csv files for each agd files :

-   originalfilename\_settings.csv

-   originalfilename\_raw.csv

Or to get two csv files for all agd files :

-   settings.csv which is a table with 28 columns (the 28 settings names) and as many rows as there are agd files in the directory.

-   raw\_data.csv which is a table with all measurements for all files, with a column "fileName" for identifying the individual sets of measurements.

This is how a call to the function looks like:

``` r
pathToDirectory <- system.file("extdata", package = "convertagd")
batch_read_agd(pathToDirectory, tz="GMT",
                allInOne=TRUE)
```