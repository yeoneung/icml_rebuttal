# Check in

We thank you for your review and appreciate your time reviewing our paper.

The end of the rebuttal phase is approaching. We would be grateful if we could hear your feedback regarding our answers to the reviews. We are happy to address any remaining points during the remaining period.

Thanks in advance,

# Reviewer 1
Thank you for your insightful comments and engaging questions. Here are answers to your concerns and questions.

> Q1: How does the integration of preconditioned Langevin dynamics with Thompson sampling specifically address the challenge of sampling efficiency in high-dimensional spaces, and what inspired this novel approach over other potential methods?

- Answer to Q1:
  - Equation (8) demonstrates that the iteration count scales with $\lambda\_{\min}$, approximately $\lambda_{\min} \approx 1/n$ where $n$ represents the state dimension. Consequently, the iteration growth remains at most linear in $n$. As seen in the middle of the [page](https://ibb.co/kmf19C6),  our empirical verification for $n=2,4,6,8,10$ reveals that the number of step iterations does not scale exponentially in dimension when the preconditioner is used. The right of [page](https://ibb.co/kmf19C6) shows (total computation time)/$n\^k$ with various values of $k$ where $n$ denotes the dimension. For those experiments, $A\_\ast$ and $B\_\ast$ in [page](https://ibb.co/kmf19C6) are used. 

  - Our approach is motivated by Theorem 2.3, where it is established that the stepsize $\gamma= O(\lambda_{\min}/\lambda_{\max}^2)$ and the number of iterations $N$ satisfies $N = O((\lambda_{\max}/\lambda_{\min})^2)$ for achieving a convergence rate of $1/\lambda_{\min}$ with ULA. Normalizing the potential $\nabla^2U$ with a preconditioner derived from the data (Lemma 3.1), we achieve a uniform bound on the curvature of $\nabla^2 U$, enabling a smaller number of step iterations as suggested in (8) while attaining better regret bound.

> Q2: Given the paper's reduction of restrictive assumptions in achieving an $O(\sqrt{T})$ regret bound, what theoretical or practical limitations were encountered in removing these assumptions, and how were they overcome?

- Answer to Q2: Relaxing the restrictive assumption in (Ouyang et al., 2019) results in exponential growth of the state norm and escalating costs. While introducing a stabilizing controller in prior work mitigates this issue, selecting such a controller without knowledge of the true system parameters is impractical. To address this challenge, we employ a verifiable compact set as utilized by (Abeille & Lazaric, 2018). Our rigorous analysis demonstrates that the concentration between distributions of approximate and the true system parameter converges at a rate of $\tilde O(1/\sqrt{\lambda_{\min}})$. We deduce that $\lambda_{\min}$ increases polynomially over time $t$ as the learning progresses. To achieve this, we inject noise once in each episode. Combining them, we demonstrate that the state achieves a uniform bound, a critical aspect where the key difficulty lies in removing the aforementioned restrictive assumption.

> Q3: Can you summarize the core difficulties in theoretical analysis of this integration? How do overcome them?

- Answer to Q3: Sampling from a posterior distribution, such as Thompson sampling, often demands significant computational resources in learning LQR problems. The implementation of preconditioning aims to alleviate this challenge. While sample complexity for naive ULA is well-studied, those for preconditioned ULA are relatively less explored. In our paper, we introduce modified stepsize and the number of step iterations, leading to improved computational efficiency. Besides, our carefully chosen parameters result in better regrets.

# Reviewer 2
We thank you for the review and appreciate your time reviewing our paper.

> Q1: Sec 2.2: Can truth be random? First, authors say that $\theta\^\ast$ is an unknown true parameter, but later in the same section, they say that $\theta\^\ast$ is random. Moreover, if you know the distribution of $\theta\^\ast$, why do you update it using data? If you recall, the objective of the Bayesian experiment is to recover the truth, so if the truth itself is random, can we recover the random truth using the Bayesian posterior?

- Answer to Q1: In the Bayesian regime, we focus on the distribution of the true system parameter by tracing the posterior. Once data is collected, the posterior distribution for the true system parameter is updated via Bayes' rule and it will have a peak around a certain value, which gives higher 'confidence'. Point estimation for the true system parameter is also available via Maximum a posteriori estimation (MAP).  

> Q2: Assumption 2.1: Are these standard assumptions? Please discuss 101C2- Why is the policy deterministic, given that the authors build on TS? $u\_t$ dim is $n\_u$, so why policy maps to $\mathbb{R}\^m$?

- Answer to Q2: 
  - We respectfully claim that Assumption 2.1 aligns with established standards, equivalent to enforcing log-concavity on the density function and Lipschitz smoothness on the gradient of the density function as discussed in (Dwivedi et al., 2018). Additionally, we note that another study addressing Bayesian regret bounds (Ouyang et al., 2019) introduces the Gaussian noise assumption, which is a special case of our assumption. To be more illustrative, let us consider probability density functions defined in $\mathbb{R}$. Then any probability density function $p\_w(z) \sim \mathcal{N}(0, {\sigma}\^2)$ where $ \frac{1}{\overline m}\leq \sigma\^2 \leq \frac{1}{\underline m}$ satisfies Assumption 2.1. One can also include some asymmetric distributions as well. For instance, for $z\in \mathbb{R}$, one can set $p_w(z)$ to satisfy $-\frac{d\^2 \log p\_w(z)}{d z\^2}=\underline m$ if $z<-1$, $-\frac{d\^2 \log p\_w(z)}{d z\^2}=\overline m$ if $z>1$ and $-\frac{d\^2 \log p\_w(z)}{d z\^2}=(\overline m -\underline m)/2 \cdot z + \underline m + (\overline m - \underline m )/2$ if $z\in(-1,1)$, which represents an asymmetric probability distribution. Hence, we carefully claim that  Assumption 2.1 seems to be restrictive but includes many other distributions beyond Gaussian distributions.
  - It is true that the policy is stochastic rather than deterministic as the proposed policy is given by $u_t=K x_t$ or $u_t=K x_t+ \nu_t$ using sampled system parameters. The inclusion of Section 2.1 serves to inspire readers regarding such an exploration scheme, drawing parallels to the stochastic Linear Quadratic Regulation (LQR) problem under Gaussian perturbation, where the optimal control action is deterministic and given as $u_t=K x_t$ (Theorem 2.2). We acknowledge the typo regarding the notation $\pi_t: H_t \rightarrow \mathbb{R}^m$, which should be corrected to $\pi_t: H_t \rightarrow \mathbb{R}^{n_u}$, and we assure you that this will be reflected in the revised version.

> Q3: Section 2.3: it would be helpful if the authors clearly described the relation between episode k and time there. In particular, I am confused because it is not clear whether the posterior is sampled only at the beginning of the episode or at each time t after it is updated. (This is only clear after looking at the algorithm)

- Answer to Q3: We will add the relation between $t$ and $k$ in the beginning of Section 2.3 for better clarity.

> Q4: Theorem 2.3: Please define O() precisely. What does $N \geq O( (\lambda\_{\max} / \lambda\_{\min})\^2)$ mean? Moreover, how does this theorem imply the concentration of $X\_n$ to sample from $p$?

- Answer to Q4:  
  - Here, $a_n = O(b_n)$ means $\limsup_{n \to \infty} |a_n/b_n |< \infty$, and $a_n = \Omega(b_n)$ indicates $\liminf_{n \to \infty}|a_n/b_n |>0$. We also admit that $N \geq O( (\lambda_{\max} / \lambda_{\min})^2)$ should be changed to $N = \Omega( (\lambda_{\max} / \lambda_{\min})^2)$.  
  - $X_N$ comes from the ULA given in line 164, C2 and its distribution is denoted by $p_N$ after $N$ iterations. The joint distribution between $p_N$ and $p$ comes from the shared Brownian motion, so the LHS in Theorem 2.3 should be changed to the Wasserstein distance.
 
> Q5: 197-202C2: Is it obvious that preconditioning by $P\_t$ (7) reduces $N$? If so, how? How is the preconditioning introduced in the main algorithm different from the typical preconditioning used in ULA?

- Answer to Q5: The reduction in the number of iterations $N$ by implementing the preconditioner is not obvious at first glance. A typical preconditioning used in ULA does not fully exploit the upper and lower bound of $P^{-1/2} \nabla^2 U P^{1/2}$. To obtain the reduction. we need to carefully design stepsize and iterations leveraging the uniform bound (Lemma A.3). Based on it, we reduce the number of iterations achieving a better concentration.

> Q6: Sec. 3.2: The idea for choosing stabilizing parameters is adapted from Abeille&Lazaric(2018). I think the authors should specify their contribution clearly.
- Answer to Q6: Of course! We will specify it and clearly distinguish our contribution from Abeille&Lazaric(2018)

# Reviewer 3
We appreciate all your invaluable comments. To address your concerns, we emphasize the following points.

> Q1: What is the goal/motivation of this paper? If the regret bound is the same as the ones obtained in prior works, why do we need to design a new algorithm? What's the advantage of the TS-based algorithm?
- Answer to Q1: In Bayesian learning of LQR, we acknowledge existing limitations regarding LQR's evolution under Gaussian noise and the reliance of learning algorithms on this structural property, as well as the imposition of an unrealizable assumption on the feasible set. Adopting the feasible set proposed by [8], we propose a novel computationally tractable algorithm, resulting in state-of-the-art regret $O(\sqrt{T})$ rather than $O(\sqrt{T\log T})$ even when LQR evolves under non-Gaussian noise. Our research contributes to the theoretical verification of implementing a preconditioner in reducing sample complexity as well as the derivation of such an improved regret.

> Q2: I don't understand the relationship between TS and ULA. Why don't we directly implement TS on LQR? Any computational difficulty?
- Answer to Q2: Sampling from a posterior distribution, such as Thompson sampling, often demands significant computational resources in learning LQR problems. The implementation of preconditioning aims to alleviate this challenge. While sample complexity for naive ULA is well-studied, those for preconditioned ULA are relatively less explored. In our paper, we introduce modified stepsize and the number of step iterations, leading to improved computational efficiency. Besides, our carefully chosen parameters result in better regrets.

> Q3: The paper claims that a stabilizing policy is not necessary or does not have to be prespecified. Which theoretical result supports this claim?
- Answer to Q3: To prevent exponential growth of the state norm, a stabilizing policy from the prespecified compact set is needed. We introduce 'once per episode' exploration noise $\nu_t$, resulting in the persistence of excitation (Prop 4.4). Additionally, we develop concentration properties of approximate/exact posterior distribution (Thm 4.5). Combining them, we control the probability of exponential state growth, leading to a uniform bound for the state (Thm 5.1) and an improved regret bound (Thm 5.2).

> Q4: The assumptions are complicated and lack of explanation. For example, Assumption 2.1 assumes the noise distribution is known. In practice, I don't think this holds. Justify all the assumptions!

- Answer to Q4: According to the literature for learning LQR, the noise is assumed to be known [1-7]. Hence, we respectfully assert that the assumption is standard and further claim that it is weaker. 

> Q5: Although the regret bound matches the SOTA, what about the sample complexity? Note that a rejection sampling is used in the algorithm design. Can you quantify the number of samples used in the estimation?
- Answer to Q5: We acknowledge the absence of addressing sample complexity arising from rejection. However, we recognize this as an interesting direction for future research. One possible approach is to trace the sample complexity of proximal Langevin dynamics which is devised for sampling from compact support [8]. In this work, the authors discuss the convergence of the distribution of proximal Langevin dynamics defined in the whole space to the distribution defined on the compact support. The core idea is to introduce a smooth regularizer that smoothly extends the distribution function to noncompact metric space.  

Answer to minor questions:
> Q1: Why could you assume the prior distribution as known? What can be an initial guess of the prior distribution in practice?
-Answer to Q1: If a reasonable prior is given, it helps the algorithm to learn optimal policy. Otherwise, we can simply assume that the prior is Gaussian satisfying Assumption 3.2, with the mean of each component arbitrarily set to 0.5 for experiments.

> Q2: $P$ is used in both the ARE and in defining the preconditioned matrix. Use two notations
- Answer to Q2: We appreciate the suggestion and commit to addressing it in the updated version of our paper.

> Q3: In algorithm 1, $\theta\_j$ is updated in each time step while $\tilde \theta\_k$ is updated by the end of each episode. What is the difference between these two parameters?
-Answer to Q3: Sampling $\theta_j$ for each $j$ is concerned with simulating the discretized SDE given as $X\_{j+1} = X\_j - \gamma\_j \nabla U(X\_j) + \sqrt{2\gamma_j} W\_j,$. After iterating $\tilde N\_k$ times, we check if $\theta_{\tilde N_k}$ satisfies the rejection condition. If satisfied, we set $\tilde \theta_k := \theta_{\tilde N_k}$. Hence, $\theta_j$ denotes an intermediate random sample along the SDE while $\tilde \theta\_k$ represents a system parameter we use for the next episode mimicking the exact posterior distribution.

References 

[1] Dean S, Mania H, Matni N, Recht B, Tu S. On the sample complexity of the linear quadratic regulator. Found. Comput. Math. 20:633–79, 2017

[2] Gagrani, M., Sudhakara, S., Mahajan, A., Nayyar, A., and Ouyang, Y. A modified Thompson sampling-based learning algorithm for unknown linear systems. In 2022 IEEE 61st Conference on Decision and Control (CDC), pp.6658–6665. IEEE, 2022.

[3] Lale, S., Azizzadenesheli, K., Hassibi, B., and Anandkumar, A. Reinforcement learning with fast stabilization in linear dynamical systems. In International Conference on Artificial Intelligence and Statistics, pp. 5354–5390. PMLR, 2022.

[4] Kargin, T., Lale, S., Azizzadenesheli, K., Anandkumar, A., and Hassibi, B. Thompson sampling achieves O($\sqrt{T}$) regret in linear quadratic control. In Conference on Learning Theory, pp. 3235–3284. PMLR, 2022.

[5] Jedra, Y. and Proutiere, A. Minimal expected regret in linear quadratic control. In International Conference on Artificial Intelligence and Statistics, pp. 10234–10321. PMLR, 2022.

[6] Ouyang, Y., Gagrani, M., and Jain, R. Posterior sampling based reinforcement learning for control of unknown linear systems. IEEE Transactions on Automatic Control, 65(8):3600–3607, 2019.

[7] Abeille, M. and Lazaric, A. Improved regret bounds for Thompson sampling in linear quadratic control problems. In International Conference on Machine Learning, pp.1–9. PMLR, 2018.

[8] Brosse, N., Durmus, A., Moulines, É., & Pereyra, M., Sampling from a log-concave distribution with compact support with proximal Langevin Monte Carlo. In Conference on learning theory (pp. 319-342). PMLR, 2017. 

# Reviewer 4

Thank you so much for carefully reading our paper and giving us invaluable input. Your comments are all to the point and we would like to address them very carefully. Besides, in the paper you mentioned, the authors implement the inverse of the Fisher information matrix for more efficient gradient descent and convergence guarantee which aligns with the core idea of our paper yet the scope is a bit different as we focus on improved regret. We will make sure to include it in the revised version. 

> Q1: In theorem 2.3, you only state the separate distribution of $x$ and $\tilde x$, but did not specify their joint distribution. We cannot compute the expectation in this case. I guess they are not independent, right? As a follow up, I notice that you say “we use the same Brownian motion” in line 583, proof of Lemma A.1. Maybe you mean the Wasserstein distance between the two distributions in this theorem and also in Lemma A.1?In theorem 2.3, you only state the separate distribution of and, but did not specify their joint distribution. We cannot compute the expectation in this case. I guess they are not independent, right? As a follow up, I notice that you say “we use the same Brownian motion” in line 583, proof of Lemma A.1. Maybe you mean the Wasserstein distance between the two distributions in this theorem and also in Lemma A.1?
- Answer to Q1: You are absolutely right that two distributions $p$ and $p_N$ are dependent so that joint distribution should have been specified. As pointed out, the expectations in Theorem 2.3 and Lemma A.1 are taken over joint distribution generated by the shared Brownian motion. We will revise the statement to clarify it in the revised version.

> Q2: As a follow up, for Proposition 4.1 and Lemma A.3, maybe you also mean the Wasserstein distance is bounded by Dp. Or alternatively, at least we shall specify that $\theta\_t$ and $\tilde \theta\_t$ share the same realization of Brownian motion in the statement of the proposition.
- Answer to Q2: Similar to Q1, Proposition 4.1 holds only if samples from exact and approximate posterior share the same Brownian motion. On the other hand, two key properties for the exact posterior $\mu_t$ stated in Proposition 4.2 and the first part of Theorem 4.5 hold true regardless of the coupling as only stochastic properties are leveraged.

Summarizing answer to Q1 and Q2, whenever expectation is taken for $\mu\_t$ and $\tilde \mu\_t$, we mean that joint distribution comes from the coupling induced by the shared Brownian motion.

> Q3: Page 27 line 1435. Why can you get such a form? If $X\^\top X$ and $Y\^\top Y$ are defined in line 1442, why must the other two parts be $X\^\top Y$ and $Y\^\top X$ ? Also, there should be a summation when you define $Y\^\top Y$ in line 1442.
- Answer to Q3. Sorry for the confusion. For $\mathcal{J}\_k = \\{n\_i:n\_1 \< n\_2 \< \ldots \< n\_{k(k+1)/2}\\}$, we newly definte $X$ and $Y$ as $X=[ w\_{n\_1-1} \quad \cdots \quad_{n\_{k(k+1)/2}-1} ]^\top$ and $Y = [ K\_{\nu(n\_1)}w\_{n_1-1} \quad \cdots \quad K\_{\nu(n\_{k(k+1)/2})}w\_{ n\_{k(k+1)/2}-1} ]^{\top}$. Then it is clear to have such a form.
  
> Q4: Page 36 line 1927. I don’t think we can apply tower rule this way. $\overline \theta\_\ast$ denotes the true parameter on the left, but a random variable on the right. They are not the same although you use the same notation. Maybe you need to use Prop 4.1, Prop 4.2, and triangle inequality to derive this inequality. As a follow up, a similar issue hold at page 38, the last inequality for estimation for $R\_3$.
- Answer to Q4: Regarding the tower rule on page 36 line 1927, if I understood your question correctly, we always mean the true system parameter random variable by $\overline \theta_*$ throughout the paper, and hence, conditioning with respect to the history collected before $k$th episode makes sense. In line 1927, $\overline \theta\_*$ in both equations are random variables following the exact posterior distribution. This way the inner expectation of  $\mathbb{E} [\mathbb{E}\_{\mu\_k, \tilde \mu\_k} [|\overline \theta\_\* -\theta\_k|\^2 | h\_{t\_k}]]$ is taken for two distributions $\mu_k$ and $\tilde \mu\_k$ that share the Brownian motion, and outer expectation is taken over all histories before the $k$ th episode.



For minor problems raised, here are the answers to them. We would first deeply thank the reviewer for spending time and carefully reading our paper. Your comments are mostly to the point and we believe that they will help us significantly improve the quality of the paper.

1. The term ULA first appears in line 61. We will change 'Preconditioned ULA for approximate TS' to 'Preconditioned Unadjusted Langevin Algorithm (ULA) for approximate TS' in the revised version.

2-4. You are absolutely right. They will be corrected accordingly.

5. Conditioning should be removed. All of them will be changed to $p\_w (x\_{t+1} - \Theta\^{\top}  z\_t )$.

6. Argmin is computed via Newton's method. The minimum is always achieved as the potential $U$ always convex. We will specify this part in the revised version.

7. You are right. Thank you for pointing out.

8-9. Thank you for the suggestion. We agree that it makes more sense to define $V(\tau)= \frac{1}{2} e\^{\alpha \tau}|\theta\_\tau-\theta\_*|\_{P\_t}\^2$ where $\theta\_\tau$ is a solution $\mathrm{d}\theta\_s = -P\_t^{-1}\nabla U\_t(\theta\_s)\mathrm{d}s + \sqrt{2}P\_t\^{-\frac{1}{2}}\mathrm{d}B\_s$ at time $\tau$ given $t$ and initial state $\theta_0$ that can be any vector. The initial vector should have been specified in the statement as pointed out.

10-11. We admit that there are some typos. The updated proof will include the following. 
- $F_1$: The estimate for $F_1$ presented in lines 992 - 1007 will result in 

$F_1 \leq \frac{\alpha-2m}{2}\int_0\^\tau e\^{\alpha \eta}|\theta\_\eta-\theta\_*|\_{P\_t}\^2 \mathrm{d}\eta+\int\_0\^\tau e\^{\alpha \eta} \nabla\_{\theta} U\_1(\theta\_\ast)\^{\top} (\theta_{\ast}-\theta\_\eta)\mathrm{d}\eta+\int\_0\^\tau e\^{\alpha \eta} \nabla\_{\theta} U\^\prime\_t(\theta\_\ast)\^{\top}(\theta\_\ast-\theta\_\eta)\mathrm{d}\eta$.

Therefore, lines 1025 - 1033 are modified to

$F\_1 \leq \frac{\alpha-m}{2}\int\_0\^\tau e\^{\alpha \eta}|\theta\_\eta-\theta\_\ast|\_{P\_t}\^2 \mathrm{d}\eta +\frac{1}{m} \int\_0\^\tau e\^{\alpha \eta} |P\_t\^{-\frac{1}{2}}\nabla\_{\theta} U\_1(\theta\_\ast)|\^2\mathrm{d}\eta+\frac{1}{m} \int\_0\^\tau e\^{\alpha \eta} |P\_t\^{-\frac{1}{2}}\nabla\_{\theta} U\^\prime\_t(\theta\_\ast)|\^2\mathrm{d}\eta$.

Choosing $\alpha=m$, the final result in line 1037 will be modified to
$F_1 \leq C_0 e^{\alpha \tau}+\frac{1}{m}\int_0^\tau e^{\alpha \eta}|P_t^{-\frac{1}{2}}\nabla_{\theta} U^\prime_t(\theta_\ast)|^2 \mathrm{d}\eta$.

- $F_3$: Line 988 should be updated to
$F_3 = \sqrt{2} \int_0^\tau e^{\alpha \eta} (\theta_\eta - \theta_\ast)^{\top}P_t^{\frac{1}{2}}\mathrm{d}B_\eta$.
Accordingly, line 1056 should is modified to
$F_3 \leq \mathbb{E} \bigg[\bigg(\frac{16 e^{\alpha \Delta}}{\alpha}\bigg)^{\frac{1}{2}}\Big (\sup_{0\leq \tau \leq \Delta}e^{\alpha \tau}|\theta_\tau-\theta_\ast|_{P_t}^2 \Big )^{\frac{1}{2}} \bigg]$.


13-14. Thank you for pointing out. You are comments are all correct and to the point. They will be all reflected in the new version of the paper. Regarding line 1461, 

'we also have the following event is a subevent of $E_{1,k}$:' 

should be changed to

 'we can define $E_{3,k}$ that contains the event $E_{1,k}$:' 

15. You are correct, it should be $|L_s|^2$ instead of $|L_s|$, and hence, the constant $\sqrt{M_K^2+2}$ used for $E_{4,k}$ and $E_{5,k}$ needs to be changed to just $M_K^2+2$. As you might have noticed already, it does not change the core property, the persistence of excitation (growth of $\lambda_{\min}$. It will be corrected in the updated version for sure.

16. Correct. We will fix it to $1/\sqrt{\lambda_{\min,t}}$.  

17. Yes, it should be $|\tilde \theta_t-\theta_*| > \epsilon_0$.

18. Thank you so much, we mean $J$ is Lipscthiz continuous in $\mathcal{C}$.

19. You are right! It should be 4.

20. Here is the corrected proof.
$\mathbb{E} \bigg [\sum\_{k=1}\^{n\_T} \sum\_{t=t\_k}\^{t\_{k+1} -1} |\nu\_t|\^2|\bar{\theta}\_\ast - \tilde{\theta}\_k|\bigg ] \leq \sum\_{t=t\_k}\^{t\_{k+1} -1} \sqrt{\mathbb{E}[|\nu_t|^4]}\sqrt{\mathbb{E}[|\bar{\theta}\_\ast - \tilde{\theta}\_k|^2] } \leq 2(4\bar{L}^2_\nu)^2 \sum\_{k=1}\^{n\_T} \sum\_{t=t\_k}\^{t\_{k+1} -1}\frac{1}{\sqrt{t_k}}$.

21. We appreciate again. We will fix it.

22. We will add the reference 'Dynamic Programming and Optimal Control 4th Edition, Volume 2' by Dimitri P. Bertsekas.





# Reviewer 5

We appreciate your thoughtful engagement with our paper. Despite acknowledging a lack of expertise, your perspective is invaluable. Allow us to elaborate on the assumptions regarding (a) the prior distribution (Assumption 3.2), (b) injected noise $\nu$ (Assumption 3.3), and (c) the system parameter:

(a) Regarding the prior distribution, we choose a Gaussian distribution, a common choice when prior information is unavailable. e.g., [2, 6]

(b) The injection of noise $\nu_s$ at $t=t_k - 1$ is for better exploration, with the Gaussian distribution selected for its analytical convenience and standard usage. e.g., [3,4,5]

(c) Concerning $\mathcal{C}$ (lines 220-223), borrowed from [8] we ensure that $\theta \in \mathcal{C}$ satisfies specific conditions derived from $K(\theta)$ and $J(\theta)$ as $K$ and $J$ are continuously differentiable.

Here are the answers to questions raised. 
> Q1: One of the contributions outlined is "online learning of LQR without a stabilizing parameter set". I wonder how this compares to the work of Black-Box Control for Linear Dynamical Systems, Chen and Hazan 2021?
- Answer to Q1: We emphasize that our Bayesian regret necessitates integration over the prior distribution, unlike 'Black-Box Control for Linear Dynamical Systems,' which derives high probability regret. Moreover, the assumption on system noise $|w_t|\leq 1$ in the paper is not necessary in our paper, as it imposes unnecessary constraints for tail analysis.

> How restrictive is the curvature assumption on $p\_w$?
- Q2: We respectfully claim that Assumption 2.1 aligns with established standards, equivalent to enforcing log-concavity on the density function and Lipschitz smoothness on the gradient of the log of density function as discussed in [1,7]. Additionally, we note that another study addressing Bayesian regret bounds [2,6] introduces the Gaussian noise assumption, which is a special case of our assumption. To be more illustrative, let us consider probability density functions defined in $\mathbb{R}$. Then any probability density function $p\_w(z) \sim \mathcal{N}(0, {\sigma}\^2)$ where $ \frac{1}{\overline m}\leq \sigma\^2 \leq \frac{1}{\underline m}$ satisfies Assumption 2.1. One can also include some asymmetric distributions as well. For instance, for $z\in \mathbb{R}$, one can set $p_w(z)$ to satisfy $-\frac{d\^2 \log p\_w(z)}{d z\^2}=\underline m$ if $z<-1$, $-\frac{d\^2 \log p\_w(z)}{d z\^2}=\overline m$ if $z>1$ and $-\frac{d\^2 \log p\_w(z)}{d z\^2}=(\overline m -\underline m)/2 \cdot z + \underline m + (\overline m - \underline m )/2$ if $z\in(-1,1)$, which represents an asymmetric probability distribution. Hence, we carefully claim that  Assumption 2.1 seems to be restrictive but includes many other distributions beyond Gaussian distributions.

# References

[1]  Dean S, Mania H, Matni N, Recht B, Tu S. On the sample complexity of the linear quadratic regulator. Found. Comput. Math. 20:633–79, 2017

[2] Gagrani, M., Sudhakara, S., Mahajan, A., Nayyar, A., and Ouyang, Y. A modified Thompson sampling-based learning
algorithm for unknown linear systems. In 2022 IEEE 61st Conference on Decision and Control (CDC), pp.6658–6665. IEEE, 2022.

[3] Lale, S., Azizzadenesheli, K., Hassibi, B., and Anandkumar, A. Reinforcement learning with fast stabilization in linear dynamical systems. In International Conference on Artificial Intelligence and Statistics, pp. 5354–5390. PMLR, 2022.

[4] Kargin, T., Lale, S., Azizzadenesheli, K., Anandkumar, A., and Hassibi, B. Thompson sampling achieves O($\sqrt{T}$) regret in linear quadratic control. In Conference on Learning Theory, pp. 3235–3284. PMLR, 2022.

[5] Jedra, Y. and Proutiere, A. Minimal expected regret in linear quadratic control. In International Conference on Artificial Intelligence and Statistics, pp. 10234–10321. PMLR, 2022.

[6] Ouyang, Y., Gagrani, M., and Jain, R. Posterior sampling based reinforcement learning for control of unknown linear systems. IEEE Transactions on Automatic Control, 65(8):3600–3607, 2019.

[7] Dwivedi, R., Chen, Y., Wainwright, M. J., and Yu, B. Logconcave sampling: Metropolis-Hastings algorithms are fast! In Conference on learning theory, pp. 793–797. PMLR, 2018.

[8] Abeille, M. and Lazaric, A. Improved regret bounds for Thompson sampling in linear quadratic control problems. In International Conference on Machine Learning, pp.1–9. PMLR, 2018.





# Reviewer 1


Thank you for your insightful comments and engaging questions. Here are answers to your concerns and questions.

>From a theoretical guarantee perspective, this work analyzes the expected regret, whereas previous works have focused more on high probability bounds, which better reflect the impact of noise.

>In terms of noise assumptions, adaptive control typically assumes that the noise only has bounded second moments, which is more general or classical. While this paper relaxes some assumptions compared to certain works, it still imposes a strong assumption. The purpose of such an assumption is often to obtain non-asymptotic theoretical guarantees. Traditional adaptive control results are mostly asymptotic. Additionally, I am not sure how much weaker your new assumption is compared to the Gaussian assumption, because generally, bounded higher moments can also provide non-asymptotic guarantees.


>From the perspective of the development of online adaptive control, the significance of this work is not very evident. The main contribution of this work, in my opinion, is removing the term in the regret bound, achieving a clean bound. However, there are many more important breakthroughs needed, such as focusing on more general curvature settings, such as convex or strongly convex, Lipschitz or smooth settings, and allowing for more adverse noise conditions, even non-stochastic (online non-stochastic control).

>The experiments lack comparison with similar algorithms. There are many works studying online adaptive control for LQR, as mentioned in the paper, but the experiments only compare with one algorithm.



# Reviewer 2
>Suppose $\theta_t$ is the target variate, its stationary distribution should be the same as in the unadjusted version. Why would we expect any improvement in the exact posterior rate (Proposition 4.2) rather than merely recovering the rate noted in [1]?

>The better convergence rate for preconditioned LMC seems to stem from assumptions related to the condition number, which only shows an improvement with a better constant number.

>With the same rate, it remains unclear why preconditioned LMC would provide better performance in concentration rates and regrets. Could the authors clarify how their concentration rate differs from previous studies [1]? Additionally, does this affect the results in Section 4.3 and the optimal regrets?

>In Algorithm 1 Line 12, does the author use the full gradient or stochastic gradient for the objective?

>What is the prior used in the analysis? Does that affect the results?

# Reviewer 3

>The effectiveness of the excitation mechanism and preconditioner may heavily depend on specific problem settings and may not generalize well.

>There is limited comparison with existing state-of-the-art methods in stochastic bandits or RL algorithms, which could provide insights into the algorithm's advantages and limitations in different contexts.

>Why don't we directly implement TS on LQR? Can you explain the motivation of applying ULA?

>The assumptions in this paper are strong. Can you explain their necessity and realizability? What theoretical problems would be encountered without these assumptions?

>What computational challenges or practical considerations arise when scaling the algorithm to high-dimensional state spaces?

