---
layout: post
title: "Know The Math - Everything Logistic Regression"
date: 2020-07-01 22:00:00 +0530
description: ' "Aadhe idhar jao ... aadhe idhar jao ... aur baaki hamare saath aao." - Asrani from movie Sholay '
categories: [ tech, know_the_math, machine_learning ]
image: assets/images/demo1.jpg
tags: featured
author: raghav
---

[site_link_for_portfolio_proj]: https://raghavsikaria.github.io/posts/2020-06-20-time-series-analysis-and-prediction
[linear_regression]: ../assets/post_imgs/2020-07-01-ktm-logistic-regression/linear_regression.png
[logit_derivation]: ../assets/post_imgs/2020-07-01-ktm-logistic-regression/logit_derivation.png
[mle_cost_function]: ../assets/post_imgs/2020-07-01-ktm-logistic-regression/mle_cost_function.png
[cost_function_gradient]: ../assets/post_imgs/2020-07-01-ktm-logistic-regression/cost_function_gradient.png

Hello reader, welcome to my _Know The Math_ series, a less formal series of blogs, where I take you through the *notoriously complex* backend mathematics which forms the soul for machine learning algorithms. The idea for the series originated from my Project - [NIFTYBANK Index Time series analysis, Prediction, Deep Learning & Bayesian Hyperparameter Optimization][site_link_for_portfolio_proj] where people also asked to explain the underlying concepts.

Before we begin, I need you to conduct a ceremony:

1. Stand up in front of a mirror, and say it out aloud, _"I am going to learn the Math behind Logistic Regression, today!"_
2. Take a out a naice register & pen to go through the derivation with me.

Before we begin, one _surprise_ for the uninitiated - Logistic Regression is actually a classification algorithm. Let that sink in. Today, we are going to go through Binary Logistic Regression and the same concepts can be extended for multiclass problem.

## Contents

1. [Getting The Idea](#getting-the-idea)
2. [Maximum Likelihood Estimation (MLE) & The Cost Function](#maximum-likelihood-estimation-mle--the-cost-function)
3. [The Cost Function Gradient](#the-cost-function-gradient)
4. [References](#references)

## Getting The Idea

Well, since we have to classify our given input samples to 1 of the 2 classes, the clever way humans have found is to achieve this by finding probability for one of the classes (say Class 1). And if let's say that, the found probability is greater than a chosen threshold (say >= 0.5), we can can conclude that it can be classified for 1st class; otherwise the 2nd one! The question now is, how do we find that magical probability? We start at the bottom **->** from _Linear Regression_.

![linear regression][linear_regression]
*Linear Regression - The Unsung Hero*

We can safely say, that the range of y is from negative infinity to positive infinity over the domain of x. We also know that probability of any event is bound by [0,1]. So is there anyway, by which we can make use of the above equation, apply some transformation to it and have the same bounds? Hear me out and solve with me:

![logit derivation][logit_derivation]
*Logit & the Sigmoid*

## Maximum Likelihood Estimation (MLE) & The Cost Function

MLE is a statistical algorithm and is basically a probabilistic framework from estimating the parameters for a model. It's vast potential leaves us with alot to appreciate and even more to understand, however, for our purpose we can continue like this:

![mle and cost function][mle_cost_function]
*MLE and our Cost Function - The Cross Entropy*

## The Cost Function Gradient

![Finding the gradient of the cost function][cost_function_gradient]
*Finding the gradient of the cost function*

From here on, we simply have to carry on the Gradient Descent algorithm which will iteratively update the W matrix by substracting the the above gradient times the chosen learning rate hyperparameter from the W matrix.

Hope I was able to help you out with the concepts mentioned! If not please leave feedback in comments as to how I can improve, since I plan to continue the _Know The Math_ series. Am also attaching some references for additional information and sources which helped me write this blog.

## References

+ <https://web.stanford.edu/class/archive/cs/cs109/cs109.1166/pdfs/40%20LogisticRegression.pdf>
+ <https://towardsdatascience.com/an-introduction-to-logistic-regression-8136ad65da2e>
+ <http://www.haija.org/derivation_logistic_regression.pdf>
+ <https://machinelearningmastery.com/logistic-regression-with-maximum-likelihood-estimation/#:~:text=The%20parameters%20of%20a%20logistic,framework%20called%20maximum%20likelihood%20estimation.&text=The%20parameters%20of%20the%20model,Bernoulli%20distribution%20for%20each%20example.>
+ <https://win-vector.com/2011/09/14/the-simpler-derivation-of-logistic-regression/>


> यो मामजमनादिं च वेत्ति लोकमहेश्वरम्‌ ।   
> असम्मूढः स मर्त्येषु सर्वपापैः प्रमुच्यते ॥                  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-- Bhagavad Gita 10.3 ॥

(Read about this Shloka from the Bhagavad Gita here at [http://sanskritslokas.com/](http://sanskritslokas.com/gita-slokas1.html))