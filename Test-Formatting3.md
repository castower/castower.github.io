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

#toc_container {
    background: #f9f9f9 none repeat scroll 0 0;
    border: 1px solid #aaa;
    display: table;
    font-size: 95%;
    margin-bottom: 1em;
    padding: 20px;
    width: auto;
}

.toc_title {
    font-weight: 700;
    text-align: center;
}

.toc_list {
   text-align: center;
}

#toc_container li, #toc_container ul, #toc_container ul li{
    list-style: outside none none !important;
}
</style>
<title>Cinema and Clouds</title>
</head>

<center>
<div id="toc_container">
<p class="toc_title">Contents</p>
<p class="toc_list"><center><a href="#First_Point_Header">Introduction</a></center></p>
</div>
</center>


<body>

<h1 id="First_Point_Header"> Introduction </h1>

<img style="margin: 50px 50px" src="/img/keywords.jpg" width="225" height="151" alt="Keywords article pic" align="right"/><p> Although data science projects often employ large amounts of numeric data, some projects examine patterns within text and require a different set of tools. In this code-through tutorial, we are going to explore several packages in R that enable researchers to analyze qualitative data sets and discover cool patterns. We are also going to create two word clouds based on movie plot summaries.

In order to complete this tutorial, you will need access to R or RStudio. If you are not familiar with either of these software or would like a refresher on the basics, check out the first steps of my previous code-through <a href="https://castower.github.io/2020-02-29-waffles/" target="_blank">here</a>. I walk you through the entire set-up process beginning with the software.

If you're all set to go with R and RStudio, then proceed below:  </p>

<hr>

<h1> Set-Up </h1>

<h2> Library of Packages </h2>

<p> Before we begin analyzing and our data and creating word clouds, first, we need to load a few packages into our 'library'. Specifically we need the following packages: </p>


<div class="chunk" id="unnamed-chunk-2"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">library</span><span class="hl std">(dplyr)</span>  <span class="hl com"># helps organize our data</span>
<span class="hl kwd">library</span><span class="hl std">(kableExtra)</span>  <span class="hl com"># creates elegant tables for output</span>
<span class="hl kwd">library</span><span class="hl std">(quanteda)</span>  <span class="hl com"># processes our textual data for anaylsis</span>
<span class="hl kwd">library</span><span class="hl std">(wordcloud2)</span>  <span class="hl com"># creates the wordclouds</span>
</pre></div>
</div></div>

<p markdown="1"> You can easily install all of these packages with the following code: </p>

<p markdown="1"> `installed.packages("nameofpackage")` </p>

<hr>

<h2> Data </h2>

