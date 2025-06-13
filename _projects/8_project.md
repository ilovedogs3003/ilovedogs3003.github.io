---
layout: page
title: Education by Region
description: Educational Modeling with Longitudinal Data
img: assets/img/gss_all_reg_ed.png
importance: 5
category: Python
related_publications: true
code_url: https://drive.google.com/file/d/1_F1ssjQa_4PZLKqFrf9S5aYKSt7SqJ6I/view?usp=sharing
code_label: Open in Colab
---
<hr>
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
<p>The linked Jupyter Notebook provides a more detailed overview of the data cleaning and handling methods. One particular component that is a bit nebulous is how ```reg_broad_simp``` is defined and used, as that is just my personal file of customized functions.</p>
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
These simple lines of code will highlight the dependent variable of interest, along with some possible concerns:
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
<h6>Breaking it Down by Region:</h6>
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
    The image on the left shows the more specific region classifications, as defined by the US Census Bureau. The image on the right is the simplified version of that categorization system. Both images show a persistent trend of the South reporting years of schooling lower than the other regions and a general increase the average years of schooling attained as time progresses. 
</div>
<h4>OLS v. Panel OLS Regressions</h4>
Albeit seldom discussed, there are a myriad of factors data scientist must consider when deciding on a methodology and interpreting the output. Previous labs of mine have demonstrated the extent to which sampling can influence the outcomes; <b>this lab highlights key differences in two similar analytical methods when dealing with cross-sectional panel data.</b>
<h5><b>OLS Regression Outputs</b></h5>
<p>
    The first set of regressions included four models: <b>Model 1</b>, <b>Model 1 YC</b>, <b>Model 2</b>, and <b>Model 2 YC</b>.
</p>
<details>
    <summary><i>Click to View Model Set-Up</i></summary>
    <pre><code>import statsmodels.formula.api as smf

# Model 1: Uses simplified region categories and includes year as a categorical variable
model1 = smf.ols(
        formula="educ ~ incom16 + sex + age + region_midwest + region_south + region_west + C(year)",
        data=df_clean_dummies
).fit()

# Center the year variable at 1972 for models using a continuous year
df_clean_dummies['year_centered'] = df_clean_dummies['year'] - 1972

# Model 1 YC: Same as Model 1, but year is continuous and centered
model1_yc = smf.ols(
        formula="educ ~ incom16 + sex + age + region_midwest + region_south + region_west + year_centered",
        data=df_clean_dummies
).fit()

# Model 2: Uses specific region categories and includes year as a categorical variable
model2 = smf.ols(
        formula="educ ~ age + sex + incom16 + C(reg16) + C(year)",
        data=df_clean_dummies
).fit()

# Model 2 YC: Same as Model 2, but year is continuous and centered
model2_yc = smf.ols(
        formula="educ ~ age + sex + incom16 + C(reg16) + year_centered",
        data=df_clean_dummies
).fit()
    </code></pre>
</details>
<div style="height: 8px;"></div>
<p>
    <b>Model 1</b> and <b>Model 2</b> both regress age, sex, income, and year variables on reported years of schooling (<code>educ</code>). The difference is that <b>Model 1</b> uses simplified region categories (broader groupings), while <b>Model 2</b> uses more specific region categories. <b>Model 1 YC</b> and <b>Model 2 YC</b> are the same as their respective base models, except they replace the year variable with a continuous, centered-at-1972 year variable.
</p>
<details>
  <summary><i>Click to view output</i></summary>
  <div style="max-height: 500px; overflow-y: auto; background: #f9f9f9; padding: 10px; border: 1px solid #ccc;">
    <pre><code>
==================================================================
                        Model 1   Model 1 YC  Model 2   Model 2 YC
------------------------------------------------------------------
Intercept              11.2636*** 11.5019*** 11.2637*** 11.5028***
                       (0.0979)   (0.0705)   (0.1086)   (0.0848)  
region_midwest[T.True] -0.2712*** -0.2679***                      
                       (0.0337)   (0.0337)                        
region_south[T.True]   -1.0207*** -1.0262***                      
                       (0.0328)   (0.0329)                        
region_west[T.True]    -0.2776*** -0.2740***                      
                       (0.0395)   (0.0395)                        
