---
layout: post
title: "Portfolio Allocation & Efficient Frontier Generation"
date: 2020-05-31 22:00:00 +0530
description: ' "Risk to Spiderman ko bhi lena padta hai, main to phir bhi salesman hoon" - Ranbir Kapoor from movie Rocket Singh '
categories: [ tech, python, data_visualization, bokeh ]
image: assets/thumbnails/thumbnail_2020-05-31-portfolio-allocation-and-efficient-frontier-generation.jpg
tags: featured
author: raghav
---

[nifty_bank_stocks_lognormal_daily_returns_img_path]: ../assets/post_imgs/2020-05-31-portfolio-allocation-and-efficient-frontier-generation/NIFTYBANK_df_log_returns_histogram.png
[nifty_bank_stocks_cumulative_returns_img_path]: ../assets/post_imgs/2020-05-31-portfolio-allocation-and-efficient-frontier-generation/NIFTYBANK_df_norm_returns.png
[nifty_bank_hiiks_efficient_frontier_img_path]: ../assets/post_imgs/2020-05-31-portfolio-allocation-and-efficient-frontier-generation/NIFTYBANK_HIIKS_portfolio_efficient_frontier.png
[nifty_bank_efficient_frontier_img_path]: ../assets/post_imgs/2020-05-31-portfolio-allocation-and-efficient-frontier-generation/NIFTYBANK_portfolio_efficient_frontier.png
[nifty_bank_hiiks_efficient_frontier_gif_path]: ../assets/post_imgs/2020-05-31-portfolio-allocation-and-efficient-frontier-generation/NIFTYBANK_HIIKSportfolio_efficient_frontier_gif.gif


Hello reader, ever heard of the terms _Asset/Portfolio allocation_? If you have, chances are you've also heard about the MPT - Modern Portfolio Theory and Markovitz Efficient Frontier! Well this post and project are quite technical and will be based on these topics.

