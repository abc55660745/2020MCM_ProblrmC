# 2020MCM_ProblrmC
2020数模美赛MCM C题程序部分

以下是我对这次美赛的一点点感想，原文可以到[我的博客](https://www.bismarck.xyz:8000/29.html)查看

今年我算是第一次参加了数模美赛，体验了一把参赛的感觉。
说来差不多4天的时间完成模型构建、数据处理、还要写一篇完整的英文论文时间是非常紧张的，这次跟我一起合作的两位队友也非常给力，在DDL前6小时成功交稿。

![发送截图](https://i.loli.net/2020/03/10/zAPQernbUDgXJ3i.jpg)

图里因为有很多不能泄露的信息所以有很多码，请谅解。
我这次主要负责的是编程部分，也参与了一部分算法的工作，现在比赛论文提交已经截止了，我也将代码上传了github，其中也包含了C题官方给的3个数据集：

> [点我前往GitHub查看源码](https://github.com/abc55660745/2020MCM_ProblrmC)

美赛的体验是相当紧张的，毕竟给的时间很短，我觉得比较好的一个节奏可能是第一题完成相关资料的收集以及模型的确定，这一部分需要参赛队员一起讨论，一个人的力量真的是有限的。然后第二天编程应该完成整体程序的编写，论文也应该搭好基本的框架。整个第三天就是计算数据、处理表格，然后完善论文。第四天主要就是对论文修修补补
。
这里格外要说的就是一定要提前12个小时左右完成论文，千万别拖到最后，不然出各种bug是真的坑爹，比如这次我们组的论文最后导出到PDF就疯狂出错，换了4个导出引擎才成功导出，但是还是存在排版错误，又用Acrobat修改了半天，最后才在晚上接近两点提交了。

------------


再向下就是有关我对这次算法和代码部分的认知了

这次我们选的是C题，大家百度一下就能找到这次C题的翻译和原文我就不贴上来了。C题主要是给了一堆评论要求分析。

首先要求是要确定一个给予评分和文本的产品度量，我们先来看看我们有些什么（已经去除掉一些可能用不上的东西：
> *商品ID、商品标题、评分、支持该评论的票数、对该评论的总票数、评论的标题、评论的文本、评论者是否已购买商品、评论者是否为vine用户（可以参考淘宝88会员*

商品相关的几个东西很明显是给后面分析商品采用的，那么我们可以使用的就只有后面的几个数据。

看到一堆数据，要求聚合，第一反应直接就是平均，但是众所周知电商领域有各种水军啊、还有各种乱七八糟的东西，直接平均显然是不可取的，并且题目给了这么多数据放在哪里也不太好。

那么不能直接平均，那么我们使用平均的加强版——`加权平均`，加权平均带来的问题就是使用什么作为权重。

再来扫视一遍他给的数据，有了，用支持评论的票数，但是也带来了两个问题

1. 当该评论的踩过多的时候就会失去可靠性
2. 数据集里大部分评论的总票数都是0或者1，这样少的票数明显也是靠不住的

如何解决上面两个问题，再仔细阅读题目，还要有基于文本的度量，要能够获得一个权重。

从文本到权重，自然就是现在火的一塌糊涂的神经网络啦，当然，数据集数量不够、给的比赛时间不足，直接撸一个神经网络是不现实的，但是我们也可以借鉴神经网络的思路，首先要有一个能把文本转换为计算机可计算的数据的方法，这方面比较常用的是NLP，但同样还是上述的问题。同时基于官方给的都是英文数据，我直接采用了Python的TextBlob库来通过文本生成数字信息。

借鉴神经网络，我们写一个式子，权重等于评论除了评分、票数以外其他数据以及TextBlob生成的数据的线性组合，当然，由于TextBlob仅考虑词频，在很多方面也不够精确，尤其是句子短的时候，所以我们还要对上述等式进行一定的微调，这样我们就获得了权重的计算方法。

参考神经网络中的其他部分，将目标设定为对该评论的支持率（通过对票数计算产生），使用梯度下降法即可求参。

在这一部分内容过后，剩下的就显得较为简单，只需要对不同时间，不同商品的评论按上述方法聚合，产生各种表格，即可完成其他的题目

综上而言，看出美赛对模型方面其实并没有设计的特别难，真正有难度的是在短时间内团队能不能完成所有的步骤并且产出结果，也就是团队的综合能力。

------------


以上大概就是我对这次参加美赛的想法了，希望对你有帮助，也希望能够获奖

~~虽然我觉得第一次参加就得奖纯粹A是在开蟠桃会~~
~~但人总是要有梦想的，万一实现了呢~~
