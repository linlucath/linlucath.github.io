---
title: 'Lecture8_Notes'
publishDate: 2025-11-28
description: 'TODO'
tags:
  - TODO
language: 'English'
heroImage: { src: './default.jpg', color: '#D58388' }
---

Okay, welcome back to lecture eight. Today we're going to talk about CNN architectures, and this is really getting into the details of convolutional neural networks. Hopefully this will be pretty interesting.

欢迎回到第八讲。今天我们将讨论CNN架构，这将深入探讨卷积神经网络的细节。希望这会很有趣。

In the last lecture, we left off talking about convolutional networks. In particular, we talked about these different building blocks that we can use to build up convolutional networks, and we saw that convolutional neural networks are just neural networks that are built up of convolution layers, pooling layers, fully connected layers, some activation function probably ReLU, and some normalization layer often batch normalization.

在上一讲中，我们结束了对卷积网络的讨论。具体来说，我们讨论了用于构建卷积网络的各种不同构建模块，我们看到卷积神经网络就是由卷积层、池化层、全连接层、某些激活函数（可能是ReLU）以及某些归一化层（通常是批归一化）构成的神经网络。

But we were left with a big question of how do we actually combine these basic ingredients to actually hook up and make big high-performing convolutional neural networks. Even once you've defined these operations, you have a lot of freedom in how you might just choose to stick them together, what the hyper parameters are going to be, and just knowing these basic ingredients is far from enough to know how to actually get good performance out of convolutional neural networks.

但我们留下了一个大问题：如何实际组合这些基本要素来连接并构建出高性能的大型卷积神经网络。即使你定义了这些操作，在如何将它们组合在一起、超参数将是什么等方面，你仍有很大的自由度。仅仅了解这些基本要素远不足以知道如何实际从卷积神经网络中获得良好性能。

So rather than leaving you totally in the dark, today we're going to cover a historical overview of many different types of deep convolutional neural network architectures that people have used over the past few years or so.

因此，为了不让你完全摸不着头脑，今天我们将概述过去几年左右人们使用的许多不同类型的深度卷积神经网络架构的历史。

A good way to ground this discussion is in the ImageNet classification challenge. Remember we talked about the ImageNet dataset in the first two lectures, that it was this very large scale dataset for image classification that had about 1.2 million training images, and classification networks had to recognize about 1,000 different categories in this 1.2 million image dataset for training.

进行此讨论的一个好方法是基于ImageNet分类挑战赛。记得我们在前两讲中讨论过ImageNet数据集，它是一个用于图像分类的超大规模数据集，拥有约120万张训练图像，分类网络必须在这个用于训练的120万张图像数据集中识别大约1000个不同类别。

ImageNet was a huge benchmark for image classification because they held a yearly challenge from 2010 to 2017 where different teams would enter their best performing image classification system, and everyone around the world would compete against each other to try to build the best classification system.

ImageNet是图像分类的一个巨大基准，因为从2010年到2017年，他们每年都举办一次挑战赛，不同的团队会提交他们性能最佳的图像分类系统，世界各地的每个人都会相互竞争，试图构建最好的分类系统。

The ImageNet classification challenge really drove a lot of research and a lot of intense progress in convolutional neural network design over the past several years.

ImageNet分类挑战赛确实在过去几年中推动了大量研究，并在卷积神经网络设计方面取得了许多重大进展。

So I thought it would be useful to ground this discussion by stepping through some of the highest performing winners in the different years of the ImageNet competition.

因此，我认为通过逐步介绍ImageNet竞赛不同年份中一些表现最佳的获胜者来进行此讨论会很有用。

As we've already seen, in 2010 and 2011, the first two years the competition was run, the winning systems were not neural network based at all. They were these compositions of multiple layers of hand-designed features together with some linear classifier on top.

正如我们已经看到的，在2010年和2011年，即竞赛举办的头两年，获胜的系统完全不是基于神经网络的。它们是多层手工设计特征与顶部线性分类器的组合。

But in 2012, as you should probably remember by now, was the year that convolutional neural networks first became a huge mainstream topic in computer vision research when the AlexNet architecture just crushed all the other competition on the ImageNet challenge in 2012.

但在2012年，正如你现在可能还记得的那样，是卷积神经网络首次成为计算机视觉研究中主流热门话题的一年，当时AlexNet架构在2012年的ImageNet挑战赛中击败了所有其他竞争对手。

So what is AlexNet? What actually did AlexNet look like? Well, AlexNet was a deep convolutional neural network. By today's standards, I think it actually wouldn't be considered that deep, as we'll see as we go on in the lecture.

那么什么是AlexNet？AlexNet实际上是什么样子的？嗯，AlexNet是一个深度卷积神经网络。以今天的标准来看，我认为它实际上不会被认为那么深，随着课程的深入我们会看到这一点。

But AlexNet accepted 227 by 227 pixel inputs. It had five convolutional layers, they used max pooling throughout, and it had three fully connected layers that followed the convolutional layers, and it used ReLU nonlinearities throughout.

但AlexNet接受227x227像素的输入。它有五个卷积层，全程使用最大池化，在卷积层之后有三个全连接层，并且全程使用ReLU非线性激活函数。

In fact, AlexNet was one of the first major convolutional neural networks that used ReLU nonlinearities.

事实上，AlexNet是最早使用ReLU非线性激活函数的主要卷积神经网络之一。

There's a couple other kind of quirks and features of the AlexNet architecture that are not so much used anymore. One is that it had this funny layer called local response normalization, which has not really been used to date anymore, so we won't talk about it in detail.

AlexNet架构还有一些其他特性和特点现在已不太使用。其中之一是它有一个叫做局部响应归一化的特殊层，这个层至今已不再真正使用，所以我们不会详细讨论它。

But it was a different type of normalization and maybe a very early precursor to something like batch norm, but nowadays we prefer to use batch normalization instead.

但它是一种不同类型的归一化，也许是像批归一化这类方法的早期先驱，但如今我们更喜欢使用批归一化。

Another kind of quirky bit about AlexNet is that when Alex Krizhevsky and his collaborators were working on this network back in 2012, back in 2011 and so forth, it was trained on graphics cards GPUs.

关于AlexNet的另一个奇特之处是，当Alex Krizhevsky和他的合作者在2012年、2011年左右研究这个网络时，它是在图形处理器GPU上训练的。

The biggest GPU that they had at the time was a GTX 580, which had only three gigabytes of memory. If you look at maybe the GPUs you guys are using on Colab today, they have something like twelve or sixteen gigabytes of memory.

他们当时拥有的最大GPU是GTX 580，只有3GB内存。如果你看看今天你们在Colab上可能使用的GPU，它们有大约12或16GB的内存。

So back in 2011, the GPUs available just didn't have very much GPU memory. So in order to get this neural network fit into GPU memory, it was actually distributed across two different physical GTX 580 cards in kind of a complicated scheme where some of the network ran on one card and some of the network ran on another card.

所以在2011年，可用的GPU确实没有很大的显存。因此，为了让这个神经网络适应GPU内存，它实际上被分布在两个不同的物理GTX 580卡上，采用了一种复杂的方案，即网络的一部分在一个卡上运行，另一部分在另一个卡上运行。

This was kind of an implementation detail that was required in order to fit this network onto the GPU hardware that was available at that time.

这算是一种实现细节，是为了让这个网络适应当时可用的GPU硬件所必需的。

This idea of splitting neural networks across GPUs is still sometimes used today, but in general it's not a very common thing to see with most of the networks that we'll see in this lecture.

这种在多个GPU上分割神经网络的想法今天有时仍在使用，但总的来说，在我们本讲将看到的大多数网络中，这并不是很常见。

And of course, at the top of the slide here is this very very famous figure from the AlexNet paper that shows this convolutional neural network design of the AlexNet, and you can see that it has these five convolutional layers and it's split into two chunks at the top and the bottom to fit onto the two GPUs that it was distributed against.

当然，在幻灯片顶部是AlexNet论文中非常著名的图表，展示了AlexNet的卷积神经网络设计，你可以看到它有五个卷积层，并且在顶部和底部被分成两块，以适应它被分布到的两个GPU。

But one kind of funny thing about this figure is that it looks it's actually kind of clipped at the top, and if you look at the paper itself, even in the AlexNet paper itself, this figure was actually clipped at the top.

但关于这个图表的一个有趣的事情是，它看起来实际上在顶部被裁剪了，如果你看论文本身，即使在AlexNet论文本身中，这个图表实际上在顶部也是被裁剪的。

So even though this is a very important paper now where everyone is stuck looking at this clipped figure because that's the version of the figure that actually was published in the paper.

所以，尽管这是一篇非常重要的论文，现在每个人都只能看这个被裁剪的图表，因为这就是实际发表在论文中的版本。

I'd also like to point out just as a historic note, AlexNet, I think it's hard to overstate just how influential this paper has been.

我还想指出，作为一个历史记录，AlexNet，我认为很难夸大这篇论文的影响力有多大。

If you look at the number of citations that this paper has gotten per year since it was published in 2012, it's already gotten something like 46,000 citations since 2012, and if you look at this citation trend, it seems to be still growing exponentially.

如果你看看这篇论文自2012年发表以来每年获得的引用次数，自2012年以来它已经获得了大约46,000次引用，如果你看这个引用趋势，它似乎仍在呈指数级增长。

So this is certainly one of the most highly cited papers posted in computer science, but okay, I think across all disciplines in all areas of science in the last few years.

所以这无疑是计算机科学领域被引用次数最多的论文之一，而且我认为在最近几年所有科学领域的所有学科中也是如此。

To put this into context, I think it's also interesting to compare these citations with some other famous scientific papers throughout history.

为了理解这一点，我认为将其与历史上其他一些著名科学论文的引用次数进行比较也很有趣。

For example, if we look at Darwin's Origin of Species back in 1859, it has something like a similar number of citations as AlexNet does today.

例如，如果我们看看达尔文1859年的《物种起源》，它的引用次数与AlexNet今天的引用次数相似。

Or Claude Shannon's A Mathematical Theory of Communication, which invented the field of information theory, had something like 69,000 citations and it was published in 1948.

或者克劳德·香农的《通信的数学理论》，它开创了信息论领域，大约有69,000次引用，它发表于1948年。

If we look at contemporary research, there was another extremely important piece of scientific research published in 2012, which was the experimental discovery of the Higgs boson particle at the Large Hadron Collider.

如果我们看看当代研究，2012年还发表了另一项极其重要的科学研究，即在大型强子对撞机上希格斯玻色子粒子的实验发现。

