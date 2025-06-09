---
layout: default
title: "Infinitesimal Generator Learning"
permalink: /IG/
---

<script type="text/javascript" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>




## Learning the Infinitesimal Generator (IG) of Continuous-Time Markov Processes

<div style="border-left: 4px solid #007acc; background: #f5faff; padding: 1em; margin: 1em 0; border-radius: 6px;">
  <span style="font-size: 1.2em;">📄 <strong>
    <a href="https://arxiv.org/abs/2410.14477" target="_blank" style="text-decoration: none; color: #007acc;">
      Laplace transform based low-complexity learning of continuous Markov semigroups
    </a>
  </strong></span><br>
  Vladimir R. Kostic, Karim Lounici, Hélène Halconruy, Timothée Devergne, Pietro Novelli, and Massimiliano Pontil<br>
  <em>2025 International Conference on Machine Learning</em>
</div>

<div style="border-left: 4px solid #007acc; background: #f5faff; padding: 1em; margin: 1em 0; border-radius: 6px;">
  <span style="font-size: 1.2em;">📄 <strong>
    <a href="https://neurips.cc/virtual/2024/poster/95855" target="_blank" style="text-decoration: none; color: #007acc;">
      Learning the infinitesimal generator of stochastic diffusion processes
    </a>
  </strong></span><br>
  Vladimir Kostic, Hélène Halconruy, Timothée Devergne, Karim Lounici, and Massimiliano Pontil<br>
  <em>Advances in Neural Information Processing Systems, 37:137806--137846, 2024</em>
</div>



In modeling continuous-time stochastic systems—such as Langevin dynamics—data are typically collected at discrete time intervals $\Delta t$. Although the ultimate goal is to recover the eigenfunctions of the infinitesimal generator $L$, which capture the system’s essential dynamics, researchers often estimate the associated Koopman (or transfer) operator $A_{\Delta t} = e^{\Delta t L}$ instead. This operator shares the same eigenfunctions as $L$, but is simpler to estimate and analyze using standard operator learning methods. However, these methods break down as $\Delta t \to 0$, a regime often required in molecular dynamics to observe fast transient events. In this regime, the spectral gap of $A_{\Delta t}$ collapses:
$
gap_i(A_{\Delta t}) \approx \Delta t\,  gap_i(L) \to 0$ as $\Delta t \to 0$, even though $gap_i(L) > 0$, which severely degrades the accuracy of spectral estimation. This presents a core challenge: fine time resolution (small $\Delta t$) is needed for modeling fast events, but it undermines the ability to recover meaningful dynamics via $A_{\Delta t}$.\\
\textbf{Contribution:} To overcome this, we aim to recover $L$, as it governs the underlying continuous dynamics and is independent of $\Delta t$. However, since $L$ is an unbounded operator, direct estimation is challenging. Instead, we estimate its resolvent $\mathcal{R}_\mu = (\mu I - L)^{-1}$ (for some arbitrary complex $\mu$), a bounded—and in many important cases, compact—operator, which shares the same eigenfunctions as $L$.

We propose two approaches:

*1. Laplace Reduced Rank Regression (LaRRR).* A model-agnostic method based on the Laplace representation

$$
\mathcal{R}_\mu = \int_0^\infty A_t e^{-\mu t} dt.
$$

We propose a numerical approximation of the Laplace representation as a weighted sum of RRR estimators of $A_{j\, \Delta t}$, $j\in [J]$ for some $J$ where $A_{ t} f(x) = \mathbb{E}[f(X_{t}) \mid X_0 = x]$ for any function $f \in L^2_{\pi}$. This method is broadly applicable to a large class of processes beyong the self-adjoint case.

*2. Physics-Informed Learning via Dirichlet Forms.* For time-reversible diffusions with an associated Dirichlet form, we exploit this structure to define a training loss that reflects the underlying physics. When the Dirichlet form is known up to a set of parameters, we propose an efficient estimate from data. This approach leads to faster convergence compared to the agnostic LaRRR method, though it is limited to cases where the generator is self-adjoint and does not scale well to very large systems.


<figure>
  <img src="/assets/pics/1d3w_Langvin_dual.png" alt="IG" style="width:100%; max-width:800px;">
  <figcaption>
    <em>Comparison of our IG learning approach (LaRRR) to the Tranfer Operator (TO) RRR on the task of estimating the slowest timescales (eigenvalues) of the process.</em> 
     As predicted by our theory, while LaRRR (blue) is not affected by decreasing the time-lag $\Delta t$, the error of TO method (red) explodes as $\Delta {t}\to0$. The main figure shows three quartiles of relative error for $\lambda_2$ across 10 independent trajectories of a 1D Langevin process on a triple-well potential, while the inset figure shows distributions for the leading three eigenvalues for $\Delta t=0.005$.
  </figcaption>
</figure>


