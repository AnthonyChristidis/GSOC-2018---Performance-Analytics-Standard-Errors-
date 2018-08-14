## GSOC 2018 - Performance Analytics Standard Errors

The current finance industry practice in reporting risk and performance measure estimates of assets
and portfolios does not typically include reporting the standard error of these estimates: consumers have
no clue as to how accurate those estimates are. With the recent work of Chen and Martin (2018), a new
approach based on influence functions has been developed to provide an accurate estimate of standard
errors of risk and performance of assets and portfolios for returns with both serially uncorrelated and seri-
ally correlated returns. This project involves: 
1. Developing a new R package named InfluenceFunctions, and
2. Integrating the R package EstimatorStandardError in conjunction with InfluenceFunctions
into the existing R package PerformanceAnalytics, with the goal of giving PerformanceAnalytics pack-
age users more functionality and the option for the rst time to report the standard errors of a very
wide range of risk and performance measure estimates of assets and portfolios when returns are serially
correlated as well as when they are uncorrelated.

Anthony Christidis ([anthony.christidis@stat.ubc.ca](anthony.christidis@stat.ubc.ca))
