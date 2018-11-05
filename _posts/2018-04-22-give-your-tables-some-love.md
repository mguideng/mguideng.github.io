---
layout: post
title: Looking to format a table in R?
subtitle: (No, probably not...) Well now you can!
bigimg: 
tags: [r-project, kableextra, location-quotients, nevada, las-vegas]
comments: true
---

R is not that great for final product data visualization, but if there was a simple way to give my plain tables a little love, I figured, 'Why not?' So I asked The Google, and it brought me to [this demo](https://haozhu233.github.io/kableExtra/awesome_table_in_html.html) by Hao Zhu, creator of the [kableExtra](https://CRAN.R-project.org/package=kableExtra) R package. The features to manipulate table styles seemed straightforward enough for a noob like me with no experience programming in CSS, so I decided to give it a run. The lucky table chosen for the makeover was from tabular data to calculate location quotients (LQs) in Las Vegas. The LQs quantified how concentrated a particular industry is in a region as compared to the nation. Specifically, I wanted to highlight the obvious about my table: gaming and hospitality drives the Vegas economy. It took a few iterations, but I figured out how to style everything the way I wanted (after augmentation with the `formattable` package). Check out the before and after tables below.

This is the basic HTML table in `kable`:
![](/img/before.PNG)


And here's the makeover after some formatting help from `kableExtra`.
![](/img/after.PNG)

=======

**Learning points**

* It's a nice bundle of functions. It's one that is easy to use and easy to understand; thus, it should be popular for those looking to keep the simplicity of the [Bootstrap](https://www.w3schools.com/bootstrap/bootstrap_tables.asp) table style while adding flexible customization. It's better suited for model-output documents and statistical result-type tables, rather than for fully leveraging on the power of visualization to help interpret the data.


<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
/*
var disqus_config = function () {
this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};
*/
(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://https-mguideng-github-io.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
