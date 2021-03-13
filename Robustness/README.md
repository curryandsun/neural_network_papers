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
- [OOD Detection](#OOD-detection)
- [Open-set Recognition](#Open-set-Recognition)
- [Adversarial](#adversarial)
- [Noisy Label](#noisy-label)


# OOD Detection
> Out-of-distribution (OOD) Detection:it considers a multi-class dataset as in-distribution and samples outliers from another multi-class dataset(from different domains). 

- [SSD: A Unified Framework for Self-Supervised Outlier Detection](https://openreview.net/forum?id=v5gjXpmR8J) (ICLR 2021)
    - 3分
    - contrastive self-supervised学表示，再聚类。test时算马氏距离做OOD Detection。idea并没有什么novelty，与NIPS18的Mahalanobis方法很相似，只不过因为没有label信息，因此需要自监督+聚类来确定簇中心和协方差矩阵。
    - 对数据的access分的很清晰:如果可以用in-distribution的label?那就把contrastive变为supervised contrastive；如果可以用少量OOD data?引入所谓的few-shot OOD setting，其实就是将OOD data也考虑在距离的计算之内从而更为精准。
    - related work很清晰。

- [CSI: Novelty Detection via Contrastive Learningon Distributionally Shifted Instances](https://arxiv.org/abs/2007.08176) (NIPS 2020)
    - 3分
    - contrastive学表示再打分的大框架。
    - 有意思的一个点是:In particular, we verify that the “hard” augmentations, thought to be harmful for contrastive representation learning, can be helpful for OOD detection.也就是将“hard” augmentations后的样本作为neg项加入contrastive loss的计算，这样学到的表示对classify没有提升，但对OOD有帮助。

- [Contrastive Training for Improved Out-of-Distribution Detection](https://arxiv.org/abs/2007.05566) (arXiv 2020)
    - 3分
    - contrastive学表示，test时计算与每个类的马氏距离。也就是说前面是无监督的，后面却需要label，这点很奇怪，[SSD](https://openreview.net/forum?id=v5gjXpmR8J)方法加了一步聚类显得自然很多。

- [Generalized ODIN: Detecting Out-of-distribution Image without Learning from Out-of-distribution Data](https://arxiv.org/abs/2002.11297) (CVPR 2020)
    - 4分
    - motivation:在train以及valid过程中，应该保持OOD dataset是unseen状态。
    - 在之前的包括baseline，ODIN等方法中，其超参其实是对着测试集的OOD dataset调的，而这显然不符合现实setting，也无法迁移到别的OOD dataset之上。
    - 文中的核心贡献应该是对ODIN方法做了改进，使得其超参无需在OOD dataset中调得。文中大篇幅的概率方面的解释偏于讲故事。

- [Unsupervised Out-of-Distribution Detection by Maximum Classifier Discrepancy](https://arxiv.org/abs/1908.04951) (ICCV 2019)
    - 4分
    - 很有意思的idea:OOD样本更集中于boundary附近，那么用两个不同的classifier，OOD样本显然会比ID样本有更大output difference，那么就可以用这一点来做OOD detection。
    - 需要用到unlabel的ID和OOD的混合dataset来做finetune，用来进一步Maximum Classifier Discrepancy，这点和[OE](https://arxiv.org/abs/1812.04606)方法很类似。

- [Deep Anomaly Detection with Outlier Exposure](https://arxiv.org/abs/1812.04606) (ICLR 2019)
    - 4分
    - motivation:用OOD dataset对训练好的nn做finetune(如使得OOD data的输出分布更接近均匀分布),期望其泛化到test时的OOD data中。也就是说该方法needs access to some OOD datasets。

- [A Simple Unified Framework for Detecting Out-of-Distribution Samples and Adversarial Attacks](https://arxiv.org/abs/1807.03888) (NIPS2018)
    - 5分
    - supervised学表示，再把每个类的features看作是高斯分布，test时计算马式距离。
    - why Mahalanobis distance? 
        - 1.abnormal samples can be characterized better in the representation space of DNNs, rather than the “label-overfitted” output space of softmax-based posterior distribution used in the prior works for detecting them. 
        - 2.马氏距离相比于欧氏距离考虑到各种属性之间的联系，即消除了一些属性相关性，更重要的是实验效果都比欧氏距离好。
    - 两个trick:1.与ODIN类似的input preprocessing。2.将浅层features也考虑进来。

- [Learning Confidence for Out-of-Distribution Detection in Neural Networks](https://arxiv.org/abs/1802.04865) (arXiv2018)
    - 3分
    - motivation:让nn的输出多一个分支来实现直接输出置信度c。
    - 文中很多方法(如加一个c相关的Loss,限制c的penalty数目)最终目的仍然是为了输出的c更接近真实的置信度。
    - 方法非常直观，但效果存疑。

- [Enhancing The Reliability of Out-of-distribution Image Detection in Neural Networks](https://arxiv.org/abs/1706.02690) (ICLR2018)
    - 5分
    - 1.On in-distribution images, modern neural networks tend to produce outputs with larger variance across class labels.
    - 2.neural networks have larger norm of gradient of log-softmax scores when applied on in-distribution images.
    - 根据上述两个观察，提出了用temperature scaling和类似FGSM的input preprocessing来进一步拉大ID和OOD样本输出概率分布的差别，再用max-softmax方法来判定。该方法记为ODIN。
    - 文中的OOD指标相比于baseline更加清晰。

- [A Baseline for Detecting Misclassified and Out-of-Distribution Examples in Neural Networks](https://arxiv.org/abs/1610.02136) (ICLR2017)
    - 4分
    - out-of-distribution的检测是一个不平衡二分类问题，需要用ROC，PR等指标衡量。
    - OOD检测最直接的思路就是取softmax后最大的概率值作为判断标准，小于阈值则认为是OOD。文中用该方法建立了一系列baseline。
    - 观察发现OOD样本通常也有较大的confidence，因此这样的方法势必有局限性。

# Open-Set Recognition

> Open-Set Recognition(OSR):some classes of a dataset as in-distribution and some other classes as a source of outliers(from a same domain).The classifier should do well in both classfication and detection.

- [Learning Open Set Network with Discriminative Reciprocal Points](https://arxiv.org/abs/2011.00178) (ECCV2020_spotlight)
    - 4分
    - motivation:一般nn后面接的线性层每个类都有一个模板向量来代表类的表示。而这篇文章则反之，用Reciprocal Points来代表不属于此类的表示。不属于某类的包括other classes和unknown classes，那么这种方法相比于常规做法就能对unknown classes做更好的建模。
    - loss中的两项正好表示两个目标:
        - 1.分类：距离类Reciprocal Points最远才表示属于此类。
        - 2.保证Reciprocal Points是非此类的表示：类内样本到Reciprocal Points的距离要足够远。

# Adversarial

> Adversarial attack and defense

- [Adversarial Examples Improve Image Recognition](https://arxiv.org/abs/1911.09665) (CVPR2020)
    - 4分
    - motivation:之前的adversarial training在大数据集上虽然可以提高鲁棒性，但都会降低在clean data上的准确率，因此作者希望通过adversarial样本来提高clean data准确率。
    - 文中认为之前adversarial training的问题出在distribution  mismatch，即对抗样本和真实样本的分布是不一致的。因此可以多加一个辅助的BN，使得二者通过不同的BN以免混淆分布。

- [Explaining and Harnessing Adversarial Examples](https://arxiv.org/abs/1412.6572) (ICLR2015)
    - 4分
    - 经典的FGSM方法。
    - 从线性模型出发，一个原始样本perturbation η如果满足无穷范数小于其图像分辨率，那么分类器理应无法分出区别。但是经过W^T*η之后，却会因为高维度而将perturbation放大而导致误分。
    - 最终的FGSM方法中，求梯度就是linearize the cost function around the current value of θ，取sign是为了obtaining an optimal max-norm constrained pertubation。



# Noisy Label

> 标签噪声。

- [Early-Learning Regularization Prevents Memorization of Noisy Labels](https://arxiv.org/abs/2007.00151) (NIPS2020)
    - 5分
    - motivation:DNN在noisy label的学习过程中有’early learning‘的现象:first use patterns, not brute force memorization, to fit real data，then memorize noisy label。因此，将早期模型的输出作为后期的一个正则化，有点像self-KD。
    - 在KD中，拟合target用的是KL散度，但文中用的是内积。原因是KL散度相比内积对target的拟合程度更高，是想利用到dark knowledge，对本文来说会过拟合。
    - 从梯度的角度说明了两点: (1) ensure that the contribution to the gradient from examples with clean labels remains large, and (2) neutralize the influence of the examples with wrong labels on the gradient.
    - 文章的图画的是真好，看图就非常容易明白作者的idea。