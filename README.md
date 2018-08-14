# GSOC 2018 - Performance Analytics Standard Errors

## Logistics

* Student Developer: Anthony Christidis ([anthony.christidis@stat.ubc.ca](anthony.christidis@stat.ubc.ca))
* Project Proposal: [Performance Analytics Standard Errors](https://drive.google.com/open?id=1J8bPaL-230V42wpGpXs7YHJSusYYKTrf)
* Mentors: 1. [anthony.christidis@stat.ubc.ca](anthony.christidis@stat.ubc.ca)
2. [anthony.christidis@stat.ubc.ca](anthony.christidis@stat.ubc.ca)

## Project Background

The current finance industry practice in reporting risk and performance measure estimates of assets
and portfolios does not typically include reporting the standard error of these estimates: consumers have
no clue as to how accurate those estimates are. A new approach based on influence functions has been developed to provide an accurate estimate of standard errors of risk and performance of assets and portfolios for returns with both serially uncorrelated and serially correlated returns. This project involves: 
1. Developing a new R package named **InfluenceFunctions** and integrating it into the **EstimatorStandardError** R package, 
2. Extending the R package named **glmGammaNet** to support both the Gamma and Exponential distributions when applying, and integrate this package into **EstimatorStandardError**, and
3. Integrating the R package **EstimatorStandardError** into the existing R package PerformanceAnalytics

with the goal of giving the **PerformanceAnalytics** package users more functionality and the option for the first time to report the standard errors of a very wide range of risk and performance measure estimates of assets and portfolios when returns are serially correlated as well as when they are uncorrelated.


