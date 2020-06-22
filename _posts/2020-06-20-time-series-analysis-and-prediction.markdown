---
layout: post
title: "NIFTYBANK - Time Series Analysis, Prediction & Hyper Parameter Optimization"
date: 2020-06-20 22:00:00 +0530
tags: tech python data_visualization bokeh machine_learning time_series hyperparameter_optimization
description: ' "Sahi time par sahi jagah hona, sahi time par sahi baat karna, aur sahi time par sahi kaam uthana, isi ko luck kehte hai." - Danny Denzongpa from movie Luck '
---

[github_repository]: https://github.com/raghavsikaria/Project-Rajasuya
[github_repository_for_portfolio_proj]: https://github.com/raghavsikaria/Portfolio-Optimization-and-Efficient-Frontier
[site_link_for_portfolio_proj]: https://raghavsikaria.github.io/posts/2020-05-31-portfolio-allocation-and-efficient-frontier-generation
[nifty_bank_stocks_lognormal_daily_returns_img_path]: ../assets/post_imgs/2020-05-31-portfolio-allocation-and-efficient-frontier-generation/NIFTYBANK_df_log_returns_histogram.png
[nifty_bank_stocks_cumulative_returns_img_path]: ../assets/post_imgs/2020-05-31-portfolio-allocation-and-efficient-frontier-generation/NIFTYBANK_df_norm_returns.png
[kriti_mahajan_profile]: https://www.linkedin.com/in/kriti-mahajan-174101b9/
[shreenivas_kunte_profile]: https://www.linkedin.com/in/shreenivas-kunte-cfa-cipm-0795897/
[cfa_india_soc_profile]: https://www.linkedin.com/company/cfasocietyindia/
[cfa_session_youtube_link]: https://www.youtube.com/watch?v=Rr-ztgKuaSA
[niftybank_2000-2019]: ../assets/post_imgs/2020-06-20-time-series-analysis-and-prediction/niftybank_2000-2019.PNG
[niftybank_2000-2019_returns]: ../assets/post_imgs/2020-06-20-time-series-analysis-and-prediction/niftybank_2000-2019_returns.PNG
[niftybank_adf_test_results]: ../assets/post_imgs/2020-06-20-time-series-analysis-and-prediction/niftybank_adf_test_results.png
[niftybank_returns_adf_test_results]: ../assets/post_imgs/2020-06-20-time-series-analysis-and-prediction/niftybank_returns_adf_test_results.png
[niftybank_returns_normalization_comparison]:  ../assets/post_imgs/2020-06-20-time-series-analysis-and-prediction/niftybank_2000-2019_returns_normalization_comparison.PNG
[niftybank_acf_pacf]: ../assets/post_imgs/2020-06-20-time-series-analysis-and-prediction/niftybank_2000-2019_acf_pacf.PNG
[niftybank_returns_arima]: ../assets/post_imgs/2020-06-20-time-series-analysis-and-prediction/niftybank_2000-2019_returns_arima_predictions.PNG

**DISCLAIMER** - _What this project is not!_:

1. This project does not give any investment advice & is purely for research and academic purposes
2. The code is not meant for production, error-catching and logging mechanism have been skipped intentionally

**One word** for my fellow developers & interested folks:

1. If you like the project or find it interesting, please star it on GitHub
2. Kindly provide a link back to this project or my profile on LinkedIn/GitHub if you use any of the codes in any capacity (courtsey in Open Source)
3. Since the project is in active developement, if you find anything wrong/sinful: please raise an issue in GitHub repository [here][github_repository]
4. If you want to enhance/improve/help or contribute in any capacity: please raise a PR in GitHub repository [here][github_repository]
5. I am quite dissatisfied with the code design, and would appreciate if anyone can improve over it!

> I can't recall ever once having seen the name of a market timer on Forbes' annual list of the richest people in the world. If it were truly possible to predict corrections, you'd think somebody would have made billions by doing it.                               
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Peter Lynch

