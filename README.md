# GSOC 2018 - Performance Analytics Standard Errors

## Logistics

* Student Developer: [Anthony Christidis](https://www.stat.ubc.ca/users/anthony-christidis) ([anthony.christidis@stat.ubc.ca](anthony.christidis@stat.ubc.ca))
* Project Proposal: [Performance Analytics Standard Errors](https://drive.google.com/open?id=1J8bPaL-230V42wpGpXs7YHJSusYYKTrf)
* Mentors: [Dr. Douglas Martin](https://amath.washington.edu/people/douglas-martin), [Brian Peterson](http://www.braverock.com/brian/), [Xin Chen](https://amath.washington.edu/people/xin-chen) and [Dr. Ruben Zamar](https://www.stat.ubc.ca/~ruben/website/)

## Project Background

The current finance industry practice in reporting risk and performance measure estimates of assets
and portfolios does not typically include reporting the standard error of these estimates: consumers have
no clue as to how accurate those estimates are. A new approach based on influence functions has been developed to provide an accurate estimate of standard errors of risk and performance of assets and portfolios for returns with both serially uncorrelated and serially correlated returns. This project involves: 
1. Developing a new *R* package named **InfluenceFunctions** with full documentation and integrating it into the **EstimatorStandardError** *R* package, 
2. Extending the *R* package named **glmGammaNet** to support both the Gamma and Exponential distributions as it relates to fitting these distributions to the spectral density of the influence functions of risk measures, and integrate this package into **EstimatorStandardError**, and
3. Integrating the *R* package **EstimatorStandardError** into the existing *R* package **PerformanceAnalytics** (along with a working vignette)

with the goal of giving the **PerformanceAnalytics** package users more functionality and the option for the first time to report the standard errors of a very wide range of risk and performance measure estimates of assets and portfolios when returns are serially correlated as well as when they are uncorrelated.

## Repository Links

As explained in the previous section, this project contains multiple software packages that are interconnected to each other (see [project proposal](https://drive.google.com/open?id=1J8bPaL-230V42wpGpXs7YHJSusYYKTrf) for more details). Below are some details, notes and examples for each of theses packages.

### *InfluenceFunctions* Package

* Repositorty Link: [*InfluenceFunctions*](https://github.com/AnthonyChristidis/InfluenceFunctions)

* R code to download the package directly from Github: 
```
devtools::install_github("AnthonyChristidis/InfluenceFunctions", build_vignette=TRUE)
```

* Package Details: This package was created entirely during the project. It computes the influence functions time series of various risk and performance measures. The computation is available both in *R* and *C++*, with pre-whitening and robust filtering options of the time series available as well. Additional functions to plot the influence functions of risk and performance measures are also available. A plot method is also available (implementation is in **ggplot2**).

* Sample Code:
```
# Load package
library(InfluenceFunctions)

# Load data
data(edhec)
data(CA.fund)

# Compute basic TS for the mean 
if.mean <- IF.mean(edhec$CA)
plot(if.mean)
# We can also use the wrapper function IF
if.mean <- IF(edhec$CA, risk="mean")

# ggplot2 implementation of the IF TS plot
IF.TS.plot(if.mean)

# Repeat the above but use pre-whitening and robust estimation
if.mean.pre.robust <- IF.mean(edhec$CA, pre.whiten=TRUE, robust.filtering=TRUE, robust.method="Boudt")
plot(if.mean.pre.robust)

# We also have another robust filtering method available
if.mean.pre.robust2 <- IF.mean(edhec$CA, pre.whiten=TRUE, robust.filtering=TRUE, robust.method="Martin")
plot(if.mean.pre.robust2)

# Compare the two IF TS
plot(if.mean, col="black")
lines(if.mean.pre.robust, col="red")
lines(if.mean.pre.robust2, col="blue")

# Plot IF for various risk measures
IF.mean.plot()
IF.SD.plot()
IF.VaR.plot()
IF.ES.plot()
IF.SR.plot()

# Again, we can use the wrapper function
IF.plot(risk="OmegaRatio")
```

* Note: For the full details of the **InfluenceFunctions** package and for more detailed sample code with output, refer to the vignette available after the installation of the package.
```
browseVignettes("InfluenceFunctions")
```


### *glmnetRcpp* Package

* Repositorty Link: [*glmnetRcpp*](https://github.com/AnthonyChristidis/glmnetRcpp)

* R code to download the package directly from Github: 
```
devtools::install_github("AnthonyChristidis/glmnetRcpp")
```

* Package Details: This package fits the exponential distribution with an elastic net penalty to the influence functions time series in the computation of the standard errors of risk and performance measures. The packages was initially developed by Xin Chen and Daniel Hanson ([*glmnetRcpp*](https://github.com/chenx26/glmnetRcpp)).

### *glmGammaNet* Package

* Repositorty Link: [*glmGammaNet*](https://github.com/AnthonyChristidis/glmGammaNet)

* R code to download the package directly from Github: 
```
devtools::install_github("AnthonyChristidis/glmGammaNet")
```

* Package Details: The main task for this package was to merge the [*glmnetRcpp*](https://github.com/AnthonyChristidis/glmnetRcpp) package to the **glmGammaNet** package, such that the overall function from **glmGammaNet** supports both the Exponential and Gamma distributions. This package was then implemented into the **EstimatorStandardError** package. Initially, only the Exponential distribution from **glmnetRcpp** was available to fit the spectral density of the influence functions of the risk and performance measures.

This issue will need to be fixed. In the meantime, it is recommended to download the package files from [here](https://github.com/AnthonyChristidis/glmGammaNet), and to install the package via the command prompt. Here is a [link](http://web.mit.edu/insong/www/pdf/rpackage_instructions.pdf) on how to do so (see Section 3, *Building R Package with Command Line Tools*).

### *EstimatorStandardError* Package

* Repositorty Link: [*EstimatorStandardError*](https://github.com/AnthonyChristidis/EstimatorStandardError)

* R code to download the package directly from Github: 
```
devtools::install_github("AnthonyChristidis/EstimatorStandardError")
```

* Package Details: The main task for this package was to integrate the **InfluenceFunctions** and **glmGammaNet** packages into **EstimatorStandardError**. This allows the user to use both pre-whitening and robust filtering of the influence functions time series prior to fitting either the Exponential or Gamma distributions.

* Sample Code: 
```
#--------- Load package
library(EstimatorStandardError)

# Load data
data(edhec)
colnames(edhec)=c("CA", "CTAG", "DIS", "EM",
                  "EMN", "ED", "FIA", "GM", "L/S",
                  "MA", "RV", "SS", "FoF")
                  
#--------- Set parameters                  
nboot=500
return.coeffs = TRUE
d.GLM.EN = 5
alpha.ES = 0.05
alpah.EN = 0.5
seed = 12345
k_fold_iter = 1000
set.seed(seed)

#--------- Expected Shortfall estimates and their S.E.'s
se.ES <- ES.SE(edhec$CA, p = 1 - alpha.ES, method="historical",nsim = nboot,
               se.method = c("IFiid","IFcor","BOOTiid","BOOTcor"),
               standardize = FALSE, return.coeffs = return.coeffs,
               k_fold_iter = k_fold_iter,
               d.GLM.EN = d.GLM.EN,
               alpha = alpha.EN, exponential.dist = FALSE)

se.ES.exp <- ES.SE(edhec$CA, p = 1 - alpha.ES, method="historical",nsim = nboot,
                   se.method = c("IFcor"),
                   standardize = FALSE, return.coeffs = return.coeffs,
                   k_fold_iter = k_fold_iter,
                   d.GLM.EN = d.GLM.EN, clean = "none", prewhiten = FALSE,
                   alpha = alpha.EN, exponential.dist = TRUE)

#--------- Sortino Ratios estimates and their S.E.'s
se.SoR.const <- SortinoRatio.SE(edhec$CA, sortino.method = "const",
                                se.method = c("IFiid","IFcor","BOOTiid","BOOTcor"),
                                exponential.dist=TRUE)

se.SoR.mean <- SortinoRatio.SE(edhec$CA, sortino.method = "mean",
                               se.method = c("IFiid","IFcor","BOOTiid","BOOTcor"),
                               exponential.dist=TRUE)

#--------- Sharpe Ratio estimates and their S.E.'s
se.SR <- SharpeRatio.SE(edhec$CA, Rf = 0, p = 0.95, FUN="StdDev", nsim=nboot,
                        se.method = c("IFiid","IFcor","BOOTiid","BOOTcor"),
                        weights=NULL, annualize = FALSE,
                        exponential.dist=TRUE)
```

### *PerformanceAnalytics* Package

* Repositorty Link: [*PerformanceAnalytics*](https://github.com/AnthonyChristidis/PerformanceAnalytics)

* R code to download the package directly from Github: 
```
devtools::install_github("AnthonyChristidis/PerformanceAnalytics")
```

* Package Details: The main task for this package, which was the final step, was to integrate the computation of standard errors for risk and performance measures into **PerformanceAnalytics**. It was implemented for all risk and performance measures available in the package. A vignette is also being developed to demonstrate the new functionality of **PerformanceAnalytics** to compute standard errors for risk and performance measures.

* Sample Code:
```
#--------- Load package
library(PerformanceAnalytics)

#--------- Load data
data(edhec)
colnames(edhec)=c("CA", "CTAG", "DIS", "EM",
                  "EMN", "ED", "FIA", "GM", "L/S",
                  "MA", "RV", "SS", "FoF")
                  
#--------- Set parameters                  
nboot=500
return.coeffs = TRUE
d.GLM.EN = 5
alpha.ES = 0.05
alpah.EN = 0.5
seed = 12345
k_fold_iter = 1000
set.seed(seed)


#--------- Expected Shortfall
ES(edhec, p=.95, method="historical", 
   SE=TRUE, se.method=c("IFiid","IFcor","BOOTiid","BOOTcor"), exponential.dist=TRUE)

#--------- Value-at-Risk
VaR(edhec, p=.95, method="historical", SE=TRUE, se.method=c("IFiid","IFcor","BOOTiid","BOOTcor"), exponential.dist=TRUE)

#--------- Standard Deviation
StdDev(edhec, SE=TRUE, se.method=c("IFiid","IFcor","BOOTiid","BOOTcor"), exponential.dist=TRUE)

```



