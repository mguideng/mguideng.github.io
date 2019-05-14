---
layout: post
title: Accessing SQLite and Querying with DBI in R 
subtitle: To Analyze Flight Activity at McCarran Airport in Las Vegas
bigimg: /img/2019-05-01_files/mccarranairport.jpg
tags: [r-project, flights, SQLite, DBI, queries]
hidden: true
output: 
  html_document: 
    keep_md: yes
    df_print: default
    self_contained: no
    code_folding: hide
    toc: yes
    toc_depth: 3
    toc_float:
      collapsed: no
---





_This post provides an analysis demo using flights data in **R**. The source data includes information on over 7 million scheduled nonstop U.S. domestic flights during 2018. Given the data's large size, the **DBI** package will used to access a **SQLite** database where the data is stored. That way, database queries can be generated outside of **R** without having to bring in the entire dataset as an object._ 

_To make it even more manageable, the focus will be on my hometown airport: McCarran International Airport (LAS), the main airport that services the Las Vegas metro area. The queries will revolve around summarizing flights activity at LAS and looking at a key on-time performance measure: flight delays. Findings will be displayed via tables and charts._

_For those interested in the **R** codes, toggle buttons are available throughout to show them. They are also available in its entirety on my [GitHub](https://github.com/mguideng/R-SQLite-Flights)._

=======

# INTRO (R + SQLITE + FLIGHTS DATA)

**R** provides various ways to access Comma Separated Values (CSV) files. CSV files are in a plain text format designed to easily exchange data between different applications. An easy way to import the data from a CSV file into **R** and keep the object in memory is to use the built-in `read.csv` function. 

Alternatively, if the CSV file is already stored as a table in an outside relational database, **R** can likely access the data since it can connect to almost any existing database type. A common method to do this is through the use of the **DBI** (DataBase Interface) package, which provides a database interface definition for communicating between **R** and relational database management systems (DBMS). Among them is [**SQLite**](https://sqlite.org/about.html), a light-weight, open source database engine. 

Since I currently store large datasets in my private **SQLite** DBMS, I opted for the latter option. That way, rather than reading in the entire dataset, I can use Structured Query Language (SQL) statements to retrieve just the chunks of data needed for specific analysis. The smaller chunks of data can be useful - necessary even - when handling very large datasets on personal computers with memory limitations. 

The subject data for this post is on arrival and departure information for non-stop domestic flights during 2018, by commercial airline carrier and by airport (both origin and destination). The airline carriers report the information to the Bureau of Transportation Statistics (BTS) and is publically available. Considering these airlines together make nearly 20,000 flight trips each day throughout nearly 360 US airports, they provide sizable metadata for the data analytics community looking to practice on larger datasets. It's used widely to test out statistical inference models and predictive machine learning algorithms. 

All the data will not be used since the focus will be on answering a series of questions about flight activity and on-time performance for a single airport: McCarran International Airport (or "LAS"), the main airport in the Las Vegas, Nevada metro area. Furthermore, the aim here is not so much about applying advanced analytical techniques. It will be simpler than that: to practice SQL queries to get the data needed to perform basic summaries that generate insight about LAS. Lastly, since exhibits are an effective way to succinctly report findings, the questions will be answered through corresponding tables and charts.

Thus, the learning goals covered here will be to:

- Access a database and run SQL queries in **R** (using **DBI** and **RSQLite** ), and
- Summarize the data and generate exhibits for each query (using **dplyr**, **ggplot2**, and **DT**).


# ABOUT THE DATA

The source data was abridged from the [U.S. Bureau of Transportation Statistics](http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time) and extracted as a set of plain-text CSV files. The data was then loaded into a **SQLite** database and saved as _"flights2018.db"_. 

The _"flights2018.db"_ database schema consists of four tables: 

- **flights** (month, dayofwk, carrier, origin, dest, num_flights, cancelled, dep_del, dep_del15, arr_del, arr_del15, distance, ad_carrier, ad_weather, ad_nas, ad_security, ad_lateaircraft)
- **airports** (code, city_name, airport_name)
- **carriers** (code, carrier_name)
- **days** (code, day)

The main table is _flights_, and we'll go over the details for all the column fields in a bit. For now, note that the names of the day ("dayofwk"), airline carrier ("carrier"), and airport ("origin" and "dest") for a scheduled flight are not explicitly given. Instead, they contain a code that will be used to join the related tables that provide the names as follows: 

- `flights.dayofwk` references `days.code`
- `flights.carrier` references `carrier.code`
- both `flights.origin` and `flights.dest` reference `airports.code`


# CONNECTING TO A SQLITE DATABASE

In order to interact with **SQLite**, we need to bring in **RSQLite**, the package that embeds **SQLite** in **R**. It is DBI-compliant, which means that it can serve as a back-end extension to **DBI** and make use of its common functions.  According to the **RSQLite** vignette, **DBI** is installed with **RSQLite** so there is no need to install it separately.


```r
install.packages("DBI")
```

After installation, the packages' libraries will be loaded into the **R** environment. **DBI** will be loaded first, not **RSQLite**, since the functions used will primarily come from **DBI**. 


```r
library(DBI)
library(RSQLite)
```

Connection to the database is made through the function call `dbConnect()`.


```r
# Connect to database (Note: this is generated on my local computer and connected to my local SQLite database.)
db <- dbConnect(RSQLite::SQLite(), dbname = "flights2018.db")
```

The first parameter is the name of the DBMS to connect to: `SQLite()`. The second is the name of the database file:`dbname = flights2018.db`. Note that **R** uses the current working directory contained in `getwd()` by default to find the database file. Otherwise, it needs a full directory path that leads to it. Lastly, this connection is assigned to a variable name "db", which will be used in the codes throughout the rest of this post.



## Check tables

From here, we can check whether we are accessing the database file correctly with the `dblistTable` function. We simply pass the database connection name through it.



```r
dbListTables(db)
```

```
## [1] "airports" "carriers" "days"     "flights"
```

As expected, all four tables are accessible and available for interaction within the **R** environment. 

## Check fields

How about the column (field) names? We can use `dbListFields()` to list them all.


```r
dbListFields(db, "flights")
```

```
##  [1] "month"           "dayofwk"         "carrier"        
##  [4] "origin"          "dest"            "num_flights"    
##  [7] "cancelled"       "dep_del"         "dep_del15"      
## [10] "arr_del"         "arr_del15"       "distance"       
## [13] "ad_carrier"      "ad_weather"      "ad_nas"         
## [16] "ad_security"     "ad_lateaircraft" "city_pairs"
```

## Definitions 

These 17 fields contain the following information: 

- **month** and **dayofwk**: dates of a scheduled flight, beginning with 1 for January (months) and 1 for Monday (day of week).    
- **carrier**: code assigned by IATA to identify an airline carrier.     
- **origin** and **dest**: code assigned by US DOT to identify a flight's origin and destination airports, respectively.    
- **num_flights**: equal to 1 since each entry corresponds to a single flight.    
- **cancelled**: indicator for whether a scheduled flight was cancelled (1 = cancelled).    
- **dep_del** and **arr_del**: minutes delayed at departure and at arrival, respectively, measured as the difference between scheduled and actual times (negative values = early).    
- **dep_del15** and **arr_del15**: indicator for whether the departure and arrival was delayed by 15 minutes or more (1 = delayed).   
- **distance**: distance in air miles between a flight's origin and destination airports.    
- **ad_carrier**, **ad_weather**, **ad_nas**, **ad_security**, and **ad_lateaircraft**: minutes contributed to arrival delay(s), reason(s) due to a carrier, weather, National Aviation System, airport security, and late-arriving aircraft, respectively (0 or more only when arr_del15 = 1, NA otherwise)


# RESEARCH QUESTIONS

The data in the _flights_ table provide for the development of a lot of interesting questions. The ones to address here include the following:

- What are the busiest days of the week and months of the year at LAS? 
- What are the most popular cities paired with LAS (both directions to and from)?  
- How does LAS fare when it comes to on-time performance (cancellations and delays)?  
- What are the main reasons for flight delays from LAS?