Hello reader, this project is completetly about Time Series - its analysis, prediction modelling and hyperparameter optimization - specifically Bayesian HPO using Gausian Processes. I have made sure that the code is reusable & generic enough. You can find some cool coding hacks and extensive API usage of libraries in the code. You can also try it out on different data sets as well! We'll be coding together and apply ARIMA, Deep Learning - CNNs, LSTMs & the likes. It's going to be extremely technical, all code has been open sourced [here on GitHub][github_repository] and I have tried my best to aid you with explicit code comments and annotations.
The project has been implemented in Python.

+ Deep Learning library - tf.keras (yes, there's difference between this and Keras)
+ Visualization library - Bokeh
+ Hyperparameter Optimization library - Skopt (scikit-optimize)
+ Data processing library - Pandas, Numpy, sklearn & statsmodels

Will also be writing in detail in upcoming weeks about underlying math and theory of the concepts involved in the project!

I attended [CFA Society, India's][cfa_india_soc_profile] webinar on 11th April '20 by [Kriti Mahajan][kriti_mahajan_profile] moderated by [Shreenivas Kunte, CFA, CIPM][shreenivas_kunte_profile] on the topic- _"Practitioners' Insights: Using a Neural Network to Predict Stock Index Prices"_ which gave me the idea & inspiration to invest my time in this direction and this project was born!

P.S: Like always all code can be found [here][github_repository] on GitHub.

## Objective

+ To analyse a Time Series dataset (in this project - NIFTYBANK specifically, but you can use any dataset)
+ To conduct Data exploration for understanding the data
+ To process time-series data, normalise it and conduct data-stationarity testing
+ To prepare benchmark in predictive modelling using traditional algorithms like ARIMA
+ To apply Deep Learning techniques & improve on predictive modelling performance
  + To automate modelling process
  + To automate & apply Bayesian Hyperparameter Optimization techniques and find optimal hyperparameters for the models
  + To automate & generate markdown document for each model which summarizes model in its entirity - from architecture, to HPO process to performance

## Contents

1. [Data Exploration, Analysis & Processing](#data-exploration-analysis--processing)
    + [NIFTYBANK - A glance](#niftybank---a-glance)
    + [Checking for Covariance Stationarity](#checking-for-covariance-stationarity)
    + [Normalizing the Data](#normalizing-the-data)
    + [Correlation between all NIFTYBANK Index stocks](#correlation-between-all-niftybank-index-stocks)
    + [NIFTYBANK Index Stocks - Daily Lognormalized Returns](#niftybank-index-stocks---daily-lognormalized-returns)
    + [NIFTYBANK Index Stocks - Normalized Cumulative Returns](#niftybank-index-stocks---normalized-cumulative-returns)
2. [Generating Benchmark using ARIMA](#generating-benchmark-using-arima)
3. [Deep Neural Networks](#deep-neural-networks)
    + [Simple Convolutional Neural Network](#simple-convolutional-neural-network)
    + [Simple LSTM Network](#simple-lstm-network)
4. [Understanding the Code](#understanding-the-code)
5. [Acknowledgements](#acknowledgements)
6. [References](#references)

## Data Exploration, Analysis & Processing

The current index has 12 constituents of which Bandhan Bank was the newest entry(added when it went Public on 27th March 2018). Let's see what the index looks like today (As of 19th June 2020)

|Company|Symbol|Sector|Ownership|Closing Price|Market Cap (Cr.)|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Axis Bank|AXISBANK|Financial Services - Bank|Private|417.05|117,691.88|
| Bandhan Bank|BANDHANBNK|Financial Services - Bank|Private|290.60|46,794.72|
| Bank of Baroda|BANKBARODA|Financial Services - Bank|Public|47.05|21,739.77|
| Federal Bank|FEDERALBNK|Financial Services - Bank|Private|50.95|10,156.17|
| HDFC Bank|HDFCBANK|Financial Services - Bank|Private|1,033.35|567,337.93|
| ICICI Bank|ICICIBANK|Financial Services - Bank|Private|363.80|235,585.60|
| IDFC First Bank|IDFCFIRSTB|Financial Services - Bank|Private|25.85|14,663.01|
| IndusInd Bank|INDUSINDBK|Financial Services - Bank|Private|483.65|33,544.02|
| Kotak Mahindra|KOTAKBANK|Financial Services - Bank|Private|1,302.50|257,739.27|
| PNB |PNB|Financial Services - Bank|Public|34.50|32,466.67|
| RBL Bank|RBLBANK|Financial Services - Bank|Private|168.90|8,592.13|
| SBI |SBIN|Financial Services - Bank|Public|184.50|164,659.08|

For this project, I have worked on a private data set which has Time-Series data comprising of more than 350 macro-economic variables pertaining to India. Currently I am trying to make it public so that everyone can explore it. The data spans 20 years starting from 2000-01-18 and ending at 2020-02-13.

### NIFTYBANK - A glance

![NIFTYBANK - 2000 to 2019][niftybank_2000-2019]
*Nifty Bank progression over 2 decades based on closing prices*

NIFTYBank has come a long way since it's inception. At first look, it's quite difficult to judge for Variance for this data. However, just by looking at Mean progression: one can conclude that this data is not covariance stationary.

### Checking for Covariance Stationarity

Time series modelling is quite the demanding task. We need to mathematically check for covariance stationarity on our data, and if not we need to transform the data such that it becomes stationary.
Let's check for the same on our data. We'll use the Augmented Dickey-Fuller statistical test for this. Here's a snippet that'll give you an idea on how we can conduct this test:

~~~python
  import pandas as pd
  from statsmodels.tsa.stattools import adfuller as aft

  def apply_aft_test_to_df_column(df_column: 'DataFrame column') -> dict:
      """Applies AFT Stationarity checking test to given DataFrame Column & returns calculated parameters as dict."""

      # Conduct AFT test on the given DF Column
      t_stat,p_val,no_of_lags,no_of_observation,crit_val,_ = aft(df_column, autolag='AIC')

      return {'T Stat':t_stat,'P value':p_val,'Number of Lags':no_of_lags,'Number of Observations':no_of_observation,'Critical value @ 1%':crit_val['1%'],'Critical value @ 5%':crit_val['5%'],'Critical value @ 10%':crit_val['10%']}

  # df = OUR DATA THAT WE WANT TO CHECK FOR COVARIANCE STATIONARITY
  df_columns = df.columns.values
  data_stationarity_information = {}

  for column in df_columns:
      # Conducting AFT test for specific column
      response = DataStationarityUtils.apply_aft_test_to_df_column(df[column])
      # Storing AFT test results for the column
      data_stationarity_information[column] = response

  # Converting stored AFT results into DataFrame
  data_stationarity_information_df = pd.DataFrame.from_dict(data_stationarity_information, orient='index')
  data_stationarity_information_df.index.name = "Features"
  # Generating stationarity verdict
  data_stationarity_information_df['Stationarity Verdict'] = data_stationarity_information_df['P value'] < 0.05
~~~

Here are the results:

![NIFTYBANK - ADF Test results][niftybank_adf_test_results]
*ADF Test results & verdict on stationarity over all features in dataset*

Quite a few features are not stationary, especially our dependent feature of NIFTYBank. We could have gone ahead with the same data for our modelling as all features are cointegrated(economically linked to same macro-economic variables), but presence of few stationary features pose a problem. Hence, we'll make all features stationary by transforming the data for _daily returns_ of all the features, and then checking for stationarity! Here is a glance at NIFTYBank Returns:

![NIFTYBANK - 2000 to 2019 - Returns][niftybank_2000-2019_returns]
*Nifty Bank Returns progression over 2 decades based on closing prices*

Let's check for covariance stationarity on this transformed data (All calculated P-values are very small, close to zero & have been rounded off to 5 decimal places in the below result):

![NIFTYBANK Returns - ADF Test results][niftybank_returns_adf_test_results]
*ADF Test results & verdict on stationarity over all features in transformed dataset*

We can now safely conclude that dataset can be called covariance stationary and proceed with further processing.

### Normalizing the Data

We know that our features have different ranges and even follow their own different distributions, and this can cause issues when we use this data in our models. An easily observable issue is that some features might influence the result more/less than others. We will first standardize our data by calculating Z-score for every data point across all features. This will ensure that our data across all features has a mean tending to zero and standard deviation tending to 1. And then, we'll apply feature scaling to range [-1,1] to ensure that all features abide by these ranges! 

~~~python
  # Assuming we have split our data into training, validation & testing sets
  # (Refer for repository for splitting data util functions)
  # Here is how were are going to STANDARDIZE it:
  from sklearn.preprocessing import StandardScaler, MinMaxScaler

  normalizer = StandardScaler()
  # Remember we 'fit_transform' only on the TRAINING data
  standardized_training_data = normalizer.fit_transform(training_data.values)
  standardized_validation_data = normalizer.transform(validation_data.values) 
  standardized_testing_data = normalizer.transform(testing_data.values)

  # The standardized data can now be fed for feature scaling
  minmax_scaler = MinMaxScaler(feature_range=(-1,1))
  minmax_norm_training_data = minmax_scaler.fit_transform(standardized_training_data)
  minmax_norm_validation_data = minmax_scaler.transform(standardized_validation_data)
  minmax_norm_testing_data = minmax_scaler.transform(standardized_testing_data)
~~~

Here is a visual aid to show effects of both these transformations on our data. (Have taken only the Dependent variable here for illustration):

![NIFTYBANK Returns - effects of transformation][niftybank_returns_normalization_comparison]
*Effects of transformation on 1 of the features on our data*

Do check the max & min values, mean and standard deviation across all plots above. It will definitely bring more clarity!
Before proceeding further, let's visit the constituents of NIFTYBANK briefly. (This analysis is part of my [NIFTYBANK Portfolio Optimization & Efficient Frontier Generation project][site_link_for_portfolio_proj]. You can visit the project's repository [here][github_repository_for_portfolio_proj] to get codes for this. Also, NIFTYBANK data from 4th April 2018 to 22th May 2020 has been considered for this.)

### Correlation between all NIFTYBANK Index stocks

(Try it out here on this blog, it's interactive!)

### NIFTYBANK Index Stocks - Daily Lognormalized Returns

We can find out about the daily returns of all constituents by simply taking log of change of price at intervals of 1 day.

![NIFTYBANK Index Stocks - Daily Lognormalized Returns][nifty_bank_stocks_lognormal_daily_returns_img_path]

### NIFTYBANK Index Stocks - Normalized Cumulative Returns

I have taken the base day as 4th April 2020, and calculated the cumulative returns with respect to that day.

![NIFTYBANK Index Stocks - Normalized Cumulative Returns][nifty_bank_stocks_cumulative_returns_img_path]

## Generating Benchmark using ARIMA

Let's apply the widely known ARIMA model to our data. This is a traditional statistical algorithm, and we'll consider results from this model as our benchmark for comparing other complex Deep Learning models.
Let's first have a look at Autocorrelation & Partial Autocorrelation function plots! Here is a snippet which will give us an idea on how we can do this:

~~~python
  # Assuming df = DATA on which we wish to plot the ACF & PACF functions
  import statsmodels.graphics.tsaplots as tsaplots

  fig = plt.figure(figsize=(12,8))
  ax1 = fig.add_subplot(211)
  fig = tsaplots.plot_acf(df['Nifty Bank returns'].astype(float), lags=25, ax=ax1)
  ax2 = fig.add_subplot(212)
  fig = tsaplots.plot_pacf(df['Nifty Bank returns'].astype(float), lags=25, ax=ax2)
~~~

![NIFTYBank ACF & PACF Plot][niftybank_acf_pacf]
*ACF & PACF Plots on our NIFTYBANK Returns dependent variable*

A look at the plots tells us that with 95% confidence, correlation values post 1st lag are mostly statistical flukes and can safely be ignored i.e within 1st lag, AR is significant! This will help us decide on hyperparameters for our ARIMA model. These are the results I recieved on an ARIMA model:

![NIFTYBANK returns ARIMA][niftybank_returns_arima]
*Prediction results & MSE on validation + test data via ARIMA model to be considered as a benchmark*

Now that we somewhat have a benchmark, let's try out Deep Neural Networks.

## Deep Neural Networks

Let us now try and dive into Deep Neural Networks. I have prepared 2 very basic networks - a simple convolutional neural network and a simple LSTM network. Neural Networks are notoriously famous for their complex backend and black-box nature! I'll be covering detailed analysis of these networks in upcoming weeks. For now, you may refer the code in repository and try them out yourself! Here are some results from the auto-generated Markdown reports I achieved from these models (Interactive plots of the below graphs can be found in HTML files in this model's assets in repository):

### Simple Convolutional Neural Network

[scn_model_plot_path]: ../assets/post_imgs/2020-06-20-time-series-analysis-and-prediction/simple_cnn_assets/simple_cnn_model_plot_TS_22-06-2020_13-57-20-PM.png
[scn_model_training_hist_path]: ../assets/post_imgs/2020-06-20-time-series-analysis-and-prediction/simple_cnn_assets/simple_cnn_model_training_hist_22-06-2020_13-57-20-PM.png
[scn_model_prediction_path]: ../assets/post_imgs/2020-06-20-time-series-analysis-and-prediction/simple_cnn_assets/simple_cnn_model_prediction_22-06-2020_13-57-52-PM.png
[scn_hpo_plot_convergence_path]: ../assets/post_imgs/2020-06-20-time-series-analysis-and-prediction/simple_cnn_assets/hpo_plot_convergence.png
[scn_hpo_plot_evaluation_path]: ../assets/post_imgs/2020-06-20-time-series-analysis-and-prediction/simple_cnn_assets/hpo_plot_evaluation.png
[scn_hpo_plot_objective_path]: ../assets/post_imgs/2020-06-20-time-series-analysis-and-prediction/simple_cnn_assets/hpo_plot_objective.png
[scn_hpo_plot_regret_path]: ../assets/post_imgs/2020-06-20-time-series-analysis-and-prediction/simple_cnn_assets/hpo_plot_regret.png
[scn_model_hpo_iterations_path]: ../assets/post_imgs/2020-06-20-time-series-analysis-and-prediction/simple_cnn_assets/hpo_iterations.png

#### Model Summary 

```txt 
Model: "simple_convolutional_network"
_________________________________________________________________
Layer (type)                 Output Shape              Param #   
=================================================================
input_2 (InputLayer)         [(None, 30, 600)]         0         
_________________________________________________________________
conv1d_1 (Conv1D)            (None, 15, 24)            28824     
_________________________________________________________________
activation_2 (Activation)    (None, 15, 24)            0         
_________________________________________________________________
max_pooling1d_1 (MaxPooling1 (None, 15, 24)            0         
_________________________________________________________________
dropout_1 (Dropout)          (None, 15, 24)            0         
_________________________________________________________________
flatten_1 (Flatten)          (None, 360)               0         
_________________________________________________________________
dense_1 (Dense)              (None, 1)                 361       
_________________________________________________________________
activation_3 (Activation)    (None, 1)                 0         
=================================================================
Total params: 29,185
Trainable params: 29,185
Non-trainable params: 0
```

#### Model Plot

<span style="display:block;text-align:center">![Simple Convolutional Network][scn_model_plot_path]</span>

#### Hyperparameter Optimization

##### Iterations & Results

<span style="display:block;text-align:center">![HPO Iterations Table to be added when HPO is conducted][scn_model_hpo_iterations_path]</span>

##### Convergence Plot

<span style="display:block;text-align:center">![convergence plot to be added when HPO is conducted][scn_hpo_plot_convergence_path]</span>

##### Evaluation Plot

<span style="display:block;text-align:center">![evaluation plot to be added when HPO is conducted][scn_hpo_plot_evaluation_path]</span>

##### Objective Plot

<span style="display:block;text-align:center">![objective plot to be added when HPO is conducted][scn_hpo_plot_objective_path]</span>

##### Regret Plot

<span style="display:block;text-align:center">![regret plot to be added when HPO is conducted][scn_hpo_plot_regret_path]</span>

#### Model Training History

<span style="display:block;text-align:center">![Simple Convolutional Network][scn_model_training_hist_path]</span>

#### Model Predictions & MSE

<span style="display:block;text-align:center">![Simple Convolutional Network - Predictions - Will be added when predictions are generated][scn_model_prediction_path]</span>

### Simple LSTM Network

[sln_model_plot_path]: simple_lstm_model_plot_TS_22-06-2020_20-33-59-PM.png
[sln_model_training_hist_path]: simple_lstm_model_training_hist_22-06-2020_20-33-59-PM.png
[sln_model_prediction_path]: simple_lstm_model_prediction_22-06-2020_20-34-51-PM.png
[sln_hpo_plot_convergence_path]: hpo_plot_convergence.png
[sln_hpo_plot_evaluation_path]: hpo_plot_evaluation.png
[sln_hpo_plot_objective_path]: hpo_plot_objective.png
[sln_hpo_plot_regret_path]: hpo_plot_regret.png
[sln_model_hpo_iterations_path]: hpo_iterations.png

#### Model Summary 

```txt 

Model: "simple_lstm_network"
_________________________________________________________________
Layer (type)                 Output Shape              Param #   
=================================================================
input_1 (InputLayer)         [(None, 30, 600)]         0         
_________________________________________________________________
lstm (LSTM)                  (None, 20)                49680     
_________________________________________________________________
activation (Activation)      (None, 20)                0         
_________________________________________________________________
dropout (Dropout)            (None, 20)                0         
_________________________________________________________________
dense (Dense)                (None, 1)                 21        
_________________________________________________________________
activation_1 (Activation)    (None, 1)                 0         
=================================================================
Total params: 49,701
Trainable params: 49,701
Non-trainable params: 0
_________________________________________________________________

``` 

#### Model Plot

<span style="display:block;text-align:center">![Simple LSTM Network][sln_model_plot_path]</span>

#### Hyperparameter Optimization

##### Iterations & Results

<span style="display:block;text-align:center">![HPO Iterations Table to be added when HPO is conducted][sln_model_hpo_iterations_path]</span>

##### Convergence Plot

<span style="display:block;text-align:center">![convergence plot to be added when HPO is conducted][sln_hpo_plot_convergence_path]</span>

##### Evaluation Plot

<span style="display:block;text-align:center">![evaluation plot to be added when HPO is conducted][sln_hpo_plot_evaluation_path]</span>

##### Objective Plot

<span style="display:block;text-align:center">![objective plot to be added when HPO is conducted][sln_hpo_plot_objective_path]</span>

##### Regret Plot

<span style="display:block;text-align:center">![regret plot to be added when HPO is conducted][sln_hpo_plot_regret_path]</span>

#### Model Training History

<span style="display:block;text-align:center">![Simple LSTM Network][sln_model_training_hist_path]</span>

#### Model Predictions & MSE

<span style="display:block;text-align:center">![Simple LSTM Network - Predictions - Will be added when predictions are generated][sln_model_prediction_path]</span>

## Understanding the Code

Here is quick primer to understand repository and structure of code:

~~~txt
.
├── assets
│   ├── simple_cnn
│   └── simple_lstm
├── config
│   └── model_configurations.py
├── driver.ipynb
├── initiate_predictive_modelling.py
├── LICENSE
├── models
│   └── neural_network_models.py
├── README.md
└── utils
    ├── data_processing_utils.py
    ├── data_stationarity_utils.py
    ├── data_visualization_utils.py
    └── model_utilities.py
~~~

+ assets: You can find all plots, html files, markdown documents here
+ config/model_configurations.py: You can find all config variables set here for the entire project
+ driver.ipynb: A jupyter notebook to show how to go about modelling! It can give an idea on how all utilities interact
+ initiate_predictive_modelling.py: A temporary piece of code I created over other utilities so that one can carry out end to end process
+ utils/data_processing_utils.py: You can find all Data processing tools here - reading CSV, dealing with NA, interpolation of data, saving df, computing lagged features, splitting time series data, data standardization, data normalization, etc
+ utils/data_stationarity_utils.py: You can find functions here to check for covariance stationarity over given data
+ utils/data_visualization_utils.py: You can find all visualization related functions here - bokeh utils, skopt plots, etc
+ utils/model_utilities.py: You can find all model related utilities here - saving your model, generating predictions, conducting HPO, etc

If I've had your interest so far, let's collaborate over [GitHub][github_repository] and make this better.
You can also reach out to me incase you have any queries pertaining to anything. Hope this helps!

## Acknowledgements

My sincere thanks to [Kriti Mahajan][kriti_mahajan_profile], [Shreenivas Kunte, CFA, CIPM][shreenivas_kunte_profile] & [CFA Society, India][cfa_india_soc_profile] to conduct and host the webinar. It was quite insightful & gave me all the inspritaion I needed to take this project up. The entire session can be found [here][cfa_session_youtube_link] on Youtube.

## References

#### Time Series modelling & tf.keras Resources

+ Kriti Mahajan's Colab Notebook <https://colab.research.google.com/drive/1PYj_6Y8W275ficMkmZdjQBOY7P0T2B3g#scrollTo=mCTtyN5OP22z>
+ <https://www.tensorflow.org/tutorials/structured_data/time_series>
+ <https://www.tensorflow.org/api_docs/python/tf/keras>
+ <https://www.tensorflow.org/guide/keras/sequential_model>
+ <https://www.pyimagesearch.com/2019/10/28/3-ways-to-create-a-keras-model-with-tensorflow-2-0-sequential-functional-and-model-subclassing/>
+ <https://www.pyimagesearch.com/2019/10/21/keras-vs-tf-keras-whats-the-difference-in-tensorflow-2-0/>

#### Hyperparameter Optimization Resources

+ <https://www.kdnuggets.com/2018/12/keras-hyperparameter-tuning-google-colab-hyperas.html>
+ <https://medium.com/@crawftv/parameter-hyperparameter-tuning-with-bayesian-optimization-7acf42d348e1>
+ <https://github.com/Hvass-Labs/TensorFlow-Tutorials/blob/master/19_Hyper-Parameters.ipynb>
+ <https://github.com/crawftv/Skopt-hyperparameter-tutorial/blob/master/scikit_optimize_tutorial.ipynb>
+ <https://towardsdatascience.com/hyperparameter-optimization-in-python-part-1-scikit-optimize-754e485d24fe>
+ <https://scikit-optimize.github.io/stable/>
+ <https://www.kaggle.com/schlerp/tuning-hyper-parameters-with-scikit-optimize>
+ <https://www.kdnuggets.com/2019/06/automate-hyperparameter-optimization.html>
+ <https://towardsdatascience.com/hyperparameter-optimization-in-python-part-0-introduction-c4b66791614b>

#### NIFTYBank Data Resources

+ <https://www.niftyindices.com/indices/equity/sectoral-indices/nifty-bank>
+ <https://economictimes.indiatimes.com/markets/nifty-bank/indexsummary/indexid-1913,exchange-50.cms>
+ <https://www.moneycontrol.com/stocks/marketstats/indexcomp.php?optex=NSE&opttopic=indexcomp&index=23>



> यदा यदा हि धर्मस्य ग्लानिर्भवति भारत।                       
> अभ्युत्थानमधर्मस्य तदात्मानं सृजाम्यहम् ॥४-७॥                    
>
> परित्राणाय साधूनां विनाशाय च दुष्कृताम् ।                      
> धर्मसंस्थापनार्थाय सम्भवामि युगे युगे ॥४-८॥                   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-- Bhagavad Gita ॥

(Read about this Shloka from the Bhagavad Gita here at [www.thedivineindia.com](https://www.thedivineindia.com/hi/yada-yada-hi-dharmasya.html))