---
layout: default
title: "Koopman Operator Regression"
permalink: /koopman/
---

<script type="text/javascript" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>




## Koopman Operator Regression (KOR) for Dynamical Systems



<div style="border-left: 4px solid #007acc; background: #f5faff; padding: 1em; margin: 1em 0; border-radius: 6px;">
  <span style="font-size: 1.2em;">📄 <strong><a href="https://neurips.cc/virtual/2023/poster/71933" target="_blank" style="text-decoration: none; color: #007acc;">
    Sharp spectral rates for Koopman operator learning
  </a></strong></span><br>
  Vladimir Kostic, Karim Lounici, Pietro Novelli, and Massimiliano Pontil<br>
  <em>NeurIPS 2023 (Spotlight paper)</em>
</div>


We focus on Markovian systems $\\left\\lbrace X_{t} : t \\in \\mathbb{N} \\right\\rbrace $, with state $X_t \in \mathcal{X}$, assuming time-homogeneity and existence of an invariant measure $\pi$. The Koopman operator is then defined as:

{% raw %}

$$
(A_{\pi} f)(x) := \mathbb{E}[f(X_{t+1}) \mid X_t = x],\quad x \in \mathcal{X},
$$
{% endraw %}

a bounded linear operator on $L^2(\pi)$, the space of square-integrable functions with respect to $\pi$. This operator coincides with the transfer operator in stochastic processes and is self-adjoint when the system is time-reversible, as is the case for many physical dynamics, including Langevin systems.

In applications like Molecular dynamics, researchers are particularly interested in the spectral decomposition of $A_{\pi}$ (assuming it exists) as it reveals some physical properties of a molecule (meta-stable states):

$$
A_{\pi} = \sum_{i \in \mathbb{N}} \lambda_i\, \psi_i \otimes \psi_i.
$$


A key challenge in learning data-driven models for ergodic stochastic dynamical systems using the kernel Koopman (transfer) operator approach is the emergence of \emph{spurious eigenvalues}—artifacts in the spectrum of the estimated operator that do not appear in the true transfer operator.
This can occur even when the estimated transfer operator is accurate in the operator norm. 


We highlight that the root cause of spurious eigenvalues is a mismatch between the intrinsic metric $L^2(\pi)$ of the stochastic process and the proxy metric $\Vert\cdot\Vert$ imposed by the Reproducing Kernel Hilbert Space (RKHS) $\mathcal{H}$ used in learning. When these metrics are not equivalent, spectral estimation becomes unreliable.

We introduce the concept of *metric distortion* to quantify this mismatch and analyze its effect on spectral bias. Our main result shows that eigenvalue estimation error scales with the metric distortion:

$$
|\widehat{\lambda}_i - \lambda_i| \leq \textcolor{red}{\eta_i} \cdot \mathcal{E}(\widehat{A}),
$$

where $\widehat{A} = \sum_i \widehat\lambda_i \, \widehat\psi_i \otimes \widehat\psi_i$ is our estimator, $\eta_i = \Vert\widehat\psi_i\Vert / \Vert \widehat\psi_i\Vert_{L^2_{\pi} }$ is the metric distortion and $\mathcal{E}(\widehat{A})$ is the operator norm estimation error. 


<figure>
  <img src="/assets/pics/evals_distribution.svg" alt="Eigenvalue distortion" style="width:100%; max-width:800px;">
  <figcaption>
    <em>Experience on spurious eigenvalue phenomenon.</em> The true Koopman operator has three eigenvalues. We use Reduced Rank Regression (RRR) and Principal Component Regression (PCR) estimators. We compare their eigenvalues to the truth when using different kernels with various metric distorsion. The good kernel has small metric distortion, the bad kernel introduces larger metric distorsion and the ugly kernel has a very large metric distorsion. PCR is highly sensitive to even small metric distortion, often producing spurious eigenvalues early on. In contrast, RRR remains robust under moderate distortion. However, when distortion becomes too large, both methods ultimately break down.
  </figcaption>
</figure>





Furthermore we build an estimator $\eta_i$ of this distortion and use it to design a data-driven RKHS selection procedure that minimizes distortion and thereby improves both spectral recovery and forecasting.



<figure>
  <img src="/assets/pics/ala2_model_selection.svg" alt="model selection" style="width:100%; max-width:800px;">
  <figcaption>
    <em>Model Selection using metric distorsion.</em> 
    Forecasting RMSE on the Alanine Dipeptide dataset for 19 different RRR estimators, each corresponding to a different kernel, which show how the best model, according to the empirical spectral bias metric, also attains the best forecasting performances by a large margin.
  </figcaption>
</figure>




