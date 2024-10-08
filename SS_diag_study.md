---
title: "Sample Size Diagnostic Accuracy Study"
author: "A.Amstutz"
date: "2024-09-06"
output:
  html_document:
    keep_md: yes
    toc: TRUE
    toc_float: TRUE
    code_folding: hide
  pdf_document:
    toc: yes
---

## Load packages

```r
library(tidyverse)
library(kableExtra)
library(ggplot2) # survival/TTE analyses and other graphs
library(pwr) # package for standard sample size calculations: https://cran.r-project.org/web/packages/pwr/vignettes/pwr-vignette.html / https://bookdown.org/pdr_higgins/rmrwr/sample-size-calculations-with-pwr.html 
```

## Literature
* Akoglu et al: User's guide to sample size estimation in diagnostic accuracy studies:
https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9639742/
* Formula for Single-text design: If a new diagnostic test (new test or new to the study population) is investigated in a prospective cohort that the disease status and prevalence are known, this approach is preferred [Table 2, Equation 1]. Researchers try to be sure with a confidence level of 95% that their predetermined sensitivity or specificity lies within the marginal error of d (desired width of one-half of the confidence interval [CI]). Sensitivity and specificity values are ascertained by previously published data or clinician experience/judgment.
* Those formulas are defined using normal approximation to construct a confidence interval for the true sensitivity and specificity value with a confidence level of (1-α)% and a maximum marginal error of d. Se (sensitivity) and Sp (specificity) are predetermined values ascertained by previously published data or clinician experience/judgment. Estimated sample sizes should be adjusted for disease prevalence (Equations 4a and 4b)

## Sample Size calculation for Screening/Diagnostic Accuracy Study of rheumatic heart disease among school kids in Tansania
* Primary objective: To determine the sensitivity, specificity and accuracy of a screening program for rheumatic heart disease according to the 2023 WHF screening criteria in a cohort of children in a rural area in Tanzania incorporating focused handheld echocardiography performed by trained non-expert clinical officers versus comprehensive echocardiography performed by blinded medical personnel fully trained in echocardiography using high-end ultrasound equipment and expert reviewers.
* As the focus of this study is on screening, the main diagnostic parameter of interest for the sample size calculation is the sensitivity (in order to rule out RHD with highest possible certainty during screening).
* From a prior high-quality meta-analysis, including 15578 children, we assume a sensitivity of 0.79 [0·73–0·84] for handheld devices compared to full echocardiography in a screening setting.
* From prior evidence, we expect a prevalence of 3% RHD (2% with mild (stage A or B) rheumatic heart disease, 1% with advanced (stage C or D) rheumatic heart disease)
* We set the type 1 error to the conventional 5% (translates into 95% confidence interval) and aim for a marginal effect of maximum 5%, i.e., in 95 out of 100 studies, the true value of sensitivity will fall within the range of ± 5%.

* Use single-test design formula for Sensitivity: n1_Se = ((Z^2 * Se * (1-Se)) / d^2)
* Then, adjust for disease prevalence: N_Se = n1_Se/P

* Use single-test design formula for Specificity: n1_Sp = ((Z^2 * Sp * (1-Sp)) / d^2)
* Then, adjust for disease prevalence: N_Sp = n1_Sp/P

## Fix parameters

```r
Se <- 0.79 # expected sensitivity
Se_low <- 0.73 # lower confidence interval of expected sensitivity
Sp <- 0.85 # expected specificity
P <- 0.03 # prevalence
Z <- 1.96 # alpha error 5%, 95% CI
d <- 0.05 # margin of error (+/- 5%)
```

## Sample Size calculation

```r
n_Se <- ((Z^2*Se*(1-Se))/d^2)
N_Se <- n_Se/P

n_Se_low <- ((Z^2*Se_low*(1-Se_low))/d^2)
N_Se_low <- n_Se_low/P

n_Sp <- ((Z^2*Sp*(1-Sp))/d^2)
N_Sp <- n_Sp/(1-P)

# Print
cat("Sample Size for Sensitivity:", ceiling(N_Se), "\n")
```

```
## Sample Size for Sensitivity: 8498
```

```r
cat("Sample Size for Sensitivity (lower CI):", ceiling(N_Se_low), "\n")
```

```
## Sample Size for Sensitivity (lower CI): 10096
```

```r
cat("Sample Size for Specificity:", ceiling(N_Sp), "\n")
```

```
## Sample Size for Specificity: 202
```





