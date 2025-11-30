---
title: 'Lecture9_Notes'
publishDate: 2025-11-29
description: 'TODO'
tags:
  - TODO
language: 'English'
heroImage: { src: './default.jpg', color: '#D58388' }
---

Today we're going to talk about deep learning hardware and software. This will be a very applied practical lecture where we'll see lots of code on the screen and walk through it.

今天我们将讨论深度学习硬件和软件。这将是一堂非常实用的应用课程，我们将在屏幕上看到大量代码并进行详细讲解。

As you'll recall from last lecture, we talked about CNN architectures and saw how the field has progressed from architectures like AlexNet to VGG to ResNet. We saw this huge proliferation of different convolutional neural network architectures that people have used throughout the years.

正如上节课所回顾的，我们讨论了CNN架构，并了解了该领域如何从AlexNet、VGG到ResNet等架构不断发展。我们看到了多年来人们使用的各种卷积神经网络架构的大量涌现。

We talked about the trade-offs of computation and memory and saw how future advancements like VGG and especially ResNet gave us viable regular designs for CNNs that made it very easy to scale up and scale down their sizes.

我们讨论了计算和内存之间的权衡，并了解了像VGG特别是ResNet这样的未来进展如何为我们提供了可行的常规CNN设计，使得调整网络规模变得非常容易。

Now that we have some understanding of the types of architectures that people implement with CNNs, it's useful to think about the actual hardware and software systems on which those architectures will ultimately run.

既然我们对人们使用CNN实现的架构类型有了一定了解，现在有必要思考这些架构最终将运行的实际硬件和软件系统。

First is deep learning hardware. Here's a picture of a computer with interesting components inside - the central processing unit or CPU at the top, and two giant graphics processing units or GPUs that actually take more physical space than the CPU.

首先是深度学习硬件。这是一台计算机的内部图片，包含有趣的组件 - 顶部的中央处理器或CPU，以及两个巨大的图形处理器或GPU，它们实际上比CPU占用更多的物理空间。

When it comes to deep learning, there's a clear winner in the GPU debate between Nvidia and AMD. Nvidia dominates because their software stack for utilizing hardware for general-purpose computing and especially for deep learning is much more advanced than AMD's.

在深度学习领域，Nvidia和AMD之间的GPU之争有一个明确的赢家。Nvidia占据主导地位，因为他们在利用硬件进行通用计算特别是深度学习的软件堆栈比AMD先进得多。

It's interesting to look historically at the trends of computational power of both CPUs and GPUs. This plot shows gigaflops per dollar, where we can see clear trends of increasing computational efficiency over time.

从历史角度观察CPU和GPU计算能力的发展趋势很有趣。这张图表显示了每美元获得的千兆浮点运算次数，我们可以清楚地看到随着时间的推移，计算效率不断提高的趋势。

Both flops per dollar have been increasing over time for both CPUs and GPUs, but GPUs have seen a dramatic reduction in computing cost since around 2012.

随着时间的推移，CPU和GPU的每美元浮点运算性能都在提高，但自2012年左右以来，GPU的计算成本出现了急剧下降。

The divergence in computing cost between CPUs and GPUs became huge from 2006 to 2012. The GTX 580 GPU was used for training AlexNet, which was a breakthrough result in deep learning.

从2006年到2012年，CPU和GPU之间的计算成本差异变得巨大。GTX 580 GPU被用于训练AlexNet，这是深度学习领域的突破性成果。

Even though CPUs have somewhat flattened in their cost of computing, GPUs have been continually accelerating. The period from 2012 until now has seen massive explosion in deep learning due to rising availability of very cheap compute.

尽管CPU的计算成本在一定程度上趋于平稳，但GPU一直在持续加速发展。从2012年至今，由于非常廉价的计算资源日益普及，深度学习经历了巨大的爆发式增长。

Let's drill down into specifics with current CPUs and GPUs. A top-of-the-line consumer CPU like the Ryzen 9 3950X has 16 cores and hits about 4.8 teraflops per second.

让我们深入了解当前CPU和GPU的具体细节。像Ryzen 9 3950X这样的顶级消费级CPU拥有16个核心，每秒可达到约4.8万亿次浮点运算。

Comparing this to the current top-of-the-line consumer GPU, the NVIDIA Titan RTX, we see it is significantly more powerful computationally and can achieve more than three times the total number of floating point operations per second.

将其与当前顶级消费级GPU NVIDIA Titan RTX进行比较，我们发现它在计算能力上明显更强大，每秒可执行的浮点运算总数是前者的三倍以上。

The cartoon picture you should have is that CPUs tend to have fewer but faster and more powerful cores, while GPUs have cores that run at lower clock speeds but there are just a lot more of them.

您应该形成的概念图是：CPU倾向于拥有更少但更快、更强大的核心，而GPU的核心运行时钟频率较低，但数量要多得多。

If we compare 16 cores on the CPU versus 4608 cores on a GPU, you can see that the GPU has many more individual compute elements that let it do more computation overall.

如果我们比较CPU的16个核心与GPU的4608个核心，您可以看到GPU拥有更多的独立计算单元，使其能够执行更多的整体计算。

Diving inside the RTX Titan GPU, we see it's like a mini computer with its own fans, memory modules, and processor. The core computing elements are streaming multi-processors or SMs.

深入RTX Titan GPU内部，我们看到它就像一台微型计算机，拥有自己的风扇、内存模块和处理器。核心计算元素是流多处理器或SMs。

Inside each streaming multi-processor, we find the actual computing elements - 32-bit floating-point cores that perform floating-point arithmetic. Each streaming multi-processor has 64 of these FP32 cores.

在每个流多处理器内部，我们找到了实际的计算元素 - 执行浮点运算的32位浮点核心。每个流多处理器拥有64个这样的FP32核心。

Next to the FP32 cores are tensor cores, which are specialized hardware elements that NVIDIA has introduced for deep learning. These are specifically meant for deep learning computations.

FP32核心旁边是张量核心，这是NVIDIA为深度学习引入的专用硬件元素。这些专门用于深度学习计算。

Tensor cores are specialized hardware that do chunks of matrix multiplication. They can compute A times B plus C for 4x4 matrices in a single clock cycle.

张量核心是执行矩阵乘法块的专用硬件。它们可以在单个时钟周期内计算4x4矩阵的A乘以B加C。

These tensor cores use mixed precision - performing multiplication using 16-bit floating point and addition using 32-bit floating point. This allows them to pack more compute into efficient space.

这些张量核心使用混合精度 - 使用16位浮点数进行乘法运算，使用32位浮点数进行加法运算。这使得它们能够在高效的空间内封装更多计算。

When we consider the tensor core units, the total throughput for this device reaches 130 teraflops per second, which is a massive amount of computation.

当我们考虑张量核心单元时，该设备的总吞吐量达到每秒130万亿次浮点运算，这是巨大的计算量。

GPUs are still dramatically more efficient and have dramatically more computational ability than CPUs, especially when considering the special-purpose tensor core hardware now shipping inside NVIDIA GPUs. All you have to do is flip your input data type to 16-bit. If you have the proper hardware installed and the correct video drivers, then it will automatically accelerate any computation that can be accelerated and utilize the tensor cores.

GPU仍然比CPU效率高得多，计算能力也强得多，特别是考虑到现在NVIDIA GPU中搭载的专用张量核心硬件。您只需将输入数据类型转换为16位。如果您安装了合适的硬件和正确的视频驱动程序，系统会自动加速所有可加速的计算，并利用张量核心。

I should point out that optimizing these models with mixed precision becomes more finicky. There are tricks people use, like deciding which parts of the model to compute in full precision versus mixed precision, and employing optimization techniques to maintain numerical stability at lower precision.

需要指出的是，使用混合精度优化这些模型会变得更加棘手。人们会采用一些技巧，比如决定模型的哪些部分使用全精度计算，哪些使用混合精度计算，并运用优化技术来在低精度下保持数值稳定性。

That said, I think it's worthwhile because tensor cores provide nearly a 10x speedup over the FP32 cores in the GPU.

尽管如此，我认为这是值得的，因为张量核心相比GPU中的FP32核心提供了近10倍的速度提升。

Matrix multiplication is the prototypical example of an operation that is much faster on a GPU compared to a CPU. Each element in the output matrix is an inner product between two large vectors, and this problem is trivially parallelizable because each output element can be computed independently.

矩阵乘法是GPU相比CPU速度大幅提升的典型操作示例。输出矩阵中的每个元素都是两个大向量的内积，这个问题可以轻松并行化，因为每个输出元素都可以独立计算。

This type of problem maps perfectly onto a GPU architecture. You can assign each output element to different streaming multiprocessors or sets of FP32 cores within the GPU, making it an ideal parallel problem for GPU hardware.

这类问题完美契合GPU架构。您可以将每个输出元素分配给GPU中不同的流多处理器或FP32核心组，使其成为GPU硬件的理想并行问题。

In contrast, a traditional single-core CPU model must iteratively compute each output one by one and lacks the ability to parallelize across many computing elements.

相比之下，传统的单核CPU模型必须逐个迭代计算每个输出，并且缺乏跨多个计算元素并行化的能力。

Matrix multiplication is also a perfect example of an operation accelerated by tensor cores. If a computing element in the GPU can compute 4x4 matrix multiplies plus additions in a single clock cycle, you can break the output matrix into 4x4 chunks and distribute them across tensor cores.

矩阵乘法也是张量核心加速操作的完美示例。如果GPU中的计算元素可以在单个时钟周期内计算4x4矩阵乘法及加法，您可以将输出矩阵分解为4x4块，并将其分配给不同的张量核心。

The question is whether 4x4 is the limit for a single operation. For the current generation of tensor cores, that size is hardwired into the hardware.

问题是4x4是否是单次操作的限制。对于当前一代的张量核心，这个尺寸是硬件固定的。

