---
layout: post
title: It's Harvesting Season
subtitle: Scraping Ripe Data
bigimg: /img/2018-08-01_files/berries.jpg
tags: [r-project, rvest, glassdoor, webscraping]
output: 
  html_document: 
    keep_md: yes
hidden: false
---

_It's officially August and growing season is at its peak in the Southwest U.S. With harvesting in full swing, there's so much produce available and on sale right now. You know what's freely abundant year round? Data! And with the potential of web scraping, the only real limitation is being able to access the information online. For this post, I'll demo ha'rvesting company reviews from the Glassdoor website by scraping text data in R using `rvest`._

=======

Analysts like to look out for interesting datasets as they browse the web. If we're lucky, it's neatly packaged and readily available, but that is hardly ever the case. HTML is in an unstructured format and no one wants to manually "copy and paste" into a spreadsheet to gather the data. I admit I've done this plenty of times before...and I hope not to ever have to again. 

### Enter web scraping  
Web scraping is a technique of automatically mining information from a website. It's a way to build your own dataset that doesn't involve cramped fingers from Ctrl+C and Ctrl+V. I think all skilled data analysts should have some scraping tools because there's so many possibilities in harvesting an abundance of data from the wide open spaces of the Internet.

Most things on the web can be scraped actually and there's many methods to do so, but did you know R has these capabilities? Better yet, the popular `rvest` package makes the process easy through an HTML parsing method. I've had a few folks ask me how I've done this using R. Having applied it recently to Glassdoor, I'll demo that site as an example.

