# Note-for-SegGAN
make some notes for the paper named SegGAN

论文名称: Semantic Segmentation using Adversarial Networks  
论文地址: [https://arxiv.org/abs/1611.08408](https://arxiv.org/abs/1611.08408)  

在这篇论文中，作者首次将生成对抗网络(GAN)应用到语义分割中来。整个网络的框架如下图所示，这个网络由两部分组成。左边是一个分割器，就是一个普通的分割网络，作者根据不同的数据集选用了不同的结构。对于Stanford Background dataset，作者选用了[multi-scale segmentation network](http://yann.lecun.com/exdb/publis/pdf/farabet-pami-13.pdf),
而对于Pascal VOC 2012 dataset，作者选用了[Dilated-8 architecture](https://arxiv.org/abs/1511.07122).  
![SegGAN框架](image/framework.png)  