You can emulate different matrix multiplies in software by padding with zeros and splitting. This is why powers of 2 are often most efficient on GPUs, as they minimize wasted computation during splitting.

您可以通过补零和分割在软件中模拟不同的矩阵乘法。这就是为什么2的幂在GPU上通常最有效，因为它们最大限度地减少了分割过程中的计算浪费。

Neural networks often use sizes that are powers of 2 due to this underlying hardware optimization.

由于这种底层硬件优化，神经网络通常使用2的幂作为尺寸。

GPUs are programmed using CUDA, an extension of C/C++ that allows code to run directly on the GPU. While CUDA programming is fun and offers a different way of thinking about problem decomposition, it is beyond the scope of this class.

GPU使用CUDA进行编程，CUDA是C/C++的扩展，允许代码直接在GPU上运行。虽然CUDA编程很有趣，并提供了一种不同的问题分解思维方式，但它超出了本课程的范围。

In practice, deep learning practitioners rarely need to program in CUDA because NVIDIA provides heavily optimized routines for operations like matrix multiplication, convolution, and batch normalization.

实际上，深度学习从业者很少需要编写CUDA代码，因为NVIDIA为矩阵乘法、卷积和批量归一化等操作提供了高度优化的例程。

Most people use PyTorch and rely on these optimized routines rather than implementing everything in CUDA themselves.

大多数人使用PyTorch，并依赖这些优化例程，而不是自己用CUDA实现所有内容。

So far, we have discussed single GPU devices, but there is increasing interest in scaling computation beyond single GPUs.

到目前为止，我们讨论了单个GPU设备，但人们越来越关注将计算扩展到单个GPU之外。

In practice, it is common to use servers with eight GPUs, distribute computation across them, or even stack multiple servers in data centers for large-scale training.

实际上，通常使用配备八个GPU的服务器，将计算分布在这些GPU上，甚至将多个服务器堆叠在数据中心中进行大规模训练。

This creates a hierarchical decomposition from servers to GPUs, to streaming multiprocessors, to tensor cores, with multiple levels of parallel computing hierarchy.

这创建了从服务器到GPU，到流多处理器，再到张量核心的层次分解，具有多个层次的并行计算结构。

For many years, NVIDIA was the only major player in deep learning hardware, but recently Google entered the scene with its TPU.

多年来，NVIDIA一直是深度学习硬件领域唯一的主要参与者，但最近谷歌推出了其TPU加入竞争。

Google's first publicly discussed hardware was the Cloud TPU v2, with about 180 teraflops of compute per board, similar to the latest NVIDIA cards.

谷歌首个公开讨论的硬件是Cloud TPU v2，每块板卡提供约180 teraflops的计算能力，与最新的NVIDIA卡相当。

TPUs also contain specialized hardware for low or mixed precision matrix multiplication in few clock cycles.

TPU也包含专用硬件，可在少数时钟周期内执行低精度或混合精度矩阵乘法。

You cannot buy TPUs but can rent them on Google Cloud. Interestingly, you can use Cloud TPU v2s for free on Colab, though we haven't used them for assignments.

您无法购买TPU，但可以在Google Cloud上租用。有趣的是，您可以在Colab上免费使用Cloud TPU v2，尽管我们未在作业中使用它们。

TPUs truly shine when assembled into TPU pods. A TPU pod can have 64 chips in one machine, providing up to 11.5 petaflops per second of computation.

TPU在组装成TPU Pod时才真正发挥威力。一个TPU Pod可以在一台机器中包含64个芯片，提供高达每秒11.5 petaflops的计算能力。

You can rent a TPU v2 pod on Google Cloud for $384 per hour. The subsequent TPU v3 offers even higher performance, with 420 teraflops per chip and rents for $8 per hour.

您可以在Google Cloud上以每小时384美元的价格租用TPU v2 Pod。后续的TPU v3提供更高性能，每芯片420 teraflops，租金为每小时8美元。

A TPU v3 pod contains 256 devices, delivering over 100 petaflops of compute in one programmable hardware unit. However, you must contact sales for pricing, as it is too expensive to list.

TPU v3 Pod包含256个设备，在一个可编程硬件单元中提供超过100 petaflops的计算能力。但您需要联系销售获取报价，因为价格过高未在网站上列出。

A major caveat with TPUs is that you need to use TensorFlow, Google's deep learning framework. However, recent commits in PyTorch's GitHub suggest possible future TPU support.

TPU的一个主要注意事项是您需要使用TensorFlow，即谷歌的深度学习框架。然而，PyTorch的GitHub上的近期提交表明未来可能支持TPU。

When comparing GPUs for gaming versus deep learning, key differences include memory amount and type. Compute GPUs have more memory to store activations for backpropagation and use high-bandwidth memory for faster data transfer.

比较用于游戏和深度学习的GPU时，关键区别包括内存容量和类型。计算GPU具有更多内存来存储反向传播的激活值，并使用高带宽内存以实现更快的数据传输。

Consumer GPUs typically have less memory and use GDDR6, while compute GPUs use high-bandwidth memory. This is crucial because data transfer speed often limits performance more than computation speed itself.

消费级GPU通常内存较少并使用GDDR6，而计算GPU使用高带宽内存。这一点至关重要，因为数据传输速度通常比计算速度本身更限制性能。

Increased memory bandwidth enhances overall training speed, even if compute speed appears similar on paper.

增加内存带宽可提高整体训练速度，即使计算速度在纸面上看起来相似。

Deep learning software offers many choices. Early frameworks like Caffe, Torch, and Theano came from academic groups, but modern frameworks are increasingly built and maintained by industry giants.

深度学习软件提供了许多选择。早期的框架如Caffe、Torch和Theano来自学术团体，但现代框架越来越多地由行业巨头构建和维护。

Mainstream frameworks today are PyTorch and TensorFlow. These frameworks centralize around computational graphs for forward and backward passes.

当今的主流框架是PyTorch和TensorFlow。这些框架围绕前向传播和反向传播的计算图概念构建。

Key features of deep learning frameworks include: rapid prototyping with common layers and utilities, automatic gradient computation via backpropagation, and efficient execution on GPUs/TPUs.

深度学习框架的关键特性包括：使用通用层和工具进行快速原型设计、通过反向传播自动计算梯度，以及在GPU/TPU上的高效执行。

The ease of running code on GPUs today is a triumph of frameworks like PyTorch and TensorFlow, which have made GPU computing accessible without requiring deep hardware knowledge.

如今在GPU上运行代码的便捷性是PyTorch和TensorFlow等框架的胜利，它们使得无需深入了解硬件知识即可进行GPU计算。

A final caveat is software versions. This class uses PyTorch 1.2, as Colab recently updated from version 1.1. There was no public announcement. There was no release notes. They just silently swapped the PyTorch version on everyone using Colab, and it was a surprise. So I think that actually bit some people on the homework around random seeds, because when PyTorch switched from 1.1 to 1.2, I think the written outputs you get for the fixed random seed actually changed. And I think we saw some confusion on Piazza around that point, because when PyTorch updated to 1.2, we had developed the assignment on 1.1 and the random seeds changed a little bit. So I apologize for that, but it just happened silently.

最后一个注意事项是软件版本。本课程使用PyTorch 1.2，因为Colab最近从1.1版本更新。没有公开公告，也没有发布说明。他们只是悄悄地为所有使用Colab的用户更换了PyTorch版本，这令人意外。我认为这实际上在涉及随机种子的作业中给一些人带来了麻烦，因为当PyTorch从1.1版本切换到1.2版本时，对于固定随机种子得到的输出结果实际上发生了变化。我们在Piazza上看到了一些关于这一点的困惑，因为当PyTorch更新到1.2时，我们是在1.1版本上开发的作业，随机种子发生了一些变化。我为此道歉，但这一切都是悄悄发生的。

Also a big caveat is that if you're looking at older PyTorch code, especially pre-1.0, there was a lot of breaking changes in the PyTorch API, especially between 0.4 and 1.0. So I think PyTorch 1.0 has been relatively stable in API for about the last year or so. But if you're out there in the wild on the internet looking at random GitHub repos, you'll still see a lot of really old outdated PyTorch code that might not work under the more stable releases today. So that's just a caveat to watch out for.

还有一个重要的注意事项是，如果你查看旧的PyTorch代码，特别是1.0之前的版本，PyTorch API有很多破坏性的变更，尤其是在0.4和1.0之间。我认为PyTorch 1.0在过去一年左右的时间里API相对稳定。但如果你在互联网上随意浏览GitHub仓库，仍然会看到很多非常陈旧过时的PyTorch代码，这些代码在如今更稳定的版本下可能无法工作。所以这是一个需要注意的地方。

So I think the way I think about PyTorch is that there's kind of three different levels of abstraction that it gives you for building your neural network models. The lowest level of abstraction is the idea of a tensor, and that's the level of abstraction that you've been working with so far in all the homework assignments, where a PyTorch tensor is just an array, kind of like a multi-dimensional array that runs on GPUs, and you can do operations on it. It's basically like NumPy but runs on the GPU if you're familiar with other libraries.

我认为PyTorch提供了三种不同层次的抽象来构建神经网络模型。最低层次的抽象是张量的概念，这也是你们迄今为止在所有作业中一直使用的抽象层次，PyTorch张量就是一个数组，类似于运行在GPU上的多维数组，你可以对其执行操作。它基本上就像NumPy，但如果你熟悉其他库，它运行在GPU上。

But PyTorch gives you two other levels. And in the first couple homework assignments, you've seen how you can use only this tensor API for building neural network models and computing gradients and performing gradient descent. And you can do all of that stuff using just this tensor API. But PyTorch gives us a couple other higher levels of abstraction for thinking about building neural network models.