This was published the same year as the AlexNet paper, and this is a fundamentally important advance in basic science that we observe a new fundamental particle in the universe, and this has only 14,000 citations compared to AlexNet's 46,000.

这与AlexNet论文同年发表，这是在基础科学中的一个根本性重要进展，我们观测到了宇宙中的一个新的基本粒子，但这只有14,000次引用，而AlexNet有46,000次。

So I need to caveat here that looking at citation counts for papers is really kind of a very coarse measure of their impact, and it's really unfair to compare citation counts across time and across different disciplines.

所以我需要在此说明，查看论文的引用次数确实是对其影响力的一种非常粗略的衡量，并且跨时间和跨学科比较引用次数确实是不公平的。

But that said, I think it's pretty clear to many people that this AlexNet paper and this AlexNet architecture represents an important advance not just within computer vision or computer science, but really across all of human knowledge as a whole.

但话虽如此，我认为对许多人来说很清楚的是，这篇AlexNet论文和AlexNet架构代表了一项重要的进步，不仅仅是在计算机视觉或计算机科学领域，而是真正跨越了整个人类知识领域。

Hopefully that's not overstating it too much.

希望这没有过分夸大。

But with that historical context in mind, what actually is, what does the AlexNet architecture actually look like?

但有了这个历史背景，AlexNet架构实际上是什么，它到底长什么样？

Well, AlexNet starts off with an input image of 227 by 227 pixels and works on RGB images, so it has three input channels.

嗯，AlexNet以一个227x227像素的输入图像开始，处理RGB图像，因此它有3个输入通道。

The first convolutional layer has 64 filters, a kernel size of 11 by 11, a stride of four, and a pad of two.

第一个卷积层有64个滤波器，核大小为11x11，步长为4，填充为2。

So given those settings for this first convolutional layer, what is the number of channels in the output of that first convolutional layer? Yeah, it should be 64, because recall that for a convolution layer, the number of channels is always equal to the number of filters.

那么，给定第一个卷积层的这些设置，第一个卷积层输出的通道数是多少？是的，应该是64，因为回想一下，对于卷积层，通道数总是等于滤波器的数量。

Now the next question is, what is the output size here? In this table, AlexNet collapsed height and width into one column because everything is squared.

下一个问题是，这里的输出尺寸是多少？在这个表格中，AlexNet将高度和宽度合并为一列，因为所有尺寸都是正方形的。

Anyone want to take a guess at the output spatial size of this convolution layer? Yeah, 56.

有人想猜一下这个卷积层的输出空间尺寸吗？是的，56。

If you remember this formula from the slide on the last lecture, we know that the output size is equal to the input size minus the kernel size plus 2 times the padding divided by the stride plus 1.

如果你还记得上一讲幻灯片中的这个公式，我们知道输出尺寸等于输入尺寸减去核大小加上2倍填充，除以步长，再加1。

If you plug in those numbers, you see that we get 56 for the output of this first layer.

如果你代入这些数字，你会看到第一层的输出是56。

Now another question, how much memory would this output feature map consume in kilobytes? Well, I don't know if it's reasonable to do this multiplication in your head, but the number of elements in that output tensor is going to be C by output size by output size.

另一个问题，这个输出特征图会消耗多少千字节的内存？嗯，我不知道在脑子里做这个乘法是否合理，但那个输出张量中的元素数量将是通道数乘以输出尺寸乘以输出尺寸。

So the number of elements in that output tensor is something like 200,000, and we typically store these elements in 32-bit floating-point, so each element takes 4 bytes of memory.

所以该输出张量中的元素数量大约是200,000，我们通常以32位浮点数存储这些元素，因此每个元素占用4字节内存。

So multiply that by 4 and divide out, and you see that this layer takes about 784 kilobytes of memory to store the output of this layer.

所以将其乘以4再换算一下，你会看到这一层大约需要784千字节的内存来存储其输出。

Now the next question, how many parameters are, how many learnable parameters are in this layer of a network? Well for this one, we remember what is the shape of a weight for a convolutional layer, and we remember that the shape of the weight for a convolutional layer is a four-dimensional tensor of size output channels by input channels by kernel size by kernel size.

下一个问题，这个网络层中有多少参数，有多少可学习参数？对于这个，我们记得卷积层的权重形状是什么，我们记得卷积层的权重形状是一个四维张量，大小为输出通道数乘以输入通道数乘以核大小乘以核大小。

So output channels of 64, input channels is 3, kernel size is 11, plus there's a learnable bias which is a vector of the same number as the number of output channels.

所以输出通道是64，输入通道是3，核大小是11，再加上一个可学习的偏置，它是一个向量，数量与输出通道数相同。

So the total number of learnable weights here is about 23,000.

所以这里可学习权重的总数大约是23,000。

Next, how many floating-point operations does it take to compute this convolution layer? Well again, I think it's maybe tricky to do this multiplication in your head.

接下来，计算这个卷积层需要多少次浮点运算？嗯，再次说明，我认为在脑子里做这个乘法可能很棘手。

But in order to compute this, first off, when we talk about floating-point operations in a neural network, we usually count the number of multiply-adds, where a multiply together with an add counts as one floating-point operation for the purpose of counting operations in a neural network.

但是为了计算这个，首先，当我们在神经网络中讨论浮点运算时，我们通常计算乘加运算的次数，其中一次乘法加上一次加法在计算神经网络操作时算作一次浮点运算。

This is because many actual bits of computing hardware can perform a floating-point multiplication and an accumulation in a single cycle, so we tend to count a multiply and an add as a single operation.

这是因为许多实际的计算机硬件可以在单个周期内执行一次浮点乘法和一次累加，因此我们倾向于将一次乘法和一次加法计为一次操作。

Now to count the number of operations that it costs to perform this convolution layer, we think how many output elements are there in the tensor, and that's C_out by output size by output size.

现在来计算执行这个卷积层所需的操作次数，我们考虑张量中有多少个输出元素，即C_out乘以输出尺寸乘以输出尺寸。

And how many operations does it take to compute each element of that output tensor? Well recall that each element of that output tensor is the result of computing an inner product between a convolutional filter, which has size C_in by K by K, and another chunk of the input which has size C_in by K by K.

计算该输出张量的每个元素需要多少次操作？回想一下，该输出张量的每个元素是计算卷积滤波器（大小为C_in x K x K）与另一块输入（大小为C_in x K x K）之间的内积的结果。

And a dot product taking a dot product of two vectors with n elements takes n multiplies and adds once you count the bias term.

而计算两个n元素向量的点积需要n次乘法和加法（如果计入偏置项）。

So when you multiply all that out, you see that this first convolutional layer takes something like 73 megaflops in order to compute the convolution of this first layer.

所以当你把所有这些东西乘出来时，你会看到第一个卷积层大约需要73兆次浮点运算来计算其卷积。

Now the second layer in AlexNet is a pooling layer immediately following, oh I mean this actually goes, there's a ReLU, so I'm sort of omitting the ReLUs from many of the architectures in this lecture because it's always assumed there'll be some ReLU or some non-linearity immediately following the convolution layer.

现在AlexNet中的第二层是紧随其后的池化层，哦，我的意思是，实际上这里有一个ReLU，所以我在本讲的许多架构中省略了ReLU，因为总是假设在卷积层之后会立即有某个ReLU或某种非线性。

So immediately after the ReLU in the first convolution, AlexNet has its first pooling layer, and the pooling layer here for AlexNet's first pooling layer has a kernel size of three, a stride of two, and a pad of zero.

所以在第一个卷积的ReLU之后，AlexNet有它的第一个池化层，而AlexNet的第一个池化层的核大小为3，步长为2，填充为0。

So given those parameters, what should the output shape of this first pooling layer in AlexNet be? Well, the number of channel dimensions is the same, because recall that pooling layers operate independently on each input channel, so pooling layers don't change the number of channels.

那么给定这些参数，AlexNet中第一个池化层的输出形状应该是什么？嗯，通道维度数量是相同的，因为回想一下，池化层在每个输入通道上独立操作，所以池化层不会改变通道数。

And here this pooling layer has the effect of downsampling the input spatially by a factor of two.

这里这个池化层的作用是在空间上将输入下采样两倍。

AlexNet's kind of a funny architecture and all the numbers don't actually divide evenly in AlexNet, which is a little bit annoying.

AlexNet的架构有点奇怪，而且所有的数字在AlexNet中实际上并不能整除，这有点烦人。

So here we have to, after we do the division by the stride, we also have to round down to get the output spatial size of 27 by 27.

所以在这里，我们除以步长之后，还必须向下取整才能得到27x27的输出空间尺寸。

How much memory does the output of the pooling layer take? And we see that we have the same procedure of four bytes per element multiplied by the number of elements in the tensor gives us the amount of memory usage of this layer.

池化层的输出占用多少内存？我们看到我们使用相同的步骤：每个元素4字节乘以张量中的元素数量，就得到这一层的内存使用量。

Next, how many learnable parameters in this pooling layer? Zero, because recall pooling layers have no learnable parameters; they simply take a max over their receptive field.

接下来，这个池化层中有多少可学习参数？零，因为回想一下，池化层没有可学习参数；它们只是在其感受野内取最大值。

Then how many floating-point operations does it take to compute this pooling layer? Well here it's a bit difficult to do the multiplication in your head, but again we have this similar way of thinking about how many elements are in the output tensor, which is number of output channels by output size by output size.

那么计算这个池化层需要多少次浮点运算？嗯，这里在心里做乘法有点困难，但我们再次使用类似的方法来思考输出张量中有多少个元素，即输出通道数乘以输出尺寸乘以输出尺寸。

And how many floating-point operations does it take to compute one element in the output tensor? Well recall that each element in the output tensor is the result of taking a max over the receptive field within one channel, so we have to take the max of a three by three grid of elements.

计算输出张量中的一个元素需要多少次浮点运算？回想一下，输出张量中的每个元素是在一个通道内的感受野上取最大值的结果，所以我们必须取一个3x3元素网格的最大值。

So we need to find the maximum of nine elements; you can imagine that taking approximately nine floating-point operations, maybe eight, but for simplicity we'll just say that it's equal to the kernel size squared.

所以我们需要找到九个元素中的最大值；你可以想象这大约需要九次浮点运算，也许是八次，但为简单起见，我们就说它等于核大小的平方。

So if you multiply this out, we see that this max pooling layer takes only about 0.4 megaflops, which you should notice is very, very small compared to the convolution layer.

