---
layout: post
title: It's Harvesting Season
subtitle: Scraping Ripe Data
bigimg: /img/2018-08-01_files/berries.jpg
tags: [r-project, rvest, glassdoor, webscraping]
output: 
  html_document: 
    keep_md: yes
hidden: true
---

_It's officially August and growing season is at its peak in the Southwest U.S. With harvesting in full swing, there's so much produce available and on sale right now. Data is freely abundant year round, however, and with the potential of web scraping, the only real limitation is being able to access the information online. For this post, I'll demo ha'rvesting company reviews from the Glassdoor website by scraping the text data in R using `rvest`._

=======

Analysts like to look out for interesting datasets as they browse the web. If we're lucky, it's neatly packaged and ready for download, but that is hardly ever the case. HTML is in an unstructured format and no one wants to manually "copy and paste" into a spreadsheet to gather the data. I admit I've done this plenty of times before...and I hope not to ever have to again. 

###Enter web scraping

Web scraping is a technique of automatically mining information from a website. It's a way to build your own dataset that doesn't involve cramping your fingers. I think all skilled data analysts should have some scraping tools because there's so many possibilities in harvesting an abundance of data from the wide open spaces of the Internet.

Most things on the web can be scraped actually and there's many methods to do so, but did you know R has these capabilities? Better yet, the popular `rvest` package makes the process easy through an HTML parsing method. I've had a few folks ask me how I've done this using R. Having applied it recently to Glassdoor, I'll demo that site as an example.

###Text Scraping Demo

