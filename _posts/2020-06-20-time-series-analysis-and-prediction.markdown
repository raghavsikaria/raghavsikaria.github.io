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

**DISCLAIMER** - _What this project is not_:

1. This project does not give any investment advice & is purely for research and academic purposes
2. The code is not meant for production, error-catching and logging mechanism have been skipped intentionally

**One word** for my fellow developers & interested folks:

**IF**

+ You like the project -> please star it on GitHub
+ You find anything wrong -> please raise an issue in GitHub repository [here][github_repository]
+ You want to enhance/improve/help or contribute in any capacity -> please raise a PR in GitHub repository [here][github_repository]
+ You use any of the codes -> kindly provide a link back to this project or my profile on LinkedIn/GitHub (courtsey in Open Source)
...there's always scope in improving the code design!

> I can't recall ever once having seen the name of a market timer on Forbes' annual list of the richest people in the world. If it were truly possible to predict corrections, you'd think somebody would have made billions by doing it.                               
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Peter Lynch

Hello reader, this project is completetly about Time Series - its analysis, prediction modelling and hyperparameter optimization(HPO) - specifically Bayesian HPO using Gausian Processes. I have made sure that the code is reusable. You can find some cool coding hacks and extensive API usage of libraries in the code. What's more?! Since the code is generic, you can also try it out on different data sets as well! We'll employ ARIMA, Deep Learning - CNNs, LSTMs & the likes. It's going to be extremely technical, all code has been open sourced [here][github_repository] and I have tried my best to aid you with explicit code comments and annotations.
The project has been implemented in Python & following are the libraries utilised:

