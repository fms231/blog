---
title: 共享GPU内存与专用GPU内存
date: 2024-02-01 10:00:00 
categories: [科普]
---

# 独立显卡：共享GPU内存与专用GPU内存
## 概念
所谓专用GPU内存，就是专门给GPU使用的内存，该部分内存存在于显卡当中，也叫做VRAM(Video Random Access Memory)。
所谓共享GPU内存，就是CPU和GPU都可以共同使用的内存（GPU的使用优先级最高），该部分内存从系统内存（RAM）中划分出来。在Windows系统中，共享GPU内存大小为系统内存的一半，例如系统内存为16G，那么共享GPU内存为8G。

## 联系
当专用GPU内存不足时，会从共享GPU内存中分配内存给GPU使用。由于专用GPU内存的读写速度比共享GPU内存的读写速度快，因此，共享GPU内存并不能提供和专用GPU内存一样的性能。
此外，专用GPU内存是显卡的一部分，并与GPU核心紧密相关，而物理内存RAM需要使用PCIe连接将数据发送到GPU核心，这进一步影响了共享GPU内存的性能。
![共享GPU与专用GPU](https://img.sysgeek.cn/img/2023/03/shared-gpu-memory-4.jpg)
对于一些游戏来说，如果专用GPU内存不足，会自动从共享GPU内存中分配内存给GPU使用。但是一些深度学习的框架，如TensorFlow、PyTorch等，并不会自动从共享GPU内存中分配内存给GPU使用，所以可能会看到：专用GPU内存利用接近100%，共享GPU内存利用接近0%的情况。
![专用GPU内存不足而共享GPU内存空闲](/img/RAM.png)
`显卡内存 = 专用GPU内存 + 共享GPU内存`
