---
layout: page
title: General Social Survey
description: Educational Modeling with Longitudinal Data
img: assets/img/gss_all_reg_ed.png
importance: 2
category: Python
related_publications: true
code_url: https://drive.google.com/file/d/1_F1ssjQa_4PZLKqFrf9S5aYKSt7SqJ6I/view?usp=sharing
code_label: Open in Colab
---

<p>
    This project looks at longitudinal educational data from the
        <i>
            <a href="https://gss.norc.org/us/en/gss.html" target="_blank" rel="noopener">
            General Social Survey (GSS)
            </a>
        </i> 
        and interprets a series of regressions to better understand their methodological strengths, weaknesses, and purposes.
</p>
<br>
The primary purpose of this lab was to assess the changes that occurred throughout the years in the realm of higher education.
<br>
<br>
<h4>Understanding the Data:</h4>
<p>
    The GSS is a nationally representative survey conducted by NORC at the University of Chicago that has tracked social, political, and demographic trends in the United States since 1972. It collects data on Americans’ attitudes, behaviors, and attributes, making it one of the most widely used sources for studying societal change over time.
</p>
<p>
    The variables of interest for this project include
        <ul>
            <li>ID: Categorical identifier for each response</li>
            <li>Sex: Categorical, with 1 being male and 2 being female</li>
            <li>Educ: Categorical and ordinal, total number of years spent in school</li>
            <li>Reg16: Location respondent lived in when they were 16</li>
            <li>Income16: Categorical, perceived income in comparison to the average when the respondent was 16</li>
        </ul>
</p>
<p>The linked Jupyter Notebook provides a more detailed overview of the data cleaning and handling methods. One particular component that's a bit nebulous is how ```reg_broad_simp``` is defined and used, as that's just my personal file of customized functions.</p>
<details>
  <summary>You can click here to see how the <code>reg_broad_simp</code> function operates</summary>
  <pre><code>
def reg_broad_simp(reg_code):
    """
    Simplify region codes to broader categories.
    Parameters:
        reg_code (str or int): The region code to simplify.
    Returns:
        str: The simplified region name or None if not found.
    """
    numeric_map = {
        1: 'northeast',
        2: 'northeast',
        3: 'midwest',
        4: 'midwest',
        5: 'south',
        6: 'south',
        7: 'south',
        8: 'west',
        9: 'west'
    }
    if isinstance(reg_code, (int, float)) and not isinstance(reg_code, bool):
        return numeric_map.get(int(reg_code), None)
    if reg_code in ['new england', 'middle atlantic']:
        return 'northeast'
    elif reg_code in ['east north central', 'west north central']:
        return 'midwest'
    elif reg_code in ['south atlantic', 'east south central', 'west south central']:
        return 'south'
    elif reg_code in ['mountain', 'pacific']:
        return 'west'
    else:
        return None
  </code></pre>
</details>
<br>
<h4>A Look at the Data:</h4>
These simple lines of code will highlight the dependent variable we are specifically looking at, along with some possible concerns:
```python
# Plot the mean years of schooling by survey year
plt.figure(figsize=(10, 6))
sns.lineplot(
    data=df_clean,
    x='year',
    y='educ',
    estimator='mean',
    errorbar=('ci', 95),
    marker='o'
)
plt.title('Mean Years of Schooling Over Time')
plt.xlabel('Survey Year')
plt.ylabel('Average Years of Education')
plt.grid(False)
plt.show()
```
This code visualizes how the average years of education reported by respondents has changed over time, using the cleaned GSS dataset. The line plot includes 95% confidence intervals to indicate the uncertainty around the mean estimate for each year.
<div class = "row">
    <div class = "col-sm mt-3 mt-md-0">
        {% include figure.liquid loading ="eager" path ="assets/img/gss_educ_all.png" title ="Line Graph of Educ Over Time" class ="img-fluid rounded z-depth-1" %}
    </div>
</div>
<h6><b>Interpretation:</b></h6>

<p>
 At a baseline, we see how the main concern when trying to parse out the relationship between any independent variable and changes in educational attainment is the influence that the mere passage of time has.
</p>
<p>
  Running the 
  <a class="btn btn-link p-0" data-bs-toggle="collapse" href="#UnconditionalCollapse" role="button" aria-expanded="false" aria-controls="UnconditionalCollapse">
    <b>unconditional model</b>
  </a> 
  highlights a significant relationship between time and mean years of education. Particularly, a one-unit increase in year is associated with a .055 increase in reported education scores (<i>p</i> < .001). However, as noted by the condition number, the regression suffers from high multicollinearity. That is to say, year may too strongly predict changes in education.
</p>
<div class="collapse" id="UnconditionalCollapse">
  <div class="card card-body border-0 p-0 mt-3">
    <div class="row justify-content-center">
      <div class="col-md-6 text-center">
        {% include figure.liquid 
           loading="eager" 
           path="assets/img/gss_unc_m0.png" 
           title="Fixed Effects Results" 
           class="img-fluid rounded z-depth-1" %}
      </div>
    </div>
  </div>
</div>
<h6>Let's Break it Down by Region:</h6>
<div class="row justify-content-sm-center">
  <!-- Image 1 -->
  <div class="col-sm-6 col-md-6 mt-3 mt-md-0">
    <div class="ratio ratio-4x3">
      <img src="{{ 'assets/img/gss_all_reg_ed.png' | relative_url }}" class="img-fluid rounded z-depth-1" alt="Distribution" style="object-fit: contain;">
    </div>
  </div>
  <!-- Image 2 -->
  <div class="col-sm-6 col-md-6 mt-3 mt-md-0">
    <div class="ratio ratio-4x3">
      <img src="{{ 'assets/img/gss_educ_simp.png' | relative_url }}" class="img-fluid rounded z-depth-1" alt="Correlation Matrix" style="object-fit: contain;">
    </div>
  </div>
</div>
<div class="caption">
    The image on the left shows the broader region classifications, as defined by the US Census Bureau. The image on the right is the simplified version of that categorization system. Both images show a persistent trend of the South reporting years of schooling lower than the other regions and a general increase the average years of schooling attained as time progresses. 
</div>
