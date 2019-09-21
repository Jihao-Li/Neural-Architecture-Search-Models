# NAS-Models
This is my summary of some famous NAS models(in progress). At present, only the Chinese version is available.
# Organized in chronological order(arXiv v1)
## table 1
| 模型结构 | 提出时间 | 搜索方法 | 搜索数据集 | Cell或super-net |
| :---: | :---: | :---: | :---: | :---: |
| SMASH | 2017.8.17 | 随机搜索 | cifar10和cifar100 | macro搜索方式 |
| ENAS | 2018.2.9 | 强化学习 | cifar10 | 搜索Cell |
| DARTS | 2018.6.24 | 梯度方法 | cifar10 | 搜索Cell |
| One-Shot | 2018.7.3 | 随机搜索 | 分别在cifar10和ImageNet搜索 | 以Cell为基础，直接搜索模型 |
| MnasNet | 2018.7.31 | 强化学习 | ImageNet | 搜索super-net |
| NAO | 2018.8.22 | 梯度方法 | cifar10 | 搜索Cell |
| ProxylessNAS | 2018.12.2 | 梯度方法或强化学习 | 分别在cifar10和ImageNet搜索 | super-net |
| FBNet | 2018.12.9 | 梯度方法| ImageNet代理数据集 | 搜索super-net |
| ChamNet | 2018.12.21 | 进化算法 | ImageNet | 在Basenetwork上调整 |
| SNAS | 2018.12.24 | 梯度方法 | cifar10 | 搜索Cell |
| SPOS | 2019.3.31 | 进化算法 | ImageNet | 搜索super-net |
| Randomly Wired | 2019.4.2 | 随机图搜索 | ImageNet | 搜索super-net |
| Single-Path NAS | 2019.4.5 | 梯度方法 | ImageNet | 搜索super-net |
| DenseNAS | 2019.6.23 | 梯度方法 | ImageNet代理数据集 | 搜索super-net |
| FairNAS | 2019.7.3 | 强化学习和进化算法 | ImageNet | 搜索super-net |

## table 2
| 模型结构 | 搜索阶段更新参数时的显存 |
| :---: | :---: | 
| SMASH | 一条路径+HyperNet权重 |
| ENAS | 一条路径的权重+RL系统(REINFORCE) |
| DARTS | 全部路径的权重 |
| One-Shot | 全部路径的权重(但有dropout path) |
| MnasNet | 一条路径的权重+RL系统(PPO) |
| NAO | 全部可能的网络结构及其权重+编解码系统(LSTM) |
| ProxylessNAS | 两条路径的权重 |
| FBNet | 全部路径的权重 |
| ChamNet | 不更新参数。如果搜索到的结构性能不佳，则会通过进化算法进行选择，优胜劣汰 |
| SNAS | 全部路径的权重 |
| SPOS | 一条路径的权重 |
| Randomly Wired | 全部路径的权重。但随机图内部并不是所有结点之间都有连线，所以该方法的连接关系可能会比其它用全部路径的方法简单，也可能会更复杂 |
| Single-Path NAS | 一条路径的权重。但该路径是搜索空间中k和e值最大的那条 |
| DenseNAS | 全部路径的权重 |
| FairNAS | 多个一条路径的权重，串行或并行 | 

## table 3
| 模型结构 | 是否在ImageNet实验 |
| :---: | :---: | 
| SMASH | 否。只将cifar10或cifar100迁移到了STL10 |
| ENAS | 否 |
| DARTS | 是。cifar10迁移到ImageNet |
| One-Shot | 是。直接在ImageNet上进行搜索，挑选正确率最高的模型，再抛弃所有权重，重新进行训练|
| MnasNet | 是。直接在ImageNet上进行搜索，但不把搜索到的网络结构进行充分训练，只训练5个epoch |
| NAO | 否。只将cifar10迁移到了cifar100 |
| ProxylessNAS | 是。直接在ImageNet上搜索 |
| FBNet | 是。ImageNet代理数据集迁移到ImageNet |
| ChamNet | 是。先取部分ImageNet数据集，每个类别取50个样本，训练predictors(这里比较费时间，但该处的训练是one-time cost的，后边直接使用predictors，不再训练它们)，然后再在ImageNet上进行网络搜索 |
| SNAS | 是。cifar10迁移到ImageNet(同DARTS) |
| SPOS | 是。直接从ImageNet上进行搜索 |
| Randomly Wired | 是。直接从ImageNet上进行搜索 |
| Single-Path NAS | 是。直接从ImageNet上进行搜索 |
| DenseNAS | 是。ImageNet代理数据集迁移到ImageNet(同FBNet) |
| FairNAS | 是。直接从ImageNet上进行搜索 | 

