### Linear Regression

Linear Regression model is used to obtain relationship between a continuous outcome variable and one or more predictor variables. It is generally a good practice to understand the intution behind this approach. On the algorithm front, there are two techniques commonly used to optimize the parameters of the model, Normal Equation Method (uses Linear Algebra) and Gradient Descent (uses differential calculus, commonly used in a lot of Machine Learning Models). 

**Intution**

General equation for linear regression with n independent variables is, 

Y(x) = &theta;<sub>o</sub> + &theta;<sub>1</sub>x<sub>1</sub> + &theta;<sub>2</sub>x<sub>2</sub> + ... + &theta;<sub>n</sub>x<sub>n</sub> + &straightepsilon;

and the prediction equation is, 

Y&#770;(x) = h<sub>&theta;</sub>(x) = &theta;<sub>o</sub> + &theta;<sub>1</sub>x<sub>1</sub> + &theta;<sub>2</sub>x<sub>2</sub> + ... + &theta;<sub>n</sub>x<sub>n</sub> 

_where_, Y is actual dependent variable, 
x<sub>1</sub>, x<sub>2</sub>, .... , x<sub>n</sub> are independent variables, 
h<sub>&theta;</sub>(x) is predicted dependent variable,
&theta;<sub>o</sub>, &theta;<sub>1</sub>, ... , &theta;<sub>n</sub> are coefficients of independent variables and
&straightepsilon; is the error term.

The error metric considered here is Sum of Square of Error (SSE) because of which the model is also known as Ordinay Least Square (OLS). Our objective is to minimize SSE in order to reach as close to Y as possible.

SSE = &Sigma;(h<sub>&theta;</sub>(x) - Y(x))<sup>2</sup>

_Case 1:_

Consider the situation where we do not have any independent variables. One of the possible ways of prediction in this case would be to just take the average of Y. This can be suitably depicted using the above equations,

Y&#773; = h<sub>&theta;</sub>(x) = &theta;<sub>o</sub>, where &theta;<sub>o</sub> is a constant.