所以如果你把这个乘出来，我们会看到这个最大池化层只消耗大约0.4兆次浮点运算，你应该注意到这与卷积层相比非常非常小。

So this is a fairly general trend in convolutional neural networks that the convolution layers tend to have a lot of, contain tend to cost a lot of compute, tend to take a lot of floating-point operations, whereas the max pooling layers or other types of pooling layers generally cost very little floating-point operations.

所以这是卷积神经网络中一个相当普遍的趋势，即卷积层往往包含大量计算，往往消耗大量浮点运算，而最大池化层或其他类型的池化层通常消耗非常少的浮点运算。

So much so that when sometimes people write papers and calculate operations in a neural network, they'll even not even count the operations of the max pooling layers just because the number of operations there is so small compared to the number of convolution layers.

以至于有时人们写论文计算神经网络中的操作时，他们甚至不计入最大池化层的操作，仅仅因为那里的操作数量与卷积层相比是如此之小。

Now AlexNet has five more convolution layers; I'm not going to walk through this exact procedure for each one of those layers, but we can similarly compute the output size and the number of memory and parameters and flops for each one of these five convolution layers in AlexNet.

现在AlexNet还有五个卷积层；我不打算对每一层都详细讲解这个过程，但我们可以类似地计算AlexNet中这五个卷积层每一层的输出尺寸以及内存、参数和浮点运算次数。

And interspersed with these convolution layers are more pooling layers, and by the time we finish with all the convolution layers and all of the pooling layers, AlexNet is left with an output tensor with 256 channels and a 6x6 spatial size.

这些卷积层之间穿插着更多的池化层，当我们完成所有卷积层和所有池化层后，AlexNet剩下一个具有256个通道和6x6空间尺寸的输出张量。

And after all of these convolution layers terminate, then we have this flattening operation that flattens out all of that, that destroys all the spatial structure in that input tensor and just flattens it out into a vector.

在所有这些卷积层结束后，我们有一个展平操作，它将所有东西展平，破坏了输入张量中的所有空间结构，并将其展平为一个向量。

So then this flatten layer just flattens everything into a vector, and it has no parameters and no flops.

所以这个展平层只是将所有内容展平为一个向量，它没有参数，也不消耗浮点运算。

Now after this flattening operation, we have our first fully connected layer with 4096 hidden units. Again, we can compute the memory, the parameters, and the flops for this first fully connected layer.

在这个展平操作之后，我们有第一个全连接层，具有4096个隐藏单元。同样，我们可以计算这第一个全连接层的内存、参数和浮点运算次数。

After the first fully connected layer, we've got two more fully connected layers, one more with 4096 hidden units and the final fc8 layer with 1,000 units to produce the 1,000 scores for our 1,000 categories.

在第一个全连接层之后，我们还有两个全连接层，另一个有4096个隐藏单元，最后的fc8层有1000个单元，为我们的1000个类别产生1000个分数。

So this is the AlexNet architecture, and the question is like how was this designed? The unfortunate reality I think behind AlexNet is that the exact configuration of these convolution layers was really a lot of trial and error.

所以这就是AlexNet架构，问题在于它是如何设计的？我认为AlexNet背后不幸的现实是，这些卷积层的具体配置很大程度上是反复试验的结果。

So the exact settings of these are somewhat mysterious, but it seems to work well in practice.

所以这些具体设置有些神秘，但在实践中似乎效果很好。

As we'll see moving forward, people wanted to try to find principles for designing neural networks that let them scale up and scale down and didn't rely so much on extensive trial and error of explicitly tuning the filter sizes and strides and everything for every individual layer.

随着我们继续深入，我们会看到人们想要尝试找到设计神经网络的原则，使其能够放大和缩小，并且不过度依赖于对每个单独层显式调整滤波器大小、步长等所有参数的大量试错。

But for AlexNet, I think the best answer is that these settings are really trial and error. But if we look over at these last three columns and look at the memory, the parameters, and the FLOPs marching down through the network, then we start to see some very interesting trends that hold true not just in AlexNet but also across a lot of different convolutional neural network architectures.

但对于AlexNet，我认为最好的答案是这些设置确实是试错的结果。但如果我们观察最后这三列，查看内存、参数和浮点运算量在网络中的变化趋势，我们会发现一些非常有趣的规律，这些规律不仅适用于AlexNet，也适用于许多不同的卷积神经网络架构。

Here we've already pointed out one trend: as we mentioned, all of the pooling layers take such little floating-point operations that they all round down to zero, so we can effectively discount the floating-point operations of the pooling layers when trying to count the number of operations in a network.

这里我们已经指出了一个趋势：正如我们提到的，所有池化层的浮点运算量都非常小，以至于都舍入为零，因此在计算网络中的操作数量时，我们可以有效地忽略池化层的浮点运算。

And here we can redraw that exact same data of the number of memory, the amount of memory, the number of parameters, and the number of FLOPs for each layer of this network and convert it into some bar charts.

这里我们可以重新绘制相同的数据——每层的内存使用量、参数数量和浮点运算量，并将其转换为柱状图。

So here we see a couple very interesting trends: if we look at the chart on the left, this shows the amount of memory that is used at the outputs of the first five convolutional layers and at the outputs of the three fully connected layers.

在这里我们看到几个非常有趣的趋势：如果我们查看左侧的图表，它显示了前五个卷积层和三个全连接层输出端的内存使用量。

And here we see this very clear trend that the vast majority of the memory usage in AlexNet actually comes from storing the activations at the early convolutional layers.

在这里我们看到一个非常明显的趋势，即AlexNet中绝大部分的内存使用实际上来自于早期卷积层的激活值存储。

This happens because at those early convolutional layers, the outputs have a relatively high spatial resolution and a relatively high number of filters, so when you multiply that out, it happens that most of the memory usage happens in the very first couple of layers of the network.

这是因为在那些早期卷积层中，输出具有相对较高的空间分辨率和相对较多的滤波器数量，因此当你计算时，大部分内存使用发生在网络的最初几层。

Now if we look at the middle plot, this shows the number of parameters in each layer, and this shows the opposite trend from compute: this shows that the convolutional layers have very very few parameters, whereas the fully connected layers actually take a very very large number of parameters.

现在如果我们看中间的图表，它显示了每层的参数数量，这显示了与计算量相反的趋势：卷积层的参数非常少，而全连接层实际上需要非常大量的参数。

The layer with the single largest number of parameters is that very first fully connected layer that happens after the flattening operation, because if you think about what happens in an FC6 layer: we had this spatial tensor of 6x6x256, and that gets fully connected into 4096 hidden dimensions.

参数数量最多的单个层是展平操作后的第一个全连接层，因为如果你思考FC6层中发生的情况：我们有一个6x6x256的空间张量，它被全连接到4096个隐藏维度。

So the weight matrix is now 6 times 6 times 256 times 4096, so that one weight matrix of FC6 has something like 37 million parameters—almost 38 million parameters in just that one fully connected layer of the neural network.

因此权重矩阵现在是6×6×256×4096，所以FC6的那一个权重矩阵就有大约3700万个参数——仅神经网络中那一个全连接层就有近3800万个参数。

In fact, basically all of the learnable parameters in AlexNet come from these fully connected layers, whereas if you look at the amount of computation that each layer costs, then you see yet another trend: the fully connected layers take very little computation because they're just multiplying a very large matrix, whereas the vast majority of the computation in this neural network comes from all the convolutional layers.

事实上，AlexNet中几乎所有可学习参数都来自这些全连接层，而如果你查看每层的计算量，你会看到另一个趋势：全连接层的计算量非常少，因为它们只是乘以一个非常大的矩阵，而该神经网络中的绝大部分计算来自所有卷积层。

Especially layers that take a lot of computation are layers that have convolutions with large numbers of filters at high spatial resolutions, and this is quite a general trend across many different neural network designs—it's not just AlexNet.

特别消耗计算量的层是那些在高空间分辨率下具有大量滤波器的卷积层，这是许多不同神经网络设计中相当普遍的趋势——不仅仅是AlexNet。

You'll have most of the memory usage in the early convolutional layers, most of the parameters in the fully connected layers, and most of the computation in the convolutional layers.

你会在早期卷积层拥有大部分内存使用，在全连接层拥有大部分参数，在卷积层拥有大部分计算量。

So these trends are kind of interesting to keep in mind as we move on to later architectures that try to address more efficient architectures that try to fix some of these trends.

因此，在我们继续讨论后来试图解决这些趋势、追求更高效架构的设计时，记住这些趋势是很有趣的。

So that's our brief overview of the AlexNet architecture—that's what happened in 2012.

这就是我们对AlexNet架构的简要概述——这就是2012年发生的事。

What happened in 2013? Well in 2013, pretty much all of the entrants to this competition now switched over to using neural networks, and the winner of the competition was also an 8-layer network called ZFNet after the authors Matt Zeiler and Rob Fergus.

2013年发生了什么？在2013年，几乎所有的竞赛参赛者都转向使用神经网络，竞赛的获胜者也是一个8层网络，名为ZFNet，以作者Matt Zeiler和Rob Fergus的名字命名。

ZFNet is basically a bigger AlexNet—I told you that AlexNet was essentially produced via trial and error, well ZFNet is more trial and less error.

ZFNet基本上是一个更大的AlexNet——我告诉过你AlexNet基本上是通过试错产生的，而ZFNet是更多尝试和更少错误。

So basically in ZFNet, it's the same basic idea as AlexNet except they tweaked some of the layer configurations: in particular, in the first convolutional layer, AlexNet had 11x11 stride 4, but it turns out it works better if you use 7x7 stride 2—who would have thought?

所以基本上在ZFNet中，其基本思想与AlexNet相同，只是他们调整了一些层的配置：特别是在第一个卷积层中，AlexNet使用11x11步长4，但事实证明使用7x7步长2效果更好——谁会想到呢？

And for those later convolutional layers in convolutional layers 3, 4 & 5, instead of using 384, 384, 256 filters like in AlexNet, instead we increase the number of filters and use 512, 1024, 512, and who knows—this also tends to work better.

对于后面的卷积层3、4和5，不像AlexNet那样使用384、384、256个滤波器，而是增加滤波器数量并使用512、1024、512，谁知道呢——这也往往效果更好。

To be a little bit less facetious, I think the takeaway from ZFNet is that it's just a bigger version of AlexNet.

稍微认真一点说，我认为ZFNet的要点是它只是AlexNet的一个更大版本。

