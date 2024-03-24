# Reviewer 1

Thank you for your insightful comments and engaging questions. Here are answers to your concerns and questions.
- Q1:
  - Equation (8) demonstrates that the iteration count scales with $\lambda\_{\min}$, approximately $\lambda_{\min} \approx 1/n$ where $n$ represents the state dimension. Consequently, the iteration growth remains at most linear in $n$. Although not explicitly stated in the paper, our computations for $n=2,4,6,8,10$ reveal sublinear growth.
  - Our approach is further motivated by Theorem 2.3, where it is established that the step size $\gamma= O(\lambda_{\min}/\lambda_{\max}^2)$ and the number of iterations $N$ satisfy $N = O((\lambda_{\max}/\lambda_{\min})^2)$ for achieving a convergence rate of $1/\lambda_{\min}$ with ULA. By normalizing the potential $\nabla^2U$ with a preconditioner derived from the data (Lemma 3.1), we achieve a uniform bound on the curvature of $\nabla^2 U$, enabling a smaller number of step iterations as suggested in (8) while attaining superior regret bounds.
- Q2:
  - Relaxing the restrictive assumption in (Ouyang et al., 2019) results in exponential growth of the state norm and escalating costs. While introducing a stabilizing controller in prior work mitigates this issue, selecting such a controller without knowledge of the true system parameters is impractical. To address this challenge, we employ a verifiable compact set as utilized by (Abeille & Lazaric, 2018). Our rigorous analysis demonstrates that the concentration between distributions of approximate and the true system parameter converges at a rate of $\tilde O(1/\sqrt{\lambda_{\min}})$. We deduce that $\lambda_{\min}$ increases polynomially over time $t$ as the learning progresses. To achieve this, we inject noise once in each episode. Combining these novel findings, we demonstrate that the state achieves a uniform bound, a critical aspect where the key difficulty lies in removing the aforementioned restrictive assumption.
- Q3:
  - Sampling from a posterior distribution, such as Thompson sampling, often demands significant computational resources in learning LQR problems. The implementation of preconditioning aims to alleviate this challenge. While sample complexity for naive ULA is well-studied, those for preconditioned ULA are relatively less explored. In our paper, we introduce modified step sizes and the number of step iterations, leading to improved computational efficiency. Moreover, our carefully chosen parameters result in better regrets.

# Reviewer 2

We thank you for the review and appreciate your time reviewing our paper.
- Q1: In the Bayesian regime, we focus on the distribution of the true system parameter by tracing the posterior. Once data is collected, the posterior distribution for the true system parameter is updated via Bayes' rule and it will have a peak around a certain value, which gives better 'confidence'. Point estimation for the true system parameter is also available via Maximum a posteriori estimation (MAP).  
- Q2: 
  - Concerning Assumption 2.1, we would kindly refer to the answer to Q2 raised by 'Reviewer is77' due to the space constraint.
  - It is true that the policy is stochastic rather than deterministic as the learning proceeds based on Thompson Sampling (TS). In our algorithm, we use the policy $u_t=K x_t$ or $u_t=K x_t+ \nu_t$. The inclusion of Section 2.1 serves to inspire readers regarding such an exploration scheme, drawing parallels to the stochastic Linear Quadratic Regulation (LQR) problem under Gaussian perturbation, where the optimal control action is deterministic and given as $u_t=K x_t$ (Theorem 2.2). We acknowledge the typo regarding the notation $\pi_t: H_t \rightarrow \mathbb{R}^m$, which should be corrected to $\pi_t: H_t \rightarrow \mathbb{R}^{n_u}$, and we assure you that this will be reflected in the revised version.
- Q3: We will add the relation between $t$ and $k$ in the beginning of Section 2.3 for better clarity.
- Q4: Here, $a_n = O(b_n)$ means $\limsup_{n \to \infty} |a_n/b_n |< \infty$, employ $a_n = \Omega(b_n)$ for $\liminf_{n \to 
  - \infty}|a_n/b_n |>0$. We also admit that $N \geq O( (\lambda_{\max} / \lambda_{\min})^2)$ should be changed to $N = \Omega( (\lambda_{\max} / \lambda_{\min})^2)$.  
  - $X_N$ comes from the ULA given in line 164, C2 and its distribution is denoted by $p_N$. The joint distribution between $p_N$ and $p$ comes from the shared Brownian motion, so the LHS in Theorem 2.3 should be changed to the Wasserstein distance.
- Q5: The reduction of the number of iterations $N$ by implementing the preconditioner should not be obvious at first glance. A typical preconditioning used in ULA does not fully exploit the upper and lower bound of $P^{-1/2} \nabla^2 U P^{1/2}$ while we leverage the uniform bound of this quantity by introducing a structural assumption on $p_w$. Using this bound we could reduce the number of iterations achieving a better concentration.
- Q6: Definitely ! We will specify it giving more credit to their work.


# Reviewer 3
I have the following major concerns:

