Table of Contents formatting tips 
<br>
https://www.tipsandtricks-hq.com/simple-table-of-contents-toc-using-pure-html-and-css-code-9217

<p>
  <style>
    #toc_container {
    background: #f9f9f9 none repeat scroll 0 0;
    border: 1px solid #aaa;
    display: table;
    font-size: 95%;
    margin-bottom: 1em;
    padding: 20px;
    width: auto;
    float: left
    vertical-align: text-top;
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
</p>

<hr>

<p> Example:
  <body>
    <h1 id="First_Point_Header"> Introduction </h1>
  </body>
 </p>
 
<hr>

Table of Contents Formatting:

<center>
<div id="toc_container" style="margin: 10px 10px">
<p class="toc_title">Contents</p>
<p class="toc_list"><center><a href="#First_Point_Header">Introduction</a></center></p>
<p class="toc_list"><center><a href="#Second_Point_Header">Set-Up</a></center></p>
<p class="toc_list"><center><a href="#Third_Point_Header">Preview Data</a></center></p>
<p class="toc_list"><center><a href="#Fourth_Point_Header">Word Clouds</a></center></p>
<p class="toc_list"><center><a href="#Fourth_Sub_Point_1">Romance Movies</a></center></p>
<p class="toc_list"><center><a href="#Fourth_Sub_Point_2">Crime Movies</a></center></p>
<p class="toc_list"><center><a href="#Fifth_Point_Header">Comparison and Conclusion</a></center></p>
<p class="toc_list"><center><a href="#Sixth_Point_Header">Resources</a></center></p>
</div>
</center>

<hr>

Open Link in new tab format
<br>
<p markdown="1"> [go](http://stackoverflow.com){:target="_blank" rel="noopener"} </p>

<hr>

<h1> Example of image next to text: </h1>

<img style="margin: 50px 50px" src="/img/keywords.jpg" width="225" height="151" alt="Keywords article pic" align="right"/><p> Although data science projects often employ large amounts of numeric data, some projects examine patterns within text and require a different set of tools. In this code-through tutorial, we are going to explore several packages in R that enable researchers to analyze qualitative data sets and discover cool patterns. We are also going to create two word clouds based on movie plot summaries.

In order to complete this tutorial, you will need access to R or RStudio. If you are not familiar with either of these software or would like a refresher on the basics, check out the first steps of my previous code-through <a href="https://castower.github.io/2020-02-29-waffles/" target="_blank">here</a>. I walk you through the entire set-up process beginning with the software.

If you're all set to go with R and RStudio, then proceed below:  </p>

<hr>
