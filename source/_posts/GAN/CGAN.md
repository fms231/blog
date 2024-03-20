---
title: Conditional GAN
date: 2024-2-1 20:00:00
tags: [CGAN, Deep Learning]
categories: 论文精读
---

# Conditional GAN介绍
传统GAN的局限性是：能生成真实的图片，但是该图片是随机的，不能生成特定的图片。比如生成猫的图片，我们并不能指定说生成一只橘黄色的猫，或者生成一只黑色的猫。
为了解决这个问题，Conditional GAN应运而生。Conditional GAN也是一种生成对抗网络，它可以生成特定的图片。它的输入是一个噪声向量和一个条件向量，输出是一个符合条件向量的图片。条件向量可以是任何东西，比如一个标签，一个类别，一个描述等等。

# Conditional GAN的网络架构
## 生成器
生成器的输入是一个噪声向量和一个条件向量，输出是一个符合条件向量的图片。生成器的网络架构如下：
![生成器](/img/CGAN/g.png)
将条件向量进过一个编码层转换为神经网络可以识别的向量，然后将噪声向量和条件向量拼接在一起，输入到生成器的神经网络中，生成一张图片。
![生成器](/img/CGAN/generator.png)

## 判别器
判别器的输入是一张图片和一个条件向量，输出是一个概率值，表示这张图片是真实的概率。判别器的网络架构如下：
![判别器](/img/CGAN/d.png)
判别器除了判断图片是真实的还是生成的，还要判断这张图片符合条件向量，如果图片很真实但不符合条件向量，那么判别器依旧会给出一个很低的概率值。
判别器也需要将条件向量转换为神经网络可以识别的向量，然后将图片和条件向量拼接在一起，输入到判别器的神经网络中，判断这张图片是真实的概率。
![判别器](/img/CGAN/discriminator.png)
## 损失函数
在原来GAN的损失函数中加上条件即可
原来的GAN的损失函数：
![GAN的损失函数](/img/CGAN/gan.png)
CGAN的损失函数
![CGAN的损失函数](/img/CGAN/cgan.png)

# LSGAN
LSGAN的损失函数就是将交叉熵损失函数BCEloss()换成MSEloss()


DCGAN：用卷积神经网络替换掉GAN的MLP。Deep Convolutional GAN深度卷积生成对抗网络。反卷积
![DCGAN](https://pytorch.org/tutorials/_images/dcgan_generator.png)
![DCGAN](/img/GAN/DCGAN.png)
pix2pix：将条件GAN中的标签换成了图片，用来做图像翻译。
GAN存在的问题：训练不稳定，生成的图片不够清晰，模式崩溃
WGAN：用Wasserstein距离替换掉GAN的JS散度，WGAN-CP：用Wasserstein距离替换掉GAN的JS散度，同时加上梯度惩罚
WGAN-GP：用Wasserstein距离替换掉GAN的JS散度，同时加上梯度惩罚。GAN的重大突破，解决了训练不稳定的问题。

BAM：Bottleneck Attention Module瓶颈注意力模块
CBAM：Convolutional Block Attention Module卷积块注意力模块
SAGAN：Self-Attention GAN自注意力生成对抗网络 2018年.Spectral Normalization。自注意力机制引入GAN中使得CNN能够捕捉长距离信息的关联。
生成器和判别器以不同的学习率进行训练会更好。
BigGAN：基于SAGAN的大规模生成对抗网络。在GAN中引入了truncation trick截断技巧，使得生成的图片更加清晰。
