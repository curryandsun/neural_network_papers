# Table of Contents

- [Table of Contents](#table-of-contents)
- [Domain Adaptation](#domain-adaptation)

# Domain Adaptation

> 领域自适应。以下以S表示source domain，T表示target domain。

- [Domain Separation Networks](https://arxiv.org/abs/1608.06019) (NIPS2016)
    - 5分
    - motivation:把features划分为domain-specific和domain-invariant两类。这和long-tail问题中分成class-specific和class-invariant两类的想法异曲同工。
    - 相较于之前只做domain-invariant的工作来说，相当于多提取了一个domain-specific的特征。注意最后分类的时候还是只用的domain-invariant的特征，也就是说domain-specific的特征只是为了帮助提取到更好的domain-invariant的特征。
    - similarity loss就采取之前的MMD或者domain classification loss。difference loss使得一个domain的两个特征趋于正交。

- [Deep Reconstruction-Classification Networks for Unsupervised Domain Adaptation](https://arxiv.org/abs/1607.03516) (ECCV2016)
    - 4分
    - 方法包括两点：1.supervised source label prediction and 2.unsupervised target data reconstruction。其中第二点就是迫使网络提取的feature能够重建T上的样本，也就相当于在feature上混入了T的信息。

- [Domain-Adversarial Neural Networks](https://arxiv.org/abs/1412.4446) (JMLR2016)
    - 5分
    - 受到Ben-David理论工作启发的直接应用。Theorem 2刻画了学习器在T上的泛化能力与以下几点相关：
      - 1.在S上的empirical risk。
      - 2.S与T的H-divergence。
      - 3.最优学习器在S和T上取得的risk之和。
    - 第3点刻画任务本身的难度，即是否存在一个学习器在S和T上都有较好的泛化能力。
    - 第1，2点是试图去优化的。即 a trade-off between the minimization of the source risk and the H-divergence。
    - H-divergence难以直接优化，因此采用辨别domain来源的替代任务进行估计。优化时最大化这个辨别任务的loss，即辨别的error越大，则越难分辨S和T，则两者的H-divergence越小。这一优化目标可以看成是find domain-invariant representations。

- [Learning Transferable Features with Deep Adaptation Networks](hhttps://arxiv.org/abs/1502.02791) (ICML2015)
- [Deep Domain Confusion: Maximizing for Domain Invariance](https://arxiv.org/abs/1412.3474) (arXiv2014)
    - 5分
    - The maximization of the domain classification loss in [DANN](https://arxiv.org/abs/1412.4446) is replaced by the minimization of the Maximum Mean Discrepancy (MMD) metric. MMD度量了S和T上features分布的差异，最小化MMD相当于对两个domain上的features做了一个confusion。 