So if you look at the first convolutional layer, when we change from stride 4 to stride 2, that means that we are aggressively downsampling the input in space at the very first layer.

所以如果你看第一个卷积层，当我们将步长从4改为2时，这意味着我们在第一层就对输入进行了激进的空间下采样。

So with this 11x11 stride 4 in AlexNet, we immediately spatially downsampled by a factor of 4, whereas for ZFNet that first convolutional layer will only downsample by a factor of two, which means that all the other feature maps moving throughout the network will now have a higher spatial resolution.

在AlexNet中使用11x11步长4，我们立即在空间上下采样了4倍，而对于ZFNet，第一个卷积层只下采样2倍，这意味着网络中所有其他特征图现在将具有更高的空间分辨率。

Higher spatial resolution means more receptive fields, means more compute, so ZFNet actually is going to cost a lot more computation than AlexNet.

更高的空间分辨率意味着更多的感受野，意味着更多的计算，所以ZFNet实际上将比AlexNet消耗更多的计算量。

And for the later convolutional layers, by increasing the number of filters, this also just makes the network bigger—it has more learnable parameters, it takes more memory, it takes more compute.

对于后面的卷积层，通过增加滤波器数量，这也只是使网络更大——它有更多可学习参数，需要更多内存，需要更多计算。

So I think the takeaway from AlexNet to ZFNet is that bigger networks tend to work better, but at this point in time there was not really a principled mechanism for making the networks bigger or smaller at will.

所以我认为从AlexNet到ZFNet的要点是更大的网络往往效果更好，但在那个时候，还没有一个原则性的机制来随意使网络变大或变小。

Instead they kind of had to reach into individual layers and tune both the individual parameters one at a time in order to make the networks bigger, but in doing so they were able to achieve a fairly large increase in performance over AlexNet, and we saw the error rate on this ImageNet challenge dropped from 16.4 down to 11.7 with ZFNet.

相反，他们不得不深入到各个层，并逐个调整各个参数以使网络变大，但通过这样做，他们能够在性能上实现相对于AlexNet的相当大提升，我们看到ImageNet挑战赛的错误率随着ZFNet从16.4下降到11.7。

Now 2014 was when things started to get very very interesting, and 2014 brought around the so-called VGG architecture from Karen Simonyan and Andrew Zisserman.

现在2014年是事情开始变得非常非常有趣的时候，2014年带来了Karen Simonyan和Andrew Zisserman提出的所谓VGG架构。

Now VGG was really one of the first architectures to have a principled design throughout—we saw that AlexNet and ZFNet were designed in somewhat of an ad hoc way: there was some number of convolution layers, there was some number of pooling layers, but the exact configurations of each layer were set independently by hand through trial and error.

VGG确实是第一个具有贯穿始终的原则性设计的架构之一——我们看到AlexNet和ZFNet是以某种临时方式设计的：有一定数量的卷积层，有一定数量的池化层，但每层的具体配置是通过试错手动独立设置的。

This makes it very hard to scale networks up or down, so instead, as we moved into now starting in 2014, people started to move away from these hand-designed bespoke convolutional architectures and instead wanted to move to architectures that had some design principles that were used to guide the overall configuration of the network.

这使得网络很难扩展或缩小，因此相反，随着我们进入2014年，人们开始远离这些手工设计的定制卷积架构，而是希望转向具有一些设计原则的架构，这些原则用于指导网络的整体配置。

The VGG networks in particular just followed a couple very very clean and simple design principles: the design principles of VGG were that all convolution layers are going to be 3x3 stride 1, all pooling layers are going to be max pooling layers 2x2 stride 2, and after a max pooling layer we're going to double the number of channels, and then we're going to have some number of convolution layers and eventually some fully connected layers, and the number of hidden units in the fully connected layers were the same as AlexNet.

VGG网络特别遵循了几个非常非常清晰简单的设计原则：VGG的设计原则是所有卷积层都是3x3步长1，所有池化层都是最大池化层2x2步长2，在最大池化层之后我们将通道数加倍，然后我们将有一定数量的卷积层，最后是一些全连接层，全连接层中的隐藏单元数量与AlexNet相同。

So with these simple design rules, it lets you not have to think so hard about the exact configuration of each layer in your neural network.

因此，有了这些简单的设计规则，你就不必那么费力地思考神经网络中每层的具体配置。

And also, this network had five convolutional stages—remember that AlexNet had 5 convolutional layers—now VGG pushed that forward and moves to deeper networks: we're now rather than five individual convolutional layers, we have five stages where each stage consists of a couple convolution layers and a pooling layer.

而且，这个网络有五个卷积阶段——记住AlexNet有5个卷积层——现在VGG推进了这一点并转向更深的网络：我们现在不是五个独立的卷积层，而是有五个阶段，每个阶段由几个卷积层和一个池化层组成。

So the VGG architecture is like conv-conv-pool, conv-conv-pool, and for however many performance stages you're going to have.

所以VGG架构类似于卷积-卷积-池化，卷积-卷积-池化，无论有多少个性能阶段。

There were several different VGG architectures that were tested, but the ones that were most popular were the 16-layer and the 19-layer VGG architectures, which had always two convolutional layers in the first few stages and either three or four convolutional layers in the last two stages.

测试了几种不同的VGG架构，但最流行的是16层和19层VGG架构，它们在前几个阶段总是有两个卷积层，在最后两个阶段有三个或四个卷积层。

So that's pretty much all you need to know in order to know how to build a VGG network, but it's useful to think about why people chose these particular design principles for designing the network in this way.

所以这几乎是你需要知道的关于如何构建VGG网络的全部内容，但思考人们为什么选择这些特定的设计原则以这种方式设计网络是有用的。

Well first, let's think about why it makes sense to have only 3x3 convolutions in your network.

首先，让我们思考为什么在网络中只使用3x3卷积是有意义的。

You saw in AlexNet and in ZFNet that the size of a convolutional kernel in each layer was a hyperparameter, and people played around with different convolutional kernel sizes at different layers.

你在AlexNet和ZFNet中看到，每层卷积核的大小是一个超参数，人们在不同层尝试了不同的卷积核大小。

Well let's think about two different options that we could have as alternatives: as one alternative, we could imagine a convolutional layer with 5x5 kernels that takes C channels of input and produces C channels of output that operates on an input of size H by W, and here we can assume that we have padding of 2 and stride of 1 so the output size is the same spatial size as the input.

让我们思考两个可以作为替代方案的不同选项：作为一个替代方案，我们可以想象一个具有5x5核的卷积层，它接受C个输入通道并产生C个输出通道，在大小为H×W的输入上操作，这里我们可以假设我们有2的填充和1的步长，因此输出大小与输入具有相同的空间大小。

This convolutional layer would have a number of parameters of 25 C squared because we've got C convolutional filters, each one has 5x5xC, so 25 C squared learnable parameters in this layer ignoring bias, and the number of floating-point operations that it costs to compute this convolutional layer is now 25 C squared HW because the number of outputs from the layer is going to be H by W by C, and the cost of computing every one of those outputs will be 5x5xC, so the overall cost of the layer is 25 C squared HW.

这个卷积层将有25 C平方个参数，因为我们有C个卷积滤波器，每个都有5x5xC，所以这一层有25 C平方个可学习参数（忽略偏置），计算这个卷积层所需的浮点运算量是25 C平方 HW，因为该层的输出数量将是H×W×C，计算每个输出的成本将是5x5xC，因此该层的总成本是25 C平方 HW。

Now let's contrast this with a stack of two convolutional layers that each have kernel size 3x3 that also produce C channels as input and produce C channels as output.

现在让我们将其与两个卷积层的堆栈进行对比，每个卷积层具有3x3的核大小，也接受C个通道作为输入并产生C个通道作为输出。

As we remember from our discussion of receptive fields in the previous lecture, we know that if we stack two 3x3 convolutions, then it has an effective receptive field size of 5x5.

正如我们从上一讲关于感受野的讨论中记得的，我们知道如果我们堆叠两个3x3卷积，那么它有一个5x5的有效感受野大小。

So in terms of how much of the input can we see after this number of layers in terms of receptive fields, this 5x5 convolutional layer and this pair of 3x3 convolutional layers are somehow equivalent in terms of the amount of the input that they are able to see.

因此在感受野方面，就这些层之后我们能看到的输入量而言，这个5x5卷积层和这对3x3卷积层在它们能够看到的输入量方面是等价的。

But if we compute the number of parameters for these things, we see that each of these two convolutional layers, the number of parameters is 9 C squared, so the total number of parameters for the stack of two 3x3 convs is 18 C squared.

但如果我们计算这些的参数数量，我们看到这两个卷积层中每一层的参数数量是9 C平方，所以两个3x3卷积堆栈的总参数数量是18 C平方。

Similarly, the number of floating-point operations for this stack of two convolutional layers is only 18 C squared HW because again the output is C HW and the cost of computing any one of those outputs is 3x3xC, so each output costs 9C, and multiply it all out we've got two layers so the overall cost of the stack of two convs is 18 C squared HW.

类似地，这两个卷积层堆栈的浮点运算量只有18 C平方 HW，因为同样输出是C HW，计算任何其中一个输出的成本是3x3xC，所以每个输出成本是9C，乘以所有输出我们有两层，所以两个卷积堆栈的总成本是18 C平方 HW。

Now we see something interesting: even though these two layers have the same receptive field size, the stack of two 3x3 convolutions has fewer learnable parameters and it costs less computation.

现在我们看到一些有趣的事情：即使这两层具有相同的感受野大小，两个3x3卷积的堆栈具有更少的可学习参数并且计算成本更低。

So the insight from the VGG network is that well maybe there's no reason to have larger filter sizes at all, because anytime you wanted to have a 5x5 filter, you could have instead replaced it with two 3x3 convolutions.

所以VGG网络的见解是，也许根本没有理由使用更大的滤波器大小，因为任何时候你想要一个5x5滤波器，你都可以用两个3x3卷积代替它。

By a similar argument, rather than a single 7x7 filter, you could have replaced it with a stack of three 3x3 convolution layers.

通过类似的论证，而不是单个7x7滤波器，你可以用三个3x3卷积层的堆栈代替它。

So with that in mind, it lets us sort of throw away the kernel size as a hyperparameter, and the only thing we need to worry about is how many of these 3x3 conv layers are going to stack within each stage.

因此，考虑到这一点，它让我们可以抛弃核大小作为超参数，我们唯一需要担心的是在每个阶段内要堆叠多少个这样的3x3卷积层。