+ Deep Learning library - tf.keras (yes, there's difference between this and Keras)
+ Visualization library - Bokeh
+ Hyperparameter Optimization library - Skopt (scikit-optimize)
+ Data processing library - Pandas, Numpy, sklearn & statsmodels

Will also be writing in detail in upcoming weeks about underlying math and theory of the concepts involved in the project!

I attended [CFA Society, India's][cfa_india_soc_profile] webinar on 11th April '20 by [Kriti Mahajan][kriti_mahajan_profile] moderated by [Shreenivas Kunte, CFA, CIPM][shreenivas_kunte_profile] on the topic- _"Practitioners' Insights: Using a Neural Network to Predict Stock Index Prices"_ which gave me the idea & inspiration to invest my time in this direction and this project was born!

P.S: Like always all code can be found [here][github_repository].

## Objective

+ To analyse a Time Series dataset (in this project - NIFTYBANK specifically, but you can use any dataset)
+ To conduct Data exploration for understanding the data
+ To process time-series data, normalise it and conduct data-stationarity testing
+ To prepare benchmark in predictive modelling using traditional algorithms like ARIMA
+ To apply Deep Learning techniques & try and improve on predictive modelling performance
  + To automate modelling process
  + To automate & apply Bayesian HPO techniques and find optimal hyperparameters for the models
  + To automate & generate markdown document for each model which summarizes the model in its entirety - from architecture to HPO process to performance

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

The current index has 12 constituents of which Bandhan Bank was the newest entry (added when it went Public on 27th March 2018). Let's see what the index looks like today (As of 19th June 2020)

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

For this project, I have worked on a private data set which has Time Series data comprising of more than 350 macro-economic variables pertaining to India. Currently I am trying to make it public so that everyone can explore it. The data spans 20 years starting from 2000-01-18 and ending at 2020-02-13.

### NIFTYBANK - A glance

![NIFTYBANK - 2000 to 2019][niftybank_2000-2019]
*Nifty Bank progression over 2 decades based on closing prices*

NIFTYBank has come a long way since it's inception. At first look, it's quite difficult to judge for variance for this data. However, just by looking at mean progression: one can conclude that this data is not covariance stationary.

### Checking for Covariance Stationarity

Time series modelling is quite challenging. We need to mathematically check for covariance stationarity on our data and if not, we need to transform the data such that it becomes stationary.
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

Quite a few features are not stationary, especially our dependent feature of NIFTYBank. We could have gone ahead with the same data for our modelling as all features are cointegrated (economically linked to same macro-economic variables), but presence of few stationary features pose a problem. Hence, we'll make all features stationary by transforming the data for _daily returns_ of all the features, and then checking for stationarity. Here is how we can transform our data:

~~~python
  # Assuming df = our DATA which we want to transform
  df_columns = df.columns.values
  daily_pct_change_df = pd.DataFrame(index=df.index)

  # Generating new columns as percentage change
  # on a single day basis
  for col in df_columns:
      daily_pct_change_df[f'{col} returns'] = df[col].pct_change(1)

  # Dropping first row as it contains all NAN values!
  daily_pct_change_df.drop(daily_pct_change_df.index[0],inplace=True)
~~~

Here is a glance at NIFTYBank Returns:

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

  <head>
      <meta charset="utf-8">
      <title>Bokeh Plot</title>

        <script type="text/javascript" src="https://cdn.pydata.org/bokeh/release/bokeh-1.4.0.js"></script>
        <script type="text/javascript" src="https://cdn.pydata.org/bokeh/release/bokeh-widgets-1.4.0.js"></script>
<script type="text/javascript" src="https://cdn.bokeh.org/bokeh/release/bokeh-api-1.4.0.js"></script>        <script type="text/javascript">
            Bokeh.set_log_level("info");
        </script>
  </head>
  <body>
              <div class="bk-root" id="b1acd5a7-bd8a-4cba-a320-3eb0a9d2a7b6" data-root-id="2410"></div>
        <script type="application/json" id="2515">
          {"b1cbeec3-d674-46ae-a871-081f04b6158e":{"roots":{"references":[{"attributes":{"children":[{"id":"2408","type":"Row"},{"id":"2409","type":"Row"}]},"id":"2410","type":"Column"},{"attributes":{"callback":null,"overlay":{"id":"2426","type":"BoxAnnotation"}},"id":"2375","type":"BoxSelectTool"},{"attributes":{},"id":"2358","type":"CategoricalScale"},{"attributes":{"below":[{"id":"2362","type":"CategoricalAxis"}],"center":[{"id":"2365","type":"Grid"},{"id":"2369","type":"Grid"}],"left":[{"id":"2366","type":"CategoricalAxis"}],"plot_height":600,"plot_width":800,"renderers":[{"id":"2399","type":"GlyphRenderer"}],"right":[{"id":"2403","type":"ColorBar"}],"title":{"id":"2352","type":"Title"},"toolbar":{"id":"2382","type":"Toolbar"},"toolbar_location":"above","x_range":{"id":"2354","type":"FactorRange"},"x_scale":{"id":"2358","type":"CategoricalScale"},"y_range":{"id":"2356","type":"FactorRange"},"y_scale":{"id":"2360","type":"CategoricalScale"}},"id":"2351","subtype":"Figure","type":"Plot"},{"attributes":{},"id":"2360","type":"CategoricalScale"},{"attributes":{"source":{"id":"2349","type":"ColumnDataSource"}},"id":"2400","type":"CDSView"},{"attributes":{},"id":"2379","type":"ZoomInTool"},{"attributes":{},"id":"2424","type":"Selection"},{"attributes":{},"id":"2371","type":"SaveTool"},{"attributes":{},"id":"2374","type":"WheelZoomTool"},{"attributes":{"format":"%.1f"},"id":"2402","type":"PrintfTickFormatter"},{"attributes":{},"id":"2422","type":"CategoricalTickFormatter"},{"attributes":{"axis_line_color":{"value":null},"formatter":{"id":"2420","type":"CategoricalTickFormatter"},"major_label_text_font_size":{"value":"12pt"},"major_tick_line_color":{"value":null},"ticker":{"id":"2367","type":"CategoricalTicker"}},"id":"2366","type":"CategoricalAxis"},{"attributes":{},"id":"2381","type":"CrosshairTool"},{"attributes":{"data_source":{"id":"2349","type":"ColumnDataSource"},"glyph":{"id":"2397","type":"Rect"},"hover_glyph":null,"muted_glyph":null,"nonselection_glyph":{"id":"2398","type":"Rect"},"selection_glyph":null,"view":{"id":"2400","type":"CDSView"}},"id":"2399","type":"GlyphRenderer"},{"attributes":{"active_drag":"auto","active_inspect":"auto","active_multi":null,"active_scroll":"auto","active_tap":"auto","autohide":true,"tools":[{"id":"2370","type":"HoverTool"},{"id":"2371","type":"SaveTool"},{"id":"2372","type":"PanTool"},{"id":"2373","type":"ResetTool"},{"id":"2374","type":"WheelZoomTool"},{"id":"2375","type":"BoxSelectTool"},{"id":"2376","type":"TapTool"},{"id":"2377","type":"UndoTool"},{"id":"2378","type":"RedoTool"},{"id":"2379","type":"ZoomInTool"},{"id":"2380","type":"ZoomOutTool"},{"id":"2381","type":"CrosshairTool"}]},"id":"2382","type":"Toolbar"},{"attributes":{"axis_line_color":{"value":null},"formatter":{"id":"2422","type":"CategoricalTickFormatter"},"major_label_orientation":1.5707963267948966,"major_label_text_font_size":{"value":"12pt"},"major_tick_line_color":{"value":null},"ticker":{"id":"2363","type":"CategoricalTicker"}},"id":"2362","type":"CategoricalAxis"},{"attributes":{},"id":"2363","type":"CategoricalTicker"},{"attributes":{"callback":null},"id":"2376","type":"TapTool"},{"attributes":{},"id":"2372","type":"PanTool"},{"attributes":{"color_mapper":{"id":"2350","type":"LinearColorMapper"},"formatter":{"id":"2402","type":"PrintfTickFormatter"},"label_standoff":6,"location":[0,0],"major_label_text_font_size":{"value":"5pt"},"ticker":{"id":"2401","type":"BasicTicker"}},"id":"2403","type":"ColorBar"},{"attributes":{"children":[{"id":"2407","type":"Select"}]},"id":"2408","type":"Row"},{"attributes":{"dimension":1,"grid_line_color":null,"ticker":{"id":"2367","type":"CategoricalTicker"}},"id":"2369","type":"Grid"},{"attributes":{"desired_num_ticks":10},"id":"2401","type":"BasicTicker"},{"attributes":{"callback":null,"height":50,"js_property_callbacks":{"change:value":[{"id":"2406","type":"CustomJS"}]},"options":["Cividis","Gray","Inferno","Magma","Viridis","Turbo"],"title":"Color Palette","value":"cividis","width":200},"id":"2407","type":"Select"},{"attributes":{"callback":null,"factors":["AXISBANK","BANDHANBNK","BANKBARODA","FEDERALBNK","HDFCBANK","ICICIBANK","IDFCFIRSTB","INDUSINDBK","KOTAKBANK","PNB","RBLBANK","SBIN"]},"id":"2356","type":"FactorRange"},{"attributes":{},"id":"2420","type":"CategoricalTickFormatter"},{"attributes":{"fill_alpha":{"value":0.1},"fill_color":{"value":"#1f77b4"},"height":{"units":"data","value":1},"line_alpha":{"value":0.1},"line_color":{"value":"#1f77b4"},"width":{"units":"data","value":1},"x":{"field":"level_0"},"y":{"field":"parameters"}},"id":"2398","type":"Rect"},{"attributes":{},"id":"2367","type":"CategoricalTicker"},{"attributes":{},"id":"2378","type":"RedoTool"},{"attributes":{"fill_color":{"field":"correlation","transform":{"id":"2350","type":"LinearColorMapper"}},"height":{"units":"data","value":1},"line_color":{"value":null},"width":{"units":"data","value":1},"x":{"field":"level_0"},"y":{"field":"parameters"}},"id":"2397","type":"Rect"},{"attributes":{},"id":"2380","type":"ZoomOutTool"},{"attributes":{"bottom_units":"screen","fill_alpha":{"value":0.5},"fill_color":{"value":"lightgrey"},"left_units":"screen","level":"overlay","line_alpha":{"value":1.0},"line_color":{"value":"black"},"line_dash":[4,4],"line_width":{"value":2},"render_mode":"css","right_units":"screen","top_units":"screen"},"id":"2426","type":"BoxAnnotation"},{"attributes":{"callback":null,"factors":["AXISBANK","BANDHANBNK","BANKBARODA","FEDERALBNK","HDFCBANK","ICICIBANK","IDFCFIRSTB","INDUSINDBK","KOTAKBANK","PNB","RBLBANK","SBIN"]},"id":"2354","type":"FactorRange"},{"attributes":{"callback":null,"data":{"correlation":{"__ndarray__":"AAAAAAAA8D+bCITlVUThP/09Ry8J1tI/Btk4m6WW6D/4Tl64EQ/rP0N+RC7MWuY/uwFsO/ib5j/2JwV8+SvUP8VhV5vRAOQ/KqGE9An41T8AhbzjTYHaP7YnE2S9luw/mwiE5VVE4T8AAAAAAADwPymaOFKaEek/prqRokuo5j8OvZnAejndPzVFb0Leoak/vntIAuw75z+G9douPX3qP6Ny2l1s6co/pzDlpng85z8r+dluZYvlP7pQi6QhJOU//T1HLwnW0j8pmjhSmhHpPwAAAAAAAPA/tmJDbiVx5j84ThVFRxqxP0CHWKowKdO/SnpO+jzK5z9nNhir7lbtP5LuDDlUAse/z/irLkF+7j9ptb7UxLXqP2e0ZuJVt98/Btk4m6WW6D+mupGiS6jmP7ZiQ24lceY/AAAAAAAA8D8A3xOvYfjjP3xX8evA2NM/OcMfPMh66j+VE9Ijn3jlP9teBgmGB9g//PPKqVdn5z9X2BzpzHjlP76LMVRDwOo/+E5euBEP6z8OvZnAejndPzhOFUVHGrE/AN8Tr2H44z8AAAAAAADwP2yRo80M/+o/vWWSl4p13j939zqgUpfEP0hP54G+3Ow/sTMpZc8qsz+47XPf6JyrPyLkdgxOFuk/Q35ELsxa5j81RW9C3qGpP0CHWKowKdO/fFfx68DY0z9skaPNDP/qPwAAAAAAAPA/zabl38wSyj/ZSwMqJx3Pv7rHX/dvsOs/cPXilPHa0r8eL2bcSCjTv7D2z4DOnuI/uwFsO/ib5j++e0gC7DvnP0p6Tvo8yuc/OcMfPMh66j+9ZZKXinXeP82m5d/MEso/AAAAAAAA8D+u80aEu+LnPxHqmTKhrso/ZCmOEQop6j8SzHtyIxnmP/yibmSrP+Y/9icFfPkr1D+G9douPX3qP2c2GKvuVu0/lRPSI5945T939zqgUpfEP9lLAyonHc+/rvNGhLvi5z8AAAAAAADwPwlgcnhdxLW/HJ5evq5/7D/N/6CHTUDpP1tP8a6SQd8/xWFXm9EA5D+jctpdbOnKP5LuDDlUAse/214GCYYH2D9IT+eBvtzsP7rHX/dvsOs/EeqZMqGuyj8JYHJ4XcS1vwAAAAAAAPA/ZiIKwigDyr+sCaK8HD3Rv45ijVYH8+E/KqGE9An41T+nMOWmeDznP8/4qy5Bfu4//PPKqVdn5z+xMyllzyqzP3D14pTx2tK/ZCmOEQop6j8cnl6+rn/sP2YiCsIoA8q/AAAAAAAA8D9Dk005NgvsP9cte9abbN4/AIW8402B2j8r+dluZYvlP2m1vtTEteo/V9gc6cx45T+47XPf6JyrPx4vZtxIKNO/Esx7ciMZ5j/N/6CHTUDpP6wJorwcPdG/Q5NNOTYL7D8AAAAAAADwP0sYY+pQ2eA/ticTZL2W7D+6UIukISTlP2e0ZuJVt98/vosxVEPA6j8i5HYMThbpP7D2z4DOnuI//KJuZKs/5j9bT/GukkHfP45ijVYH8+E/1y171pts3j9LGGPqUNngPwAAAAAAAPA/","dtype":"float64","shape":[144]},"index":[0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,101,102,103,104,105,106,107,108,109,110,111,112,113,114,115,116,117,118,119,120,121,122,123,124,125,126,127,128,129,130,131,132,133,134,135,136,137,138,139,140,141,142,143],"level_0":["AXISBANK","AXISBANK","AXISBANK","AXISBANK","AXISBANK","AXISBANK","AXISBANK","AXISBANK","AXISBANK","AXISBANK","AXISBANK","AXISBANK","BANDHANBNK","BANDHANBNK","BANDHANBNK","BANDHANBNK","BANDHANBNK","BANDHANBNK","BANDHANBNK","BANDHANBNK","BANDHANBNK","BANDHANBNK","BANDHANBNK","BANDHANBNK","BANKBARODA","BANKBARODA","BANKBARODA","BANKBARODA","BANKBARODA","BANKBARODA","BANKBARODA","BANKBARODA","BANKBARODA","BANKBARODA","BANKBARODA","BANKBARODA","FEDERALBNK","FEDERALBNK","FEDERALBNK","FEDERALBNK","FEDERALBNK","FEDERALBNK","FEDERALBNK","FEDERALBNK","FEDERALBNK","FEDERALBNK","FEDERALBNK","FEDERALBNK","HDFCBANK","HDFCBANK","HDFCBANK","HDFCBANK","HDFCBANK","HDFCBANK","HDFCBANK","HDFCBANK","HDFCBANK","HDFCBANK","HDFCBANK","HDFCBANK","ICICIBANK","ICICIBANK","ICICIBANK","ICICIBANK","ICICIBANK","ICICIBANK","ICICIBANK","ICICIBANK","ICICIBANK","ICICIBANK","ICICIBANK","ICICIBANK","IDFCFIRSTB","IDFCFIRSTB","IDFCFIRSTB","IDFCFIRSTB","IDFCFIRSTB","IDFCFIRSTB","IDFCFIRSTB","IDFCFIRSTB","IDFCFIRSTB","IDFCFIRSTB","IDFCFIRSTB","IDFCFIRSTB","INDUSINDBK","INDUSINDBK","INDUSINDBK","INDUSINDBK","INDUSINDBK","INDUSINDBK","INDUSINDBK","INDUSINDBK","INDUSINDBK","INDUSINDBK","INDUSINDBK","INDUSINDBK","KOTAKBANK","KOTAKBANK","KOTAKBANK","KOTAKBANK","KOTAKBANK","KOTAKBANK","KOTAKBANK","KOTAKBANK","KOTAKBANK","KOTAKBANK","KOTAKBANK","KOTAKBANK","PNB","PNB","PNB","PNB","PNB","PNB","PNB","PNB","PNB","PNB","PNB","PNB","RBLBANK","RBLBANK","RBLBANK","RBLBANK","RBLBANK","RBLBANK","RBLBANK","RBLBANK","RBLBANK","RBLBANK","RBLBANK","RBLBANK","SBIN","SBIN","SBIN","SBIN","SBIN","SBIN","SBIN","SBIN","SBIN","SBIN","SBIN","SBIN"],"parameters":["AXISBANK","BANDHANBNK","BANKBARODA","FEDERALBNK","HDFCBANK","ICICIBANK","IDFCFIRSTB","INDUSINDBK","KOTAKBANK","PNB","RBLBANK","SBIN","AXISBANK","BANDHANBNK","BANKBARODA","FEDERALBNK","HDFCBANK","ICICIBANK","IDFCFIRSTB","INDUSINDBK","KOTAKBANK","PNB","RBLBANK","SBIN","AXISBANK","BANDHANBNK","BANKBARODA","FEDERALBNK","HDFCBANK","ICICIBANK","IDFCFIRSTB","INDUSINDBK","KOTAKBANK","PNB","RBLBANK","SBIN","AXISBANK","BANDHANBNK","BANKBARODA","FEDERALBNK","HDFCBANK","ICICIBANK","IDFCFIRSTB","INDUSINDBK","KOTAKBANK","PNB","RBLBANK","SBIN","AXISBANK","BANDHANBNK","BANKBARODA","FEDERALBNK","HDFCBANK","ICICIBANK","IDFCFIRSTB","INDUSINDBK","KOTAKBANK","PNB","RBLBANK","SBIN","AXISBANK","BANDHANBNK","BANKBARODA","FEDERALBNK","HDFCBANK","ICICIBANK","IDFCFIRSTB","INDUSINDBK","KOTAKBANK","PNB","RBLBANK","SBIN","AXISBANK","BANDHANBNK","BANKBARODA","FEDERALBNK","HDFCBANK","ICICIBANK","IDFCFIRSTB","INDUSINDBK","KOTAKBANK","PNB","RBLBANK","SBIN","AXISBANK","BANDHANBNK","BANKBARODA","FEDERALBNK","HDFCBANK","ICICIBANK","IDFCFIRSTB","INDUSINDBK","KOTAKBANK","PNB","RBLBANK","SBIN","AXISBANK","BANDHANBNK","BANKBARODA","FEDERALBNK","HDFCBANK","ICICIBANK","IDFCFIRSTB","INDUSINDBK","KOTAKBANK","PNB","RBLBANK","SBIN","AXISBANK","BANDHANBNK","BANKBARODA","FEDERALBNK","HDFCBANK","ICICIBANK","IDFCFIRSTB","INDUSINDBK","KOTAKBANK","PNB","RBLBANK","SBIN","AXISBANK","BANDHANBNK","BANKBARODA","FEDERALBNK","HDFCBANK","ICICIBANK","IDFCFIRSTB","INDUSINDBK","KOTAKBANK","PNB","RBLBANK","SBIN","AXISBANK","BANDHANBNK","BANKBARODA","FEDERALBNK","HDFCBANK","ICICIBANK","IDFCFIRSTB","INDUSINDBK","KOTAKBANK","PNB","RBLBANK","SBIN"]},"selected":{"id":"2424","type":"Selection"},"selection_policy":{"id":"2425","type":"UnionRenderers"}},"id":"2349","type":"ColumnDataSource"},{"attributes":{"args":{"cir":{"id":"2399","type":"GlyphRenderer"},"col_sch":{"Cividis":["#FFE945","#FFE542","#FBE147","#F6DE4C","#F1DA50","#EDD654","#E8D257","#E4CE5B","#E0CB5D","#DCC860","#D7C462","#D3C065","#CEBD67","#CAB969","#C6B66B","#C2B36C","#BEB06E","#BAAC6F","#B6A971","#B1A672","#ADA273","#A99F74","#A69C75","#A29976","#9E9676","#9A9377","#968F77","#928C78","#8E8978","#8A8678","#878478","#838178","#7F7E78","#7B7B78","#787876","#757575","#717273","#6F6F72","#6B6D71","#686A70","#64676F","#60646E","#5D616E","#595E6D","#565C6C","#52596C","#4E566B","#4A536B","#46506B","#424E6B","#3D4B6B","#38486B","#35466B","#2F436B","#29406B","#223D6C","#1A3A6C","#0E376D","#00346E","#00326E","#002F6F","#002D6D","#002A66","#00285F","#002558","#002251","#00204C"],"Gray":["#ffffff","#fbfbfb","#f7f7f7","#f3f3f3","#efefef","#ebebeb","#e7e7e7","#e3e3e3","#e0e0e0","#dcdcdc","#d8d8d8","#d4d4d4","#d0d0d0","#cccccc","#c8c8c8","#c5c5c5","#c1c1c1","#bdbdbd","#b9b9b9","#b5b5b5","#b1b1b1","#adadad","#aaaaaa","#a6a6a6","#a2a2a2","#9e9e9e","#9a9a9a","#969696","#929292","#8e8e8e","#8b8b8b","#878787","#838383","#7f7f7f","#7b7b7b","#777777","#737373","#707070","#6c6c6c","#686868","#646464","#606060","#5c5c5c","#585858","#555555","#515151","#4d4d4d","#494949","#454545","#414141","#3d3d3d","#393939","#363636","#323232","#2e2e2e","#2a2a2a","#262626","#222222","#1e1e1e","#1b1b1b","#171717","#131313","#0f0f0f","#0b0b0b","#070707","#030303","#000000"],"Inferno":["#FCFEA4","#F6FA95","#F2F485","#F1EE74","#F1E864","#F3E056","#F5D948","#F7D13C","#F8CB34","#F9C32A","#FABB21","#FBB318","#FBAC10","#FBA40A","#FB9D06","#FA9706","#F99008","#F8880C","#F68111","#F47A16","#F2741C","#EF6D21","#ED6825","#E9622A","#E65C2E","#E25733","#DD5238","#D94D3D","#D44841","#CF4446","#CB4049","#C53D4D","#BF3951","#BA3655","#B43358","#AE305B","#A72D5F","#A32B61","#9C2963","#962666","#902468","#892269","#83206B","#7D1D6C","#781C6D","#72196D","#6B176E","#65156E","#5F126E","#58106D","#520E6C","#4B0C6B","#460A69","#400966","#390962","#32095D","#2B0A56","#240B4E","#1D0C45","#190B3E","#130A34","#0E082A","#090621","#050418","#020210","#010007","#000003"],"Magma":["#FBFCBF","#FCF5B7","#FCEEB0","#FCE6A8","#FDDFA1","#FDD89A","#FDD193","#FEC98D","#FEC488","#FEBC82","#FEB57C","#FEAE76","#FEA671","#FD9F6C","#FD9768","#FC9265","#FB8A62","#FA825F","#F97B5D","#F7735C","#F56C5B","#F3655C","#F0605D","#ED595F","#E85461","#E44E64","#DE4A67","#D9466A","#D3426D","#CD3F70","#C83D72","#C23A75","#BB3877","#B53679","#AE347B","#A7317D","#A12F7E","#9C2E7F","#952C80","#8F2A80","#892881","#822581","#7C2381","#762181","#711F81","#6B1C80","#651A80","#5E177F","#58157E","#52127C","#4B1079","#450F76","#400F73","#390F6E","#321067","#2B115E","#251155","#1F114B","#1A1041","#160E3A","#110C31","#0D0A28","#09071F","#050417","#02020F","#010007","#000003"],"Turbo":["#7a0402","#870801","#940c01","#a01101","#ac1601","#b61c01","#c02302","#c92903","#d02f04","#d73606","#de3e08","#e5460a","#ea500d","#ef5a11","#f46516","#f76e1a","#fa7a1f","#fc8624","#fd9229","#fe9e2e","#fda932","#fbb336","#f9ba38","#f6c23a","#f0cb3a","#ead339","#e3da37","#dae236","#d1e834","#c8ee33","#c0f233","#b6f735","#acfa37","#a1fc3d","#95fe44","#87fe4d","#78fe59","#6dfd62","#5dfb6f","#4df97c","#3ff58a","#32f197","#27eda3","#1ee8af","#1ae4b6","#17debf","#18d7ca","#1bcfd4","#20c6df","#26bde9","#2db4f1","#34aaf8","#39a2fc","#3f98fe","#438efd","#4584f9","#467af2","#4670e8","#4666dd","#455ed2","#4353c2","#4148b0","#3f3d9c","#3c3285","#38266c","#341b51","#30123b"],"Viridis":["#FDE724","#F3E51E","#E9E419","#DFE318","#D4E11A","#CAE01E","#BFDF24","#B5DD2B","#ADDC30","#A2DA37","#97D83E","#8DD644","#83D34B","#79D151","#70CE56","#69CC5B","#60C960","#57C665","#4FC369","#47C06E","#40BD72","#39B976","#35B778","#2FB37B","#2AB07E","#26AC81","#23A883","#20A585","#1FA187","#1E9D88","#1E9A89","#1E978A","#1F938B","#208F8C","#228B8D","#23888D","#24848D","#26818E","#277D8E","#287A8E","#2A768E","#2C728E","#2D6E8E","#2F6A8D","#30678D","#32638D","#345F8D","#365B8C","#38578C","#3A538B","#3C4E8A","#3D4A89","#3F4788","#414286","#423D84","#443982","#45347F","#462F7C","#472A79","#472676","#482172","#481C6E","#471669","#471163","#460B5E","#450558","#440154"]},"color_bar":{"id":"2403","type":"ColorBar"},"high":1.0,"low":-0.2993890441449487},"code":"\n    // JavaScript code goes here\n    var chosen_color = cb_obj.value;\n    var color_mapper = new Bokeh.LinearColorMapper({palette:col_sch[chosen_color], low:low, high:high});\n    cir.glyph.fill_color = {field: 'correlation', transform: color_mapper};\n    color_bar.color_mapper.low = low;\n    color_bar.color_mapper.high = high;\n    color_bar.color_mapper.palette = col_sch[chosen_color];\n    "},"id":"2406","type":"CustomJS"},{"attributes":{},"id":"2373","type":"ResetTool"},{"attributes":{},"id":"2425","type":"UnionRenderers"},{"attributes":{"children":[{"id":"2351","subtype":"Figure","type":"Plot"}]},"id":"2409","type":"Row"},{"attributes":{},"id":"2377","type":"UndoTool"},{"attributes":{"callback":null,"tooltips":[["Parameters","@level_0 - @parameters"],["Correlation","@correlation"]]},"id":"2370","type":"HoverTool"},{"attributes":{"high":1.0,"low":-0.2993890441449487,"palette":["#FFE945","#FFE542","#FBE147","#F6DE4C","#F1DA50","#EDD654","#E8D257","#E4CE5B","#E0CB5D","#DCC860","#D7C462","#D3C065","#CEBD67","#CAB969","#C6B66B","#C2B36C","#BEB06E","#BAAC6F","#B6A971","#B1A672","#ADA273","#A99F74","#A69C75","#A29976","#9E9676","#9A9377","#968F77","#928C78","#8E8978","#8A8678","#878478","#838178","#7F7E78","#7B7B78","#787876","#757575","#717273","#6F6F72","#6B6D71","#686A70","#64676F","#60646E","#5D616E","#595E6D","#565C6C","#52596C","#4E566B","#4A536B","#46506B","#424E6B","#3D4B6B","#38486B","#35466B","#2F436B","#29406B","#223D6C","#1A3A6C","#0E376D","#00346E","#00326E","#002F6F","#002D6D","#002A66","#00285F","#002558","#002251","#00204C"]},"id":"2350","type":"LinearColorMapper"},{"attributes":{"text":"Correlation Matrix"},"id":"2352","type":"Title"},{"attributes":{"grid_line_color":null,"ticker":{"id":"2363","type":"CategoricalTicker"}},"id":"2365","type":"Grid"}],"root_ids":["2410"]},"title":"Bokeh Application","version":"1.4.0"}}
        </script>
        <script type="text/javascript">
          (function() {
            var fn = function() {
              Bokeh.safely(function() {
                (function(root) {
                  function embed_document(root) {

                  var docs_json = document.getElementById('2515').textContent;
                  var render_items = [{"docid":"b1cbeec3-d674-46ae-a871-081f04b6158e","roots":{"2410":"b1acd5a7-bd8a-4cba-a320-3eb0a9d2a7b6"}}];
                  root.Bokeh.embed.embed_items(docs_json, render_items);

                  }
                  if (root.Bokeh !== undefined) {
                    embed_document(root);
                  } else {
                    var attempts = 0;
                    var timer = setInterval(function(root) {
                      if (root.Bokeh !== undefined) {
                        clearInterval(timer);
                        embed_document(root);
                      } else {
                        attempts++;
                        if (attempts > 100) {
                          clearInterval(timer);
                          console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing");
                        }
                      }
                    }, 10, root)
                  }
                })(window);
              });
            };
            if (document.readyState != "loading") fn();
            else document.addEventListener("DOMContentLoaded", fn);
          })();
        </script>
  </body>

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