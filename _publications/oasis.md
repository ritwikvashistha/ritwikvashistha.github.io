---
title: "OASIS: Observation-Aware Simulation-Based Inference via Distributional Matching"
authors: "A. Farahi, C. Zhou, R. Vashistha"
collection: publications
category: preprints
permalink: /publication/oasis
excerpt: 'A framework for scientific inference when observations contain measurement errors, selection biases, and survey-related distortions using MMD-based reweighting.'
date: 2026-06-01
venue: 'arXiv preprint'
paperurl: 'https://arxiv.org/abs/2606.22572'
arxiv: '2606.22572'
keywords:
  - simulation-based inference
  - measurement error
  - selection bias
  - maximum mean discrepancy
  - galaxy clusters
  - astronomical surveys
  - survey distortions
  - likelihood-free inference
  - posterior estimation
  - observational astronomy
---
## Abstract
We present OASIS, a framework designed for scientific inference when real-world observations contain measurement errors, selection biases, and other survey-related distortions. The method addresses a key challenge: simulators typically generate ideal, noise-free quantities, while actual data passes through complex observational processing. Rather than comparing raw observations to simulator outputs or relying on limited summaries, OASIS constructs a pseudo-posterior by reweighting prior samples according to a maximum mean discrepancy (MMD) loss between the empirical distributions of observed and simulated data. This approach avoids both manual feature engineering and learned neural approximations. The framework includes theoretical results regarding Monte Carlo consistency and parameter concentration. Testing demonstrates robust recovery in regression scenarios with various noise types, and application to galaxy cluster astronomy reveals effectiveness with nonlinear relationships, heterogeneous errors, selection functions, and incomplete observational coverage.

## Links
- [Paper Link](https://arxiv.org/abs/2606.22572)
