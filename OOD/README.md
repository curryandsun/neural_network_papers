# Table of contents

- [Table of Contents](#table-of-contents)
- [OOD Detection](#OOD-detection)
- [Open-set Recognition](#Open-set-Recognition)
- [Anomaly Detection and Localization](#anomaly-detection-and-localization)



# OOD Detection
> Out-of-Distribution (OOD) detection(similiar with anomaly detection, novelty detection): it considers a multi-class dataset as In-Distribution (ID) and considers samples from another multi-class dataset as OOD. In OOD detection, we often pay more attention to detection than classification.

> Following terms also include anomaly detection and novelty detection(they are usually considered as the single-class case).

- [Semantically Coherent Out-of-Distribution Detection](https://arxiv.org/abs/2108.11941) (ICCV 2021)
    - 5分
    - 对于benchmark的讨论很有意思：指出之前的方法near-perfect的原因可能在于对于dataset low-level feature的overfit。
    - 方法需要用到ID与OOD混合的unlabel data,实验中直接将TinyImageNet作为unlabel data因为作者认为其本身就是混杂着ID和OOD样本的。
    - 方法比较简单，对unlabel data做筛选，ID的就当成label data训练，OOD的就用OE。

- [Bridging In- and Out-of-distribution Samples for Their Better Discriminability](https://arxiv.org/abs/2101.02500) (arXiv 2021)
    - 3分
    - motivation:use corrupted images as the intermediate of ID and OOD.
    - 对每种transformation定义一个soft label，而这个soft label对应于这种transformation后的在原ID网络中的acc，也就是说acc越低认为corrupt更严重，则更偏向于OOD。

- [SSD: A Unified Framework for Self-Supervised Outlier Detection](https://openreview.net/forum?id=v5gjXpmR8J) (ICLR 2021)
    - 4分
    - contrastive self-supervised学表示，再聚类。test时算马氏距离做OOD Detection。idea并没有什么novelty，与NIPS18的Mahalanobis方法很相似，只不过因为没有label信息，因此需要自监督+聚类来确定簇中心和协方差矩阵。
    - 对数据的access分的很清晰:如果可以用in-distribution的label?那就把contrastive变为supervised contrastive；如果可以用少量OOD data?引入所谓的few-shot OOD setting，其实就是将OOD data也考虑在距离的计算之内从而更为精准。

- [Detecting Out-of-Distribution Examples with Gram Matrices](http://proceedings.mlr.press/v119/sastry20a.html) (ICML 2020)
    - 4分
    - 所谓的Gram Matrices其实就是一个样本activation map(c*h*w)中channel-wise correlations(c*c),那么其实这个就可以表征训练集的一些特征，测试样本若与其bia较大则可以认为是OOD。
    - 这其实和KD里面用feature来蒸馏的做法很类似，能不能把KD那套搬到OOD里面来用?

- [CSI: Novelty Detection via Contrastive Learningon Distributionally Shifted Instances](https://arxiv.org/abs/2007.08176) (NIPS 2020)
    - 4分
    - contrastive学表示再打分的大框架。
    - 有意思的一个点是:In particular, we verify that the “hard” augmentations, thought to be harmful for contrastive representation learning, can be helpful for OOD detection.也就是将“hard” augmentations后的样本作为neg项加入contrastive loss的计算，这样学到的表示对classify没有提升，但对OOD有帮助。

- [Contrastive Training for Improved Out-of-Distribution Detection](https://arxiv.org/abs/2007.05566) (arXiv 2020)
    - 3分
    - supervised + contrastive学表示，test时计算与每个类的马氏距离。也就是利用contrastive生成更general的features,想法类似于[Generative-Discriminative Feature Representations for Open-Set Recognition](https://openaccess.thecvf.com/content_CVPR_2020/html/Perera_Generative-Discriminative_Feature_Representations_for_Open-Set_Recognition_CVPR_2020_paper.html)。

- [Generalized ODIN: Detecting Out-of-distribution Image without Learning from Out-of-distribution Data](https://arxiv.org/abs/2002.11297) (CVPR 2020)
    - 4分
    - motivation:在train以及valid过程中，应该保持OOD dataset是unseen状态。
    - 在之前的包括ODIN,Maha等方法中，其超参其实是对着测试集的OOD dataset调的，而这显然是错误且在现实应用中无法接受的。
    - 文中的核心贡献应该是对ODIN方法做了改进，使得其超参无需在OOD dataset中调得。文中大篇幅的概率方面的解释偏于讲故事。
    - 在DomainNet上做的实验非常有意思。

- [Unsupervised Out-of-Distribution Detection by Maximum Classifier Discrepancy](https://arxiv.org/abs/1908.04951) (ICCV 2019)
    - 3分
    - 很有意思的idea:OOD样本更集中于boundary附近，那么用两个不同的classifier，OOD样本显然会比ID样本有更大output difference，那么就可以用这一点来做OOD detection。
    - 需要用到unlabel的ID和OOD的混合dataset来做finetune，用来进一步Maximum Classifier Discrepancy。这里其实是如果是只有OOD的dataset理论上是更好的，因此其实并没有发挥混合dataset中ID样本的作用。
    - 实验的setting其实是transductive的，但文中并没有明确表明这点，这点很有问题。

- [Deep Anomaly Detection with Outlier Exposure](https://arxiv.org/abs/1812.04606) (ICLR 2019)
    - 4分
    - motivation:用OOD dataset对训练好的nn做finetune(如使得OOD data的输出分布更接近均匀分布),期望其泛化到test时的OOD data中。也就是说该方法needs access to some OOD datasets。

- [A Simple Unified Framework for Detecting Out-of-Distribution Samples and Adversarial Attacks](https://arxiv.org/abs/1807.03888) (NIPS2018)
    - 5分
    - supervised学表示，再把每个类的features看作是高斯分布，test时计算马式距离。
    - why Mahalanobis distance? 
        - 马氏距离相比于欧氏距离考虑到各种属性之间的联系，即消除了一些属性相关性，更重要的是实验效果都比欧氏距离好。
    - 本质上就是把softmax分类换成了LDA，为什么会好呢？个人感觉对着OOD调超参起到了很大作用。LDA甚至需要满足特征服从高斯分布这样的假设，也就是说如果前面的表示学习做的没那么好，马氏距离也就不会这么好用了。
    - 这里也就有一个有意思的点：表示做好了，OOD其实也就不难了，也就没什么好做的了。但其实现在大部分OOD工作就是在toy数据集上做着学好表示后的事情。什么样的OOD场景是难的且该去做的其实是值得思考的问题。

- [Training Confidence-calibrated Classifiers for Detecting Out-of-Distribution Samples](https://arxiv.org/abs/1711.09325) (ICLR2018)
    - 4分
    - 利用GAN来生成靠近boundary的样本当作OOD样本，约束网络在OOD样本上输出均匀分布。和[OE](https://arxiv.org/abs/1812.04606)方法很类似，只不过这里用GAN来生成而不是直接利用OOD dataset。

- [Enhancing The Reliability of Out-of-distribution Image Detection in Neural Networks](https://arxiv.org/abs/1706.02690) (ICLR2018)
    - 5分
    - 1.On in-distribution images, modern neural networks tend to produce outputs with larger variance across class labels.
    - 2.neural networks have larger norm of gradient of log-softmax scores when applied on in-distribution images.
    - 根据上述两个观察，提出了用temperature scaling和类似FGSM的input preprocessing来进一步拉大ID和OOD样本输出概率分布的差别，再用max-softmax方法来判定。该方法记为ODIN。
    - 文中的OOD指标相比于baseline更加清晰。

- [Deep One-Class Classification](http://proceedings.mlr.press/v80/ruff18a.html) (ICML2018)
    - 4分
    - 传统的kernel-based one-class方法有OCSVM以及SVDD，其中OCSVM希望将映射后的高维特征与原点更好的区分开来，也就是找到一个良好的half-space；而SVDD是希望映射后的高维特征分布在半径很小的超球内。
    - 而本文的思路就是将SVDD中的kernel映射改为用cnn学得的表示，再用类似SVDD的优化目标端到端地优化。

- [A Baseline for Detecting Misclassified and Out-of-Distribution Examples in Neural Networks](https://arxiv.org/abs/1610.02136) (ICLR2017)
    - 4分
    - out-of-distribution的检测是一个不平衡二分类问题，需要用ROC，PR等指标衡量。
    - OOD检测最直接的思路就是取softmax后最大的概率值作为判断标准，小于阈值则认为是OOD。文中用该方法建立了一系列baseline。
    - 观察发现OOD样本通常也有较大的confidence，因此这样的方法势必有局限性。

# Open-Set Recognition

> Open-Set Recognition(OSR): some classes of a dataset as in-distribution and some other classes as a source of outliers. The classifier should do well in both classfication and detection.

- [Generative-Discriminative Feature Representations for Open-Set Recognition](https://openaccess.thecvf.com/content_CVPR_2020/html/Perera_Generative-Discriminative_Feature_Representations_for_Open-Set_Recognition_CVPR_2020_paper.html) (CVPR2020)
    - 3分
    - motivation:auto-encoder + rotation loss来提高instance-wise的表示。

- [Learning Open Set Network with Discriminative Reciprocal Points](https://arxiv.org/abs/2011.00178) (ECCV2020_spotlight)
    - 4分
    - motivation:一般nn后面接的线性层每个类都有一个模板向量来代表类的表示。而这篇文章则反之，用Reciprocal Points来代表不属于此类的表示。不属于某类的包括other classes和unknown classes，那么这种方法相比于常规做法就能对unknown classes做更好的建模。
    - loss中的两项正好表示两个目标:
        - 1.分类：距离类Reciprocal Points最远才表示属于此类。
        - 2.保证Reciprocal Points是非此类的表示：类内样本到Reciprocal Points的距离要足够远。

- [Classification-Reconstruction Learning for Open-Set Recognition](https://arxiv.org/abs/1812.04246) (CVPR2019)
    - 3分
    - motivation:认为openmax方法中利用logits来表征某个样本其实是不利于unknown detection的(class-wise)。
    - 因此用类似ladder nets的网络结构 + reconstruction loss来计算另一个中间表示z(instance-wise,可以用self-supervised?)，结合logits与z再用openmax。

- [Towards Open Set Deep Networks](https://arxiv.org/abs/1511.06233) (CVPR2016)
    - 5分
    - 将softmax改造成多出一个unknown类的openmax。
    - Assume that d of the inliers follows a  Weibull distribution：首先用每个类trainning data的logits(av)到类中心的距离去fit c个Weibull分布，测试时计算样本logits属于每个类对应分布的class-belongingness,再据此调整最后的输出概率分布。


# Anomaly Detection and Localization

> 不仅需要检测出异常图片，还需要定位异常的位置，通常使用[MVTec AD](https://openaccess.thecvf.com/content_CVPR_2019/html/Bergmann_MVTec_AD_--_A_Comprehensive_Real-World_Dataset_for_Unsupervised_Anomaly_CVPR_2019_paper.html)数据集。

- [DRAEM -- A discriminatively trained reconstruction embedding for surface anomaly detection](https://arxiv.org/abs/2108.07610) (ICCV2021)
    - 4分
    - simulated anomaly generation的算法比较高明。
    - 想法有点类似GAN：生成器负责把异常图片恢复为正常图片，判别器负责找到异常图片与恢复图片的异常区域，这样在测试的时候就可以直接输出segmentation的结果。

- [CutPaste: Self-Supervised Learning for Anomaly Detection and Localization](https://arxiv.org/abs/2104.04015) (CVPR2021)
    - 3分
    - CutPaste即crop图片随机一块在粘贴到随机位置，再将这样操作后的图片认为是异常图片，就变成了二分类self-supervised问题。

- [PaDiM: a Patch Distribution Modeling Framework for Anomaly Detection and Localization](https://arxiv.org/abs/2011.08785) (ICPR2021)
- [Sub-Image Anomaly Detection with Deep Pyramid Correspondences](https://arxiv.org/abs/2005.02357) (arXiv2020)
- [Deep Nearest Neighbor Anomaly Detection](https://arxiv.org/abs/2002.10445) (arXiv2020)
    - 4分
    - 方法无需训练，只需要在ImageNet上pretrain的CNN。
    - 后续的方法有image-level的，也有patch-level的；有基于KNN的，有基于混合高斯的。但本质思路就是建模normal image特征的distribution，测试时再与之进行对比。属于baseline方法，但效果出奇地好。

- [Explainable Deep One-Class Classification](https://arxiv.org/abs/2007.01760) (ICLR2021)
    - 3分
    - 本质就是二分类情况下的[CAM](https://arxiv.org/abs/1512.04150)。所谓的upsample也并不重要，在CAM原paper中是一笔带过的。

- [Patch SVDD: Patch-level SVDD for Anomaly Detection and Segmentation](https://arxiv.org/abs/2006.16067) (ACCV2020)
    - 3分
    - SVDD要求所有样本映射到一个中心点C附近，而Patch SVDD则要求相邻patch的特征相似。