C(year)[T.1973]        0.2295**              0.2271**             
                       (0.1039)              (0.1038)             
C(year)[T.1974]        0.4673***             0.4660***            
                       (0.1037)              (0.1037)             
C(year)[T.1975]        0.3513***             0.3545***            
                       (0.1035)              (0.1035)             
C(year)[T.1976]        0.3900***             0.3908***            
                       (0.1038)              (0.1038)             
C(year)[T.1977]        0.3439***             0.3434***            
                       (0.1034)              (0.1034)             
C(year)[T.1978]        0.5569***             0.5556***            
                       (0.1031)              (0.1031)             
C(year)[T.1980]        0.6827***             0.6891***            
                       (0.1044)              (0.1044)             
C(year)[T.1982]        0.6467***             0.6542***            
                       (0.0985)              (0.0985)             
C(year)[T.1983]        0.9380***             0.9451***            
                       (0.1022)              (0.1021)             
C(year)[T.1984]        1.0145***             1.0255***            
                       (0.1042)              (0.1042)             
C(year)[T.1985]        1.0692***             1.0785***            
                       (0.1031)              (0.1030)             
C(year)[T.1986]        0.9840***             0.9964***            
                       (0.1044)              (0.1044)             
C(year)[T.1987]        1.0476***             1.0638***            
                       (0.0989)              (0.0989)             
C(year)[T.1988]        1.1594***             1.1628***            
                       (0.1043)              (0.1042)             
C(year)[T.1989]        1.3682***             1.3722***            
                       (0.1034)              (0.1033)             
C(year)[T.1990]        1.5285***             1.5337***            
                       (0.1064)              (0.1063)             
C(year)[T.1991]        1.5260***             1.5315***            
                       (0.1036)              (0.1036)             
C(year)[T.1993]        1.6952***             1.7027***            
                       (0.1022)              (0.1021)             
C(year)[T.1994]        1.7250***             1.7243***            
                       (0.1035)              (0.1034)             
C(year)[T.2002]        2.2127***             2.2166***            
                       (0.0983)              (0.0982)             
C(year)[T.2004]        2.4322***             2.4295***            
                       (0.0929)              (0.0929)             
C(year)[T.2006]        2.3547***             2.3501***            
                       (0.0908)              (0.0908)             
C(year)[T.2008]        2.3009***             2.2948***            
                       (0.0979)              (0.0979)             
C(year)[T.2010]        2.3253***             2.3247***            
                       (0.0975)              (0.0975)             
C(year)[T.2012]        2.4380***             2.4338***            
                       (0.0987)              (0.0986)             
C(year)[T.2014]        2.5821***             2.5839***            
                       (0.0935)              (0.0935)             
C(year)[T.2016]        2.5646***             2.5657***            
                       (0.0910)              (0.0910)             
C(year)[T.2018]        2.6426***             2.6386***            
                       (0.0947)              (0.0947)             
C(year)[T.2021]        3.5846***             3.5844***            
                       (0.0871)              (0.0871)             
C(year)[T.2022]        2.9622***             2.9568***            
                       (0.0886)              (0.0886)             
C(year)[T.2024]        3.0656***             3.0750***            
                       (0.0893)              (0.0893)             
incom16                0.6892***  0.6910***  0.6863***  0.6881*** 
                       (0.0135)   (0.0135)   (0.0135)   (0.0135)  
sex                    -0.1285*** -0.1261*** -0.1270*** -0.1244***
                       (0.0236)   (0.0236)   (0.0236)   (0.0236)  
age                    -0.0254*** -0.0254*** -0.0253*** -0.0253***
                       (0.0007)   (0.0007)   (0.0007)   (0.0007)  
year_centered                     0.0582***             0.0581*** 
                                  (0.0007)              (0.0007)  
C(reg16)[T.2.0]                              -0.0024    0.0024    
                                             (0.0605)   (0.0606)  
C(reg16)[T.3.0]                              -0.3054*** -0.3003***
                                             (0.0587)   (0.0588)  
C(reg16)[T.4.0]                              -0.1964*** -0.1842***
                                             (0.0661)   (0.0662)  
C(reg16)[T.5.0]                              -0.9220*** -0.9270***
                                             (0.0601)   (0.0602)  
