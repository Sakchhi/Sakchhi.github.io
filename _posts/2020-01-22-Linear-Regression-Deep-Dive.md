Linear Regression Deep Dive
===========================

[![Sakchhi Srivastava](https://miro.medium.com/fit/c/96/96/0*i-KxHPykXfvUEaQg.jpg)](https://medium.com/@sakchhi.sri?source=post_page-----f266480da10d----------------------)[Sakchhi Srivastava](https://medium.com/@sakchhi.sri?source=post_page-----f266480da10d----------------------)FollowDraft · 5 min read

Linear Regression is one of the simplest and widely used models for regression modeling. It is models the relationship between a continuous, dependent variable Y and independent variable(s) X by fitting linear equation to the observed data.

**Simple Linear Regression** model identifies relationship between a independent feature and the dependent target variable. This relationship can be expressed as the equation for a straight line _viz_ **_y = b1.X + b0_**. **Mutiple Linear Regression** identifies the relationship of the target variable with more than 1 independent feature. The parameters _b0, b1, …, bn_ can be identified by minimizing the **_Residual Sum of Squares_** using **Ordinary Least Squares (OLS)** method.

Least Squares method minimizes the vertical deviation of predicted values from actual values. These error terms are called **Residuals.** Squaring the error ensures that positive and negative deviations do not cancel out.

<img class="cu t u gl ak" src="https://miro.medium.com/max/1000/1\*gNf4sUo5iEHOjgwnkuCZrw.png" width="500" height="331" role="presentation"/>

* * *

**Python Implementation**

We will explore Linear Regression in Python using **Student Performance data** from [**UCI-ML repository**](https://archive.ics.uci.edu/ml/datasets/Student+Performance). The dataset contains information about student achievement in secondary education of two Portuguese schools. The data attributes include student grades, demographic, social and school related features. For detailed data dictionary, visit the link. This is how the first few rows of the dataset look like.

<img alt="Sample of the data" class="cu t u gl ak" src="https://miro.medium.com/max/1488/1\*rtOPLdgv46Eg\_Q8ygNH87A.png" width="744" height="802"/>

Since the dataset contains categorical columns like Educational level of parents, gender of students, etc, we will [**One-Hot Encode**](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html)  these. This will automatically one-hot encode all the columns containing text data and create (n-1) new features, where n is the number of levels in the feature.

```
df\_transformed = pd.get\_dummies(df\_raw, drop\_first=True)
```

Some features like health, travel time for students are ordinal features, though they appear as numerical. So we [**ordinal encode**](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OrdinalEncoder.html) those feature using scikit-learn’s Ordinal Encoder class.

```
ordinal\_features = \['Medu', 'Fedu', 'traveltime', 'studytime', 'famrel', 'freetime', 'goout', 'Dalc', 'Walc', 'health'\]from sklearn.preprocessing import OrdinalEncoderord\_enc = OrdinalEncoder()df\_ord\_enc = df\_transformed.copy()  
df\_ord\_enc\[ordinal\_features\] = ord\_enc.fit\_transform(df\_transformed\[ordinal\_features\])
```

Splitting the dataset into test and train sample, with **Final Grade G3** as target feature, we get the final training set for our Linear Regression model.

```
from sklearn.model\_selection import train\_test\_splitX\_train, X\_test, y\_train, y\_test = train\_test\_split(df\_ord\_enc.drop(columns=\[‘G3’\]), df\_ord\_enc.G3, test\_size=0.1)
```

Using the [**LinearRegression**](http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html) class from scikit-learn’s linear model, we fit the model on the training set.

```
from sklearn.linear\_model import LinearRegressionlin\_reg = LinearRegression()  
lin\_reg.fit(X\_train, y\_train)
```

We can then make predicitons for the test set.

```
y\_pred = lin\_reg.predict(X\_test)
```

LinearRegression class also has methods to checking the performance of the model using [**R-squared**](https://en.wikipedia.org/wiki/Coefficient_of_determination) metric. Simply speaking, R-square metric explains how much of the variance in the target variable is explained by the independent features. For a model that perfectly predicts the target value, it would be 1, and 0 for a model that always predicts a constant value. For our model, this metric is **~0.78**, which is a decent fit.

<img class="cu t u gl ak" src="https://miro.medium.com/max/1522/1\*o\_cFDHjDov2co2L7CyDe-Q.png" width="761" height="83" role="presentation"/>

* * *

**Assumptions**

Although it’s one of the most widely used algorithms for drawing powerful insights, the relationships are only valid if the data does not violate the assumptions of Linear Regression. Some of these assumptions are:

1.  The relationship between independent features and the dependent variable is **linear**.
2.  There is no [**auto-correlation**](https://en.wikipedia.org/wiki/Autocorrelation) in the data ie there is no correlation between the error terms. Time-series data is generally auto-correlated. [**Durbin-Watson test**](https://en.wikipedia.org/wiki/Durbin%E2%80%93Watson_statistic) can give us an estimate of the auto-collinearity in the data.
3.  The independent features should not be correlated. **Multi-collinearity** in data can skew the contribution of some features. **Correlation analysis** of the features can identify highly correlated features.
4.  The error terms should have constant variance ie data should be [**homoskedastic**](https://en.wikipedia.org/wiki/Homoscedasticity). This is generally due to presence of outliers in the data. A plot of residuals-vs-fitted values can be used to detect heteroskedasticity in the dataset.
5.  The error terms should be normally distributed. It is due to presence of unusual data points. This can be identified using [**QQ plots**](https://en.wikipedia.org/wiki/Q%E2%80%93Q_plot).

* * *

**Validation Metrics**

There are some standard metrics that can help evaluate how good the model is at predicting new data points. Most of these metrics are available in scikit-learn’s metrics package. We will discuss a few of these.

1.  **Mean Absolute Error** — It is the mean absolute difference between the predicted value and observed values. This gives an idea how off the predictons are from actual values on an average. It is easy to interpret as it is in the same units as the target variable.
2.  **Mean Squared Error** — It is the mean of the square of the error in prediction. Since the error terms are squared, it is not as interpretable as MAE.
3.  **Root Mean Square Error** — It is the square root of MSE. It is prefered when large errors are less desirable as it gives greater weight to large errors.
4.  **R-squared** — It measures the goodness-of-fit of the model. R-squared measures what proportion of variance in the target variable can be explained by the independent features. The maximum value can be 1 and values can be negative too.
5.  **Adjusted R-squared** — R-squared will keep increasing as we increase the number of features in the data. So, even if additional features are noise, R-squared will increase. Adjusted R-squared adjusts the R-squared value for the number of independent features being used in the model. The value increases only if addition of new feature improves the model more than expected by random chance, else the value decreases. This can be 1 for perfectly fitting model and can be negative for poorly fitted models.

* * *

In the next article we will look at few more statistics and plots that help us better understand regression models and our dataset using the Statsmodel package.

The code demonstrating the concepts is available on [Github](https://github.com/Sakchhi/Blog-Posts-Code/blob/master/Regression/Linear_Regression/Linear%20Regression.ipynb).