The other piece about this is that if we stack two 3x3 convolutional filter layers after one another, we can actually insert ReLUs in between those two convolutional layers, which actually provides us more depth and more nonlinear computation compared to a single 5x5 convolution.

关于这一点的另一部分是，如果我们将两个3x3卷积滤波器层一个接一个地堆叠，我们实际上可以在这两个卷积层之间插入ReLU，与单个5x5卷积相比，这实际上为我们提供了更多的深度和更多的非线性计算。

So not only does the stack of two 3x3 convolutions have fewer parameters, it has fewer FLOPs, and it allows more nonlinear computation, so it just seems like a clear win over a single 5x5 convolutional layer.

因此，两个3x3卷积的堆栈不仅参数更少，FLOPs更少，而且允许更多的非线性计算，所以它似乎明显优于单个5x5卷积层。

So that's the idea behind this first design rule.

这就是第一个设计规则背后的思想。

So then let's think about the second design rule in VGG: it says that all of our pooling layers are going to be 2x2 max pooling stride 2 pad 0, which means that every pooling layer is going to halve the spatial resolution of the input feature map.

那么让我们思考VGG中的第二个设计规则：它说我们所有的池化层将是2x2最大池化步长2填充0，这意味着每个池化层将使输入特征图的空间分辨率减半。

The other rule here is that every time after we pool, we will double the number of channels.

这里的另一个规则是，每次池化之后，我们将通道数加倍。

So then let's think about what happens between two stages when we follow these rules.

那么让我们思考当我们遵循这些规则时，两个阶段之间会发生什么。

For instance, one of our layers inside stage one would have an input of size C by 2H by 2W, and the layer would be a 3x3 convolution with C input channels and C output channels.

例如，阶段一内部的其中一个层将具有大小为C×2H×2W的输入，该层将是一个具有C个输入通道和C个输出通道的3x3卷积。

If you multiply all this out, we see that the amount of memory consumed by this output tensor is going to be 4HW C—it's the number of elements in the output tensor after this convolution.

如果你计算所有这些，我们看到这个输出张量消耗的内存量将是4HW C——这是卷积后输出张量中的元素数量。

The number of parameters is 9 C squared excluding the bias, and the number of floating-point operations is 4HW times 9 C squared—using right actually that's that doesn't seem right I think that's an error but that's ok the same errors propagated over here so the argument still holds.

参数数量是9 C平方（不包括偏置），浮点运算数量是4HW乘以9 C平方——实际上这似乎不对，我认为这是一个错误，但没关系，相同的错误传播到这里，所以论证仍然成立。

After we move to the next stage, then the number of channels would be doubled and the spatial resolution would be halved.

当我们移动到下一个阶段后，通道数将加倍，空间分辨率将减半。

When this happens, we can see that the memory is reduced by a factor of two, and the number of parameters increases by a factor of four, but the number of floating-point operations stays the same.

当这种情况发生时，我们可以看到内存减少了一半，参数数量增加了四倍，但浮点运算量保持不变。

Now here's the error: these two number of floating-point operations I think are both off by a factor of nine, but since they're both off by a factor of nine, it's still true that these two layers in two subsequent stages still cost the same number of floating-point operations.

现在这里有个错误：我认为这两个浮点运算量都偏离了九倍，但由于它们都偏离了九倍，这两个后续阶段中的层仍然花费相同数量的浮点运算量这一事实仍然成立。

So this design principle has actually been followed by many many convolutional architectures following VGG.

因此，这个设计原则实际上被许多许多跟随VGG的卷积架构所遵循。

The basic idea is that we want to preserve this equivalence—we want each convolutional layer to cost the same amount of floating-point operations, and we can do that by halving spatial size and doubling the channels at the end of each convolutional stage.

基本思想是我们希望保持这种等价性——我们希望每个卷积层花费相同数量的浮点运算，我们可以通过在每个卷积阶段结束时将空间大小减半和通道数加倍来实现这一点。

So then another thing to point out is that we can compare AlexNet and VGG16 side by side: remember that AlexNet had five convolutional layers and three fully connected layers, and now all of the VGG networks also have five convolutional stages and three fully connected layers.

那么另一件需要指出的事情是，我们可以并排比较AlexNet和VGG16：记住AlexNet有五个卷积层和三个全连接层，现在所有VGG网络也有五个卷积阶段和三个全连接层。

Now we can draw this same plot of memory, parameters, and floating-point operations to compare at a stage-by-stage basis between AlexNet and VGG.

现在我们可以绘制相同的内存、参数和浮点运算量图，在AlexNet和VGG之间进行逐阶段比较。

Here the overwhelming result from these graphs is that VGG is just a gigantic network compared to AlexNet—if we look at the number of memory, you can't even see these blue bars on these graphs—VGG is just dwarfing AlexNet on all of these different aspects of computation.

这些图表中最压倒性的结果是，与AlexNet相比，VGG只是一个巨大的网络——如果我们看内存数量，你甚至看不到这些图表上的蓝色条——VGG在所有这些不同的计算方面都使AlexNet相形见绌。

It takes dramatically more memory: if you look at the total amount of memory consumed by storing activations for all these outputs, then VGG is something like 25 times greater.

它需要显著更多的内存：如果你查看存储所有这些输出的激活值所消耗的总内存量，那么VGG大约是25倍。

If you look at the total number of learnable parameters, AlexNet had about 61 million, VGG16 has 138 million, so more than twice as many learnable parameters.

如果你看可学习参数的总数，AlexNet有大约6100万，VGG16有1.38亿，所以可学习参数是两倍多。

The real killer is the computation: if you add up the total number of floating-point operations that it takes to compute a single forward pass in AlexNet versus a single forward pass in VGG, we see that VGG is more than 19 times more expensive in terms of floating-point operations.

真正的杀手是计算：如果你累加在AlexNet中计算单次前向传播与在VGG中计算单次前向传播所需的总浮点运算量，我们看到VGG在浮点运算方面贵了19倍多。

So VGG16 is just this absolutely massive network, and again we still get this story that we saw moving from AlexNet to ZFNet: that bigger networks tend to achieve better results on these large-scale ImageNet challenge.

所以VGG16只是一个绝对庞大的网络，我们再次得到了从AlexNet到ZFNet所看到的故事：更大的网络往往在这些大规模ImageNet挑战上取得更好的结果。

But now with VGG, it gives us a guiding principle—a couple of guiding design principles—that let us easily scale up or scale down the network, and we no longer have to go in and fiddle with the individual hyperparameters of every layer.

但现在有了VGG，它给了我们一个指导原则——几个指导设计原则——让我们可以轻松地扩展或缩小网络，我们不再需要深入并调整每层的各个超参数。

Now 2014 was such an interesting year that there's actually two convolutional neural network architectures that came out of that year's challenge that we need to talk about.

现在2014年是如此有趣的一年，实际上有来自那年挑战赛的两个卷积神经网络架构我们需要讨论。

One was VGG—I should point out another very amazing thing about this VGG architecture is that it was done in academia by one grad student and one faculty member, so that was quite a heroic effort on their part.

一个是VGG——我应该指出关于这个VGG架构的另一件非常惊人的事情是，它是在学术界由一名研究生和一名教师完成的，所以这是他们相当英勇的努力。

The other network that we need to talk about from 2014 was from Google. This was a very large team with access to a very large amount of computation. So I think it was quite a testament to the VGG team in 2014 that even though they didn't win the ImageNet classification challenge that year, they really held their own against this corporate team with access to many more resources.

2014年我们需要讨论的另一个网络来自谷歌。这是一个拥有大量计算资源的庞大团队。因此我认为这充分证明了2014年VGG团队的实力——尽管当年他们没有赢得ImageNet分类挑战赛，但在面对这个拥有更多资源的公司团队时，他们确实展现了自己的竞争力。

Now the main takeaway of GoogleNet is that they wanted to be cute. Remember there was LeNet, one of the earliest convolutional neural network architectures created by Yann LeCun. Now in homage to LeNet and Yann LeCun, this Google team decided to name their network GoogleNet. It's very cute.

GoogleNet的主要特点是他们想取个俏皮的名字。记得最早的卷积神经网络架构之一是由Yann LeCun创建的LeNet。为了向LeNet和Yann LeCun致敬，这个谷歌团队决定将他们的网络命名为GoogleNet。这非常有趣。

The overwhelming idea behind GoogleNet was to focus on efficiency. If you look at the trend from AlexNet to ZF to VGG, the trend that we can see is that bigger networks perform better. But with GoogleNet, the team was really focused on trying to design efficient convolutional neural networks because Google actually wants to run these things for real in data centers and on mobile phones.

GoogleNet背后的核心思想是注重效率。从AlexNet到ZF再到VGG的发展趋势可以看出，更大的网络表现更好。但GoogleNet团队真正专注于设计高效的卷积神经网络，因为谷歌实际上需要在数据中心和移动设备上实际运行这些网络。

So they save a lot of money if they can get the same performance with a cheaper convolutional network design. So they were really focused on trying to build a network that worked really well while also minimizing the overall complexity of the network.

如果他们能用更经济的卷积网络设计获得相同的性能，就能节省大量资金。因此他们真正致力于构建一个性能优异同时又能最小化网络整体复杂度的网络。

They had a couple innovations that were made popular by the GoogleNet architecture that were carried forward to many other following neural network architectures. One is the use of a stem network at the very first couple of convolutional layers which very aggressively down samples the input image.

GoogleNet架构推广了几项创新，这些创新被许多后续的神经网络架构所采用。其中之一是在最初的几个卷积层使用stem网络，它非常激进地对输入图像进行下采样。

The very expensive layers were those big convolutions on feature maps of very large spatial size. So to avoid those expensive convolutions on large spatial feature maps, they use a lightweight stem that quickly down samples the input.

计算成本最高的层是在具有很大空间尺寸的特征图上进行的大卷积运算。为了避免在大型空间特征图上进行这些昂贵的卷积运算，他们使用轻量级的stem来快速对输入进行下采样。

I'm not going to walk through the stem design in detail, but you can see that it very quickly down samples from the input resolution of 224x224 all the way down to 28x28 using only a couple layers. So that you can spend the bulk of computation now operating at this lower spatial resolution.

我不会详细讲解stem设计，但你可以看到它仅用几层就快速将输入分辨率从224x224下采样到28x28。这样你就可以将大部分计算资源用于在这个较低的空间分辨率上进行操作。

Here we could actually compare to VGG16. If you look at the component of GoogleNet that down samples from 224 down to 28 in spatial resolution, that entire part of the network costs about 418 megaflops for GoogleNet.