但PyTorch还提供了另外两个层次。在前几个作业中，你们已经看到了如何仅使用这个张量API来构建神经网络模型、计算梯度并执行梯度下降。你可以仅使用这个张量API完成所有这些工作。但PyTorch为我们提供了另外几个更高层次的抽象来思考如何构建神经网络模型。

The second is this autograd level for automatic gradients. And this is the heart of PyTorch that lets us automatically build up computational graphs and backpropagate through them in order to compute gradients. And finally, there's yet another level of abstraction called module level, where a module is now something like an object-oriented neural network layer that actually stores inside of itself state, like learnable weights. And by composing modules, it makes it very easy to build big neural network models.

第二个是用于自动梯度的autograd层次。这是PyTorch的核心，它让我们能够自动构建计算图并通过它们进行反向传播以计算梯度。最后，还有另一个称为模块层次的抽象，其中模块现在类似于面向对象的神经网络层，它实际上在自身内部存储状态，比如可学习的权重。通过组合模块，可以非常容易地构建大型神经网络模型。

So basically, the way this breaks down is that on the first three assignments, we constrained you to only use this tensor interface in PyTorch. But starting on assignments 4, 5 & 6, then you'll be using the full generality of these different layers of abstraction to perform different types of computation.

基本上，这样的安排是：在前三个作业中，我们限制你们只使用PyTorch中的这个张量接口。但从作业4、5和6开始，你们将使用这些不同抽象层次的全部功能来执行不同类型的计算。

So as kind of a running example throughout the rest of the lecture, we're gonna use this example of training a two-layer fully connected network with ReLU activations and an L2 loss function as kind of a running example to see how this works both in different frameworks and in different layers of abstraction.

因此，作为本讲座剩余部分的一个贯穿始终的示例，我们将使用训练一个带有ReLU激活函数和L2损失函数的两层全连接网络作为示例，来了解这在不同框架和不同抽象层次中是如何工作的。

So something like this code on the screen now is something that you should be very familiar with by this point in the class. This is basically training a neural network using only these tensor operations. So at the top, we're kind of creating random tensors for our data and our weights. Here we're doing a forward pass where it's a matrix multiply and ReLU and another matrix multiply and an L2 loss function. And then here is computing the backward pass where we manually compute the gradients of the loss with respect to the weights. And then here is a gradient descent step where we actually update the weights. And this type of code should be very familiar to you at this point in the semester.

现在屏幕上像这样的代码，你们在课程的这个阶段应该非常熟悉了。这基本上就是仅使用这些张量操作来训练神经网络。在顶部，我们为数据和权重创建随机张量。这里我们进行前向传播，包括矩阵乘法、ReLU、另一个矩阵乘法和L2损失函数。然后这里计算反向传播，我们手动计算损失相对于权重的梯度。接着是梯度下降步骤，我们实际更新权重。这种类型的代码在本学期这个阶段你们应该非常熟悉。

And again, you know that in order to move and do all this computation on GPU, all you need to do is change this device argument that the tensors are placed on, and then all of your compute transparently runs on GPU.

同样，你们知道，为了在GPU上移动并执行所有这些计算，你们需要做的就是更改张量所在的设备参数，然后你们的所有计算就会透明地在GPU上运行。

Now we can move to the next level of abstraction, which is autograd. And first observation is that the code is quite a lot shorter, so that's hopefully a good thing. And the idea with autograd is that tensor was so far we've constructed tensors in various ways, but whenever you construct a tensor in PyTorch, there's always another flag you can set called `requires_grad`. And all you have to do to make PyTorch build computational graphs for you is set `requires_grad=True` on the tensors that you want to build computational graphs for.

现在我们可以转向下一个抽象层次，即autograd。首先观察到的是代码短了很多，所以这应该是件好事。autograd的想法是，到目前为止我们以各种方式构造张量，但每当你在PyTorch中构造一个张量时，总可以设置另一个名为`requires_grad`的标志。要让PyTorch为你构建计算图，你只需在希望为其构建计算图的张量上设置`requires_grad=True`。

So here is an example of now training the exact same fully connected neural network model but using the autograd level of abstraction in PyTorch. So here we can see that we're still initializing random tensors for the weights and the data. But now for our weight matrices w1 and w2, when we construct them, we passed this additional flag `requires_grad=True` when constructing the tensor. And this tells PyTorch that these tensors are ones that we want it to track and build computational graphs for us.

这里有一个示例，现在训练完全相同的全连接神经网络模型，但使用PyTorch中的autograd抽象层次。这里我们可以看到，我们仍然为权重和数据初始化随机张量。但现在对于我们的权重矩阵w1和w2，在构造它们时，我们在构造张量时传递了这个额外的标志`requires_grad=True`。这告诉PyTorch，这些张量是我们希望它跟踪并为我们构建计算图的张量。

And now our forward pass is actually very much abbreviated because we no longer need to explicitly keep track of all these intermediate results in the computation ourselves, because any intermediate results that will be needed for backpropagation will be stored by PyTorch automatically somewhere in this computational graph that is being built up in the background for us.

现在我们的前向传播实际上大大简化了，因为我们不再需要自己显式地跟踪计算中的所有中间结果，因为任何反向传播所需的中间结果都会被PyTorch自动存储在后台为我们构建的计算图中的某个地方。

So these lines are then computing the forward pass. It's the exact same sequence of operations that you saw in the non-autograd example. The only difference is that now we can just throw away these intermediates because we don't need to explicitly store them. And now after we've computed the forward pass and the loss, then we have this very magical line called `loss.backward()`. And this one line of code is telling PyTorch to traverse the graph for us and compute all the gradients with respect to all of the weights that were taken as input.

所以这些行随后计算前向传播。这与你在非autograd示例中看到的操作序列完全相同。唯一的区别是，现在我们可以直接丢弃这些中间结果，因为我们不需要显式存储它们。在我们计算了前向传播和损失之后，我们有了这非常神奇的一行代码`loss.backward()`。这一行代码告诉PyTorch为我们遍历图，并计算相对于所有作为输入的权重的所有梯度。

So it's walked us through this a little bit more concretely. The way that this works is that whenever PyTorch performs a primitive operation on tensors, it checks whether any of the inputs to that operation have this `requires_grad=True` flag set. And if any of the inputs to a PyTorch operator have `requires_grad=True`, that means then PyTorch will in the background start building up a computational graph data structure that represents that computation.

让我们更具体地了解一下这个过程。其工作原理是，每当PyTorch对张量执行一个基本操作时，它会检查该操作的任何输入是否设置了`requires_grad=True`标志。如果PyTorch操作符的任何输入具有`requires_grad=True`，那就意味着PyTorch将在后台开始构建一个表示该计算的计算图数据结构。

So when this first function of this matrix multiplication between X and w1 runs, then because w1 has `requires_grad=True`, then PyTorch will silently start building up this computational graph in the background that has the inputs X and w1, has a node for matrix multiplication, and then has an output which is this tensor that is stored in the graph, but it's not given a name because we don't have any need for it in our code.

因此，当这个在X和w1之间进行矩阵乘法的第一个函数运行时，由于w1具有`requires_grad=True`，PyTorch将在后台悄悄地开始构建这个计算图，该图包含输入X和w1，有一个矩阵乘法节点，然后有一个输出，即存储在图中的这个张量，但它没有被赋予名称，因为我们在代码中不需要它。

Then when we perform the clamp operation, then the output of this... so the other rule is that when PyTorch performs an operation on any input that has `requires_grad=True`, then the output also has `requires_grad=True`. So it all kind of works out recursively.

然后当我们执行clamp操作时，这个操作的输出... 另一个规则是，当PyTorch对任何具有`requires_grad=True`的输入执行操作时，输出也具有`requires_grad=True`。所以这一切都是以递归方式工作的。

So then the next thing is that when this `.clamped` line executes, then it's operating on this anonymous output tensor that also has `requires_grad=True`. So this line will build up a new chunk of the computational graph. When this matrix multiply line runs, then we build up more computational graph and now rope in w2. When we perform the subtraction, we get more computational graph. This power adds more to the graph, and finally this sum performed is the final part of the graph.

接下来，当这个`.clamped`行执行时，它是在操作这个同样具有`requires_grad=True`的匿名输出张量。所以这一行将构建计算图的一个新部分。当这个矩阵乘法行运行时，我们构建了更多的计算图，现在涉及w2。当我们执行减法时，我们得到更多的计算图。这个操作向图中添加了更多内容，最后执行的这个求和是图的最后部分。

So then you basically every time PyTorch performs a primitive operator, it's just adding more on to whatever computational graph has been built so far. And now when you call `loss.backward()` more concretely, you can see that we're calling `loss.backward()` and loss is kind of the end thing, which is a scalar at the very end of the computational graph.

所以你基本上每次PyTorch执行一个基本操作符时，它只是在已经构建的计算图上添加更多内容。现在当你更具体地调用`loss.backward()`时，你可以看到我们正在调用`loss.backward()`，而损失是最终的东西，它是计算图最末端的一个标量。

And then when you call `loss.backward()`, a couple things happen. One is that PyTorch searches through the graph to find any leaf nodes that have `requires_grad=True`. So the leaf nodes are the inputs of the thing, all of the inputs to the graph that would be x, w1, w2, and y in this case. And now it searches for any of those inputs that have `requires_grad=True` set. In this case, w1 and w2 have `requires_grad=True` set, which means that now it will automatically do some graph search to find out the path between that output node loss and all of the input nodes that have `requires_grad=True` set.

然后当你调用`loss.backward()`时，会发生几件事。一是PyTorch遍历图以找到任何具有`requires_grad=True`的叶节点。叶节点是图的输入，在这种情况下，就是x, w1, w2和y。现在它搜索那些设置了`requires_grad=True`的输入。在这种情况下，w1和w2设置了`requires_grad=True`，这意味着现在它将自动进行一些图搜索，以找出输出节点损失与所有设置了`requires_grad=True`的输入节点之间的路径。

