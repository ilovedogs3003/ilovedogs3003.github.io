---
layout: page
title: Supervised Machine Learning
description: KNN, OLS, Ridge and Lasso Regressions in Educational Modeling
img: assets/img/ml2_univariate_pairplot.png
importance: 3
category: Python
related_publications: true
code_url: https://colab.research.google.com/drive/1-1-iBaIiSke0XXZ4rbwetS5zAmVokZ-p?usp=sharing
code_label: Open in Colab
---
<hr>
<p>
    This project applies principles of machine learning to educational data from the <em>Caschool</em> dataset, part of the <code>Ecdat</code> package in R. It contains data on public elementary schools in California, originally compiled for educational economics research. Key variables include average test scores, student–teacher ratios, enrollment, income, and poverty indicators. The data is commonly used to study the relationship between school resources and student performance.
</p>
<div style="text-align: right; font-size: 0.9em;">
  <i><strong>Source:</strong> Wooldridge, J. M. (2016). <em>Introductory Econometrics: A Modern Approach</em>. Dataset available via the 
  <a href="https://vincentarelbundock.github.io/Rdatasets/csv/Ecdat/Caschool.csv" target="_blank">Rdatasets repository</a>.</i>
</div>
<details>
  <summary style="text-align: right; cursor: pointer;"><i>Click to view the variables within the dataframe</i></summary>
  <ul style="text-align: left; padding-left: 1.5em;">
    <li><code>distcod</code>: district code, integer</li>
    <li><code>county</code>: county, integer</li>
    <li><code>district</code>: name of district, object</li>
    <li><code>grspan</code>: grades served in each school, object</li>
    <li><code>enrltot</code>: total number of students enrolled, int64</li>
    <li><code>teachers</code>: total number of teachers, float64</li>
    <li><code>calwpct</code>: percent qualifying for income assistance, float64</li>
    <li><code>mealpct</code>: percent qualifying for free or reduced lunch, float64</li>
    <li><code>computer</code>: computers available to the students, int64</li>
    <li><code>testscr</code>: California standardized test score average, float64</li>
    <li><code>compstu</code>: computers per student, float64</li>
    <li><code>expnstu</code>: expenses per student, float64</li>
    <li><code>str</code>: student-teacher ratio, float64</li>
    <li><code>avginc</code>: average income at the school, float64</li>
    <li><code>elpct</code>: percent of students that are English language learners, float64</li>
    <li><code>readscr</code>: reading scores, float64</li>
    <li><code>mathscr</code>: math scores, float64</li>
  </ul>
</details>
<div style="height: 4px;"></div>
<h4 style="color: #1e90ff;">Understanding the Data</h4>
<p>
    The dependent variable of interest, <strong>testscr</strong>, is a combination of reading and math scores on the Stanford 9 standardized test administered to 5th grade students.
</p>
<p>
    The three predictor variables I will be focusing on include:
</p>
<ol>
    <li><code>avginc</code> – district average income</li>
    <li><code>elpct</code> – percent of English learners</li>
    <li><code>calwpct</code> – percent qualifying for income assistance</li>
</ol>
<strong>NOTE:</strong> The assignment required selecting just three variables that might be related to the target feature. While many variables could plausibly influence test performance, I have chosen <code>avginc</code>, <code>elpct</code>, and <code>calwpct</code> because they <b>circumvent the idea of wealth as a metric for student success.</b>
<div style="margin-bottom: 4px; width: 100%; text-align: left; margin-bottom: 0;">
  <iframe src="https://ilovedogs3003.github.io/lfs/html/ml2_descriptive_stats_apa.html"
          width="100%" height="400px" style="border: none; display: block; margin: 0; padding: 0;" loading="eager"></iframe>
</div>
<h4 style="color: #1e90ff;"><b>Univariate and Bivariate Distributions</b></h4>
<details>
  <summary style="text-align: right; cursor: pointer;"><i>Click to View the Code</i></summary>
  <pre><code class="language-python">
import seaborn as sns

sns.set(style="ticks", color_codes=True)
g = sns.pairplot(df_select[['testscr', 'avginc', 'elpct', 'calwpct']], height=2.2)

