---
layout: post
title: Text Mining Glassdoor Reviews
subtitle: Case of Gaming Manufacturing in Las Vegas
bigimg: /img/slot.jpg
tags: [r-project, glassdoor, webscraping, text-mining, text-analytics, sentiment-analysis]
output: 
  html_document: 
    keep_md: yes
hidden: true
---























This post summarizes company reviews on Glassdoor. It contains tables and charts to show what employees write the most about to describe their workplace experiences, and whether they tend to be expressed in a more negative, positive, or neutral way.

A variety of text analytics tasks were applied on a case of top companies in the gaming manufacturing sector that have a large presence in Las Vegas. These include:

* Aristocrat Technologies,
* IGT, and
* Scientific Games.
 
Companies in this sector provide gambling products by designing, developing and manufacturing gaming displays, slot machines, card counters and shufflers, transaction systems, and related service support.

To date, over 1,500 reviews total have been posted among the three companies. Each company has a different number of reviews where the amount of text analyzed is larger for IGT and smaller for Aristocrat and Scientific Games. As such, bar graphs in a panel layout for all three companies may not necessarily be comparable across charts, however are displayed this way to make it easier to see patterns within each individual chart.

=======

**PART 1: REVIEWER PROFILE**

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
  <tr grouplength="10"><td colspan="3" style="border-bottom: 1px solid;"><strong>Aristocrat - Total Reviews: 300</strong></td></tr>
<tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2008 </td>
   <td style="text-align:right;width: 2cm; "> 1 </td>
   <td style="text-align:right;width: 2cm; "> 0.3 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2010 </td>
   <td style="text-align:right;width: 2cm; "> 3 </td>
   <td style="text-align:right;width: 2cm; "> 1.0 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2011 </td>
   <td style="text-align:right;width: 2cm; "> 6 </td>
   <td style="text-align:right;width: 2cm; "> 2.0 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2012 </td>
   <td style="text-align:right;width: 2cm; "> 19 </td>
   <td style="text-align:right;width: 2cm; "> 6.3 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2013 </td>
   <td style="text-align:right;width: 2cm; "> 33 </td>
   <td style="text-align:right;width: 2cm; "> 11.0 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2014 </td>
   <td style="text-align:right;width: 2cm; "> 25 </td>
   <td style="text-align:right;width: 2cm; "> 8.3 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2015 </td>
   <td style="text-align:right;width: 2cm; "> 57 </td>
   <td style="text-align:right;width: 2cm; "> 19.0 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2016 </td>
   <td style="text-align:right;width: 2cm; "> 60 </td>
   <td style="text-align:right;width: 2cm; "> 20.0 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2017 </td>
   <td style="text-align:right;width: 2cm; "> 70 </td>
   <td style="text-align:right;width: 2cm; "> 23.3 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2018 </td>
   <td style="text-align:right;width: 2cm; "> 26 </td>
   <td style="text-align:right;width: 2cm; "> 8.7 </td>
  </tr>
  <tr grouplength="11"><td colspan="3" style="border-bottom: 1px solid;"><strong>IGT - Total Reviews: 876</strong></td></tr>
<tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2008 </td>
   <td style="text-align:right;width: 2cm; "> 3 </td>
   <td style="text-align:right;width: 2cm; "> 0.3 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2009 </td>
   <td style="text-align:right;width: 2cm; "> 7 </td>
   <td style="text-align:right;width: 2cm; "> 0.8 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2010 </td>
   <td style="text-align:right;width: 2cm; "> 33 </td>
   <td style="text-align:right;width: 2cm; "> 3.8 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2011 </td>
   <td style="text-align:right;width: 2cm; "> 45 </td>
   <td style="text-align:right;width: 2cm; "> 5.1 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2012 </td>
   <td style="text-align:right;width: 2cm; "> 34 </td>
   <td style="text-align:right;width: 2cm; "> 3.9 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2013 </td>
   <td style="text-align:right;width: 2cm; "> 72 </td>
   <td style="text-align:right;width: 2cm; "> 8.2 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2014 </td>
   <td style="text-align:right;width: 2cm; "> 171 </td>
   <td style="text-align:right;width: 2cm; "> 19.5 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2015 </td>
   <td style="text-align:right;width: 2cm; "> 158 </td>
   <td style="text-align:right;width: 2cm; "> 18.0 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2016 </td>
   <td style="text-align:right;width: 2cm; "> 141 </td>
   <td style="text-align:right;width: 2cm; "> 16.1 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2017 </td>
   <td style="text-align:right;width: 2cm; "> 148 </td>
   <td style="text-align:right;width: 2cm; "> 16.9 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2018 </td>
   <td style="text-align:right;width: 2cm; "> 64 </td>
   <td style="text-align:right;width: 2cm; "> 7.3 </td>
  </tr>
  <tr grouplength="11"><td colspan="3" style="border-bottom: 1px solid;"><strong>Scientific Games - Total Reviews: 375</strong></td></tr>
<tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2008 </td>
   <td style="text-align:right;width: 2cm; "> 2 </td>
   <td style="text-align:right;width: 2cm; "> 0.5 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2009 </td>
   <td style="text-align:right;width: 2cm; "> 2 </td>
   <td style="text-align:right;width: 2cm; "> 0.5 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2010 </td>
   <td style="text-align:right;width: 2cm; "> 5 </td>
   <td style="text-align:right;width: 2cm; "> 1.3 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2011 </td>
   <td style="text-align:right;width: 2cm; "> 2 </td>
   <td style="text-align:right;width: 2cm; "> 0.5 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2012 </td>
   <td style="text-align:right;width: 2cm; "> 4 </td>
   <td style="text-align:right;width: 2cm; "> 1.1 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2013 </td>
   <td style="text-align:right;width: 2cm; "> 7 </td>
   <td style="text-align:right;width: 2cm; "> 1.9 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2014 </td>
   <td style="text-align:right;width: 2cm; "> 22 </td>
   <td style="text-align:right;width: 2cm; "> 5.9 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2015 </td>
   <td style="text-align:right;width: 2cm; "> 55 </td>
   <td style="text-align:right;width: 2cm; "> 14.7 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2016 </td>
   <td style="text-align:right;width: 2cm; "> 81 </td>
   <td style="text-align:right;width: 2cm; "> 21.6 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2017 </td>
   <td style="text-align:right;width: 2cm; "> 121 </td>
   <td style="text-align:right;width: 2cm; "> 32.3 </td>
  </tr>
  <tr>
   <td style="text-align:right;width: 3cm;  padding-left: 2em;" indentlevel="1"> 2018 </td>
   <td style="text-align:right;width: 2cm; "> 74 </td>
   <td style="text-align:right;width: 2cm; "> 19.7 </td>
  </tr>
</tbody>
</table>

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
  <tr grouplength="5"><td colspan="3" style="border-bottom: 1px solid;"><strong>Aristocrat</strong></td></tr>
<tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Not Given </td>
   <td style="text-align:right;width: 2cm; "> 89 </td>
   <td style="text-align:right;width: 2cm; "> 29.7 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Noida (India) </td>
   <td style="text-align:right;width: 2cm; "> 78 </td>
   <td style="text-align:right;width: 2cm; "> 26.0 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Las Vegas, NV </td>
   <td style="text-align:right;width: 2cm; "> 75 </td>
   <td style="text-align:right;width: 2cm; "> 25.0 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Sydney (Australia) </td>
   <td style="text-align:right;width: 2cm; "> 24 </td>
   <td style="text-align:right;width: 2cm; "> 8.0 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> North Ryde (Australia) </td>
   <td style="text-align:right;width: 2cm; "> 11 </td>
   <td style="text-align:right;width: 2cm; "> 3.7 </td>
  </tr>
  <tr grouplength="5"><td colspan="3" style="border-bottom: 1px solid;"><strong>IGT</strong></td></tr>
<tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Not Given </td>
   <td style="text-align:right;width: 2cm; "> 328 </td>
   <td style="text-align:right;width: 2cm; "> 37.4 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Reno, NV </td>
   <td style="text-align:right;width: 2cm; "> 183 </td>
   <td style="text-align:right;width: 2cm; "> 20.9 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Las Vegas, NV </td>
   <td style="text-align:right;width: 2cm; "> 110 </td>
   <td style="text-align:right;width: 2cm; "> 12.6 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Providence, RI </td>
   <td style="text-align:right;width: 2cm; "> 30 </td>
   <td style="text-align:right;width: 2cm; "> 3.4 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Austin, TX </td>
   <td style="text-align:right;width: 2cm; "> 26 </td>
   <td style="text-align:right;width: 2cm; "> 3.0 </td>
  </tr>
  <tr grouplength="5"><td colspan="3" style="border-bottom: 1px solid;"><strong>Scientific Games</strong></td></tr>
<tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Not Given </td>
   <td style="text-align:right;width: 2cm; "> 156 </td>
   <td style="text-align:right;width: 2cm; "> 41.6 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Alpharetta, GA </td>
   <td style="text-align:right;width: 2cm; "> 40 </td>
   <td style="text-align:right;width: 2cm; "> 10.7 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Chicago, IL </td>
   <td style="text-align:right;width: 2cm; "> 39 </td>
   <td style="text-align:right;width: 2cm; "> 10.4 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Bengaluru (India) </td>
   <td style="text-align:right;width: 2cm; "> 33 </td>
   <td style="text-align:right;width: 2cm; "> 8.8 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Las Vegas, NV </td>
   <td style="text-align:right;width: 2cm; "> 22 </td>
   <td style="text-align:right;width: 2cm; "> 5.9 </td>
  </tr>
</tbody>
<tfoot><tr><td style="padding: 0; border: 0;" colspan="100%">
<sup></sup> Note: Top 5 based on percent ranking.</td></tr></tfoot>
</table>

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
  <tr grouplength="5"><td colspan="3" style="border-bottom: 1px solid;"><strong>Aristocrat</strong></td></tr>
<tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Anonymous Employee </td>
   <td style="text-align:right;width: 2cm; "> 146 </td>
   <td style="text-align:right;width: 2cm; "> 48.7 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Senior Software Engineer </td>
   <td style="text-align:right;width: 2cm; "> 17 </td>
   <td style="text-align:right;width: 2cm; "> 5.7 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Software Engineer </td>
   <td style="text-align:right;width: 2cm; "> 10 </td>
   <td style="text-align:right;width: 2cm; "> 3.3 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Director </td>
   <td style="text-align:right;width: 2cm; "> 7 </td>
   <td style="text-align:right;width: 2cm; "> 2.3 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Team Lead </td>
   <td style="text-align:right;width: 2cm; "> 6 </td>
   <td style="text-align:right;width: 2cm; "> 2.0 </td>
  </tr>
  <tr grouplength="5"><td colspan="3" style="border-bottom: 1px solid;"><strong>IGT</strong></td></tr>
<tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Anonymous Employee </td>
   <td style="text-align:right;width: 2cm; "> 432 </td>
   <td style="text-align:right;width: 2cm; "> 49.3 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Software Engineer </td>
   <td style="text-align:right;width: 2cm; "> 46 </td>
   <td style="text-align:right;width: 2cm; "> 5.3 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Senior Software Engineer </td>
   <td style="text-align:right;width: 2cm; "> 16 </td>
   <td style="text-align:right;width: 2cm; "> 1.8 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Manager </td>
   <td style="text-align:right;width: 2cm; "> 14 </td>
   <td style="text-align:right;width: 2cm; "> 1.6 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Project Manager </td>
   <td style="text-align:right;width: 2cm; "> 14 </td>
   <td style="text-align:right;width: 2cm; "> 1.6 </td>
  </tr>
  <tr grouplength="5"><td colspan="3" style="border-bottom: 1px solid;"><strong>Scientific Games</strong></td></tr>
<tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Anonymous Employee </td>
   <td style="text-align:right;width: 2cm; "> 205 </td>
   <td style="text-align:right;width: 2cm; "> 54.7 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Software Engineer </td>
   <td style="text-align:right;width: 2cm; "> 23 </td>
   <td style="text-align:right;width: 2cm; "> 6.1 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Manager </td>
   <td style="text-align:right;width: 2cm; "> 8 </td>
   <td style="text-align:right;width: 2cm; "> 2.1 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Computer Operator </td>
   <td style="text-align:right;width: 2cm; "> 7 </td>
   <td style="text-align:right;width: 2cm; "> 1.9 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 5cm;  padding-left: 2em;" indentlevel="1"> Senior Software Engineer </td>
   <td style="text-align:right;width: 2cm; "> 7 </td>
   <td style="text-align:right;width: 2cm; "> 1.9 </td>
  </tr>
