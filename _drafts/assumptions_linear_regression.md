---
layout: post
comments: true
title: "Assumptions of Linear Regression"
date: 2019-06-29
---

In the last post, I described the workings of a Linear Regression Model. Now, with any model there is a randomness (error or residual) associated which we call the noise in the data. Usually, we can represent it as **Data = Formula + Noise**. For us to understand how good our model is, we should explore this randomness for any improvements possible.

General equation for linear regression with n independent variables is, 

Y(x) = &theta;<sub>o</sub> + &theta;<sub>1</sub>x<sub>1</sub> + &theta;<sub>2</sub>x<sub>2</sub> + ... + &theta;<sub>n</sub>x<sub>n</sub> + &epsilon;

Following are the assumptions that we should be checking in case of a Linear Regression Model:

1. Linearity - The independent variables should be having a linear relationship with the dependent variable. This is inherently because we are trying to build a linear model and any non linearity would not be taken into account by our linear equation. The implications can be severe deflections of predictions from actual values. This happens when model is used to extrapolate for data beyond range of data model was built on. This assumption can be verified by simply looking at the scatter plot between the variables.

![Scatter Plot Non Linear](/images/Assumptions_Linear_Regression/Scatter_plot_non_linear.png)

In case we find any non linear relationship appearing in the scatter plot like the one above, it should be used to apply transformations on the independent variable. Here, since the relationship appears quadratic, we will apply square root on the independent variable and check the scatter plot once again.

![Scatter Plot Non Linear](/images/Assumptions_Linear_Regression/Scatter_plot_linear.png)

Since, now the relationship is linear, we are good to include the variable in the model.

2. Normality of Residuals - The residuals of a linear regression model are supposed to follow normal distribution by mathematical definition. This can be checked by looking at the histogram of residuals from the model. 

Histogram, Q-Q Plot, Kolmogorov-Smirnov test

**insert histogram of normal distribution**

In case we dont get a normal distribution out of the histogram, it means the confidence intervals we get would not be much reliable but the model can still be considered for predictions.

3. Constant Error Variance of Residuals (Homoscedasticity) - The Residuals are also expected to be uniformly distributed across the data. This can checked by plotting residuals against the fitted values.

**insert graph**

if the variance of error is not constant, it means there is a scope of improvement in the model and we are missing out on an important factor. **improve this**

An ideal graph would look something like this.

**insert ideal graph**

4. Independence of Residuals - The basic assumption of this model is that the observations are not correlated with each other. This means if value of a particular data point is high, value for subsequent data points will also be high or vice versa for lower values. This can be checked by evaluating autocorrelation for the dependent variable or plotting residuals of the model in the order they appear in the data.

Durbin Watson Test

**insert graph**

In case we find a pattern appearing in this graph, that means there is a correlation present in the data and we should be using time series models instead.

Apart from these assumptions we should also check for presence of multicollinearity in the data.

**Multicollinearity**

This refers to the situation where the independent variables(predictors) are highly correlated among each other. The problem with this is, if we look at the equation of linear regression, we say a unit change in x<sub>1</sub> results in &theta;<sub>1</sub> amount of change in Y, _given_ other predictor variables are constant but, in presence of high correlation, there's a good chance (**correlation vs causation**) other variables won't remain constant. 

This can be checked by looking at the correlation matrix for the predictors. Another way of determining this would be to consider Variance Inflation Factor(VIF). VIF is calculated for each and every predictor without taking the outcome variable into consideration.

**Calculation of VIF :** 

Suppose we have 4 independent variables x<sub>1</sub>, x<sub>2</sub>, x<sub>3</sub> & x<sub>4</sub> and Y is the dependent variable. For obtaining VIF for x<sub>1</sub>, a linear regression model is built with x<sub>1</sub> as outcome and x<sub>2</sub>, x<sub>3</sub> & x<sub>4</sub> as predictors. Next, we look at the R Squared for this model. This will tell us if x<sub>1</sub> can be predicted by using other independent variables. This process is repeated for every predictor variable.

VIF = 1/(1-R<sup>2</sup>)

So, a R<sup>2</sup> of 0.9 would be equivalent to VIF of 10. The general thought is that, if a variable can be predicted using other predictor variables, then it is replacable in the linear regression equation.

Y(x) = &theta;<sub>o</sub> + &theta;<sub>1</sub>x<sub>1</sub> + &theta;<sub>2</sub>x<sub>2</sub> + ... + &theta;<sub>n</sub>x<sub>n</sub> + &epsilon;

and,
x<sub>1</sub> = &theta;<sub>ox<sub>1</sub></sub> + &theta;<sub>2x<sub>1</sub></sub>x<sub>2</sub> + ... + &theta;<sub>nx<sub>1</sub></sub>x<sub>n</sub> + &epsilon;<sub>x<sub>1</sub></sub> 

Replacing x<sub>1</sub> in Y,
Y(x) = &theta;<sub>o</sub> + &theta;<sub>1</sub>(&theta;<sub>ox<sub>1</sub></sub> + &theta;<sub>2x<sub>1</sub></sub>x<sub>2</sub> + ... + &theta;<sub>nx<sub>1</sub></sub>x<sub>n</sub> + &epsilon;<sub>x<sub>1</sub></sub>) + &theta;<sub>2</sub>x<sub>2</sub> + ... + &theta;<sub>n</sub>x<sub>n</sub> + &epsilon;

Once we have VIF calculated for all the predcitors, a threshold is decided beyond which we will start dropping variables from the model.
This a recursive process where the variable with highest VIF is dropped one at a time. After dropping a variable, VIF is recalculated and the process is repeated untill all the variable are within the decided VIF threshold. General considered threshold for VIF is 4 but this can vary based on business requirements and problem statement.