Let's say we want to scrape text data from the company reviews for Tesla. The URL is:  
[teslaurl](https://www.glassdoor.com/Reviews/Tesla-Reviews-E43129.htm)

Tesla has nearly 1,500 total reviews, so with 10 reviews per results page, we'll be scraping across ~150 web pages for the following:

**Date** - of when review was posted  
**Summary** - e.g., "Engineer"  
**Title** - e.g., "Current Employee - Anonymous Employee"  
**Pros** - upsides of the workplace  
**Cons** - downsides of the workplace  
**Helpful** - number marked as being helpful, if any

![](https://raw.githubusercontent.com/mguideng/rvest-scrape-glassdoor/master/images/gd-tesla.PNG)

With a few lines of code from the `rvest` package, we can get these text data into a table:

<script src="https://gist.github.com/mguideng/f5d175d7e1424750630217d33546cb0e.js"></script>

Whoa. One moment, I was data-less and then a few minutes later, a table just appeared! We now have a dataframe with 1,460 rows, one for each reviewer, that can be exported to a CSV file. What just happened here? Is this magic? 

###Nope, it's `rvest`!

To scrape the data, I first needed to identify the right HTML elements to select. The important thing to understand is that HTML consists of text and its tags, where tags have class attributes. By selecting certain tagged elements of a web page based on its attributes, you can then extract (or parse) the text contents of interest.

Here's a snippet of the HTML code that generates the Glassdoor page from the screenshot above (accessed by right-clicking from the web page, and clicking on "View page source"):

![](https://raw.githubusercontent.com/mguideng/rvest-scrape-glassdoor/master/images/devtools.PNG)

We see that the code contains the **Summary** text "Engineer", along with its tags (indicated by the angle brackets). The tag's attributes are specified by `<...class=reviewLink.><span class= "summary ">&#034;Engineer&#034;</span>`, where the particular `<span>` tag has a class attribute with a value of `summary` nested within another class attribute of `reviewLink`. 

I just confused myself honestly. Luckily I can rely on the [SelectorGadget](https://selectorgadget.com) tool to identify the CSS class names for me. This tool compliments the use of the `rvest` package and both are created by R expert Hadley Wickam. Using these two allowed me to perform the web scraping in R. Make sure to check out how they work together [here](https://cran.r-project.org/web/packages/rvest/vignettes/selectorgadget.html) because it will do a better job at explaining this process. Go ahead!

![](https://raw.githubusercontent.com/mguideng/mguideng.github.io/master/img/2018-08-01_files/gd-cat.gif)

Ok so here's the CSS class names descriptions for each variable:

**Date** = `#ReviewsFeed .subtle.small`  
**Summary** = `.reviewLink .summary:not([class*='hidden'])`  
**Title** = `#ReviewsFeed .hideHH`  
**Pros** = `#ReviewsFeed .pros:not([class*='hidden'])`  
**Cons** = `#ReviewsFeed .cons:not([class*='hidden'])`  
**Helpful** = `.tight`

Now that we've identified these, we can take a look at how they were used with the functions in `rvest`: 

* `read_html()`: returns an HTML document from the glassdoor.com URL by converting it into an XML object. Think of the HTML document as if it were a bunch of node objects representing the document's contents. It's stored as 'pg' in R's environment.
* `html_nodes()`: selects parts of the XML object based on the CSS class names we identified. The stored results (e.g., 'rev.date', 'rev.sum', 'rev.title') will be lists of the node objects and their tagged class attributes.
* `html_text()`: extracts the only the text from the tagged nodes.

There's more functions available in `rvest`, so refer to the [PDF documentation]( https://cran.r-project.org/web/packages/rvest/rvest.pdf) to explore them further.

I used the `map_dfr` function from the `purr` package to bind together the rows from each results page to create a single dataframe. Now we've successfully scraped all the variables for the company reviews and combined them in a dataframe, we can use regular expressions to create additional variables:

**Reviewer ID** - 1 to N reviewers by date, sorted from first to last  
**Year** - from Date  
**Location** - e.g., Las Vegas, NV  
**Position** - e.g., Engineer  
**Status** - full-time or part-time

<script src="https://gist.github.com/mguideng/1c9af5d66d7b05e53151101242767f64.js"></script>

###Alas, here's the final output

![](https://raw.githubusercontent.com/mguideng/rvest-scrape-glassdoor/master/images/tesla-df-output.PNG)

Since most of the information scraped is text data, rather than numerical or factor data, this dataset is better suited for text analytics areas such as applying Natural Language Processing techniques (e.g., tokenization, lemmatization) for concept extraction (e.g., topic modeling, sentiment analysis, and examining the frequency and relative importance of words).

=======

**Learning points**

  * HTML and CSS is a knowledge area I'm still learning about. SelectorGadget helped to identify the class names to pass through `html_nodes()`, however it had its limitations. It was picking up additional tags that were hidden and could not be deselected by the point-and-click tool. I couldn't figure out how to exclude the hidden tags without reducing the output manually. So special thanks to Kirk N. for poking around the page source code in Chrome for his solution, which was to add the `:not([class*='hidden']` descriptor.

  * Writing a scraper is fun, challenging, and opens up so many possibilities for data analysts. Although most things on the web are scrapable, there are restrictions and it may not be completely allowable; make sure to check the site's Terms of Use (especially if you create an account that requires a log-in) and refer to the restrictions (which can often be found by adding "/robots.txt" to the end of the website URL).

  * [Best practices for web scraping in R](https://gist.github.com/abelsonlive/3769469) recommends inserting a sleep interval in between each sequential scrape. I've started doing that just beginning with this mini-project. Not only do I not want my scraper booted, it's also not polite to bombard web servers with hundreds/thousands of immediately sequential requests. Here, I ran a loop for ~150 iterations, and in each iteration, the system was set to sleep for 5 seconds. For a well-behaved bot, I would suggest allowing 2 seconds minimum (5 seconds ideally) between each page request.

  * Most websites are developer-friendly and provide an Application Programming Interface (API) for obtaining data. Depending on my specific needs, APIs may be sufficient so there will be no need to write a scraper through HTTP requests. The API route involves registering with the organization to get a key to make requests using that key. Glassdoor has an API available, however not for the company reviews. 
