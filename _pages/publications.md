---
layout: page
permalink: /publications/
title: publications

years:  [2022, 2021, 2020, 2019, 2017]
nav: true
nav_order: 1
---
<!-- add year for each paper
description:  publications by categories in reversed chronological order. generated by jekyll-scholar. -->
 
<!-- _pages/publications.md -->
<div class="publications">

{%- for y in page.years %}
  <h2 class="year">{{y}}</h2>
  {% bibliography -f papers -q @*[year={{y}}]* %}
{% endfor %}

</div>
