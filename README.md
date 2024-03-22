# Reviewer 1

We thank you for your insightful comments and interesting questions! We are glad that you positively highlight our theoretical insights. Please find the answers to your questions below.

- Q1: 

(a) Thank you for bringing up the dimensionality problem. 

(b) For the motivation of our approach, we would like to mention Theorem 2.3 where it is noted that the stepsize $\gamma= O\big (\frac{\lambda_{\min}}{\lambda_{\max}^2}\big )$ and the number of iterations $N$ satisfy $N = O  \big ((\frac{\lambda_{\max}}{\lambda_{\min}} )^2\big )$ to achieve $1/{\lambda_{\min}}$ rate of convergence when the standard ULA is used. On the other hand, by normalizing the potential $\nabla^2U$ with the preconditioner constructed from the data, we have a uniform bound of the curvature of $\nabla^2 U$ (Lemma 3.1), which allows us to use a smaller number of step iterations as suggested in (8) while achieving a better regret bound.

- Q2: 

- Q3: 

# Reviewer 2

We thank you for the review and appreciate your time reviewing our paper as well as some positive inputs.

- Q1: Sec 2.2: Can truth be random? First, authors say that \theta^* is an unknown true parameter, but later in the same section, they say that \theta^* is random. Moreover, if you know the distribution of \theta^*, why do you update it using data? If you recall, the objective of the Bayesian experiment is to recover the truth, so if the truth itself is random, can we recover the random truth using the Bayesian posterior?

- Q2
  
(a) With regard to Assumption 2.1, we respectfully suggest that Assumption 2.1 aligns with established standards, equivalent as enforcing log-concavity on the density function and Lipschitz smoothness on the gradient of the density function, as discussed in (Dwivedi et al., 2018). Additionally, we note that another study addressing Bayesian regret bounds (Ouyang et al., 2019) introduces the Gaussian noise assumption, which remains consistent with our assumption.

(b) As highlighted, the stochastic nature of the policy is indeed a central aspect, as we adopt the framework of Thompson Sampling (TS) where actions are expressed as $u_t=K x_t$ or $u_t=K x_t+ \nu_t$, as illustrated in Figure 1. The inclusion of Section 2.1 serves to inspire readers regarding such an exploration scheme, drawing parallels to the stochastic Linear Quadratic Regulation (LQR) problem under Gaussian perturbation, where the optimal control action is expressed as $u_t=K x_t$ (Theorem 2.2). Lastly, we acknowledge the typo regarding the notation $\pi_t: H_t \rightarrow \mathbb{R}^m$, which should be corrected to $\pi_t: H_t \rightarrow \mathbb{R}^{n_u}$, and we assure you that this will be reflected in the revised version.

- Q3: Section 2.3: it would be helpful if the authors clearly described the relation between episode k and time there.
In particular, I am confused because it is not clear whether the posterior is sampled only at the beginning of the episode or at each time t after it is updated. (This is only clear after looking at the algorithm)

- Q4: We admit that $N \geq O( (\lambda_{\max} / \lambda_{\min})^2)$ should be changed to $N \geq \Omega( (\lambda_{\max} / \lambda_{\min})^2)$ where $\Omega $ indicates. In Theorem 2.3, we Moreover, how does this theorem imply the concentration of X_n to sample from p?
- We use $a_n = O(b_n)$ whenever $\limsup_{n \to \infty} |a_n/b_n |< \infty$, employ $a_n = \Omega(b_n)$ for $\liminf_{n \to \infty}|a_n/b_n |>0$.

- Q5: 197-202C2: Is it obvious that preconditioning by P_t (7) reduces N? If so, how? How is the preconditioning introduced in the main algorithm different from the typical preconditioning used in ULA?

- Q6: Sec. 3.2: The idea for choosing stabilizing parameters is adapted from Abeille & Lazaric(2018). I think the authors should specify their contribution clearly.

# Reviewer 3
I have the following major concerns:

