---
layout: page
title: Teach for America
description: The Importance of Adequate Research Design
img: assets/img/tfa_pgd.png
importance: 1
category: Python
related_publications: true
code_url: https://colab.research.google.com/drive/106-ebadclMc1p4M85cKFXClv8y88y6KN?usp=sharing
code_label: Open in Colab
---
<hr>
<p>This project interprets a myriad of multivariate regressions and highlights the importance of understanding the data you're working with when doing so. Through upsampling and various other visualizations and analyses, this project demonstrates how adequate research design can impact conclusions befitting the data. 
</p>
<div style="text-align: right; font-size: 0.9em;">
  <i><strong>Source:</strong> Decker et al. (2004). <em>The effect of Teach For America on students: Findings from a national evaluation.</em></i>
</div>
<br>
<p>
  While I still have access to the data, I do not have access to a dictionary detailing how the variables were defined or how the data was collected. Thus, the purpose of this project is <b> not to make any declarative statements, but demonstrate some of the things I have learned through my master's program.</b>
</p>
<p>
  This project and data specifically focuses on the impact that Teach for America teachers have on student outcomes.
</p>
<h4 style="color: #1e90ff;"><b>Multivariate Regressions</b></h4>
The following is a perfect example of what <b>not to do</b>. You should not run a series of regression models <b>without checking assumptions</b> or properly understanding the distribution of your data and how that may <i>influence</i> or <b>bias</b> the outcomes.
<p>
  In this specific analysis, I am trying to see the baseline relationship between end of year math scores and treatment (i.e., whether the student had a Teach for America teacher). A less knowledgeable social scientist <i>may</i> feel compelled to tell you that--although the results are not significant--they indicate the potential of fruitful results. Significance not withstanding, those who received treatment scored <b>1.38 points higher, on average, on their end of year math scores</b>.
</p> 
<p>
  However, a closer inspection at the R-squared reveals a model that explains less than .1% of the variation within the data. Similarly, the F-statistic--an indicator of model strength--being relatively low highlights the inefficacy of this being a sufficient analysis. 
</p>
<div class="row justify-content-sm-center">
    <div class="col-sm-8 col-md-6 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/tfa_brms.png" title="Bivariate OLS Model" class="img-fluid rounded z-depth-0" %}
    </div>
</div>
<h6 style="color: #1e90ff;"><b>Adding More Variables</b></h6>
<p>The following models add the number of days absent, the number of days suspended, whether the student receives free lunch (as a proxy for socioeconomic status), and the number of students.</p>
<p>
  The only difference between the models is that the first defines the number of days suspended as a continuous variable, while the latter defines it as categorical. For susbequent analyses, I am choosing to define the number of days suspended as a categorical variable where 0 = never suspended and 1 = suspended 1 or more times. The substantive justification for doing so is the belief that <b>the total number of days suspended does not matter, but rather that students being suspended at all share some form of baseline characteristics that may set them apart from other students.</b>
</p>
<div class="row">
  <div class="col-sm-6 mt-3 mt-md-0">
    <div style="text-align: center; font-weight: bold; margin-bottom: 0.5em;">
      Days Suspended as Categorical
    </div>
    {% include figure.liquid path="assets/img/tfa_ols_suscat.png" title="Suspended Days as Categorical" class="img-fluid rounded z-depth-0" %}
  </div>
  <div class="col-sm-6 mt-3 mt-md-0">
    <div style="text-align: center; font-weight: bold; margin-bottom: 0.5em;">
     Days Suspended as Continuous
    </div>
    {% include figure.liquid path="assets/img/tfa_ols_suscon.png" title="Multivariate OLS with Suspended Days as Continuous" class="img-fluid rounded z-depth-0" %}
  </div>
</div>
<p>The first model, where the number of days suspended is defined as categorical variable, has an R-squared and F-statistic larger than the second model where it is defined as a continuous variable(<i>F</i> = 13.76 v. 12.44; R-squared = .039 v. .035). This gives the aforementioned justification some credence.</p>
<div style="height: 4px;"></div>
<h4 style="color: #1e90ff;"><b>Unconditional Model v. First Model</b></h4>
The inclusion of additional variables allows us to control for their influence over the dependent variable. With these variables incorporated, we saw a considerable increase in our model's efficacy. Forgoing discussion of model parameters, a noteworthy increase is the estimated influence of treatment on math scores: albeit still insignificant, the unconditional model estimated Teach for America teachers to increase math scores, on average, by 1.37 points while the first model estimated that influence to be 1.51.
<p><i>It is important to note that the proper way of constructing a taxonomy of models is to include each additional variable one at a time. From there, compare the impact that each variable has on the regression results to better understand their explanatory power.</i></p>
<h6 style="color: #1e90ff;"><b>Visualizing It</b></h6>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/tfa_scatter.png" title="TFA Scatter" class="img-fluid rounded z-depth-0" %}
    </div>
</div>
I could be verbose here about the implications of students missing more than 20 days (truancy and court involvement usually happens at around 10 days); however, what is important to understand here is that--considering the sample as a whole--there is a slight negative association between the number of days absent and end of year math scores.
<div style="height: 4px;"></div>
In the context of Education Policy, it is <b>crucial</b> to understand how such relationships may differ between schools. Knowing where resources need to be differently allocated allows us to better address educational disparities. 
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/tfa_schoolmeans.png" title="School Means" class="img-fluid rounded z-depth-0" %}
    </div>