What is the goal/motivation of this paper? If the regret bound is the same as the ones obtained in prior works, why do we need to design a new algorithm? What's the advantage of the TS-based algorithm?
I don't understand the relationship between TS and ULA. Why don't we directly implement TS on LQR? Any computational difficulty?
The paper claims that a stabilizing policy is not necessary or does not have to be prespecified. Which theoretical result supports this claim?
The assumptions are complicated and lack of explanation. For example, Assumption 2.1 assumes the noise distribution is known. In practice, I don't think this holds. Justify all the assumptions!
Although the regret bound matches the SOTA, what about the sample complexity? Note that a rejection sampling is used in the algorithm design. Can you quantify the number of samples used in the estimation?
I also have minor questions:

We deeply appreciate your insightful comments and inputs.

Reviewer 3
Q1. Bayesian setting에서 이전 work들은 크게 두 가지 문제를 갖고 있다. 첫 번째는 Gaussian noise에 관해서만 문제를 해결할 수 있었고, 두 번째는 unrealizable한 compact set에 대해 rejection sampling하는 restrictive한 가정을 사용했다는 점이다. 우리는 preconditioned ULA를 사용하여 더 general한 noise에 대해 TS-based algorithm을 사용할 수 있도록 하였고, 실험적으로 구현 가능한 가정을 사용하였다. 그 결과 regret bound를 기존의 $O(\sqrt{T\log T})$에서  $O(\sqrt{T })$로 향상시킬 수 있었다. 
Q2. 랑주뱅과 톰슨샘플링의 결합에서의 핵심은 랑주뱅으로 posterior의 근사샘플링을 하는 것이다. Mazumdar는 multi-armed bandit에서 reward function에 대해 strongly log concave하다는 가정 하에 ULA에 관한 이론적인 분석을 하였다. 이 이론적인 결과를 LQR setting에 naive하게 적용하게 된다면 reward function에 대응되는 것이 log-concave하기 때문에, posterior가 strongly log-concavity가 data에 따라 linear하게 증가하지 않는다. 그 결과 number of iteration에 중요한 요소인condition number가 엄청나게 증가하여 computational inefficiency를 야기한다. 그러므로 이 문제를 해결하기 위하려 preconditioning matrix를 도입하였으며 이로 인해 lamda_min과 \lambda_max의 격차를 normalizing함으로써 줄일 수 있었고 이로인해 computational efficiency를 달성할 수 있었다. 

Q3.기존에 stabilizing policy가 필요했던 이유는, state norm의 폭발을 막아 좋은 regret을 얻기 위함이다. 이를 쓰지 않고 state moment의 constant bound를 얻기 위해서 다음과 같은 True system parameter에 대한 concentration은 O(1/\sqrt{lambda_min})이고, 점점 수렴할수록 true system parameter와 가까운 값을 뽑을 가능성이 큼으로, state의 폭발을 야기하는 system parameter를 샘플링할 가능성이 적어진다(1769~1778)는 사실에 착안하였다. 이를 위해 action에 noise를 첨가하는 방식으로 lambda_min의 polynomial한 증가를 이끌어내어 true system parameter와 멀리 떨어져 있어 state의 폭발을 야기하는 system parameter를 뽑을 확률을 최대한 작게 control하여 state moment의 constant bound(Theorem 5.1)를 유도하였다. 


- Q1: The 
- Q2: 
- Q3: 
- Q4:

Answer to the minor questions
- Q1: In regard to the prior distribution $\mu_1$, we only need to assume that the prior distribution $\mu_1$ follows the Gaussian whose covariance is less than 1 to carry over our analysis. Hence, any choice for the mean of $\mu_1$ can be used. In our empirical verification, we have set the mean of each component of the prior distribution to be 0.5 which is simply our random guess.

- Q2: Thank you for the suggestion. We will fix it in the updated version.

- Q3: Sampling $\theta_j$ for each $j$ corresponds to simulating the discretized SDE given as $\theta_{j+1}=\theta_j - \tilde \gamma_k \tilde P_k^{-1}\nabla U_k(\theta_j) +\sqrt{2\gamma \tilde P_k^{-1}} W_j$ where the Brownian motion $W_j$ has to be taken into account. After iterating $\tilde N_k$ times, we check that if $\theta_{\tilde N_k}$ satisfies the rejection condition (line 220 - 222),
>Following (Abeille & Lazaric, 2018),  we let $\mathcal{C}:=\{\theta \in \mathbb{R}^{dn}:|\theta|\leq S,|A+BK(\theta)|\leq \rho<1, J(\theta)\leq M_J\}$ for some $S,\rho,M_J>0$ and ${\theta} = \mathrm{vec}([A \quad B]^{\top})$. 

Once satisfied, we set $\tilde \theta_k := \theta_{\tilde N_k}$. In short, sampling $\theta_j$ is an intermediate system parameter used for the Langevin iteration, and $\tilde \theta_k$ denotes the system parameter we use for the next episode $k+1$. 

# Reviewer 4

