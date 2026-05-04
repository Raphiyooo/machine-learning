# Statistical learning

## 1. $X$ and $Y$

Every statistical learning problem starts with data, which is divided into two main parts:
* **$X$ (The Inputs):** Also known as predictors/independent variables/features.
* **$Y$ (The Output):** Also known as the response/dependent variable.


We define our data using two dimensions: $n$ (number of observations) and $p$ (number of predictors).
* **The Predictor Vector:** For a single observation $i$, the input is a vector of $p$ features: 
    $$x_i = (x_{i1}, x_{i2}, \dots, x_{ip})^T$$
* **The Full Dataset:** We use $X = (X_1, X_2, \dots, X_p)$ to predict the response $Y$.


## 2. Important function + error

All of this statistical learning plays about estimating the true relationship between inputs and outputs:

$$Y = f(X) + \epsilon$$
* **$f$ (The True Function):** The fixed, unknown relationship.
* **$\epsilon$ (The Error Term):** A random variable independent of $X$

When we build a model, we create an estimate, $\hat{f}$, which gives us our prediction, $\hat{Y}$. Why is it never perfect?
$$E(Y - \hat{Y})^2 = [f(X) - \hat{f}(X)]^2 + Var(\epsilon)$$
* **Reducible Error ($[f(X) - \hat{f}(X)]^2$):** We can fix this by using a more appropriate statistical learning technique.

* **Irreducible Error ($Var(\epsilon)$):** Why is it irreducible? As written above $$Y = f(X) + \epsilon$$ Y always has a error in it, now matter how good our data ($X$) is, the prediction cant be perfect. We can never fix this because $X$ doesn't contain all the information in the universe.

> 💡 **Note:** Irreducible error is the variable that cant be predicted. For example if you build a model where it should predict how likely one person is to buy something, it could be that that person has had a bad day, therefore not buying a product but the other day deciding to buy it.

### Inference vs Prediction
Often one is interested in understanding the associaton between $Y$ and $X_1, X_2, \dots, X_p$, not really predicting the outcome, rather questions like: which predictors are associated with the response? Relationship between response and predictors, are more important. This is called inference. In comparison prediction is used when a mobile phone company builds a face ID feature and need to check if the user wanting to enter the phone is the owner or not.
**In short:**
* Inference: "How does $X_1$ affect $Y$?"
* Prediction: "What will $Y$ be, given $X$?

## 3. Estimating $f$

How do we actually build $\hat{f}$?
* We need to apply a statistical learning method. Find a function $\hat{f}$ such that $Y ≈ \hat{f}(X)$ for any $(X, Y)$

### Parametric Methods
1. We make an assumption for the shape for $f$ first. The most common assumption is that $f$ is linear:
$$f(X) = \beta_0 + \beta_1 X_1 + \beta_2 X_2 + \dots + \beta_p X_p$$
Instead of guessing a complex multi-dimensional curve, we only have to calculate $p+1$ coefficients (the $\beta$ values).

2. After the model was selected, we need an approach that uses the training data to train the model. (A lot of these will be discussed later, e.g. least squares)

**The Advantage of Parametric Methods**
It drastically simplifies the math. Instead of trying to calculate an complex function $f$, you only need to estimate a specific set of parameters (like $\beta_0, \beta_1, \dots, \beta_p$ in a linear model).

**The Disadvantage of Parametric Methods**
The model you choose will almost never perfectly match the true, unknown form of $f$. If your initial guess about the shape of the data is completely wrong, your estimate will be wrong as well. 

**The Trap of Overfitting**
To fix a poor parametric estimate, you might be thinking to use a highly flexible, complex model that can fit many different shapes. But there is a catch:
* Fitting a more flexible model requires estimating a much greater number of parameters.
* This leads to **overfitting**—a phenomenon where the complex model works *too* hard and follows the errors (or random noise) in the data far too closely. 

