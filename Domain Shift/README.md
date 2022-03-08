# Table of Contents

- [Table of Contents](#table-of-contents)
- [Domain Adaptation](#domain-adaptation)
  - [Unsupervised Domain Adaptation](#unsupervised-domain-adaptation)
  - [Open-set Domain Adaptation](#open-set-domain-adaptation)
- [Domain Generalization](#domain-generalization)

# Domain Adaptation

> 领域自适应。以下以S表示source domain，T表示target domain。

## Unsupervised Domain Adaptation

> target domain数据没有label，最主要的DA场景。

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

## Open-set Domain Adaptation

> 在DA中考虑open-set的类别，包括open-set DA和universal DA。

- [Open Set Domain Adaptation](http://openaccess.thecvf.com/content_ICCV_2017/papers/Busto_Open_Set_Domain_ICCV_2017_paper.pdf) (ICCV2019)
    - 5分
    - 新setting：考虑DA中S和T都存在unknown class，且S和T中的unknown class互相不重叠。在测试时，将所有unknown class考虑为同一个类，即需要进行K+1的分类任务，此时考虑open-set acc即可。为了和closed-set进行对比，也考虑closed-set acc。
    - 这个问题的难点在于如何让DA中的feature alignment只在shared class上做，而消除unknown class的负面影响。
    - 方法核心类似于self-training，分为两步迭代进行：
      - 根据T与S上的feature的距离，给T上的样本打pseudo label（包括unknown）。
      - 根据pseudo label，将S的feature做映射，使得S与T上相同类（不包括unknown）的feature尽可能接近。这一步相当于DA中经典的feature alignment，且只在shared class中的相同类上做，忽略了unknown class。


# Domain Generalization

> Domain Generalization通常关注multi-source training data的图片分类问题，类似的概念有[OOD generalization](https://arxiv.org/abs/2108.13624)。DG与DA的区别在于前者训练时target domain是未知，因此是一个更难但更泛化的任务。其中DG又分为有或没有domain-label的两种setting，现在的研究更关注后者。

- [A Style and Semantic Memory Mechanism for Domain Generalization](https://arxiv.org/abs/2112.07517) (ICCV2021)
    - 3分
    - 如果想把contrastive learning用到DG中该怎么用？这篇文章给了一个很好的范式。
    - 总体来说，就是先类似[DSN](https://arxiv.org/abs/1608.06019)一样将feature分解为正交的semantic feature和domain feature两部分，再对两者分别做类似MoCo的contrastive learning。

- [Learning to Diversify for Single Domain Generalization](https://arxiv.org/abs/2108.11726) (ICCV2021)
- [Domain Generalization with MixStyle](https://arxiv.org/abs/2104.02008) (ICLR2021)
- [Domain Generalization Using a Mixture of Multiple Latent Domains](https://arxiv.org/abs/1911.07661v1) (AAAI2020)
    - 4分
    - 三篇文章共同的重要假设：visual domain is closely related to image style。
    - 前两篇文章通过style transfer的方法（具体做法类似[AdaIN](https://arxiv.org/abs/1703.06868)）来做domain augmentation。
    - 第三篇文章通过style cluster来确定domain-label，再用传统的adversarial框架。

- [Deep Stable Learning for Out-Of-Distribution Generalization](https://arxiv.org/abs/2104.07876) (CVPR2021)
    - 3分
    - 还是基于reweight的stable learning的那一套，但希望扩展到DNN上。存在两个难点：
      - 如何在DNN的feature space上刻画non-linear dependencies？文中采用了RFF来刻画feature space两个维度之间的关联，并通过reweight来尽可能减少各个维度之间的关联，即所谓的learning sample weights for decorrelation。
      - 如何避免global reweight的高资源开销？文中采用了saving and reloading stratagy，类似于contrastive learning中的memory bank。

- [DecAug: Out-of-Distribution Generalization via Decomposed Feature Representation and Semantic Augmentation](https://arxiv.org/abs/2012.09382) (AAAI2021)
    - 4分
    - 对数据集domain shift程度划分出两个标准，比较有趣：
      - Correlation：即domain与semantic label的相关性，相关性越强，也就越难学到domain-irrelevant的特征，任务也就越难。
      - Diversity：即存在的domain的多样性。
    - 不同于[DSN](https://arxiv.org/abs/1608.06019)中直接优化两个feature正交，文中的优化classification loss和domain loss对feature的两个梯度正交。这样确保了feature对两个loss的贡献方向是正交的，也就可以通过文中的两个branch把这正交的两部分分别提取出来作为semantic feature以及domain feature。

- [Self-Challenging Improves Cross-Domain Generalization](https://arxiv.org/abs/2007.02454) (ECCV2020_oral)
    - 4分
    - 根据loss对feature的梯度做了类似dropout的事情，扔掉梯度大的地方，强制模型去关注所有feature（问题：这样可能会扔掉前景的特征反而让模型更关注背景？个人感觉这个方法work的点更偏向于防止overfit到source domain上）。
    - 具体实现时即有spatial-wise又有channel-wise。

- [Domain Generalization by Solving Jigsaw Puzzles](https://arxiv.org/abs/1903.06864) (CVPR2019)
    - 3分
    - Jigsaw Puzzles这样的self-supervised signals有利于DG。那么rotate或者contrastive learning有用吗？
    - 这个做法没有用到training data存在多个domain的这一点。

- [Stable Prediction across Unknown Environments](https://arxiv.org/abs/1806.06270) (KDD2018)
    - 4分
    - 什么是Stable Prediction？就是在不同domain上预测的error的均值和方差都低。
    - 重要的假设：首先将X分为stable features S和noisy features V，其中对于Y的预测只与S有关。且在不同的domain上都存在同样的P(y|s)来刻画S到Y的映射。在这样的假设下，我们只需要模型学得P(y|s)即可。
    - 文章的核心在于Eq(3)和Eq(4):
      - Eq(3)表示通过reweight来消除features与T的关联。
      - Eq(4)表示将特征中的每一个维度分别当作控制变量，reweight后希望尽可能消除其与其它维度之间的关联。这是因为直接消除S和V之间的关联是很难做到的，那么干脆decorrelate all features，从而使得最后模型最终学得的就是S到Y的映射。