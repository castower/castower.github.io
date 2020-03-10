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

<p> Although data science projects often employ large amounts of numeric data, some projects examine patterns within text and require a different set of tools. In this code-through tutorial, we are going to explore several packages in R that enable researchers to analyze qualitative data sets and discover cool patterns. We are also going to create two word clouds based on movie plot summaries.

In order to complete this tutorial, you will need access to R or RStudio. If you are not familiar with either of these software or would like a refresher on the basics, check out the first steps of my previous code-through <a href="https://castower.github.io/2020-02-29-waffles/" target="_blank">here</a>. I walk you through the entire set-up process beginning with the software.

If you're all set to go with R and RStudio, then proceed below:  </p>

<hr>

<h1> Set-Up </h1>

<h2> Library of Packages </h2>

<p> Before we begin analyzing and our data and creating word clouds, first, we need to load a few packages into our 'library'. Specifically we need the following packages: </p>


<div class="chunk" id="unnamed-chunk-1"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">library</span><span class="hl std">( dplyr )</span> <span class="hl com"># helps organize our data</span>
</pre></div>
<div class="message"><pre class="knitr r">## 
## Attaching package: 'dplyr'
</pre></div>
<div class="message"><pre class="knitr r">## The following objects are masked from 'package:stats':
## 
##     filter, lag
</pre></div>
<div class="message"><pre class="knitr r">## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl kwd">library</span><span class="hl std">( kableExtra )</span> <span class="hl com"># creates elegant tables for output</span>
</pre></div>
<div class="message"><pre class="knitr r">## 
## Attaching package: 'kableExtra'
</pre></div>
<div class="message"><pre class="knitr r">## The following object is masked from 'package:dplyr':
## 
##     group_rows
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl kwd">library</span><span class="hl std">( quanteda )</span> <span class="hl com"># processes our textual data for anaylsis</span>
</pre></div>
<div class="message"><pre class="knitr r">## Package version: 1.5.2
</pre></div>
<div class="message"><pre class="knitr r">## Parallel computing: 2 of 2 threads used.
</pre></div>
<div class="message"><pre class="knitr r">## See https://quanteda.io for tutorials and examples.
</pre></div>
<div class="message"><pre class="knitr r">## 
## Attaching package: 'quanteda'
</pre></div>
<div class="message"><pre class="knitr r">## The following object is masked from 'package:utils':
## 
##     View
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl kwd">library</span><span class="hl std">( wordcloud2 )</span> <span class="hl com"># creates the wordclouds</span>
</pre></div>
</div></div>

You can easily install all of these packages with the following code: 

`installed.packages("nameofpackage")`

<hr>

<h2> Data </h2>

