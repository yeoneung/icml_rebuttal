##Reviewer 1

We thank you for your insightful comments and interesting questions! We are glad that you positively highlight our theoretical insights. Please find the answers to your questions below.

Q1: 

(a) Thank you for bringing up the dimensionality problem. 

(b) For the motivation of our approach, we would like to mention Theorem 2.3 where it is noted that the stepsize $\gamma= O\big (\frac{\lambda_{\min}}{\lambda_{\max}^2}\big )$ and the number of iterations $N$ satisfy $N \geq O  \big ((\frac{\lambda_{\max}}{\lambda_{\min}} )^2\big )$ to achieve $1/{\lambda_{\min}}$ rate of convergence when the standard ULA is used. On the other hand, by normalizing the potential $\nabla^2U$ with the preconditioner constructed from the data, we have a uniform bound of the curvature of $\nabla^2 U$ (Lemma 3.1), which allows us to use a smaller number of step iterations as suggested in (8) while achieving a better regret bound.

Q2: 

Q3: 

##Reviewer 2


Sec 2.2: Can truth be random? First, authors say that \theta^* is an unknown true parameter, but later in the same section, they say that \theta^* is random. Moreover, if you know the distribution of \theta^*, why do you update it using data? If you recall, the objective of the Bayesian experiment is to recover the truth, so if the truth itself is random, can we recover the random truth using the Bayesian posterior?
Assumption 2.1: Are these standard assumptions? Please discuss 101C2- Why is the policy deterministic, given that the authors build on TS? u_t dim is n_u, so why policy maps to \R^m?
Section 2.3: it would be helpful if the authors clearly described the relation between episode k and time there. In particular, I am confused because it is not clear whether the posterior is sampled only at the beginning of the episode or at each time t after it is updated. (This is only clear after looking at the algorithm)
Theorem 2.3: Please define O() precisely. What does N \geq O( (\lambda_max / \lambda_min)^2) mean? Moreover, how does this theorem imply the concentration of X_n to sample from p?
197-202C2: Is it obvious that preconditioning by P_t (7) reduces N? If so, how? How is the preconditioning introduced in the main algorithm different from the typical preconditioning used in ULA?
Sec. 3.2: The idea for choosing stabilizing parameters is adapted from Abeille&Lazaric(2018). I think the authors should specify their contribution clearly.


