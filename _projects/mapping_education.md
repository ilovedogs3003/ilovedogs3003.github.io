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
            {% include figure.liquid loading="eager" path="assets/img/edxb_school_zones_dropout_map.png" title="Dropout Rates by School Zone" class="img-fluid rounded z-depth-0" %}
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
    <b>Borough Level:</b> The first map presents average dropout rates aggregated at the borough level. It reveals striking disparities: The Bronx and Brooklyn exhibit the highest average dropout rates, with deep red shading signaling values around 16%, while Manhattan and Staten Island show comparatively lower dropout rates. However, this broader view can obscure within-borough variation and may overgeneralize the challenges each borough faces. It is also important to keep in mind the relatively small fluctuation presented here.
</p>

<p>
    <b>School Zone Level:</b><br> The second map disaggregates data by individual school zones, offering a more granular and nuanced picture. Within both Brooklyn and the Bronx, we observe pockets of extremely high dropout rates concentrated in specific zones while neighboring zones fare better. This localized perspective highlights the importance of looking beyond borough-level summaries to identify educational inequities that persist within smaller geographic areas.
</p>

<p>
    Together, these visualizations highlight the need for targeted, zone-specific interventions, as citywide or borough-wide policies may miss the communities most at risk.
</p>

<h5 style="color:rgb(224, 10, 10);">Interactive Summary Tables</h5>
<div style="margin-bottom: 4px; width: 100%; text-align: left; margin-bottom: 0;">
    <iframe src="https://ilovedogs3003.github.io/lfs/html/edxb_int_sumtab.html"
                    width="100%" height="1000px" style="border: none; display: block; margin: 0; padding: 0;" loading="eager"></iframe>
</div>


<h4 style="color:rgb(224, 10, 10);">Bivariate Choropleth Maps</h4>
<p>Bivariate choropleth maps are thematic maps that display the spatial relationship between two variables at once. By blending two color scales, these maps help identify areas where high or low values of both variables intersect. This allows for a more nuanced understanding of overlapping inequalities than single-variable maps alone.</p>

<div style="text-align: center;">
    <iframe src="https://ilovedogs3003.github.io/lfs/maps/edxb_bivariate_dems.html"
            width="100%" height="600" style="border:none;" loading="eager"></iframe>
    <div style="font-size: 0.98em; color: #555; margin-top: 8px;">
        <em>
            This map visualizes the concentration of students of color alongside dropout rates across NYC. Areas with higher proportions of students of color tend to show comparably higher dropout rates.
        </em>
    </div>
</div>
<br>
<h4 style="color:rgb(224, 10, 10);">Geospatial Analysis</h4>

<div style="display: flex; flex-wrap: wrap; gap: 24px; justify-content: center; align-items: flex-start; margin-bottom: 2em;">
    <div class="model-section" style="flex: 1 1 300px; min-width: 280px; max-width: 350px; background: #fafafa; border-radius: 8px; padding: 18px; box-shadow: 0 2px 8px rgba(0,0,0,0.04);">
        <h3>1. Ordinary Least Squares</h3>
        <p>
            OLS is a baseline linear regression model that assumes independence between observations.
            It estimates the relationship between dependent and independent variables by minimizing the sum of squared residuals.
        </p>
        <ul>
            <li><strong>Assumes:</strong> No spatial autocorrelation in the data.</li>
            <li><strong>Limitation:</strong> If residuals are spatially autocorrelated, standard errors and significance tests may be biased.</li>
            <li><strong>Use Case:</strong> When data points are spatially independent or when spatial effects are negligible.</li>
        </ul>
        <button class="expand-btn" onclick="toggleModelOutput('ols-output')">Show Model Output</button>
        <div id="ols-output" style="display:none; margin-top:12px;">
            <iframe src="https://ilovedogs3003.github.io/lfs/html/edxb_ols_regression_summary.html"
                width="100%" height="600" style="border:none;" loading="eager"></iframe>
        </div>
    </div>

    <div class="model-section" style="flex: 1 1 300px; min-width: 280px; max-width: 350px; background: #fafafa; border-radius: 8px; padding: 18px; box-shadow: 0 2px 8px rgba(0,0,0,0.04);">
        <h3>2. Spatial Lag Model</h3>
        <p>
            The Spatial Lag Model includes a spatially lagged dependent variable (e.g., <code>W * y</code>) as a predictor. It assumes that what happens in one location directly influences outcomes in neighboring locations.
        </p>
        <ul>
            <li><strong>Captures:</strong> Spatial dependence in the outcome variable.</li>
            <li><strong>Implication:</strong> A change in one region can "spill over" and affect neighboring areas.</li>
            <li><strong>Use Case:</strong> When you're interested in modeling diffusion, feedback loops, or interdependence (e.g., crime in one area affecting nearby areas).</li>
        </ul>
        <button class="expand-btn" onclick="toggleModelOutput('lag-output')">Show Model Output</button>
        <div id="lag-output" style="display:none; margin-top:12px;">
            <iframe src="https://ilovedogs3003.github.io/lfs/html/edxb_lag_model_summary.html"
                width="100%" height="600" style="border:none;" loading="eager"></iframe>
        </div>
    </div>

    <div class="model-section" style="flex: 1 1 300px; min-width: 280px; max-width: 350px; background: #fafafa; border-radius: 8px; padding: 18px; box-shadow: 0 2px 8px rgba(0,0,0,0.04);">
        <h3>3. Spatial Error Model</h3>
        <p>
            The Spatial Error Model assumes that spatial dependence exists in the error term, not in the dependent variable itself.
            It captures omitted spatially structured variables that influence the outcome indirectly.
        </p>
        <ul>
            <li><strong>Captures:</strong> Spatial autocorrelation in the error term (e.g., unmeasured contextual factors).</li>
            <li><strong>Implication:</strong> Accounts for bias and inefficiency in OLS due to spatially correlated unobservables.</li>
            <li><strong>Use Case:</strong> When residual spatial patterns remain even after controlling for observed variables.</li>
        </ul>
        <button class="expand-btn" onclick="toggleModelOutput('error-output')">Show Model Output</button>
        <div id="error-output" style="display:none; margin-top:12px;">
            <iframe src="https://ilovedogs3003.github.io/lfs/html/edxb_error_model_summary.html"
                width="100%" height="600" style="border:none;" loading="eager"></iframe>
        </div>
    </div>
