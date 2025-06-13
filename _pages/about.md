---
layout: about
title: about
permalink: /
subtitle: '<span style="color: purple;">Quantitative Social Scientist</span> &#124; Focused on Education Policy &amp; Data Science'

profile:
  align: right
  image: prof_pic.jpg
  image_circular: true # crops the image to make it circular
  more_info: >
    <p> </p>

selected_papers: false # includes a list of papers marked as "selected={true}"
social: true # includes social icons at the bottom of the page

announcements:
  enabled: true # includes a list of news items
  scrollable: true # adds a vertical scroll bar if there are more than 3 news items
  limit: 5 # leave blank to include all the news in the `_news` folder

latest_posts:
  enabled: false
  scrollable: true # adds a vertical scroll bar if there are more than 3 new posts items
  limit: 3 # leave blank to include all the blog posts
---

Coming from a small town in rural Colombia, I understand the impact education can have on the trajectory of one's life. Motivated by my lived experience and a drive to create meaningful change, I am passionate about addressing disparities in our education system by **bridging the gap between research and practice**.
<p>
  This portfolio is rooted in education policy, but its core principles—rigorous analysis, thoughtful design, and evidence-based insight—extend beyond that. These projects showcase the power and versatility of strong data science, regardless of the domain.
</p>
<br><br><br><br>
<h2>Featured Projects</h2>

<div class="row row-cols-1 row-cols-md-3 g-4">
  {% assign sorted_projects = site.projects | sort: "importance" | slice: 0, 6 %}
  {% for project in sorted_projects %}
    <div class="col d-flex align-items-stretch">
      <div class="card w-100 h-100">
        <div class="card-body d-flex flex-column p-0 border-0 shadow-none h-100">
          {% include projects.liquid %}
        </div>
      </div>
    </div>
  {% endfor %}
</div>
