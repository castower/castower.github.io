---
layout: page
title: Cinema and Clouds
subtitle: Courtney Stowers | March 9, 2020
---

<html>

<head>
<style type="text/css">
.inline {
  background-color: #f7f7f7;
  border:solid 1px #B0B0B0;
}
.error {
	font-weight: bold;
	color: #FF0000;
}
.warning {
	font-weight: bold;
}
.message {
	font-style: italic;
}
.source, .output, .warning, .error, .message {
	padding: 1em;
}
.source {
  background-color: #f5f5f5;
}
.left {
  text-align: left;
}
.right {
  text-align: right;
}
.center {
  text-align: center;
}
.hl.num {
  color: #AF0F91;
}
.hl.str {
  color: #317ECC;
}
.hl.com {
  color: #AD95AF;
  font-style: italic;
}
.hl.opt {
  color: #000000;
}
.hl.std {
  color: #585858;
}
.hl.kwa {
  color: #295F94;
  font-weight: bold;
}
.hl.kwb {
  color: #B05A65;
}
.hl.kwc {
  color: #55aa55;
}
.hl.kwd {
  color: #BC5A65;
  font-weight: bold;
}
</style>
<title>Cinema and Clouds</title>
</head>

<body>

<h1> Introduction </h1>

<p markdown="1"> Although data science projects often employ large amounts of numeric data, some projects examine patterns within text and require a different set of tools. In this code-through tutorial, we are going to explore several packages in R that enable researchers to analyze qualitative data sets and discover cool patterns. We are also going to create two word clouds based on movie plot summaries.

In order to complete this tutorial, you will need access to R or RStudio. If you are not familiar with either of these software or would like a refresher on the basics, check out the first steps of my previous code-through <a href="https://castower.github.io/2020-02-29-waffles/" target="_blank">here</a>. I walk you through the entire set-up process beginning with the software.

If you're all set to go with R and RStudio, then proceed below:  </p>

<hr>

# Set-Up

## Library of Packages

[Google](https://www.google.com/)

Before we begin analyzing and our data and creating word clouds, first, we need to load a few packages into our 'library'. Specifically we need the following packages:


<div class="chunk" id="unnamed-chunk-1"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">summary</span><span class="hl std">(cars)</span>
</pre></div>
<div class="output"><pre class="knitr r">##      speed           dist       
##  Min.   : 4.0   Min.   :  2.00  
##  1st Qu.:12.0   1st Qu.: 26.00  
##  Median :15.0   Median : 36.00  
##  Mean   :15.4   Mean   : 42.98  
##  3rd Qu.:19.0   3rd Qu.: 56.00  
##  Max.   :25.0   Max.   :120.00
</pre></div>
</div></div>

<p>You can also embed plots, for example:</p>

<div class="chunk" id="unnamed-chunk-2"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">plot</span><span class="hl std">(cars)</span>
</pre></div>
</div><div class="rimage default"><img src="figure/unnamed-chunk-2-1.png" title="plot of chunk unnamed-chunk-2" alt="plot of chunk unnamed-chunk-2" class="plot" /></div></div>

<div class="chunk" id="unnamed-chunk-3"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">plot</span><span class="hl std">(cars)</span>
</pre></div>
</div></div>


</body>
</html>