<p markdown= "1">Now that we have our packages installed, the next thing that we need is our dataset. For the purposes of this code-through we are going to be using a dataset of over 5000 movies from IMDB that we will be access from [data.world](https://data.world/studentoflife/imdb-top-250-lists-and-5000-or-so-data-records): https://data.world/studentoflife/imdb-top-250-lists-and-5000-or-so-data-records </p>

![](/charts/pic1.png)

<p> We will click the blue `Explore this dataset` button and then scroll down to select the `IMDBdata_MainData.csv` dataset.

Next, we will click the download button where we will select the download URL for R. </p>

![](/charts/pic2.png)

<p>Data.World provides us with the exact code we need to get started and import our data. Therefore we will copy and paste into a codeblock as follows: </p>

<p>You can also embed plots, for example:</p>

<div class="chunk" id="unnamed-chunk-2"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">df</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">read.csv</span><span class="hl std">(</span><span class="hl str">&quot;https://query.data.world/s/rr46ndg7fyne54q7oonmvzxbaxg3zn&quot;</span><span class="hl std">,</span> <span class="hl kwc">header</span><span class="hl std">=</span><span class="hl num">TRUE</span><span class="hl std">,</span> <span class="hl kwc">stringsAsFactors</span><span class="hl std">=</span><span class="hl num">FALSE</span><span class="hl std">);</span>
</pre></div>
</div></div>

<hr>

<h1> Preview Dataset </h1>

<p> A quick peek at the column names of the dataset reveal the various fields available for us to use to explore the 5,000+ movies in the set. </p>

<div class="chunk" id="unnamed-chunk-3"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">colnames</span><span class="hl std">(df)</span>
</pre></div>
<div class="output"><pre class="knitr r">##  [1] &quot;Title&quot;          &quot;Year&quot;           &quot;Rated&quot;          &quot;Released&quot;      
##  [5] &quot;Runtime&quot;        &quot;Genre&quot;          &quot;Director&quot;       &quot;Writer&quot;        
##  [9] &quot;Actors&quot;         &quot;Plot&quot;           &quot;Language&quot;       &quot;Country&quot;       
## [13] &quot;Awards&quot;         &quot;Poster&quot;         &quot;Ratings.Source&quot; &quot;Ratings.Value&quot; 
## [17] &quot;Metascore&quot;      &quot;imdbRating&quot;     &quot;imdbVotes&quot;      &quot;imdbID&quot;        
## [21] &quot;Type&quot;           &quot;DVD&quot;            &quot;BoxOffice&quot;      &quot;Production&quot;    
## [25] &quot;Website&quot;        &quot;Response&quot;       &quot;tomatoURL&quot;
</pre></div>
</div></div>

<p> For ease of reference, we will change all of these titles to be lowercase with the following function: </p>

<div class="chunk" id="unnamed-chunk-4"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">colnames</span><span class="hl std">(df)</span><span class="hl kwb">&lt;-</span> <span class="hl kwd">tolower</span><span class="hl std">(</span><span class="hl kwd">colnames</span><span class="hl std">(df))</span>
<span class="hl kwd">colnames</span><span class="hl std">(df)</span>
</pre></div>
<div class="output"><pre class="knitr r">##  [1] &quot;title&quot;          &quot;year&quot;           &quot;rated&quot;          &quot;released&quot;      
##  [5] &quot;runtime&quot;        &quot;genre&quot;          &quot;director&quot;       &quot;writer&quot;        
##  [9] &quot;actors&quot;         &quot;plot&quot;           &quot;language&quot;       &quot;country&quot;       
## [13] &quot;awards&quot;         &quot;poster&quot;         &quot;ratings.source&quot; &quot;ratings.value&quot; 
## [17] &quot;metascore&quot;      &quot;imdbrating&quot;     &quot;imdbvotes&quot;      &quot;imdbid&quot;        
## [21] &quot;type&quot;           &quot;dvd&quot;            &quot;boxoffice&quot;      &quot;production&quot;    
## [25] &quot;website&quot;        &quot;response&quot;       &quot;tomatourl&quot;
</pre></div>
</div></div>

<p> For our analysis, we will focus specifically on the movie titles, genres, and plots, so we will create a smaller dataset with only these variables: </p>

<div class="chunk" id="unnamed-chunk-5"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">dat</span> <span class="hl kwb">&lt;-</span> <span class="hl std">df[</span><span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;title&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;genre&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;plot&quot;</span> <span class="hl std">)]</span>
</pre></div>
</div></div>

A preview of this dataset gives us a glimpse into what we're working with:

<div class="chunk" id="unnamed-chunk-6"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">head</span><span class="hl std">( dat )</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">kable</span><span class="hl std">()</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">kable_styling</span><span class="hl std">()</span>
</pre></div>
<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> title </th>
   <th style="text-align:left;"> genre </th>
   <th style="text-align:left;"> plot </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Code Name: K.O.Z. </td>
   <td style="text-align:left;"> Crime, Mystery </td>
   <td style="text-align:left;"> A look at the 17-25 December 2013 corruption scandal in Turkey, from the viewpoint of the Erdogan government. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saving Christmas </td>
   <td style="text-align:left;"> Comedy, Family </td>
   <td style="text-align:left;"> Kirk is enjoying the annual Christmas party extravaganza thrown by his sister until he realizes he needs to help out Christian, his brother-in-law, who has a bad case of the bah-humbugs. ... </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Superbabies: Baby Geniuses 2 </td>
   <td style="text-align:left;"> Comedy, Family, Sci-Fi </td>
   <td style="text-align:left;"> A group of smart-talking toddlers find themselves at the center of a media mogul's experiment to crack the code to baby talk. The toddlers must race against time for the sake of babies everywhere. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Daniel der Zauberer </td>
   <td style="text-align:left;"> Comedy, Crime, Fantasy </td>
   <td style="text-align:left;"> Evil assassins want to kill Daniel Kublbock, the third runner up for the German Idols. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Manos: The Hands of Fate </td>
   <td style="text-align:left;"> Horror </td>
   <td style="text-align:left;"> A family gets lost on the road and stumbles upon a hidden, underground, devil-worshiping cult led by the fearsome Master and his servant Torgo. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Pledge This! </td>
   <td style="text-align:left;"> Comedy </td>
   <td style="text-align:left;"> At South Beach University, a beautiful sorority president takes in a group of unconventional freshman girls seeking acceptance into her house. </td>
  </tr>
</tbody>
</table>

</div></div>


</body>
</html>
