---
title: "Convolutional Maximum Mean Discrepancy for Inference in Noisy Data"
authors: "R. Vashistha, J. M. Phillips, A. Sarkar, A. Farahi"
collection: publications
category: preprints
permalink: /publication/convmmd
excerpt: 'A novel framework for inference with samples corrupted by potentially heteroscedastic noise using convolutional MMD.'
date: 2026-04-13
venue: 'arXiv preprint'
paperurl: 'https://arxiv.org/abs/2604.12022'
arxiv: '2604.12022'
keywords:
  - measurement error
  - errors-in-variables regression
  - maximum mean discrepancy
  - MMD
  - kernel methods
  - noisy data inference
  - heteroscedastic noise
  - statistical inference
  - distribution testing
  - noise-corrupted samples
---
*A short, plain-language walkthrough of the paper. For the formal statements, see the [abstract](#paper-abstract) and the [full paper](https://arxiv.org/abs/2604.12022).*

## The problem

Presence of measurement error or noise in the data is a big problem in modern statistics because it makes data analysis complicated. Datasets across fields as diverse as biology, economics, epidemiology, and astronomy are known to be often corrupted with noise or measurement error. In many of these settings the noise process or law is *known*. It may be quantified through calibration, replication, or physical modeling. However, many standard tools or models offer limited guidance on how to incorporate the information about the noise into formal inference. Ignoring measurement noise is not harmless because it can lead to biased estimation, inflated variance, and a loss of inferential power that can undermine scientific conclusions.

Kernel methods based on **Maximum Mean Discrepancy (MMD)** — which measures the distance between two distributions through their embeddings in a reproducing kernel Hilbert space (RKHS) — have become a popular tool for likelihood-free testing and estimation. But almost all existing MMD methods assume clean, noise-free observations, an assumption that may not be true for many scientific datasets. This paper closes that gap.

## The key idea

Instead of trying to correct for noise first and then do inference, we incorporate the noise directly into the definition of our statistical model. Given a known noise law \\(m\\), we compare two distributions \\(p\\) and \\(q\\) *only after both have been convolved with the noise* — a quantity we call the **convolutional MMD (convMMD)**:

$$\text{convMMD}(p, q, m) = \text{MMD}(p * m,\; q * m).$$

Two results make this practical:

- **It remains a valid metric.** Under standard kernel conditions, \\(\text{convMMD}(p,q,m) = 0\\) if and only if \\(p = q\\), so noisy samples can still uniquely identify the underlying distributions.
- **Noise simply widens the kernel.** For translation-invariant kernels, computing convMMD on noisy data is mathematically equivalent to computing ordinary MMD on the *clean* data with a smoother, wider-bandwidth kernel. In effect, the noise is absorbed into the kernel.

To estimate the parameters \\(\theta\\) of a model from noisy observations, we minimize the convMMD between the observed data and data simulated from the model (convolved with the same noise). This is a likelihood-free, simulation-based objective that avoids intractable integrals and is optimized efficiently with stochastic gradient descent. One of the main advantages of this idea is that it allows us to estimate the parameters directly without going through a two-step process: denoising data and then estimating parameters. 

The resulting estimator is **consistent and asymptotically normal**, and perhaps counterintuitively it retains the parametric \\(\sqrt{N}\\) convergence rate *despite* the measurement error. Noise does not degrade the rate; it only inflates the asymptotic variance by an explicitly characterized amount. The estimator also inherits MMD's robustness to outliers.

## Illustration 

<div style="text-align: center; margin: 2em 0;">
  <img src="/images/convmmd-density-deconvolution.png" alt="convMMD density deconvolution under Gaussian, Laplace, and Student's t noise" style="max-width: 100%; height: auto;">
  <p style="font-size: 0.9em; color: #666;"><em>Estimated latent density under three noise distributions. convMMD (purple) tracks the true density (black) even under heavy-tailed Laplace and Student's t noise, where the Gaussian-based method XDGMM (red) degrades.</em></p>
</div>

On a three-component Gaussian mixture, convMMD recovers the latent density from noisy samples. When the noise is Gaussian, it matches the specialized Bayesian method (XDGMM). But when the noise is heavy-tailed — Laplace or Student's t — the Gaussian-based methods degrade, since the Gaussian likelihood assigns small probability to outliers, while the convMMD estimator remains stable. This flexibility is the payoff of not baking a Gaussian assumption into the estimator.

## Real Data Example

<div style="text-align: center; margin: 2em 0;">
  <img src="/images/convmmd-astronomy-scaling.png" alt="convMMD fit to the temperature-richness scaling relation of DES galaxy clusters" style="max-width: 100%; height: auto;">
  <p style="font-size: 0.9em; color: #666;"><em>Temperature–richness scaling relation for 110 Dark Energy Survey galaxy clusters, where both axes carry measurement error. The convMMD fit (RMSE 0.242) improves on the established Bayesian <code>linmix</code> fit (RMSE 0.263).</em></p>
</div>

The same framework handles regression when *both* variables are measured with error (errors-in-variables regression). A naive least-squares fit suffers from **attenuation bias** — measurement error in the covariate shrinks the estimated slope toward zero — while convMMD corrects for it.

We apply it to 110 galaxy clusters from the Dark Energy Survey (DES) to estimate the scaling relation between optical richness \\(\lambda_{RM}\\) and hot gas temperature \\(T_X\\), two cluster mass proxies that are both observed with quantified, cluster-specific uncertainty. convMMD faithfully captures the underlying relationship and, on a held-out test set of the best-measured clusters, achieves a lower RMSE (0.24) than the established Bayesian approach <code>linmix</code> (0.26).

## Significance

convMMD provides a **unified, likelihood-free, simulation-based framework** for inference under classical measurement error — covering density deconvolution and errors-in-variables regression within a single estimator, and accommodating noise in the covariates, the outcomes, or both. It is competitive with specialized likelihood-based methods under Gaussian noise and noticeably more robust when the noise is not Gaussian.

---

<h2 id="paper-abstract">Paper abstract</h2>

<details>
<summary>Read the full abstract</summary>
<p>Modern data analyses frequently encounter settings where samples of variables are contaminated by measurement error. Ignoring measurement noise can substantially degrade statistical inference, while existing correction techniques are often computationally costly and inefficient. Recent advances in kernel methods based on Maximum Mean Discrepancy (MMD) have enabled efficient, likelihood-free inference, but they typically assume precise data, overlooking contamination by measurement error. In this work, we introduce a novel framework for inference with samples corrupted by potentially heteroscedastic noise from a known distribution. Central to our approach is the convolutional MMD (convMMD), which compares distributions after noise convolution and retains metric validity under standard kernel conditions. We establish finite-sample deviation bounds that are unaffected by measurement error and prove an equivalence between testing under noise and kernel smoothing. Leveraging these insights, we then introduce a convMMD-based estimator for inference with noisy, heteroscedastic observations by minimizing the convMMD between observed and model-generated data. We establish its consistency and asymptotic normality, and provide an efficient implementation using stochastic gradient descent. We demonstrate the practical effectiveness of our approach through simulations and applications in astronomy, anthropometry, and the social sciences. Our approach provides a unified, likelihood-free, simulation-based framework for efficient and robust inference for broad classes of problems in noisy data.</p>
</details>

## Links
- [Paper (arXiv)](https://arxiv.org/abs/2604.12022)
