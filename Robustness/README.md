# 论文阅读笔记

**记录一些读过的论文，给出个人对论文的评分情况并简述论文insight**

**评分细节：** 

- 5分：值得反复阅读的论文，非常具有启发性

- 4分：值得精读的论文，具有一定的启发性

- 3分：值得粗略阅读的论文，了解论文大概思想后会有一定收获

- 2分：价值不大的论文，读完基本没有收获

- 1分：粗制滥造的论文

> 论文将按照年份/评分依次排序，绝大多数论文将会给出[arXiv](https://arxiv.org/)的链接.


# Table of contents

- [论文阅读笔记](#论文阅读笔记)
- [Table of Contents](#table-of-contents)
- [OoD Detection](#OoD-detection)
- [Adversarial](#adversarial)
- [Noisy label](#noisy-label)


# OoD Detection

> out-of-distribution detection,类似的概念有open-set recognition,novelty detection,anomaly detection等。

- [Generalized ODIN: Detecting Out-of-distribution Image without Learning from Out-of-distribution Data](https://arxiv.org/abs/2002.11297) (CVPR 2020)
    - 4分
    - motivation:在train以及valid过程中，应该保持OoD dataset是unseen状态。
    - 在之前的包括baseline，ODIN等方法中，其超参其实是对着测试集的OoD dataset调的，而这显然不符合现实setting，也无法迁移到别的OoD dataset之上。
    - 文中的核心贡献应该是对ODIN方法做了改进，使得其超参无需在OoD dataset中调得。文中大篇幅的概率方面的解释无法令我认同。

- [Deep Anomaly Detection with Outlier Exposure](https://arxiv.org/abs/1812.04606) (ICLR 2019)
    - 4分
    - motivation:用OoD dataset对训练好的nn做finetune(如使得OoD data的输出分布更接近均匀分布),期望其泛化到test时的OoD data中。
    - 如Baseline以及ODIN方法其实都没有涉及到对原有网络的重新训练，因此无需考虑in-distribution data的分类准确率。而OE方法其实是在其他方法的基础上再对网络进行finetune，是会影响分类准确率的，但文中只讨论了detection这一方面。

- [Learning Confidence for Out-of-Distribution Detection in Neural Networks](https://arxiv.org/abs/1802.04865) (arXiv2018)
    - 3分
    - motivation:让nn的输出多一个分支来实现直接输出置信度c。
    - 文中很多方法(如加一个c相关的Loss,限制c的penalty数目)最终目的仍然是为了输出的c更接近真实的置信度。
    - 方法非常直观，但效果存疑。

- [Enhancing The Reliability of Out-of-distribution Image Detection in Neural Networks](https://arxiv.org/abs/1706.02690) (ICLR2018)
    - 5分
    - 1.On in-distribution images, modern neural networks tend to produce outputs with larger variance across class labels.
    - 2.neural networks have larger norm of gradient of log-softmax scores when applied on in-distribution images.
    - 根据上述两个观察，提出了用temperature scaling和类似FGSM的input preprocessing来进一步拉大ID和OoD样本输出概率分布的差别，再用max-softmax方法来判定。该方法记为ODIN。
    - 文中的OoD指标相比于baseline更加清晰。

- [A Baseline for Detecting Misclassified and Out-of-Distribution Examples in Neural Networks](https://arxiv.org/abs/1610.02136) (ICLR2017)
    - 4分
    - OoD检测需要既保证in-distribution的分类准确率，又保证out-of-distribution的检测率。而后者是一个不平衡二分类问题，需要用ROC，PR等指标衡量。
    - OoD检测最直接的思路就是取softmax后最大的概率值作为判断标准，小于阈值则认为是OoD。文中用该方法建立了一系列baseline。
    - 观察发现OoD样本通常也有较大的confidence，因此这样的方法势必有局限性。

# Adversarial

> Adversarial attack and defense

- [Adversarial Examples Improve Image Recognition](https://arxiv.org/abs/1911.09665) (CVPR2020)
    - 4分
    - motivation:之前的adversarial training在大数据集上虽然可以提高鲁棒性，但都会降低在clean data上的准确率，因此作者希望通过adversarial样本来提高clean data准确率。
    - 文中认为之前adversarial training的问题出在distribution  mismatch，即对抗样本和真实样本的分布是不一致的。因此可以多加一个辅助的BN，使得二者通过不同的BN以免混淆分布。

# Noisy Label

> 标签噪声。

- [Early-Learning Regularization Prevents Memorization of Noisy Labels](https://arxiv.org/abs/2007.00151) (NIPS2020)
    - 5分
    - motivation:DNN在noisy label的学习过程中有’early learning‘的现象:first use patterns, not brute force memorization, to fit real data，then memorize noisy label。因此，将早期模型的输出作为后期的一个正则化，有点像self-KD。
    - 在KD中，拟合target用的是KL散度，但文中用的是内积。原因是KL散度相比内积对target的拟合程度更高，是想利用到dark knowledge，对本文来说会过拟合。
    - 从梯度的角度说明了两点: (1) ensure that the contribution to the gradient from examples with clean labels remains large, and (2) neutralize the influence of the examples with wrong labels on the gradient.
    - 文章的图画的是真好，看图就非常容易明白作者的idea。