And then after finding these paths between the output and the inputs that need gradient, it will actually perform backpropagation and backpropagation through each of these nodes one at a time. And then after backpropagation finishes, it actually will throw away that graph and just free all the memory that was used for that computational graph. And it will store the gradients that were computed for the inputs in these in `w1.grad` and `w2.grad`, which will now be new tensors that contain the gradients computed during backpropagation.

在找到输出和需要梯度的输入之间的这些路径之后，它实际上将执行反向传播，并逐个节点地进行反向传播。在反向传播完成后，它实际上会丢弃那个图，并释放用于该计算图的所有内存。它会将计算出的输入梯度存储在`w1.grad`和`w2.grad`中，这些现在将是包含在反向传播期间计算的梯度的新张量。

And now that this `loss.backward()` has magically computed all the gradients for us, then now we can use this `w1.grad` and `w2.grad` to perform our gradient descent step. And then a very important step and a very common source of errors is that you need to explicitly set those gradients to zero after you perform your gradient descent step.

现在这个`loss.backward()`已经神奇地为我们计算了所有梯度，那么现在我们可以使用这个`w1.grad`和`w2.grad`来执行我们的梯度下降步骤。然后一个非常重要的步骤，也是一个非常常见的错误来源是，在执行梯度下降步骤后，你需要显式地将这些梯度设置为零。

The idea is that when we perform backpropagation, if some of your tensors might already have some gradients hanging around in their associated tensors, and when you call `loss.backward()`, it actually doesn't overwrite the existing gradients. Instead, it computes the new gradients and adds them to whatever old gradients were already there. So that means that if the normal thing is we want to compute fresh gradients at every iteration, which means you need to explicitly zero the gradients of your tensors on every iteration.

其原理是，当我们执行反向传播时，如果你的某些张量可能已经在其关联的张量中有一些梯度存在，那么当你调用`loss.backward()`时，它实际上不会覆盖现有的梯度。相反，它会计算新的梯度并将它们添加到任何已有的旧梯度上。这意味着，通常我们在每次迭代时都想计算新的梯度，这就意味着你需要在每次迭代时显式地将张量的梯度归零。

Um, and I'm embarrassed to admit I've made this bug more times than I want to say. But it's easy to forget these lines, and sometimes things will still train even if you forget to zero the gradient, so that can be very, very difficult to debug. I think it's maybe a bit of a design flaw in PyTorch. I think it maybe would have been better to overwrite by default and accumulate if you want to opt into that, but this is the API that we have to live with now because it's 1.0 and it's supposed to be stable.

呃，我很不好意思地承认，我犯这个错误的次数多得我不想说。但很容易忘记这些行，而且有时即使你忘记将梯度归零，模型仍然会训练，所以这可能非常非常难以调试。我认为这可能是PyTorch的一个设计缺陷。我认为也许默认覆盖，如果你想选择累积再另行设置会更好，但这就是我们现在必须接受的API，因为它是1.0版本并且应该是稳定的。

And now another bit of weirdness that you see in this code is that these gradient updates actually are scoped under this `with torch.no_grad():` context manager. And what this means is that any operations that fall under a no_grad context manager mean that we're telling PyTorch to just don't track, don't build computational graph for any operations that happen within this context, even if those tensors did indeed have `requires_grad=True`. So this context manager just overrides whatever the `requires_grad` flag was on individual tensors.

现在你在这段代码中看到的另一个奇怪之处是，这些梯度更新实际上是在这个`with torch.no_grad():`上下文管理器的范围内进行的。这意味着，任何在no_grad上下文管理器下的操作都意味着我们告诉PyTorch不要跟踪、不要为在此上下文中发生的任何操作构建计算图，即使那些张量确实具有`requires_grad=True`。所以这个上下文管理器会覆盖各个张量上原有的`requires_grad`标志。

And the reason for this is that we don't want to backpropagate through our SGD steps. That would cause memory to leak from iteration to iteration, and it would just be very confusing, and it would not be the SGD algorithm that we mean to implement. So whenever you're doing an update rule or zeroing gradients or anything that is outside the computational graph, then you want to scope it under one of these no_grad context managers.

这样做的原因是我们不希望通过我们的SGD步骤进行反向传播。那会导致内存迭代间泄漏，并且会非常令人困惑，而且它也不会是我们想要实现的SGD算法。所以，每当你执行更新规则或梯度归零或任何在计算图之外的操作时，你都希望将其放在这些no_grad上下文管理器之一的范围内。

Now PyTorch autograd is also extensible. In this example, we've kind of written out forward pass by calling these basic PyTorch operators.

现在PyTorch的autograd也是可扩展的。在这个例子中，我们通过调用这些基本的PyTorch操作符写出了前向传播。

Uh, yeah, I was there. The question was, are the gradients calculated numerically or analytically? Well, they use the backpropagation algorithm that we've talked about so far in this class, which is not... wait, it's not finite differences. That's what we usually think of when we say numeric gradients, right? It's not really either. It's a different thing. Right, numeric gradients is usually something like a finite differences approximation using the limit definition of the derivative, and backpropagation is not that.

呃，是的，我在听。问题是，梯度是数值计算还是解析计算的？嗯，它们使用的是我们在这门课中到目前为止讨论的反向传播算法，它不是... 等等，它不是有限差分。那通常是我们说数值梯度时想到的，对吧？它也不是真正的那种。它是不同的东西。对，数值梯度通常像是使用导数极限定义的有限差分近似，而反向传播不是那样。

When you say symbolic differentiation, that usually means you build up some symbolic data structure and then manipulate those structures symbolically to compute some new expression for the gradients, and backpropagation is also not quite that. Backpropagation is instead this structured application of the chain rule in order to compute it, sort of use the chain rule in the right way at every point in the computation in order to compute our gradients. So it looks a little bit like symbolic differentiation, but it's not quite traditional symbolic differentiation either. But it's the backpropagation algorithm that we've been covering so far in this class.

当你说符号微分时，那通常意味着你构建一些符号数据结构，然后符号化地操作这些结构来计算梯度的某个新表达式，而反向传播也不完全是那样。反向传播反而是链式法则的结构化应用，以便计算梯度，有点像在计算的每个点上以正确的方式使用链式法则来计算我们的梯度。所以它看起来有点像符号微分，但也不完全是传统的符号微分。但它是我们在这门课中到目前为止一直在讨论的反向传播算法。

Now in this example, we've implemented our forward pass of the network using only these basic operators in PyTorch, like matrix multiplication, clamping, subtraction, etc. And it would be kind of a pain if you had to do all of your computation that way. Thankfully, PyTorch integrates very nicely with sort of basic abstract software abstractions in Python.

在这个例子中，我们仅使用PyTorch中的这些基本操作符实现了网络的前向传播，比如矩阵乘法、clamp、减法等等。如果你必须用这种方式完成所有计算，那会有点痛苦。幸运的是，PyTorch与Python中那种基本的软件抽象集成得非常好。

So for example, you can define a Python function which inputs PyTorch tensors and outputs PyTorch tensors, and then use that Python function inside the forward pass of your neural network, and this will work just fine. So in this example, we're defining a sigmoid function and using this mathematical definition of the sigmoid in order to define a new function that we can then use in the forward pass of our network.

例如，你可以定义一个输入PyTorch张量并输出PyTorch张量的Python函数，然后在神经网络的前向传播中使用该Python函数，这将正常工作。所以在这个例子中，我们定义了一个sigmoid函数，并使用sigmoid的这个数学定义来定义一个新函数，然后我们可以在网络的前向传播中使用它。

But it's important to point out that when you use Python functions to perform modular computation inside neural networks or modularly structure your neural networks, then they're still at the computational graph level. The computational graph does not know about Python functions. Then really the way this works is that when you call the Python function, then each primitive PyTorch operation that happens inside of your Python function will just keep on adding to the overall computational graph.

但需要指出的是，当你使用Python函数在神经网络内部执行模块化计算或以模块化方式构建神经网络时，它们仍然处于计算图层面。计算图并不知道Python函数。实际上，其工作方式是，当你调用Python函数时，发生在你的Python函数内部的每个基本PyTorch操作只会继续添加到整体计算图中。

So that means that another way to put that is that defining things using Python functions lets your code look sort of nice and modular and structured, but then every time your code runs, there will just be some giant computational graph that is kind of like a flattened version of all of the operations that you've performed as your program traced through all the different functions that you called.

这意味着，换一种说法是，使用Python函数定义东西可以让你的代码看起来漂亮、模块化和结构化，但每次你的代码运行时，只会有一个巨大的计算图，它有点像你的程序追踪所有你调用的不同函数时所执行的所有操作的扁平化版本。

So in particular, when this sigmoid function runs, then you can see that it will just add on more and more nodes to the computational graph for each of these primitive operations that we use to implement the sigmoid function. So we've got this minus 1, this exponential, this plus 1, and this division.

具体来说，当这个sigmoid函数运行时，你可以看到，它会为我们用来实现sigmoid函数的每个基本操作向计算图添加越来越多的节点。所以我们有这个减1、这个指数、这个加1和这个除法。

And you may know that the sigmoid function is actually... if it's that, then when computing gradients through the sigmoid function, it will backpropagate through each of these primitive nodes one by one using normal backpropagation. But you may know that computing gradients through the sigmoid function in this way will actually be very numerically unstable. And actually, if you implement the backward pass of sigmoid by backpropagating through this graph, then you'll very frequently get NaNs or not-a-number overflow errors or infinities or other bad numerical things in your computation.