What is the goal/motivation of this paper? If the regret bound is the same as the ones obtained in prior works, why do we need to design a new algorithm? What's the advantage of the TS-based algorithm?
I don't understand the relationship between TS and ULA. Why don't we directly implement TS on LQR? Any computational difficulty?
The paper claims that a stabilizing policy is not necessary or does not have to be prespecified. Which theoretical result supports this claim?
The assumptions are complicated and lack of explanation. For example, Assumption 2.1 assumes the noise distribution is known. In practice, I don't think this holds. Justify all the assumptions!
Although the regret bound matches the SOTA, what about the sample complexity? Note that a rejection sampling is used in the algorithm design. Can you quantify the number of samples used in the estimation?
I also have minor questions:

We deeply appreciate your insightful comments and inputs.

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

I think the paper “Single timescale actor-critic method to solve the linear quadratic regulator with convergence guarantees” (JMLR2023) can be added to the related work. This paper uses the inverse of Fisher information matrix as preconditioner for policy gradient in the LQR problem. It also injects a noise in the control for exploration. Below are details of my questions.

Major problems

In theorem 2.3, you only state the separate distribution of 
 and 
, but did not specify their joint distribution. We cannot compute the expectation in this case. I guess they are not independent, right? As a follow up, I notice that you say “we use the same Brownian motion” in line 583, proof of Lemma A.1. Maybe you mean the Wasserstein distance between the two distributions in this theorem and also in Lemma A.1?

As a follow up, for Proposition 4.1 and Lemma A.3, maybe you also mean the Wasserstein distance is bounded by Dp. Or alternatively, at least we shall specify that 
 and 
 share the same realization of Brownian motion in the statement of the proposition.

Page 27 line 1435. Why can you get such a form? If 
 and 
 are defined in line 1442, why must the other two parts be 
 and 
? Also, there should be a summation when you define 
 in line 1442.

Page 36 line 1927. I don’t think we can apply tower rule this way. 
 denotes the true parameter on the left, but a random variable on the right. They are not the same although you use the same notation. Maybe you need to use Prop 4.1, Prop 4.2, and triangle inequality to derive this inequality. As a follow up, a similar issue hold at page 38, the last inequality for estimation for 
.

# Reviewer 5

Thank you for your sincere engagement with our paper. Despite a stated lack of expertise, we appreciate your perspective.

> Q1: One of the contributions outlined is "online learning of LQR without a stabilizing parameter set". I wonder how this compares to the work of Black-Box Control for Linear Dynamical Systems, Chen and Hazan 2021?



> Q2: How restrictive is the curvature assumption on $p_w$?

Here is our answer to your question. With regard to Assumption 2.1, we respectfully suggest that Assumption 2.1 aligns with established standards, equivalent as enforcing log-concavity on the density function and Lipschitz smoothness on the gradient of the density function, as discussed in (Dwivedi et al., 2018). Additionally, we note that another study addressing Bayesian regret bounds (Ouyang et al., 2019) introduces the Gaussian noise assumption, which remains consistent with our assumption. To be more illustrative, let us consider probability density functions defined in $\mathbb{R}$. Then any probability density function $p_w(z) \sim \mathcal{N}(0, {\sigma}^2)$ where $ \frac{1}{\overline m}\leq \sigma^2 \leq \frac{1}{\underline m}$ satisfies Assumption 2.1. One can also include some asymmetric distributions as well. For instance, for $z\in \mathbb{R}$, one can $p_w(z)$ to satisfy $ -\frac{d^2 \log p_w(z)}{d z^2}=\underline m$ if $z<-1$, $ -\frac{d^2 \log p_w(z)}{d z^2}=\overline m$ if $z>1$ and $-\frac{d^2 \log p_w(z)}{d z^2}=(\overline m -\underline m)/2 \cdot z + \underline m + (\overline m - \underline m )/2$ if $z\in(-1,1)$, which represents an asymmetric probability distribution. Hence, we carefully claim that  Assumption 2.1 seems to restrictive but include many other distributions beyond Gaussian distributions.