## table 4
| 模型结构 | 是否需要retrain |
| :---: | :---: | 
| SMASH | 是。随机采样网络结构，训练HyperNet的权重；训练完毕后，将候选的网络结构输入到HyperNet中，导出网络结构的权重，判断网络性能；之后将性能好的网络结构保留，重新训练权重 |
| ENAS | 是。抛弃搜索权重，重新定义Cell的堆叠层数，只在cifar10上进行retrain |
| DARTS | 是。抛弃搜索权重，重新定义Cell的堆叠层数，分别在cifar10和ImageNet上进行retrain |
| One-Shot | 是。抛弃搜索权重，但Cell的堆叠层数保留，不重新定义 |
| MnasNet | 是。找到性能最好的15个网络结构(应该不抛弃搜索时的权重)，然后再在ImageNet上进行充分训练 |
| NAO | 是。抛弃搜索权重，重新定义Cell的堆叠层数，分别在cifar10和cifar100上进行retrain |
| ProxylessNAS | 原文中未说明 |
| FBNet | 是。抛弃搜索权重，根据概率采样性能好的super-net，在ImageNet上进行retrain |
| ChamNet | 文中未说明，但应该需要retrain。进化算法搜索网络结构时并没有用梯度下降优化网络权重，所以应该需要retrain(该结论不是很确定) |
| SNAS | 是。抛弃搜索权重，重新定义Cell的堆叠层数，分别在cifar10和ImageNet上进行retrain |
| SPOS | 是。搜索时不更新权重，权重都是一开始被初始化的，进化算法找到最好的super-net后再在ImageNet上进行retrain |
| Randomly Wired | 否。搜索的过程，同时也是训练网络权重的过程 |
| Single-Path NAS | 是。抛弃搜索权重，根据学得的阈值导出super-net，再在ImageNet上进行retrain(该结论不是很确定) |
| DenseNAS | 是。抛弃搜索权重，根据维特比算法导出最优super-net，在ImageNet上进行retrain |
| FairNAS | 原文中未说明 | 

## table 5
| 模型结构 | 约束项 |
| :---: | :---: | 
| SMASH | 无 |
| ENAS | 无 |
| DARTS | 无 |
| One-Shot | 无 |
| MnasNet | 搜索时得到的网络结构整体的latency。注意，不是每个block的 |
| NAO | 无 |
| ProxylessNAS | 通过每个block时的运行时间：latency |
| FBNet | 通过每个block时的运行时间：latency(同ProxylessNAS) |
| ChamNet | latency约束：计算每个op的latency，整体网络的latency即这些op的latency之和；energy约束：用一个predictor来预测energy，该predictor用GP+BO方法，它也需要训练。注意energy并不是在硬件上实际测量的energy |
| SNAS | 参数量、FLOPs、MAC |
| SPOS | FLOPs(可能还有其它约束) |
| Randomly Wired | 无 |
| Single-Path NAS | 通过每个block时的运行时间：latency。因为该模型不是多路径的，所以latency的计算方式和ProxylessNAS有差别 |
| DenseNAS | 通过每个block时的运行时间：latency(同ProxylessNAS) |
| FairNAS | FLOPs、参数量 | 

# Download link
* [SMASH: One-Shot Model Architecture Search through HyperNetworks](https://arxiv.org/abs/1708.05344)
* [Progressive Neural Architecture Search](https://arxiv.org/abs/1712.00559)
* [Regularized Evolution for Image Classifier Architecture Search](https://arxiv.org/abs/1802.01548)
* [Efficient Neural Architecture Search via Parameter Sharing](https://arxiv.org/abs/1802.03268)
* [DARTS DIFFERENTIABLE ARCHITECTURE SEARCH](https://arxiv.org/abs/1806.09055)
* [Understanding and Simplifying One-Shot Architecture Search](http://proceedings.mlr.press/v80/bender18a/bender18a.pdf)
* [MnasNet: Platform-Aware Neural Architecture Search for Mobile](https://arxiv.org/abs/1807.11626)
* [Neural Architecture Optimization](https://arxiv.org/abs/1808.07233)
* [PROXYLESSNAS: DIRECT NEURAL ARCHITECTURE SEARCH ON TARGET TASK AND HARDWARE](https://arxiv.org/abs/1812.00332)
* [FBNet: Hardware-Aware Efficient ConvNet Design via Differentiable Neural Architecture Search](https://arxiv.org/abs/1812.03443)
* [ChamNet: Towards Efficient Network Design through Platform-Aware Model Adaptation](https://arxiv.org/abs/1812.08934)
* [SNAS: STOCHASTIC NEURAL ARCHITECTURE SEARCH](https://arxiv.org/abs/1812.09926)
* [Single Path One-Shot Neural Architecture Search with Uniform Sampling](https://arxiv.org/abs/1904.00420)
* [Exploring Randomly Wired Neural Networks for Image Recognition](https://arxiv.org/abs/1904.01569)
* [Single-Path NAS Designing Hardware-Efficient ConvNets in less than 4 Hours](https://arxiv.org/abs/1904.02877)
* [Densely Connected Search Space for More Flexible Neural Architecture Search](https://arxiv.org/abs/1906.09607)
# Contributing
If you find any mistakes above or have something to add, please contact me. Thank you!
