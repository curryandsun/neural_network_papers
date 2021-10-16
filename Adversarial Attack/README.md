# Table of Contents

- [Table of Contents](#table-of-contents)
- [Adversarial](#adversarial)

# Adversarial

> Adversarial attack and defense.

- [Adversarial Examples Improve Image Recognition](https://arxiv.org/abs/1911.09665) (CVPR2020)
    - 4分
    - motivation:之前的adversarial training在大数据集上虽然可以提高鲁棒性，但都会降低在clean data上的准确率，因此作者希望通过adversarial样本来提高clean data准确率。
    - 文中认为之前adversarial training的问题出在distribution  mismatch，即对抗样本和真实样本的分布是不一致的。因此可以多加一个辅助的BN，使得二者通过不同的BN以免混淆分布。

- [Explaining and Harnessing Adversarial Examples](https://arxiv.org/abs/1412.6572) (ICLR2015)
    - 5分
    - 经典的FGSM方法。
    - 从线性模型出发，一个原始样本perturbation η如果满足无穷范数小于其图像分辨率，那么分类器理应无法分出区别。但是经过W^T*η之后，却会因为高维度而将perturbation放大而导致误分。
    - 最终的FGSM方法中，求梯度就是linearize the cost function around the current value of θ，取sign是为了obtaining an optimal max-norm constrained pertubation。


