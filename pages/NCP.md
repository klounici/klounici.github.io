---
layout: default
title: "Neural Conditional Probability Operator"
permalink: /NCP/
---

<script type="text/javascript" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>




## Neural Conditional Probability Operator for Uncertainty Quantification

<div style="border-left: 4px solid #007acc; background: #f5faff; padding: 1em; margin: 1em 0; border-radius: 6px;">
  <span style="font-size: 1.2em;">📄 <strong>
    <a href="https://neurips.cc/virtual/2024/poster/92949" target="_blank" style="text-decoration: none; color: #007acc;">
      Neural Conditional Probability for Uncertainty Quantification
    </a>
  </strong></span><br>
  Vladimir Kostic, Grégoire Pacreau, Giacomo Turri, Pietro Novelli, Karim Lounici, and Massimiliano Pontil<br>
  <em>Advances in Neural Information Processing Systems, Vol. 37, NeurIPS 2024</em>
</div>



<div style="border-left: 4px solid #007acc; background: #f5faff; padding: 1em; margin: 1em 0; border-radius: 6px;">
  <span style="font-size: 1.2em;">📄 <strong>
    <a href="https://arxiv.org/abs/2505.19809" target="_blank" style="text-decoration: none; color: #007acc;">
      Equivariant Representation Learning \\for Symmetry-Aware Inference with Guarantees.
    </a>
  </strong></span><br>
  Ordoñez-Apraez, Danie, Frohlick,Alek, Kostic, Vladimir, Lounici, Karim, Brandt, Vivien, and Pontil, Massimiliano. Submitted.<br>
  <em></em>
</div>



We study the problem of estimating conditional distributions $\mathbb{P}[Y \mid X]$ from joint samples $(x_i, y_i)_{i=1}^n$ with values in $\mathcal{X}\times\mathcal{Y}$, a central task in UQ. Standard conditional training methods, which fit separate models by partitioning the conditioning space $\mathcal{X}$, suffer from high computational cost, sample inefficiency, and poor scalability in high dimensions due to the curse of dimensionality.

We propose an unconditional operator-learning approach based on the conditional expectation operator $$E\,:\, L^2_{\mu_Y} \to L^2_{\mu_X}$$
as follows

$$
E[h](x) := \mathbb{E}[h(Y) \mid X = x], \quad \forall h \in L^2_{\mu_Y}
$$

Under the mild assumption that $(X,Y)$ admits a joint density $\rho(x, y)$ w.r.t the product of marginal probability measures of $X$ and $Y$, then $E$ is compact and admits Singular Value Decomposition:

$$
E = \mathbf{1}_{\mathcal{X}} \otimes \mathbf{1}_{\mathcal{Y}} + \sum_{i=1}^\infty \sigma_i\,  u_i \otimes v_i,
$$

from which we deduce that

$$
\rho(x, y) = 1 + \sum_{i=1}^\infty \sigma_i\, u_i(x) v_i(y),
$$

We learn a truncated version of this decomposition using neural networks, optimizing a regularized self-supervised loss that enforces the operator structure and orthonormality constraints. **A key advantage of our method is that only a single unconditional training phase is needed to learn the truncated SVD**, after which conditional distributions for arbitrary conditioning events $A \subseteq \mathcal{X}$ can be estimated without retraining. 

We define the estimator of the conditional probability as

$$
\widehat{\mathbb{P}}[Y \in B \mid X \in A] := \widehat{\mathbb{E}}_Y[\mathbf{1}_B] + \sum_{i=1}^d \widehat{\sigma}_i \frac{ \widehat{\mathbb{E}}_{X}[\widehat{u}_i \mathbf{1}_A] }{ \widehat{\mathbb{E}}_X[\mathbf{1}_A] } \widehat{\mathbb{E}}_Y[\widehat{v}_i \mathbf{1}_B],
$$

where $\widehat{\mathbb{E}}_X$ and $\widehat{\mathbb{E}}_Y$ denote empirical expectations over the input and output samples, respectively, and $\widehat{u}_i, \widehat{v}_i$ are estimators of the true singular functions obtained during the representation learning phase.

We derive non-asymptotic error bounds for the estimated conditional probabilities, including for rare conditioning events:

$$
\mathbb{P}[Y \in B \mid X \in A] - \widehat{\mathbb{P}}[Y \in B \mid X \in A] = O_{\mathbb{P}} \left( \sqrt{ \frac{ \mathbb{P}[Y \in B] }{ \mathbb{P}[X \in A] } } \left( \sigma_{d+1} + \mathcal{E} + \sqrt{\tfrac{d}{n}} \right) \right),
$$

where $\mathcal{E}$ is the optimization gap of the neural network representation training phase for learning the truncated SVD of $E$. Note that the statistical complexity of this learning task depends on $\sigma_{d+1}$--an intrinsic measure of the complexity of the joint distribution--rather than on the ambient dimensions of $\mathcal{X}$ or $\mathcal{Y}$. In experiments, our method produces reliable conditional confidence intervals even for heavy-tailed conditional distributions (e.g., Cauchy), and outperforms both the popular Conditional Conformal Prediction and Normalizing Flows—a model with at least 10 times more parameters—in both accuracy and stability.

<br><br>

<figure>
  <img src="/assets/pics/cauchy_means_kernel_better.svg" alt="IG" style="width:100%; max-width:800px;">
  <figcaption>
    <em>$90\%$ conditional confidence intervals for $Y \mid X = x$</em> For each value of $x$, the conditional distribution $Y \mid X = x$ follows a Cauchy distribution with location parameter $x^2$ and scale parameter $1 + x$. We compare our NCP to Conditional Conformal Prediction (CCP) and Normalizing Flows (NF) on estimating conditional confidence intervals for several values of $x$. CCP fails due to instability in estimating the undefined mean of the Cauchy distribution, while NF likely overestimates intervals probably due to instability from Jacobian terms.
  </figcaption>
</figure>
