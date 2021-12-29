# Table of Contents

- [Table of Contents](#table-of-contents)
- [Domain Adaptation](#domain-adaptation)
- [OOD Generalization](#ood-generalization)

# Domain Adaptation

> 领域自适应。以下以S表示source domain，T表示target domain。

- [ToAlign: Task-oriented Alignment for Unsupervised Domain Adaptation](https://arxiv.org/abs/2106.10812) (NIPS2021)
    - 3分
    - 用Grad-CAM找到source feature中task-oriented的部分，并使得target feature仅与该部分做align。这样的做法在别的领域并不新鲜，如在Long-tail中有类似的[工作1](https://arxiv.org/abs/2008.03673)、[工作2](https://ojs.aaai.org/index.php/AAAI/article/view/16458)。

- [Maximum Classifier Discrepancy for Unsupervised Domain Adaptation](https://arxiv.org/abs/1712.02560) (CVPR2018_oral)
    - 5分
    - 提出的问题很好：之前的工作简单把S和T的feature做align，这样做没有考虑类别信息。oracle的做法应该是只把S和T中类别相等的feature做align。
    - 提出的做法很novel（OOD检测中的[MCD](https://arxiv.org/abs/1908.04951)是直接受到这篇的启发）：从[ADDA](https://arxiv.org/abs/1702.05464)的框架来看，本文实质上就是把adversarial objective改成了classifier discrepancy。
    - 这个做法真的可以解决这个问题吗？本文其实暗含一个假设就是S和T中同类的样本的feature应该是接近的（就像论文中的Figure 2 一样），这样在minimize discrepancy这一步时才能将T上的feature移动到对应类别的support上。如果这个假设不成立，那移动的方向就有可能是错误的，那也并不能解决提出的问题。

- [Adversarial Discriminative Domain Adaptation](https://arxiv.org/abs/1702.05464) (CVPR2017)
    - 5分
    - 还是Adversarial Domain Adaptation的这一套框架，总结了这一套框架的三个设计点：
      - 1.从feature extractor可以是discriminative或者generative的。
      - 2.S和T的两个feature extractor可以是共享或者不共享参数的。不同于之前大部分工作共享参数，本文没有共享。
      - 3.adversarial objective可以是之前的MMD或者domain classification loss。本文从GAN中受到启发，采用所谓的GAN loss，也就是generator with the standard loss function with inverted labels。

- [Domain Separation Networks](https://arxiv.org/abs/1608.06019) (NIPS2016)
    - 4分
    - motivation:把features划分为domain-specific和domain-invariant两类。这和long-tail问题中分成class-specific和class-invariant两类的想法异曲同工。
    - 相较于之前只做domain-invariant的工作来说，相当于多提取了一个domain-specific的特征。注意最后分类的时候还是只用的domain-invariant的特征，也就是说domain-specific的特征只是为了帮助提取到更好的domain-invariant的特征。
    - similarity loss就采取之前的MMD或者domain classification loss。difference loss使得一个domain的两个特征趋于正交。

- [Deep Reconstruction-Classification Networks for Unsupervised Domain Adaptation](https://arxiv.org/abs/1607.03516) (ECCV2016)
    - 3分
    - 方法包括两点：1.supervised source label prediction and 2.unsupervised target data reconstruction。其中第二点就是迫使网络提取的feature能够重建T上的样本，也就相当于在feature上混入了T的信息。

- [Domain-Adversarial Neural Networks](https://arxiv.org/abs/1412.4446) (JMLR2016)
    - 5分
    - 受到Ben-David理论工作启发的直接应用。Theorem 2刻画了学习器在T上的泛化能力与以下几点相关：
      - 1.在S上的empirical risk。
      - 2.S与T的H-divergence。
      - 3.最优学习器在S和T上取得的risk之和。
    - 第3点刻画任务本身的难度，即是否存在一个学习器在S和T上都有较好的泛化能力。
    - 第1，2点是试图去优化的。即 a trade-off between the minimization of the source risk and the H-divergence。
    - H-divergence难以直接优化，因此采用辨别domain来源的替代任务进行估计。优化时存在一个gradient reversal layer，也就是minimize discriminator的L<sub>d</sub> loss使其能够鉴别domain的来源，并maximize feature extractor的L<sub>d</sub> loss使不同domain的feature难以分辨，这就体现了adversarial。

- [Learning Transferable Features with Deep Adaptation Networks](https://arxiv.org/abs/1502.02791) (ICML2015)
- [Deep Domain Confusion: Maximizing for Domain Invariance](https://arxiv.org/abs/1412.3474) (arXiv2014)
    - 5分
    - The maximization of the domain classification loss in [DANN](https://arxiv.org/abs/1412.4446) is replaced by the minimization of the Maximum Mean Discrepancy (MMD) metric. MMD度量了S和T上features分布的差异，最小化MMD相当于对两个domain上的features做了一个confusion。 


# OOD Generalization

> 包括Domain Generalization(通常关注multi-source training data的图片分类问题)，具体概念可以参考[关于OOD generalization的综述](https://arxiv.org/abs/2108.13624)。其与Domain Adaptation的区别在于训练时没有target domain的知识，因此是一个更难的任务。

- [Learning to Diversify for Single Domain Generalization](https://arxiv.org/abs/2108.11726) (ICCV2021)
- [Domain Generalization with MixStyle](https://arxiv.org/abs/2104.02008) (ICLR2021)
    - 3分
    - 重要假设：visual domain is closely related to image style。
    - 出发点在于，如果训练数据中本身就有充足的domain，那么ERM就可以提取到domain-invariant的特征，因此一个显而易见的做法就是domain augmentation。
    - 两篇文章的核心都是通过style transfer的方法（具体做法类似[AdaIN](https://arxiv.org/abs/1703.06868)）来做domain augmentation。

- [Deep Stable Learning for Out-Of-Distribution Generalization](https://arxiv.org/abs/2104.07876) (CVPR2021)
    - 3分
    - 还是基于reweight的stable learning的那一套，但希望扩展到DNN上。存在两个难点：
      - 如何在DNN的feature space上刻画non-linear dependencies？文中采用了RFF来刻画feature space两个维度之间的关联，并通过reweight来尽可能减少各个维度之间的关联，即所谓的learning sample weights for decorrelation。
      - 如何避免global reweight的高资源开销？文中采用了saving and reloading stratagy，类似于contrastive learning中的memory bank。

- [Self-Challenging Improves Cross-Domain Generalization](https://arxiv.org/abs/2007.02454) (ECCV2020_oral)
    - 4分
    - 根据loss对feature的梯度做了类似dropout的事情，扔掉梯度大的地方，强制模型去关注所有feature（问题：这样可能会扔掉前景的特征反而让模型更关注背景？个人感觉这个方法work的点更偏向于防止overfit到source domain上）。
    - 具体实现时即有spatial-wise又有channel-wise。

- [Stable Prediction across Unknown Environments](https://arxiv.org/abs/1806.06270) (KDD2018)
    - 4分
    - 什么是Stable Prediction？就是在不同domain上预测的error的均值和方差都低。
    - 重要的假设：首先将X分为stable features S和noisy features V，其中对于Y的预测只与S有关。且在不同的domain上都存在同样的P(y|s)来刻画S到Y的映射。在这样的假设下，我们只需要模型学得P(y|s)即可。
    - 文章的核心在于Eq(3)和Eq(4):
      - Eq(3)表示通过reweight来消除features与T的关联。
      - Eq(4)表示将特征中的每一个维度分别当作控制变量，reweight后希望尽可能消除其与其它维度之间的关联。这是因为直接消除S和V之间的关联是很难做到的，那么干脆decorrelate all features，从而使得最后模型最终学得的就是S到Y的映射。