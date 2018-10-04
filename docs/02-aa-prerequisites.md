
# Prerequisits: Basic statistical terms {#basics}

This chapter introduces some important terms useful for doing data analyses.
It also introduces the essentials of the classical frequentist tests such as t- and Chisquare test. We will not use them later but we think it is important to know how to interpret the results in order to be able to understand 100 years of scientific literature. For each classical test, we provide a suggestion how to do it in a Bayesian way and we discuss some differences between the Bayesian and frequentist statistics. 


## Scale of measurement


Scale   | Examples          | Properties        | Coding in R | 
:-------|:------------------|:------------------|:--------------------|
Nominal | Sex, genotype, habitat  | Identity (values have a unique meaning) | `factor()` |
Ordinal | Elevational zones | Identity and magnitude (values have an ordered relationship) | `ordered()` |
Numeric | Discrete: counts;  continuous: body weight, wing length | Identity, magnitude, and equal intervals | `intgeger()` `numeric()` |



## Correlations

### Basics of correlations
  
- variance $\hat{\sigma^2} = s^2 = \frac{1}{n-1}\sum_{i=1}^{n}(x_i-\bar{x})^2$  
  
    
      
- standard deviation $\hat{\sigma} = s = \sqrt{s^2}$  
  
    
  
- covariance $q = \frac{1}{n-1}\sum_{i=1}^{n}((x_i-\bar{x})*(y_i-\bar{y}))$  


### Pearson correlation coefficient
  
standardized covariance

  $r=\frac{\sum_{i=1}^{n}(x_i-\bar{x})(y_i-\bar{y})}{\sqrt{\sum_{i=1}^{n}(x_i-\bar{x})^2\sum_{i=1}^{n}(y_i-\bar{y})^2}}$



### Spearman correlation coefficient
rank correlation  
correlation between rank(x) and rank(y)  
  
  robust against outliers

### Kendall's tau
rank correlation  

I = number of pairs (i,k) for which $(x_i < x_k)$ & $(y_i > y_k)$ or viceversa  
$\tau = 1-\frac{4I}{(n(n-1))}$



## Principal components analyses PCA
rotation of the coordinate system