C(reg16)[T.6.0]                              -1.3237*** -1.3258***
                                             (0.0677)   (0.0678)  
C(reg16)[T.7.0]                              -0.9577*** -0.9539***
                                             (0.0652)   (0.0653)  
C(reg16)[T.8.0]                              -0.4011*** -0.3863***
                                             (0.0749)   (0.0750)  
C(reg16)[T.9.0]                              -0.2230*** -0.2193***
                                             (0.0638)   (0.0639)  
R-squared              0.1827     0.1784     0.1839     0.1796    
R-squared Adj.         0.1822     0.1783     0.1833     0.1794    
N                      57146      57146      57146      57146     
R-squared              0.1827     0.1784     0.1839     0.1796    
==================================================================
Standard errors in parentheses.
* p<.1, ** p<.05, ***p<.01
Model 1 = Simplified region categories
Model 2 = More specific region categories
YC = Same set up except year is now centered at 1972 and continuous

    </code></pre>
  </div>
</details>
<div style="height: 8px;"></div>
<p>
    Not surprisingly, all four models produce relatively similar results, with the primary differences occurring within models rather than between them. For instance, both <b>Model 1</b> and <b>Model 2</b> estimate the intercept at 11.26, while <b>Model 1 YC</b> and <b>Model 2 YC</b> estimate it at 11.50. If you were to run these regressions without centering the year variable, you would see an intercept of roughly -100. Although this does not affect the substance, explanatory power, or predictive capabilities of the model, it highlights the importance of appropriately handling the time variable in your analysis.
</p>
<p>
    The decision to treat the year variable as either <strong>categorical</strong> or <strong>continuous</strong> depends on the assumptions you make about how time influences your outcome variable.
</p>

<b>Continuous Year</b> (e.g., <code>+ year</code>)
<ul>
    <li><strong>Assumes a linear trend</strong>: Each additional year changes the outcome by a constant amount.</li>
    <li><strong>More parsimonious</strong>: Uses a single coefficient, which saves degrees of freedom.</li>
    <li><strong>Best when</strong> the outcome changes gradually or linearly over time.</li>
    <li><strong>Limitation</strong>: Can be misleading if the trend is nonlinear, stepwise, or volatile.</li>
</ul>

<b>Categorical year</b> (e.g., <code>+ C(year)</code>)
<ul>
    <li><strong>Gives each year its own coefficient</strong>: Allows for flexible time fixed effects.</li>
    <li><strong>Captures nonlinearities and unobserved shocks</strong> (e.g., policy changes, recessions).</li>
    <li><strong>Preferred in panel data and repeated cross-sections</strong>, especially with fixed effects or DiD models.</li>
    <li><strong>Limitation</strong>: Uses more degrees of freedom and can be harder to interpret individually.</li>
</ul>

<b>Recommendation for Cross-Sectional Longitudinal Data</b>
<p>
    When working with repeated cross-sections or panel data, it is generally best to treat year as a <strong>categorical variable</strong>. This allows you to control for year-specific effects and avoids making restrictive assumptions about the shape of time trends.
</p>
<p>
    However, if your dataset spans few years and you believe the effect of time is linear, a continuous specification may suffice.
</p>
<h5><b>Panel OLS Regression Outputs</b></h5>
<p>
    Panel OLS regression models extend standard Ordinary Least Squares (OLS) techniques to datasets that observe multiple entities (e.g., individuals, schools, states) over time. This type of data structure is known as <strong>panel data</strong> or <strong>longitudinal data</strong>.
</p>
<p>
    Panel OLS (or fixed effects regression) accounts for <strong>unobserved heterogeneity</strong>across entities that do not change over time and might otherwise bias results. This is accomplished by:
</p>
<ul>
    <li><b>Entity fixed effects:</b> Control for all time-invariant characteristics of each unit (e.g., baseline characteristics of individuals or regions).</li>
    <li><b>Time fixed effects:</b> Control for shocks or events that affect all entities in a given year (e.g., national policy changes, recessions).</li>
</ul>
<p>
    Our two Panel OLS Models are set up in the same fashion as OLS <b>Model 1</b> and OLS <b>Model</b> 2.
</p>
<details>
  <summary>Click to View Model Set-Up</summary>
  <pre><code>from linearmodels.panel import PanelOLS

