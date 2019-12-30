# GSOC 2018 - Performance Analytics Standard Errors

## Logistics

* Student Developer: [Anthony Christidis](https://www.stat.ubc.ca/users/anthony-christidis) ([anthony.christidis@stat.ubc.ca](anthony.christidis@stat.ubc.ca))
* Project Proposal: [Performance Analytics Standard Errors](https://drive.google.com/open?id=1J8bPaL-230V42wpGpXs7YHJSusYYKTrf)
* Mentors: [Dr. Douglas Martin](https://amath.washington.edu/people/douglas-martin), [Brian Peterson](http://www.braverock.com/brian/), [Xin Chen](https://amath.washington.edu/people/xin-chen) and [Dr. Ruben Zamar](https://www.stat.ubc.ca/~ruben/website/)

## Project Background

The current finance industry practice in reporting risk and performance measure estimates of assets
and portfolios does not typically include reporting the standard error of these estimates: consumers have
no clue as to how accurate those estimates are. A new approach based on influence functions has been developed to provide an accurate estimate of standard errors of risk and performance of assets and portfolios for returns with both serially uncorrelated and serially correlated returns. This project involves: 
1. Developing a new *R* package named **RPEIF** with full documentation,
2. Developing the *R* package named **RPEGLMEN** to support both the Gamma and Exponential distributions as it relates to fitting these distributions to the spectral density of the influence functions of risk measures, and integrate this package into **RPESE**, and
3. Integrating the *R* package **EstimatorStandardError** into the existing *R* package **PerformanceAnalytics** (along with a working vignette).

with the goal of giving the **PerformanceAnalytics** package users more functionality and the option for the first time to report the standard errors of a very wide range of risk and performance measure estimates of assets and portfolios when returns are serially correlated as well as when they are uncorrelated.

## Repository Links

As explained in the previous section, this project contains multiple software packages that are interconnected to each other (see [project proposal](https://drive.google.com/open?id=1J8bPaL-230V42wpGpXs7YHJSusYYKTrf) for more details). Below are some details, notes and examples for each of theses packages.

### *RPEIF* Package

* Repositorty Link: [*RPEIF*](https://github.com/AnthonyChristidis/RPEIF)


* You can install the **development** version from [GitHub](https://github.com/AnthonyChristidis/RPEIF).
```
devtools::install_github("AnthonyChristidis/RPEIF")
```
* You can install the **stable** version on [R CRAN](https://cran.r-project.org/package=RPEIF)
```
install.packages("RPEIF")
```

* Package Details: This package was created entirely during the project. It computes the influence functions time series of various risk and performance measures. The computation robust filtering options of the time series are available. Additional functions to plot the influence functions of risk and performance measures are also available.

* Sample Code:
```
# Sample Code
library(RPEIF)
# Computing the IF of the returns (with outlier cleaning and prewhitening)
# Loading the data
data(edhec, package="PerformanceAnalytics")
colnames(edhec) = c("CA", "CTAG", "DIS", "EM","EMN", "ED", "FIA",
                    "GM", "LS", "MA", "RV", "SS", "FoF")
outIF <- IF(risk="mean",
            returns=edhec[,"CA"], evalShape=FALSE, retVals=NULL, nuisPars=NULL,
            IFplot=TRUE, IFprint=TRUE,
            prewhiten=TRUE,
            cleanOutliers=TRUE, cleanMethod=c("locScaleRob", "Boudt")[1], eff=0.99, alpha.robust=0.05)
```

* Note: For the full details of the **RPEIF** package and for more detailed sample code with output, refer to the [RPEIF vignette](https://cran.r-project.org/web/packages/RPEIF/vignettes/RPEIFVignette.pdf) available after the installation of the package.
```
browseVignettes("RPEIF")
```


### *RPEGLMEN* Package

* Repositorty Link: [*RPEGLMEN*](https://github.com/AnthonyChristidis/RPEGLMEN)

* You can install the **development** version from [GitHub](https://github.com/AnthonyChristidis/RPEGLMEN).
```
devtools::install_github("AnthonyChristidis/RPEGLMEN")
```
* You can install the **stable** version on [R CRAN](https://cran.r-project.org/package=RPEGLMEN)
```
install.packages("RPEGLMEN")
```

* Package Details: This package is designed to provide the user to fit an Exponential or Gamma distribution to the response variable with an elastic net penalty on the predictors. This package is of particular use in combination with the [RPEIF](https://github.com/AnthonyChristidis/RPEIF) and [RPESE](https://github.com/AnthonyChristidis/RPESE) packages, in which the influence function of a time series of returns is used to compute the standard error of a risk and performance measure. See [Chen and Martin (2018)](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3085672) for more details.

* Additional Reference: For the computational details to fit a Gamma distribution on data with an elastic net penalty, see [Chen, Arakvin and Martin (2018)](https://arxiv.org/abs/1804.07780).

* Note: For the full details of the **RPEGLMEN** package and for more detailed sample code with output, refer to the [RPEGLMEN vignette](https://cran.r-project.org/web/packages/RPEGLMEN/vignettes/RPEGLMENVignette.pdf) available after the installation of the package.
```
browseVignettes("RPEGLMEN")
```

### *RPESE* Package

* Repositorty Link: [*RPESE*](https://github.com/AnthonyChristidis/RPESE)

* You can install the **development** version from [GitHub](https://github.com/AnthonyChristidis/RPESE).
```
devtools::install_github("AnthonyChristidis/RPESE")
```

* You can install the **stable** version on [R CRAN](https://cran.r-project.org/package=RPESE)
```
install.packages("RPESE")
```

* Package Details: This package provides functions for computing standared error estimates for risk and performance measures of asset or portfolio returns. It works in combination with the [RPEIF](https://github.com/AnthonyChristidis/RPEIF) and [RPESE](https://github.com/AnthonyChristidis/RPESE) packages.


* Sample Code: 
```
# Sample Code
library(RPESE)
# Loading data from Performance Analytics
data(edhec, package="PerformanceAnalytics")
colnames(edhec) = c("CA", "CTAG", "DIS", "EM","EMN", "ED", "FIA",
                    "GM", "LS", "MA", "RV", "SS", "FoF")
# Computing the standard errors for the three influence functions based approaches
ES.out <- ES.SE(edhec, se.method=c("IFiid","IFcor","IFcorAdapt"),
          prewhiten=FALSE, 
          cleanOutliers=FALSE, 
          fitting.method=c("Exponential", "Gamma")[1])
# Print output
printSE(ES.out)
```
* Note: For the full details of the **RPESE** package and for more detailed sample code with output, refer to the [RPESE vignette](https://cran.r-project.org/web/packages/RPESE/vignettes/RPESEVignette.pdf) available after the installation of the package.
```
browseVignettes("RPESE")
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
   SE=TRUE, se.method=c("IFiid","IFcor","BOOTiid","BOOTcor"), fitting.method="Exponential")

#--------- Value-at-Risk
VaR(edhec, p=.95, method="historical", SE=TRUE, se.method=c("IFiid","IFcor","BOOTiid","BOOTcor"), fitting.method="Exponential")

#--------- Standard Deviation
StdDev(edhec, SE=TRUE, se.method=c("IFiid","IFcor","BOOTiid","BOOTcor"), fitting.method="Exponential")

```



