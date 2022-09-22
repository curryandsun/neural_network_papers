# Table of Contents

- [Table of Contents](#table-of-contents)
- [Reviews](#reviews)
- [Model-agnostic Methods](#model-agnostic-methods)
- [Model-specific Methods](#model-specific-methods)
  - [NN-based](#nn-based)
  - [Tree-based](#tree-based)

# Reviews

> Causal inference frameworks include the potential outcome framework (by Donald Rubin) and the structural causal model (by Judea Pearl). Treatment Effect Estimation is an important task in the potential outcome framework.

> W:treatments, X:variables, Y:outcomes, C:Confounders

- [A Survey on Causal Inference](https://arxiv.org/abs/2002.02770) (arXiv2020)
    - 5分
    - 关于potential outcome framework下的causal inference的综述。
    - 三个经典的assumption:
      - Assumption 1: unit之间独立（如在社交网络中是不成立的）以及the single version for each treatment。
      - Assumption 2: 在给定X时，W与Y独立。这表示当给定X时，W可以看成random给的。这个假设隐含着所有confounders都包含在给定的X之中，但在实际情况中显然可能存在一些confounders没有被观测到。
      - Assumption 3 Positivity表明在给定X时，任意的W都是可能的。
    - Two major challenges in treatment effect estimation and confounder bias are:
      - Missing counterfactuals outcome: 对照实验是代价高昂的，现实中往往只能观测到一个outcome，而其余的treatment下的outcome即counterfactual outcome.
      - Confounder bias: Confounders are the variables that affect both the treatment assignment and the outcome. Confounders导致了treatment group和control group之间的selection bias，相当于covariate shift问题。
    - Propensity score即e(x)=P(W=1|X=x)，刻画了给定X=x对于treatment的偏好。实际上这里的X也可以写成C。实际应用中通常用lr去估计。

# Model-agnostic Methods

> 与具体使用的模型无关，强调的是方案的框架与思路，又被称作meta-learner。

- [Meta-learners for Estimating Heterogeneous Treatment Effects using Machine Learning](https://arxiv.org/abs/1706.03461) (PNAS2019)
- [Nonparametric estimation of heterogeneous treatment effects: From theory to learning algorithms](https://arxiv.org/abs/1706.03461) (AISTATS2021)
- [Towards optimal doubly robust estimation of heterogeneous causal effects](https://arxiv.org/abs/2004.14497) (arXiv2020)
- [Quasi-Oracle Estimation of Heterogeneous Treatment Effects](https://arxiv.org/abs/1712.04912) (arXiv2017)
  - 5分
  - meta-learner中又包括indirect learners以及direct learners，后者目标是直接建模effect，而前者并不是。
    - indirect learners: 包括S-learner和T-learner两种最基本的baseline。
    - direct learners: 通常是两阶段的建模过程，第一阶段首先得到effect的伪标记，即论文中常提到的pseudo-outcomes。第二阶段再利用effect的伪标记做回归直接建模得到从X到effect的模型。


# Model-specific Methods

> 与具体使用的模型相关的方法，包括基于NN的，基于树的方法等等。

## NN-based

> 使用NN时，基于表示学习来缓解selection bias带来的对照组和实验组的分布偏移。

- [Learning Representations for Counterfactual Inference](https://arxiv.org/abs/1605.03661) (ICML2016)
- [Estimating individual treatment effect: generalization bounds and algorithms](https://arxiv.org/abs/1606.03976) (ICML2017)
    - 4分
    - 借用DA中的那一套方法和理论做表征对齐。用NN时非常好用的方法。

## Tree-based

> 使用Tree时，通过改变节点分裂的方式缓解selection bias。

- [Bayesian Nonparametric Modeling for Causal Inference](https://www.google.com/search?q=Bayesian+Nonparametric+Modeling+for+Causal+Inference&oq=Bayesian+Nonparametric+Modeling+for+Causal+Inference&aqs=chrome..69i57j0i30j69i61.2065j0j4&sourceid=chrome&ie=UTF-8) (JCGS2011)
- [Generalized Random Forests](https://arxiv.org/abs/1610.01271) (arXiv2016)
  - 5分
  - 非常知名的两个基于树的方法，分别为BART和Causal Forest。