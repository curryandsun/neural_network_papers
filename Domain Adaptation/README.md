# Table of Contents

- [Table of Contents](#table-of-contents)
- [Domain Adaptation](#domain-adaptation)

# Domain Adaptation

> 领域自适应。以下以S表示source domain，T表示target domain。

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

