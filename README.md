# Reviewer 1

We thank you for your insightful comments and interesting questions! We are glad that you positively highlight our theoretical insights. Please find the answers to your questions below.

- Q1: 

(a) Thank you for bringing up the dimensionality problem. 

(b) For the motivation of our approach, we would like to mention Theorem 2.3 where it is noted that the stepsize $\gamma= O\big (\frac{\lambda_{\min}}{\lambda_{\max}^2}\big )$ and the number of iterations $N$ satisfy $N = O  \big ((\frac{\lambda_{\max}}{\lambda_{\min}} )^2\big )$ to achieve $1/{\lambda_{\min}}$ rate of convergence when the standard ULA is used. On the other hand, by normalizing the potential $\nabla^2U$ with the preconditioner constructed from the data, we have a uniform bound of the curvature of $\nabla^2 U$ (Lemma 3.1), which allows us to use a smaller number of step iterations as suggested in (8) while achieving a better regret bound.

- Q2: Ouyang의 restrictive한 가정을 제거하면 state의 norm이 지수적으로 커지게되면서 덩달아 cost가 커져 이론적으로나, 실험적으로 둘다 regret이 지수적으로 가파르게 증가하는 문제가 생긴다. 이를 해결하기 위해 우선 Abeille에서 사용되었던 compact set에 rejection하는 방식으로 바꾸었다. True system parameter에 대한 concentration은 O(1/\sqrt{lambda_min})이고, 점점 수렴할수록 true system parameter와 가까운 값을 뽑을 가능성이 큼으로, state의 폭발을 야기하는 system parameter를 샘플링할 가능성이 적어진다. 그러므로 action에 noise를 첨가하는 방식으로 lambda_min의 polynomial한 증가를 이끌어내어 true system parameter와 멀리 떨어져 있어 state의 폭발을 야기하는 system parameter를 뽑을 확률을 최대한 작게 control하여 state moment의 constant bound를 유도하였다. 

- Q3: 랑주뱅과 톰슨샘플링의 결합에서의 핵심은 랑주뱅으로 posterior의 근사샘플링을 하는 것이다. 상당부분 mazumdar의 결과를 가져왔는데, 마줌다는 reward function에 대해 strongly log concave하다는 가정을 하였다. LQR setting에서는 reward function에 대응되는 것이 log-concave하기 때문에 naive하게 가져오면 posterior가 strongly log-concavity가 data에 따라 linear하게 증가하지않아, number of iteration에 중요한 요소인condition number가 엄청나게 증가하여 computational inefficiency를 야기한다. 그러므로 이 문제를 해결하기 위하려 preconditioning matrix를 도입하였으며 이로 인해 lamda_min과 \lambda_max의 격차를 normalizing함으로써 줄일 수 있었고 이로인해 computational efficiency를 달성할 수 있었다. 

# Reviewer 2

We thank you for the review and appreciate your time reviewing our paper as well as some positive inputs.

- Q1: Sec 2.2: Can truth be random? First, authors say that \theta^* is an unknown true parameter, but later in the same section, they say that \theta^* is random. Moreover, if you know the distribution of \theta^*, why do you update it using data? If you recall, the objective of the Bayesian experiment is to recover the truth, so if the truth itself is random, can we recover the random truth using the Bayesian posterior?

- Q2
  
(a) With regard to Assumption 2.1, we respectfully suggest that Assumption 2.1 aligns with established standards, equivalent as enforcing log-concavity on the density function and Lipschitz smoothness on the gradient of the density function, as discussed in (Dwivedi et al., 2018). Additionally, we note that another study addressing Bayesian regret bounds (Ouyang et al., 2019) introduces the Gaussian noise assumption, which remains consistent with our assumption.

(b) As highlighted, the stochastic nature of the policy is indeed a central aspect, as we adopt the framework of Thompson Sampling (TS) where actions are expressed as $u_t=K x_t$ or $u_t=K x_t+ \nu_t$, as illustrated in Figure 1. The inclusion of Section 2.1 serves to inspire readers regarding such an exploration scheme, drawing parallels to the stochastic Linear Quadratic Regulation (LQR) problem under Gaussian perturbation, where the optimal control action is expressed as $u_t=K x_t$ (Theorem 2.2). Lastly, we acknowledge the typo regarding the notation $\pi_t: H_t \rightarrow \mathbb{R}^m$, which should be corrected to $\pi_t: H_t \rightarrow \mathbb{R}^{n_u}$, and we assure you that this will be reflected in the revised version.