你可能知道sigmoid函数实际上是... 如果是这样，那么当通过sigmoid函数计算梯度时，它将使用正常的反向传播逐个通过这些基本节点进行反向传播。但你可能知道，以这种方式通过sigmoid函数计算梯度实际上在数值上会非常不稳定。实际上，如果你通过在这个图上进行反向传播来实现sigmoid的反向传播，那么你在计算中会经常遇到NaN（非数字）、溢出错误、无穷大或其他不良的数值问题。

So PyTorch also gives you... and if you'll recall from a couple lectures ago, we saw that for the particular case of the sigmoid function, the local gradient of the sigmoid function has this very nice mathematical form that you can work out on paper, which is that backpropagating through the sigmoid as an entire unit unto itself then we get this very nice expression for the local gradient of the sigmoid function.

所以PyTorch也给你提供了... 如果你还记得几节课前的内容，我们看到对于sigmoid函数这个特定情况，sigmoid函数的局部梯度有一个非常简洁的数学形式，你可以在纸上推导出来，即通过将sigmoid作为一个整体单元进行反向传播，我们得到了sigmoid函数局部梯度的这个非常简洁的表达式。

And in cases like this where we actually have some special knowledge about the way that gradients should be computed, then we can... then PyTorch gives us another layer of abstraction to implement this, and that is to implement a new autograd function that is very similar to these little forward and backward modular APIs that we've talked about previously.

在这种情况下，我们实际上对梯度的计算方式有一些特殊的知识，那么我们可以... PyTorch为我们提供了另一个抽象层次来实现这一点，那就是实现一个新的autograd函数，它非常类似于我们之前讨论的那些小型前向和反向模块化API。

So you can, by defining a new autograd function, this lets you define new primitive operations that will give rise to just one node in the computational graph. So when defining a new autograd function, you can see that we define a forward which computes the forward pass, but then we also define an explicit backward function which is receiving the upstream gradients, computing local gradient, and returning the downstream gradients.

所以你可以通过定义一个新的autograd函数，这让你可以定义新的基本操作，这些操作将在计算图中只产生一个节点。因此，在定义一个新的autograd函数时，你可以看到我们定义了一个计算前向传播的forward，但然后我们还定义了一个显式的backward函数，它接收上游梯度，计算局部梯度，并返回下游梯度。

And now if we were to use this autograd function version of sigmoid in our computational graph, then this would give rise to only a single node in the computational graph. And in order to backpropagate through it, it would just use this backward function that we implemented for this one.

现在，如果我们在计算图中使用这个autograd函数版本的sigmoid，那么这将在计算图中只产生一个节点。并且为了通过它进行反向传播，它将只使用我们为这个节点实现的backward函数。

In order to backpropagate through it, it would just use this backward function that we implemented for this one node.

为了通过它进行反向传播，它只会使用我们为这个节点实现的backward函数。

But in practice, it's very nice that PyTorch gives you this flexibility and this freedom to very easily implement new fundamental elements of computational graphs.

但在实践中，PyTorch提供了这种灵活性和自由度，让你能够非常容易地实现计算图的新基本元素，这一点非常好。

But in practice this is less common to see; I think it's much more common in practice to just use Python functions to implement most things.

但在实践中这种情况比较少见；我认为在实践中更常见的是直接使用Python函数来实现大多数功能。

But sometimes you need to actually use the mechanism on the right to define your own new primitive operators.

但有时你确实需要使用右侧的机制来定义自己的新原始运算符。

Of course then the next layer of abstraction in PyTorch is the nn module, which gives us some kind of object-oriented API for building up neural network models.

当然，PyTorch中的下一个抽象层是nn模块，它为我们提供了一种面向对象的API来构建神经网络模型。

And now that this becomes very expressive very quickly, so here you can see that nn module gives us this object-oriented API.

这使得表达力迅速增强，所以在这里你可以看到nn模块为我们提供了这种面向对象的API。

We're now in torch.nn and Sequential is some container object that is meant to maintain a sequence of layer objects.

我们现在在torch.nn中，Sequential是一个容器对象，用于维护一系列层对象。

Then within each of the layers we provide it layer objects like a linear layer that stores the learnable parameters like weight and bias as attributes inside of that object.

然后在每个层中，我们为其提供层对象，比如线性层，它将可学习参数（如权重和偏置）作为属性存储在该对象内部。

This means that we can now define the structure of our neural network model by just composing these layer objects and sticking them into containers.

这意味着我们现在可以通过组合这些层对象并将它们放入容器中来定义神经网络模型的结构。

Now when we compute the forward pass, all we need to do is pass the data to this object that we built and that computes the forward pass for us.

现在当我们计算前向传播时，我们只需要将数据传递给我们构建的这个对象，它就会为我们计算前向传播。

Then torch.nn also gives you common loss functions so you don't need to implement those anymore from scratch.

然后torch.nn还提供了常见的损失函数，因此你不再需要从头开始实现这些函数。

Then we can still call loss.backward() to compute gradients and then the gradient descent step now looks very similar.

然后我们仍然可以调用loss.backward()来计算梯度，然后梯度下降步骤现在看起来非常相似。

We iterate over all the learnable parameters in this model and update them using our gradient descent rule.

我们遍历模型中所有可学习参数，并使用我们的梯度下降规则更新它们。

Of course it's kind of annoying to implement your own gradient descent rules all the time, so PyTorch also provides optimizer objects that implement common gradient descent rules.

当然，总是自己实现梯度下降规则有点烦人，因此PyTorch还提供了实现常见梯度下降规则的优化器对象。

So here you can see that we can build an optimizer object that encapsulates the Adam optimization algorithm and we pass it the model parameters that we'd like to optimize as well as the hyperparameters like the learning rate.

所以在这里你可以看到，我们可以构建一个封装了Adam优化算法的优化器对象，并将我们想要优化的模型参数以及学习率等超参数传递给它。

Now in our training loop, all we need after computing gradients by calling loss.backward() is to call optimizer.step() and that will automatically make the gradient step for us.

现在在我们的训练循环中，通过调用loss.backward()计算梯度后，我们只需要调用optimizer.step()，它就会自动为我们执行梯度步骤。

Of course we also need to remember to explicitly zero the gradients after each step, and this is again a common source of bugs.

当然，我们还需要记住在每个步骤后显式地将梯度归零，这又是一个常见的错误来源。

Another very common thing to do in PyTorch is actually define your own new nn modules.

在PyTorch中另一个非常常见的做法是实际定义你自己的新nn模块。

In this example our model had a structure that kind of made sense as a sequential sequence of layer objects.

在这个例子中，我们的模型具有一个结构，该结构作为层对象的顺序序列是合理的。

But in more general situations that aren't just sequences, then you'll need to define your own module subclass that represents your computation.

但在更一般的情况下，不仅仅是序列，那么你需要定义自己的模块子类来表示你的计算。

So here we're again defining a two-layer neural network with a non-linearity by defining our own custom subclass of the module class.

所以在这里，我们通过定义模块类的自定义子类，再次定义了一个带有非线性的两层神经网络。

In particular you can see that here the initializer of our custom subclass takes in the sizes of the hidden layers and the sizes of the outputs that we need.

具体来说，你可以看到这里我们自定义子类的初始化器接收隐藏层的大小和我们需要的输出大小。

Then it actually constructs layer objects that are these linear objects that are constructed in the initializer and then assigned as member variables inside our own module object.

然后它实际上构建了层对象，这些是在初始化器中构建的线性对象，然后作为成员变量分配给我们自己的模块对象。

Now in this forward pass we can use any of those module objects that were built in the initializer to perform our computation.

现在在这个前向传播中，我们可以使用在初始化器中构建的任何这些模块对象来执行我们的计算。

So you can see that we compute the forward pass by passing the input to the first layer object, then clamping the output with ReLU, and passing the output of that to the next layer object to predict the final scores.

所以你可以看到，我们通过将输入传递给第一个层对象来计算前向传播，然后用ReLU钳制输出，并将该输出传递给下一个层对象以预测最终分数。

Then the rest of the training loop looks very similar.

然后训练循环的其余部分看起来非常相似。

I should also point out that a very common pattern is that you'll see people mix and match modules and sequential containers or nest custom modules inside of other custom modules.

我还应该指出，一个非常常见的模式是，你会看到人们混合匹配模块和顺序容器，或者在其他自定义模块中嵌套自定义模块。

This is a way to very powerfully and very quickly and very easily build up very complex neural network architectures.

这是一种非常强大、快速且轻松地构建非常复杂的神经网络架构的方法。

So here's a kind of a little toy example that gives you a hint of what you can do with this.

所以这里有一个小玩具示例，让你大致了解你可以用它做什么。

Remember in the last lecture we talked about how many neural network modules are built up of these homogeneous blocks, right? Something like a residual network has this residual block design, and then the overall network is repeating that same block design over and over again.

还记得上一讲我们讨论了多少神经网络模块是由这些同质块构建的吗？比如残差网络具有这种残差块设计，然后整个网络一遍又一遍地重复相同的块设计。

So in situations like that, it would be very common to define a custom module subclass for the block that you want to use, and then to build your model by instantiating multiple instances of that subclass and stacking them together in maybe a sequential container.

所以在这样的情况下，为你想要使用的块定义一个自定义模块子类会非常常见，然后通过实例化该子类的多个实例并将它们堆叠在一起（可能在顺序容器中）来构建你的模型。

So here in this example we've defined a kind of a weird little neural network block structure that I don't think is actually a good idea but it fits on the slide.

所以在这个例子中，我们定义了一种有点奇怪的小神经网络块结构，我认为这实际上不是一个好主意，但它适合放在幻灯片上。

Here we were imagining a little kind of block design that computes two linear layers in parallel.

在这里我们设想了一种小型块设计，它并行计算两个线性层。

Our input goes through one fully connected layer on the left, then a separate fully connected layer on the right with different weights and biases, and then the outputs of those two fully connected layers are multiplied element-wise.