So sometime back, I finished the famous [**Python For Financial Analysis and Algorithmic Trading**](https://www.udemy.com/course/python-for-finance-and-trading-algorithms/) by the genius instructor [**Jose Portilla**](https://www.linkedin.com/in/jmportilla/), on _Udemy_.
In this project, I attempt to reuse the code provided in the course, extend it for generalization in Python and add some great interactive visualizations using Bokeh. I chose to work on NIFTYBANK Index Portfolio.

P.S: Like always all code can be found [here](https://github.com/raghavsikaria/Portfolio-Optimization-and-Efficient-Frontier) on GitHub.

**TL;DR** - This is what this project attempts to achieve:

![NIFTYBANK - HDFC,ICICI,INDUSIND,KOTAK,SBI - Efficient Portfolio Frontier][nifty_bank_hiiks_efficient_frontier_gif_path]

## Objective

+ Get some basic insights into the NIFTYBANK Index
+ Conduct Monte Carlo Simulation over a portfolio to find the optimal mix of weights with maximum sharpe ratio possible
+ Find and generate the efficient frontier for the portfolio
+ All the above objectives have to be aided with crisp and interactive visualizations

## Contents

1. [Data Generation](#)
2. [Data Exploration](#data-exploration)
    1. [Correlation between all NIFTYBANK Index stocks](#correlation-between-all-niftybank-index-stocks)
    2. [NIFTYBANK Index Stocks - Daily Lognormalized Returns](#niftybank-index-stocks---daily-lognormalized-returns)
    3. [NIFTYBANK Index Stocks - Normalized Cumulative Returns](#niftybank-index-stocks---normalized-cumulative-returns)
3. [Generating portfolios using Monte Carlo Simulation](#generating-portfolios-using-monte-carlo-simulation)
4. [Efficient Frontier Generation](#efficient-frontier-generation)
5. [Plotting the Efficient Frontier and Universe of Portfolios](#plotting-the-efficient-frontier-and-universe-of-portfolios)
6. [Acknowledgements](#acknowledgements)
7. [References](#references)


## Data Generation

Well, to work on a portfolio of NIFTYBANK we first need the data! The current index has 12 constituents of which Bandhan Bank was the newest entry(added when it went Public on 27th March 2018). Hence, for this project I have decided to work only on data from 4th April 2018 to 22th May 2020. Let's see what the index looks like today (As of 25th May 2020)

|Company|Symbol|Sector|Ownership|Closing Price|Market Cap (Cr.)|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Axis Bank|AXISBANK|Financial Services - Bank|Private|341.30|96,313.68|
| Bandhan Bank|BANDHANBNK|Financial Services - Bank|Private|202.15|32,551.63|
| Bank of Baroda|BANKBARODA|Financial Services - Bank|Public|37.10|17,142.30|
| Federal Bank|FEDERALBNK|Financial Services - Bank|Private|38.45|7,663.49|
| HDFC Bank|HDFCBANK|Financial Services - Bank|Private|852.40|467,451.36|
| ICICI Bank|ICICIBANK|Financial Services - Bank|Private|292.70|189,517.39|
| IDFC First Bank|IDFCFIRSTB|Financial Services - Bank|Private|19.85|9,547.66|
| IndusInd Bank|INDUSINDBK|Financial Services - Bank|Private|348.20|24,149.75|
| Kotak Mahindra|KOTAKBANK|Financial Services - Bank|Private|1,153.20|220,699.92|
| PNB |PNB|Financial Services - Bank|Public|26.70|25,126.38|
| RBL Bank|RBLBANK|Financial Services - Bank|Private|110.55|5,623.80|
| SBI |SBIN|Financial Services - Bank|Public|151.40|135,118.62|

Pandas_datareader + yfinance are going to be our best friends for getting Stock data free of charge on the run!
This little code snippet should fix us up:

~~~ python
# Here are all our imports
import pandas_datareader.data as web
from pandas_datareader import data as pdr
import datetime
import pandas as pd
import yfinance as yf

# This is the magic line -> Essentially tells pandas_datareader to have Yahoo API backend engine
yf.pdr_override

# Giving time range for the data we need
start = datetime.datetime(2018, 4, 2)
end = datetime.datetime(2020, 5, 22)

# All constituents of NIFTYBANK Index
niftybank_stocks =['AXISBANK','BANDHANBNK','BANKBARODA','FEDERALBNK','HDFCBANK','ICICIBANK','IDFCFIRSTB','INDUSINDBK','KOTAKBANK','PNB','RBLBANK','SBIN']

# Creating our Data Frame object to store all stock data
index_col = pd.date_range(start=start, end=end)
nifty_bank_df = pd.DataFrame(index=index_col)
nifty_bank_df.index.name = 'Date'

# We iterate for all NIFTYBANK stocks, fetch their Adjusted Close
# Price from the API call and populate our data frame
for stock in niftybank_stocks:
    nifty_bank_df[f'{stock} Adjusted Close'] = pdr.get_data_yahoo(stock+'.NS', start, end)['Adj Close']

# Some basic cleaning required
# We have to remove all weekends!
nifty_bank_df = nifty_bank_df[(nifty_bank_df.index.dayofweek != 5)&(nifty_bank_df.index.dayofweek != 6)]

# And some other days which mostly are public holidays
# when Market is closed
nifty_bank_df = nifty_bank_df.dropna(axis=0,how='all')

# Finally we store our generated Data in CSV format
nifty_bank_df.to_csv('nifty_bank_df_2018-4-2_to_2020-5-22.csv',index_label='Date')
~~~


## Data Exploration

Let's try and churn out some insights from our generated data.

### Correlation between all NIFTYBANK Index stocks

I have generated this correlation matrix using one of my other projects. You can find the entire code [here](https://github.com/raghavsikaria/Bokeh_CorrelationMatrix) in GitHub. (Try it out here on this blog, it's interactive!)

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

~~~ python
df_log_daily_return = pd.DataFrame(index=df.index)
df_log_daily_return.index.name = 'Date'
for column in df.columns.values:
    df_log_daily_return[f'{column} Log Daily Return'] = np.log(df[column]/df[column].shift(1))
~~~

![NIFTYBANK Index Stocks - Daily Lognormalized Returns][nifty_bank_stocks_lognormal_daily_returns_img_path]

### NIFTYBANK Index Stocks - Normalized Cumulative Returns

I have taken the base day as 4th April 2020, and calculated the cumulative returns with respect to that day.

~~~ python
df_normalised_return = pd.DataFrame(index=df.index)
df_normalised_return.index.name = 'Date'
for column in df.columns.values:
    df_normalised_return[f'{column} Normed Return'] = df[column]/df.iloc[0][column]
~~~

![NIFTYBANK Index Stocks - Normalized Cumulative Returns][nifty_bank_stocks_cumulative_returns_img_path]

## Generating portfolios using Monte Carlo Simulation

Well if you've reached till here - this is the fun part! We are going to create many - thousands of portfolios from our NIFTYBANK Index stocks. This is the basic idea - we have 12 stocks in our portfolio. We have to come up with as many portfolios as possible by allocating different weights to each these stocks. These weights we'll be generating in a randomised manner over thousands of iterations. _Imagine_, each iteration will give us a new set of 12 random weights, each set of unique random weights will generate a new portfolio for us and each portfolio will have it's own risk (volatility) and it's return!
Is there a fixed number of iterations or portfolios that we have to generate? No. There is not because there are infinitely many portfolios possible. However, generating a few thousand portfolios ought to give us some idea.
Before we begin, some mythbusters:

1.  "Portfolio with the maximum return is the best." - **NO**
2.  "Portfolio with the minimum risk is the best." - **NO**
3.  "Is there any correct answer to these questions?" - **NO**

In a nutshell, the answers are completely subjective and depend on the investor - her/his ability, willingness, appetite, etc. Let's take this topic some other day!

Meanwhile let's go through some Python which will help us with our portfolios:
* Input variable _df_ is our previously created Data Frame which contains all our stock data

~~~ python
def generate_all_portfolios(df: 'Data Frame of all stocks', returns_mean: 'Mean Data Frame of all Stocks', returns_covariance: 'Covariance Data Frame of all stocks', number_of_portfolios: 'int' = 5000) -> 'dict':
    """Runs Monte Carlo simulation and creates desired number of portfolios for universe. Taken from Jose's abovementioned course!"""
    
    # Seeting seed for future reproducibility
    np.random.seed(55)

    number_of_stocks = len(df.columns)

    # Initializing portfolio particulars
    all_weights = np.zeros((number_of_portfolios,number_of_stocks))
    ret_arr = np.zeros(number_of_portfolios)
    vol_arr = np.zeros(number_of_portfolios)
    sharpe_arr = np.zeros(number_of_portfolios)

    # Beginning our iterations and generating all the portfolios
    for index in tqdm(range(number_of_portfolios)):

        # Here we generate the random weights for our stocks
        weights = np.array(np.random.random(number_of_stocks))

        # We make sure that all weights lie between 0 and 1
        weights = weights / np.sum(weights)
        all_weights[index,:] = weights

        # We calculate annualised returns for our created portfolio
        # assuming 252 trading days in an year
        ret_arr[index] = np.sum(returns_mean * weights *252)

        # We calculate the annualised volatility (standard deviation)
        # for our portfolio by exploiting some wicked Linear Algebra
        vol_arr[index] = np.sqrt(np.dot(weights.T, np.dot(returns_covariance * 252, weights)))

        # Finally we find out the Sharpe Ratio for our portfolio
        # assuming 0 (Zero) Risk Free rate
        sharpe_arr[index] = ret_arr[index]/vol_arr[index]
    
    return {
        'max_sharpe_ratio': sharpe_arr.max(),
        'max_sharpe_ratio_index': sharpe_arr.argmax(),
        'max_sharpe_ratio_return': ret_arr[sharpe_arr.argmax()],
        'max_sharpe_ratio_volatility': vol_arr[sharpe_arr.argmax()],
        'all_portfolio_weights': all_weights,
        'all_portfolio_returns': ret_arr,
        'all_portfolio_volatility': vol_arr,
        'all_portfolio_sharpe_ratio': sharpe_arr
    }
~~~


## Efficient Frontier Generation

Efficient Frontier in layman terms is the set of portfolios wherein we get set of portfolios with maximum return for any given risk, or lowest risk for any given return. And therefore, anyone opting for portfolio which lies inside the efficient frontier is essentially just taking on more risk which will not be rewarded with extra return i.e. sub-optimal portfolio! We'll be relying heavily on the _Scipy_ mathematical computation Python library for achieving this.

* Inputs _frontier_min_return_ and _frontier_max_return_ to the below function are simply the range of returns over which we wish to find optimal portfolios

~~~ python
from scipy.optimize import minimize

def generate_efficient_frontier(number_of_stocks: 'int', frontier_min_return: 'float', frontier_max_return: 'float', number_of_frontier_portfolios: 'int' = 100) -> 'Frontier Volatility & Returns, lists':
    """Generates efficient frontier for the given range of desirable portfolio returns. Taken from Jose's abovementioned course!"""
    
    frontier_volatility = []

    # We generate 'number_of_frontier_portfolios' number of returns between our return ranges
    # so that we can generate optimal portfolios amongst them!
    frontier_returns = np.linspace(frontier_min_return, frontier_max_return, number_of_frontier_portfolios)

    # Initial weights that we choose for our portfolio
    init_guess = [1/number_of_stocks]*number_of_stocks

    # Contraining the bounds for each weight between 0 & 1
    bounds = tuple([(0,1) for _ in range(number_of_stocks)])

    # Iterating over different returns and churning out
    # portfolios with minimum risk
    for possible_return in tqdm(frontier_returns):
        cons = ({'type':'eq','fun': PortfolioGenerator.check_sum},
                {'type':'eq','fun': lambda w: get_ret_vol_sr(w)[0] - possible_return}
            )
        
        # GROUND ZERO - the real magic of SCIPY-optimize happens here
        result = minimize(minimize_volatility,init_guess,method='SLSQP',bounds=bounds,constraints=cons)
        
        # Storing the volatility of the generated portfolio
        frontier_volatility.append(result['fun'])

    return frontier_volatility, frontier_returns
~~~


## Plotting the Efficient Frontier and Universe of Portfolios

And now at last, we have to visualise our universe of created portfolios and efficient frontier mentioned in above sections. I have deliberatly not embedded interactive versions of the plots here as they were making the site pretty heavy. You can head over to the [repository](https://github.com/raghavsikaria/Portfolio-Optimization-and-Efficient-Frontier) and checkout all the Plot HTML files and play around with them.

~~~ python
def plot_efficient_frontier(title: 'str', plot_save_path: 'str', plot_image_save_path: 'str', all_portfolio_volatility: 'np array', all_portfolio_returns: 'np array', all_portfolio_sharpe_ratio: 'np array', max_sharpe_ratio_location: '[x,y] Coordinates', max_sharpe_ratio: 'float', efficient_frontier_returns: 'list', efficient_frontier_volatility: 'list', plot_width: 'int' = 1000, plot_height: 'int' = 800, axes_labels: '[X-axis label, Y-axis label]' = ['Portfolio Volatility', 'Portfolio Returns']) -> 'Bokeh Plot Object':
    """Generates plot for the entire portfolio universe and efficient frontier curve."""

    # Creating a color mapper so that we have a smooth gradient type color 
    # effect for portfolios ranging from lowest to highest Sharpe Ratio
    mapper = LinearColorMapper(palette=inferno(256), low=all_portfolio_sharpe_ratio.min(), high=all_portfolio_sharpe_ratio.max())

    # Preparing main ColumnDataSource which will be fed to the Bokeh Plot
    source = ColumnDataSource(data=dict(all_portfolio_volatility = all_portfolio_volatility, all_portfolio_returns = all_portfolio_returns, all_portfolio_sharpe_ratio = all_portfolio_sharpe_ratio))

    # Initializing our Bokeh Plot
    p = figure(plot_width=plot_width, plot_height=plot_height, toolbar_location='right', x_axis_label=axes_labels[0], y_axis_label=axes_labels[1], tooltips=[('Sharpe Ratio', '@all_portfolio_sharpe_ratio'),('Portfolio Return', '@all_portfolio_returns'),('Portfolio Volatility', '@all_portfolio_volatility')])

    # Adding circular rings for Sharpe Ratios of each portfolio
    p.circle(x='all_portfolio_volatility', y='all_portfolio_returns', source=source, fill_color={'field': 'all_portfolio_sharpe_ratio', 'transform': mapper}, size=8)

    # Adding circular ring for Maximum Sharpe Ratio portfolio
    p.circle(x=max_sharpe_ratio_location[0], y=max_sharpe_ratio_location[1], fill_color='white', line_width=5, size=20)

    # Adding Text annotation for Maximum Sharpe Ratio portfolio
    maximum_sharpe_ratio_portfolio = Label(x=max_sharpe_ratio_location[0]+0.005, y=max_sharpe_ratio_location[1]+0.02, text=f'Maximum SR ~ {round(max_sharpe_ratio, 4)}', render_mode='css', border_line_color='black', border_line_alpha=1.0, background_fill_color='white', background_fill_alpha=0.8)
    p.add_layout(maximum_sharpe_ratio_portfolio)

    # Adding the Efficient Frontier curve to our plot
    p.line(x=efficient_frontier_volatility, y=efficient_frontier_returns, line_width=2, line_color='white', legend='Efficient Frontier', line_dash='dashed')

    # Adding a color bar to indicate about varying Sharpe Ratio
    color_bar = ColorBar(color_mapper=mapper, major_label_text_font_size="5pt",ticker=BasicTicker(desired_num_ticks=10),formatter=PrintfTickFormatter(format="%.1f"),label_standoff=6, border_line_color=None, location=(0, 0))
    p.add_layout(color_bar, 'right')

    # Adding title to our plot
    p.add_layout(Title(text=title, text_font_size="16pt"), 'above')
    p.title.text_font_size = '20pt'

    # Fixing up Plot properties
    p.background_fill_color = "black"
    p.background_fill_alpha = 0.9
    p.xgrid.grid_line_color = None
    p.ygrid.grid_line_color = None

    # Fixing up Plot legend
    p.legend.background_fill_alpha = 0.5
    p.legend.background_fill_color = 'black'
    p.legend.label_text_color = 'white'
    p.legend.click_policy = 'mute'

    # Saving the plot
    output_file(plot_save_path)
    save(p)
    export_png(p, filename = plot_image_save_path)

    return p
~~~

#### NIFTYBANK - HDFC,ICICI,INDUSIND,KOTAK,SBI - Efficient Portfolio Frontier

Owing to hardware limitation from my end, I first tried with only the abovementioned constituents of NIFTYBANK. Here is what the plot looks like:

![NIFTYBANK - HDFC,ICICI,INDUSIND,KOTAK,SBI - Efficient Portfolio Frontier][nifty_bank_hiiks_efficient_frontier_img_path]

However, am also adding what I came up with for the entire NIFTBANK portfolio universe. Please note this is *misleading*.
Reasons:
* Since the universe of 12 constituents is too big, we need several hundreds of thousands of iterations to even begin to get an idea about the universe. However, my hardware limitations allowed me only a few thousand portfolios which you see in the Plot.
* Nevertheless, the efficient frontier does give us some sort of an idea about our portfolio universe and what our possible optimal set of portfolios could have been
* The max Sharpe Ratio pointed out on this one, is only for the portfolios that we have generated from our limited number of iterations!

![NIFTYBANK - Efficient Portfolio Frontier][nifty_bank_efficient_frontier_img_path]


This was basically the entire project. If you're up for it, let's collaborate over [GitHub](https://github.com/raghavsikaria/Portfolio-Optimization-and-Efficient-Frontier) and make this better.
You can also reach out to me incase you have any queries pertaining to Bokeh or anything Python. Hope this helps!

## Acknowledgements

My sincere thanks to [Jose Portilla](https://www.linkedin.com/in/jmportilla/) and Udemy for curating and delivering one of the best Financial Analysis courses ever!

## References
* https://www.udemy.com/course/python-for-finance-and-trading-algorithms/
* https://www.niftyindices.com/indices/equity/sectoral-indices/nifty-bank
* https://economictimes.indiatimes.com/markets/nifty-bank/indexsummary/indexid-1913,exchange-50.cms
* https://www.moneycontrol.com/stocks/marketstats/indexcomp.php?optex=NSE&opttopic=indexcomp&index=23






> कर्मण्येवाधिकारस्ते मा फलेषु कदाचन ।   
> मा कर्मफलहेतुर्भुर्मा ते संगोऽस्त्वकर्मणि ॥  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-- Bhagavad Gita ॥

(Read about this Shloka from the Bhagavad Gita here at [www.pravakta.com](https://www.pravakta.com/karmanyevadhikaraste-maa-phaleshu-kadachan-i-e-karma-yoga/))