- Q3: Thank you for the suggestion. We will add the relation between $t$ and $k$ in the beginning of Section 2.3 for better clarity.


- Q4:  We use $a_n = O(b_n)$ whenever $\limsup_{n \to \infty} |a_n/b_n |< \infty$, employ $a_n = \Omega(b_n)$ for $\liminf_{n \to \infty}|a_n/b_n |>0$. We also admit that $N \geq O( (\lambda_{\max} / \lambda_{\min})^2)$ should be changed to $N = \Omega( (\lambda_{\max} / \lambda_{\min})^2)$.  For the concentration, $X_N$ comes from the ULA given by $X_{j+1} = X_j - \gamma_j \nabla U(X_j) + \sqrt{2\gamma_j} W_j$ and its distribution is denoted by $p_N$. Since probability density functions exist for both $p_N$ and $p$, the integration in Theorem 2.3 measures the difference between them.

- Q5: The reduction of the number of iterations $N$ by implementing the preconditioner should not be obvious at first glance. A typical preconditioning used in ULA does not fully exploit the upper and lower bound of $P^{-1/2} \nabla^2 U P^{1/2}$ while we leverage the uniform bound of this quantity by introducing a structural assumption on $p_w$. Using this bound we could reduce the number of iterations achieving a better concentration.

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

weakness 1
우리의 주제는 Bayesian setting에서의 TS-based LQR online learning이고 이와 비교할 수 있는 paper는 ouyang과 gagrini가 있다. $\tilde{O}(\sqrt(T))$ regret bound를 얻기 위해서 그들은 unrealizable 한 compact set을 고려하였다. Ouyang은 compact set에 rejection하기 위해 반드시 우리가 구하고자 하는 true system parameter를 알아야 하고, gargrini는 실험적으로 구현 불가능한 compact set을 제시하였다. 이 두 paper의 한계점은 related work에서 102c1~60c2에 잘 서술되어 있다. 또한 이런 한계점들을 우리가 어떻게 극복하고 $O(\sqrt(T))$의 bound를 얻었는지 193c1~207c1, 에 서술되어있다. 2578 에는 실험적으로 ouyang과 comparison한 결과가 있다.

weakness 2
두 번째 assumption은 prior에 관한 것이다. Prior는 우리가 선택해서 잡을 수 있기 때문에 restrictive한 assumption이 아니다. 세 번째 assumption은 우리가 action을 취할 때 고의적으로 넣는 perturbation이다. 다시 말해 우리가 구하고자 하는 unknown system parameter과는 무관한, 우리가 원하는 대로 잡을 수 있는 값이다.

> Q1: One of the contributions outlined is "online learning of LQR without a stabilizing parameter set". I wonder how this compares to the work of Black-Box Control for Linear Dynamical Systems, Chen and Hazan 2021?


> Q2: How restrictive is the curvature assumption on $p_w$?

Here is our answer to your question. With regard to Assumption 2.1, we respectfully suggest that Assumption 2.1 aligns with established standards, equivalent as enforcing log-concavity on the density function and Lipschitz smoothness on the gradient of the density function, as discussed in (Dwivedi et al., 2018). Additionally, we note that another study addressing Bayesian regret bounds (Ouyang et al., 2019) introduces the Gaussian noise assumption, which remains consistent with our assumption. To be more illustrative, let us consider probability density functions defined in $\mathbb{R}$. Then any probability density function $p_w(z) \sim \mathcal{N}(0, {\sigma}^2)$ where $ \frac{1}{\overline m}\leq \sigma^2 \leq \frac{1}{\underline m}$ satisfies Assumption 2.1. One can also include some asymmetric distributions as well. For instance, for $z\in \mathbb{R}$, one can $p_w(z)$ to satisfy $ -\frac{d^2 \log p_w(z)}{d z^2}=\underline m$ if $z<-1$, $ -\frac{d^2 \log p_w(z)}{d z^2}=\overline m$ if $z>1$ and $-\frac{d^2 \log p_w(z)}{d z^2}=(\overline m -\underline m)/2 \cdot z + \underline m + (\overline m - \underline m )/2$ if $z\in(-1,1)$, which represents an asymmetric probability distribution. Hence, we carefully claim that  Assumption 2.1 seems to restrictive but include many other distributions beyond Gaussian distributions.
