# Note-for-SegGAN
make some notes for the paper named SegGAN

论文名称: Semantic Segmentation using Adversarial Networks  
论文地址: [https://arxiv.org/abs/1611.08408](https://arxiv.org/abs/1611.08408)  

在这篇论文中，作者首次将生成对抗网络(GAN)应用到语义分割中来。整个网络的框架如下图所示，这个网络由两部分组成。左边是一个分割器，就是一个普通的分割网络，作者根据不同的数据集选用了不同的结构。对于Stanford Background dataset，作者选用了[multi-scale segmentation network](http://yann.lecun.com/exdb/publis/pdf/farabet-pami-13.pdf),
而对于Pascal VOC 2012 dataset，作者选用了[Dilated-8 architecture](https://arxiv.org/abs/1511.07122)。
右边则是一个对抗网络，相当于GAN中的判别器。输入分别是Segmentor生成的label map或者是Ground truth。为了与生成label map的resolution相匹配(经过Convnet导致分辨率下降)，Ground truth在输入进判别器之前要经过下采样。输出为0或1，0表示判别器预测为生成样本，1表示判别器预测为真实样本。通过这种对抗训练，可以使生成的label map与Ground truth保持高阶一致性(higher-order consistency)。  
```
Tips: 高阶一致性是相对于低阶一致性来说的。像L1，L2损失就属于低阶一致性，因为它统计的是单独像素之间的数值差异再累加起来，而高阶一致性倾向一种整体的连续性，使生成的图像在视觉上看起来更逼近原始图像，正如SRGAN中所说的一样。
```
![SegGAN框架](image/framework.png)  

SegGAN采用的损失函数结合了多分类交叉熵损失和对抗损失，并用一个超参数$ \lambda $来平衡这两项损失:
$$
l(\theta_s,\theta_a) = \sum_{n=1}^{N}l_{mce}(s(x_n),y_n)-\lambda[l_{bce}(a(x_n,y_n),1)+l_{bce}(a(x_n,s(x_n)),0)]
$$
其中$ x \in H\times W\times 3 $为输入图像，$ s(x) \in H\times W\times C $为分类器生成的label map(C为所有类别的总数，对于每一个类别，都会输出一张label map)，y自然就是Ground truth了。$ \theta_s $和$ \theta_a $分别是Segmentor和Adversarial network需要学习的参数。