Thank you so much for carefully reading our paper and giving us invaluable input. Your comments are all to the point and we would like to address them very carefully. Besides, in the paper you mentioned, the authors implement the inverse of the Fisher information matrix for more efficient gradient descent and convergence guarantee which aligns with the core idea of our paper yet the scope is a bit different as we focus on improved regret. We will make sure to include it in the revised version. 

- Q1: You are absolutely right that two distributions $p$ and $p_N$ are dependent so that joint distribution should have been specified. As pointed out, the expectations in Theorem 2.3 and Lemma A.1 are taken over joint distribution generated by the shared Brownian motion. We will revise the statement to clarify it in the revised version.

- Q2: Similar to Q1, Proposition 4.1 holds only if samples from exact and approximate posterior share the same Brownian motion. On the other hand, two key properties for the exact posterior $\mu_t$ stated in Proposition 4.2 and the first part of Theorem 4.5 hold true regardless of the coupling as only stochastic properties are leveraged. 

Summarizing Q1 and Q2, whenever expectation is taken for $\mu\_t$ and $\tilde \mu\_t$, we mean that joint distribution comes from the coupling induced by the shared Brownian motion.

- Q3. Sorry for the confusion. For $\mathcal{J}\_k = \\{n_i:n\_1<n_2<\ldots<n\_{k(k+1)/2}\\}$, we newly definte $X$ and $Y$ as $X=[ w\_{n\_1-1} \quad \cdots \quad \_{n\_{k(k+1)/2}-1} ]^\top$ and $Y = [ K_{\nu(n_1)}w_{n_1-1} \quad \cdots \quad K\_{\nu(n\_{k(k+1)/2})}w\_{ n\_{k(k+1)/2}-1} ]^{\top}$. Then it is clear to have such a form.

- Q4: Regarding the tower rule in page 36 line 1927, if I understood your question correctly, we always mean the true system parameter random variable by $\overline \theta_*$ throughout the paper, and hence, conditioning with respect to the history collected before $k$th episode makes sense. In line 1927, $\overline \theta\_*$ in both equations are random variables following the exact posterior distribution. This way the inner expectation of  $\mathbb{E} [\mathbb{E}\_{\mu\_k, \tilde \mu\_k} [|\overline \theta\_\* -\theta\_k|\^2 | h\_{t\_k}]]$ is taken for two distributions $\mu_k$ and $\tilde \mu\_k$ that share the Brownian motion, and outer expectation is taken over all histories before the $k$th episode.

For the minor problems raised, we will be updated in the revised version.

# Reviewer 5

We sincerely appreciate your thoughtful engagement with our paper. Despite acknowledging a lack of expertise, your perspective is invaluable to us.

Allow us to elaborate on the assumptions regarding (a) the prior distribution (Assumption 3.2), (b) injected noise $\nu$ (Assumption 3.3), and (c) the system parameter:

- Regarding the prior distribution, we choose a Gaussian distribution, a common choice when prior information is unavailable.
- The injection of noise $\nu_s$ at $t=t_k - 1$ is for better exploration, with the Gaussian distribution selected for its analytical convenience and standard usage.
- Concerning $\mathcal{C}$ (lines 220-223), borrowed from (Abeille & Lazaric, 2018), we ensure that $\theta \in \mathcal{C}$ satisfies specific conditions derived from $K(\theta)$ and $J(\theta)$ as $K$ and $J$ are continuously differentiable.

Q1: We emphasize that our Bayesian regret necessitates integration over the prior distribution, unlike 'Black-Box Control for Linear Dynamical Systems,' which derives high probability regret. Moreover, the assumption on system noise $|w_t|\leq 1$ in the paper is not necessary in our paper, as it imposes unnecessary constraints for tail analysis.

Q2: We respectfully claim that Assumption 2.1 aligns with established standards, equivalent to enforcing log-concavity on the density function and Lipschitz smoothness on the gradient of the density function, as discussed in (Dwivedi et al., 2018). Additionally, we note that another study addressing Bayesian regret bounds (Ouyang et al., 2019) introduces the Gaussian noise assumption, which remains consistent with our assumption. To be more illustrative, let us consider probability density functions defined in $\mathbb{R}$. Then any probability density function $p_w(z) \sim \mathcal{N}(0, {\sigma}^2)$ where $ \frac{1}{\overline m}\leq \sigma^2 \leq \frac{1}{\underline m}$ satisfies Assumption 2.1. One can also include some asymmetric distributions as well. For instance, for $z\in \mathbb{R}$, one can $p_w(z)$ to satisfy $ -\frac{d^2 \log p_w(z)}{d z^2}=\underline m$ if $z<-1$, $ -\frac{d^2 \log p_w(z)}{d z^2}=\overline m$ if $z>1$ and $-\frac{d^2 \log p_w(z)}{d z^2}=(\overline m -\underline m)/2 \cdot z + \underline m + (\overline m - \underline m )/2$ if $z\in(-1,1)$, which represents an asymmetric probability distribution. Hence, we carefully claim that  Assumption 2.1 seems to restrictive but include many other distributions beyond Gaussian distributions.
