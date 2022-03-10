# Table of Contents

- [Table of Contents](#table-of-contents)
- [Reviews](#reviews)
- [Non-Bayesian Methods](#non-bayesian-methods)
- [Bayesian Methods](#bayesian-methods)

# Reviews

> Predictive uncertainty is important because it enables accurate assessment of risk, allows practitioners to know how accuracy may degrade, and allows a system to abstain from decisions due to low confidence.

- [Aleatoric and Epistemic Uncertainty in Machine Learning: An Introduction to Concepts and Methods](https://arxiv.org/abs/1910.09457) (arXiv2019)
    - 5分
    - 关于uncertainty的综述。
    - Aleatoric uncertainty refers to the irreducible part of the uncertainty, which is due to the non-deterministic nature of the sought input/output dependency. Epistemic uncertainty is, uncertainty due to a lack of knowledge about the perfect predictor, for example caused by uncertainty about the parameters of a model. In principle, this uncertainty can be reduced.
    - Aleatoric Uncertainty指的是任务本身的不确定性（如数据存在noise或任务本身很困难），即使best model也只能给出不确定的预测。而Epistemic Uncertainty指的是由于认知不足（如数据量不足），训练得到的模型与best model之间存在的差异。
    - 两个概念是相对给定的(X,Y,H,P)而言的。增加数据量（X空间没变）可以降低Epistemic Uncertainty，增加数据维度（X空间改变）可以降低Aleatoric Uncertainty。而OOD问题可以看成是训练数据只占了真实分布P（X，Y）的一小部分，因此存在很大的Epistemic Uncertainty。
    - 在BNN中，epistemic uncertainty is modeled by placing a prior distribution over a model’s weights(因此也叫model uncertainty), aleatoric uncertainty on the other hand is modeled by placing a distribution over the output of the model(这里指的是在fix W的情况下的output，在分类中就是softmax的输出，而在回归问题中通常输出一个Gaussian，如果没有fix那么可以认为output刻画的是total uncertainty).
    - 当假设空间H非常大（如DNN）时，model uncertainty essentially disappears, while approximation uncertainty might be high。此时，inductive inference will essentially be of a local nature: A class y is approximated by the region in the instance space in which examples from that class have been seen, aleatoric uncertainty occurs where such regions are overlapping（如两类非常相似，本身就难以区分）, and epistemic uncertainty where no examples have been encountered so far(如OOD样本)。
    - 公式(22)和(23)很有意思，后者是先ensemble再求熵用于估计total uncertainty，前者是先求熵再ensemble用于估计Aleatoric Uncertainty，而两者的差就是Epistemic Uncertainty。容易验证：当Epistemic Uncertainty为0时，相当于所有单个模型都是一样的，那total uncertainty显然等于Aleatoric Uncertainty，因此差确实为0。

# Non-Bayesian Methods

> Non-Bayesian的方法。

- [On Calibration of Modern Neural Networks](https://arxiv.org/abs/1706.04599) (ICML2017)
    - 5分
    - 第一篇考虑DNN中的calibration问题的工作。
    - 有意思的一个点是对DNN很难overfit的一个猜测：overfit可能并不是发生在test error上，而是发生在test NLL loss上，也就是说在不断训练过程中，虽然模型的预测准确率保持很高，但是模型的预测趋向于overconfident。
    - 如果只有训练集很难做calibration，因为训练集准确率很高，那么模型只需要输出高置信度即可。因此可以通过post-hoc calibration方法找到一个映射，使得模型的输出经过这个映射后在验证集上达到最优。
    - 但这样的方法显然是在期望意义下的，是对模型整体的一个校准。如果我们需要考虑数据本身，比如用户需要模型对自己特定的某个输入产生准确的置信度，这样的方法是不精准的。

- [Simple and Scalable Predictive Uncertainty Estimation using Deep Ensembles](https://arxiv.org/abs/1612.01474) (NIPS2017)
    - 4分
    - 通过ensemble的方法来刻画uncertainty。
    - How to evaluate predictive uncertainty？ held-out validation set + proper scoring rules.
    - 方法很简单，认为ensemble可以使得Predictive Uncertainty Estimation更加精准，可以分为分类和回归两种情况：
      - 分类：直接将多个model输出的后验概率分布做平均。注意这里仍然是使用后验概率分布加scoring rules的方式来刻画uncertainty。
      - 回归：单个模型训练时输出的$\sigma$项是在刻画aleatoric uncertainty。ensemble后的$\sigma$项可以分解为aleatoric uncertainty和epistemic uncertainty两项。

# Bayesian Methods

> BNNs(Bayesian Neural Networks) learn a posterior distribution over parameters that quantifies parameter
uncertainty, a type of epistemic uncertainty that can be reduced through the collection of additional data.

- [What Uncertainties Do We Need in Bayesian Deep Learning for Computer Vision?](https://arxiv.org/abs/1703.04977) (NIPS2017)
    - 5分
    - 分离aleatoric uncertainty和epistemic uncertainty。
    - 为什么要分离？
      - it is important to model aleatoric uncertainty for:
        - Large data situations, where epistemic uncertainty is explained away,
        - Real-time applications, because we can form aleatoric models without expensive Monte Carlo samples.
      - And epistemic uncertainty is important for:
        - Safety-critical applications, because epistemic uncertainty is required to understand examples which are different from training data,
        - Small datasets where the training data is sparse.
    - 具体做法：
      - 认为aleatoric uncertainty是data-dependent的，因此让模型直接输出一个$\sigma$项去建模aleatoric uncertainty，具体实现时增加一个head即可。此时loss本质上是对Gaussian做MLE。
      - epistemic uncertainty即多次采样输出的方差，这里不管用BNN，MC-dropout或ensemble，思想都是一致的。
      - 分类问题在logit空间上做即可。