In this case, SSE is called Sum of Square Total, SST = &Sigma;(Y(x)-Y&#773;)<sup>2</sup> **(also called variance of Y)**

The error SST is considered as a reference for Linear Regression Models to be evaluated as we start including independent variables.
This is because, the error metric is supposed to decline with increase in variables (otherwise including the variable is useless).

_Case 2:_

After inclusion of independent variables, the error metric i.e. SSE = &Sigma;(Y(x)-h<sub>&theta;</sub>(x))<sup>2</sup> = &Sigma;(Y(x)-Y&#770;(x))<sup>2</sup>

Intutively, SST is the error of Y with no independent variable (Total variance) while SSE is the error of Y with independent variables included (Unexplained variance). From this, we can compute the variance that model in _case 2_ is able to explain compared to _case 1_. Let's call it SSR (Sum of Square of Regression also called explained variance). 

So, **SSR = SST-SSE**

![Linear Regression](https://github.com/abhisang32/abhisang32.github.io/blob/master/Linear_Regression_Help_files/Linear_Regression.png)

Now we can define another evaluation metric for our model i.e. R-Squared = (Explained Variance)/(Total Variance)

R<sup>2</sup> = SSR/SST = (SST-SSE)/SST = 1-(SSE/SST)

R<sup>2</sup> varies between 0 and 1. Higher the R<sup>2</sup> &#8658; better the model. **(Caution! Overfitting)**

Mathematically, R<sup>2</sup> may have negative values. However, when this happens, the error of the model is greater than SST, so the predicitons are worse than the average predictions with no independent variables and model is useless.

**Overfitting**

Once the model is built on the data (called as train data), we have to test it against unseen or out of sample data (called as test data) to verify that the patterns or equations discovered apply to them as well. Often, it so happens that while trying to learn the relationship, we end up learning the noise present in the data as well. This leads to really good results for the train data but when applied to test data, the result declines. 

In order to prevent overfitting, instead of relying on R<sup>2</sup> alone (as R<sup>2</sup> will always increase as we increase no. of variables), a new metric was designed, Adjusted R-Squared.

Adj R<sup>2</sup> = 1 - <sup>(1-R<sup>2</sup>)*(n-1)</sup>&frasl;<sub>(n-p-1)</sub>, _where_, n is no. of data points and p is no. of variables.

Looking at the equation, as p increases &#8658; R<sup>2</sup> increases &#8658; Adj R<sup>2</sup> increases,
On the other hand, as p increases &#8658; Adj R<sup>2</sup> decreases.
So, if increase in p does not increases R<sup>2</sup> as it should, Adj R<sup>2</sup> will decrease. Hence, Adj R<sup>2</sup> is often used as the metric to come up with optimum list of variables in the final model.

**Theory**

1. Normal Equation Method - 

This method is based on Linear Algebra. 

Y = X&beta; + &straightepsilon;

Matrix Representation - 

![Normal_Equation](https://github.com/abhisang32/abhisang32.github.io/blob/master/Linear_Regression_Help_files/Normal_Equation.PNG)

So, &straightepsilon; = Y - X&beta;

Now, our error metric SSE can be represented in matrix form as, &straightepsilon;&#884;&straightepsilon; = &straightepsilon;<sub>1</sub><sup>2</sup> + &straightepsilon;<sub>2</sub><sup>2</sup> + ... + &straightepsilon;<sub>n</sub><sup>2</sup> = &Sigma;&straightepsilon;<sub>i</sub><sup>2</sup>

Therefore, &straightepsilon;&#884;&straightepsilon; = (Y - X&beta;)&#884;(Y - X&beta;)

&#8658; &straightepsilon;&#884;&straightepsilon; = Y&#884;Y - Y&#884;X&beta; - &beta;&#884;X&#884;Y + &beta;&#884;X&#884;X&beta;

Here, 2nd and 3rd term on the RHS are transpose of each other. Also, they are scalar values with dimension (1 x 1). So, they are equal to each other.

&#8658; &straightepsilon;&#884;&straightepsilon; = Y&#884;Y - 2&beta;&#884;X&#884;Y + &beta;&#884;X&#884;X&beta;

Now, the above equation is a function of &beta; and our objective is to minimize it. In order to do this, we differentiate the equation with respect to &beta; and equate to Zero.

&#8658; <sup>&#x2202;(&straightepsilon;&#884;&straightepsilon;)</sup>&frasl;<sub>&#x2202;&beta;</sub> = -2X&#884;Y + 2X&#884;X&beta; = 0

&#8658; X&#884;X&beta; = X&#884;Y

&#8658; (X&#884;X)<sup>-1</sup>X&#884;X&beta; = (X&#884;X)<sup>-1</sup>X&#884;Y (Pre multiply by inverse of X&#884;X)

&#8658; I&beta; = (X&#884;X)&#884;X&#884;Y (I is the identity matrix)

**&#8658; &beta; = (X&#884;X)<sup>-1</sup>X&#884;Y**

If we analyze the above equation, the first term X&#884;X is the covariance matrix of independent variables while second term X&#884;Y is covariance between X and Y. This is when we consider the variables in their standardized form.

2. Gradient Descent - 

Gradient tells us the slope of a function at a given position. **It is a vector which always points towards the maximum increase of a function**. So, the general procedure is to define the objective function, randomly initialize parameters and gradually move in the opposite direction as that of the gradient in order to minimize the function's value.

![Gradient Descent](https://github.com/abhisang32/abhisang32.github.io/blob/master/Linear_Regression_Help_files/Gradient_Descent.png)

The Objective function we have is SSE. Let's denote it by J(&theta;) = <sup>1</sup>&frasl;<sub>2n</sub>&Sigma;{h<sub>&theta;</sub>(x<sup>(i)</sup>) - Y<sup>(i)</sup>}<sup>2</sup>; _where_ i = 1 to n. n is no. of observations and k is no. of variables.

The additional term 1&frasl;2 is for mathematical convenience while n is merely to make the error metric MSE(Mean Square Error). This would not affect the output that we get.

Gradient of J is defined as, <sup>&#x2202;J(&theta;)</sup>&frasl;<sub>&#x2202;&theta;<sub>j</sub></sub> = <sup>1</sup>&frasl;<sub>n</sub>&Sigma;{h<sub>&theta;</sub>(x<sup>(i)</sup>) - Y<sup>(i)</sup>}x<sup>(i)</sup><sub>j</sub>; _where_ j = 0 to k.

The update equation for &theta;,

Repeat untill Convergence,
&theta;<sub>j</sub>:= &theta;<sub>j</sub> - &alpha;<sup>&#x2202;J(&theta;)</sup>&frasl;<sub>&#x2202;&theta;<sub>j</sub></sub>; _where_ &alpha; is learning rate. 

&alpha; varies between 0 and 1 and needs to be optimized. After every update we calculate J(&theta;) and observe the change. If the change is less than the specified precision i.e. convergence is achieved, the algorithm stops.

**Note:** The update for the &theta;s happen simultaneously.

The process described above is called **Batch Gradient Descent**.

An alternate process is **Stochastic Gradient Descent** where we update &theta;s based on 1 data point at a time. The data points are chosen randomly(stochastic). Another alternate approach is **Mini Batch Gradient Descent** where we update &theta;s based on certain fixed no. of data points at a time.

**Code** 
- Gradient Descent

```
import numpy as np
import pandas as pd

class LinearRegression_Self:

    #get data
    def __init__(self,data,x_index,y_index):
        self.data = data #data
        self.x = data.iloc[:,x_index] #independent variables
        self.y = data.iloc[:,y_index] #dependent variable
        
    #get parameters    
    def fit(self,precision,learning_rate,max_iter):
        n_var = self.x.shape[1] #no. of variables
        n_obs = self.x.shape[0] #no. of observations
        theta = np.ones((n_var + 1)) #initializing parameters
        iters = 0 #initialize counter for iterations
        diff = 1 #initial values for change in objective function
        self.x = np.append(np.ones((n_obs,1)), self.x, axis=1) #include constant variable for constant term in equation
        J_new = (0.5/n_obs)*sum((np.array([sum([self.x[j,i]*theta[i] for i in range(n_var+1)]) for j in np.arange(n_obs)])-self.y)**2) # Evaluate Objective function
        while diff > precision and iters <= max_iter:
            J_curr = J_new
            dJ = np.array([(1/n_obs) * sum((np.array([sum([self.x[j,i]*theta[i] for i in range(n_var+1)]) for j in np.arange(n_obs)])-self.y)*self.x[:,i]) for i in np.arange(n_var+1)]) #gradient of objective function
            theta = theta - (learning_rate/n_obs)*dJ #Update parameters
            J_new = (0.5/n_obs)*sum((np.array([sum([self.x[j,i]*theta[i] for i in range(n_var+1)]) for j in np.arange(n_obs)])-self.y)**2) #Re-evaluate Objective function
            diff = abs(J_curr - J_new) #Update change in objective function value
            iters += 1 #update iteration no.
            print("Objective Function for Iteration ",iters," is ",J_new)
        return(theta) #Return Parameters 

```