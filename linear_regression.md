# Linear Regression

### How to use these files:
While reading, hopefully you will have a few questions, these files serve as a base line, if you don't understand something, ask your favourite AI or search the internet for the topic. I recommend giving this document into an AI, while reading ask questions and write it down.

Its a simple approach for supervised learning and useful for predicting a quantitive response. Even though it is simple, it is the foundation for almost all advanced machine learning models.

## 1. Estimating Coefficients

We assume there is a linear relationship between our input $X$ and our output $Y$. 

$$Y \approx \beta_0 + \beta_1 X$$
*   **$\beta_0$ (Intercept):** The expected value of $Y$ when $X = 0$.
*   **$\beta_1$ (Slope):** The average increase in $Y$ associated with a one-unit increase in $X$.

These coefficients ($\beta_0$ and $\beta_1$) are unknown parameters. We have to use our training data to produce estimates, which we call $\hat{\beta}_0$ and $\hat{\beta}_1$.

## 2. How Do We Fit the Line? (Least Squares & RSS)

We want to find an intercept and a slope so that our resulting line is as close as possible to the data points. 

*   **Residuals:** A residual is the difference between the actual observed response and the response predicted by our linear model. (In the picture below its referred to as the "ERROR")
*   **Residual Sum of Squares (RSS):** We square all of these residuals and add them together. 
*   **Least Squares:** The most common approach to fitting the model is minimizing the least squares criterion. The algorithm tests different lines until it finds the specific $\hat{\beta}_0$ and $\hat{\beta}_1$ that result in the absolute lowest possible RSS.

> **Note:** If you want a deep dive into linear regression, have a look at my youtube channel, i implemented linear regression using least squares in c++ from scratch.