# Model 1: Uses simplified region categories and includes year as a categorical variable
# Drop observations with NA values in any variable listed, but assign to a new DataFrame for panel regression
df_panel = df_clean_dummies.dropna(subset=['reg16', 'year', 'educ', 'age', 'sex', 'incom16', 'broad_region'])
df_panel = df_panel.set_index(['broad_region', 'year'])
# Declare independent variables, exclude fixed effects
exog_vars = ['age', 'sex', 'incom16']
exog = sm.add_constant(df_panel[exog_vars])
mod = PanelOLS(df_panel['educ'], exog, entity_effects=True, time_effects=True)
res = mod.fit()
...
**<>**              This is a separate Code Cell                **<>**
# Model 2 Uses the more-specific region categories and includes year as a categorical variable
df_panel.reset_index(inplace=True)  # Reset index to prepare for region-specific regression
df_panel = df_panel.set_index(['reg16', 'year'])
exog_vars = ['age', 'sex', 'incom16']
exog = sm.add_constant(df_panel[exog_vars])
mod2 = PanelOLS(df_panel['educ'], exog, entity_effects=True, time_effects=True)
res2 = mod2.fit()
  </code></pre>
</details>
<p>Running the Panel OLS regressions produces the following results:</p>
<div class="row">
  <!-- Left column -->
  <div class="col-md-6">
    <h5>Model 1 Output</h5>
    <pre><code>
                          PanelOLS Estimation Summary                           
================================================================================
Dep. Variable:                   educ   R-squared:                        0.0741
Estimator:                   PanelOLS   R-squared (Between):              0.1875
No. Observations:               57146   R-squared (Within):               0.0540
Date:                Thu, May 29 2025   R-squared (Overall):              0.0577
Time:                        06:48:34   Log-likelihood                -1.399e+05
Cov. Estimator:            Unadjusted                                           
                                        F-statistic:                      1522.8
Entities:                           4   P-value                           0.0000
Avg Obs:                    1.429e+04   Distribution:                 F(3,57108)
Min Obs:                       9022.0                                           
Max Obs:                    1.949e+04   F-statistic (robust):             1522.8
                                        P-value                           0.0000
Time periods:                      32   Distribution:                 F(3,57108)
Avg Obs:                       1785.8                                           
Min Obs:                       1285.0                                           
Max Obs:                       3353.0                                           
                                                                                
                             Parameter Estimates                              
==============================================================================
            Parameter  Std. Err.     T-stat    P-value    Lower CI    Upper CI
------------------------------------------------------------------------------
const          12.565     0.0644     195.24     0.0000      12.439      12.691
age           -0.0254     0.0007    -37.469     0.0000     -0.0267     -0.0241
sex           -0.1285     0.0236    -5.4427     0.0000     -0.1748     -0.0822
incom16        0.6892     0.0135     51.048     0.0000      0.6628      0.7157
==============================================================================

F-test for Poolability: 252.76
P-value: 0.0000
Distribution: F(34,57108)

Included effects: Entity, Time
    </code></pre>
  </div>

  <!-- Right column -->
  <div class="col-md-6">
    <h5>Model 2 Output</h5>
    <pre><code>
                          PanelOLS Estimation Summary                           
================================================================================
Dep. Variable:                   educ   R-squared:                        0.0734
Estimator:                   PanelOLS   R-squared (Between):              0.2431
No. Observations:               57146   R-squared (Within):               0.0532
Date:                Thu, May 29 2025   R-squared (Overall):              0.0578
Time:                        06:58:52   Log-likelihood                -1.399e+05
Cov. Estimator:            Unadjusted                                           
                                        F-statistic:                      1507.3
Entities:                           9   P-value                           0.0000
Avg Obs:                       6349.6   Distribution:                 F(3,57103)
Min Obs:                       2798.0                                           
Max Obs:                    1.187e+04   F-statistic (robust):             1507.3
                                        P-value                           0.0000
Time periods:                      32   Distribution:                 F(3,57103)
Avg Obs:                       1785.8                                           
Min Obs:                       1285.0                                           
Max Obs:                       3353.0                                           
                                                                                
                             Parameter Estimates                              
==============================================================================
            Parameter  Std. Err.     T-stat    P-value    Lower CI    Upper CI
