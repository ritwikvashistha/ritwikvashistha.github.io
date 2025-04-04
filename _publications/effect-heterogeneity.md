---
title: "Effect Heterogeneity with Earth Observation in Randomized Controlled Trials: Exploring the Role of Data, Model, and Evaluation Metric Choice"
collection: publications
category: preprints
permalink: /publication/effect-heterogeneity
excerpt: 'This paper looks at how different modelling choices affect treatment effect heterogeneity for earth observation data'
date: 2024-07-16
venue: 'arXiv'
status: 'arxiv'
paperurl: 'https://arxiv.org/abs/2407.11674'
---

## Abstract
Many social and environmental phenomena are associated with macroscopic changes in the built environment, captured by satellite imagery on a global scale and with daily temporal resolution. While widely used for prediction, these images and especially image sequences remain underutilized for causal inference, especially in the context of randomized controlled trials (RCTs), where causal identification is established by design. In this paper, we develop and compare a set of general tools for analyzing Conditional Average Treatment Effects (CATEs) from temporal satellite data that can be applied to any RCT where geographical identifiers are available. Through a simulation study, we analyze different modeling strategies for estimating CATE in sequences of satellite images. We find that image sequence representation models with more parameters generally yield a greater ability to detect heterogeneity. To explore the role of model and data choice in practice, we apply the approaches to two influential RCTs -- Banerjee et al. (2015), a poverty study in Cusco, Peru, and Bolsen et al. (2014), a water conservation experiment in Georgia, USA. We benchmark our image sequence models against image-only, tabular-only, and combined image-tabular data sources, summarizing practical implications for investigators in a multivariate analysis. Land cover classifications over satellite images facilitate interpretation of what image features drive heterogeneity. We also show robustness to data and model choice of satellite-based generalization of the RCT results to larger geographical areas outside the original. Overall, this paper shows how satellite sequence data can be incorporated into the analysis of RCTs, and provides evidence about the implications of data, model, and evaluation metric choice for causal analysis.

##  Links
- [Paper PDF](https://arxiv.org/abs/2407.11674)
- [Code](https://github.com/AIandGlobalDevelopmentLab/causalimages-software)