这里我们可以与VGG16进行比较。如果你查看GoogleNet中将空间分辨率从224下采样到28的组件，这部分网络在GoogleNet中大约消耗418兆次浮点运算。

If you look at the equivalent spatial downsampling in VGG16 that goes from 224 down to 28, you can see that that same amount of spatial down sampling in VGG16 costs more than 7 gigaflops. So that same amount of spatial down sampling was nearly 18 times as expensive in VGG16 compared to GoogleNet.

如果你查看VGG16中从224下采样到28的等效空间下采样，可以看到VGG16中相同量的空间下采样消耗超过7千兆次浮点运算。因此相同量的空间下采样在VGG16中的成本比GoogleNet高出近18倍。

The other innovation in GoogleNet is this so-called inception module. They were very clever because they got to go deeper as they called it inception. The idea is that they had this little module that they called inception that was this local structure that was repeated throughout the entire network.

GoogleNet的另一项创新是所谓的inception模块。他们非常聪明，因为他们将其称为inception以表示可以更深。这个想法是他们有一个称为inception的小模块，这是一个在整个网络中重复的局部结构。

Just as VGG used this simple repeated structure of conv-conv-pool, now GoogleNet used this little inception module design that was repeated throughout the entire architecture many times.

就像VGG使用简单的conv-conv-pool重复结构一样，GoogleNet使用这个小的inception模块设计，在整个架构中多次重复。

The inception module introduced this idea of parallel branches of computation. In VGG, the convolutional kernel size is always a hyperparameter that we want to try to avoid. In VGG they took the approach of replacing kernels with a stack of 3x3 convolutions.

inception模块引入了并行计算分支的概念。在VGG中，卷积核大小始终是一个我们试图避免的超参数。在VGG中，他们采用用3x3卷积堆叠替换其他卷积核的方法。

GoogleNet took a different approach and they said that in order to eliminate the kernel size as a hyperparameter, we're just going to do all the kernel sizes all the time.

GoogleNet采取了不同的方法，他们说为了消除卷积核大小这个超参数，我们将同时使用所有尺寸的卷积核。

So they've been inside this inception module they had four parallel branches: one that does a 1x1 convolution, one that does a 3x3 convolution, one that does a 5x5 convolution, and one that does a max pooling with a stride of one.

因此在这个inception模块内部，他们有四个并行分支：一个进行1x1卷积，一个进行3x3卷积，一个进行5x5卷积，还有一个进行步长为1的最大池化。

So within every one of these layers it does all the things. So there's no need to tune kernel sizes as a hyperparameter because you've got all the kernel sizes in all the places.

因此在每一层中它都执行所有操作。这样就不需要将卷积核大小作为超参数进行调优，因为你在所有位置都使用了所有尺寸的卷积核。

The other bit of innovation in the inception module was the use of 1x1 convolutions before expensive spatial convolutions that was used to reduce the number of channels before doing these expensive spatial convolutions.

inception模块的另一项创新是在昂贵的空间卷积之前使用1x1卷积，用于在执行这些昂贵的空间卷积之前减少通道数。

We'll revisit this idea of 1x1 convolutional bottlenecks when we talk about residual networks in a few minutes. So I don't want to talk about that in detail here.

当我们稍后讨论残差网络时，会重新审视这个1x1卷积瓶颈的概念。所以这里我不打算详细讨论。

The other innovation in GoogleNet is the use of global average pooling at the very end of the network. If you remember back to VGG and AlexNet, we saw that the vast majority of the parameters in VGG and AlexNet were coming from these giant fully connected layers at the very end of the network.

GoogleNet的另一项创新是在网络最后使用全局平均池化。如果你回顾VGG和AlexNet，我们看到VGG和AlexNet中绝大多数参数来自网络末端那些巨大的全连接层。

Because one of the ways that they focused on efficiency is by reducing the number of parameters in the network, GoogleNet simply eliminates those large fully connected layers.

因为他们关注效率的方法之一是减少网络中的参数数量，GoogleNet直接消除了那些大型全连接层。

Remember in AlexNet and VGG at the end of the convolution layers we had this flatten operation that destroyed spatial information by flattening the convolutional tensor into a giant vector.

记得在AlexNet和VGG中，在卷积层末尾我们有一个展平操作，通过将卷积张量展平为巨大向量来破坏空间信息。

GoogleNet uses a different strategy for destroying spatial information. Rather than flattening the tensor, instead they use an average pooling with a kernel size equal to the final spatial size of the last convolutional layer.

GoogleNet使用不同的策略来破坏空间信息。不是展平张量，而是使用一个核大小等于最后一个卷积层最终空间尺寸的平均池化。

In particular, the last convolution at the end of this last inception module in GoogleNet, the output tensor has a spatial size of 7x7 with 1024 feature maps. Now then they apply an average pooling with kernel size equal to 7x7.

具体来说，在GoogleNet最后一个inception模块末尾的最后一个卷积层，输出张量的空间尺寸为7x7，具有1024个特征图。然后他们应用一个核大小为7x7的平均池化。

The stride doesn't matter because it only fits in one place. So what that means is within every of those 1024 channels, they take the average of the values of those channels across all the spatial positions in the input tensor.

步长无关紧要，因为它只能在一个位置拟合。这意味着在1024个通道中的每一个通道内，他们取输入张量中所有空间位置上这些通道值的平均值。

This now also destroys spatial information but rather than flattening into a giant tensor, instead it actually reduces the total number of elements. So it ends up with this compact vector with only 1024 elements.

这同样破坏了空间信息，但不是展平为巨大张量，而是实际上减少了元素总数。最终得到一个只有1024个元素的紧凑向量。

So then there's only one fully connected layer in GoogleNet which then goes from this 1024 output from global average pooling that converts to the 1000 classes, where again 1000 is the number of categories in the ImageNet dataset.

因此GoogleNet中只有一个全连接层，它将全局平均池化的1024维输出转换为1000个类别，其中1000是ImageNet数据集中的类别数量。

So GoogleNet is able to eliminate a huge number of learnable parameters by simply eliminating these fully connected layers and instead replacing them with the idea of global average pooling.

因此GoogleNet通过简单地消除这些全连接层并用全局平均池化的概念替代它们，能够消除大量可学习参数。

This is something that got picked up by a lot of different convolutional neural networks following GoogleNet. We can also compare this side by side with VGG and just see how profoundly this affects the number of parameters in last couple of layers.

这是许多不同的卷积神经网络在GoogleNet之后采用的方法。我们也可以将其与VGG进行并列比较，看看这对最后几层的参数数量产生了多么深远的影响。

Another piece of awkwardness in GoogleNet is that they had to rely on this idea of auxiliary classifiers. I should point out that GoogleNet actually occurred before batch normalization.

GoogleNet的另一个尴尬之处是他们不得不依赖辅助分类器的概念。我应该指出GoogleNet实际上出现在批量归一化之前。

Before the discovery of batch normalization, it was very difficult to train networks that had more than about 10 layers or so. And whenever people wanted to train deeper networks than about 10 layers without batch normalization, they had to resort to some ugly hacks.

在批量归一化发现之前，训练超过10层左右的网络非常困难。当人们想要在没有批量归一化的情况下训练超过10层的更深网络时，他们不得不采用一些不太优雅的技巧。

One of the ugly hacks that was used in GoogleNet is this idea of auxiliary classifiers. So here what they did is they attached auxiliary global average pooling and fully connected layers at several internal points in the network.

GoogleNet中使用的一个不太优雅的技巧就是辅助分类器的概念。他们所做的是在网络中的几个内部点附加辅助的全局平均池化和全连接层。

So this thing was actually outputting three different sets of class scores: one from the end of the network and two from these intermediate parts of the network. Then for these intermediate classifiers they also compute loss and propagate gradients coming back through all three of these classifiers.

因此这个网络实际上输出三组不同的类别分数：一组来自网络末端，两组来自这些中间部分。然后对于这些中间分类器，他们还计算损失并通过所有这三个分类器反向传播梯度。

This had the effect of making gradients propagate more easily through the network because they now inject gradient at the very top of the network at the final classifier and they also inject gradient directly in these two auxiliary classifiers.

这产生了使梯度更容易通过网络传播的效果，因为他们现在在网络的最高处（最终分类器）注入梯度，同时也在这两个辅助分类器中直接注入梯度。

This was a trick that they used in order to get things to converge in order to get deep networks to converge at that time.

这是他们当时为了让深度网络收敛而使用的一个技巧。

You can see that 2014 was kind of a dark time for neural network practitioners. We had to resort to all these crazy hacks to get your networks to converge once they got beyond a certain depth.

你可以看到2014年对神经网络从业者来说是一个黑暗时期。一旦网络超过一定深度，我们不得不采用所有这些疯狂技巧来让网络收敛。

Thankfully things changed in 2015. Actually one of the important things that happened between 2014 and 2015 is when batch normalization was discovered.

幸运的是，情况在2015年发生了变化。实际上2014年到2015年间发生的重要事件之一就是批量归一化的发现。

Once batch normalization was discovered, people found that they were able to train VGG and train GoogleNet from scratch without any of these tricks by just using batch normalization instead.

一旦批量归一化被发现，人们发现他们能够从头开始训练VGG和GoogleNet，而不需要任何这些技巧，只需使用批量归一化即可。

But then there was an extremely important innovation in neural network architecture design that happened in the 2015 iteration of the ImageNet challenge and those were called residual networks or ResNets.

但随后在2015年ImageNet挑战赛中出现了神经网络架构设计中极其重要的创新，这些被称为残差网络或ResNets。

Here you can see something amazing happen: the number of layers jumped in one year from 22 all the way up to 152. So this was a very important innovation in the history of neural network architecture design.

在这里你可以看到令人惊讶的事情发生：层数在一年内从22层跃升至152层。这是神经网络架构设计历史上非常重要的创新。

You can also see that the error dropped dramatically again from 6.7 almost down to 3.6. So ResNets were kind of a very important moment in neural network architecture design.

你还可以看到错误率再次显著下降，从6.7几乎降至3.6。因此ResNets是神经网络架构设计中非常重要的时刻。

As we mentioned, once batch normalization had been discovered, people realized that they were able to train networks that were fairly deep with even with dozens of layers.

正如我们提到的，一旦批量归一化被发现，人们意识到他们能够训练相当深的网络，甚至达到几十层。

So then the question is what happens if we just keep stacking layers and stacking layers and try to train very very deep networks?

