---
layout: post
title: Text Mining Company Reviews (in R)
subtitle: Case of MBB Consulting
bigimg: /img/mckbuilding.jpg
tags: [r-project, glassdoor, webscraping, text-mining, text-analytics, sentiment-analysis]
output: 
  html_document: 
    keep_md: yes
---























_This post is about applying basic text mining tasks to Glassdoor company reviews to find out what employees write the most about to describe their workplace experiences, and whether they tend to be expressed in a more negative, positive or neutral way. Final results are shown as visuals, both tables and graphs._

=======

Ever wondered what it's like to work for a specific company? If so, you've probably looked into [Glassdoor](https://www.glassdoor.com). The site contains user-generated content on which people share feedback about company-specific compensation, interview experiences, and post anonymous reviews that evaluate the workplace. Its features include star ratings (on a 5-point scale), where employees award the company stars on a mix of factors such as workplace culture, work/life balance, and senior management. 

The star ratings are enough for some people to get an idea of whether the prospective company has the right environment for them, but for others, it doesn't tell the story about sentiments towards the workplace. Sentiments reflect people's attitudes and opinions based on what they have to say - whether they are positive, negative, or neutral - and can provide a proxy for employee satisfaction. 

It makes sense to want to work at a place where employee satisfaction is high. The more satisfied employees are, the better the attitudes are about their work, and the better their performance will be. This makes a workplace attractive. The reviews section of Glassdoor is a good resource to gauge this, but involves sifting through pages and pages of reviews - some decidedly misleading and exaggerated, others insightful and useful - and it can be a time-consuming endeavor. 

**An alternative is to streamline this task and get a high-level overview shown primarily as tables and charts.** One way to do this is through text mining. The idea is to apply web scraping to the written reviews about a company posted on Glassdoor to create a database of text. After obtaining this data, it can be applied to text analytics and [sentiment analysis](https://en.wikipedia.org/wiki/Sentiment_analysis).

We'll use the "Big Three" as an example in this post. That's **McKinsey & Co.**, **The Boston Consulting Company** and **Bain & Co.** - widely known as "MBB" - and they're the world's largest consulting firms by revenue. 

A little bit about MBB: they all focus on strategy consulting at board level and as leaders in their industry, they excel at what they do. The world's most ambitious and brightest minds in business are attracted to these consultancies. They invest heavily in their people who undergo rigorous core strategy training and engage with Fortune 500 clients. They're known for providing ongoing opportunities and support for professional advancement even after leaving the firm (i.e., exit opportunities). However, along with these benefits comes the downsides of long hours and travel requirements that tend to upend life outside of work. 

Having never worked at a Big Three, my observations about what it's like to work there are purely subjective. So with that said, let's check out the results.

**PART 1: REVIEWER PROFILE**

First, let's get an overview of the reviewers' profile in terms of the years that the reviews were posted, as well as the office locations of the MBB firms under review, and the job titles to identify their positions.

The use of peer review sites have grown to a point where today, many consider them to be a generally reliable source of feedback. The growth of Glassdoor is evident here, with MBB employee review contributions growing significantly since the site was created in 2008. To date, nearly 6,500 total reviews have been posted among the three firms.

(Keep in mind that since they all have a different number of reviews, the amount of text we'll be looking at later will be smaller for BCG and larger for McKinsey and Bain.)

<table class="table table-hover table-striped table-condensed" style="font-size: 10px; width: auto !important; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">Number of Reviews by Year</caption>
 <thead>
  <tr>
   <th style="text-align:right;"> Year </th>
   <th style="text-align:right;"> Freq </th>
   <th style="text-align:right;"> Percent </th>
  </tr>
 </thead>
<tbody>
  <tr grouplength="11"><td colspan="3" style="border-bottom: 1px solid;"><strong>Bain &amp; Co. - Total Reviews: 2,093</strong></td></tr>
<tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2008 </td>
   <td style="text-align:right;width: 2cm; "> 39 </td>
   <td style="text-align:right;width: 2cm; "> 1.9 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2009 </td>
   <td style="text-align:right;width: 2cm; "> 52 </td>
   <td style="text-align:right;width: 2cm; "> 2.5 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2010 </td>
   <td style="text-align:right;width: 2cm; "> 48 </td>
   <td style="text-align:right;width: 2cm; "> 2.3 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2011 </td>
   <td style="text-align:right;width: 2cm; "> 59 </td>
   <td style="text-align:right;width: 2cm; "> 2.8 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2012 </td>
   <td style="text-align:right;width: 2cm; "> 93 </td>
   <td style="text-align:right;width: 2cm; "> 4.4 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2013 </td>
   <td style="text-align:right;width: 2cm; "> 137 </td>
   <td style="text-align:right;width: 2cm; "> 6.5 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2014 </td>
   <td style="text-align:right;width: 2cm; "> 175 </td>
   <td style="text-align:right;width: 2cm; "> 8.4 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2015 </td>
   <td style="text-align:right;width: 2cm; "> 332 </td>
   <td style="text-align:right;width: 2cm; "> 15.8 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2016 </td>
   <td style="text-align:right;width: 2cm; "> 426 </td>
   <td style="text-align:right;width: 2cm; "> 20.3 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2017 </td>
   <td style="text-align:right;width: 2cm; "> 423 </td>
   <td style="text-align:right;width: 2cm; "> 20.2 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2018 </td>
   <td style="text-align:right;width: 2cm; "> 311 </td>
   <td style="text-align:right;width: 2cm; "> 14.8 </td>
  </tr>
  <tr grouplength="11"><td colspan="3" style="border-bottom: 1px solid;"><strong>Boston Consulting Group - Total Reviews: 1,687</strong></td></tr>
<tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2008 </td>
   <td style="text-align:right;width: 2cm; "> 39 </td>
   <td style="text-align:right;width: 2cm; "> 2.3 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2009 </td>
   <td style="text-align:right;width: 2cm; "> 54 </td>
   <td style="text-align:right;width: 2cm; "> 3.2 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2010 </td>
   <td style="text-align:right;width: 2cm; "> 51 </td>
   <td style="text-align:right;width: 2cm; "> 3.0 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2011 </td>
   <td style="text-align:right;width: 2cm; "> 57 </td>
   <td style="text-align:right;width: 2cm; "> 3.4 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2012 </td>
   <td style="text-align:right;width: 2cm; "> 94 </td>
   <td style="text-align:right;width: 2cm; "> 5.6 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2013 </td>
   <td style="text-align:right;width: 2cm; "> 120 </td>
   <td style="text-align:right;width: 2cm; "> 7.1 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2014 </td>
   <td style="text-align:right;width: 2cm; "> 137 </td>
   <td style="text-align:right;width: 2cm; "> 8.1 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2015 </td>
   <td style="text-align:right;width: 2cm; "> 244 </td>
   <td style="text-align:right;width: 2cm; "> 14.5 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2016 </td>
   <td style="text-align:right;width: 2cm; "> 317 </td>
   <td style="text-align:right;width: 2cm; "> 18.8 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2017 </td>
   <td style="text-align:right;width: 2cm; "> 364 </td>
   <td style="text-align:right;width: 2cm; "> 21.6 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2018 </td>
   <td style="text-align:right;width: 2cm; "> 211 </td>
   <td style="text-align:right;width: 2cm; "> 12.5 </td>
  </tr>
  <tr grouplength="11"><td colspan="3" style="border-bottom: 1px solid;"><strong>McKinsey &amp; Co. - Total Reviews: 2,709</strong></td></tr>
<tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2008 </td>
   <td style="text-align:right;width: 2cm; "> 63 </td>
   <td style="text-align:right;width: 2cm; "> 2.3 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2009 </td>
   <td style="text-align:right;width: 2cm; "> 88 </td>
   <td style="text-align:right;width: 2cm; "> 3.2 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2010 </td>
   <td style="text-align:right;width: 2cm; "> 138 </td>
   <td style="text-align:right;width: 2cm; "> 5.1 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2011 </td>
   <td style="text-align:right;width: 2cm; "> 95 </td>
   <td style="text-align:right;width: 2cm; "> 3.5 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2012 </td>
   <td style="text-align:right;width: 2cm; "> 166 </td>
   <td style="text-align:right;width: 2cm; "> 6.1 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2013 </td>
   <td style="text-align:right;width: 2cm; "> 196 </td>
   <td style="text-align:right;width: 2cm; "> 7.2 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2014 </td>
   <td style="text-align:right;width: 2cm; "> 242 </td>
   <td style="text-align:right;width: 2cm; "> 8.9 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2015 </td>
   <td style="text-align:right;width: 2cm; "> 412 </td>
   <td style="text-align:right;width: 2cm; "> 15.2 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2016 </td>
   <td style="text-align:right;width: 2cm; "> 490 </td>
   <td style="text-align:right;width: 2cm; "> 18.1 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2017 </td>
   <td style="text-align:right;width: 2cm; "> 543 </td>
   <td style="text-align:right;width: 2cm; "> 20.0 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2018 </td>
   <td style="text-align:right;width: 2cm; "> 279 </td>
   <td style="text-align:right;width: 2cm; "> 10.3 </td>
  </tr>
</tbody>
</table>

The locations of reviewers were not given in many instances, but from what is known, offices in New York City, Boston and Chicago are largely represented. 

<table class="table table-hover table-striped table-condensed" style="font-size: 10px; width: auto !important; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">Top 5 Number of Reviews by Location</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Location </th>
   <th style="text-align:right;"> Freq </th>
   <th style="text-align:right;"> Percent </th>
  </tr>
 </thead>
<tbody>
  <tr grouplength="5"><td colspan="3" style="border-bottom: 1px solid;"><strong>Bain &amp; Co.</strong></td></tr>
<tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Not Given </td>
   <td style="text-align:right;width: 2cm; "> 757 </td>
   <td style="text-align:right;width: 2cm; "> 36.1 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Boston, MA </td>
   <td style="text-align:right;width: 2cm; "> 186 </td>
   <td style="text-align:right;width: 2cm; "> 8.9 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Chicago, IL </td>
   <td style="text-align:right;width: 2cm; "> 135 </td>
   <td style="text-align:right;width: 2cm; "> 6.4 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> San Francisco, CA </td>
   <td style="text-align:right;width: 2cm; "> 115 </td>
   <td style="text-align:right;width: 2cm; "> 5.5 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> New York, NY </td>
   <td style="text-align:right;width: 2cm; "> 110 </td>
   <td style="text-align:right;width: 2cm; "> 5.3 </td>
  </tr>
  <tr grouplength="6"><td colspan="3" style="border-bottom: 1px solid;"><strong>Boston Consulting Group</strong></td></tr>
<tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Not Given </td>
   <td style="text-align:right;width: 2cm; "> 847 </td>
   <td style="text-align:right;width: 2cm; "> 50.2 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Boston, MA </td>
   <td style="text-align:right;width: 2cm; "> 95 </td>
   <td style="text-align:right;width: 2cm; "> 5.6 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> New York, NY </td>
   <td style="text-align:right;width: 2cm; "> 73 </td>
   <td style="text-align:right;width: 2cm; "> 4.3 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Chicago, IL </td>
   <td style="text-align:right;width: 2cm; "> 54 </td>
   <td style="text-align:right;width: 2cm; "> 3.2 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Gurgaon, Haryana (India) </td>
   <td style="text-align:right;width: 2cm; "> 51 </td>
   <td style="text-align:right;width: 2cm; "> 3.0 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> London, England (UK) </td>
   <td style="text-align:right;width: 2cm; "> 51 </td>
   <td style="text-align:right;width: 2cm; "> 3.0 </td>
  </tr>
  <tr grouplength="5"><td colspan="3" style="border-bottom: 1px solid;"><strong>McKinsey &amp; Co.</strong></td></tr>
<tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Not Given </td>
   <td style="text-align:right;width: 2cm; "> 1260 </td>
   <td style="text-align:right;width: 2cm; "> 46.5 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> New York, NY </td>
   <td style="text-align:right;width: 2cm; "> 259 </td>
   <td style="text-align:right;width: 2cm; "> 9.6 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> London, England (UK) </td>
   <td style="text-align:right;width: 2cm; "> 94 </td>
   <td style="text-align:right;width: 2cm; "> 3.5 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Chicago, IL </td>
   <td style="text-align:right;width: 2cm; "> 77 </td>
   <td style="text-align:right;width: 2cm; "> 2.8 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Gurgaon, Haryana (India) </td>
   <td style="text-align:right;width: 2cm; "> 57 </td>
   <td style="text-align:right;width: 2cm; "> 2.1 </td>
  </tr>
</tbody>
<tfoot><tr><td style="padding: 0; border: 0;" colspan="100%">
<sup></sup> Note: Top 5 based on percent ranking.</td></tr></tfoot>
</table>

A big chunk of position titles were also unknown, but it's likely that they're mostly different variations of the functional role of a consultant. 

<table class="table table-hover table-striped table-condensed" style="font-size: 10px; width: auto !important; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">Top 5 Number of Reviews by Position Title</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Position </th>
   <th style="text-align:right;"> Freq </th>
   <th style="text-align:right;"> Percent </th>
  </tr>
 </thead>
<tbody>
  <tr grouplength="5"><td colspan="3" style="border-bottom: 1px solid;"><strong>Bain &amp; Co.</strong></td></tr>
<tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Anonymous Employee </td>
   <td style="text-align:right;width: 2cm; "> 855 </td>
   <td style="text-align:right;width: 2cm; "> 40.8 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Consultant </td>
   <td style="text-align:right;width: 2cm; "> 302 </td>
   <td style="text-align:right;width: 2cm; "> 14.4 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Associate Consultant </td>
   <td style="text-align:right;width: 2cm; "> 287 </td>
   <td style="text-align:right;width: 2cm; "> 13.7 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Senior Associate Consultant </td>
   <td style="text-align:right;width: 2cm; "> 137 </td>
   <td style="text-align:right;width: 2cm; "> 6.5 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Manager </td>
   <td style="text-align:right;width: 2cm; "> 119 </td>
   <td style="text-align:right;width: 2cm; "> 5.7 </td>
  </tr>
  <tr grouplength="5"><td colspan="3" style="border-bottom: 1px solid;"><strong>Boston Consulting Group</strong></td></tr>
<tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Anonymous Employee </td>
   <td style="text-align:right;width: 2cm; "> 825 </td>
   <td style="text-align:right;width: 2cm; "> 48.9 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Consultant </td>
   <td style="text-align:right;width: 2cm; "> 250 </td>
   <td style="text-align:right;width: 2cm; "> 14.8 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Associate </td>
   <td style="text-align:right;width: 2cm; "> 114 </td>
   <td style="text-align:right;width: 2cm; "> 6.8 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Project Leader </td>
   <td style="text-align:right;width: 2cm; "> 97 </td>
   <td style="text-align:right;width: 2cm; "> 5.7 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Principal </td>
   <td style="text-align:right;width: 2cm; "> 73 </td>
   <td style="text-align:right;width: 2cm; "> 4.3 </td>
  </tr>
  <tr grouplength="5"><td colspan="3" style="border-bottom: 1px solid;"><strong>McKinsey &amp; Co.</strong></td></tr>
<tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Anonymous Employee </td>
   <td style="text-align:right;width: 2cm; "> 1224 </td>
   <td style="text-align:right;width: 2cm; "> 45.1 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Engagement Manager </td>
   <td style="text-align:right;width: 2cm; "> 233 </td>
   <td style="text-align:right;width: 2cm; "> 8.6 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Associate </td>
   <td style="text-align:right;width: 2cm; "> 227 </td>
   <td style="text-align:right;width: 2cm; "> 8.4 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Business Analyst </td>
   <td style="text-align:right;width: 2cm; "> 221 </td>
   <td style="text-align:right;width: 2cm; "> 8.1 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Senior Associate </td>
   <td style="text-align:right;width: 2cm; "> 59 </td>
   <td style="text-align:right;width: 2cm; "> 2.2 </td>
  </tr>
</tbody>
<tfoot><tr><td style="padding: 0; border: 0;" colspan="100%">
<sup></sup> Note: Top 5 based on percent ranking.</td></tr></tfoot>
</table>


**PART 2: STAR RATINGS SECTION**

Based on the Glassdoor star ratings, how do MBB employees rate the companies they work for? Pretty high actually. "Overall" ratings between 4 and 5 reflect a workplace where employees are considered "very satisfied." As a baseline comparison, the average is 3.4 where employees say satisfaction is just "OK" (based on the 700,000 employers reviewed on Glassdoor).

<table class="table table-striped table-hover table-condensed" style="font-size: 10px; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">Star Ratings</caption>
 <thead>
  <tr>
   <th style="text-align:center;"> Firm </th>
   <th style="text-align:center;"> Culture </th>
   <th style="text-align:center;"> Sr. Mgmt </th>
   <th style="text-align:center;"> Work-Life Bal. </th>
   <th style="text-align:center;"> Compensation </th>
   <th style="text-align:center;"> Career Opp. </th>
   <th style="text-align:center;"> Overall </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:center;font-weight: bold;color: black;"> Bain &amp; Co. </td>
   <td style="text-align:center;"> 4.8 </td>
   <td style="text-align:center;"> 4.5 </td>
   <td style="text-align:center;"> 3.6 </td>
   <td style="text-align:center;"> 4.7 </td>
   <td style="text-align:center;"> 4.7 </td>
   <td style="text-align:center;font-weight: bold;"> 4.7 </td>
  </tr>
  <tr>
   <td style="text-align:center;font-weight: bold;color: black;"> Boston Consulting Group </td>
   <td style="text-align:center;"> 4.3 </td>
   <td style="text-align:center;"> 3.9 </td>
   <td style="text-align:center;"> 3.1 </td>
   <td style="text-align:center;"> 4.4 </td>
   <td style="text-align:center;"> 4.3 </td>
   <td style="text-align:center;font-weight: bold;"> 4.3 </td>
  </tr>
  <tr>
   <td style="text-align:center;font-weight: bold;color: black;"> McKinsey &amp; Co. </td>
   <td style="text-align:center;"> 4.3 </td>
   <td style="text-align:center;"> 3.8 </td>
   <td style="text-align:center;"> 2.7 </td>
   <td style="text-align:center;"> 4.2 </td>
   <td style="text-align:center;"> 4.3 </td>
   <td style="text-align:center;font-weight: bold;"> 4.2 </td>
  </tr>
</tbody>
</table>

For Bain employees, satisfaction was highest in all factors when compared to others, especially when it comes to the approval of its senior leadership. McKinsey consistently received relatively lower satisfaction marks, and more so for the work-life balance factor. Ratings for BCG were split down the middle between the two others, but shared more similarities with McKinsey employees.


**PART 3: COMPANY REVIEWS SECTION**

Here's where the text mining was applied. The company reviews section of Glassdoor shows what employees have to say about the workplace. A review is divided into separate topics about the pros and cons. The _pros_ are the advantages and includes comments around what an employee liked or thought worked well in the company. Conversely, the _cons_ are the disadvantages and reflect what employees disliked or thought needed improvement. Requiring user feedback on both topics helped to make reviews more balanced.

**Pros**  
Focusing on just the pros for now, what words are employees using the most to describe their opinions and experiences? The charts below gives us a breakdown of the top words, both singular and pairs (i.e., two words that are used consecutively). For example, "consulting" is a single word, and "consulting firm" are word pairs. 

Bain employees describe something as being "fun" much more than McKinsey or BCG employees, with that word making it as one of the top ten used the most. Then there's "people," "culture," and "learning" in which all are used frequently across the firms. But _what about_ these single words? Unless used together with additional words to provide more distinct meanings, the pros of the workplace remain very vague.

![1](https://raw.githubusercontent.com/mguideng/text-mining-big3-reviews/master/images/1unigram.prosplot-1.png)

Looking at the word pairs below sheds more light about what are considered the greatest aspects about working at a Big Three: working with "smart people", the opportunities for "professional development," and the "steep learning" curves. 
These reflect the importance of knowledge within consultancy. As for the "culture," Bain employees refer to it as being a "supportive" one. BCG and McKinsey employees have less consensus about the varying words to describe their workplace culture and are not included as among the top pair words.

![2](https://raw.githubusercontent.com/mguideng/text-mining-big3-reviews/master/images/2bigram.prosplot-1.png)

Some of these words we've looked at so far are neutral, such as "people," "career," and "team." We can limit them to those categorized as being either positive (e.g., "fun" and "smart") or negative (e.g., "hard" and "uncertain") in sentiment.

![3](https://raw.githubusercontent.com/mguideng/text-mining-big3-reviews/master/images/3barcompare.pros-1.png)

BCG and McKinsey workers are more similar in their use of sentiment expressions, describing their experiences as mostly being "smart" and "amazing" and probably receiving good "benefits" packages. As for workers at Bain, there's more focus on a "fun" and "supportive" environment. Even the negative words used in the context of the pros like "challenging", "hard" and "steep" indicates that they enjoy a demanding environment that tests their abilities. Do you interpret it this way as well? 

**Cons**  
Let's move on to the cons section, where we'll examine the same visuals as above. First, the common single words.

![4](https://raw.githubusercontent.com/mguideng/text-mining-big3-reviews/master/images/4unigram.consplot-1.png)

All three firms reference the "hours" as being the most discussed downside of their workplaces, relatively more at Bain since its frequency is double that of the next word. There's the word "life." Intuitively it makes sense that it's used together with another frequent word, "balance," since after all, context is key and we're talking about the cons. For consultants, "travel" is thought of negatively, possibly because they have to do it so much which is known to be common in this industry. Also, "client" made it on the list, likely referencing the difficulties of meeting the very high demands of clients that is so common in this industry. 

The word pairs below provides some clarification, where indeed some variation of maintaining a good work-life balance is difficult given the hours and travel requirements. 

![5](https://raw.githubusercontent.com/mguideng/text-mining-big3-reviews/master/images/5bigram.consplot-1.png)

For sentiments, "hard", "difficult," and "tough" are synonymous with each other (where "tough" should be considered a negative). Taken together with what we know previously about work-life balance, it's very likely they're describing the challenges of maintaining a normal lifestyle.

![6](https://raw.githubusercontent.com/mguideng/text-mining-big3-reviews/master/images/6barcompare.cons-1.png)

**Sentiment Scoring of Pros & Cons**  
In this section, let's consider whether the written reviews as a whole tend to be expressed in a more negative, positive, or neutral light. One way to determine this is by computing a sentiment score for both the pros and cons taken together.

* For the **pros** text portions of the reviews: Positive words were given +1 point and negative words were given -1 point.

* For the **cons** portion: negative words were assigned -1, however positive words were too. This is because using a positive word to say something negative by negating is significantly more common (e.g., "It's not good" rather than "It's bad").

A net score for each review was derived by summing the +1 and -1 points. It is measured on a polar scale, with a negative value (less than zero) representing a more negative sentiment, and positive value (greater than zero) representing a more positive sentiment. Below shows the distribution of the reviewers' scores.

![7](https://raw.githubusercontent.com/mguideng/text-mining-big3-reviews/master/images/7hist-1.png)

The variability of sentiment scores shows that reviewers at MBB firms tend be quite neutral in the way they express their experiences. With such a high share of neutrally scored content, which I'll simply interpret as being between -2 and +2, it's likely that most reviewers are making general statements or statements that carry little sentiment, reflecting a lack of emotionally charged opinions.

<table class="table table-hover table-striped table-condensed" style="font-size: 10px; width: auto !important; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">Variability of Sentiment Scores</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Firm </th>
   <th style="text-align:right;"> Min </th>
   <th style="text-align:right;"> Qrtl.1 </th>
   <th style="text-align:right;"> Median </th>
   <th style="text-align:right;"> Mean </th>
   <th style="text-align:right;"> Qrtl.3 </th>
   <th style="text-align:right;"> Max </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;font-weight: bold;"> Bain </td>
   <td style="text-align:right;"> -28 </td>
   <td style="text-align:right;"> -2 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0.19 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 15 </td>
  </tr>
  <tr>
   <td style="text-align:left;font-weight: bold;"> BCG </td>
   <td style="text-align:right;"> -25 </td>
   <td style="text-align:right;"> -2 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0.22 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 15 </td>
  </tr>
  <tr>
   <td style="text-align:left;font-weight: bold;"> McKinsey </td>
   <td style="text-align:right;"> -33 </td>
   <td style="text-align:right;"> -1 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0.13 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 15 </td>
  </tr>
</tbody>
</table>

To put this a bit more into perspective, I consider neutral (i.e., for scores be near zero with little variability) to be pretty good in terms of reflecting satisfaction at the workplace. I've applied this sentiment analysis system to various other companies' reviews and score results tended to skew towards a negative polarity with high variability. A tendency for scores to be on a negative spectrum would actually be expected as normal for most employers given what we know about the prevalence of negativity bias common in review platforms (e.g., people are more motivated to write a review when they have a complaint, more words are used to describe negative experiences versus positive ones, etc.).

Lastly, the panel charts below show how the scores have changed over time at each of the three firms. A score for each review is represented by a single blue-shaded bar. It begins with the first review in 2008 and ends with the last review posted to date. Note that each chart is on a free-scale for Reviewer ID (on the x-axis) and each firm has a different number of total reviews; this makes it easier to see patterns within each individual chart however they are harder to compare across charts.

![8](https://raw.githubusercontent.com/mguideng/text-mining-big3-reviews/master/images/8senscore.sentxtplot-1.png)

Again, for our purposes, a review is considered neutral if the score falls between -2 and +2. Notice any patterns? Indeed we do see that the distribution of sentiment among reviewers are around near 0 and neutral, especially for McKinsey reviewers in the past few years. 

Another way of viewing the scores over time is by the annual averages for the three firms. In the chart below, it may appear that there are fluctuations, but it's actually subtle changes considering they're within a close fit range (-1.5 to +1.5). Also, it wasn't until around 2013/2014 that there were enough reviews on an annual basis to provide a more robust corpus body of text to run text analyses on. We've just past half of this year, so the score is incomplete for 2018. 

![9](https://raw.githubusercontent.com/mguideng/text-mining-big3-reviews/master/images/9senscore.sentxtplot.annual-1.png)

The stars are considered a reliable measure of satisfaction as is, but text analysis gives us an additional piece of the picture that can help us better understand what it's like to work at a particular employer. Given these considerations, sentiments as a proxy for satisfaction among MBB workers over the past ten years have been overall consistently positive. 

**Final Remarks**  
Of course, I'm trying to make things simple here and nothing is really that simple. When creating a sentiment analysis system, pre-processing the text and validating the methodology are crucial steps that can dramatically affect results. A solid sentiment test gets complex, so while this method doesn't fully capture the true sentiments of these reviews, it's a quick way to obtain general sentiments for my purposes. It's based on a decent sample size, the language is clean, there's consistency in format and content, and the contextual subject matter is understood. Also, it helped significantly that the reviews were already clustered by pros and cons topics to provide perspective. A simple recall test was done, where I ran 20 reviews each of ones I deemed positive, negative, and neutral in advance and the results were sensical. 

Additionally, there are some nuances that could be considered in the real world. For example, offices within the same firms at different locations are likely to have their own unique environments. There were also a few reviews I saw that were not written in the English language and should have been translated. I'm interested in getting the bigger picture, and I kept things simple for practicality. 

So I'll leave you to consider these results and encourage you to look into the many resources online where you can leverage text mining and sentiment analysis methods for yourself, perhaps even exploring the application of machine learning techniques. However, keep in mind the many limitations of applying computational methods when trying to uncover complex social phenomena that occurs in natural human language such as sarcasm, humor, persuasion, and so forth. 

=======

**Learning points**

* Learning process included basic exploration and analysis of text to identify patterns, keywords, and other attributes in the data to extract relevant information and then structure the text to derive insights.  
* Demonstrated web scraping, text analytics, and sentiment analysis on word-level using [tidy data principles](http://r4ds.had.co.nz/tidy-data.html) and using a binary lexicon to classify sentiment words.  
* Developed completely in R, the code can be found in my [github](https://github.com/mguideng/text-mining-big3-reviews).  
* Packages used: tidytext, tidyverse (includes: dplyr, ggplot2, stringr, tidyr), rvest, purr and kableExtra.  
* Inspired by [this post](https://juliasilge.com/blog/tidytext-0-1-4/) from Julia Silge, co-creator of the tidytext package.