我们的输入通过左侧的一个全连接层，然后通过右侧具有不同权重和偏置的另一个全连接层，然后这两个全连接层的输出进行逐元素相乘。

The results of those multiplication then goes through a non-linearity.

这些乘法的结果然后通过一个非线性函数。

I suspect that this would actually not perform very well at all, but it's kind of a little instructive example that you can look at.

我怀疑这实际上性能不会很好，但这是一个有点指导意义的例子，你可以看看。

Then you can see that it's very easy to implement this idea by defining our own module subclass that again in the initializer we define two separate nn.linear objects.

然后你可以看到，通过定义我们自己的模块子类来实现这个想法非常容易，在初始化器中我们再次定义了两个独立的nn.linear对象。

Then in the forward pass we use those two linear objects to compute two parallel outputs and then do this element-wise multiplication between them.

然后在前向传播中，我们使用这两个线性对象计算两个并行输出，然后在它们之间进行这种逐元素乘法。

Then you can see that when we build the model, we build a sequential container that contains multiple instantiations of this little parallel block design.

然后你可以看到，当我们构建模型时，我们构建了一个顺序容器，其中包含这个小并行块设计的多个实例。

This is a paradigm that you'll see very commonly in PyTorch code that kind of mix and match your own custom module subclasses with sequential containers.

这是一种在PyTorch代码中非常常见的范式，即将你自己的自定义模块子类与顺序容器混合匹配。

PyTorch also gives you some nice mechanisms for loading data that can automatically build mini-batches and iterate through datasets and all that kind of good stuff for you.

PyTorch还提供了一些很好的数据加载机制，可以自动构建小批量数据、遍历数据集等所有这些好东西。

PyTorch also provides a bunch of pre-trained models that you can literally get all these pre-trained models in one line.

PyTorch还提供了一堆预训练模型，你实际上可以在一行代码中获得所有这些预训练模型。

All you have to do is import torchvision, then if you want a pre-trained AlexNet you just say: alexnet = torchvision.models.alexnet(pretrained=True).

你只需要导入torchvision，然后如果你想要一个预训练的AlexNet，你只需要说：alexnet = torchvision.models.alexnet(pretrained=True)。

It will go out on the internet and download the pre-trained weights of the model automatically and cache them on disk for you and then return those weights for you to use right away in your code.

它会访问互联网，自动下载模型的预训练权重，并将其缓存到磁盘上，然后返回这些权重供你立即在代码中使用。

So this makes it very easy for you to quickly use pre-trained models to build up your own different designs in different architectures.

所以这使得你可以非常容易地快速使用预训练模型，在不同的架构中构建自己的不同设计。

Now a big point of the design in PyTorch is this idea of a dynamic computation graph.

现在PyTorch设计的一个主要点是动态计算图的概念。

What this means is that every time we run the forward pass, we build up a new graph data structure, and then when we call loss.backward() we throw away that graph data structure.

这意味着每次我们运行前向传播时，都会构建一个新的图数据结构，然后当我们调用loss.backward()时，我们会丢弃该图数据结构。

The next time we run our iteration we're gonna build up another new graph data structure from scratch and then again throw it away when we call loss.backward() again.

下一次我们运行迭代时，我们将从头开始构建另一个新的图数据结构，然后当我们再次调用loss.backward()时再次丢弃它。

This maybe seems a little bit inefficient; it seems kind of silly to just build up the graph data structure on every iteration and then throw it away at every iteration again just to rebuild the exact same thing at the next iteration.

这看起来可能有点低效；在每次迭代时构建图数据结构，然后在每次迭代时再次丢弃它，仅仅是为了在下次迭代时重建完全相同的东西，这似乎有点愚蠢。

But the benefit of dynamic computation graphs is that it lets you use normal regular Python control flow to control the flow of information through your neural network models.

但动态计算图的好处是，它允许你使用普通的Python控制流来控制信息在神经网络模型中的流动。

That lets you do very strange and funny and crazy things using very simple and intuitive code.

这让你能够使用非常简单直观的代码做非常奇怪、有趣和疯狂的事情。

So here's an example that again doesn't make sense and I really don't recommend anyone use in practice.

所以这里有一个例子，再次说明这没有意义，我真的不建议任何人在实践中使用。

Here what we're doing is we're actually initializing two different weight matrices for the second layer of our fully connected network: this is w2_a and w2_b.

这里我们做的是为全连接网络的第二层实际初始化两个不同的权重矩阵：这是w2_a和w2_b。

Now the choice of which weight matrix we use at every iteration of training is going to depend on the loss at the previous iteration of training.

现在，在每次训练迭代中使用哪个权重矩阵的选择将取决于上一次训练迭代的损失。

Again this is probably a terrible idea and I don't encourage anyone to write models that do this.

再次强调，这可能是一个糟糕的主意，我不鼓励任何人编写这样的模型。

But if you have such a crazy idea that you want to implement, you can see that it's very easy to do in PyTorch just by using normal regular Python control flow.

但如果你有这样一个疯狂的想法想要实现，你可以看到在PyTorch中通过使用普通的Python控制流很容易做到。

Now in this way on one iteration we might build up a computational graph that involves w2_a, and then we'll throw it away and the next iteration will build up a new graph that has w2_b instead.

现在通过这种方式，在一次迭代中我们可能构建一个涉及w2_a的计算图，然后我们会丢弃它，下一次迭代将构建一个包含w2_b的新图。

The main benefit of these dynamic computational graphs is that it lets the structure of the computational graph be determined by normal regular Python control flow in cases where you want to maybe perform slightly different operations on different iterations.

这些动态计算图的主要好处是，它允许计算图的结构由普通的Python控制流决定，在你可能希望在不同迭代中执行略微不同操作的情况下。

Yeah question? The question was what about TensorFlow? Well I think we'll hopefully get to that.

有问题是吗？问题是关于TensorFlow的？嗯，我想我们希望能讲到那个。

The quick answer is that yes, it does now as of yesterday.

简短的回答是，是的，从昨天开始它确实支持了。

So the big alternative to dynamic computation graphs is this notion of a static computation graph.

所以动态计算图的主要替代方案是静态计算图的概念。

Here what we want to do is have a two-stage procedure: one stage where we build up a graph and then fix the graph for all time, and then in the second stage we iterate through and reuse the same computational graph many times.

这里我们想要做的是一个两阶段过程：一个阶段我们构建图并永久固定该图，然后在第二阶段我们多次迭代并重复使用相同的计算图。

Actually in PyTorch this is a kind of a new functionality in more recent versions of PyTorch.

实际上在PyTorch中，这是较新版本PyTorch中的一种新功能。

PyTorch now gives you the ability to use static computation graphs using the JIT or just-in-time compiler.

PyTorch现在让你能够使用JIT（即时编译器）来使用静态计算图。

So what this means is that we can define our model as a Python function that takes input tensors as input and returns tensors as output.

所以这意味着我们可以将我们的模型定义为一个Python函数，该函数将输入张量作为输入并返回张量作为输出。

Then there's this very magical line called: graph = torch.jit.trace(model, example_input)

然后有这样一行非常神奇的代码：graph = torch.jit.trace(model, example_input)

What this very magical line does is it introspects the Python source code for that function, it parses the abstract syntax tree of the Python source code of that function, and then it builds a computational graph for you automatically after traversing the source code of your Python function and then returns that thing to you as a graph object that you can call.

这行非常神奇的代码所做的是：它内省该函数的Python源代码，解析该函数Python源代码的抽象语法树，然后在遍历你的Python函数的源代码后自动为你构建一个计算图，然后将其作为可调用的图对象返回给你。

In particular in this model function we see it has this conditional statement for this funny thing we were doing.

特别是在这个模型函数中，我们看到它为我们正在做的这个有趣的事情包含这个条件语句。

Now the graph that is built for you has to now include some node in the graph that captures that conditional statement.

现在为你构建的图必须在图中包含某个节点来捕获该条件语句。

Now every time in every forward pass we'll simply reuse that same graph object.

现在在每次前向传播中，我们只需重复使用同一个图对象。

This can be even more succinct: we can just add a @torch.jit.script annotation to your code and the compilation process will happen for you automatically when the function is first imported into Python.

这甚至可以更简洁：我们只需在你的代码中添加一个@torch.jit.script注解，当函数首次导入Python时，编译过程将自动为你进行。

Now one big benefit of the static computation graphs over dynamic is the potential for optimization.

现在静态计算图相对于动态的一个主要好处是优化的潜力。

You can imagine that maybe the graph you write is some long sequence of convolutions and batch norms and ReLUs and things like that.

你可以想象，也许你写的图是一些长的卷积、批归一化和ReLU等序列。

If we have a static computation graph, you can imagine using some compiler techniques to try to rewrite that graph in a way that might be more efficient computationally.

如果我们有一个静态计算图，你可以想象使用一些编译器技术来尝试以计算上更高效的方式重写该图。

For example, you might want to fuse some operations like convolution and ReLU and actually rewrite the graph in some non-trivial way that would make the computations be faster.

例如，你可能想要融合一些操作，如卷积和ReLU，并以某种非平凡的方式实际重写该图，从而使计算更快。

With a static computation graph you can amortize the cost of computing those optimizations or those graph rewrites and just do it once at the beginning of the program and then enjoy the speed ups for the rest.

使用静态计算图，你可以分摊计算这些优化或图重写的成本，只需在程序开始时执行一次，然后在其余时间享受加速。

If every iteration where it might not make sense to separately reoptimize the graph at every iteration that might be too slow.

如果在每次迭代中单独重新优化图可能没有意义，那样可能太慢。

Another big benefit of static computation graphs is this idea of serialization.

静态计算图的另一个主要好处是序列化的概念。

What happens in practice with machine learning models is that people will want to train their models in some very expressive programming language like Python, but once they have their models trained they would like to deploy those models in environments that do not depend on Python.