# Relabel axes titles for readability
labels = ['Test Score', 'Avg. Income', 'English Learners %', 'Income Assistance %']

for i, label in enumerate(labels):
    g.axes[i, 0].set_ylabel(label)  # Leftmost column (Y-axis labels)
    g.axes[-1, i].set_xlabel(label)  # Bottom row (X-axis labels)
  </code></pre>
</details>
<div class="row align-items-center">
    <div class="col-md-7">
        {% include figure.liquid loading="eager" path="assets/img/ml2_univariate_pairplot.png" title="univariate distributions" class="img-fluid rounded z-depth-0" style="max-width: 100%;" %}
    </div>
    <div class="col-md-5">
        <p>
            While I could have done each visualization independently through <code>ggplot</code>, the <code>seaborn</code> package allows for much more efficient and cohesive comparisons. The figure to the left shows the distribution of and dependency of each variable. Some things to take into consideration include <b>the lack of normal distribution</b> within most variables, and what appears to be a <b>non-linear relationship</b> between test scores and the independent variables.
        </p>
    </div>
</div>
<h4 style="color: #1e90ff;">Evaluating KNN, OLS, Ridge, and Lasso Regressions</h4>
<p>
  This section compares the performance of four regression models—<strong>Ordinary Least Squares (OLS)</strong>, 
  <strong>K-Nearest Neighbors (KNN)</strong>, <strong>Ridge</strong>, and <strong>Lasso</strong>—in predicting test scores based on school-level characteristics. 
  Each model offers a distinct approach to handling feature relationships and complexity:
</p>
<ul>
  <li><strong>OLS (Linear Regression)</strong>: Serves as the baseline model, assuming linear relationships between predictors and the outcome.</li>
  <li><strong>KNN Regression</strong>: A non-parametric model that predicts outcomes based on the average of nearby observations. </li>
  <li><strong>Ridge Regression</strong>: A regularized version of OLS that penalizes large coefficients to reduce overfitting, particularly useful when predictors are correlated.</li>
  <li><strong>Lasso Regression</strong>: Similar to Ridge but uses L1 regularization, which can shrink some coefficients to zero, effectively performing feature selection.</li>
</ul>
<p>
  Model performance is assessed using three metrics: the average cross-validation R² score (i.e., mean performance across a different number of iterations), training R² (i.e,the predictive performance of the model on the training dataset), and testing R² (the accuracy of the trained model on data that it has never seen before). These scores help evaluate each model's generalizability and overfitting risk (i.e., our estimates being too specific to the data we train the model on).
</p>
<details>
  <summary style="text-align: right; cursor: pointer;"><i>Click to View the Code</i></summary>
  <pre><code class="language-python">
#relevant libraries
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import train_test_split
from sklearn.neighbors import KNeighborsRegressor
from sklearn.model_selection import cross_val_score
from sklearn.linear_model import Ridge
from sklearn.linear_model import Lasso
import numpy as np

X = df_raw[['teachers','enrltot','avginc','elpct','calwpct', 'mealpct', 'computer', 'compstu', 'expnstu', 'str']]
y = df_raw['testscr']

#split into training and test
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=42)

# initialize models
models = {
    "Linear Regression": LinearRegression(),
    "KNN Regression": KNeighborsRegressor(n_neighbors=5),
    "Ridge Regression": Ridge(),
    "Lasso Regression": Lasso()
}

# dictionary to store results
comparison_data = {
    "Model": [],
    "Mean Cross-Validation Score": [],
    "Train R²": [],
    "Test R²": []
}

# loop through models to compute metrics
for name, model in models.items():
    model.fit(X_train, y_train)
    train_r2 = model.score(X_train, y_train)
    test_r2 = model.score(X_test, y_test)
    mean_cv_score = np.mean(cross_val_score(model, X_train, y_train, cv=10))

    # append results
    comparison_data["Model"].append(name)
    comparison_data["Mean Cross-Validation Score"].append(mean_cv_score)
    comparison_data["Train R²"].append(train_r2)
    comparison_data["Test R²"].append(test_r2)

