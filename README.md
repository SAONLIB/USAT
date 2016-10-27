### Description
USAT (Unified Score-based Association Test) uses a data-adaptive weighted score-based test statistic for testing association of multiple continuous phenotypes with a single genetic marker. The R function `usat` implements this association test. For details of this statistical method, please refer/cite:

Ray, D., Pankow, J.S., Basu, S. "[USAT: A Unified Score-based Association Test for Multiple
Phenotype-Genotype Analysis](http://onlinelibrary.wiley.com/doi/10.1002/gepi.21937/full)". *Genetic Epidemiology*, 40(1):20-34, 2016.

**Key Words:** GWAS; MANOVA; Multiple phenotypes; Multivariate analysis; Pleiotropy; Score test

### Requirements
R (>= 3.0.1), CompQuadForm, minqa, pracma, survey


### Changes
Version 1.1 - April 07, 2016
> First public release of the software.


### Usage

#### Simple example
```{r}
usat(Y, X, COV=NULL, na.check=TRUE)
```
#### Arguments
| Input | Description |
| ---: | --- |
| `Y` | The `nxK` phenotype matrix, where `n` is the number of individuals and `K` is the number of phenotypes. The joint association of all `K`  phenotypes with the single marker will be tested. `Y` needs to be in R matrix format. |
| `X` | The `nx1` column matrix for the single genetic marker, where `n` is the number of individuals. `X` needs to be in R matrix format. |
| `COV` | The `nxq` matrix of covariates that need to be adjusted in the model. `q` is the number of such covariates. `COV` needs to be in R matrix format. The default value is NULL, i.e., it is assumed there is no covariate in the model. |
| `na.check`| If value is TRUE (default), the code will check for presence of missing values (coded as `NA`). USAT requires complete observations and any individual with at least one missing value in either `Y`, `X` or `COV` will be removed. Removal of missing observations may substantially reduce the sample size `n` and hence power to detect association. If a substantial proportion of the individuals have missing data, it is recommended (if possible) to impute the missing values before using USAT. |

#### Value
| Output | Description |
| ---: | --- |
| `T.usat` | The value of the USAT test statistic (scalar). |
| `omg.opt` | The optimal weight $\omega$ based on a grid search over [0, 1]. |
| `p.usat` | The p-value of association based on the USAT statistic. |
| `n.obs` | Number of individuals (with complete observations) used for testing association. |


### A Working Example
```
source("usat_v1.1.R")

# simulate 2 phenotypes on 1000 individuals
library(MASS) # needed for multivariate normal simulation
Y<-mvrnorm(n=1000, mu=c(0,0), Sigma=matrix(c(1,0.2,0.2,1),2,2))

# simulate a single marker for 1000 individuals
X<-matrix(rbinom(n=1000, size=2, prob=0.2), ncol=1) # additive model

# apply USAT to test association
u.out<-usat(Y=Y, X=X, COV=NULL, na.check=FALSE)

# USAT test statistic and p-value
t<-u.out$T.usat
p<-u.out$p.usat
```

### Notes
1. The method USAT and its software is designed for multiple phenotypes from a random sample. If the ascertainment of individuals in the sample is non-random (e.g., in case-control retrospective study design), it is advisable to account for the sampling scheme (e.g., adjusting the sampling variable as a covariate) when using USAT. One may also use methods and tools designed for the analysis of secondary phenotypes. We proposed one such method (POM-PS) and its software is coming soon!

2. The method USAT and its software is designed for unrelated individuals. If you have two cohorts with overlapping samples and you want to analyse the combined sample, it is desirable to exclude the overlapping individuals, and any related individuals. 
 
3. If you wish to test genetic association of multiple traits (categorical and/or continuous) and you have access to summary statistics only, you may wait a little longer for the new software (metaUSAT) that is coming soon!
