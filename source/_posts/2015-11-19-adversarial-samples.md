---
title: "对抗样本和对抗网络"
date: 2015-11-19 18:07:03
tags: [GANs, ML, DL]
categories: [tech]
layout: single-column
---

所谓对抗 样本是指将实际样本略加扰动而构造出的合成样本，对该样本，分类器非常容易将其类别判错，这意味着光滑性假设(*相似的样本应该以很高的概率被判为同一类别*)某种程度上被推翻了。<!--more-->

[Intriguing properties of neural networks, by Christian Szegedy at Google, et al，2014](http://arxiv.org/pdf/1312.6199.pdf). 这篇论文应该是最早提出对抗样本概念的。论文指出，包括卷积神经网络在内的深度学习模型在对抗样本面前都十分脆弱，从而将矛头直指深度学习，似乎要为深度学习热潮降一降温。

[*Deep Neural Networks are Easily Fooled: High Confidence Predictions for Unrecognizable Images*, by Nguyen A, et al, CVPR 15](http://arxiv.org/pdf/1412.1897.pdf)一文更是发现，面对一些人类完全无法识别的样本（论文中称为 **Fooling Examples**），深度学习模型居然会以高置信度将它们进行分类，例如将噪声识别为狮子。这意味着这些模型很容易被愚弄，例如垃圾邮件发送者可能利用对抗样本来骗过垃圾邮件识别系统，目下依赖人脸验证的人脸支付等新潮科技也将面临巨大风险。当然了，只要谷歌不公开他们的垃圾邮件过滤模型的架构，骗子们似乎也无法针对它大规模制造对抗样本，所以我们不用过分紧张，把模型藏好就可以了。然而，可怕的是，在不同训练子集上用不同的架构学习的不同的模型，居然能够被同一套对抗样本愚弄！这意味着骗子们可以自己训练出一个模型，并在此模型上找到一组对抗样本之后就能通吃其他模型了，想想还真是可怕。

那么，对抗样本只是我们的模型照顾不到的一小部分法外之地吗？论文 [Exploring the Space of Adversarial Images(ICLR 2016, under review)](http://arxiv.org/abs/1510.05328)(代码在[这里](https://github.com/tabacof/adversarial))给出了否定的答案，至少在图像空间中，对抗图片绝非孤立点，而是占据了很大一部分空间！

这些研究的提出，一方面促使人们更深入思考机器和人的视觉的真正差异所在，一方面，加上深度模型本身具有的不可解释性缺陷，也让一些人开始认为深度学习不是*deep learning*, 而是*deep flaw*.对深度学习来说，这多少是不公平的指责，因为 kdnuggets上的一篇文章[(Deep Learning’s Deep Flaws)’s Deep Flaws, by Zachary Chase Lipton](http://www.kdnuggets.com/2015/01/deep-learning-flaws-universal-machine-learning.html)指出，深度学习对于对抗样本的脆弱性并不是深度学习所独有的，事实上，这在很多机器学习模型中都普遍存在（Box 大人不就说吗，*all models are wrong, but some are useful*），而深度学习反而可能是目前为止对对抗训练最有抵抗性的技术。这篇文章吸引了包括 Bengio 大牛、Ian Goodfellow 等在内的许多人的热烈讨论，Ian Goodfellow（八卦一下，Ian Goodfellow 是 Bengio 的高徒，目前任职于谷歌，是对抗网络方面的大牛）更是结合这些评论和自己的工作，写了另外一篇文章： [Deep Learning Adversarial Examples – Clarifying Misconceptions](http://www.kdnuggets.com/2015/07/deep-learning-adversarial-examples-misconceptions.html)，中文翻译在此：[深度学习对抗样本的八个误解与事实](http://m.csdn.net/article_pt.html?arcid=2825248)。


那么，导致深度模型对反抗样本力不从心的真实原因有哪些呢？一般我们知道，可能是模型过拟合导致泛化能力不够，泛化能力不够可能是由于模型均化不足或者正则不足，然而，通过更多模型均化和加入更多噪声训练等方式来应对对抗样本的企图均告失败。另外一个猜测是模型的高度非线性，深度模型动辄千百万的参数个数确实让人有点不太舒服（冯诺依曼不就说吗，给他四个参数，他就能模拟大象，五个参数，大象就能摇尾巴了（还真有人写论文探讨如何模拟大象，参见[Drawing an elephant with four complext parameters](https://publications.mpi-cbg.de/Mayer_2010_4314.pdf)）），但 Ian Goodfellow 在论文 [explaining and harnessing adversarial examples, ICLR 2015](http://arxiv.org/pdf/1412.6572.pdf) 中，通过在一个线性模型中加入对抗干扰，发现只要线性模型的输入拥有足够的维度（事实上大部分情况下，模型输入的维度都比较大，因为维度过小的输入会导致模型的准确率过低，即欠拟合），线性模型也对对抗样本表现出明显的脆弱性，这驳斥了关于对抗样本是因为模型的高度非线性的解释。事实上，该文指出，高维空间中的线性性就足以造成对抗样本，深度模型对对抗样本的无力最主要的还是由于其线性部分的存在（*primary cause of neural networks’ vulnerability to adversarial perturbation is their linear nature*）。

毫无疑问，对抗样本带来了对深度学习的质疑（如果能借此灭一灭深度学习的虚火，也是好的，现在相关领域的论文，*无深度，不论文*，灌水的太多，随便改一改模型的架构，调一调参数就能发表，结果好了也不知道好的原因 --- 吐槽完毕），但其实这也提供了一个修正深度模型的机会，因为我们可以反过来利用对抗样本来提高模型的抗干扰能力，因此有了*对抗训练(adversarial training)* 的概念。通过对抗训练，相当于加上了一种形式的正则，可以提高模型的鲁棒性。

随着对对抗样本的更多更深入研究，人们逐渐发现，对抗样本并不是诅咒，而是祝福，因为可以利用对抗样本生成对抗网络(GANs)。[Generative Adversarial Networks, by Ian Goodfellow, et al, ](http://arxiv.org/abs/1406.2661) 最早提出了 *GANs* 的概念。在 GANs 中，包含一个生成模型G和一个判别模型D，D要判别样本是来自G还是真实数据集，而G的目标是生成能够骗过D的对抗样本，可以将G看做假币生产者，而D就是警察，通过G和D的不断交手，彼此的技能都会逐渐提高，最终使得G生产的假币能够以假乱真。
受此启发，[Deep Generative Image Models using a Laplacian Pyramid of Adversarial Networks， by Emily Denton, et al](http://arxiv.org/abs/1506.05751)通过为拉普拉斯金字塔中的每一个尺度建立一个生成模型，使得最终生成的图片与自然图片达到肉眼无法区分的地步，代码参见[项目eyescream](https://github.com/facebook/eyescream) 。还有人用 GANs [生成猫脸的图片](https://github.com/aleju/cat-generator)，也有人用它[生成人脸](http://torch.ch/blog/2015/11/13/gan.html)，GANs 真是简直要被玩坏了。进一步，[Conditional generative adversarial nets for convolutional face generationby Jon Gauthier](http://www.foldl.me/2015/conditional-gans-face-generation/)提出了条件生成对抗网络。文中给出了一个将人脸老化并加上笑容的例子，简直碉堡。


![人脸老化](oldface.png)


------------------

注：本文参考了[深度学习之对抗样本问题](http://www.infoq.com/cn/news/2015/07/adversarial-examples)一文。