### "Why not use the API," you ask?  
Great question, my friend. Although most websites are developer-friendly and provide an Application Programming Interface (API) for obtaining data, there are various reasons why it may not be a better option over web scraping. In this case, the answer is simple: Glassdoor has an API available, but not for the company reviews (which are the contents I'm after). 

### Text Scraping Demo  
Let's say we want to scrape text data from the company reviews for Tesla. The URL is:  
[https://www.glassdoor.com/Reviews/Tesla-Reviews-E43129.htm](https://www.glassdoor.com/Reviews/Tesla-Reviews-E43129.htm)

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

Whoa. One moment, I was data-less and then a few minutes later, *boom!, a table appeared! We now have a dataframe with 1,460 rows, one for each reviewer, that can be exported to a CSV file. What just happened here? Is this magic? 

### Nope, it's `rvest`!  
Before I could let `rvest` do its thing and scrape the data, I first needed to spend time upfront to figure out:  
 1. how the data can be accessed, and  
 2. how the data is structured.

The first line of order is to know **how to access the data**. I explored how the URL changed as I clicked between the page results and applied different sorting. Again, the landing page will be: [https://www.glassdoor.com/Reviews/Tesla-Reviews-E43129.htm](https://www.glassdoor.com/Reviews/Tesla-Reviews-E43129.htm) and it includes the first 10 results. Clicking to page 2 changes the URL so that "_P2" is added towards the endpoint. With 1,467 total reviews (and no option to increase the number of results per page), I figured results would max out at "_P147" (i.e., round 1,467 up to the tenth which is 1,470 and divide by 10 = 147). To confirm this, I changed the URL directly and that is indeed where the last review falls off. This is shown in the R code with `maxresults`.

As for the sort, the landing page defaulted by most "Popular". What happens when I change the sort by clicking on "Date"? The URL changed to [https://www.glassdoor.com/Reviews/Tesla-Reviews-E43129.htm?sort.sortType=RD&sort.ascending=false&filter.employmentStatus=PART_TIME&filter.employmentStatus=UNKNOWN](https://www.glassdoor.com/Reviews/Tesla-Reviews-E43129.htm?sort.sortType=RD&sort.ascending=false&filter.employmentStatus=PART_TIME&filter.employmentStatus=UNKNOWN). I changed the "sort.ascending" parameter from "false" to "true" since I wanted the oldest reviews first. As for all the extra parameters regarding the employment status filters, they can be omitted since results defaulted to displaying regular and part time workers anyway when employment status was not specified in the URL. As shown in the R code, the `baseurl`, `company`, and `sort` will fetch only what is necessary in order to load the pages that will access the data. 

Every site is different. So as you browse by using the navigation tools to identify data of interest, it will be helpful to:  
 * formulate how the data output will be organized (e.g., type of sort);  
 * figure out whether you'll be applying filters for a subset of the results (e.g., only full-time workers); and  
 * pay attention to how the URLs change in the process.

### Now the tricky part: HTML  
(If you want to skip this stuff about HTML and CSS, just scroll down to the next section - Go Go (Selector)Gadget.)

Since the page's Hyper Text Markup Language (HTML) looked like something my cat would've typed out by walking across my keyboard, the tricky part (for me at least) was extracting the data out of it. In understanding **how the data is structured**, knowledge of HTML and Cascading Style Sheets (CSS) is a big advantage. HTML can be considered the building-blocks of a webpage whereas CSS describes the presentation of the webpage (e.g., font size, color, type; styling around images; layout).

Here's a snippet of the markup that generates the Glassdoor page from the screenshot above (accessed by right-clicking from the web page, and clicking on "View page source"):

![](https://raw.githubusercontent.com/mguideng/rvest-scrape-glassdoor/master/images/devtools.PNG)

I needed to identify the right HTML elements to select. In this case, the important thing to understand is that HTML consists of text (which will be our variables) and its tags, where tags have CSS `class` selectors (preceded by a "." and identifies multiple elements) and `id` selectors (preceded by "#" and identifies one element). By identifying certain tagged elements of the markup based on its attributes described by the selectors, I can then apply them to `rvest`'s functions to extract (or parse) the text contents of the variables. Each set of the variables shared a common structure and consistent pattern in its selector attributes across all ~1,500 reviews.

Take a look at the snippet of the markup again. It contains the **Summary** text "Engineer", along with its tags (indicated by the angle brackets). The tag's (incomplete) attributes are specified by `class=reviewLink lockedReview'><span class= "summary ">&#034;Engineer&#034;</span>`, where the particular `<span>` tag has a class attribute with a value of `summary` nested within the class attribute of `reviewLink`. Confused much? Don't fret. You can learn more about selectors [here](http://www.htmldog.com/guides/css/intermediate/classid/). Then you'll be more than ready to dive in and start scraping. 

### Go Go (Selector)Gadget  
Want a work-around for actually learning HTML & CSS? No problem. We can rely on the [SelectorGadget](https://selectorgadget.com) tool to identify the CSS selectors for us. This point-and-click tool compliments the use of the `rvest` package and both are created by R expert Hadley Wickam. Using these two are quite sufficient for anyone to do web scraping. Make sure to check out how they work together [here](https://cran.r-project.org/web/packages/rvest/vignettes/selectorgadget.html) because it will do a better job at explaining this process. Go ahead!

![](https://raw.githubusercontent.com/mguideng/mguideng.github.io/master/img/2018-08-01_files/gd-cat.gif)

Ok, so here are the CSS `class` and `id` descriptions for each variable:

**Date** = `#ReviewsFeed .subtle.small`  
**Summary** = `.reviewLink .summary:not([class*='hidden'])`  
**Title** = `#ReviewsFeed .hideHH`  
**Pros** = `#ReviewsFeed .pros:not([class*='hidden'])`  
**Cons** = `#ReviewsFeed .cons:not([class*='hidden'])`  
**Helpful** = `.tight`

### Back on track to `rvest`  
Now that we've identified these, we can take a look at how they were used with the functions in `rvest`: 

* `read_html()`: returns an HTML document from the glassdoor.com URL by converting it into an XML object. Think of the HTML document as if it were a bunch of node objects representing the document's contents. It's stored as `'pg'` in R's environment.
* `html_nodes()`: selects parts of the XML object based on the CSS class names we identified. The stored results (e.g., `'rev.date'`, `'rev.sum'`, `'rev.title'`) will be lists of the node objects and their tagged attributes.
* `html_text()`: extracts only the text portion from the tagged nodes.

There's more functions available in `rvest`, so refer to the [PDF documentation]( https://cran.r-project.org/web/packages/rvest/rvest.pdf) to explore them further.

In the R code, note that I used the `map_dfr` function from the `purr` package to bind together the rows from each results page (from 1 to the maximum) in order to create a single dataframe.

Now that we've successfully scraped all the variables for the company reviews and combined them in a dataframe, we can use regular expressions to create additional variables:

**Reviewer ID** - 1 to N reviewers by date, sorted from first to last  
**Year** - from Date  
**Location** - e.g., Las Vegas, NV  
**Position** - e.g., Engineer  
**Status** - full-time or part-time

<script src="https://gist.github.com/mguideng/1c9af5d66d7b05e53151101242767f64.js"></script>

### Alas, here's the final output   

![](https://raw.githubusercontent.com/mguideng/rvest-scrape-glassdoor/master/images/tesla-df-output.PNG)

You can export the table by using the `write.csv` function or analyze it directly in R.

Since most of this text information is character data, rather than numerical or factor data, this dataset is better suited for text analytics areas such as applying Natural Language Processing techniques (e.g., tokenization, lemmatization) for concept extraction (e.g., topic modeling, sentiment analysis, and examining the frequency and relative importance of words). 

=======

**Learning points**

* SelectorGadget helped to identify the selectors to pass through `html_nodes()`, however it had its limitations. It was picking up additional tags that were hidden and could not be deselected by the point-and-click tool. I couldn't figure out how to exclude the hidden tags without manually reducing the output. So special thanks to my friend Kirk N. for poking around the source code to provide a solution, which was to add the `:not([class*='hidden']` descriptor.

* Writing a scraper is fun, challenging, and opens up so many possibilities for data analysts and data scientists. Although most everything on the web is scrapable, there are restrictions and it may not be completely allowable. Make sure to check the site's Terms of Use (especially if you create an account that requires a log-in) and refer to the restrictions (which can often be found by adding "/robots.txt" to the end of the website URL) before deciding what/how to scrape and what you'll be doing with it.

* [Best practices for web scraping in R](https://gist.github.com/abelsonlive/3769469) recommends inserting a sleep interval in between each sequential scrape. I haven't considered this before, and it's a good point. Not only do I not want my scraper booted, it's also not polite to bombard web servers with hundreds/thousands of immediately sequential requests. Here, I ran a loop for ~150 iterations, and in each iteration, the system was set to sleep for 5 seconds. For a well-behaved bot, I would suggest allowing 2 seconds minimum (or 5 seconds ideally) between each page request.

* Going the API route involves registering with a site administrator to get a key and to make requests using that key. Depending on my specific needs, an API may be the better option over writing a scraper that makes HTTP requests. There can be reasons, however, why an API is not sufficient for your needs. Aside from lack of availability, what other reasons might one opt to bypass it? Consider the following scenarios: 
	+ The API available may not be as up-to-date as the visitor webpage itself and you are depending on current data; 
	+ Rate limits are more restrictive than your (polite) web scraper and you don't want to be subject to them;
	+ You simply want to gather data privately while remaining anonymous; or
	+ Just because it's a project you've been wanting to do. In that case, happy ha'rvesting!
