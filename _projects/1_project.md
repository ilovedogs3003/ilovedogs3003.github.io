---
layout: page
title: Teach for America
description: The Importance of Adequate Research Design
img: assets/img/tfa_pgd.png
importance: 1
category: work
related_publications: true
---

<h5><b>Description</b>: This project interprets a myriad of multivariate regressions and highlights the importance of understanding the data you're working with when doing so.</h5>
<h6><b><i>Data from Decker et. al (2004)</i></b></h6>


<h4><b>Data Description</b></h4>

This data was given to me in Dr. Aparna Anand's <b>EDPA 6002: Quantitative Methods for Evaluating Education Policies and Programs</b> course. While I still have access to the data, I do not have access to the data dictionary. This means that, within the dataframe, there are a lot of variables that I do not know i) how they are defined and ii) how the data was collected. Thus, the purpose of this project is <b> not to make any declarative statements, but demosntrate some of the things I've learned </b>


This data specifically focuses on the impact that Teach for America teachers have on student outcomes.


<h4><b>Multivariate Regressions</b></h4>
The following is a perfect example of what <b>not to do</b>. You should not run a series of regression models <b>without checking assumptions</b> or properly understanding the distribution of your data and how that may <i>influence</i> or <b>bias</b> the outcomes


In this specific analysis, I am trying to see the baseline relationship between end of year math scores and treatment (i.e., whether the student had a Teach for America teacher). At a baseline, a less knowledgeable social scientist <i>may</i> feel compelled to tell you that--although the results are not significant--they indicate the potential of fruitful results. Significance not withstanding, those who received treatment scored **1.38 points higher, on average, on their end of year math scores**. 


However, a closer inspection at the R-squared reveals a model that explains less than .1% of the variation within the data. Similarly, the F-statistic--an indicator of model strength--being relatively low highlights the inefficacy of this being a sufficient analysis. 
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/tfa_brms.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>


<h5><b>Now Let's Add More Variables:</b></h5>
The following models add the number of days absent, the number of days suspended, whether the student receives free lunch (as a proxy for socioeconomic status), and the number of students.


The only difference between the models is that the first defines the number of days suspended as a continuous variable, while the latter defines it as categorical. For susbequent analyses, I am choosing to define the number of days suspended as a categorical variable where 0 = never suspended and 1 = suspended 1 or more times. The substantive justificaiton for doing so is the belief that the number of days suspended doesn't matter, but the students being suspended at all share some form of baseline characteristics that may set them apart from other students.
<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/tfa_ols_suscon.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/tfa_ols_suscat.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<p>The first model, where the number of days suspended is defined as categorical variable, has an R-squared and F-statistic larger than the second model where it's defined as a continuous variable(<i>F</i> = 13.76 v. 12.44; R-squared = .039 v. .035). This gives the aforementioned justification some credence.</p>
<div style="height: 32px;"></div>
<h4><b>Unconditional Model v. First Model</b></h4>
The inclusion of additional variables allows us to control for their influence over the dependent variable. With these variables incorporated, we saw a considerable increase in our model's efficacy. Forgoing discussion of model parameters, a noteworthy increase is the estimated influence of treatment on math scores: albeit still insignificant, the unconditional model estimated Teach for America teachers to increase math scores, on average, by 1.37 points while the first model estimated that influence to be 1.51.


<h6><i>It is important to note that the proper way of constructing a taxonomy of models is to include each additional variable one at a time. From there, compare the impact that each variable has on the regression results to better understand their explanatory power.</i></h6>

<h4><b>Let's Visualize It:</b></h4>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/tfa_scatter.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
I could be verbose here about the implications of students missing more than 20 days (truancy and court involvement usually happens at around 10 days); however, what's important to understand here is that--considering the sample as a whole--there is a slight negative association between the number of absent days and end of year math scores.
```html
<div style="height: 16px;"></div>
```
In the context of Education Policy, it is <b>crucial</b> to understand how relationships differ between schools.
however, looking at how relationships differ between schools is **crucial.**



To give your project a background in the portfolio page, just add the img tag to the front matter like so:

    ---
    layout: page
    title: project
    description: a project with a background image
    img: /assets/img/12.jpg
    ---

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/1.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/3.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Caption photos easily. On the left, a road goes through a tunnel. Middle, leaves artistically fall in a hipster photoshoot. Right, in another hipster photoshoot, a lumberjack grasps a handful of pine needles.
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    This image can also have a caption. It's like magic.
</div>

You can also put regular text between your rows of images, even citations {% cite einstein1950meaning %}.
Say you wanted to write a bit about your project before you posted the rest of the images.
You describe how you toiled, sweated, _bled_ for your project, and then... you reveal its glory in the next row of images.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    You can also have artistically styled 2/3 + 1/3 images, like these.
</div>

The code is simple.
Just wrap your images with `<div class="col-sm">` and place them inside `<div class="row">` (read more about the <a href="https://getbootstrap.com/docs/4.4/layout/grid/">Bootstrap Grid</a> system).
To make images responsive, add `img-fluid` class to each; for rounded corners and shadows use `rounded` and `z-depth-1` classes.
Here's the code for the last row of images above:

{% raw %}

```html
<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-4 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
```

{% endraw %}
