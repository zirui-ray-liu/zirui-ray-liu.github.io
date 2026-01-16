---
layout: page
permalink: /publications/
title: Publications
description: Publications & Preprints.
years: [2025, 2024, 2023, 2022, 2021, 2020]
nav: false
nav_order: 1
---

[Google Scholar](https://scholar.google.com/citations?user=0i1w_egAAAAJ)

<!-- _pages/publications.md -->
<div class="publications">

{%- for y in page.years %}
  <h2 class="year">{{y}}</h2>
  {% bibliography -f {{ site.scholar.bibliography }} -q @*[year={{y}}]* %}
{% endfor %}

</div>

$$*$$ equal contribution.