<p markdown="1">Now that we have our packages installed, the next thing that we need is our dataset. For the purposes of this code-through we are going to be using a dataset of over 5000 movies from IMDB that we will be access from [data.world](https://data.world/studentoflife/imdb-top-250-lists-and-5000-or-so-data-records).</p>

<p markdown="1">![](/charts/pic1.png)</p>

<p markdown="1">We will click the blue `Explore this dataset` button and then scroll down to select the `IMDBdata_MainData.csv` dataset.

Next, we will click the download button where we will select the download URL for R. </p>

<p markdown="1">![](/charts/pic2.png)</p>

<p>Data.World provides us with the exact code we need to get started and import our data. Therefore we will copy and paste into a codeblock as follows: </p>

<div class="chunk" id="unnamed-chunk-3"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">df</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">read.csv</span><span class="hl std">(</span><span class="hl str">&quot;https://query.data.world/s/rr46ndg7fyne54q7oonmvzxbaxg3zn&quot;</span><span class="hl std">,</span> <span class="hl kwc">header</span> <span class="hl std">=</span> <span class="hl num">TRUE</span><span class="hl std">,</span>
    <span class="hl kwc">stringsAsFactors</span> <span class="hl std">=</span> <span class="hl num">FALSE</span><span class="hl std">)</span>
</pre></div>
</div></div>

<hr>

<h1> Preview Dataset </h1>

<p> A quick peek at the column names of the dataset reveal the various fields available for us to use to explore the 5,000+ movies in the set. </p>

<div class="chunk" id="unnamed-chunk-4"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">colnames</span><span class="hl std">(df)</span>
</pre></div>
<div class="output"><pre class="knitr r"> [1] &quot;Title&quot;          &quot;Year&quot;           &quot;Rated&quot;          &quot;Released&quot;      
 [5] &quot;Runtime&quot;        &quot;Genre&quot;          &quot;Director&quot;       &quot;Writer&quot;        
 [9] &quot;Actors&quot;         &quot;Plot&quot;           &quot;Language&quot;       &quot;Country&quot;       
[13] &quot;Awards&quot;         &quot;Poster&quot;         &quot;Ratings.Source&quot; &quot;Ratings.Value&quot; 
[17] &quot;Metascore&quot;      &quot;imdbRating&quot;     &quot;imdbVotes&quot;      &quot;imdbID&quot;        
[21] &quot;Type&quot;           &quot;DVD&quot;            &quot;BoxOffice&quot;      &quot;Production&quot;    
[25] &quot;Website&quot;        &quot;Response&quot;       &quot;tomatoURL&quot;     
</pre></div>
</div></div>

<p> For ease of reference, we will change all of these titles to be lowercase with the following function: </p>

<div class="chunk" id="unnamed-chunk-5"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">colnames</span><span class="hl std">(df)</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">tolower</span><span class="hl std">(</span><span class="hl kwd">colnames</span><span class="hl std">(df))</span>
<span class="hl kwd">colnames</span><span class="hl std">(df)</span>
</pre></div>
<div class="output"><pre class="knitr r"> [1] &quot;title&quot;          &quot;year&quot;           &quot;rated&quot;          &quot;released&quot;      
 [5] &quot;runtime&quot;        &quot;genre&quot;          &quot;director&quot;       &quot;writer&quot;        
 [9] &quot;actors&quot;         &quot;plot&quot;           &quot;language&quot;       &quot;country&quot;       
[13] &quot;awards&quot;         &quot;poster&quot;         &quot;ratings.source&quot; &quot;ratings.value&quot; 
[17] &quot;metascore&quot;      &quot;imdbrating&quot;     &quot;imdbvotes&quot;      &quot;imdbid&quot;        
[21] &quot;type&quot;           &quot;dvd&quot;            &quot;boxoffice&quot;      &quot;production&quot;    
[25] &quot;website&quot;        &quot;response&quot;       &quot;tomatourl&quot;     
</pre></div>
</div></div>

<p> For our analysis, we will focus specifically on the movie titles, genres, and plots, so we will create a smaller dataset with only these variables: </p>

<div class="chunk" id="unnamed-chunk-6"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">dat</span> <span class="hl kwb">&lt;-</span> <span class="hl std">df[</span><span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;title&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;genre&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;plot&quot;</span><span class="hl std">)]</span>
</pre></div>
</div></div>

<p> A preview of this dataset gives us a glimpse into what we're working with: </p>

<div class="chunk" id="unnamed-chunk-7"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">head</span><span class="hl std">(dat)</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">kable</span><span class="hl std">()</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">kable_styling</span><span class="hl std">()</span>
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

<p markdown="1"> Now we will create an even smaller dataset of only romance movies using the `grep()` function which allows us to search through text for specfic key words. Here, we will search for all movies that have romance listed as either its only genre or at least one of its genres: </p>

<div class="chunk" id="unnamed-chunk-8"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">grep</span><span class="hl std">(</span><span class="hl kwc">pattern</span> <span class="hl std">=</span> <span class="hl str">&quot;romance&quot;</span><span class="hl std">,</span> <span class="hl kwc">x</span> <span class="hl std">= dat</span><span class="hl opt">$</span><span class="hl std">genre,</span> <span class="hl kwc">value</span> <span class="hl std">=</span> <span class="hl num">TRUE</span><span class="hl std">,</span> <span class="hl kwc">ignore.case</span> <span class="hl std">=</span> <span class="hl num">TRUE</span><span class="hl std">)</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">head</span><span class="hl std">()</span> <span class="hl opt">%&gt;%</span>
    <span class="hl kwd">kable</span><span class="hl std">()</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">kable_styling</span><span class="hl std">()</span>
</pre></div>
<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> x </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Horror, Romance, Thriller </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Comedy, Romance, Sport </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Comedy, Romance </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Comedy, Musical, Romance </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Drama, Romance, Thriller </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Drama, Music, Romance </td>
  </tr>
</tbody>
</table>

</div></div>

<p markdown="1"> Using the `grepl()` function we will find all 927 movies that belong to the romance genre: </p>

<div class="chunk" id="unnamed-chunk-9"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">romance</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">grepl</span><span class="hl std">(</span><span class="hl str">&quot;romance&quot;</span><span class="hl std">, dat</span><span class="hl opt">$</span><span class="hl std">genre,</span> <span class="hl kwc">ignore.case</span> <span class="hl std">= T)</span>
<span class="hl kwd">sum</span><span class="hl std">(romance)</span>
</pre></div>
<div class="output"><pre class="knitr r">[1] 927
</pre></div>
</div></div>

<p> We will use this criteria to segment out the romance movies: </p>

<div class="chunk" id="unnamed-chunk-10"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">dat.romance</span> <span class="hl kwb">&lt;-</span> <span class="hl std">dat[romance,</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;title&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;genre&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;plot&quot;</span><span class="hl std">)]</span>
<span class="hl std">dat.romance</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">head</span><span class="hl std">(</span><span class="hl num">10</span><span class="hl std">)</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">kable</span><span class="hl std">()</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">kable_styling</span><span class="hl std">()</span>
</pre></div>
<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:left;"> title </th>
   <th style="text-align:left;"> genre </th>
   <th style="text-align:left;"> plot </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 9 </td>
   <td style="text-align:left;"> Birdemic: Shock and Terror </td>
   <td style="text-align:left;"> Horror, Romance, Thriller </td>
   <td style="text-align:left;"> A horde of mutated birds descends upon the quiet town of Half Moon Bay, California. With the death toll rising, Two citizens manage to fight back, but will they survive Birdemic? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 10 </td>
   <td style="text-align:left;"> Dream.net </td>
   <td style="text-align:left;"> Comedy, Romance, Sport </td>
   <td style="text-align:left;"> Regina, the once popular girl has to make new friends at her new, conservative school. Problems arrive when she becomes enemies with Lívia, the school's queen bee, and falls in love with ... </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 14 </td>
   <td style="text-align:left;"> The Hottie &amp; the Nottie </td>
   <td style="text-align:left;"> Comedy, Romance </td>
   <td style="text-align:left;"> A woman agrees to go on a date with a man only if he finds a suitor for her unattractive best friend. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 16 </td>
   <td style="text-align:left;"> From Justin to Kelly </td>
   <td style="text-align:left;"> Comedy, Musical, Romance </td>
   <td style="text-align:left;"> A waitress from Texas and a college student from Pennsylvania meet during spring break in Fort Lauderdale, Florida and come together through their shared love of singing. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 23 </td>
   <td style="text-align:left;"> Ben &amp; Arthur </td>
   <td style="text-align:left;"> Drama, Romance, Thriller </td>
   <td style="text-align:left;"> A pair of recently married gay men are threatened by one of the partners' brother, a religious fanatic who plots to murder them after being ostracized by his church. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 32 </td>
   <td style="text-align:left;"> Glitter </td>
   <td style="text-align:left;"> Drama, Music, Romance </td>
   <td style="text-align:left;"> A young singer dates a disc jockey who helps her get into the music business, but their relationship become complicated as she ascends to super stardom. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 36 </td>
   <td style="text-align:left;"> Space Mutiny </td>
   <td style="text-align:left;"> Action, Adventure, Romance </td>
   <td style="text-align:left;"> A pilot is the only hope to stop the mutiny of a spacecraft by its security crew, who plot to sell the crew of the ship into slavery. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 51 </td>
   <td style="text-align:left;"> Gigli </td>
   <td style="text-align:left;"> Comedy, Crime, Romance </td>
   <td style="text-align:left;"> The violent story about how a criminal lesbian, a tough-guy hit-man with a heart of gold, and a mentally challenged man came to be best friends through a hostage. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 80 </td>
   <td style="text-align:left;"> A Story About Love </td>
   <td style="text-align:left;"> Romance </td>
   <td style="text-align:left;"> Two young people stand on a street corner in a run-down part of New York, kissing. Despite the lawlessness of the district they are left unmolested. A short distance away walk Maria and ... </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 97 </td>
   <td style="text-align:left;"> The Bat People </td>
   <td style="text-align:left;"> Horror, Romance </td>
   <td style="text-align:left;"> After being bitten by a bat in a cave, a doctor undergoes an accelerating transformation into a man-bat, which ruins his vacation and causes considerable distress for his wife. </td>
  </tr>
</tbody>
</table>

</div></div>

<p> Now that we have a collection of romance movie titles, we will focus on examining the plots of the movies to see if there are any similarities or common keywords across the summaries: </p>

<div class="chunk" id="unnamed-chunk-11"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">corp.romance</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">corpus</span><span class="hl std">(dat.romance,</span> <span class="hl kwc">docid_field</span> <span class="hl std">=</span> <span class="hl str">&quot;title&quot;</span><span class="hl std">,</span> <span class="hl kwc">text_field</span> <span class="hl std">=</span> <span class="hl str">&quot;plot&quot;</span><span class="hl std">)</span>
<span class="hl std">corp.romance</span>
</pre></div>
<div class="output"><pre class="knitr r">Corpus consisting of 927 documents and 1 docvar.
</pre></div>
</div></div>

<br>

<div class="chunk" id="unnamed-chunk-12"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">corp.romance[</span><span class="hl num">1</span><span class="hl opt">:</span><span class="hl num">5</span><span class="hl std">]</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">kable</span><span class="hl std">()</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">kable_styling</span><span class="hl std">()</span>
</pre></div>
<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:left;"> x </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Birdemic: Shock and Terror </td>
   <td style="text-align:left;"> A horde of mutated birds descends upon the quiet town of Half Moon Bay, California. With the death toll rising, Two citizens manage to fight back, but will they survive Birdemic? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dream.net </td>
   <td style="text-align:left;"> Regina, the once popular girl has to make new friends at her new, conservative school. Problems arrive when she becomes enemies with Lívia, the school's queen bee, and falls in love with ... </td>
  </tr>
  <tr>
   <td style="text-align:left;"> The Hottie &amp; the Nottie </td>
   <td style="text-align:left;"> A woman agrees to go on a date with a man only if he finds a suitor for her unattractive best friend. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> From Justin to Kelly </td>
   <td style="text-align:left;"> A waitress from Texas and a college student from Pennsylvania meet during spring break in Fort Lauderdale, Florida and come together through their shared love of singing. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ben &amp; Arthur </td>
   <td style="text-align:left;"> A pair of recently married gay men are threatened by one of the partners' brother, a religious fanatic who plots to murder them after being ostracized by his church. </td>
  </tr>
</tbody>
</table>

</div></div>

<br>

<div class="chunk" id="unnamed-chunk-13"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl com"># summarize corpus</span>
<span class="hl kwd">summary</span><span class="hl std">(corp.romance)[</span><span class="hl num">1</span><span class="hl opt">:</span><span class="hl num">10</span><span class="hl std">, ]</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">kable</span><span class="hl std">()</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">kable_styling</span><span class="hl std">()</span>
</pre></div>
<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Text </th>
   <th style="text-align:right;"> Types </th>
   <th style="text-align:right;"> Tokens </th>
   <th style="text-align:right;"> Sentences </th>
   <th style="text-align:left;"> genre </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Birdemic: Shock and Terror </td>
   <td style="text-align:right;"> 32 </td>
   <td style="text-align:right;"> 36 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:left;"> Horror, Romance, Thriller </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dream.net </td>
   <td style="text-align:right;"> 31 </td>
   <td style="text-align:right;"> 40 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:left;"> Comedy, Romance, Sport </td>
  </tr>
  <tr>
   <td style="text-align:left;"> The Hottie &amp; the Nottie </td>
   <td style="text-align:right;"> 21 </td>
   <td style="text-align:right;"> 23 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:left;"> Comedy, Romance </td>
  </tr>
  <tr>
   <td style="text-align:left;"> From Justin to Kelly </td>
   <td style="text-align:right;"> 27 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:left;"> Comedy, Musical, Romance </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ben &amp; Arthur </td>
   <td style="text-align:right;"> 30 </td>
   <td style="text-align:right;"> 32 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:left;"> Drama, Romance, Thriller </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Glitter </td>
   <td style="text-align:right;"> 28 </td>
   <td style="text-align:right;"> 28 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:left;"> Drama, Music, Romance </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Space Mutiny </td>
   <td style="text-align:right;"> 24 </td>
   <td style="text-align:right;"> 30 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:left;"> Action, Adventure, Romance </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Gigli </td>
   <td style="text-align:right;"> 27 </td>
   <td style="text-align:right;"> 32 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:left;"> Comedy, Crime, Romance </td>
  </tr>
  <tr>
   <td style="text-align:left;"> A Story About Love </td>
   <td style="text-align:right;"> 32 </td>
   <td style="text-align:right;"> 39 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:left;"> Romance </td>
  </tr>
  <tr>
   <td style="text-align:left;"> The Bat People </td>
   <td style="text-align:right;"> 27 </td>
   <td style="text-align:right;"> 32 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:left;"> Horror, Romance </td>
  </tr>
</tbody>
</table>

</div></div>

<br>

<div class="chunk" id="unnamed-chunk-14"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl com"># Process text</span>

<span class="hl com"># remove mission statements that are less than 1 sentence long</span>
<span class="hl std">corp.romance</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">corpus_trim</span><span class="hl std">(corp.romance,</span> <span class="hl kwc">what</span> <span class="hl std">=</span> <span class="hl str">&quot;sentences&quot;</span><span class="hl std">,</span> <span class="hl kwc">min_ntoken</span> <span class="hl std">=</span> <span class="hl num">1</span><span class="hl std">)</span>
<span class="hl std">corp.romance</span>
</pre></div>
<div class="output"><pre class="knitr r">Corpus consisting of 927 documents and 1 docvar.
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl com"># remove punctuation</span>
<span class="hl std">tokens.romance</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">tokens</span><span class="hl std">(corp.romance,</span> <span class="hl kwc">what</span> <span class="hl std">=</span> <span class="hl str">&quot;word&quot;</span><span class="hl std">,</span> <span class="hl kwc">remove_punct</span> <span class="hl std">=</span> <span class="hl num">TRUE</span><span class="hl std">)</span>
<span class="hl kwd">head</span><span class="hl std">(tokens.romance)</span>
</pre></div>
<div class="output"><pre class="knitr r">tokens from 6 documents.
Birdemic: Shock and Terror :
 [1] &quot;A&quot;          &quot;horde&quot;      &quot;of&quot;         &quot;mutated&quot;    &quot;birds&quot;     
 [6] &quot;descends&quot;   &quot;upon&quot;       &quot;the&quot;        &quot;quiet&quot;      &quot;town&quot;      
[11] &quot;of&quot;         &quot;Half&quot;       &quot;Moon&quot;       &quot;Bay&quot;        &quot;California&quot;
[16] &quot;With&quot;       &quot;the&quot;        &quot;death&quot;      &quot;toll&quot;       &quot;rising&quot;    
[21] &quot;Two&quot;        &quot;citizens&quot;   &quot;manage&quot;     &quot;to&quot;         &quot;fight&quot;     
[26] &quot;back&quot;       &quot;but&quot;        &quot;will&quot;       &quot;they&quot;       &quot;survive&quot;   
[31] &quot;Birdemic&quot;  

Dream.net :
 [1] &quot;Regina&quot;       &quot;the&quot;          &quot;once&quot;         &quot;popular&quot;      &quot;girl&quot;        
 [6] &quot;has&quot;          &quot;to&quot;           &quot;make&quot;         &quot;new&quot;          &quot;friends&quot;     
[11] &quot;at&quot;           &quot;her&quot;          &quot;new&quot;          &quot;conservative&quot; &quot;school&quot;      
[16] &quot;Problems&quot;     &quot;arrive&quot;       &quot;when&quot;         &quot;she&quot;          &quot;becomes&quot;     
[21] &quot;enemies&quot;      &quot;with&quot;         &quot;Lívia&quot;        &quot;the&quot;          &quot;school's&quot;    
[26] &quot;queen&quot;        &quot;bee&quot;          &quot;and&quot;          &quot;falls&quot;        &quot;in&quot;          
[31] &quot;love&quot;         &quot;with&quot;        

The Hottie &amp; the Nottie :
 [1] &quot;A&quot;            &quot;woman&quot;        &quot;agrees&quot;       &quot;to&quot;           &quot;go&quot;          
 [6] &quot;on&quot;           &quot;a&quot;            &quot;date&quot;         &quot;with&quot;         &quot;a&quot;           
[11] &quot;man&quot;          &quot;only&quot;         &quot;if&quot;           &quot;he&quot;           &quot;finds&quot;       
[16] &quot;a&quot;            &quot;suitor&quot;       &quot;for&quot;          &quot;her&quot;          &quot;unattractive&quot;
[21] &quot;best&quot;         &quot;friend&quot;      

From Justin to Kelly :
 [1] &quot;A&quot;            &quot;waitress&quot;     &quot;from&quot;         &quot;Texas&quot;        &quot;and&quot;         
 [6] &quot;a&quot;            &quot;college&quot;      &quot;student&quot;      &quot;from&quot;         &quot;Pennsylvania&quot;
[11] &quot;meet&quot;         &quot;during&quot;       &quot;spring&quot;       &quot;break&quot;        &quot;in&quot;          
[16] &quot;Fort&quot;         &quot;Lauderdale&quot;   &quot;Florida&quot;      &quot;and&quot;          &quot;come&quot;        
[21] &quot;together&quot;     &quot;through&quot;      &quot;their&quot;        &quot;shared&quot;       &quot;love&quot;        
[26] &quot;of&quot;           &quot;singing&quot;     

Ben &amp; Arthur :
 [1] &quot;A&quot;          &quot;pair&quot;       &quot;of&quot;         &quot;recently&quot;   &quot;married&quot;   
 [6] &quot;gay&quot;        &quot;men&quot;        &quot;are&quot;        &quot;threatened&quot; &quot;by&quot;        
[11] &quot;one&quot;        &quot;of&quot;         &quot;the&quot;        &quot;partners&quot;   &quot;brother&quot;   
[16] &quot;a&quot;          &quot;religious&quot;  &quot;fanatic&quot;    &quot;who&quot;        &quot;plots&quot;     
[21] &quot;to&quot;         &quot;murder&quot;     &quot;them&quot;       &quot;after&quot;      &quot;being&quot;     
[26] &quot;ostracized&quot; &quot;by&quot;         &quot;his&quot;        &quot;church&quot;    

Glitter :
 [1] &quot;A&quot;            &quot;young&quot;        &quot;singer&quot;       &quot;dates&quot;        &quot;a&quot;           
 [6] &quot;disc&quot;         &quot;jockey&quot;       &quot;who&quot;          &quot;helps&quot;        &quot;her&quot;         
[11] &quot;get&quot;          &quot;into&quot;         &quot;the&quot;          &quot;music&quot;        &quot;business&quot;    
[16] &quot;but&quot;          &quot;their&quot;        &quot;relationship&quot; &quot;become&quot;       &quot;complicated&quot; 
[21] &quot;as&quot;           &quot;she&quot;          &quot;ascends&quot;      &quot;to&quot;           &quot;super&quot;       
[26] &quot;stardom&quot;     
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl com"># convert to lower case</span>
<span class="hl std">tokens.romance</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">tokens_tolower</span><span class="hl std">(tokens.romance,</span> <span class="hl kwc">keep_acronyms</span> <span class="hl std">=</span> <span class="hl num">TRUE</span><span class="hl std">)</span>
<span class="hl kwd">head</span><span class="hl std">(tokens.romance)</span>
</pre></div>
<div class="output"><pre class="knitr r">tokens from 6 documents.
Birdemic: Shock and Terror :
 [1] &quot;a&quot;          &quot;horde&quot;      &quot;of&quot;         &quot;mutated&quot;    &quot;birds&quot;     
 [6] &quot;descends&quot;   &quot;upon&quot;       &quot;the&quot;        &quot;quiet&quot;      &quot;town&quot;      
[11] &quot;of&quot;         &quot;half&quot;       &quot;moon&quot;       &quot;bay&quot;        &quot;california&quot;
[16] &quot;with&quot;       &quot;the&quot;        &quot;death&quot;      &quot;toll&quot;       &quot;rising&quot;    
[21] &quot;two&quot;        &quot;citizens&quot;   &quot;manage&quot;     &quot;to&quot;         &quot;fight&quot;     
[26] &quot;back&quot;       &quot;but&quot;        &quot;will&quot;       &quot;they&quot;       &quot;survive&quot;   
[31] &quot;birdemic&quot;  

Dream.net :
 [1] &quot;regina&quot;       &quot;the&quot;          &quot;once&quot;         &quot;popular&quot;      &quot;girl&quot;        
 [6] &quot;has&quot;          &quot;to&quot;           &quot;make&quot;         &quot;new&quot;          &quot;friends&quot;     
[11] &quot;at&quot;           &quot;her&quot;          &quot;new&quot;          &quot;conservative&quot; &quot;school&quot;      
[16] &quot;problems&quot;     &quot;arrive&quot;       &quot;when&quot;         &quot;she&quot;          &quot;becomes&quot;     
[21] &quot;enemies&quot;      &quot;with&quot;         &quot;lívia&quot;        &quot;the&quot;          &quot;school's&quot;    
[26] &quot;queen&quot;        &quot;bee&quot;          &quot;and&quot;          &quot;falls&quot;        &quot;in&quot;          
[31] &quot;love&quot;         &quot;with&quot;        

The Hottie &amp; the Nottie :
 [1] &quot;a&quot;            &quot;woman&quot;        &quot;agrees&quot;       &quot;to&quot;           &quot;go&quot;          
 [6] &quot;on&quot;           &quot;a&quot;            &quot;date&quot;         &quot;with&quot;         &quot;a&quot;           
[11] &quot;man&quot;          &quot;only&quot;         &quot;if&quot;           &quot;he&quot;           &quot;finds&quot;       
[16] &quot;a&quot;            &quot;suitor&quot;       &quot;for&quot;          &quot;her&quot;          &quot;unattractive&quot;
[21] &quot;best&quot;         &quot;friend&quot;      

From Justin to Kelly :
 [1] &quot;a&quot;            &quot;waitress&quot;     &quot;from&quot;         &quot;texas&quot;        &quot;and&quot;         
 [6] &quot;a&quot;            &quot;college&quot;      &quot;student&quot;      &quot;from&quot;         &quot;pennsylvania&quot;
[11] &quot;meet&quot;         &quot;during&quot;       &quot;spring&quot;       &quot;break&quot;        &quot;in&quot;          
[16] &quot;fort&quot;         &quot;lauderdale&quot;   &quot;florida&quot;      &quot;and&quot;          &quot;come&quot;        
[21] &quot;together&quot;     &quot;through&quot;      &quot;their&quot;        &quot;shared&quot;       &quot;love&quot;        
[26] &quot;of&quot;           &quot;singing&quot;     

Ben &amp; Arthur :
 [1] &quot;a&quot;          &quot;pair&quot;       &quot;of&quot;         &quot;recently&quot;   &quot;married&quot;   
 [6] &quot;gay&quot;        &quot;men&quot;        &quot;are&quot;        &quot;threatened&quot; &quot;by&quot;        
[11] &quot;one&quot;        &quot;of&quot;         &quot;the&quot;        &quot;partners&quot;   &quot;brother&quot;   
[16] &quot;a&quot;          &quot;religious&quot;  &quot;fanatic&quot;    &quot;who&quot;        &quot;plots&quot;     
[21] &quot;to&quot;         &quot;murder&quot;     &quot;them&quot;       &quot;after&quot;      &quot;being&quot;     
[26] &quot;ostracized&quot; &quot;by&quot;         &quot;his&quot;        &quot;church&quot;    

Glitter :
 [1] &quot;a&quot;            &quot;young&quot;        &quot;singer&quot;       &quot;dates&quot;        &quot;a&quot;           
 [6] &quot;disc&quot;         &quot;jockey&quot;       &quot;who&quot;          &quot;helps&quot;        &quot;her&quot;         
[11] &quot;get&quot;          &quot;into&quot;         &quot;the&quot;          &quot;music&quot;        &quot;business&quot;    
[16] &quot;but&quot;          &quot;their&quot;        &quot;relationship&quot; &quot;become&quot;       &quot;complicated&quot; 
[21] &quot;as&quot;           &quot;she&quot;          &quot;ascends&quot;      &quot;to&quot;           &quot;super&quot;       
[26] &quot;stardom&quot;     
</pre></div>
</div></div>

<p> In order to make the data set more precise, we will remove all of the articles from the data set: </p>

<div class="chunk" id="unnamed-chunk-15"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">tokens.romance</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">tokens_remove</span><span class="hl std">(tokens.romance,</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl kwd">stopwords</span><span class="hl std">(</span><span class="hl str">&quot;english&quot;</span><span class="hl std">),</span> <span class="hl str">&quot;nbsp&quot;</span><span class="hl std">),</span>
    <span class="hl kwc">padding</span> <span class="hl std">= F)</span>
<span class="hl kwd">head</span><span class="hl std">(tokens.romance)</span>
</pre></div>
<div class="output"><pre class="knitr r">tokens from 6 documents.
Birdemic: Shock and Terror :
 [1] &quot;horde&quot;      &quot;mutated&quot;    &quot;birds&quot;      &quot;descends&quot;   &quot;upon&quot;      
 [6] &quot;quiet&quot;      &quot;town&quot;       &quot;half&quot;       &quot;moon&quot;       &quot;bay&quot;       
[11] &quot;california&quot; &quot;death&quot;      &quot;toll&quot;       &quot;rising&quot;     &quot;two&quot;       
[16] &quot;citizens&quot;   &quot;manage&quot;     &quot;fight&quot;      &quot;back&quot;       &quot;survive&quot;   
[21] &quot;birdemic&quot;  

Dream.net :
 [1] &quot;regina&quot;       &quot;popular&quot;      &quot;girl&quot;         &quot;make&quot;         &quot;new&quot;         
 [6] &quot;friends&quot;      &quot;new&quot;          &quot;conservative&quot; &quot;school&quot;       &quot;problems&quot;    
[11] &quot;arrive&quot;       &quot;becomes&quot;      &quot;enemies&quot;      &quot;lívia&quot;        &quot;school's&quot;    
[16] &quot;queen&quot;        &quot;bee&quot;          &quot;falls&quot;        &quot;love&quot;        

The Hottie &amp; the Nottie :
 [1] &quot;woman&quot;        &quot;agrees&quot;       &quot;go&quot;           &quot;date&quot;         &quot;man&quot;         
 [6] &quot;finds&quot;        &quot;suitor&quot;       &quot;unattractive&quot; &quot;best&quot;         &quot;friend&quot;      

From Justin to Kelly :
 [1] &quot;waitress&quot;     &quot;texas&quot;        &quot;college&quot;      &quot;student&quot;      &quot;pennsylvania&quot;
 [6] &quot;meet&quot;         &quot;spring&quot;       &quot;break&quot;        &quot;fort&quot;         &quot;lauderdale&quot;  
[11] &quot;florida&quot;      &quot;come&quot;         &quot;together&quot;     &quot;shared&quot;       &quot;love&quot;        
[16] &quot;singing&quot;     

Ben &amp; Arthur :
 [1] &quot;pair&quot;       &quot;recently&quot;   &quot;married&quot;    &quot;gay&quot;        &quot;men&quot;       
 [6] &quot;threatened&quot; &quot;one&quot;        &quot;partners&quot;   &quot;brother&quot;    &quot;religious&quot; 
[11] &quot;fanatic&quot;    &quot;plots&quot;      &quot;murder&quot;     &quot;ostracized&quot; &quot;church&quot;    

Glitter :
 [1] &quot;young&quot;        &quot;singer&quot;       &quot;dates&quot;        &quot;disc&quot;         &quot;jockey&quot;      
 [6] &quot;helps&quot;        &quot;get&quot;          &quot;music&quot;        &quot;business&quot;     &quot;relationship&quot;
[11] &quot;become&quot;       &quot;complicated&quot;  &quot;ascends&quot;      &quot;super&quot;        &quot;stardom&quot;     
</pre></div>
</div></div>

<p> In order to consolidate similar words, we will stem the data set: </p>

<div class="chunk" id="unnamed-chunk-16"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl com"># stem the words in the token list:</span>
<span class="hl std">tokens.romance</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">tokens_wordstem</span><span class="hl std">(tokens.romance)</span>
<span class="hl kwd">head</span><span class="hl std">(tokens.romance)</span>
</pre></div>
<div class="output"><pre class="knitr r">tokens from 6 documents.
Birdemic: Shock and Terror :
 [1] &quot;hord&quot;       &quot;mutat&quot;      &quot;bird&quot;       &quot;descend&quot;    &quot;upon&quot;      
 [6] &quot;quiet&quot;      &quot;town&quot;       &quot;half&quot;       &quot;moon&quot;       &quot;bay&quot;       
[11] &quot;california&quot; &quot;death&quot;      &quot;toll&quot;       &quot;rise&quot;       &quot;two&quot;       
[16] &quot;citizen&quot;    &quot;manag&quot;      &quot;fight&quot;      &quot;back&quot;       &quot;surviv&quot;    
[21] &quot;birdem&quot;    

Dream.net :
 [1] &quot;regina&quot;  &quot;popular&quot; &quot;girl&quot;    &quot;make&quot;    &quot;new&quot;     &quot;friend&quot;  &quot;new&quot;    
 [8] &quot;conserv&quot; &quot;school&quot;  &quot;problem&quot; &quot;arriv&quot;   &quot;becom&quot;   &quot;enemi&quot;   &quot;lívia&quot;  
[15] &quot;school&quot;  &quot;queen&quot;   &quot;bee&quot;     &quot;fall&quot;    &quot;love&quot;   

The Hottie &amp; the Nottie :
 [1] &quot;woman&quot;     &quot;agre&quot;      &quot;go&quot;        &quot;date&quot;      &quot;man&quot;       &quot;find&quot;     
 [7] &quot;suitor&quot;    &quot;unattract&quot; &quot;best&quot;      &quot;friend&quot;   

From Justin to Kelly :
 [1] &quot;waitress&quot;     &quot;texa&quot;         &quot;colleg&quot;       &quot;student&quot;      &quot;pennsylvania&quot;
 [6] &quot;meet&quot;         &quot;spring&quot;       &quot;break&quot;        &quot;fort&quot;         &quot;lauderdal&quot;   
[11] &quot;florida&quot;      &quot;come&quot;         &quot;togeth&quot;       &quot;share&quot;        &quot;love&quot;        
[16] &quot;sing&quot;        

Ben &amp; Arthur :
 [1] &quot;pair&quot;     &quot;recent&quot;   &quot;marri&quot;    &quot;gay&quot;      &quot;men&quot;      &quot;threaten&quot;
 [7] &quot;one&quot;      &quot;partner&quot;  &quot;brother&quot;  &quot;religi&quot;   &quot;fanat&quot;    &quot;plot&quot;    
[13] &quot;murder&quot;   &quot;ostrac&quot;   &quot;church&quot;  

Glitter :
 [1] &quot;young&quot;        &quot;singer&quot;       &quot;date&quot;         &quot;disc&quot;         &quot;jockey&quot;      
 [6] &quot;help&quot;         &quot;get&quot;          &quot;music&quot;        &quot;busi&quot;         &quot;relationship&quot;
[11] &quot;becom&quot;        &quot;complic&quot;      &quot;ascend&quot;       &quot;super&quot;        &quot;stardom&quot;     
</pre></div>
</div></div>

<p> We can see which words commonly occur together: </p>

<div class="chunk" id="unnamed-chunk-17"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl com"># find frequently co-occuring words (typically compound words)</span>
<span class="hl std">ngram.romance</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">tokens_ngrams</span><span class="hl std">(tokens.romance,</span> <span class="hl kwc">n</span> <span class="hl std">=</span> <span class="hl num">2</span><span class="hl std">)</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">dfm</span><span class="hl std">()</span>
<span class="hl std">ngram.romance</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">textstat_frequency</span><span class="hl std">(</span><span class="hl kwc">n</span> <span class="hl std">=</span> <span class="hl num">10</span><span class="hl std">)</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">kable</span><span class="hl std">()</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">kable_styling</span><span class="hl std">()</span>
</pre></div>
<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> feature </th>
   <th style="text-align:right;"> frequency </th>
   <th style="text-align:right;"> rank </th>
   <th style="text-align:right;"> docfreq </th>
   <th style="text-align:left;"> group </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> fall_love </td>
   <td style="text-align:right;"> 57 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 57 </td>
   <td style="text-align:left;"> all </td>
  </tr>
  <tr>
   <td style="text-align:left;"> new_york </td>
   <td style="text-align:right;"> 37 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 36 </td>
   <td style="text-align:left;"> all </td>
  </tr>
  <tr>
   <td style="text-align:left;"> young_man </td>
   <td style="text-align:right;"> 32 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 32 </td>
   <td style="text-align:left;"> all </td>
  </tr>
  <tr>
   <td style="text-align:left;"> young_woman </td>
   <td style="text-align:right;"> 28 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 28 </td>
   <td style="text-align:left;"> all </td>
  </tr>
  <tr>
   <td style="text-align:left;"> high_school </td>
   <td style="text-align:right;"> 28 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 27 </td>
   <td style="text-align:left;"> all </td>
  </tr>
  <tr>
   <td style="text-align:left;"> best_friend </td>
   <td style="text-align:right;"> 27 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 26 </td>
   <td style="text-align:left;"> all </td>
  </tr>
  <tr>
   <td style="text-align:left;"> world_war </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:left;"> all </td>
  </tr>
  <tr>
   <td style="text-align:left;"> york_citi </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:left;"> all </td>
  </tr>
  <tr>
   <td style="text-align:left;"> true_love </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:left;"> all </td>
  </tr>
  <tr>
   <td style="text-align:left;"> war_ii </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:left;"> all </td>
  </tr>
</tbody>
</table>

</div></div>

<br>

<div class="chunk" id="unnamed-chunk-18"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">ngram.romance3</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">tokens_ngrams</span><span class="hl std">(tokens.romance,</span> <span class="hl kwc">n</span> <span class="hl std">=</span> <span class="hl num">3</span><span class="hl std">)</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">dfm</span><span class="hl std">()</span>
<span class="hl std">ngram.romance3</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">textstat_frequency</span><span class="hl std">(</span><span class="hl kwc">n</span> <span class="hl std">=</span> <span class="hl num">10</span><span class="hl std">)</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">kable</span><span class="hl std">()</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">kable_styling</span><span class="hl std">()</span>
</pre></div>
<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> feature </th>
   <th style="text-align:right;"> frequency </th>
   <th style="text-align:right;"> rank </th>
   <th style="text-align:right;"> docfreq </th>
   <th style="text-align:left;"> group </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> new_york_citi </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:left;"> all </td>
  </tr>
  <tr>
   <td style="text-align:left;"> world_war_ii </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:left;"> all </td>
  </tr>
  <tr>
   <td style="text-align:left;"> two_best_friend </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:left;"> all </td>
  </tr>
  <tr>
   <td style="text-align:left;"> woman_fall_love </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:left;"> all </td>
  </tr>
  <tr>
   <td style="text-align:left;"> fall_love_woman </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:left;"> all </td>
  </tr>
  <tr>
   <td style="text-align:left;"> meet_fall_love </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:left;"> all </td>
  </tr>
  <tr>
   <td style="text-align:left;"> drama_center_around </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:left;"> all </td>
  </tr>
  <tr>
   <td style="text-align:left;"> high_school_crush </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:left;"> all </td>
  </tr>
  <tr>
   <td style="text-align:left;"> young_woman_find </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:left;"> all </td>
  </tr>
  <tr>
   <td style="text-align:left;"> experi_chang_live </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:left;"> all </td>
  </tr>
</tbody>
</table>

</div></div>

<p> Finally we can see which words are most common in the romance movie plot summaries: </p>

<div class="chunk" id="unnamed-chunk-19"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">tokens.romance</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">dfm</span><span class="hl std">(</span><span class="hl kwc">stem</span> <span class="hl std">= T)</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">topfeatures</span><span class="hl std">()</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">kable</span><span class="hl std">()</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">kable_styling</span><span class="hl std">()</span>
</pre></div>
<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:right;"> x </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> love </td>
   <td style="text-align:right;"> 182 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> young </td>
   <td style="text-align:right;"> 146 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> woman </td>
   <td style="text-align:right;"> 144 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> man </td>
   <td style="text-align:right;"> 138 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> life </td>
   <td style="text-align:right;"> 112 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> fall </td>
   <td style="text-align:right;"> 93 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> find </td>
   <td style="text-align:right;"> 93 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> friend </td>
   <td style="text-align:right;"> 91 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> two </td>
   <td style="text-align:right;"> 90 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> new </td>
   <td style="text-align:right;"> 88 </td>
  </tr>
</tbody>
</table>

</div></div>

<p> Unsurprisingly(!!) love is the number one most frequently used word followed by young, woman, and man.</p>

<hr>

<h1> Word Clouds </h1>

<p markdown="1">Now that we've previewed our data step-by-step, we can create a wordcloud. We will repeat some of our previous steps in a more automated format using the `tm()` package. If you do not have it installed, once again you can install it via `installed.packages("tm")` </p>

<div class="chunk" id="unnamed-chunk-20"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">library</span><span class="hl std">(tm)</span>  <span class="hl com"># package for processing text</span>
</pre></div>
</div></div>

<hr>

<h2> Romance Movies </h2>

<p> For our first word cloud, we will create a new dataset creating only our plots from romance movies: </p>

<div class="chunk" id="unnamed-chunk-21"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl com"># Create a vector containing only the text</span>
<span class="hl std">romance.plot</span> <span class="hl kwb">&lt;-</span> <span class="hl std">dat.romance</span><span class="hl opt">$</span><span class="hl std">plot</span>
</pre></div>
</div></div>

<p> Then we will create a new Corpus consisting solely of the plot: </p>

<div class="chunk" id="unnamed-chunk-22"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl com"># Create a corpus</span>
<span class="hl std">romance.docs</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">Corpus</span><span class="hl std">(</span><span class="hl kwd">VectorSource</span><span class="hl std">(romance.plot))</span>
<span class="hl std">romance.docs</span>
</pre></div>
<div class="output"><pre class="knitr r">&lt;&lt;SimpleCorpus&gt;&gt;
Metadata:  corpus specific: 1, document level (indexed): 0
Content:  documents: 927
</pre></div>
</div></div>

<p> Afterwords, we will go through the same steps that we did above to clean our data set: </p>

<div class="chunk" id="unnamed-chunk-23"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">romance.docs</span> <span class="hl kwb">&lt;-</span> <span class="hl std">romance.docs</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">tm_map</span><span class="hl std">(removeNumbers)</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">tm_map</span><span class="hl std">(removePunctuation)</span> <span class="hl opt">%&gt;%</span>
    <span class="hl kwd">tm_map</span><span class="hl std">(stripWhitespace)</span>
<span class="hl std">romance.docs</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">tm_map</span><span class="hl std">(romance.docs,</span> <span class="hl kwd">content_transformer</span><span class="hl std">(tolower))</span>
<span class="hl std">romance.docs</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">tm_map</span><span class="hl std">(romance.docs, removeWords,</span> <span class="hl kwd">stopwords</span><span class="hl std">(</span><span class="hl str">&quot;english&quot;</span><span class="hl std">))</span>
</pre></div>
</div></div>

<p> And once again find our most frequently used words: </p>

<div class="chunk" id="unnamed-chunk-24"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">romance.dtm</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">TermDocumentMatrix</span><span class="hl std">(romance.docs)</span>
<span class="hl std">romance.matrix</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">as.matrix</span><span class="hl std">(romance.dtm)</span>
<span class="hl std">romance.words</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">sort</span><span class="hl std">(</span><span class="hl kwd">rowSums</span><span class="hl std">(romance.matrix),</span> <span class="hl kwc">decreasing</span> <span class="hl std">=</span> <span class="hl num">TRUE</span><span class="hl std">)</span>
<span class="hl std">romance.df</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">data.frame</span><span class="hl std">(</span><span class="hl kwc">word</span> <span class="hl std">=</span> <span class="hl kwd">names</span><span class="hl std">(romance.words),</span> <span class="hl kwc">freq</span> <span class="hl std">= romance.words)</span>
<span class="hl std">romance.df</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">head</span><span class="hl std">(</span><span class="hl num">10</span><span class="hl std">)</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">kable</span><span class="hl std">()</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">kable_styling</span><span class="hl std">()</span>
</pre></div>
<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:left;"> word </th>
   <th style="text-align:right;"> freq </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> love </td>
   <td style="text-align:left;"> love </td>
   <td style="text-align:right;"> 174 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> young </td>
   <td style="text-align:left;"> young </td>
   <td style="text-align:right;"> 146 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> woman </td>
   <td style="text-align:left;"> woman </td>
   <td style="text-align:right;"> 133 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> man </td>
   <td style="text-align:left;"> man </td>
   <td style="text-align:right;"> 132 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> life </td>
   <td style="text-align:left;"> life </td>
   <td style="text-align:right;"> 110 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> two </td>
   <td style="text-align:left;"> two </td>
   <td style="text-align:right;"> 90 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> new </td>
   <td style="text-align:left;"> new </td>
   <td style="text-align:right;"> 88 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> friends </td>
   <td style="text-align:left;"> friends </td>
   <td style="text-align:right;"> 62 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> one </td>
   <td style="text-align:left;"> one </td>
   <td style="text-align:right;"> 62 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> family </td>
   <td style="text-align:left;"> family </td>
   <td style="text-align:right;"> 62 </td>
  </tr>
</tbody>
</table>

</div></div>

<p markdown="1"> Finally, we will create a color pallete and *voila!* we've created a word cloud based on over 900 romance movie plots: </p>

<div class="chunk" id="unnamed-chunk-25"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">pallete</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">mutate</span><span class="hl std">(romance.df,</span> <span class="hl kwc">color</span> <span class="hl std">=</span> <span class="hl kwd">cut</span><span class="hl std">(freq,</span> <span class="hl kwc">breaks</span> <span class="hl std">=</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl num">0</span><span class="hl std">,</span> <span class="hl num">40</span><span class="hl std">,</span> <span class="hl num">80</span><span class="hl std">,</span> <span class="hl num">120</span><span class="hl std">,</span> <span class="hl num">160</span><span class="hl std">,</span> <span class="hl num">Inf</span><span class="hl std">),</span>
    <span class="hl kwc">labels</span> <span class="hl std">=</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;#FFCAE5&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;#BF1168&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;#EA72C4&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;#FF167F&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;A40000&quot;</span><span class="hl std">),</span> <span class="hl kwc">include.lowest</span> <span class="hl std">=</span> <span class="hl num">TRUE</span><span class="hl std">))</span>

<span class="hl std">romance.word.cloud</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">wordcloud2</span><span class="hl std">(</span><span class="hl kwc">data</span> <span class="hl std">= pallete,</span> <span class="hl kwc">color</span> <span class="hl std">= pallete</span><span class="hl opt">$</span><span class="hl std">color)</span>
<span class="hl std">romance.word.cloud</span>
</pre></div>
</div></div>

<p markdown="1"> ![](/charts/romancewordcloud.png) </p>

<p> If we would like to save our widget, then we can save it as an html, png, and pdf file. </p>

<div class="chunk" id="unnamed-chunk-26"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl com"># save it in html</span>
<span class="hl kwd">library</span><span class="hl std">(</span><span class="hl str">&quot;htmlwidgets&quot;</span><span class="hl std">)</span>
<span class="hl kwd">library</span><span class="hl std">(webshot)</span>

<span class="hl kwd">saveWidget</span><span class="hl std">(romance.word.cloud,</span> <span class="hl str">&quot;romancewordcloud.html&quot;</span><span class="hl std">,</span> <span class="hl kwc">selfcontained</span> <span class="hl std">= F)</span>

<span class="hl com"># and in png or pdf</span>
<span class="hl kwd">webshot</span><span class="hl std">(</span><span class="hl str">&quot;romancewordcloud.html&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;romancewordcloud.png&quot;</span><span class="hl std">,</span> <span class="hl kwc">delay</span> <span class="hl std">=</span> <span class="hl num">60</span><span class="hl std">,</span> <span class="hl kwc">vwidth</span> <span class="hl std">=</span> <span class="hl num">960</span><span class="hl std">,</span>
    <span class="hl kwc">vheight</span> <span class="hl std">=</span> <span class="hl num">480</span><span class="hl std">)</span>
</pre></div>
</div></div>

<hr>

<h2> Crime Movies </h2>

<p> Finally, for a comparison word cloud, we will create a second data set for crime movies to find out how keywords for this genre differ from romance movies: </p>

<div class="chunk" id="unnamed-chunk-27"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">grep</span><span class="hl std">(</span><span class="hl kwc">pattern</span> <span class="hl std">=</span> <span class="hl str">&quot;crime&quot;</span><span class="hl std">,</span> <span class="hl kwc">x</span> <span class="hl std">= dat</span><span class="hl opt">$</span><span class="hl std">genre,</span> <span class="hl kwc">value</span> <span class="hl std">=</span> <span class="hl num">TRUE</span><span class="hl std">,</span> <span class="hl kwc">ignore.case</span> <span class="hl std">=</span> <span class="hl num">TRUE</span><span class="hl std">)</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">head</span><span class="hl std">()</span> <span class="hl opt">%&gt;%</span>
    <span class="hl kwd">kable</span><span class="hl std">()</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">kable_styling</span><span class="hl std">()</span>
</pre></div>
<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> x </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Crime, Mystery </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Comedy, Crime, Fantasy </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Action, Crime, Drama </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Comedy, Crime, Romance </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Crime, Drama, Music </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Comedy, Crime, Family </td>
  </tr>
</tbody>
</table>

</div></div>

<p> We find that there are 930 movies with crime listed as a genre: </p>

<div class="chunk" id="unnamed-chunk-28"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">crime</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">grepl</span><span class="hl std">(</span><span class="hl str">&quot;crime&quot;</span><span class="hl std">, dat</span><span class="hl opt">$</span><span class="hl std">genre,</span> <span class="hl kwc">ignore.case</span> <span class="hl std">= T)</span>
<span class="hl kwd">sum</span><span class="hl std">(crime)</span>
</pre></div>
<div class="output"><pre class="knitr r">[1] 930
</pre></div>
</div></div>

<p> Once again, we will use this criteria to segment out the crime movies: </p>

<div class="chunk" id="unnamed-chunk-29"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">dat.crime</span> <span class="hl kwb">&lt;-</span> <span class="hl std">dat[crime,</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;title&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;genre&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;plot&quot;</span><span class="hl std">)]</span>
<span class="hl std">dat.crime</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">head</span><span class="hl std">(</span><span class="hl num">10</span><span class="hl std">)</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">kable</span><span class="hl std">()</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">kable_styling</span><span class="hl std">()</span>
</pre></div>
<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:left;"> title </th>
   <th style="text-align:left;"> genre </th>
   <th style="text-align:left;"> plot </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:left;"> Code Name: K.O.Z. </td>
   <td style="text-align:left;"> Crime, Mystery </td>
   <td style="text-align:left;"> A look at the 17-25 December 2013 corruption scandal in Turkey, from the viewpoint of the Erdogan government. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 4 </td>
   <td style="text-align:left;"> Daniel der Zauberer </td>
   <td style="text-align:left;"> Comedy, Crime, Fantasy </td>
   <td style="text-align:left;"> Evil assassins want to kill Daniel Kublbock, the third runner up for the German Idols. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 46 </td>
   <td style="text-align:left;"> Final Justice </td>
   <td style="text-align:left;"> Action, Crime, Drama </td>
   <td style="text-align:left;"> Homicidal Sheriff Thomas Jefferson Geronimo is tasked with escorting a mobster to Malta; when the prisoner escapes, Geronimo goes rogue to catch him. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 51 </td>
   <td style="text-align:left;"> Gigli </td>
   <td style="text-align:left;"> Comedy, Crime, Romance </td>
   <td style="text-align:left;"> The violent story about how a criminal lesbian, a tough-guy hit-man with a heart of gold, and a mentally challenged man came to be best friends through a hostage. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 63 </td>
   <td style="text-align:left;"> Girl in Gold Boots </td>
   <td style="text-align:left;"> Crime, Drama, Music </td>
   <td style="text-align:left;"> A young woman leaves her job as a waitress and travels to Los Angeles, where she strives to become the top star in the glamorous world of go-go dancing. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 69 </td>
   <td style="text-align:left;"> Baby Geniuses </td>
   <td style="text-align:left;"> Comedy, Crime, Family </td>
   <td style="text-align:left;"> Scientist hold talking, super-intelligent babies captive, but things take a turn for the worse when a mix-up occurs between a baby genius and its twin. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 75 </td>
   <td style="text-align:left;"> Tees Maar Khan </td>
   <td style="text-align:left;"> Comedy, Crime </td>
   <td style="text-align:left;"> Posing as a movie producer, a conman attempts to trick an entire village into helping him rob a treasure-laden train. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 86 </td>
   <td style="text-align:left;"> Mitchell </td>
   <td style="text-align:left;"> Action, Crime, Drama </td>
   <td style="text-align:left;"> A sleazy, incompetent detective tries to simultaneously take down heroin dealers and a socialite who murdered a burglar. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 96 </td>
   <td style="text-align:left;"> I Accuse My Parents </td>
   <td style="text-align:left;"> Crime, Drama </td>
   <td style="text-align:left;"> Young man goes to work for gangsters to impress his nightclub-singer girlfriend. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 99 </td>
   <td style="text-align:left;"> Ghosts Can't Do It </td>
   <td style="text-align:left;"> Comedy, Crime, Fantasy </td>
   <td style="text-align:left;"> Elderly Scott kills himself after a heart attack wrecks his body, but then comes back as a ghost and convinces his loving young hot wife Kate to pick and kill a young man in order for Scott to possess his body and be with her again. </td>
  </tr>
</tbody>
</table>

</div></div>

<p> Now we extract the crime plots: </p>

<div class="chunk" id="unnamed-chunk-30"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl com"># Create a vector containing only the text</span>
<span class="hl std">crime.plot</span> <span class="hl kwb">&lt;-</span> <span class="hl std">dat.crime</span><span class="hl opt">$</span><span class="hl std">plot</span>
</pre></div>
</div></div>

<p> Create a crime corpus: </p>

<div class="chunk" id="unnamed-chunk-31"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl com"># Create a corpus</span>
<span class="hl std">crime.docs</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">Corpus</span><span class="hl std">(</span><span class="hl kwd">VectorSource</span><span class="hl std">(crime.plot))</span>
<span class="hl std">crime.docs</span>
</pre></div>
<div class="output"><pre class="knitr r">&lt;&lt;SimpleCorpus&gt;&gt;
Metadata:  corpus specific: 1, document level (indexed): 0
Content:  documents: 930
</pre></div>
</div></div>

<br>

<div class="chunk" id="unnamed-chunk-32"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">crime.docs</span> <span class="hl kwb">&lt;-</span> <span class="hl std">crime.docs</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">tm_map</span><span class="hl std">(removeNumbers)</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">tm_map</span><span class="hl std">(removePunctuation)</span> <span class="hl opt">%&gt;%</span>
    <span class="hl kwd">tm_map</span><span class="hl std">(stripWhitespace)</span>
<span class="hl std">crime.docs</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">tm_map</span><span class="hl std">(crime.docs,</span> <span class="hl kwd">content_transformer</span><span class="hl std">(tolower))</span>
<span class="hl std">crime.docs</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">tm_map</span><span class="hl std">(crime.docs, removeWords,</span> <span class="hl kwd">stopwords</span><span class="hl std">(</span><span class="hl str">&quot;english&quot;</span><span class="hl std">))</span>
</pre></div>
</div></div>

<br>

<div class="chunk" id="unnamed-chunk-33"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">crime.dtm</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">TermDocumentMatrix</span><span class="hl std">(crime.docs)</span>
<span class="hl std">crime.matrix</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">as.matrix</span><span class="hl std">(crime.dtm)</span>
<span class="hl std">crime.words</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">sort</span><span class="hl std">(</span><span class="hl kwd">rowSums</span><span class="hl std">(crime.matrix),</span> <span class="hl kwc">decreasing</span> <span class="hl std">=</span> <span class="hl num">TRUE</span><span class="hl std">)</span>
<span class="hl std">crime.df</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">data.frame</span><span class="hl std">(</span><span class="hl kwc">word</span> <span class="hl std">=</span> <span class="hl kwd">names</span><span class="hl std">(crime.words),</span> <span class="hl kwc">freq</span> <span class="hl std">= crime.words)</span>
<span class="hl std">crime.df</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">head</span><span class="hl std">(</span><span class="hl num">10</span><span class="hl std">)</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">kable</span><span class="hl std">()</span> <span class="hl opt">%&gt;%</span> <span class="hl kwd">kable_styling</span><span class="hl std">()</span>
</pre></div>
<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:left;"> word </th>
   <th style="text-align:right;"> freq </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> man </td>
   <td style="text-align:left;"> man </td>
   <td style="text-align:right;"> 109 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> young </td>
   <td style="text-align:left;"> young </td>
   <td style="text-align:right;"> 89 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> two </td>
   <td style="text-align:left;"> two </td>
   <td style="text-align:right;"> 86 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> police </td>
   <td style="text-align:left;"> police </td>
   <td style="text-align:right;"> 83 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> one </td>
   <td style="text-align:left;"> one </td>
   <td style="text-align:right;"> 77 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> life </td>
   <td style="text-align:left;"> life </td>
   <td style="text-align:right;"> 69 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> murder </td>
   <td style="text-align:left;"> murder </td>
   <td style="text-align:right;"> 60 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> new </td>
   <td style="text-align:left;"> new </td>
   <td style="text-align:right;"> 59 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> crime </td>
   <td style="text-align:left;"> crime </td>
   <td style="text-align:right;"> 57 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> drug </td>
   <td style="text-align:left;"> drug </td>
   <td style="text-align:right;"> 52 </td>
  </tr>
</tbody>
</table>

</div></div>

<p> For crime moves, we find that our most frequent word is man and young comes in second place again. The third most frequently used word here is 'two', but police is not far behind for fourth place, as would be expected in a crime movie.

Now we create a new color pallete and just like that we have a second word cloud based on over 900 crime movie plots: </p>

<div class="chunk" id="unnamed-chunk-34"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">crime.pallete</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">mutate</span><span class="hl std">(crime.df,</span> <span class="hl kwc">color</span> <span class="hl std">=</span> <span class="hl kwd">cut</span><span class="hl std">(freq,</span> <span class="hl kwc">breaks</span> <span class="hl std">=</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl num">0</span><span class="hl std">,</span> <span class="hl num">25</span><span class="hl std">,</span> <span class="hl num">50</span><span class="hl std">,</span> <span class="hl num">75</span><span class="hl std">,</span> <span class="hl num">100</span><span class="hl std">,</span>
    <span class="hl num">Inf</span><span class="hl std">),</span> <span class="hl kwc">labels</span> <span class="hl std">=</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;#C9D1D6&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;#616D7A&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;#5B83AD&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;#1E1E1E&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;#13365B&quot;</span><span class="hl std">),</span> <span class="hl kwc">include.lowest</span> <span class="hl std">=</span> <span class="hl num">TRUE</span><span class="hl std">))</span>

<span class="hl std">crime.word.cloud</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">wordcloud2</span><span class="hl std">(</span><span class="hl kwc">data</span> <span class="hl std">= crime.pallete,</span> <span class="hl kwc">color</span> <span class="hl std">= crime.pallete</span><span class="hl opt">$</span><span class="hl std">color)</span>
<span class="hl std">crime.word.cloud</span>
</pre></div>
</div></div>

<p markdown="1"> ![](/charts/crimewordcloud.png) </p>

<div class="chunk" id="unnamed-chunk-35"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl com"># save it in html</span>
<span class="hl kwd">library</span><span class="hl std">(</span><span class="hl str">&quot;htmlwidgets&quot;</span><span class="hl std">)</span>
<span class="hl kwd">library</span><span class="hl std">(webshot)</span>

<span class="hl kwd">saveWidget</span><span class="hl std">(crime.word.cloud,</span> <span class="hl str">&quot;crimewordcloud.html&quot;</span><span class="hl std">,</span> <span class="hl kwc">selfcontained</span> <span class="hl std">= F)</span>

<span class="hl com"># and in png or pdf</span>
<span class="hl kwd">webshot</span><span class="hl std">(</span><span class="hl str">&quot;crimewordcloud.html&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;crimewordcloud.png&quot;</span><span class="hl std">,</span> <span class="hl kwc">delay</span> <span class="hl std">=</span> <span class="hl num">60</span><span class="hl std">,</span> <span class="hl kwc">vwidth</span> <span class="hl std">=</span> <span class="hl num">960</span><span class="hl std">,</span> <span class="hl kwc">vheight</span> <span class="hl std">=</span> <span class="hl num">480</span><span class="hl std">)</span>
</pre></div>
</div></div>

<hr>

<h1> Comparison and Conclusion: </h1>

<p>Now that we have completed this tutorial, we have been able to discover some similarities in individual words between the two word clouds: </p>

<p markdown="1">

![](/charts/romancewordcloud.png)

![](/charts/crimewordcloud.png) </p>

<p markdown="1"> However, the collective data from both sets of plots clearly displays how some of the same keywords can carry very different meanings between genres. We have also learned two methods for processing data: the first method using `grep()`, `grepl()` and `quanteda()` is quite a bit longer than using the `tm()` methods, but it has its perks of letting us see our data at every stage. Similarly `tm()` does not provide as much insight into into textual analysis, but for the purposes of quick projects like the wordclouds, it makes the process much easier!</p>

<hr>

</body>
</html>

# Resources

Now that you have created your first wordcloud, I encourage you to try out the features and give it a try yourself! The following links contain tutorials and resources to guide you each step of the way:

* [Create a word cloud with r](https://towardsdatascience.com/create-a-word-cloud-with-r-bde3e7422e8a) This tutorial from 'towards data science' by Celine Van den Rul served as the inspiration for this code-through and includes great step-by-step details on all of the features of `wordcloud2()` as well as its more basic predecessor `wordcloud()`. Both packages have unique benefits!

* [Wordcloud](https://analyticstraining.com/how-to-create-a-word-cloud-in-r/) 
This article focuses on the first `wordcloud()` and has lots of great insight.

* [R Graph Gallery: Wordcloud2](https://www.r-graph-gallery.com/196-the-wordcloud2-library.html)
The R Graph Gallery has excellent tutorials on various data visualization strategies in R and is a wonderful guide for Wordcloud2.

* CRAN also has wonderful resources for `WordCloud2()` available for view [here](https://cran.r-project.org/web/packages/wordcloud2/wordcloud2.pdf) in PDF form and [here](https://cran.r-project.org/web/packages/wordcloud2/vignettes/wordcloud.html) in website form.

* [Webshot](https://cran.r-project.org/web/packages/webshot/webshot.pdf) is a neat package that allows us to create static saved images of the ever-changing word clouds that we create.

* [Coolors](https://coolors.co/) is a great site to find colors for your word cloud palettes.

* Last, but certainly not least, [Data.World](https://data.world/studentoflife/imdb-top-250-lists-and-5000-or-so-data-records) is an excellent place to find data on just about any subject and the pre-configured R URL download links are especially helpful! 

---

# About The Author

This code-through tutorial was created by Courtney Stowers for CPP 527 Data Science 2 in the Master of Science in Program Evaluation and Data Analytics program at Arizona State University. She can be contacted at: `courtney.stowers@asu.edu`.

---