那么问题是，如果我们只是不断堆叠层数并尝试训练非常非常深的网络，会发生什么？

Well here is kind of a cartoon picture of the types of plots people saw at that time. Here's the training curve where the x-axis is the number of training iterations and the y-axis is the test error.

这是当时人们看到的图表类型的示意图。这是训练曲线，其中x轴是训练迭代次数，y轴是测试误差。

We're comparing a 56 layer model and a 20 layer model. And here's something very strange happens: we see that the 56 layer model actually performs worse than the 20 layer model.

我们比较一个56层模型和一个20层模型。这里发生了非常奇怪的事情：我们看到56层模型实际上比20层模型表现更差。

This is surprising because the previous trend that we had talked about was that bigger neural networks tended to work better up to this point in time.

这令人惊讶，因为我们之前讨论的趋势是，直到此时，更大的神经网络往往表现更好。

So it was very surprising to all of a sudden see the bigger deeper networks now performing worse once you got past a certain point.

因此突然看到更大更深的网络在超过某个点后表现更差，这非常令人惊讶。

The initial guess of what was going on is that maybe these networks had started overfitting. That you can imagine maybe once you got to a 56 layer network and you have batch normalization, maybe this was just such a large network that it was now overfitting the ImageNet training set.

最初的猜测是可能这些网络开始过拟合了。你可以想象，一旦达到56层网络并且有批量归一化，可能这个网络太大了，以至于现在对ImageNet训练集过拟合了。

But in order to test this hypothesis, we can look at the training performance of these same networks. And if you look at the performance on the training set of the same 20 layer and 56 layer networks, you see that the network was not overfitting.

但为了检验这个假设，我们可以查看这些相同网络的训练性能。如果你查看相同20层和56层网络在训练集上的性能，你会发现网络并没有过拟合。

That in fact this 56 layer network was somehow underfitting the training set. That somehow there's a problem in optimization that somehow this deeper model, even once we have batch normalization, once you get to a certain depth, we're no longer able to efficiently optimize very deep networks.

事实上，这个56层网络在某种程度上对训练集欠拟合。在优化方面存在某种问题，即使我们有了批量归一化，一旦达到某个深度，我们就不再能够有效优化非常深的网络。

This is a problem and this is also surprising because we should expect that a deeper model should have the capacity to emulate a shallower model.

这是一个问题，同时也令人惊讶，因为我们期望更深的模型应该有能力模拟更浅的模型。

What do I mean by that? You could imagine that a 56 layer network could emulate a 20 layer network because we could imagine taking the 20 layer network copying all of its layers into the 56 layer network and have all of the other remaining layers just learn the identity function.

我这是什么意思？你可以想象一个56层网络可以模拟20层网络，因为我们可以想象将20层网络的所有层复制到56层网络中，并让所有其他剩余层只学习恒等函数。

So in principle, if our optimizers were working properly, then the deeper network should always have the capacity to represent the same functions as the shallower networks.

因此原则上，如果我们的优化器工作正常，那么更深的网络应该总是有能力表示与更浅网络相同的函数。

So if we are actually underfitting, then it means that we have an optimization problem that somehow these deeper networks are not able to efficiently learn these identity functions in order to emulate shallower networks.

因此如果我们实际上欠拟合，那么意味着我们有一个优化问题，即这些更深的网络无法有效学习这些恒等函数来模拟更浅的网络。

The solution is to change the design of the network to make it easier for it to learn identity functions on unused layers. And this should make it easier for deeper networks to learn to emulate shallower networks in case they have too many layers and more layers than they actually needed.

解决方案是改变网络设计，使其更容易在未使用的层上学习恒等函数。这应该使更深的网络更容易学习模拟更浅的网络，以防它们有过多的层，比实际需要的层更多。

So here is the design change that was proposed by residual networks. Previously on the left here we have the kind of plain convolutional block that we had seen in VGG that is a stack of two consecutive convolutional layers.

这就是残差网络提出的设计变更。之前在左边是我们见过的VGG中的普通卷积块，即两个连续卷积层的堆叠。

Global average pooling from GoogleNet again eliminates the fully connected layers and reduces the total number of parameters in the network.

GoogleNet中的全局平均池化再次消除了全连接层，并减少了网络中的参数总量。

With these simple patterns, the only things you need to choose are the initial width of the network (which was 64 in all their experiments) and the number of blocks per stage.

通过这些简单模式，您只需要选择网络的初始宽度（在他们所有实验中均为64）以及每个阶段的块数。

This gives us the ResNet-18 which has two residual blocks per stage, which means four convolutions per stage. Four times four is sixteen convolutions, plus the convolution in the stem and the linear layer at the end, so if you add that together that's 18 layers with learnable weights.

这就得到了ResNet-18，每个阶段有两个残差块，即每个阶段有四次卷积。四乘四等于十六次卷积，加上起始的卷积和末端的线性层，总共是18个具有可学习权重的层。

The question is what we mean by downsampling in this context. We mean any operation that reduces the spatial extent of the image, so that could be strided convolution, max pooling, or average pooling.

问题是在这个上下文中下采样是什么意思。我们指的是任何减小图像空间范围的运算，因此可以是步进卷积、最大池化或平均池化。

Taking every other pixel is not used - I don't think I've ever seen that used in a neural network context, but it's differentiable. You could try it but I don't recommend it.

隔像素采样未被使用——我不认为我在神经网络上下文中见过这种用法，但它是可微的。你可以尝试，但我不推荐。

What's interesting about this ResNet is that they become very efficient, achieving very low errors on ImageNet with very few floating-point operations.

这个ResNet的有趣之处在于它们变得非常高效，能够用极少的浮点运算在ImageNet上实现非常低的错误率。

There's also a 34-layer version of ResNet which just adds more blocks to some stages but otherwise the design is exactly the same.

还有一个34层的ResNet版本，只是在某些阶段添加了更多块，但其他方面设计完全相同。

We can compare this to VGG which had something like 13 gigaflops for the whole network and got errors of about 9.6%, whereas the ResNet-34 was only 3.6 gigaflops and actually had lower errors.

我们可以将其与VGG进行比较，VGG整个网络大约需要13 gigaflops，错误率约为9.6%，而ResNet-34仅需3.6 gigaflops，实际上错误率更低。

A lot of these gains in efficiency were due to the aggressive downsampling at the beginning and the global average pooling at the end.

这些效率上的提升很大程度上归功于起始阶段的激进下采样和末端的全局平均池化。

As we go to deeper residual networks, they actually modified the block design.

当我们转向更深的残差网络时，他们实际上修改了块设计。

Here on the left is the so-called basic block used in residual networks, which has two 3x3 convolutions with ReLUs and batch norms in between them, and a residual shortcut around the 3x3 convolutions.

左边是残差网络中使用的所谓基础块，它有两个3x3卷积，中间有ReLU和批归一化，并且在3x3卷积周围有一个残差捷径。

We can compute the total floating-point operations for this block, counting only the convolutional layers, and see that each convolutional layer costs 9HWC², so the total computational cost is 18HWC².

我们可以计算这个块的总浮点运算（仅计算卷积层），并看到每个卷积层的成本是9HWC²，因此总计算成本是18HWC²。

For deeper residual networks, they introduced an alternative block design called the bottleneck block.

对于更深的残差网络，他们引入了一种替代的块设计，称为瓶颈块。

The bottleneck block consists of three convolutional layers: the first is a 1x1 convolution that reduces the number of channels from 4C down to C, then a 3x3 convolution, then another 1x1 convolution that expands the number of channels from C back up to 4C.

瓶颈块由三个卷积层组成：第一个是1x1卷积，将通道数从4C减少到C；然后是一个3x3卷积；接着是另一个1x1卷积，将通道数从C扩展回4C。

The computational cost of this bottleneck design is 17HWC², which is slightly less than the basic block.

这个瓶颈设计的计算成本是17HWC²，略低于基础块。

This means we get less computation but more non-linearity and more sequential computation, with the intuition that deeper layers should be able to perform more complex types of computation.

这意味着我们获得了更少的计算量，但更多的非线性和更多的顺序计算，其直觉是更深的层应该能够执行更复杂的计算类型。

By switching from the basic block to the bottleneck block, we can build deeper networks without increasing computational cost.

通过从基础块切换到瓶颈块，我们可以在不增加计算成本的情况下构建更深的网络。

We've seen the 18-layer and 34-layer residual networks that use the basic block.

我们已经看到了使用基础块的18层和34层残差网络。

If we take the 34-layer ResNet and replace all basic blocks with bottleneck blocks, this increases the total number of layers from 34 to 50 but does not really change the overall computational cost.

如果我们取34层ResNet并用瓶颈块替换所有基础块，这会将总层数从34增加到50，但并不会真正改变总体计算成本。

By making this change that increases the number of layers without increasing computation, the error on ImageNet actually decreases from 5.85% down to 7.13%, which is a fairly large reduction given the similar computational cost.

通过进行这种增加层数而不增加计算的更改，ImageNet上的错误率实际上从5.85%下降到7.13%，考虑到相似的计算成本，这是一个相当大的减少。

We can define 101-layer and 152-layer versions of residual networks that have the same basic design using bottleneck blocks and just more blocks per stage.

我们可以定义101层和152层版本的残差网络，它们具有相同的基本设计，使用瓶颈块，只是每个阶段有更多的块。

The clear trend with residual networks is that as you stack these layers to go deeper, the networks tend to work better.

残差网络的明显趋势是，随着你堆叠这些层以变得更深，网络往往表现更好。

This was a big deal in 2015 - ResNets crushed everything, winning every track in the ImageNet competition (classification, localization, detection) and also swept every challenge in the MS COCO dataset.

这在2015年是一件大事——ResNet碾压了一切，赢得了ImageNet竞赛的每个赛道（分类、定位、检测），并且横扫了MS COCO数据集的所有挑战。

The main thing they did was take existing methods for all these different tasks and just swap in their 152-layer residual network, crushing everyone that year.

他们做的主要事情是采用所有这些不同任务的现有方法，然后直接换入他们的152层残差网络，在那一年碾压了所有人。

This got everyone to wake up and pay attention, and from that time forward residual networks became a widely used baseline for computer vision tasks.

这让大家警醒并开始关注，从那时起，残差网络成为了计算机视觉任务中广泛使用的基线。

There was a follow-up paper that played with the exact order of convolution, batch norm, and ReLU, finding that shuffling the orders could get it to work a bit better.