# convert dictionary to DataFrame
comparison_df = pd.DataFrame(comparison_data)
print(comparison_df)
  </code></pre>
</details>
<div style="width: 100%; text-align: left;">
  <iframe src="https://ilovedogs3003.github.io/lfs/html/ml2_regression_comparison.html"
          width="100%" height="250" style="border: none; display: block; margin: 0;" loading="eager"></iframe>
</div>
<p>
  <b>Interpretation:</b> Every model other than KNN appears to be a better choice in this case, a result which is not surprising. KNN often struggles when:
</p>
<ul>
  <li>A relatively large number of input variables are used, increasing the risk of noise and reducing the effectiveness of distance-based comparisons (a symptom of the "curse of dimensionality").</li>
  <li>The number of observations is limited, making it harder to find reliable "neighbors," which can bias the predicted values.</li>
</ul>
<p>
 The other three models (OLS, Ridge, and Lasso) each achieved high and nearly identical scores across all metrics. Their cross-validation R² (~0.78), training R² (~0.805), and test R² (~0.807) suggest strong generalization without signs of overfitting. In contrast, the KNN model performed poorly, with a negative cross-validation R² and very low test R² (0.085), indicating that it failed to capture meaningful patterns in the data.
</p>
<h5 style="color: #1e90ff;">Implementing Standard Scalers</h5>
<p>
    I am a big fan of KNN and its many variations for data imputation. In the next section, we use StandardScaler, which standardizes each feature by subtracting the mean and dividing by the standard deviation. This transforms each variable to have a mean of 0 and a standard deviation of 1, making comparisons across features with different units or scales more meaningful.
</p>
<p>This step is especially important for models like K-Nearest Neighbors and regularized regressions (Ridge, Lasso), which are sensitive to the scale of the input features; recall that test-score had a mean of 654.16 while the other variables were around ~15%.</p>

<details>
  <summary style="text-align: right; cursor: pointer;"><i>Click to View the Code</i></summary>
  <pre><code class="language-python">
from sklearn import preprocessing
# data was previously split into training and test datasets using
'''
X_train, X_test, y_train, y_test = train_test_split(X, y)
'''

# initialize models
scaled_models = {
    "Scaled Linear Regression": LinearRegression(),
    "Scaled KNN Regression": KNeighborsRegressor(n_neighbors=5),
    "Scaled Ridge Regression": Ridge(),
    "Scaled Lasso Regression": Lasso()
}

# dictionary to store results
scaled_comparison_data = {
    "Model": [],
    "Mean Cross-Validation Score": [],
    "Train R²": [],
    "Test R²": []
}

# scale the data
scaler = preprocessing.StandardScaler()
scaler.fit(X_train)
X_train_scaled = scaler.transform(X_train)
X_test_scaled = scaler.transform(X_test)

# loop through models to compute metrics
for name, model in scaled_models.items():
    mean_cv_score = np.mean(cross_val_score(model, X_train_scaled, y_train, cv = 10))
    model.fit(X_train_scaled, y_train) 
    train_r2 = model.score(X_train_scaled, y_train)  
    test_r2 = model.score(X_test_scaled, y_test)

    #append results
    scaled_comparison_data["Model"].append(name)
    scaled_comparison_data["Mean Cross-Validation Score"].append(mean_cv_score)
    scaled_comparison_data["Train R²"].append(train_r2)
    scaled_comparison_data["Test R²"].append(test_r2)

# convert & print
scaled_comparison_df = pd.DataFrame(scaled_comparison_data)
print(scaled_comparison_df)
  </code></pre>
</details>
<div style="width: 100%; text-align: left;">
  <iframe src="https://ilovedogs3003.github.io/lfs/html/ml2_scaled_comparison.html"
          width="100%" height="250" style="border: none; display: block; margin: 0;" loading="eager"></iframe>