</div>

<script>
function toggleModelOutput(id) {
    var el = document.getElementById(id);
    var btn = event.target;
    if (el.style.display === "none") {
        el.style.display = "";
        btn.textContent = "Hide Model Output";
    } else {
        el.style.display = "none";
        btn.textContent = "Show Model Output";
    }
}
</script>

<h5 style="color:rgb(224, 10, 10);">Interpretation of Model Results</h5>

  <div class="section">
    <h6>1. OLS Model</h6>
    <p>
      The Ordinary Least Squares (OLS) model suggests a moderate fit with an R-squared of 0.375, meaning roughly 38% of the variation in dropout rates is explained by the covariates. Several predictors are statistically significant:
    </p>
    <ul>
      <li><strong>% Black</strong>, <strong>% Hispanic</strong>, <strong>% Students with Disabilities</strong>, <strong>% English Language Learners</strong>, and <strong>Cohort Size</strong> are positively associated with dropout rates and significant at conventional levels (p &lt; 0.05).</li>
      <li>Borough effects are not significant, indicating limited unique contribution once demographic factors are controlled for.</li>
      <li>The F-statistic is highly significant, indicating the model overall is meaningful.</li>
    </ul>
    <p>
      However, diagnostic metrics (Omnibus, Jarque-Bera) suggest non-normality in residuals, and the high condition number indicates possible multicollinearity.
    </p>
  </div>

  <div class="section">
    <h6>2. Spatial Lag Model</h6>
    <p>
      The Spatial Lag Model improves slightly on fit (Pseudo R-squared = 0.3805). This model includes a spatially lagged dependent variable (<code>W_perc_dropout</code>), though this term is not significant (p = 0.357), implying weak spatial autocorrelation in dropout rates.
    </p>
    <ul>
      <li>Key predictors remain significant: <strong>% Black</strong>, <strong>% Hispanic</strong>, <strong>% Students with Disabilities</strong>, <strong>% ELL</strong>, <strong>Cohort Size</strong>.</li>
      <li>Impact decomposition shows small indirect effects, suggesting limited spatial spillover effects between neighboring areas.</li>
      <li>Borough effects are larger in magnitude than OLS, though still not statistically significant.</li>
    </ul>
    <p>
      This model is appropriate when you suspect the outcome in one region is directly influenced by outcomes in neighboring areas.
    </p>
  </div>

  <div class="section">
    <h6>3. Spatial Error Model</h6>
    <p>
      The Spatial Error Model has similar fit (Pseudo R-squared = 0.379). Unlike the Lag model, it assumes spatial autocorrelation is in the residuals. The <code>lambda</code> parameter is very small and not statistically significant (p = 0.859), suggesting no major unaccounted spatial error correlation.
    </p>
    <ul>
      <li>Significant predictors are consistent with the other models: <strong>% Black</strong>, <strong>% Hispanic</strong>, <strong>% Students with Disabilities</strong>, <strong>% ELL</strong>, <strong>Cohort Size</strong>.</li>
      <li>Spatial dependence seems negligible, with both <code>W_perc_dropout</code> and <code>lambda</code> being insignificant.</li>
    </ul>
    <p>
      The Spatial Error Model is ideal when unobserved spatial processes (not captured by predictors) might be driving patterns, but here it adds minimal value over OLS.
    </p>
  </div>