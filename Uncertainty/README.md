# Table of Contents

- [Table of Contents](#table-of-contents)
- [Reviews](#reviews)
- [Methods](#methods)

# Reviews

> 关于uncertainty的综述。

- [Aleatoric and Epistemic Uncertainty in Machine Learning: An Introduction to Concepts and Methods](https://arxiv.org/abs/1910.09457) (arXiv2019)
    - 5分
    - 关于uncertainty的综述。
    - Aleatoric uncertainty refers to the irreducible part of the uncertainty, which is due to the non-deterministic nature of the sought input/output dependency. Epistemic uncertainty is, uncertainty due to a lack of knowledge about the perfect predictor, for example caused by uncertainty about the parameters of a model. In principle, this uncertainty can be reduced.
    - Aleatoric Uncertainty指的是任务本身的不确定性，即使best model也只能给出不确定的预测。而Epistemic Uncertainty指的是由于认知不足（如数据量不足），训练得到的模型与best model之间存在的差异。
    - 两个概念是相对给定的(X,Y,H,P)而言的。增加数据量（X空间没变）可以降低Epistemic Uncertainty，增加数据维度（X空间改变）可以降低Aleatoric Uncertainty。而OOD问题可以看成是训练数据只占了真实分布P（X，Y）的一小部分，存在严重的认知不足，因此存在很大的Epistemic Uncertainty。
    - 典型的例子就是抛硬币：模型非常置信地预测下一次的概率是正反各50%，那这个“正反各50%”刻画Aleatoric Uncertainty，“非常置信”刻画Epistemic Uncertainty。也就是说，uncertainty可以是一个新的维度，用于刻画模型输出的不确定性，这个概念和OOD中经常直接用softmax输出本身来刻画uncertainty是有区别的。

# Methods

> 刻画uncertainty的具体方法。

- [On Calibration of Modern Neural Networks](https://arxiv.org/abs/1706.04599) (ICML2017)
    - 5分
    - 第一篇考虑DNN中的calibration问题的工作。
    - 有意思的一个点是对DNN很难overfit的一个猜测：overfit可能并不是发生在test error上，而是发生在test NLL loss上，也就是说在不断训练过程中，虽然模型的预测准确率保持很高，但是模型的预测置信度趋向于overconfident。
    - 如果只有训练集很难做calibration，因为训练集准确率很高，那么模型只需要输出高置信度即可。因此可以通过post-hoc calibration方法找到一个映射，使得模型的输出经过这个映射后在验证集上达到最优。
    - 但这样的方法显然是在期望意义下的，是对模型整体的一个校准。如果我们需要考虑数据本身，比如用户需要模型对自己特定的某个输入产生准确的置信度，这样的方法是不精准的。

- [Simple and Scalable Predictive Uncertainty Estimation using Deep Ensembles](https://arxiv.org/abs/1612.01474) (NIPS2017)
    - 5分
    - 通过ensemble的方法来刻画uncertainty。
    - How to evaluate predictive uncertainty？ held-out validation set + proper scoring rules.
    - 方法很简单，可以分为分类和回归两种情况：
      - 分类：直接将多个model输出的后验概率分布做平均，相当于降低了model uncertainty，使其更接近best model。注意这里仍然是使用后验概率分布加scoring rules的方式来刻画uncertainty，而没有使用新的维度。
      - 回归：单个模型训练时就考虑方差这一新的维度来刻画uncertainty。ensemble同样相当于降低了model uncertainty，使得均值以及方差的估计都更加精准。