</tbody>
<tfoot><tr><td style="padding: 0; border: 0;" colspan="100%">
<sup></sup> Note: Top 5 based on percent ranking.</td></tr></tfoot>
</table>

=======

**PART 2: STAR RATINGS SECTION**

<table class="table table-striped table-hover table-condensed" style="font-size: 10px; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">Star Ratings</caption>
 <thead>
  <tr>
   <th style="text-align:center;"> Company </th>
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
   <td style="text-align:center;font-weight: bold;color: black;"> Aristocrat </td>
   <td style="text-align:center;"> 3.5 </td>
   <td style="text-align:center;"> 2.9 </td>
   <td style="text-align:center;"> 3.4 </td>
   <td style="text-align:center;"> 3.7 </td>
   <td style="text-align:center;"> 3.2 </td>
   <td style="text-align:center;font-weight: bold;"> 3.4 </td>
  </tr>
  <tr>
   <td style="text-align:center;font-weight: bold;color: black;"> IGT </td>
   <td style="text-align:center;"> 3.0 </td>
   <td style="text-align:center;"> 2.6 </td>
   <td style="text-align:center;"> 3.1 </td>
   <td style="text-align:center;"> 3.2 </td>
   <td style="text-align:center;"> 2.8 </td>
   <td style="text-align:center;font-weight: bold;"> 3.2 </td>
  </tr>
  <tr>
   <td style="text-align:center;font-weight: bold;color: black;"> Scientific Games </td>
   <td style="text-align:center;"> 3.0 </td>
   <td style="text-align:center;"> 2.5 </td>
   <td style="text-align:center;"> 3.1 </td>
   <td style="text-align:center;"> 3.0 </td>
   <td style="text-align:center;"> 2.6 </td>
   <td style="text-align:center;font-weight: bold;"> 3.1 </td>
  </tr>
</tbody>
</table>

=======

**PART 3: COMPANY REVIEWS SECTION**

**Pros**  

![1](https://raw.githubusercontent.com/mguideng/mguideng.github.io/master/img/2018-07-22_files/1unigram.prosplot-1.png)

![2](https://raw.githubusercontent.com/mguideng/mguideng.github.io/master/img/2018-07-22_files/2bigram.prosplot-1.png)

![3](https://raw.githubusercontent.com/mguideng/mguideng.github.io/master/img/2018-07-22_files/3barcompare.pros-1.png)

**Cons**  

![4](https://raw.githubusercontent.com/mguideng/mguideng.github.io/master/img/2018-07-22_files/4unigram.consplot-1.png)

![5](https://raw.githubusercontent.com/mguideng/mguideng.github.io/master/img/2018-07-22_files/5bigram.consplot-1.png)

![6](https://raw.githubusercontent.com/mguideng/mguideng.github.io/master/img/2018-07-22_files/6barcompare.cons-1.png)

**Sentiment Scoring of Pros & Cons**  

![7](https://raw.githubusercontent.com/mguideng/mguideng.github.io/master/img/2018-07-22_files/7hist-1.png)

<table class="table table-hover table-striped table-condensed" style="font-size: 10px; width: auto !important; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">Variability of Sentiment Scores</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Company </th>
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
   <td style="text-align:left;font-weight: bold;"> Aristocrat </td>
   <td style="text-align:right;"> -16 </td>
   <td style="text-align:right;"> -2 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> -0.9 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 9 </td>
  </tr>
  <tr>
   <td style="text-align:left;font-weight: bold;"> IGT </td>
   <td style="text-align:right;"> -33 </td>
   <td style="text-align:right;"> -2 </td>
   <td style="text-align:right;"> -1 </td>
   <td style="text-align:right;"> -1.3 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 6 </td>
  </tr>
  <tr>
   <td style="text-align:left;font-weight: bold;"> Scientific Games </td>
   <td style="text-align:right;"> -28 </td>
   <td style="text-align:right;"> -2 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> -1.2 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 10 </td>
  </tr>
</tbody>
</table>

![8](https://raw.githubusercontent.com/mguideng/mguideng.github.io/master/img/2018-07-22_files/8senscore.sentxtplot-1.png)

![9](https://raw.githubusercontent.com/mguideng/mguideng.github.io/master/img/2018-07-22_files/9senscore.sentxtplot.annual-1.png)

