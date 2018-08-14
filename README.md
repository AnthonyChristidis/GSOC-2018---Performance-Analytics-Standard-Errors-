# GSOC 2018 - Performance Analytics Standard Errors

## Logistics

* Student Developer: [Anthony Christidis](https://www.stat.ubc.ca/users/anthony-christidis) ([anthony.christidis@stat.ubc.ca](anthony.christidis@stat.ubc.ca))
* Project Proposal: [Performance Analytics Standard Errors](https://drive.google.com/open?id=1J8bPaL-230V42wpGpXs7YHJSusYYKTrf)
* Mentors: [Dr. Douglas Martin](https://amath.washington.edu/people/douglas-martin), [Brian Peterson](http://www.braverock.com/brian/), [Xin Chen](https://amath.washington.edu/people/xin-chen) and [Dr. Ruben Zamar](https://www.stat.ubc.ca/~ruben/website/)

## Project Background

The current finance industry practice in reporting risk and performance measure estimates of assets
and portfolios does not typically include reporting the standard error of these estimates: consumers have
no clue as to how accurate those estimates are. A new approach based on influence functions has been developed to provide an accurate estimate of standard errors of risk and performance of assets and portfolios for returns with both serially uncorrelated and serially correlated returns. This project involves: 
1. Developing a new *R* package named **InfluenceFunctions** and integrating it into the **EstimatorStandardError** *R* package, 
2. Extending the *R* package named **glmGammaNet** to support both the Gamma and Exponential distributions as it relates to fitting these distributions to the spectral density of the influence functions of risk measures, and integrate this package into **EstimatorStandardError**, and
3. Integrating the *R* package **EstimatorStandardError** into the existing *R* package PerformanceAnalytics

with the goal of giving the **PerformanceAnalytics** package users more functionality and the option for the first time to report the standard errors of a very wide range of risk and performance measure estimates of assets and portfolios when returns are serially correlated as well as when they are uncorrelated.

## Repository Links

As explained in the previous section, this project contains multiple software packages that are interconnected to each other (see project proposal for more details). Below are some details, notes and examples for each of theses packages.

### **InfluenceFunctions** Package


