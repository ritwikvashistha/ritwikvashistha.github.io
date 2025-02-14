---
title: "Application of statistical machine learning in biomarker selection"
collection: publications
category: manuscripts
permalink: /publication/biomarker-selection
excerpt: 'This paper is about doing biomarker selection in JAVELIN Bladder 100 phase 3 trial data'
date: 2023-10-26
venue: 'Nature Scientific Reports'
paperurl: 'https://www.nature.com/articles/s41598-023-45323-9'
---
## Abstract
In the recent JAVELIN Bladder 100 phase 3 trial, avelumab plus best supportive care significantly prolonged overall survival relative to best supportive care alone as first-line maintenance therapy following first-line platinum-based chemotherapy in patients with advanced urothelial cancer (aUC). Discovering biomarkers using genomic profiling to understand potential patient heterogeneity is essential to help improve patient care with precision medicine. For the JAVELIN Bladder 100 trial, it is unclear which variable selection methods can most reliably identify biomarkers to inform patient care because the dataset is characterized by high collinearity and low signal. The aim of this paper was to evaluate available selection methods and their ability to discover prognostic and predictive biomarkers in patients with aUC receiving first-line maintenance therapy. A simulation study evaluated the performance of popular variable selection approaches for high-dimensional data including penalized regression models, random survival forests, and Bayesian variable selection methods. For Bayesian variable selection methods, a modified Bayesian Information Criterion (BIC) thresholding rule was proposed in addition to the traditional BIC thresholding rule. These methods were applied to the JAVELIN Bladder 100 dataset to investigate potential biomarkers associated with survival benefit. Results from the simulations demonstrated the strengths and limitations of the different methods. The variable selection methods demonstrated low false discovery rates under different conditions. However, their performance declined in the presence of high collinearity. Using the JAVELIN Bladder 100 data, we identified some potentially significant biomarkers across multiple models. Several lasso-related methods were able to identify potentially biologically meaningful variables in the trial. Some variable selection methods (such as stochastic search variable selection and random survival forest) may not be well suited to this type of data due to the presence of extreme collinearity and low signal. Future research should explore novel variable selection methods that may be more suitable for identifying prognostic and predictive biomarkers in this population.

##  Links
- [Paper Link](https://www.nature.com/articles/s41598-023-45323-9)