有一篇后续论文研究了卷积、批归一化和ReLU的确切顺序，发现调整这些顺序可以让它工作得更好一些。

Here is a comparison of computational complexity - a plot with x-axis as number of floating-point operations, y-axis as accuracy on ImageNet, and dot size as number of learnable parameters.

这是计算复杂性的比较——一个图表，x轴是浮点运算次数，y轴是ImageNet上的准确率，点的大小是可学习参数的数量。

From this plot we can see that VGG has very high memory and computation requirements and is inefficient overall.

从这个图中我们可以看到，VGG具有非常高的内存和计算需求，整体效率低下。

GoogleNet is very small and efficient but not quite as high-performing as some later networks.

GoogleNet非常小且高效，但性能不如后来的一些网络。

AlexNet down in the corner has relatively low compute but still has many parameters due to large fully connected layers.

位于角落的AlexNet计算量相对较低，但由于大的全连接层，仍然有很多参数。

Residual networks give us a fairly simple design with moderate efficiency but high accuracy as we scale to deeper networks.

残差网络为我们提供了一种相当简单的设计，具有中等效率，但随着我们扩展到更深的网络，准确率很高。

In 2016, the winner was model ensemble averaging, which was not very exciting - it just took all the winning architectures from the last couple years and averaged them together.

在2016年，获胜者是模型集成平均，这并不令人兴奋——它只是取了过去几年所有获胜的架构并将它们平均在一起。

There were attempts to improve residual networks, leading to ResNeXt which had multiple parallel bottleneck branches.

有人尝试改进残差网络，导致了ResNeXt，它具有多个并行的瓶颈分支。

The idea was that if one bottleneck branch is good, why not have multiple bottleneck branches in parallel?

其想法是，如果一个瓶颈分支是好的，为什么不在并行中拥有多个瓶颈分支呢？

In ResNeXt, the basic building block has G parallel pathways, each being a little bottleneck block with inner channel dimension C.

在ResNeXt中，基本构建块有G个并行路径，每个都是一个内部通道维度为C的小瓶颈块。

The outputs from these parallel bottleneck blocks are added together.

这些并行瓶颈块的输出被加在一起。

We can set up a quadratic equation to find little C such that this multi-path architecture with G parallel branches has the same computational cost as the original bottleneck design.

我们可以建立一个二次方程来找到小C，使得这个具有G个并行分支的多路径架构具有与原始瓶颈设计相同的计算成本。

It turns out that with 64 channels and 4 parallel pathways, we can set little C to 24 channels on each pathway for the same computational cost.

事实证明，对于64个通道和4个并行路径，我们可以将每个路径上的小C设置为24个通道，以获得相同的计算成本。

There's an operation in PyTorch convolution called grouped convolution that lets us implement this idea of parallel pathways.

在PyTorch卷积中有一个称为分组卷积的操作，可以让我们实现这种并行路径的想法。

In ResNeXt design, as we keep the same computational cost but increase the number of parallel pathways within each block, the performance actually increases.

在ResNeXt设计中，当我们保持相同的计算成本但增加每个块内的并行路径数量时，性能实际上提高了。

We can start with a baseline 50-layer ResNet model and increase the number of parallel pathways, and the accuracy increases.

我们可以从一个基线50层ResNet模型开始，增加并行路径的数量，准确率会提高。

The same trend holds for the 101-layer ResNet - we can increase the number of pathways and get improved performance while maintaining the same computational complexity.

同样的趋势适用于101层ResNet——我们可以增加路径数量并在保持相同计算复杂度的同时获得改进的性能。

In 2017, people built on top of ResNeXt and added squeeze and excitation that made things work a bit better.

在2017年，人们在ResNeXt的基础上构建并添加了压缩和激励，使效果更好一些。

2017 was the end of the road - after 2017, people decided that the juice had been squeezed out of the ImageNet dataset and the challenge was shutting down.

2017年是终点——2017年之后，人们认为ImageNet数据集的潜力已被榨干，挑战赛也关闭了。

Even though the ImageNet challenge ended, people still went nuts trying to design bigger and more interesting neural network architectures.

尽管ImageNet挑战赛结束了，人们仍然疯狂地尝试设计更大、更有趣的神经网络架构。

Many lessons were carried forward into later neural network designs: aggressive downsampling, trading computation, maintaining computation while building efficient networks, repeated block structures.

许多经验教训被延续到后来的神经网络设计中：激进的下采样、权衡计算、在构建高效网络的同时保持计算量、重复的块结构。

One architecture you might see is the densely connected neural network, which uses concatenation shortcuts rather than additive shortcuts like residual networks.

你可能会看到的一种架构是密集连接的神经网络，它使用连接捷径而不是像残差网络那样的加法捷径。

Rather than adding previous features to later features, they concatenate previous features with later features to reuse the same features at different parts in the network.

它们不是将先前的特征加到后面的特征上，而是将先前的特征与后面的特征连接起来，以在网络的不同部分重复使用相同的特征。

People are trying, but follow-up research has actually... This paper has become kind of a joke in the community just because of the unbelievable scale of resources that were used in this paper. But actually, follow-up papers on architecture search have significantly reduced this search time.

人们正在努力尝试，但后续研究实际上... 这篇论文在学术界几乎成了笑话，仅仅因为其中使用了难以置信的资源规模。但实际上，架构搜索领域的后续论文已经显著减少了这种搜索时间。

But people always love to compare to this paper and say, "Oh look, we're ten thousand times more efficient than this previous one!" If that's a good way to get your papers accepted... But the takeaway is that if you have the resources to burn, then neural architecture search actually has been used to find neural network architectures which are themselves very efficient.

但人们总是喜欢与这篇论文比较，然后说："看，我们比之前的方案高效一万倍！"如果这是让论文被接收的好方法... 但关键启示是，如果你有足够的资源可以挥霍，那么神经架构搜索实际上已经被用于寻找本身就非常高效的神经网络架构。

So this is kind of another plot that is nice to summarize a lot of the architectures that we've seen so far in this lecture. Here on the x-axis we're showing the computational cost of running the neural network, that is the number of multiply-adds, and the y-axis now is the accuracy on ImageNet.

这是另一个很好的图表，可以总结我们在本讲座中看到的许多架构。在x轴上我们展示了运行神经网络的计算成本，即乘加运算的次数，而y轴现在是ImageNet上的准确率。

You can see that all these dots correspond to different neural network architectures that we've talked about in this lecture, and all these red lines correspond to different neural network architectures that were learned using a neural architecture search method.

您可以看到所有这些点对应我们在本讲座中讨论过的不同神经网络架构，而所有这些红线对应使用神经架构搜索方法学习到的不同神经网络架构。

What you can see is that this neural architecture search method was able to learn a set of different architectures that achieved higher accuracy at a lower computational cost compared to other architectures. So that's kind of a next frontier in neural network architecture design.

可以看到的是，这种神经架构搜索方法能够学习到一组不同的架构，与其他架构相比，这些架构以更低的计算成本实现了更高的准确率。这可以说是神经网络架构设计的下一个前沿领域。

The kind of summary from what we've seen today is that in the early days of convolutional neural networks, as we move from AlexNet to ZFNet to VGG, people were just focused on training ever bigger neural networks in order to get higher and higher accuracies.

从我们今天所看到的内容总结来看，在卷积神经网络的早期阶段，当我们从AlexNet发展到ZFNet再到VGG时，人们只是专注于训练越来越大的神经网络以获得越来越高的准确率。

GoogleNet was one of the first to focus on efficiency, and in doing so they wanted to get high accuracy while also being aware of the computational cost.

GoogleNet是最早关注效率的网络之一，通过这样做，他们希望在获得高准确率的同时也关注计算成本。

Residual networks gave us a way to scale networks to become very big, and we were able to train networks with hundreds of layers once we had these key ingredients of batch normalization and residual networks.

残差网络为我们提供了一种将网络扩展到非常大的方法，一旦我们掌握了批归一化和残差网络这些关键要素，我们就能够训练具有数百层的网络。

After ResNet, people started to focus on efficiency more and more, and somehow that became the guiding principle of a lot of neural network architecture design after residual networks.

在ResNet之后，人们开始越来越关注效率，这在某种程度上成为了残差网络之后许多神经网络架构设计的指导原则。

So then we saw this huge proliferation of different architectures that are trying to achieve higher or same accuracy at lower computational cost. This includes these tiny networks like MobileNets and ShuffleNets, and neural architecture search promises to maybe one day design all of our neural networks for us.

于是我们看到了不同架构的大量涌现，这些架构试图以更低的计算成本实现更高或相同的准确率。这包括像MobileNets和ShuffleNets这样的小型网络，而神经架构搜索有望在将来某天为我们设计所有的神经网络。

But now the final question is, you know, this is all great, but what architectures should I actually use in practice? Well, my advice is: don't be a hero.

但现在最后一个问题是，这一切都很棒，但实际上我应该使用什么架构呢？我的建议是：不要逞英雄。

For most applications, don't try to design your own neural network architecture. You're going to cause yourself sadness, and you don't have 800 GPUs to burn for a month, I think.

对于大多数应用，不要尝试设计自己的神经网络架构。你会给自己带来痛苦，而且我认为你也没有800个GPU可以烧一个月。

So what you should probably do in most situations is take an existing neural network architecture and adapt it for your problem. And that's what I do in my research, and that's what I recommend you guys do for any projects that you undertake.

因此在大多数情况下，你应该做的是采用现有的神经网络架构并针对你的问题进行适配。这就是我在研究中采用的方法，也是我建议你们在任何项目中采用的方法。

In particular, despite the number of things that have come after, I think ResNet-50 and ResNet-101 are still really great solid choices that work. If you want something to just work and not have to fiddle with it too much, those are the choices that I usually grab for.

特别是，尽管后续出现了许多新架构，我认为ResNet-50和ResNet-101仍然是真正出色且可靠的选择。如果你想要一个能直接工作而不需要过多调整的东西，这些通常是我的首选。

If you are concerned about computational cost for some reason, then look to some kind of mobile network like ShuffleNet. But in general, you really shouldn't be trying to design your own neural network architectures.

如果你出于某种原因关注计算成本，那么可以考虑某种移动网络，如ShuffleNet。但总的来说，你真的不应该尝试设计自己的神经网络架构。

So that kind of is all the stuff we wanted to cover today, and next time we'll talk about some of the actual software and hardware that we use to actually train these different networks.

这就是我们今天想要涵盖的所有内容，下次我们将讨论一些实际用于训练这些不同网络的软件和硬件。

]