机器学习模型在实践中发生的情况是，人们希望在一些非常具有表现力的编程语言（如Python）中训练他们的模型，但一旦他们的模型训练好，他们希望将这些模型部署在不依赖Python的环境中。

For example, what you can do with the static computation graph is train up your model in Python, then export the static computation graph as a data structure on disk, and then load up that static computation graph object into a C++ API to then run your trained model in a way that does not depend on the Python interpreter anymore.

例如，你可以使用静态计算图在Python中训练你的模型，然后将静态计算图作为数据结构导出到磁盘上，然后将该静态计算图对象加载到C++ API中，从而以不再依赖Python解释器的方式运行你训练好的模型。

This is again whereas with dynamic computation graphs, the code that builds the graph and the code that executes the graph is all kind of intertwined.

这再次说明，而对于动态计算图，构建图的代码和执行图的代码都是相互交织的。

So if you want to use this thing in production, you'll probably need to depend on a Python interpreter.

所以如果你想在生产中使用这个东西，你可能需要依赖Python解释器。

This was a big motivation for all of these tech companies really building in strong static graph functionality to their deep learning frameworks.

这是所有这些科技公司在其深度学习框架中真正构建强大静态图功能的一个主要动机。

A big downside of static computation graphs is debugging.

静态计算图的一个主要缺点是调试。

If any of you have used TensorFlow before, you see that it's sometimes very difficult to debug because there ends up with a lot of indirection between the Python code that you write and the code that eventually ends up getting executed.

如果你们中有人以前使用过TensorFlow，你会发现它有时很难调试，因为你编写的Python代码和最终执行的代码之间最终存在很多间接性。

So you can sometimes get very difficult very confusing error messages; it can be very hard to know what's going on and what broke and very difficult to profile performance or other things like that.

所以你有时会得到非常困难、非常令人困惑的错误消息；很难知道发生了什么以及什么出了问题，并且很难分析性能或其他类似的事情。

Whereas with a dynamic computation graph, the code you write is pretty much the code that runs, so they tend to be much easier to debug.

而对于动态计算图，你编写的代码几乎就是运行的代码，因此它们往往更容易调试。

Now some applications of dynamic computation graphs are things where the structure of a model depends in some way on the input to the model.

现在动态计算图的一些应用是模型的结构在某种程度上依赖于模型输入的情况。

A canonical example of this is a recurrent neural network where maybe the number of time steps—and we'll talk about these in detail in a later lecture—but the idea is that we input a sequence and now the number of time steps in the model is equal to the number of steps in the sequence.

一个典型的例子是循环神经网络，其中可能的时间步数——我们将在后面的讲座中详细讨论这些——但想法是我们输入一个序列，现在模型中的时间步数等于序列中的步数。

We want to perform different amounts of computation depending on the length of the sequence that gets passed into the model.

我们希望根据传入模型的序列长度执行不同数量的计算。

There are also examples like recursive neural networks that maybe get used in NLP.

还有一些例子，比如在NLP中可能使用的递归神经网络。

So now the input to the model is some kind of semantic parse of a sentence, and now the structure—the way that the neural network model performs its computation—is going to vary dynamically based on the structure of the parse tree that gets passed as input.

所以现在模型的输入是句子的某种语义解析，现在结构——神经网络模型执行其计算的方式——将根据作为输入传递的解析树的结构动态变化。

So these are—by the way I don't expect you to know understand the details of these—these are just two meant to give you some flavor of where dynamic computation graphs can really shine.

所以这些——顺便说一句，我不指望你们了解这些细节——这两个例子只是为了让你了解动态计算图真正可以发挥作用的领域。

Here's an example from Johnson et al. two years ago that is another example of dynamic computation graphs.

这是Johnson等人两年前的一个例子，是动态计算图的另一个例子。

Here we have actually one part of the model predicting what structure the second part of the model should use.

这里我们实际上有模型的一部分预测模型的第二部分应该使用什么结构。

The first part of the model actually predicts some kind of program, and then that program is then implemented by the second part of the neural network model.

模型的第一部分实际上预测某种程序，然后该程序由神经网络模型的第二部分实现。

So in order to do this, not only does the computation of the neural network model depend on the input, the computation that the model performs depends on output of a previous part of the model.

因此，为了做到这一点，不仅神经网络模型的计算依赖于输入，模型执行的计算还依赖于模型前一部分的输出。

In order to implement models like this, I use PyTorch because you need this heavy dependence on dynamic computation graphs to perform crazy models like this.

为了实现这样的模型，我使用PyTorch，因为你需要严重依赖动态计算图来执行这样疯狂的模型。

I think there's a lot of open area for people to try out really crazy ideas once we have this ability to build dynamic computation graphs very efficiently.

我认为一旦我们具备了非常高效地构建动态计算图的能力，就有很多开放领域供人们尝试真正疯狂的想法。

So that gives us just a couple minutes to talk about TensorFlow.

所以我们只有几分钟时间来谈谈TensorFlow。

TensorFlow I mentioned has actually been going through kind of a schism in the last year or so, and the kind of the classic version of TensorFlow is TensorFlow 1.0.

我提到的TensorFlow实际上在过去一年左右经历了一种分裂，经典的TensorFlow版本是TensorFlow 1.0。

Actually yesterday was the final release or the release candidate of the final release of TensorFlow 1.0.

实际上昨天是TensorFlow 1.0最终版本的最终发布或发布候选版。

TensorFlow 1.0 actually used static computation graphs by default everywhere, and later versions—more recently come to some of the later versions of TensorFlow 1.0 added some options to use dynamic computation graphs—but the main mode for doing computation in TensorFlow 1.0 was actually static computation graphs.

TensorFlow 1.0实际上在所有地方默认使用静态计算图，后来的版本——最近的一些TensorFlow 1.0后期版本添加了一些使用动态计算图的选项——但TensorFlow 1.0中进行计算的主要模式实际上是静态计算图。

Now TensorFlow 2.0 was actually released this week on Monday, and in TensorFlow 2.0 dynamic computation graphs are the default and there is instead option to use static computation graphs.

现在TensorFlow 2.0实际上在本周星期一发布，在TensorFlow 2.0中，动态计算图是默认设置，反而有使用静态计算图的选项。

I think right now is a very dangerous time to read TensorFlow code on the internet because you'll see some horrible horrible mix of 1.0 and 2.0, and sometimes even when you google bits of documentation they link between each other and it's just a complete mess.

我认为现在是在互联网上阅读TensorFlow代码非常危险的时期，因为你会看到一些可怕的1.0和2.0混合，有时甚至当你谷歌一些文档时，它们相互链接，完全是一团糟。

So I think maybe you should be careful, be very careful about reading TensorFlow code over the next couple of months, but hopefully in a year or so maybe we will settle on the 2.0 API.

所以我想也许你应该小心，在接下来的几个月里阅读TensorFlow代码要非常小心，但希望在大约一年后，我们可能会稳定在2.0 API上。

So kind of to give you a flavor of this classic TensorFlow 1.0, then it had the structure kind of like this that I don't want to walk through the details here.

所以为了让你体验一下经典的TensorFlow 1.0，它的结构有点像这样，我不想在这里详细讲解。

But in TensorFlow 1.0 your code always had two big chunks: one at the top is this stage where you define your computational graph, and then at the bottom is where you actually repeatedly run your computational graph.

但在TensorFlow 1.0中，你的代码总是有两个大块：顶部是定义计算图的阶段，然后底部是你实际重复运行计算图的地方。

This is kind of the classic way that TensorFlow code was written, and this can be very difficult to debug now because what can happen is that in the piece of your code where you build the computational graph, maybe you have a shape error or a data type error or you're mismatching the API in some way and you pass a tensor that doesn't make sense to some function. Well then you don't actually get the error in the line of your code that caused the problem; instead, you only get an error message when you actually try to run the graph.

这是TensorFlow代码编写的经典方式，但现在调试起来可能非常困难。因为可能发生的情况是：在构建计算图的代码部分，你可能会出现形状错误、数据类型错误，或者以某种方式不匹配API，并向某个函数传递了一个没有意义的张量。但你不会在导致问题的代码行中直接收到错误提示，而是只有在实际运行计算图时才会收到错误信息。

So what that means is that when you have an error, then either your stack trace will actually point to this mysterious session.run line and you'll get a stack trace deep into the guts of TensorFlow, and the thing that caused the problem was maybe you had a shape error on one of the lines 10 out of 10 lines earlier here. That can make debugging this classic TensorFlow code very challenging.

这意味着当你遇到错误时，你的堆栈跟踪要么会指向这个神秘的session.run行，你会得到一个深入TensorFlow内部的堆栈跟踪，而问题的根源可能是前面10行中的某一行出现了形状错误。这使得调试这种经典的TensorFlow代码变得极具挑战性。

But now in TensorFlow 2.0, it seems like they basically copied PyTorch API to some extent because the dynamic graph API in PyTorch had been very popular, very widely used, very easy to work with and very easy to debug.

但现在在TensorFlow 2.0中，他们似乎在某种程度上借鉴了PyTorch的API，因为PyTorch的动态图API非常受欢迎，应用广泛，易于使用且易于调试。

So now if you look at this two-layer network example in TensorFlow 2.0, it actually looks a lot like PyTorch. Then you can see that here at the top we're kind of defining some TensorFlow tensors, not PyTorch tensors, that we're gonna use to store our weights in our data.

所以现在如果你看TensorFlow 2.0中的这个两层网络示例，它实际上看起来很像PyTorch。你可以看到在顶部我们定义了一些TensorFlow张量（不是PyTorch张量），我们将用它们来存储数据中的权重。