<div class="figure" style="text-align: left">
<img src="02-aa-prerequisites_files/figure-html/unnamed-chunk-2-1.png" alt="Principal components are eigenvectors of the covariance or correlation matrix" width="240" />
<p class="caption">(\#fig:unnamed-chunk-2)Principal components are eigenvectors of the covariance or correlation matrix</p>
</div>



rotation of the coordinate system so that   

* first component explains most variance  
* second component explains most of the remaining variance and is perpendicular to the first one  
* third component explains most of the remaining variance and is perpendicular to the first two  
* ...  

$(x,y)$ becomes $(pc1, pc2)$  
where  
$pc1_i= b_{11} x_i + b_{12} y_i$  
$pc2_i = b_{21} x_i + b_{22} y_i$ with $b_{jk}$ being loadings


```r
pca <- princomp(cbind(x,y), cor=TRUE)
loadings(pca)
```

```
## 
## Loadings:
##   Comp.1 Comp.2
## x  0.707  0.707
## y  0.707 -0.707
## 
##                Comp.1 Comp.2
## SS loadings       1.0    1.0
## Proportion Var    0.5    0.5
## Cumulative Var    0.5    1.0
```
loadings of a component can be multiplied by -1


proportion of variance explained by each component  
number of components = number of variables

```r
summary(pca)
```

```
## Importance of components:
##                           Comp.1    Comp.2
## Standard deviation     1.2899873 0.5795971
## Proportion of Variance 0.8320336 0.1679664
## Cumulative Proportion  0.8320336 1.0000000
```
outlook: components with low variance are shrinked to a higher degree in Ridge regression


### Inferential statistics

> there is never a "yes-or-no" answer  
> there will always be uncertainty  
Amrhein (2017)[https://peerj.com/preprints/26857]

The decision whether an effect is important or not cannot not be done based on data alone. For a decision we should carefully consider the consequences of each decision, the aims we would like to achieve and the data. Consequences, needs and wishes of different stakeholders can be formally combined with the information in data by using methods of the decision theory. In most data analyses, particularly in basic research and when working on case studies, we normally do not consider consequences of decisions. In these cases, our job is extracting the information of data so that this information later can be used by other scientists, stakeholders and politicians to make decisions.

Therefore, statistics is describing pattern in data and quantifying the uncertainty of the described patterns that is due to the fact that the data is just a (small) random sample from the population we would like to know. 

Quantification of uncertainty is only possible if  
  
1. the mechanisms under study are known
2. the observations are a random sample from the population of interest

Solutions:  
to 1. working with models and reporting assumptions  
to 2. study design

> reported uncertainties always are too small!


Example: Number of stats courses before starting a PhD among all PhD students

```r
# simulate the virtual true data
set.seed(235325)   # set seed for random number generator

# simulate fake data of the whole population
statscourses <- rpois(300000, rgamma(300000, 2, 3))  

# draw a random sample from the population
n <- 12            # sample size
y <- sample(statscourses, 12, replace=FALSE)         
```


<img src="02-aa-prerequisites_files/figure-html/unnamed-chunk-6-1.png" width="432" style="display: block; margin: auto;" />



We observe the sample mean, what do we know about the population mean?  

Frequentist solution: How would the sample mean scatter, if we repeat the study many times?  

Bayesian solution: For any possible value, what is the probability that it is the true population mean?  

<img src="02-aa-prerequisites_files/figure-html/unnamed-chunk-7-1.png" width="336" style="display: block; margin: auto;" />


## Standard deviation and standard error  

<img src="02-aa-prerequisites_files/figure-html/unnamed-chunk-8-1.png" width="672" style="display: block; margin: auto;" />



frequentist SE = SD/sqrt(n)  
  
Bayesian SE = SD of posterior distribution


## Central limit theorem / law of large numbers
  
    
<img src="02-aa-prerequisites_files/figure-html/unnamed-chunk-9-1.png" width="432" style="display: block; margin: auto;" />



normal distribution = Gaussian distribution  
 
 $p(\theta) = \frac{1}{\sqrt{2\pi}\sigma}exp(-\frac{1}{2\sigma^2}(\theta -\mu)^2) = Normal(\mu, \sigma)$  
   
     
       
   $E(\theta) = \mu$, $var(\theta) = \sigma^2$, $mode(\theta) = \mu$


  
    
<img src="02-aa-prerequisites_files/figure-html/unnamed-chunk-10-1.png" width="432" style="display: block; margin: auto;" />




## Bayes theorem

  
  
$P(A|B) = \frac{P(B|A)P(A)}{P(B)}$  

car    | flowers        | wine           | **sum**               | 
:------|:---------------|:---------------|:------------------|
no | 2  | 1  | **3** |
yes   | 2  | 2  | **4** |
-------|----------------|----------------|-------------------|
**sum**    | **4**| **3**| **7** |

What is the probability that the person likes wine given it has no car?  
$P(A) =$ likes wine $= 0.43$  
$P(B) =$ no car $= 0.43$   

$P(B|A) =$ proportion car-free people among the wine liker $= 0.33$

Knowing whether a persons owns a car increases the knowledge of the birthday preference.


## Bayes theorem for continuous parameters

$p(\theta|y) = \frac{p(y|\theta)p(\theta)}{p(y)} = \frac{p(y|\theta)p(\theta)}{\int p(y|\theta)p(\theta) d\theta}$    


$p(\theta|y)$: posterior distribution

$p(y|\theta)$: likelihood, data model

$p(\theta)$: prior distribution

$p(y)$: scaling constant



## Single parameter model

$p(y|\theta) = Norm(\theta, \sigma)$, with $\sigma$ known 
  
  
$p(\theta) = Norm(\mu_0, \tau_0)$  

$p(\theta|y) = Norm(\mu_n, \tau_n)$, where
 $\mu_n= \frac{\frac{1}{\tau_0^2}\mu_0 + \frac{n}{\sigma^2}\bar{y}}{\frac{1}{\tau_0^2}+\frac{n}{\sigma^2}}$ and
 $\frac{1}{\tau_n^2} = \frac{1}{\tau_0^2} + \frac{n}{\sigma^2}$
  
    
  $\bar{y}$ is a sufficient statistics  
  $p(\theta) = Norm(\mu_0, \tau_0)$ is a conjugate prior for $p(y|\theta) = Norm(\theta, \sigma)$, with $\sigma$ known.



Posterior mean = weighted average between prior mean and $\bar{y}$ with weights
equal to the precisions ($\frac{1}{\tau_0^2}$ and $\frac{n}{\sigma^2}$)
<img src="02-aa-prerequisites_files/figure-html/unnamed-chunk-12-1.png" width="2800" style="display: block; margin: auto;" />



## A model with two parameters
$p(y|\theta, \sigma) = Norm(\theta, \sigma)$ 
  
\begin{center}
  \includegraphics[width=0.5\textwidth]{images/snowfinch2.jpg}
\end{center}


```r
# weight (g)
y <- c(47.5, 43, 43, 44, 48.5, 37.5, 41.5, 45.5)
n <- length(y)
```



$p(y|\theta, \sigma) = Norm(\theta, \sigma)$ 
  
    
$p(\theta, \sigma) = N-Inv-\chi^2(\mu_0, \sigma_0^2/\kappa_0; v_0, \sigma_0^2)$ conjugate prior

  
$p(\theta,\sigma|y) = \frac{p(y|\theta, \sigma)p(\theta, \sigma)}{p(y)} = N-Inv-\chi^2(\mu_n, \sigma_n^2/\kappa_n; v_n, \sigma_n^2)$, with  

  
$\mu_n= \frac{\kappa_0}{\kappa_0+n}\mu_0 + \frac{n}{\kappa_0+n}\bar{y}$  
  
  $\kappa_n = \kappa_0+n$  
  
  $v_n = v_0 +n$  
  
  $v_n\sigma_n^2=v_0\sigma_0^2+(n-1)s^2+\frac{\kappa_0n}{\kappa_0+n}(\bar{y}-\mu_0)^2$

  
 $\bar{y}$ and $s^2$ are sufficient statistics  

Joint, marginal and conditional posterior distributions
<img src="02-aa-prerequisites_files/figure-html/unnamed-chunk-14-1.png" width="384" style="display: block; margin: auto;" />



## t-distribution
marginal posterior distribution of a normal mean with unknown variance and conjugate prior distribution  

  
$p(\theta|v,\mu,\sigma) = \frac{\Gamma((v+1)/2)}{\Gamma(v/2)\sqrt{v\pi}\sigma}(1+\frac{1}{v}(\frac{\theta-\mu}{\sigma})^2)^{-(v+1)/2}$  


$v$ degrees of freedom  
$\mu$ location  
$\sigma$ scale



## Frequentist one-sample t-test
H0: the mean weight is equal to exactly 40g.  

$t = \frac{\bar{y}-\mu_0}{\frac{s}{\sqrt{n}}}$

```r
t.test(y, mu=40)
```

```
## 
## 	One Sample t-test
## 
## data:  y
## t = 3.0951, df = 7, p-value = 0.01744
## alternative hypothesis: true mean is not equal to 40
## 95 percent confidence interval:
##  40.89979 46.72521
## sample estimates:
## mean of x 
##   43.8125
```


## Nullhypothesis test
p-value: Probability of the data or more extreme data given the null hypothesis is true.

<img src="02-aa-prerequisites_files/figure-html/unnamed-chunk-16-1.png" width="768" style="display: block; margin: auto;" />


## Confidence interval

```r
# lower limit of 95% CI
mean(y) + qt(0.025, df=7)*sd(y)/sqrt(n) 
# upper limit of 95% CI
mean(y) + qt(0.975, df=7)*sd(y)/sqrt(n) 
```


<img src="02-aa-prerequisites_files/figure-html/unnamed-chunk-18-1.png" width="576" style="display: block; margin: auto;" />



## Posterior distribution
<img src="02-aa-prerequisites_files/figure-html/unnamed-chunk-19-1.png" width="768" style="display: block; margin: auto;" />


 Two different theories - one single result!


## Posterior probability
Probability $P(H:\mu<=40) =$ 0.01
<img src="02-aa-prerequisites_files/figure-html/unnamed-chunk-20-1.png" width="768" style="display: block; margin: auto;" />

## Monte Carlo simulation (parametric bootstrap)  
  
Monte Carlo integration: numerical solution of $\int_{-1}^{1.5} F(x) dx$ 
<img src="02-aa-prerequisites_files/figure-html/unnamed-chunk-21-1.png" width="768" style="display: block; margin: auto;" />


sim is solving a mathematical problem by simulation
How sim is simulating to get the marginal distribution of $\mu$:

<img src="02-aa-prerequisites_files/figure-html/unnamed-chunk-22-1.png" width="384" style="display: block; margin: auto;" />


## 3 methods for getting the posterior distribution

* analytically
* approximation
* Monte Carlo simulation



## Grid approximation
  
$p(\theta|y) = \frac{p(y|\theta)p(\theta)}{p(y)}$ 
  
For example, one coin flip (Bernoulli model) 
  
data: y=0  (a tail)  
likelihood: $p(y|\theta)=\theta^y(1-\theta)^{(1-y)}$


<img src="02-aa-prerequisites_files/figure-html/unnamed-chunk-23-1.png" width="480" style="display: block; margin: auto;" />


## Monte Carlo simulations

* Markov chain Monte Carlo simulation (BUGS, Jags)
* Hamiltonian Monte Carlo (Stan)

<img src="02-aa-prerequisites_files/figure-html/unnamed-chunk-24-1.png" width="960" />


## Comparison of the locations between two groups 
Boxplot:  
Median, 50% box, extremes observation within 1.5 times the interquartile range, outliers  

The uncertainties of the means do not show the uncertainty of the difference between the means!  

<img src="02-aa-prerequisites_files/figure-html/unnamed-chunk-25-1.png" width="480" />

## Difference between two means


```r
mod <- lm(ell~birthday, data=dat)
mod
```

```
## 
## Call:
## lm(formula = ell ~ birthday, data = dat)
## 
## Coefficients:
##  (Intercept)  birthdaywine  
##       37.250         1.083
```

```r
bsim <- sim(mod, n.sim=nsim)
quantile(bsim@coef[,2], prob=c(0.025, 0.5, 0.975))
```

```
##      2.5%       50%     97.5% 
## -6.938854  1.094108  9.176722
```


## Two-sample t-test

```r
t.test(ell~birthday, data=dat, var.equal=TRUE)
```

```
## 
## 	Two Sample t-test
## 
## data:  ell by birthday
## t = -0.33541, df = 5, p-value = 0.7509
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -9.385932  7.219266
## sample estimates:
## mean in group flower   mean in group wine 
##             37.25000             38.33333
```



## Wilxocon test

```r
wilcox.test(ell~birthday, data=dat)
```

```
## 
## 	Wilcoxon rank sum test with continuity correction
## 
## data:  ell by birthday
## W = 6.5, p-value = 1
## alternative hypothesis: true location shift is not equal to 0
```


## Randomisation test

```r
diffH0 <- numeric(nsim)
for(i in 1:nsim){
  randbirthday <- sample(dat$birthday)
  rmod <- lm(ell~randbirthday, data=dat)
  diffH0[i] <- coef(rmod)[2]
}
mean(abs(diffH0)>abs(coef(mod)[2])) # p-value
```

```
## [1] 0.7094
```

<img src="02-aa-prerequisites_files/figure-html/unnamed-chunk-30-1.png" width="288" />


* Produces the distribution of a test statistics given the null hypothesis.  
* assumption: all observations are independent  
* becomes unfeasible when data is structured



## Bootstrap

```r
diffboot <- numeric(nsim)
for(i in 1:nsim){
  nbirthday <- 1
  while(nbirthday==1){
    bootrows <- sample(1:nrow(dat), replace=TRUE)
    nbirthday <- length(unique(dat$birthday[bootrows]))
  }
  rmod <- lm(ell~birthday, data=dat[bootrows,])
  diffboot[i] <- coef(rmod)[2]
}
quantile(diffboot, prob=c(0.025, 0.975))
```

```
##      2.5%     97.5% 
## -4.200000  8.333333
```


* result is a confidence interval  
* assumption: all observations are independent!


```r
hist(diffboot); abline(v=coef(mod)[2], lwd=2, col="red")
```

<img src="02-aa-prerequisites_files/figure-html/unnamed-chunk-32-1.png" width="768" />


## F-test
Comparison of two variances  
H0: Var(X1)=Var(X2) -> $F = \frac{Var(X1)}{Var(X2)} \approx 1$  
even more complicated density function than the t-distribution!
<img src="02-aa-prerequisites_files/figure-html/unnamed-chunk-33-1.png" width="768" />


* We have not yet met any Bayesian example where the F-distribution is used.
* is used in the frequentist version of ANOVA


## Analysis of variance ANOVA
Aim: comparison between means  
Method: comparison of between-group with within-group variance
<img src="02-aa-prerequisites_files/figure-html/unnamed-chunk-34-1.png" width="768" />

Total sum of squares (SS) =  SST = $\sum_1^n{(y_i-\bar{y})^2}$  
Within-group SS = SSW = $\sum_1^n{(y_i-\bar{y_g})^2}$: unexplained variance  
Between-group SS = SSB = $\sum_1^n{(\bar{y_g}-\bar{y})^2}$: explained variance  
<img src="02-aa-prerequisites_files/figure-html/unnamed-chunk-35-1.png" width="768" />



H0: $\bar{y_1}=\bar{y_2}=\bar{y_3}$  
  
Expectation given H0:  
  
* Between-group variance is due to natural variation (within-group variance)  
* SSB/df_between = SSW/df_within, where df_between= number of groups -1 and df_within = n-number of groups  
* MSB = SSB/df_between, MSW = SSW/df_within  
* MSB/MSW ~ F(df_between, df_within)



NEED TO INSERT A BAYESIAN ANAOVA HERE

## Chisquare test
* correlations between two categorical variables
* comparison of two distributions (goodness of fit)

```r
table(dat$birthday, dat$statsfeeling)
```

```
##         
##          negative neutral positive
##   flower        3       1        0
##   wine          1       1        1
```
expected values $E_{ij}$ given H0: rowsum*colsum/total

$\chi^2$ measures the difference between the observed $O_{ij}$ and expected $E_{ij}$ values as:  
$\chi^2=\sum_{i=1}^{m}\sum_{j=1}^{k}\frac{(O_{ij}-E_{ij})^2}{E_{ij}}$    
The $\chi^2$-distribution has 1 parameter, the degrees of freedom $v$ = $(m-1)(k-1)$.
<img src="02-aa-prerequisites_files/figure-html/unnamed-chunk-37-1.png" width="672" />


```r
chisq.test(table(dat$birthday, dat$statsfeeling))
```

```
## 
## 	Pearson's Chi-squared test
## 
## data:  table(dat$birthday, dat$statsfeeling)
## X-squared = 1.8958, df = 2, p-value = 0.3875
```
no cell should have a count less than 5...

## Bayesian way of analysing correlations between categorical variables
* log-linear model (Poisson model) for the counts
* estimating proportions using a binomial or a multinomial model




```r
# log-linear model
mod <- glm(count~gift+feel + gift:feel, 
           data=datagg, family=poisson)
bsim <- sim(mod, n.sim=nsim)
round(t(apply(bsim@coef, 2, quantile, 
              prob=c(0.025, 0.5, 0.975))),2)
```

```
##                            2.5%     50%    97.5%
## (Intercept)               -0.02    1.10     2.26
## giftwine                  -3.35   -1.09     1.13
## feelneutral               -3.37   -1.10     1.22
## feelpositive          -82593.73 -217.91 85128.08
## giftwine:feelneutral      -2.48    1.10     4.69
## giftwine:feelpositive -85129.15  220.68 82595.26
```
the interaction parameters measure the strength of the correlation  




```r
# binomial model
tab <- table(dat$statsfeeling,dat$birthday)
mod <- glm(tab~rownames(tab),  family=binomial)
bsim <- sim(mod, n.sim=nsim)
```

<img src="02-aa-prerequisites_files/figure-html/unnamed-chunk-42-1.png" width="768" />


## Summary
Bayesian data analysis = applying the Bayes theorem for summarizing knowledge based on data, priors and the model assumptions.  

Frequentist statistics = quantifying uncertainty by hypothetical repetitions  
p-values are not wrong per se but they lead scientists and politicians to wrong decisions.