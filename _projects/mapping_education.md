---
layout: page
title: Education by Borough
description: Geospatial Analysis and Modeling
img: assets/img/edxb_bivariate.png
importance: 2
category: Python
related_publications: true
code_url: https://colab.research.google.com/drive/1-1-iBaIiSke0XXZ4rbwetS5zAmVokZ-p?usp=sharing
code_label: Open in Colab
---
<hr>
<p>The purpose of this project was to use open access data to visualize educational patterns in NYC graduation rates. Observations from the visualizations were subsequently tested using three different types of statistical analyses.</p>
<h4 style="color:rgb(224, 10, 10);">Contextualizing Education in NYC</h4>
<p>
    New York City hosts the largest public school system in the United States, serving over one million students across more than 1,800 schools. Yet this immense scale is accompanied by deeply entrenched educational disparities shaped by race, income, neighborhood, and historical patterns of segregation. For further reading,
         <i>
            <a href="https://journals.sagepub.com/doi/full/10.1177/23328584211038939" target="_blank" rel="noopener">
            Kafka & Matheny (2021)</a>
        </i> 
    do a fantastic job highlighting the historical patterns of gentrification in the context of education in New York City.
<p>
    You can click on the provided Colab Notebook to access the entirety of the code and the data cleaning process. As someone invested in the replicability of research, you can also access the original datasets below.
</p>

<h5 style="color:rgb(224, 10, 10);">Shapefiles</h5>

<ul>
    <li>
        <a href="https://www.nyc.gov/content/planning/pages/resources/datasets/school-districts" target="_blank" rel="noopener">
            School Districts Shapefiles
        </a>
    </li>
    <li>
        <a href="https://data.cityofnewyork.us/Education/School-Point-Locations/jfju-ynrr/about_data" target="_blank" rel="noopener">
            School Locations
        </a>
    </li>
</ul>

<h5 style="color:rgb(224, 10, 10);"> School Level Data </h5>

<ul>
    <li>
        <a href="https://data.cityofnewyork.us/Education/2010-AP-College-Board-School-Level-Results/itfs-ms3e/about_data" target="_blank" rel="noopener">
            AP Scores
        </a>
    </li>
    <li>
        <a href="https://data.cityofnewyork.us/Education/2005-2015-Graduation-Rates-District-All/fq9e-fd84/about_data" target="_blank" rel="noopener">
            District Level Graduation Rates
        </a>
    </li>
    <li>
        <a href="https://data.cityofnewyork.us/Education/2013-2018-Demographic-Snapshot-School/s52a-8aq6/about_data" target="_blank" rel="noopener">
            Demographics
        </a>
    </li>
</ul>
<p>
    There was a lot of other data processed, all of which is provided to you via the Colab Notebook 
</p>


<p>
    Graduation rates vary widely across boroughs and districts, often reflecting broader inequalities in access to resources, experienced educators, and academic support. <b>This project seeks to visualize such patterns and examine their statistical underpinnings to better inform policy and community-driven solutions</b>.
</p>
<p>Let's take a look at each of the boroughs, the school districts within them, and their corresponding dropout rates</p>

<div class="mb-3">
    <label for="levelSelect" class="form-label"><strong>View Level:</strong></label>
    <select id="levelSelect" class="form-select" style="max-width: 250px;">
        <option value="borough" selected>Borough Level</option>
        <option value="zone">School Zone Level</option>
    </select>
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0" style="max-width:66%; margin:auto;">
        <div id="borough-img">
            <h5 class="text-center mb-3">Dropout Rates by Borough</h5>
            {% include figure.liquid loading="eager" path="assets/img/edxb_borough_dropout_map.png" title="Dropout Rates by Borough" class="img-fluid rounded z-depth-0" %}
        </div>
        <div id="zone-img" style="display:none;">
            <h5 class="text-center mb-3">Dropout Rates by School Zone</h5>
            {% include figure.liquid loading="eager" path="assets/img/edxb_blab.png" title="Dropout Rates by School Zone" class="img-fluid rounded z-depth-0" %}
        </div>
    </div>
</div>

<script>
document.getElementById('levelSelect').addEventListener('change', function() {
    var boroughImg = document.getElementById('borough-img');
    var zoneImg = document.getElementById('zone-img');
    if (this.value === 'borough') {
        boroughImg.style.display = '';
        zoneImg.style.display = 'none';
    } else {
        boroughImg.style.display = 'none';
        zoneImg.style.display = '';
    }
});
</script>
<h4 style="color:rgb(224, 10, 10);">Interpretation</h4> 
<p>
    Based on simple visual inspections, each of the maps paints a different picture of the educational landscape in New York City.
</p>
<p>
    <b>Borough Level:</b> The first map presents average dropout rates aggregated at the borough level. It reveals striking disparities: The Bronx and Brooklyn exhibit the highest average dropout rates, with deep red shading signaling values above 30%, while Manhattan and Staten Island show comparatively lower dropout rates. However, this broader view can obscure within-borough variation and may overgeneralize the challenges each borough faces.
</p>

<p>
    <b>School Zone Level:</b><br> The second map disaggregates data by individual school zones, offering a more granular and nuanced picture. Within both Brooklyn and the Bronx, we observe pockets of extremely high dropout rates concentrated in specific zones, while neighboring zones fare better. This localized perspective highlights the importance of looking beyond borough-level summaries to identify educational inequities that persist within smaller geographic areas.
</p>

<p>
    Together, these visualizations highlight the need for targeted, zone-specific interventions, as citywide or borough-wide policies may miss the communities most at risk.
</p>

<h4 style="color:rgb(224, 10, 10);">Bivariate Choropleth Maps</h4>
<p>Bivariate choropleth maps are thematic maps that display the spatial relationship between two variables at once. By blending two color scales, these maps help identify areas where high or low values of both variables intersect. This allows for a more nuanced understanding of overlapping inequalities than single-variable maps alone.</p>

<iframe src="https://ilovedogs3003.github.io/lfs/maps/bivariate_dems.html"
        width="100%" height="600" style="border:none;" loading="eager"></iframe>

<h4 style="color:rgb(224, 10, 10);">Geospatial Analysis</h4>
<li>This will be added soon: it is simply a look into spatial autocorrelation and the variables that best predict dropout rates!</li>
<li> Formatting HTML takes a surpringly long amount of time </li>

<!--
<p>
    The map aboves depicts the relationship between the racial composition of a school and the dropout rates. It is important to not that the scale for the percent of White students was reversed to make the map more intuitive (i.e., lighter colors signals a higher % of white students). In it we see the following:
        <li>Despite brooklyn demonstrating some of the highest dropout rates, we see how those rates are congregated around districts with very low percent of white students.</li>
        <li></li>
-->