</div>
<p> After standardizing, all models showed improved or consistent performance. Unlike the previous unscaled results, the KNN model now performs much better, with a cross-validation R² of 0.709 and a test R² of 0.769. This is expected, as KNN is highly sensitive to feature scale, and standardizing ensures all variables contribute equally to distance calculations.</p>
<h5 style="color: #1e90ff;">Comparing Coefficients</h5>
<p>This next section compares the coefficients of the two regression models that performed the best: <strong>OLS</strong> and <strong>Lasso</strong>. This summary skipped a step from the codebook where <code>GridSearchCV</code> was used to fine-tune the parameters of the regression models, ultimately showing that these two outperformed the others.</p>
<details>
  <summary style="text-align: right; cursor: pointer;"><i>Click to View the Code</i></summary>
  <pre><code class="language-python">
#--*--Part Six: Compare Coefficients--*--
#retrieve the best Ridge and Lasso models
best_lasso = GridSearchCV(make_pipeline(StandardScaler(), Lasso()),
                          {'lasso__alpha': [0.1, 1, 10, 100]}, cv=10).fit(X_train, y_train).best_estimator_

#extract coefficients
lasso_coefs = best_lasso.named_steps["lasso"].coef_  # Extract Lasso coefficients
linear_coefs = LinearRegression().fit(X_train, y_train).coef_  # Fit Linear Regression

#so you can read them
readable_names = {
    "teachers": "Number of Teachers",
    "enrltot": "Total Enrollment",
    "avginc": "Average Income",
    "elpct": "English Learners %",
    "calwpct": "Income Assistance %",
    "mealpct": "Free/Reduced Lunch %",
    "computer": "Total Computers",
    "compstu": "Computers per Student",
    "expnstu": "Expenditure per Student",
    "str": "Student–Teacher Ratio"
}

#create df to compare
coef_df = pd.DataFrame({
    "Feature": X_train.columns.map(readable_names),
    "Linear": linear_coefs,
    "Lasso": lasso_coefs
})
print(coef_df)
  </code></pre>
</details>
<div style="width: 100%; text-align: left;">
  <iframe src="https://ilovedogs3003.github.io/lfs/html/ml2_coef_comp.html"
          width="100%" height="450" style="border: none; display: block; margin: 0;" loading="eager"></iframe>
</div>
<p>
  <strong>Interpretation:</strong> Both models find that higher average income, greater computer access, and increased per-student spending are positively associated with test scores. However, they differ in the estimated strength of these relationships. For instance, while the linear regression model estimates the coefficient for percent eligible for free or reduced lunch at -0.364, the Lasso regression estimates it at -9.93. These discrepancies are largely due to how the models treat variable importance: linear regression includes all predictors, whereas Lasso applies regularization to shrink or eliminate coefficients it deems less informative.
</p>

<p>
  <strong>Policy Implications:</strong> These results suggest that investments in school infrastructure and resources—such as technology access and instructional spending—may have a measurable impact on student performance. Moreover, the strong negative coefficient on free/reduced lunch eligibility in the Lasso model reinforces the idea that poverty remains a significant barrier to academic achievement.
  This highlights the need for targeted support in lower-income districts, not just through educational spending but also through broader socioeconomic interventions that reduce material hardship for students.
</p>

<h4 style="color: #1e90ff;">Key Takeaways</h4>
<ul style="line-height: 1.6;">
  <li><strong>Model Performance:</strong> Linear, Ridge, and Lasso regressions performed similarly well, with consistent R² scores across training, testing, and cross-validation. KNN initially underperformed but improved substantially after standardization.</li>

  <li><strong>Standardization Matters:</strong> Applying <code>StandardScaler</code> helped level the playing field for all models, especially KNN, which relies on distance-based calculations and is sensitive to variable scales.</li>

  <li><strong>Overfitting Not Observed:</strong> The similarity between training and test R² values across models suggests that overfitting was minimal, particularly for the regularized models (Ridge and Lasso).</li>

  <li><strong>Lasso’s Sparsity:</strong> Lasso Regression reduced the impact of less important features by shrinking their coefficients closer to zero, which aids interpretability and may prevent overfitting in more complex datasets.</li>

  <li><strong>Cross-Validation Validates Stability:</strong> Mean cross-validation scores aligned closely with test R² values, supporting the reliability and generalizability of the models.</li>
</ul>

