---
layout: benchmark
title: GeoBIM benchmark
permalink: /projects/geobim-benchmark/
---
<!--
<div class="row">
	<div class="col-md-4 col-md-offset-1 col-xs-6">
		<img class="img-responsive" src="img/benchmark.png" />
	</div>
	<div class="col-md-6 col-md-offset-1 col-xs-6">
		<h1>GeoBIM benchmark 2019</h1>
	</div>
</div>
-->
<h1>GeoBIM benchmark 2019</h1>

<div class="row">
  <div class="col-sm-12 col-xs-12"><img class="img-responsive" src="{{ "/projects/geobim-benchmark/img/logohome.gif" }}" style="max-height: 300px"></div>
  
  
</div>



<div class="well"><b>Recent news</b><br/><br/>
	{% assign sorted = site.data.geobim_news | sort: 'date' | reverse %}
  {% for news in sorted limit:3 %}
  	<p><small><span class="post-date">{{ news.date | date: "%b %d" }}</span></small> {{ news.news }}</p>
  {% endfor %}
  <a href="news.html">All news</a>
</div>

<!--
The website and the FAQ section tries to answer possible doubts about the test and the data. Do you have more and would you like to talk about them or about found issues? You can register in this doodle: [Q&A session about the GeoBIM benchmark](https://doodle.com/poll/fz6a6vvvz3dft5g9) If a few people are interested, the session will be hold on 3rd October 2019 h.10.00-11.00 CET via web (we will publish the link to join).
-->

<iframe width="560" height="315" src="https://www.youtube.com/embed/uXcGYA72IaA" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


<h3>Reference study on software support for open standards of city and building models</h3>

This benchmark investigated the available **technical solutions** to support research and activities related to GeoBIM, through the use of the **open standards IFC** (by buildingSMART) and **CityGML** (by OGC):

.

<h4>1. What is the <strong>support for IFC</strong> within BIM (and other) software?</h4>

<h4>2. What options for <strong>geo-referencing BIM</strong> data are available?</h4>

<h4>3. What is the <strong>support for CityGML</strong> within GIS (and other) tools?</h4>

<h4>4. What options for <strong>conversion (IFC↔CityGML)</strong> are available?</h4>
.

The final **Results** of the project can be accessed at the [presentations&publications](https://3d.bk.tudelft.nl/projects/geobim-benchmark/presentations-publications.html) page of this website, as well as all the resulting datasets.


<!--
### We want you

**You can be part** of this scientific initiative!

Take part in this benchmark activity by performing one or more tasks.

<div class="row">
	<div class="col-md-3 col-xs-4">
		<img class="img-responsive" src="img/puzzle.jpg" />
	</div>
	<div class="col-md-9 col-xs-8">
		<p>Volunteers will be welcome to perform one or more tasks using the tools they are familiar with, <strong>contributing to build a common reference list of tools</strong> to be used for GeoBIM-related activities.</p>
		<p>At least one <strong>scientific publication co-authored by both the benchmark proponents and volunteers</strong> will be written, and final results will be published on the website.</p>
	</div>
</div>

### How to participate

<div class="row">
	<div class="col-xs-12">
		<img class="img-responsive" src="img/howto.gif" />
	</div>
</div>

#### [**Register to this link**](https://francescanoardo.typeform.com/to/IbdpZD) to participate!

And browse the website to know all the details.

Download the [call for participation](https://www.dropbox.com/s/2f6q7ckla9wm9e6/callGeoBIMbenchmark.pdf?dl=0)
-->