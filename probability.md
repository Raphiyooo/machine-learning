# Probability

## Metrics

### mean $\bar{x}$

calculated with adding all elements and dividing by number of elements, outliers have massive impact

## median

is the exact middle element of the list, outliers have little impact

## standard deviation $\sigma$

measures how spread out the data is around the mean, calculated like this

$$\sigma = \sqrt{\frac{1}{n} \sum_{i=1}^{n} (x_i - \bar{x})^2}$$

if all data is the same, standard deviation is 0
$\sigma$ close to mean, little variance
bigger, high variance, data fluctuates more

## Variance $\sigma^2$
Variance is just the Standard Deviation before you take the square root. 
$$\sigma^2 = \frac{1}{n} \sum_{i=1}^{n} (x_i - \bar{x})^2$$
* **Why it matters:** While standard deviation is easier for humans to read (because the units match the data), algorithms and math proofs strongly prefer Variance because it avoids the messy square root calculation.

## Covariance and Correlation (Relationships)
Mean and standard deviation only look at *one* variable. Machine learning is all about how *multiple* variables interact with each other (like how $X$ affects $Y$).

### Covariance
Measures the directional relationship between two variables. 
* If $X$ goes up and $Y$ goes up, covariance is positive. 
* If $X$ goes up and $Y$ goes down, it is negative. 
* **The Problem:** The actual number could be 5, or it could be 5,000,000 depending on your units. It is very hard to interpret how "strong" the relationship is.

### Correlation ($r$)
Correlation is the upgraded, standardized version of covariance. It forces the number to always be between **-1 and 1**.
* **$1.0$:** A perfect positive relationship (A perfect upward straight line).
* **$0.0$:** Absolutely no linear relationship (A random cloud of noise).
* **$-1.0$:** A perfect negative relationship (A perfect downward straight line).

> Note: Before building a linear regression, always check the correlation between your $X$ and $Y$. If the correlation is near 0, a linear regression will be completely useless!

## Expected Value $E[X]$
* **What it is:** The theoretical "long-term average" of a random variable. If you flipped a rigged coin (70% heads) infinitely many times, the Expected Value tells you exactly what ratio you would converge on. 
* **Why it matters:** Machine learning models don't just predict a single outcome; they predict expected values. When a model minimizes error, it is mathematically trying to minimize the *Expected* Error.

## Conditional Probability $P(A | B)$
* **What it is:** The probability of an event happening, *given* that another event has already happened.
* **The Math:** Read $P(Y | X)$ as "The probability of $Y$, given $X$."
* **Why it matters:** This is the bedrock of classification

## The Normal Distribution (The Bell Curve)

The **Normal Distribution** (commonly called the Bell Curve) is the most common pattern in statistics.

![least squares method](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*F2RvkWiSR3RWgGiuy4HYFQ.png) <!-- picture from: https://medium.com/data-science-collective/the-bell-curve-for-humans-normal-distribution-central-limit-theorem-explained-7c8320cf1115 , Kalle Georgiev, 23 Sep, 2025-->

### How it Works
A Bell Curve is entirely built by just two numbers:
1.  **The Mean ($\mu$):** This dictates where the center peak of the mountain sits. Changing the mean shifts the entire curve left or right on the number line.
2.  **The Standard Deviation ($\sigma$):** This dictates how fat or skinny the mountain is. 
    * A **small $\sigma$** means the data is tightly clustered. The curve will be very tall and skinny.
    * A **large $\sigma$** means the data is highly spread out. The curve will be short and fat.

## Bayes Theorem & The Bayes Classifier

### 1. Bayes Theorem (The Core Logic)
Bayes Theorem is a mathematical formula for updating your beliefs when you get new evidence. It calculates **Conditional Probability**.

$$P(A | B) = \frac{P(B | A) \cdot P(A)}{P(B)}$$

* **Read as:** "What is the probability of $A$, given that $B$ has happened?"
* **The Analogy:** If you see someone carrying an umbrella ($B$), what is the probability that it is raining outside ($A$)? You use your prior knowledge of how often it rains ($P(A)$) and how often people carry umbrellas when it rains ($P(B|A)$) to calculate the exact probability.

### 2. How the Bayes Classifier Uses It
Imagine you are building an ML model to classify a new patient as either "Healthy" or "Sick" based on a single blood test score ($X$).

1.  **The Evidence (The Bell Curves):** The model looks at historical data and builds two separate Bell Curves: one showing the test scores of Healthy people, and one showing the test scores of Sick people.
2.  **The Theorem at Work:** You input the new patient's test score ($x_0$). The algorithm uses Bayes Theorem to calculate two specific probabilities:
    * $P(\text{Healthy} | X = x_0)$
    * $P(\text{Sick} | X = x_0)$
3.  **The Decision:** It acts like a ruthless judge. If the probability of being Sick is 51% and Healthy is 49%, it classifies the patient as Sick. 

### The Decision Boundary
Because the Healthy and Sick bell curves usually overlap, there is a specific test score where the probability of being Healthy is exactly equal to the probability of being Sick (50/50). This exact point of intersection is the **Bayes Decision Boundary**.