</div>
<p>Albeit gradual, there are considerable differences between the means at each school. Additionally, there appear to be several issues in terms of data collection (i.e., school 204 and 215 missing their reading scores).</p>
<p>
  Focusing on math scores alone, one could attempt to address these disparities by running a 
  <a class="btn btn-link p-0" data-bs-toggle="collapse" href="#fixedEffectsCollapse" role="button" aria-expanded="false" aria-controls="fixedEffectsCollapse">
    <b>Fixed Effects Model</b>
  </a> 
  .
</p>

<div class="collapse" id="fixedEffectsCollapse">
  <div class="card card-body border-0 p-0 mt-3">
    <div class="row justify-content-center">
      <div class="col-md-6 text-center">
        {% include figure.liquid 
           loading="eager" 
           path="assets/img/tfa_fe_ols_sum.png" 
           title="Fixed Effects Results" 
           class="img-fluid rounded z-depth-0" %}
      </div>
    </div>
  </div>
</div>
<p>Accounting for school fixed effects (i.e., factors within each school that could influence the dependent variable), we see a model with considerably greater explanatory power. Our model now accounts for 21% of the variation within the observed scores and the F-statistic nearly doubled.</p>

All things considered, the model insinuates that Teach for America teachers **do not** provide students with a statistically significant increase in their end of year math scores (<i>p</i> = 0.118). However, as I previously mentioned, this is an example of what <b>NOT to do.</b>
<br><br>
Let us take a closer look at the variables we were using:
<div class="row justify-content-sm-center">
  <!-- Image 1 -->
  <div class="col-sm-6 col-md-6 mt-3 mt-md-0">
    <div class="ratio ratio-4x3">
      <img src="{{ 'assets/img/tfa_schooldist.png' | relative_url }}" class="img-fluid rounded z-depth-1" alt="Distribution" style="object-fit: contain;">
    </div>
  </div>

  <!-- Image 2 -->
  <div class="col-sm-6 col-md-6 mt-3 mt-md-0">
    <div class="ratio ratio-4x3">
      <img src="{{ 'assets/img/tfa_cormatrix.png' | relative_url }}" class="img-fluid rounded z-depth-1" alt="Correlation Matrix" style="object-fit: contain;">
    </div>
  </div>
</div>
<br>
<div class="row align-items-center">
  <!-- Image column -->
  <div class="col-md-3 text-center">
    <div style="max-width: 150px; margin: auto;">
      {% include figure.liquid 
         path="assets/img/tfa_obs.png" 
         title="Observation Counts" 
         class="img-fluid rounded z-depth-0" %}
    </div>
  </div>

  <!-- Text column -->
  <div class="col-md-9">
    <p>The code and visualizations highlight several key issues within our data:</p>
    <ul>
      <li>Some schools are severely underrepresented.</li>
      <li>The number of students receiving free lunch runs the risk of autocorrelation (this is a byproduct of it being a categorical variable included in a fixed effects regression).</li>
      <li>The data is right-skewed and the distribution looks different within each of the schools.</li>
    </ul>
  </div>
</div>
<br>
<p>In an attempt to address sampling issues while retaining the distribution of the data, we can artificially duplicate observations within each school of the schools.</p> 
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/tfa_sampling.png" title="Bar Chart of Samples" class="img-fluid rounded z-depth-0" %}
    </div>
</div>
<p>While undersampling is a viable alternative, it often comes at the cost of larger standard errors and weaker predictive performance.</p>
<p>
  The 
  <a class="btn btn-link p-0" data-bs-toggle="collapse" href="#syntheticCollapse" role="button" aria-expanded="false" aria-controls="syntheticCollapse">
    <b>distribution of the data</b>
  </a> 
  appears roughly unchanged following synthetic upsampling.
</p>


<div class="collapse" id="syntheticCollapse">
  <div class="card card-body border-0 p-0 mt-3">
    <div class="row justify-content-center">
      <div class="col-md-6 text-center">
        {% include figure.liquid 
           loading="eager" 
           path="assets/img/tfa_dist_ups.png" 
           title="Fixed Effects Results" 
           class="img-fluid rounded z-depth-1" %}
      </div>
    </div>
  </div>
</div>
<br>
<h4 style="color: #1e90ff;"><b>Fixed Effects Model Addressing Undersampling</b></h4>
<div class="row align-items-center">
    <!-- Image column -->
    <div class="col-md-6 d-flex justify-content-center align-items-center" style="height:100%;">
        <div style="max-width: 800px; width: 100%;">
            {% include figure.liquid 
                path="assets/img/tfa_fe_ups_ols.png" 
                title="FE Model Upsampled Summary" 
                class="img-fluid rounded z-depth-0 w-100" %}
        </div>
    </div>
    <!-- Text column -->
    <div class="col-md-6">
        <ul>
            <li>Students with Teach for America teachers, after controlling for school fixed effects, saw an average increase of 2.82 points in their end of year math scores. These effects were highly variable, ranging from -9.54 at school 204 to 24.55 at school 614!</li>
            <li>Although not significant, students who were suspended, on average, scored -0.79 points lower on their math scores (<i>p</i> = 0.65).</li>
            <li>Each day missed was associated with a -0.35 decrease in student math scores, highlighting the importance of addressing truancy issues in schools (<i>p</i> &lt; .001).</li>
            <li>The methods described above doubled the F-statistic and slightly increased the variation in the data explained by our model (R-squared = 24%).</li>
        </ul>
        <br>
        <p>
            While this is not a comprehensive look into the specifics of the Teach for America program or the assumptions behind multivariate regressions, it demonstrates the importance of adequate research design. <b>The questions we ask and the answers we derive are only as adequate as our understanding of the data.</b>
        </p>
    </div>
</div>

