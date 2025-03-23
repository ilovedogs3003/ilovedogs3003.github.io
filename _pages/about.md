---
layout: about
title: about
permalink: /
subtitle: <a href='#'>Quantitative Social Scientist</a> | Focused on Education Policy & Data Equity

profile:
  align: right
  image: prof_pic.jpg
  image_circular: true # crops the image to make it circular
  more_info: >
    <p> </p>

selected_papers: false # includes a list of papers marked as "selected={true}"
social: true # includes social icons at the bottom of the page

announcements:
  enabled: false # includes a list of news items
  scrollable: true # adds a vertical scroll bar if there are more than 3 news items
  limit: 5 # leave blank to include all the news in the `_news` folder

latest_posts:
  enabled: false
  scrollable: true # adds a vertical scroll bar if there are more than 3 new posts items
  limit: 3 # leave blank to include all the blog posts
---

Coming from a small, rural town in Colombia, I deeply understand the impact education can have on the trajectory of one's life. Motivated by my lived experience and a desire to change the world for the better, I am passionate about addressing disparities in our education system by **bridging the gap between research and practice**. 

Through empirical methods, advocacy, and public policy, I aim to make a world where childen don't have to consider themselves lucky to have earned a high school or college education.



<h2>Featured Projects</h2>

<div class="row row-cols-1 row-cols-md-3">
  {% assign sorted_projects = site.projects | sort: "importance" | slice: 0,3 %}
  {% for project in sorted_projects %}
    {% include projects.liquid %}
  {% endfor %}
</div>

<p class="mt-4">
  <a href="{{ '/projects/' | relative_url }}" class="btn btn-outline-primary">
    View all projects →
  </a>
</p>

