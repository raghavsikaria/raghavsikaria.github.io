---
layout: post
title: "Visualizing Correlation Matrix using Bokeh(Python) - User Interactivity and JSCallBacks"
date: 2020-03-27 22:00:00 +0530
tags: tech python bokeh data_visualization machine_learning
description: "My first article? Nope."
---

[Originally published here on LinkedIn](https://www.linkedin.com/pulse/visualizing-correlation-matrix-using-bokehpython-user-sikaria/)

Hello reader, tired of procrastination amidst the Corona breakdown? If jazzy interactive graphs in your feed grab your attention, look no more, here is something that might fix you up. 

And if you've never coded before, why not start now? This very moment!

I didn't want the corona hibernation to go in vain and ended up learning some Bokeh (the best Python open source visualization library?). Wanted to re-enter the ML space which I deserted some 2 years back, started with the infamous Wine Data Set from the UCI repository, fixed myself up a correlation matrix for data analysis and it just occurred to me - I want a user friendly way to see those correlation numbers!

P.S: All code can be found [here(GitHub)](https://github.com/raghavsikaria/Bokeh_CorrelationMatrix).

TL;DR: here's what I came up with:

![2020-03-27-visualizing-correlation-matrix-using-bokeh Correlation Matrix](../assets/post_imgs/2020-03-27-visualizing-correlation-matrix-using-bokeh/correlation_matrix_gif.gif)

### Objective
To make a plot such that:

+ It visualises a correlation matrix
+ Provides interactivity the user in the form of:

    + Choosing color scheme
    + Giving user option to alter, save and other basic functionality


To prepare and save a correlation matrix, you can refer to this sample code:
```python
## READING wine.data from WINE DATASET - UCI
with open('wine.data') as myfile:
    x = myfile.read()

## SAVING wine.data in CSV
with open('wine.data.csv','w') as myfile:
    myfile.write(x)

## READING WINE DATA FROM CSV
df = pd.read_csv('wine.data.csv',header=None)
df.columns = ['class','Alcohol','Malic acid','Ash','Alcalinity of ash','Magnesium','Total phenols','Flavanoids','Nonflavanoid phenols','Proanthocyanins','Color intensity','Hue','OD280/OD315 of diluted wines','Proline']

## GENERATING CORRELATION MATRIX
corr_m = df.corr()

## SAVING CORRELATION MATRIX
corr_m.to_csv('wine_correlation.csv')
```

## Acknowledgements
Expressing forever gratefulness for all people associated with [UCI repository](https://archive.ics.uci.edu/ml/index.php).

My sincere thanks to:
Dua, D. and Graff, C. (2019). UCI Machine Learning Repository [http://archive.ics.uci.edu/ml]. Irvine, CA: University of California, School of Information and Computer Science.