------------------------------------------------------------------------------
const          12.565     0.0643     195.32     0.0000      12.439      12.691
age           -0.0253     0.0007    -37.284     0.0000     -0.0266     -0.0240
sex           -0.1270     0.0236    -5.3806     0.0000     -0.1732     -0.0807
incom16        0.6863     0.0135     50.831     0.0000      0.6598      0.7127
==============================================================================

F-test for Poolability: 222.67
P-value: 0.0000
Distribution: F(39,57103)

Included effects: Entity, Time
    </code></pre>
  </div>
</div>
<p>
  <b>Interpretation of PanelOLS Models:</b>
</p>
<p>
  The estimated coefficients across both models are remarkably similar. For instance, <code>age</code> is negatively associated with <code>educ</code> at approximately −0.025, <code>sex</code> is associated with a roughly 0.127 to 0.129 decrease in educational attainment (i.e., men have lower educational attainment, on average), and <code>incom16</code> has a strong positive effect of around 0.686–0.689, all statistically significant at the 0.001 level.
</p>
<p>
  The key difference lies in how the entity effects are structured:
  <ul>
    <li><b>Model 1</b> uses <code>broad_region</code> (n=4) as the entity index, capturing more aggregated regional variation.</li>
    <li><b>Model 2</b> uses <code>reg16</code> (n=9), allowing for more fine-grained control over regional fixed effects.</li>
  </ul>
  This change affects the <code>R-squared (Between)</code> substantially—<b>Model 2</b> explains 24.31% of between-entity variation compared to 18.75% in <b>Model 1</b>, suggesting that the more specific regional breakdown captures additional variation in average education levels across regions. This highlights a principle of geography: all things are related, but near things are more related than distant things.
</p>
<p>
  While the <code>R-squared (Within)</code> and <code>Overall</code> values are nearly identical, the slightly higher F-statistic in <b>Model 1</b> (1522.8 vs. 1507.3) suggests marginally better in-sample explanatory power when aggregating to broader regional units. However, the practical difference between the two is minimal. Both models offer robust support for the same conclusions regarding age, sex, and income’s influence on education.
</p>

<h4><b>Key Takeaways</b></h4>
<ul>
    <li><b>Time matters:</b> Both OLS and Panel OLS models show a clear upward trend in educational attainment over time, highlighting the importance of accounting for temporal effects.</li>
    <li><b>Regional differences:</b> The South consistently reports lower average years of schooling compared to other regions, regardless of model specification.</li>
    <li><b>Model choice impacts interpretation:</b> Treating year as a categorical variable captures more nuanced, year-specific effects, while a continuous specification assumes a linear trend.</li>
    <li><b>Panel OLS advantages:</b> By controlling for unobserved, time-invariant characteristics at the region level, Panel OLS models provide more robust estimates and help mitigate omitted variable bias.</li>
    <li><b>Consistent predictors:</b> Age, sex, and perceived income at age 16 are significant predictors of educational attainment across all models.</li>
    <li><b>Granularity matters:</b> Using more specific regional categories (Model 2) explains more between-region variation than broader categories (Model 1).</li>
</ul>
<!--
<p>How are these different from the Regular OLS Regressions?</p>
<ul>
    <li><b>OLS assumes independence:</b> Standard OLS does not account for repeated observations of the same unit. This can lead to biased estimates if entity-level traits influence the outcome.</li>
    <li><b>Panel OLS controls for unobserved, fixed traits:</b> By absorbing entity-specific and/or time-specific effects, Panel OLS provides more accurate causal estimates, especially when omitted variable bias is a concern.</li>
    <li><b>OLS uses cross-sectional variation only:</b> Panel OLS leverages both <i>within-unit</i> and <i>across-time</i> variation.</li>
</ul>
<p>
    While the GSS can generally be analyzed with OLS and Fixed Effects estimates, as highlighted by
    <small>
        <i>
        <a href="https://indem.uc3m.es/pdf/1529497293-Mahlendorf1.pdf" target="_blank" rel="noopener">
        Vaisey and Miles (2017),
      </a>
    </i>
  </small>
    the context of this project's question of interest warrants a Panel OLS approach
    --that being regional differences in educational attainment--a Panel OLS approach opperates in a similar fashion.
</p>
-->