![least squares method](https://media.geeksforgeeks.org/wp-content/uploads/20240611190240/Least-Square-Method-01.webp) <!-- picture from: https://www.geeksforgeeks.org/maths/least-square-method/ , 26 Dec, 2025-->

## 3. Interpreting the Metrics

Once your model gives you a linear regression model, you will see a table full of metrics. Here is exactly how to read them in the real world:

### Standard Error (SE)
*   **What it is:** The average amount that our estimate differs from the actual true value. 
*   **Practical Use:** It measures your uncertainty. A small standard error means you are very confident in your coefficient estimate; a large standard error means your estimate might be wildly off.

### Confidence Intervals
*   **What it is:** A 95% confidence interval is a range where, if we took repeated samples, 95% of those intervals would contain the true unknown value of the parameter. It is roughly calculated as $\hat{\beta}_1 \pm 2 \cdot SE(\hat{\beta}_1)$.
*   **Practical Use:** Instead of saying "Every $1 spent on ads yields exactly 50 sales," you use this to say "We are 95% confident that every $1 spent yields between 42 and 53 sales."

### The t-statistic
*   **What it is:** It measures the number of standard deviations that your coefficient estimate ($\hat{\beta}_1$) is away from zero.
$$t = \frac{\hat{\beta}_1 - 0}{SE(\hat{\beta}_1)}$$
*   **Practical Use:** You want this number to be large (typically $>2$ or $<-2$). A large t-statistic means your coefficient is far away from zero relative to its uncertainty, strongly hinting that the predictor actually matters.

### The p-value
*   **What it is:** The probability of observing a t-statistic this large purely by random chance, assuming there is actually no relationship between $X$ and $Y$.
*   **Practical Use:** A small p-value (typically $<0.05$) indicates that it is highly unlikely this relationship is just a coincidence. If $p < 0.05$, you reject the null hypothesis and declare that $X$ and $Y$ are truly related.
The p-value is calculated by finding the probability of observing any number equal to $t$ or larger in absolute value, assuming $\beta_1 = 0$. 

*   **The Logic:** Imagine a bell curve centered at zero. We take our t-statistic (e.g., $5$ and $-5$) and draw a vertical line on both sides of the curve. The p-value is the literal area under the curve outside of those lines (the "tails"). 
*   **The Conclusion:** If the t-statistic is huge, it sits way out on the skinny edges of the bell curve. The area past it (the p-value) is incredibly small. A small p-value indicates that it is highly unlikely to observe such a substantial association due to random chance.

The **Null Hypothesis** is the assumption that there is absolutely no relationship between your input ($X$) and your output ($Y$).
Mathematically, the null hypothesis states that the true slope coefficient ($\beta_1$) is exactly zero:
  $$H_0 : \beta_1 = 0$$


## 4. Assessing Model Accuracy

How well does the whole model fit the data? We look at two numbers:

*   **Residual Standard Error (RSE):** 
$$RSE = \sqrt{\frac{1}{n-2}RSS}$$
This is an estimate of the standard deviation of the error term ($\epsilon$). It represents the average amount that the response will deviate from the true regression line. It is an absolute measure of lack of fit, measured in the same units as $Y$.
*   **The $R^2$ Statistic:** 
$$R^2 = \frac{TSS - RSS}{TSS}$$
with $TSS = \sum (y_i - \bar{y})^2$

Because RSE is in the units of $Y$, it can be hard to tell what a "good" RSE is. $R^2$ represents the proportion of variance explained by the model. It always falls between 0 and 1. An $R^2$ close to 1 means the model explains a large portion of the variability in the response.

## 5. Multiple Linear Regression

What if we have more than one predictor? We could run separate simple regressions, but this ignores the fact that predictors might be correlated with each other, leading to misleading conclusions.

Instead, we extend the model to accommodate multiple predictors directly by giving each one its own slope coefficient:
$$Y = \beta_0 + \beta_1 X_1 + \beta_2 X_2 + \dots + \beta_p X_p + \epsilon$$

*   **How does $Y$ change:** In a setting with two predictors and one response, the least squares regression line becomes a 3D flat plane. The algorithm minimizes the vertical distances between the data points and this plane.

## 6. Qualitative Predictors

Not all predictors are numbers. If you have a categorical variable (like "Owns a House" vs. "Does Not Own").
*   You assign "Owns a House" a value of 1, and "Does Not Own" a value of 0.
*   The regression calculates a coefficient for this dummy variable, which represents the average difference in the output between owners and non-owners.

## 7. Real-World Example

Let's walk through a complete example from data to interpretation. Imagine you own a beachside kiosk, and you want to see if **Temperature in Celsius ($X$)** can accurately predict your **Ice Cream Sales in Dollars ($Y$)**. 

You record the temperature and your sales for 30 days. You feed this data into your statistical software, and it spits out the following linear regression table:

### The Output Table

| Predictor | Coefficient ($\hat{\beta}$) | Standard Error (SE) | t-statistic | p-value |
| :--- | :--- | :--- | :--- | :--- |
| **Intercept** | 50.00 | 15.00 | 3.33 | 0.002 |
| **Temperature** | 10.00 | 2.00 | 5.00 | < 0.0001 |

**Model Accuracy Metrics:**
* **Residual Standard Error (RSE):** $20.00
* **$R^2$ Statistic:** 0.85

---

### Step 1: Interpreting the Equation (The Coefficients)
The software found the line of best fit. Based on the "Coefficient" column, our mathematical model is:
**$$\text{Ice Cream Sales} = 50.00 + 10.00 \times \text{Temperature}$$**

* **The Intercept (50.00):** If the temperature is 0°C, we expect to sell $50 worth of ice cream.
* **The Slope (10.00):** For every 1°C increase in temperature, our ice cream sales increase by an average of $10.

### Step 2: Interpreting the Uncertainty (SE & Confidence Intervals)
We know our slope is 10.00, but how accurate is that guess? 
* **Standard Error (2.00):** Our slope estimate is off by about $2 on average.
* **Confidence Interval:** We calculate $\hat{\beta}_1 \pm 2 \cdot SE$. ($10 \pm 2 \times 2$). 
* **Real-World Meaning:** We are 95% confident that every 1°C increase in temperature yields between **$6 and $14** in additional sales.

### Step 3: Interpreting the Significance (t-statistic & p-value)
Does temperature *actually* affect sales, or is this just random luck? We look at the Null Hypothesis ($H_0$), which assumes temperature has zero effect.
* **t-statistic (5.00):** Our slope of 10 is 5 standard deviations away from zero.
* **p-value (< 0.0001):** The probability of seeing a t-statistic this large by random chance is less than 0.01%. 
* **Real-World Meaning:** Because the p-value is practically zero (way below 0.05), we fiercely reject the null hypothesis. Temperature absolutely drives ice cream sales.

### Step 4: Assessing Model Accuracy (RSE & $R^2$)
So, we know temperature matters. But how good is this model at *actually predicting* today's sales?
* **RSE ($20.00):** When we use this model to predict daily sales, our predictions will be off by about $20 on average. If we are selling $500 of ice cream a day, a $20 error is fantastic. If we only sell $40 a day, a $20 error is terrible.
* **$R^2$ (0.85):** 85% of the entire variance in our ice cream sales can be perfectly explained by the temperature outside. The remaining 15% is due to irreducible error (e.g., maybe it rained, or it was a Tuesday instead of a Saturday). This is a highly accurate model!

## I asked AI: 
> "now i really want you to connect the dots in my head in linear regression, from simple linear regression using least squares, rss, assessing the accuracy, standard error, confidence intervalls, p value t statistic, null hypothesis, rse, r squared"

The output was:

This is my absolute favorite part of statistical learning. When you learn these concepts one by one, they feel like a bunch of disconnected math formulas. But they are actually a single, chronological story. 

Let’s walk through that story from start to finish. Think of this as the **"Lifecycle of a Regression Line."**

---

### Step 1: The Goal ($X$ and $Y$)
You have some input ($X$, like advertising budget) and an output ($Y$, like sales). You suspect that as $X$ goes up, $Y$ goes up.

Your goal is to draw a straight line through the data to capture this relationship:
$$Y \approx \beta_0 + \beta_1 X$$
But you don't know the true slope ($\beta_1$) or the true intercept ($\beta_0$). You have to guess them using your data.

---

### Step 2: The Best Guess (Residuals, RSS, and Least Squares)
You can draw infinite lines through your data. How do you pick the best one?

*   **The Mistakes (Residuals):** For any line you draw, you measure how far off it is from the actual data points. These vertical distances are your residuals.
*   **The Score (RSS):** You square all those residuals (to get rid of negative numbers and heavily penalize massive mistakes) and add them up. This gives you the **Residual Sum of Squares (RSS)**. It is essentially your line's "error score."
*   **The Algorithm (Least Squares):** Least Squares is the mathematical algorithm that tilts and shifts the line until it finds the absolute lowest possible RSS.

*Bam.* You now have your estimated coefficients: $\hat{\beta}_0$ and $\hat{\beta}_1$. You have your line of best fit.

---

### Step 3: Can We Trust It? (Standard Error)
You have a slope (let's say it's 10). But data is noisy. If you collected data on a different day, the points would be slightly different, and your Least Squares algorithm would draw a slightly different line with a slightly different slope.

How much do we expect that line to wiggle around if we keep taking new samples?

That is your **Standard Error (SE)**. It is the average amount your estimate differs from the true reality.

---

### Step 4: The Safety Net (Confidence Interval)
Because we know our slope estimate wiggles by the amount of the Standard Error, we don't want to just give a single number. We want to give a safe range.

We take our estimate and build a net around it using the Standard Error (roughly: Estimate $\pm \ 2 \times SE$). This gives us our **95% Confidence Interval**.

> *"I estimate the slope is 10, but based on the noise, I am 95% confident the true slope is somewhere between 6 and 14."*

---

### Step 5: The Ultimate Test (Null Hypothesis, t-statistic, p-value)
Now the skeptic enters the room. The skeptic says, *"I think $X$ has absolutely zero effect on $Y$. Your line is just a coincidence from random noise."*

This assumption is the **Null Hypothesis ($H_0$)**, which states the true slope is exactly 0. You have to prove the skeptic wrong.

*   **The t-statistic:** You look at your estimate (10) and divide it by your wiggle room/Standard Error (2). This gives you your t-statistic ($10 / 2 = 5$). This tells you that your slope is 5 standard deviations away from the skeptic's claim of zero.
*   **The p-value:** You consult the bell curve (the map of dumb luck). You ask, *"If the skeptic is right, and the true slope is 0, what are the chances I would accidentally get a t-statistic of 5 purely by random luck?"* That probability is your p-value.
*   **The Verdict:** Because a t-statistic of 5 is huge, your p-value is microscopic (e.g., 0.0001). Because it is so highly unlikely to happen by chance, you **reject the Null Hypothesis**. You have mathematically proven the skeptic wrong. The relationship is real.

---

### Step 6: Grading the Model (RSE and $R^2$)
Okay, you beat the skeptic. You proved the relationship between $X$ and $Y$ is real. But is your model actually accurate for making predictions? A relationship can be 100% real, but still be incredibly noisy and weak.

We grade the overall model with two final numbers:

*   **Residual Standard Error (RSE):** You look at all the residuals (mistakes) around your final line. The RSE is the average size of those mistakes, measured in the units of $Y$. If predicting sales in dollars, an RSE of 20 means your predictions are off by about $20 on average.
*   **$R^2$ (R-squared):** Before you drew your line, $Y$ was just a chaotic scatter of data. $R^2$ asks: *"What percentage of that total chaos did your line successfully explain?"* It is a score from 0 to 1. An $R^2$ of 0.85 means your line explains 85% of the variation in $Y$. It is an A on the final exam.

---

### The "Connecting the Dots" Summary
1.  Use **Least Squares** to minimize the **RSS** and draw your line.
2.  Calculate the **Standard Error** to see how much that line wiggles.
3.  Build a **Confidence Interval** to give a safe range for your estimate.
4.  Check how far your estimate is from zero (**t-statistic**) to find the probability of dumb luck (**p-value**). If the p-value is tiny, the relationship is real (reject the **Null Hypothesis**).
5.  Finally, check the **RSE** and **$R^2$** to see if your newly-proven model is actually accurate enough to be useful.

---


### 🔍 Deep Dive: MSE vs. RMSE (Measuring the Mistakes)

When building a regression model, you need a single number that summarizes how "wrong" your model's predictions are. **MSE** and **RMSE** are the two most popular ways to calculate that score. 

Here is exactly how they work, and why we need both of them.

#### 1. Mean Squared Error (MSE)
**The Concept:** MSE takes every single mistake your model makes (the residuals), squares them, and then finds the average.

**📐 The Math:**
$$MSE = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2$$

*   **Why do we square the errors?** 
    1. To get rid of negative numbers. (If you guess +5 and -5, they would cancel each other out to 0 if you didn't square them).
    2. **To heavily punish large mistakes.** Because 2 squared is 4, but 10 squared is 100, MSE acts like a strict teacher. It is relatively forgiving of tiny errors, but it severely penalizes the model for massive outliers.
*   **The Problem with MSE:** The units are completely useless to humans. If you are predicting house prices in dollars, your MSE is measured in **"Squared Dollars."** If you are predicting temperature, it is in **"Squared Degrees."** You cannot practically explain that to a boss or a client.

#### 2. Root Mean Squared Error (RMSE)
**The Concept:** RMSE fixes the "weird units" problem of MSE. By simply taking the square root of the MSE, it translates the error back into the exact same units as your original data.

**📐 The Math:**
$$RMSE = \sqrt{MSE} = \sqrt{\frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2}$$

*   **The Practical Interpretation:** If you are predicting Ice Cream Sales in dollars, and your RMSE is 20, you can look your boss in the eye and say: *"On average, our model's predictions are off by about $20."* 
*   **How it relates to RSE:** In the textbook, you often see **Residual Standard Error (RSE)**[cite: 1]. RMSE and RSE are almost identical twins. The only difference is that RMSE divides by $n$ (the total number of points), while RSE divides by $n - 2$ (or $n - p - 1$) to account for "degrees of freedom" in smaller datasets[cite: 1]. In massive datasets, RMSE and RSE are practically the exact same number.

> 💡 **Learner's Note:** Imagine you are playing darts, and you want to measure how bad you are. 
> *   **The Residual:** How many inches your dart missed the bullseye.
> *   **MSE:** You measure the misses, square them, and average them. Your score is "16 Squared Inches." (Mathematically useful for algorithms to minimize, but practically confusing).
> *   **RMSE:** You take the square root of 16. Your score is "4 inches." Now you know you miss the bullseye by an average of 4 inches!

from datacamp https://www.datacamp.com/tutorial/sklearn-linear-regression

How do we interpret R2?

    R2 = 1: the model perfectly explains all the variance in the target variable. 
    R2 = 0: the model explains none of the variance; predictions are no better than simply using the mean. 
    R2 < 0: The model performs worse than simply using the mean, indicating a poor fit.

Some key considerations to keep in mind.

    Higher R2 is not always better. A high R2 may indicate overfitting, especially with complex models. 
    Adding more features can artificially increase R2, so a higher value isn't necessarily better.
    For multiple regression, use adjusted R2, which accounts for the number of predictors and avoids misleading improvements from unnecessary variables.

Variance Inflation Factor (VIF) is a metric used to detect multicollinearity among predictors. For each predictor, the VIF is calculated as: 

where Ri2 is the R2 value obtained when the predictor Xi is regressed against all other predictors in the model. A higher VIF means the predictor is highly correlated with other variables.

    VIF = 1: no multicollinearity (ideal scenario).
    VIF < 5: low to moderate multicollinearity (generally acceptable).
    VIF > 5: high multicollinearity (consider removing or combining correlated variables).
    VIF > 10: severe multicollinearity (strongly suggests variable redundancy).

To calculate the Variance Inflation Factor (VIF) for a specific feature, the computer actually performs a 'mini-regression' behind the scenes. Here is the process for one feature (e.g., AveRooms):

    Mini-Regression: It treats AveRooms as the target variable and uses all other features (MedInc, HouseAge, Population, etc.) as the predictors.
    R-squared ($R^2$$R^2$): It calculates how well those other features can predict AveRooms. If they predict it perfectly, the $R^2$$R^2$ will be near 1.0.
    The Formula: The VIF is then calculated as: $$VIF = \frac{1}{1 - R^2}$$

Why this formula?

    If $R^2$$R^2$ is low (e.g., 0): VIF = $1 / (1 - 0) = 1$$1 / (1 - 0) = 1$. This means the feature is completely unique.
    If $R^2$$R^2$ is high (e.g., 0.9): VIF = $1 / (1 - 0.9) = 10$$1 / (1 - 0.9) = 10$. This means the variance is 'inflated' by 10 times because the feature is redundant.


---
## asked ai to generate what is the difference between vif and pearson correlation and how to use them in a real project:
answer:
# 🔍 Deep Dive: Pearson Correlation vs. VIF (Catching Redundant Features)

When building a regression model, you only want features that bring *new* information to the table. If two features are telling the exact same story, they confuse the model's math—a problem known as **Multicollinearity**. 

**Pearson Correlation** and **VIF** are the two tools used to catch and eliminate these redundant features. Here is exactly how they work, and the ultimate workflow for using them together.

---

## 1. Pearson Correlation (The 1-on-1 Check)

**The Concept:** Pearson Correlation ($r$) is your quick, visual "first pass." It strictly measures the linear relationship between exactly **two features** at a time. 

**📐 The Scale:**
The Pearson coefficient always produces a number between **-1.0 and 1.0**.
* **$r \approx 1.0$ (Strong Positive):** The features are identical twins. If feature A goes up, feature B goes up perfectly with it.
* **$r \approx -1.0$ (Strong Negative):** The features are exact opposites. As one goes up, the other strictly goes down.
* **$r \approx 0.0$ (No Correlation):** The features have absolutely nothing to do with each other.

**The Practical Interpretation:** If you run a correlation on your weather data and find that `precipitation` and `surface_runoff` have a correlation of $0.88$, you have caught them red-handed. They are highly redundant. You only need one of them to predict the target.

---

## 2. Variance Inflation Factor / VIF (The 1-vs-Team Check)

**The Concept:** Sometimes, features are sneaky. What if no single feature is a direct twin of another, but Feature A's entire job can be perfectly replicated by a *combination* of Features B, C, and D working together? Pearson will never catch this because it only looks 1-on-1. VIF checks how redundant a feature is against the **entire rest of the dataset**.

**📐 The Math:**
To calculate VIF, the computer runs a secret "mini-regression" behind the scenes. It treats one feature (e.g., `Square_Footage`) as the target variable and uses all the *other* features to try and predict it. 

It calculates the $R^2$ of that mini-model (how perfectly the other features predicted it), and plugs it into this formula:

$$VIF_i = \frac{1}{1 - R_i^2}$$

**Why this formula?**
* If $R^2$ is low (e.g., 0): $VIF = 1 / (1 - 0) = 1$. The feature is completely unique.
* If $R^2$ is high (e.g., 0.9): $VIF = 1 / (1 - 0.9) = 10$. The variance is "inflated" by 10 times because the feature is highly redundant.

**The VIF Scorecard:**
* **VIF = 1:** No multicollinearity (Ideal scenario).
* **VIF < 5:** Low to moderate multicollinearity (Generally acceptable).
* **VIF > 5:** High multicollinearity (Consider removing or combining variables).
* **VIF > 10:** Severe multicollinearity (Strongly suggests variable redundancy; the math is breaking down).

---

## 3. The Ultimate Redundancy Workflow

Data scientists use these two tools together in a specific order to clean up a model:

1. **Step 1: The Quick Scan (Pearson Matrix)**
   * Generate a correlation matrix for all features. 
   * Look for any 1-on-1 pairings higher than $0.80$ (or lower than $-0.80$). 
   * *Action:* Drop the obvious twin. (e.g., Drop `surface_runoff` because you already have `precipitation`).
2. **Step 2: The Deep Scan (VIF)**
   * Run a VIF check on the surviving features to catch complex, multi-variable redundancies.
   * *Action:* Identify any feature with a VIF > 5 or 10.
3. **Step 3: The Surgical Strike**
   * If you find a feature with a high VIF, drop it. **But stop there!** * Because VIF is a team metric, dropping *one* feature changes the entire team dynamic. 
   * *Action:* Drop the single highest VIF feature, and then recalculate the VIF for everyone else. Often, the remaining high scores will magically drop back to safe levels!

---

💡 **Learner's Note:** Imagine you are hiring detectives to predict house prices.
* **Pearson Correlation** checks if Detective A and Detective B are copying each other's notes. (1-on-1)
* **VIF** checks if Detective A's notes could just be recreated by taking pieces of notes from Detectives B, C, and D combined. (1-vs-Team)