![overfitting/underfitting](https://miro.medium.com/v2/1*_7OPgojau8hkiPUiHoGK_w.png) <!-- picture from: https://medium.com/greyatom/what-is-underfitting-and-overfitting-in-machine-learning-and-how-to-deal-with-it-6803a989c76 , Mar 11, 2018, Anup Bhande-->

### Non-Parametric Methods
These make zero assumptions about the shape of $f$. They just try to get as close to the data points as possible.

**The Advantage of Non-Parametric Methods:**
The major advantage here is flexibility: by completely avoiding the assumption of a specific shape, they eliminate the need of guessing the wrong function  and can fit a much wider range of possible functions.

**The Disadvantage:**
Because they do not reduce the problem of estimating $f$ down to a small set of parameters, they suffer from one major problem: they require a *very* large number of observations to obtain an accurate estimate.

## 4. Supervised vs Unsupervised
In **Supervised Learning**, we have both the input data and the actual outcomes to guide our model. 
* **The Math:** For every observation $i = 1, \dots, n$, we observe a vector of predictor measurements $x_i$ and an associated response measurement $y_i$. 
* **The Goal:** We fit a model that relates the response to the predictors, aiming to accurately predict the response for future observations or to understand the relationship between them. Classic examples include linear regression, logistic regression, and support vector machines.

**Unsupervised Learning**
 describes the more challenging situation where we have the input data, but absolutely no outcome variable to supervise the analysis.
* **The Math:** For every observation $i = 1, \dots, n$, we observe a vector of measurements $x_i$, but there is no associated response $y_i$. 
* **The Goal:** Because we cannot fit a regression model (there is no $Y$ to predict), our goal shifts to understanding the relationships between the variables or the observations themselves. A tool here is *cluster analysis*, where we try to check if the observations fall into distinct groups (like discovering market segments based on shopping habits).


## 5. Regression vs Classification
*   **Quantitative Variables:** These take on numerical values, like a person's age, income, or the price of a house. Problems with a quantitative response are called **regression problems**.

*   **Qualitative (Categorical) Variables:** These take on values in one of $K$ different classes or categories, like a person's marital status, whether a customer defaults on debt (yes/no), or a cancer diagnosis. Problems with a qualitative response are called **classification problems**.


## 6. Model Accuracy

To prove a model works, we have to measure its error on *unseen* test data. Here 

**For Regression (Quantitative $Y$): Mean Squared Error (MSE) is most commonly used**
$$MSE = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{f}(x_i))^2$$


**For Classification (Qualitative $Y$): Error Rate**
$$\frac{1}{n} \sum_{i=1}^{n} I(y_i \neq \hat{y}_i)$$
*(The indicator variable $I$ equals 1 if the prediction is wrong, and 0 if it is correct).*


## 7 The Bias-Variance Trade-Off

The expected test MSE for a given new observation $x_0$ can *always* be decomposed into three parts:

$$E(y_0 - \hat{f}(x_0))^2 = Var(\hat{f}(x_0)) + [Bias(\hat{f}(x_0))]^2 + Var(\epsilon)$$

1. **Expected test MSE ($E(y_0 - \hat{f}(x_0))^2$):** Refers to the average test MSE (MSE on unseen data)that we would obtain if we always estimate f using a large amount of training sets and tested each at $x_0$

2.  **Variance ($Var(\hat{f}(x_0))$):** How much would our curve change if we trained it on different data (Flexible models have *high* variance).

3.  **Squared Bias ($[Bias(\hat{f}(x_0))]^2$):** The error introduced by assuming a simple shape for a complex real-world problem. (Inflexible models have *high* bias).

4.  **Irreducible Error ($Var(\epsilon)$):** The baseline noise.

> 💡 **Note:** If you make your model more complex to capture every detail (low bias), it starts memorizing random noise and jumps wildly when given new data (high variance). The goal is to find the bottom of the U-shaped curve where the total error is lowest.

* Essentially you want a method where variance and the squares bias are low

## Classification
## 8. The Bayes Classifier

For classification problems, there is a mathematical gold standard. The **Bayes Classifier** gives the lowest possible test error rate of any model.

It assigns a test observation $x_0$ to the class $j$ that maximizes this conditional probability:
$$Pr(Y = j | X = x_0)$$
*How to read a conditional probability: "What is the probability of class $j$, given that the input is exactly $x_0$?)*

> 💡 **Learner's Note:** This is an unreachable limit. In real life, we almost never know the true, underlying probabilities of $Y$ given $X$. So, we spend our time building algorithms that try to *guess* this exact formula.

## 8.1. K-Nearest Neighbors (KNN)

Since we don't know the true probabilities for the Bayes Classifier, we have to estimate them. **K-Nearest Neighbors (KNN)** is one of the simplest ways to do this. 

Instead of knowing the exact math, KNN looks at the data points closest to your new observation and takes a "majority vote."

Given a number $K$ and a new point $x_0$, KNN finds the $K$ closest training points to $x_0$. It estimates the probability for class $j$ as the fraction of those neighbors that belong to class $j$:
$$Pr(Y=j | X=x_0) = \frac{1}{K} \sum_{i \in \mathcal{N}_0} I(y_i = j)$$

![k nearest neighbors](https://substack-post-media.s3.amazonaws.com/public/images/75bbf52d-c9ff-4916-b0fa-c844c622833b_1400x1000.png) <!-- picture from: https://thepalindrome.org/p/understanding-k-nearest-neighbors , May 09 2024, Tivadar Danka and Levi-->

### The Bias-Variance Trade-off in KNN
The model completely changes depending on the number you pick for $K$. The line it draws to separate the different classes is called the **decision boundary**:
* **$K = 1$:** The model only listens to the *single* closest point. It wraps around every data point perfectly. (**Low Bias, High Variance / Overfitting**).

* **$K = 100$:** The model takes 100 neighbors into account. The boundary becomes smooth and rigid. (**High Bias, Low Variance / Underfitting**).

> 💡 **Learner's Note:** KNN is basically peer pressure. If $K=1$, you make a decision based on your single closest friend (if they are wrong, you are wrong—high variance). If $K=100$, you survey the entire town (very safe, but you lose all the nuance—high bias). The goal is to find the $K$ value perfectly in the middle!


# Linear regression