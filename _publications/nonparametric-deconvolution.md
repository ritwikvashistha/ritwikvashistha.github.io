---
title: "Nonparametric Deconvolution and Denoising using Simulation Based Inference"
authors: "R. Vashistha, A. Sarkar, A. Farahi"
collection: publications
category: preprints
permalink: /publication/nonparametric-deconvolution
excerpt: 'A likelihood-free framework for nonparametric density deconvolution and empirical Bayes denoising under additive measurement error using convMMD.'
date: 2026-06-01
venue: 'arXiv preprint'
paperurl: 'https://arxiv.org/abs/2606.21907'
arxiv: '2606.21907'
keywords:
  - deconvolution
  - denoising
  - measurement error
  - simulation-based inference
  - normalizing flows
  - empirical Bayes
  - density estimation
  - nonparametric statistics
  - image denoising
  - latent variable models
---
*A short, plain-language walkthrough of the paper. For the formal statements, see the [abstract](#paper-abstract) and the [full paper](https://arxiv.org/abs/2606.21907).*

## The problem

Many observed scientific measurements are often only corrupted proxies of the quantities we actually care about. In survey astronomy, for instance, stellar mass, luminosity, and weak-lensing shear must be inferred from observations degraded by instrumental noise, selection effects, and projection distortions — and small biases in these recovered quantities can propagate into biased cosmological conclusions. Under the additive-noise model \\(\tilde X = X + U\\) we only ever see the noisy proxy \\(\tilde X\\), while the latent signal \\(X\\) stays hidden behind a noise distribution \\(m\\).

Scientists are usually interested in solving one of the two problems when dealing with noisy data. **Deconvolution** which seeks the *distribution* of the latent signal \\(X\\); **denoising** which focuses on the true latent signal behind each individual noisy observation. Both are classically ill-posed inverse problems whose difficulty depends on the noise, with high-frequency structure being the hardest to recover. Classical methods such as Fourier inversion, kernel deconvolution, series expansions are mathematically rigorous but require delicate regularization, scale poorly with dimension, and do not exploit the expressive, learned representations that modern generative models provide.

## The key idea

Instead of inverting the noise, we propose to match the distributions *in observation space*. If the noise distribution or law \\(m\\) is known, then any candidate latent model \\(q_\theta\\) implies a noisy distribution \\(q_\theta * m\\) — simply convolve it with the noise. A good latent model is one whose noise-convolved version reproduces the observed noisy data. The paper quantifies that mismatch with the **convolutional Maximum Mean Discrepancy (convMMD)** and minimizes it over a family of latent models:

$$\hat\theta_N \;=\; \arg\min_{\theta}\ \text{MMD}^2\!\big((p * m)_N,\; q_\theta * m\big).$$

The objective is **likelihood-free and simulation-based**: it needs only samples. Draw \\(Y \sim q_\theta\\), simulate noise \\(U \sim m\\), and \\(Y + U\\) is a draw from \\(q_\theta * m\\) — so the loss is approximated by Monte Carlo and optimized with stochastic gradient descent. This sidesteps the intractable high-dimensional integrals of a deconvolved likelihood and works with any generator you can sample from, including Gaussian mixtures and normalizing flows, for multivariate homoscedastic or heteroscedastic noise.

The method has two stages that share a single learned model. First, **deconvolution**: fit the latent density \\(\hat q\\) by minimizing the convMMD loss above. To move beyond a fixed parametric form, the latent model lives in a *sieve* — a growing sequence of families (more mixture components, or wider/deeper networks) whose union is dense in the target class.

Second, **denoising**: the learned density becomes an **empirical prior**. For each noisy point \\(\tilde x_i\\), Bayes' rule gives a posterior over the latent value, \\(\pi(x \mid \tilde x_i) \propto m(\tilde x_i - x)\,\hat q(x)\\), and the denoised estimate is the posterior mean \\(\hat x_i = \mathbb{E}[X \mid \tilde X = \tilde x_i]\\). One fit therefore supports both population-level density estimation and object-level recovery.

## What the theory guarantees

The paper extends convMMD from the parametric to the nonparametric regime. A finite-sample **oracle inequality** shows that the learned model performs, in convMMD loss, almost as well as the best member of the sieve with no assumption of mixtures, flows, or any particular form. Because testing under known noise is equivalent to kernel smoothing in the latent space, the estimator attains the fast **parametric \\(O(N^{-1/2})\\) rate** in the induced RKHS geometry, even though the data are noisy.

Recovering the *density itself* in \\(L_2\\) is genuinely harder, and the rates reproduce the classical inverse-problem phase transition set by how fast the noise erases high frequencies. For **ordinary-smooth** noise (e.g. Laplace) the rate is polynomial; for **super-smooth** noise (e.g. Gaussian) it degrades to logarithmic. Accurate deconvolution then implies accurate denoising: the excess empirical-Bayes risk inherits the squared \\(L_2\\) rate of the density estimate.

## Denoising structured signals

<div style="text-align: center; margin: 2em 0;">
  <img src="/images/nonparametric-denoising-benchmark.png" alt="Empirical Bayes denoising of Two Moons, Circles, and Checkerboard under heteroscedastic Gaussian noise" style="max-width: 100%; height: auto;">
  <p style="font-size: 0.9em; color: #666;"><em>Empirical-Bayes denoising of 2D latent structure corrupted by heteroscedastic Gaussian noise plus 3% outliers. Both convMMD sieves recover the manifolds more faithfully than the NPEB and XDGMM baselines and approach the Oracle that knows the true density — on Two Moons, convMMD_NF (MSE 0.267) beats XDGMM (0.360) and NPEB (0.343), with the Oracle at 0.246.</em></p>
</div>

On three structured 2D datasets (Two Moons, Circles, Checkerboard), latent points are corrupted by heteroscedastic Gaussian noise and, to probe robustness, 3% outliers. NPEB — a discrete nonparametric empirical-Bayes method built on the NPMLE — produces speckled estimates that miss the manifolds' continuity, while XDGMM — a Gaussian-mixture prior fit by expectation-maximization — oversmooths and loses sharp boundaries and hollow interiors. Tellingly, even when convMMD uses the *same* Gaussian-mixture family as XDGMM, the simulation-based objective recovers the topology better, consistent with MMD's known robustness to corrupted mass. The two sieves also show complementary inductive biases: the continuous mapping of normalizing flows excels on continuous manifolds (Moons, Circles), while Gaussian mixtures capture clustered grids (Checkerboard).

## Scaling to high-dimensional images

<div style="text-align: center; margin: 2em 0;">
  <img src="/images/nonparametric-mnist-denoising.png" alt="MNIST image denoising under white and spatially correlated noise, comparing convMMD GAN variants against Noise2Self, SURE, and BUIFD" style="max-width: 100%; height: auto;">
  <p style="font-size: 0.9em; color: #666;"><em>MNIST image denoising (D = 784) under additive white Gaussian noise (AWGN) and spatially correlated AR(1) noise. Noise2Self leads under pure AWGN (22.0 dB) but collapses as correlation grows (9.0 dB at ρ = 0.4); by matching the global convolved distribution, convMMD assumes no pixel independence and stays stable (≈15 dB). Panels (b–c) show the same pattern in SSIM.</em></p>
</div>

To stress-test scalability far beyond the low-dimensional settings the theory emphasizes, the framework is applied to MNIST denoising in \\(D = 784\\) dimensions, learning the prior with two GAN sieves — convMMD_GAN (a fixed Gaussian kernel) and convMMD_GAN* (an adversarially learned kernel). Baselines tuned for pixel-independent white noise, such as Noise2Self and SURE, do well under AWGN but degrade sharply once the noise becomes spatially correlated. convMMD, which matches the *global* noise-convolved distribution rather than assuming independence, remains stable across correlation levels (with BUIFD, a supervised model, included as an empirical upper bound). The authors stress this is a robustness stress test that scales the measurement-error framework to high dimensions — not a bid to win a specialized image-restoration benchmark.

## Why it matters

convMMD offers a single, likelihood-free, simulation-based framework that trains expressive latent generative models directly on corrupted data while respecting a known noise mechanism — recovering both the latent distribution (deconvolution) and individual signals (denoising), with finite-sample and large-sample guarantees. In doing so it advances deconvolution and denoising from a classical inverse problem toward a general paradigm for learning under structured corruption.

---

<h2 id="paper-abstract">Paper abstract</h2>

<details>
<summary>Read the full abstract</summary>
<p>Latent signals are often obscured by measurement noise, yet encode the underlying laws and dynamics of complex systems; learning both the signals and their distributions remains a central challenge in scientific inference. The noise is often non-negligible, and the likelihoods for expressive generative models are often intractable. We utilize a convolutional maximum mean discrepancy (convMMD) loss and propose a likelihood-free framework for nonparametric density deconvolution and empirical Bayes denoising under additive measurement error. Our method learns a latent generative model by matching the observed data distribution to the noise-convolved model distribution. This yields a differentiable, simulation-based objective for multivariate homoscedastic or heteroscedastic noise, compatible with expressive sieve classes such as Gaussian mixtures and normalizing flows. The learned density then serves as an empirical prior for posterior denoising of individual latent values. Theoretically, we extend convMMD from parametric to nonparametric estimation, proving finite-sample bounds for empirical sieve minimizers and L2 convergence rates under Sobolev smoothness. These rates recover the classical inverse-problem dependence: polynomial for ordinary-smooth and logarithmic for super-smooth noises. Our method provides a practical, theoretically grounded approach to deconvolution and denoising under generative latent distribution models.</p>
</details>

## Links
- [Paper (arXiv)](https://arxiv.org/abs/2606.21907)