Now remember in PyTorch, in order to track gradients, we needed to set requires_grad equals true. Well the equivalent in TensorFlow is to wrap them in a tf.Variable object. It turns out PyTorch 0.4 used to wrap things in a PyTorch Variable object, but that was annoying API and it deprecated it. Maybe TensorFlow will catch up in 3.0.

记得在PyTorch中，为了跟踪梯度，我们需要设置requires_grad等于true。而在TensorFlow中的等效做法是将它们包装在tf.Variable对象中。实际上PyTorch 0.4曾经将东西包装在PyTorch Variable对象中，但那个API很烦人并且已被弃用。也许TensorFlow会在3.0版本中跟进。

But then once we've done that, then in TensorFlow we can tell TensorFlow that we wanted to track gradients for us by scoping our computation under this tf.GradientTape object, and that means that any operations that happen under this tf.GradientTape scope are going to build up a computational graph much like the way that PyTorch builds up computational graphs when it encounters tensors with requires_grad true.

但一旦我们完成了这个步骤，在TensorFlow中，我们可以通过将计算作用域放在这个tf.GradientTape对象下来告诉TensorFlow我们想要跟踪梯度。这意味着在这个tf.GradientTape作用域下发生的任何操作都将构建计算图，很像PyTorch在遇到requires_grad为true的张量时构建计算图的方式。

And now to compute our gradients after we exit the tf.GradientTape scope and call tape.gradient of loss with respect to the parameters, and that's a very nice line that lets you remember what you're taking derivatives of what with respect to, and that returns us new TensorFlow tensors containing the gradients.

现在要计算梯度，在我们退出tf.GradientTape作用域后，调用tape.gradient计算损失相对于参数的梯度。这是一行很好的代码，让你清楚地记得你在对什么求关于什么的导数，它会返回包含梯度的新TensorFlow张量。

And then we can perform our gradient descent step as usual. So this should look very similar to this kind of autograd version of PyTorch code that we've seen actually.

然后我们可以像往常一样执行梯度下降步骤。所以这实际上应该看起来很像我们见过的PyTorch代码的autograd版本。

And now TensorFlow 2.0 also offers a very similar annotation-based thing for static computation graphs that is very similar to the torch script annotation that we saw in PyTorch.

现在TensorFlow 2.0也为静态计算图提供了非常相似的基于注解的方法，这与我们在PyTorch中看到的torch script注解非常相似。

So it's kind of nice to see these two frameworks kind of converging on some similar ideas when in the past they used to be very different.

所以看到这两个框架在某些相似的想法上趋同是很好的，而过去它们曾经非常不同。

But now we can use static computation graphs in TensorFlow 2.0 by defining our step function as some Python function that takes the inputs and then annotating it with this tf.function annotation, and again this will perform a lot of magic and introspect the Python source code and build up a computational graph for us by inspecting the Python source code.

但现在我们可以在TensorFlow 2.0中使用静态计算图，通过将我们的步骤函数定义为接受输入的Python函数，然后用这个tf.function注解来标注它。这又会执行很多魔法操作，内省Python源代码，并通过检查Python源代码为我们构建计算图。

One thing to note about the TensorFlow version of these things is that in TensorFlow actually the gradient computation and the update actually can be part of your static graph as well, so that kind of folds the entire everything you do in one training iteration is now folded into the computational graph.

关于TensorFlow版本需要注意的一点是，在TensorFlow中，梯度计算和更新实际上也可以成为静态图的一部分，这样你在一次训练迭代中所做的所有事情现在都被折叠到计算图中。

So then in the training loop all you need to do is call the step function, and now inside the step function it will compute the forward pass, compute the gradients and make a gradient step for us.

因此在训练循环中，你只需要调用步骤函数，现在在步骤函数内部，它将计算前向传播，计算梯度并为我们执行梯度步骤。

TensorFlow 2.0 also has standardized on Keras, this package called Keras that gives a high level API for working with neural network models that is very similar to that that is similar in some ways to the nn package in PyTorch.

TensorFlow 2.0还在Keras上实现了标准化，这个名为Keras的包提供了一个用于处理神经网络模型的高级API，在某些方面与PyTorch中的nn包相似。

So now here's an equivalent of training this two-layered neural network in TensorFlow 2.0 where we're using Keras. So you can see that there is again now an object-oriented API that lets us build up our models as some sequence of layer objects.

所以现在这是在TensorFlow 2.0中使用Keras训练这个两层神经网络的等效代码。你可以看到现在又有了一个面向对象的API，让我们可以将模型构建为一系列层对象。

Now it also defines loss functions and optimizer objects for us, and now our training loop we need to just call compute the forward pass of the model, compute gradients and use the optimizer to make our gradient step.

现在它还为我们定义了损失函数和优化器对象，而在我们的训练循环中，我们只需要调用计算模型的前向传播，计算梯度并使用优化器执行梯度步骤。

And then it turns out there's a slightly different bit of API that we can use that actually has the optimizer call backward for us, so this lets you then in very similar this ends up looking very similar to this training loop using nn and autograd in PyTorch.

然后实际上有一个稍微不同的API我们可以使用，它实际上让优化器为我们调用backward，所以这最终让你看起来非常类似于在PyTorch中使用nn和autograd的训练循环。

But again that lets you build up very powerful complex neural network models with very small amounts of code.

但这再次让你能够用很少的代码构建非常强大的复杂神经网络模型。

Another very nice thing that I should mention about TensorFlow is the TensorBoard functionality. So this is basically a web server that lets you track statistics about your model and it's really great and a lot of people use it and a lot of people love it.

关于TensorFlow，我应该提到的另一个非常好的东西是TensorBoard功能。这基本上是一个Web服务器，让你可以跟踪模型的统计信息，它真的很棒，很多人使用它，很多人喜欢它。

Basically what you do is you add some logging code inside your forward pass of your model that says like log I'm at iteration 10 my loss was 25, the gradient or this I'm a teapot one my accuracy was 50% or any other statistics that you want to track over the course of training.

基本上你要做的是在模型的前向传播中添加一些日志代码，比如记录"我在第10次迭代，我的损失是25"，或者"我是茶壶一号，我的准确率是50%"，或者任何你想在训练过程中跟踪的其他统计信息。

And after you just import add this little bit of logging into your training loop, then you can start up the TensorBoard server and get all these beautiful graphs to visualize the results of your models.

在你将这点日志记录添加到训练循环后，你就可以启动TensorBoard服务器并获得所有这些漂亮的图表来可视化模型的结果。

And TensorBoard was so widely loved that the PyTorch folks actually provided API to let PyTorch talk to TensorBoard, so that's one really nice thing that a lot of people are using actually in both frameworks now.

TensorBoard如此受欢迎，以至于PyTorch团队实际上提供了API让PyTorch可以与TensorBoard通信，所以这是一个非常好的东西，现在很多人都在两个框架中使用它。

So then kind of my summary of PyTorch versus TensorFlow: I think you should have guessed that PyTorch is my personal favorite because we've used it for all the homework assignments in the class and I talked about it first before TensorFlow.

那么我对PyTorch与TensorFlow的总结是：我想你应该已经猜到了PyTorch是我个人的最爱，因为我们在课程的所有作业中都使用了它，而且我在讲TensorFlow之前先讲了它。

But I think one of the big downsides about PyTorch right now are that it cannot use TPUs, although maybe that's coming. So today if you want to use TPUs to accelerate your machine learning models, you have to use TensorFlow.

但我认为PyTorch目前的一个大缺点是它不能使用TPU，尽管可能即将支持。所以今天如果你想使用TPU来加速机器学习模型，你必须使用TensorFlow。

Another big downside about PyTorch right now is that I think it's not very easy to run PyTorch models on mobile devices. I think that the torch script, the jetting mechanisms in PyTorch have made it fairly easy to deploy PyTorch models in some non-mobile contexts, but if you want to deploy your trained models on an iPhone or something, then it's actually quite difficult to do in PyTorch right now.

PyTorch的另一个大缺点是我认为在移动设备上运行PyTorch模型不太容易。我认为PyTorch中的torch script和JIT机制使得在一些非移动环境中部署PyTorch模型相当容易，但如果你想在iPhone等设备上部署训练好的模型，目前在PyTorch中实际上相当困难。

So in TensorFlow, TensorFlow 1.0 is very confusing, very static graphs by default, very confusing API is pretty messy, difficult to debug, but this is where you'll find if you look at any TensorFlow code online right now, it'll mostly be TensorFlow 1.0 code.

在TensorFlow方面，TensorFlow 1.0非常令人困惑，默认使用静态图，API非常混乱，难以调试。但如果你现在在线查看任何TensorFlow代码，你会发现大部分都是TensorFlow 1.0代码。

I think TensorFlow 2.0 actually looks quite nice, but the jury is kind of still out. It's very new and we'll see whether or not this ends up getting adoption or being smoother way to develop models in TensorFlow. So I'm hoping that TensorFlow 2.0 will be great, but we'll see.

我认为TensorFlow 2.0实际上看起来相当不错，但还有待观察。它非常新，我们将看看它最终是否会被广泛采用，或者是否是在TensorFlow中开发模型的更顺畅的方式。所以我希望TensorFlow 2.0会很棒，但我们拭目以待。

So then to kind of summarize today, we talked about these three different bits of hardware: we talked about CPUs, GPUs and TPUs. We talked about the software that may in takeaways, the main takeaways are static versus dynamic graphs and PyTorch versus TensorFlow.

那么总结今天的内容，我们讨论了这三种不同的硬件：我们讨论了CPU、GPU和TPU。我们讨论了软件方面的要点，主要收获是静态图与动态图以及PyTorch与TensorFlow的比较。

So with all of that in mind, come back next time and we'll talk about some nuts and bolts details about getting your neural models to converge.

考虑到所有这些内容，下次回来时，我们将讨论一些关于让神经网络模型收敛的具体细节。