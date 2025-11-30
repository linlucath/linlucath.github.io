---
title: 'Lecture10_Notes'
publishDate: 2025-11-29
description: 'TODO'
tags:
  - TODO
language: 'English'
heroImage: { src: './default.jpg', color: '#D58388' }
---

Okay, welcome back to Lecture 10. We made it to double digits. That's very exciting. So today we're gonna be talking about lots of tips and tricks for how you actually go about training neural networks in practice.

好的，欢迎回到第十讲。我们达到了两位数里程碑，这非常令人兴奋。今天我们将讨论大量实用技巧，关于如何在实际中训练神经网络。

Last time we left off, we talked about the hardware and software of deep learning. We talked about different types of hardware that you run these things on like CPUs and GPUs and TPUs. We also talked about different software systems that you can use for implementing these networks. In particular, we talked about the difference between static graphs and dynamic computational graphs, and we talked about some of the trade-offs of both PyTorch and TensorFlow.

上次课程我们讨论了深度学习的硬件和软件。我们介绍了运行这些系统的不同硬件类型，如CPU、GPU和TPU。我们还探讨了可用于实现这些网络的不同软件系统。特别地，我们讨论了静态图与动态计算图的区别，并分析了PyTorch和TensorFlow各自的优缺点。

So now at this point, we've pretty much seen all of the stuff that you need to know to train neural networks. But it turns out there's still a lot of little bits and pieces that you need to actually be super effective in your ability to train networks.

到目前为止，我们已经基本涵盖了训练神经网络所需的所有知识。但实际上，要真正高效地训练网络，仍有许多细节需要掌握。

I like to try to break this up into three different categories of things that you need to know about. One is the one-time setup at the beginning before you start the training process. That's where you need to choose the architecture, the activation functions. You need to do a lot of stuff before you go and hit that train button.

我想将其分为三个需要了解的不同类别。首先是开始训练过程之前的一次性设置。这包括选择架构、激活函数等。在按下训练按钮之前，你需要完成大量准备工作。

Then once you begin training, there are certain things that you might need to do during the process of optimization, like schedule your learning rates or scale up to many machines or things like that. And then after you're done training, you might need to do some extra stuff on top of your trained networks, like model ensembles or chance for learning.

开始训练后，在优化过程中可能需要执行某些操作，例如调整学习率或扩展到多台机器等。训练完成后，可能还需要对已训练网络进行额外处理，比如模型集成或机会学习。

Over the course of today's lecture and Wednesday's lecture, we're going to walk through a lot of these little nitty-gritty details about how you actually go about training neural networks in practice. So today let's start off by talking about activation functions.

在今天和周三的课程中，我们将详细探讨这些关于实际训练神经网络的具体细节。今天我们先从激活函数开始讨论。

You'll recall that in our little model of an artificial neuron, we always have to have an activation function. We always have some kind of linear function coming in that collects the inputs from the neurons in the previous layer. Then those get summed and multiplied by your weight matrix and passed through some kind of nonlinear activation function before being passed on to the next layer.

回想一下，在我们的人工神经元小模型中，总是需要有一个激活函数。首先会有一个线性函数收集来自前一层神经元的输入，这些输入经过权重矩阵相乘和求和后，会通过某种非线性激活函数，然后传递到下一层。

As we recall, having some nonlinear activation function in our networks was absolutely critical for their processing ability. Because if we remove the activation function, then all of our linear operations just collapse onto a single linear layer. So the presence of one of these activation functions was absolutely critical in the construction of our neural networks.

我们记得，网络中拥有非线性激活函数对其处理能力至关重要。因为如果移除激活函数，所有线性操作就会坍缩成单个线性层。因此，这些激活函数的存在对于神经网络的构建绝对关键。

We saw that there's this big zoo of activation functions, but we kind of left off. We didn't really talk much about these different types of activation functions and their trade-offs when we last saw this slide. So today I want to talk in more detail about some of the pros and cons of these different activation functions and other considerations that go into choosing or constructing activation functions for neural networks.

我们看到激活函数种类繁多，但上次看到这张幻灯片时，我们并没有深入讨论这些不同类型激活函数及其权衡。今天我想更详细地探讨这些不同激活函数的优缺点，以及选择或构建神经网络激活函数时的其他考量因素。

Probably one of the most classic activation functions that have been used in neural network research going back several decades is the sigmoid activation function. Sigmoid because it has the sort of S curved shape. This was a popular activation function because it has this interpretation as a probability.

可能几十年来神经网络研究中最经典的激活函数之一就是sigmoid激活函数。之所以称为sigmoid是因为它具有S形曲线。这是一个流行的激活函数，因为它可以被解释为概率。

So you can think about neural networks in that at each of the neurons, it's either on or off, and maybe we want to have some value between zero and one that represents the probability of that feature being present. So the sigmoid activation function has this nice interpretation as the probability for the presence or absence of a boolean variable.

你可以这样理解神经网络：在每个神经元处，它要么激活要么不激活，我们可能希望有一个介于0和1之间的值来表示该特征存在的概率。因此，sigmoid激活函数可以很好地解释为布尔变量存在与否的概率。

It also has this interpretation as the firing rates of a neuron. If you recall these biological neurons, they received signals from other incoming neurons and then fire off signals at some rate. But the rate at which they fired off was nonlinearly dependent on the total rates of all the inputs coming in, and the sigmoid function is a simple way to model that kind of nonlinear dependence on firing rates.

它也可以被解释为神经元的放电频率。如果你回想一下生物神经元，它们接收来自其他传入神经元的信号，然后以某种频率发出信号。但它们放电的频率非线性地依赖于所有输入的总频率，而sigmoid函数是模拟这种对放电频率非线性依赖的简单方法。

So these are a couple reasons why classically the sigmoid nonlinearity had been very popular. But there are several reasons why it's actually not such a great nonlinearity in practice.

这些是sigmoid非线性在传统上非常流行的几个原因。但实际上有几个原因表明它在实践中并不是那么理想。

One problem is that it has these flat regimes at the beginning and the end. These saturated regimes with zero gradient effectively kill the gradient and make it very difficult to train networks in a very robust way.

一个问题是在函数两端存在平坦区域。这些饱和区域的梯度为零，实际上会扼杀梯度，使得难以以稳健的方式训练网络。

To think about this, what happens with the sigmoid function when X is maybe very very small like minus 10 or minus 100? Well in that case, it will be in this far left regime of the sigmoid nonlinearity, so the local gradient will be very very close to zero.

考虑一下，当X非常小（比如-10或-100）时，sigmoid函数会发生什么？在这种情况下，它将处于sigmoid非线性的最左侧区域，因此局部梯度将非常接近零。

That means that all of our weight updates will also be very very close to zero. Because we're going to take our upstream gradients, multiply that by the local gradients to produce these downstream gradients. At that local gradient, the effect is a value very very close to zero.

这意味着我们所有的权重更新也将非常接近零。因为我们将获取上游梯度，乘以局部梯度来产生这些下游梯度。在该局部梯度处，效果是一个非常接近零的值。

That means that no matter what the upstream gradients were, the downstream gradients will also be values that are very very close to zero. This will have the effect of making learning very slow because now all of the weight updates onto our weight matrices will be very low.

这意味着无论上游梯度是多少，下游梯度也将是非常接近零的值。这会导致学习速度非常慢，因为现在所有权重矩阵上的权重更新都会非常低。

It will also give very problematic training dynamics once we move to deep networks. Suppose that we've got a network that's maybe a hundred layers deep, and then we immediately kill the gradient at some layer. Then we'll basically have no signal to train gradients at any of the lower layers.

一旦我们转向深度网络，这还会导致非常棘手的训练动态。假设我们有一个可能一百层深的网络，然后我们在某一层立即扼杀了梯度。那么基本上我们将没有任何信号来训练任何较低层的梯度。

This problem happens both when X is very very small like minus ten as well as very very large like plus ten. The only regime where learning can proceed is if somehow the activation is somewhere within this sweet spot near x equals 0 where the sigmoid function behaves somewhat linearly.

这个问题在X非常小（如-10）和非常大（如+10）时都会发生。只有当激活值恰好处于x等于0附近的理想区域时，学习才能进行，因为在那里sigmoid函数表现得相对线性。

So that's the first major problem with the sigmoid activation function: these flat regimes are going to kill the gradient and make learning very challenging.

这就是sigmoid激活函数的第一个主要问题：这些平坦区域会消除梯度，使得学习变得非常困难。

A second problem with the sigmoid non-linearity is that its outputs are not zero-centered. You can see clearly that the outputs for the sigmoid are all positive because it's all above the x-axis.

sigmoid非线性的第二个问题是其输出不以零为中心。你可以清楚地看到sigmoid的输出都是正的，因为它全部位于x轴上方。

To think about why this property of not having zero-centered outputs is problematic, let's consider what happens when the inputs to one of our neurons are always positive. Remember here's our little diagram of one neuron inside our neural network, and we're zooming in on just one of these things.

为了理解为什么非零中心输出这个特性会带来问题，让我们考虑当神经元的输入始终为正时会发生什么。请记住这是我们神经网络中单个神经元的简化图示，我们正在放大观察其中一个单元。

Now suppose that we're building a multi-layer neural network where at every layer we use a sigmoid non-linearity. That means that the inputs to this layer will also be the result of applying a sigmoid function to the previous layer, which means in particular that all of the inputs X_i to this layer will all be positive.

现在假设我们正在构建一个多层神经网络，其中每一层都使用sigmoid非线性。这意味着该层的输入也是前一层应用sigmoid函数的结果，这特别意味着该层的所有输入X_i都将为正。

Now what can we say about the gradient? Given the fact that all of the X_i that are inputs to this layer are going to be positive, then what can we say about the gradient of the loss with respect to the W_i?

现在关于梯度我们能说什么？考虑到该层所有输入X_i都为正，那么关于损失相对于W_i的梯度我们能得出什么结论？

Remember that in order to compute the gradient of the loss with respect to the W_i, we take the local gradient and multiply by the upstream gradient. Now the local gradient is always going to be positive because the local gradient of W_i is just going to be X_i, and X_i is positive, which means that the local gradients will all be positive.

请记住，为了计算损失相对于W_i的梯度，我们需要取局部梯度并乘以上游梯度。现在局部梯度将始终为正，因为W_i的局部梯度就是X_i，而X_i是正的，这意味着所有局部梯度都将为正。

But then we multiply this by the upstream gradient, which could be positive or negative. But the upstream gradient is just a scalar, and if the upstream gradient is positive, that means that all of the gradients of loss with respect to W_i will all be positive, and similarly if the upstream gradient is negative, that means that all of the gradients of the loss with respect to W_i will be negative.

但随后我们将其乘以上游梯度，上游梯度可能为正或负。但上游梯度只是一个标量，如果上游梯度为正，意味着所有损失相对于W_i的梯度都将为正；同样，如果上游梯度为负，意味着所有损失相对于W_i的梯度都将为负。

So what that means is that all of the gradients with respect to W_i are going to have the same sign. This seems like kind of a bad property for learning.

这意味着所有相对于W_i的梯度都将具有相同的符号。这对于学习来说似乎是一个不利的特性。

For example, suppose that this means that it could be very difficult to make gradient descent steps that reach certain values of the weights that we might want to, because of this constraint that the gradients are all going to be positive or all going to be negative.

例如，假设这意味着由于梯度全部为正或全部为负的限制，可能很难通过梯度下降步骤达到我们期望的权重值。

As a pictorial example of why this might be a problem, suppose that we can have this kind of cartoon picture on the right. Here this picture is maybe a plot of W1 and W2, and we imagine that our initial value for the weights W is the origin, and maybe the value of the weights that we want to get to in order to minimize the loss is somewhere down to the bottom right.

作为说明这可能是个问题的图示例子，假设我们可以在右侧看到这种卡通图片。这里这张图可能是W1和W2的坐标图，我们想象权重W的初始值在原点，而为了最小化损失我们想要达到的权重值在右下方的某个位置。

Now in order to move from the origin to the bottom right, we need to take some steps where we want to take positive steps along W1 and negative steps along W2. But with this constraint that the gradients of the loss with respect to the weights are always going to have the same sign, there's no way that we can take steps that align in that quadrant.

现在为了从原点移动到右下方，我们需要采取一些步骤，即沿着W1方向正向移动，沿着W2方向负向移动。但由于损失相对于权重的梯度总是具有相同符号的限制，我们无法在该象限内沿此方向移动。

So the only possible way for a gradient descent procedure to make progress toward that direction is to have this very awkward zigzagging pattern, where it kind of moves up where all the gradients are positive and then moves back down to the left where all the gradients are negative, and then moves up again. That's kind of a very awkward zigzaggy pattern.

因此梯度下降过程向该方向前进的唯一可能方式是采用这种非常笨拙的锯齿形模式，即先向所有梯度为正的方向向上移动，然后向所有梯度为负的方向向左下方移动，然后再向上移动。这是一种非常笨拙的锯齿形模式。

By the way, this maybe doesn't look so bad in two dimensions, but as we scale to weights with thousands or millions of dimensions, this property is going to be very, very, very bad. Because now if we have a weight matrix with D dimensions, then if you partition up all of the possible signs of all of the elements of that weight matrix, there's going to be 2^D sort of quadrants or orthants in that high-dimensional weight space.

顺便说一下，在二维空间中这可能看起来并不那么糟糕，但当我们扩展到具有数千或数百万维度的权重时，这个特性将变得非常非常糟糕。因为现在如果我们有一个D维的权重矩阵，那么如果你划分该权重矩阵所有元素的所有可能符号，在这个高维权重空间中将会出现2^D个象限或超象限。

Under this constraint that they all have to be positive or negative, that means that any of our update directions can only move in two of those possible 2^D high-dimensional orthants. So even though this problem looks bad in two dimensions, it gets literally exponentially worse as we move to weight matrices of higher and higher dimension.

在它们都必须为正或为负的约束下，这意味着我们的任何更新方向只能在这2^D个可能的高维超象限中的两个中移动。因此，尽管这个问题在二维中看起来很糟糕，但当我们转向维度越来越高的权重矩阵时，它会呈指数级恶化。

So that seems like a bad property of this sigmoid non-linearity: the fact that it's not zero-centered and the fact in particular that its outputs are always positive leads to these very unstable and potentially awkward dynamics during training.

因此这似乎是sigmoid非线性的一个不良特性：它不以零为中心，特别是其输出始终为正的事实，导致在训练过程中出现非常不稳定且可能笨拙的动态。

I should point out though that this whole analysis about the gradients on the weights being all positive or all negative only applies to a single example. However, in practice we'll often perform mini-batch gradient descent, and once we take an average over multiple elements in a mini-batch, then we kind of relax this constraint.

不过我应该指出，这个关于权重梯度全部为正或全部为负的完整分析仅适用于单个样本。然而，在实践中我们通常会执行小批量梯度下降，一旦我们对小批量中的多个元素取平均值，我们就在某种程度上放松了这个约束。

So even though for a single example in the mini-batch we would end up with gradients that are all positive or all negative on each layer, when we consider the gradients with respect to a full mini-batch of data examples, then this is less of a problem because you could imagine that maybe even though the gradients with respect to each element are all positive or all negative, when you average them out and sum them out over the mini-batch, then you could end up with gradients for the mini-batch that are sometimes positive and sometimes negative.

因此，尽管对于小批量中的单个样本，我们在每一层上得到的梯度全部为正或全部为负，但当我们考虑相对于完整小批量数据样本的梯度时，这个问题就不那么严重了，因为你可以想象，即使相对于每个元素的梯度全部为正或全部为负，当你在小批量上对它们进行平均和求和时，你最终可能会得到小批量的梯度有时为正，有时为负。

So I think this is a problem, but it is maybe less of a problem in practice than some of the other concerns around the sigmoid non-linearity. But it is something to keep in mind nevertheless.

所以我认为这是一个问题，但在实践中，它可能不如sigmoid非线性的其他一些问题严重。但这仍然是需要记住的事情。

So that was our second problem with the sigmoid non-linearity: the fact that its outputs are not zero-centered.

这就是sigmoid非线性的第二个问题：其输出不以零为中心。

Now a third problem with the sigmoid non-linearity is the exponential function. I don't know if you know how these mathematical functions get implemented on CPUs, but something like the exponential function is fairly expensive because it's a complicated transcendental function, so it can actually take many clock cycles to compute an exponential function.

现在sigmoid非线性的第三个问题是指数函数。我不知道你是否知道这些数学函数是如何在CPU上实现的，但像指数函数这样的计算是相当昂贵的，因为它是一个复杂的超越函数，所以实际上计算指数函数可能需要许多时钟周期。

When I timed this, I did a small experiment on my MacBook CPU the other day. If I want to compare a sigmoid non-linearity versus a ReLU non-linearity for a single layer with about a million elements, then on my MacBook CPU the ReLU loop ends up being about three times faster than the sigmoid. This is because the ReLU just involves a simple threshold, whereas the sigmoid needs to compute this very expensive exponential function.

当我进行计时测试时，前几天在MacBook CPU上做了个小实验。如果要比较含约百万个元素的单层中sigmoid非线性与ReLU非线性的性能，在我的MacBook CPU上ReLU循环比sigmoid快约三倍。这是因为ReLU仅涉及简单阈值判断，而sigmoid需要计算非常昂贵的指数函数。

Now I should also point out that of these three problems, the third problem of exponential being expensive to compute is mostly a concern for mobile devices and CPU devices. For GPUs this ends up being less of a concern because the cost of simply moving data between global memory and compute elements takes more time than actually computing the nonlinearity.

需要指出的是，在这三个问题中，指数计算成本高的问题主要影响移动设备和CPU设备。对GPU而言这反而较不令人担忧，因为数据在全局内存与计算单元间传输的时间成本已超过实际计算非线性函数的时间。

In practice if you try to time these different nonlinearities on a GPU, you'll find they often all come out to be about the same speed. So you really need to move to some kind of CPU device in order to see speed differences between these different nonlinearities.

实际上在GPU上测试这些不同非线性函数时，往往会发现它们速度基本相当。因此必须切换到CPU设备才能观察到不同非线性函数的速度差异。

That gives us these three problems with the sigmoid function. Of these three problems, I really think number one is the most problematic. Problems number two and three are things you should be concerned about or should be aware of, but it's really this saturation killing the gradient that is the most problematic aspect of the sigmoid non-linearity.

以上就是sigmoid函数的三个问题。其中我认为第一个问题最为严重。第二和第三个问题需要关注和了解，但梯度饱和消失才是sigmoid非线性最致命的缺陷。

So then we can move on from sigmoid and look at another popular non-linearity that people sometimes use: the tanh non-linearity. Tanh is basically just a scaled and shifted version of sigmoid. If you look up the definitions on paper in terms of exponential functions, you can show that tanh is literally just a shifted and rescaled sigmoid.

接下来我们可以从sigmoid转向观察另一个常用非线性函数：tanh非线性。Tanh本质上是sigmoid的缩放平移版本。查阅基于指数函数的定义公式即可证明，tanh实际上就是经过平移和缩放的sigmoid函数。

So it inherits many of the same problems as the sigmoid non-linearity. It still saturates for very large and very small values, so it still results in difficult learning in those cases. But unlike the sigmoid, it is zero-centered.

因此它继承了sigmoid非线性的诸多缺陷。在极大极小值处仍会出现饱和，导致这些情况下学习困难。但与sigmoid不同之处在于它以零为中心。

If for some reason you have the urge to use a saturating non-linearity in your neural networks, then I think tanh is a slightly better choice than sigmoid. But really it's still a pretty bad choice due to these saturating regimes.

如果出于某种原因坚持要在神经网络中使用饱和非线性函数，那么tanh是比sigmoid稍好的选择。但由于存在饱和区域，这仍然是个相当糟糕的选择。

Now the next non-linearity is our good friend the ReLU, the rectified linear activation. This one is very nice and we're very familiar with it by now. It's very cheap to compute because it only involves a simple threshold.

接下来介绍我们的好朋友ReLU（修正线性单元）。这个函数非常出色且大家已很熟悉。它的计算成本极低，仅需简单阈值判断。

You can imagine for a binary implementation all we have to do is check the sign bit of the floating-point number. If it's negative we set it to zero, and if it's positive we leave it alone. So ReLU is like the cheapest possible nonlinear function you can imagine implementing.

可以设想在二进制实现中，我们只需检查浮点数的符号位：若为负则设为零，若为正则保持原值。因此ReLU堪称可实现的最廉价非线性函数。

It's very fast and can typically be done in one clock cycle on most things. It does not saturate in the positive regime, so as long as our inputs are positive we never have to worry about this saturation problem of killing our gradients.

它的执行速度极快，在多数设备上仅需一个时钟周期。在正数区域不会饱和，因此只要输入为正就无需担心梯度消失的饱和问题。

In practice when you try to train the same network architecture with sigmoid versus tanh versus ReLU, it's very often found that the ReLU version converges much faster, up to six times as reported by AlexNet.

实际应用中，当使用sigmoid、tanh和ReLU分别训练相同网络架构时，经常发现ReLU版本的收敛速度要快得多——AlexNet报告中可达六倍加速。

When you go to very deep networks like 50 or 100 layers, it can be very challenging to get sigmoid networks to converge at all unless you use something like normalization. So there are some problems with ReLU.

当网络深度达到50或100层时，若不使用归一化等技术，sigmoid网络可能完全无法收敛。但ReLU也存在一些问题。

One of course is that it's not zero-centered. Just like the sigmoid non-linearity which has all of its outputs always positive, the same applies to ReLU. All of its outputs are also non-negative.

首先是它不以零为中心。与所有输出恒为正的sigmoid非线性类似，ReLU的所有输出同样为非负值。

That means ReLU suffers from the same problem of gradients being all positive or all negative as the sigmoid. But since we know ReLU networks can be trained without much difficulty, that suggests this nonzero centering problem was maybe less of a concern than other problems with sigmoid.

这意味着ReLU存在与sigmoid相同的梯度全正或全负问题。但鉴于ReLU网络训练难度不大，这表明非零中心化问题可能不如sigmoid的其他问题严重。

The big problem with ReLU of course is what happens when X is less than zero. Here the gradient is exactly zero. If you imagine training this ReLU function when X is very large like plus ten, the local gradient will be one and learning will proceed fine.

ReLU的真正问题在于当X小于零时的情况。此时梯度精确为零。设想训练ReLU函数时，若X为+10这样的大数，局部梯度为1，学习过程正常进行。

When X is very small like minus ten, the gradient will be identically zero. This means our local gradient is exactly zero, which means our downstream gradients are also exactly zero.

当X为-10这样的极小值时，梯度将恒为零。这意味着局部梯度精确为零，导致下游梯度也精确为零。

This is somehow even worse than sigmoid because with sigmoid in the very small regime we weren't exactly zero, we were just very small. So even if gradients were very small we still had some potential hope that learning could proceed.

这某种程度上比sigmoid更糟糕，因为sigmoid在极小值区域梯度虽极小但非零。即使梯度非常小，仍存在学习进程继续的潜在希望。

But with ReLU once our values are less than zero, all our gradients are identically zero and learning cannot proceed. Our gradients will be completely zero, it will be completely dead.

但对于ReLU，一旦数值小于零，所有梯度恒为零且学习完全停滞。梯度将彻底归零，神经元完全死亡。

This leads to this potential problem that people worry about sometimes called a dead ReLU. The idea is that suppose in one of our ReLU nonlinearities, the weights of that layer get very large in magnitude such that the neuron has negative activation for every data point in our training set.

这就导致了人们常担忧的"死亡ReLU"问题。假设某个ReLU非线性层中，该层权重变得极大，导致该神经元对训练集中所有数据点都产生负激活。

In that case it means the weights corresponding to that unit will always have gradients identically equal to zero as we iterate over the training set. That unit sometimes is called a dead ReLU because it never has the potential to learn.

这种情况下，在遍历训练集时该单元对应权重将始终获得零梯度。这个单元有时被称为死亡ReLU，因为它永远失去学习能力。

Once the ReLU gets knocked off this data cloud of your training examples, all future updates it will receive are going to be zero. It will be stuck off in limbo hanging outside your data cloud for the rest of eternity no matter how long you train.

一旦ReLU偏离训练数据云，它未来获得的所有更新都将为零。无论训练多久，它都将永远滞留在数据云之外的混沌区域。

So that seems like a problem. In contrast we want our ReLUs to always somehow stay intersecting with some part of the data cloud. Those are active ReLUs and they will receive gradients and they will train.

这显然是个问题。相反地，我们希望ReLU始终能与数据云的某些部分保持交集。这些活跃的ReLU将获得梯度并进行训练。

I should point out this problem only occurs if your neuron, if your activation value is negative for your entire training set. As long as there's some element of your training set where you receive a positive activation, then that weight has the potential to get some gradient and has the potential to learn.

需要指出的是，这个问题仅当你的神经元在整个训练集上的激活值都为负时才会发生。只要训练集中存在某些元素能产生正激活，那么该权重就有机会获得梯度并具备学习潜力。

So one trick that I've seen people sometimes do, maybe I think it's less popular now, but to avoid this potential problem of dead ReLUs, one trick you might think of is to actually initialize the biases of layers that use ReLU to actually have a slightly positive value. That means that gives you the, that makes it harder to fall into this negative regime and harder to have dead neurons.

我见过有人有时会使用的一个技巧——虽然现在可能不太流行了——但为了避免ReLU神经元死亡这个潜在问题，你可以考虑的一个技巧是：将使用ReLU的层的偏置初始化为一个略为正的值。这意味着这会使神经元更难陷入负值区域，也更难出现神经元死亡的情况。

We saw that one problem with the ReLU non-linearity is the fact that it's not zero-centered, and that it has zero gradient in the negative regime. So there was an alternative proposed called the leaky ReLU that solves both of these problems.

我们看到ReLU非线性函数的一个问题是它不以零为中心，并且在负值区域梯度为零。因此有人提出了称为泄漏ReLU的替代方案，可以同时解决这两个问题。

Leaky ReLU is very simple. It looks just like ReLU except in the positive regime it computes the identity function, and when the input is negative then we're going to multiply it by a small positive constant. So rather than being identically zero, then in the negative regime the leaky ReLU instead has a small positive slope.

泄漏ReLU非常简单。它看起来就像ReLU，区别在于在正值区域它计算恒等函数，而当输入为负时，我们会将其乘以一个小的正常数。因此，在负值区域，泄漏ReLU不再完全为零，而是具有一个小的正斜率。

And what this kind of, you can kind of imagine this as a ReLU that is not exactly zero, it kind of like leaks out some of its information but just a little bit in the negative regime. And now this 0.01, this slope of the leaky ReLU in the negative regime, is a hyperparameter that you need to tune for your networks.

你可以将这种函数想象成一个不完全为零的ReLU，它在负值区域会"泄漏"出部分信息，但只是很少的一部分。现在这个0.01——即泄漏ReLU在负值区域的斜率——是一个需要为你的网络调整的超参数。

Now the advantage of a leaky ReLU is that it does not saturate in the positive regime, it's still computationally efficient because it only takes a couple instructions to execute, and unlike the ReLU or the sigmoid it doesn't ever die. The gradient never actually dies because our local gradients will never actually be zero in the negative regime. Our local gradient will just be this zero point zero one or whatever other value for that hyperparameter we have chosen.

泄漏ReLU的优势在于它在正值区域不会饱和，计算效率仍然很高，因为只需要几条指令即可执行，而且与ReLU或sigmoid不同，它永远不会死亡。梯度实际上永远不会消失，因为我们的局部梯度在负值区域永远不会真正为零。我们的局部梯度将只是这个0.01，或者我们为该超参数选择的任何其他值。

What this means is that these leaky ReLUs in fact don't suffer from this dying ReLU problem. Instead in the negative regime they'll just receive smaller gradients and have the potential to keep learning. But now an annoyance with this leaky ReLU is this 0.01, this kind of leaky hyperparameter value that we need to set.

这意味着这些泄漏ReLU实际上不会遭受ReLU死亡问题的困扰。相反，在负值区域，它们只会接收到较小的梯度，并具有持续学习的潜力。但现在泄漏ReLU的一个烦恼是这个0.01——这个我们需要设置的泄漏超参数值。

And as you've probably experienced when trying to tune even linear classifiers on the first couple assignments, the more hyperparameters you need to search over, the more pain and frustration you have when trying to train your models. So clearly whenever you see a hyperparameter, one instinct you should have is that maybe we should try to learn that value instead.

正如你可能在前几个作业中尝试调整线性分类器时所经历的那样，需要搜索的超参数越多，训练模型时的痛苦和挫败感就越强。因此很明显，每当你看到一个超参数时，你的第一反应应该是也许我们应该尝试学习这个值。

So indeed this is the exact idea behind the parametric ReLU or PReLU, where now it looks just like the leaky ReLU except the slope in the negative regime is now a learnable parameter of the network. Which now this is kind of a funny thing because now this parametric ReLU with this PReLU is actually a non-linearity that itself has learnable parameters.

这确实是参数化ReLU（PReLU）背后的确切思想，它看起来就像泄漏ReLU，只是负值区域的斜率现在变成了网络的可学习参数。这现在有点有趣，因为现在这个带有PReLU的参数化ReLU实际上是一个自身具有可学习参数的非线性函数。

But we can imagine computing that just fine. Then in the backwards pass we'll just backpropagate into this value of alpha, compute derivative of loss with respect to alpha, and then also make gradient descent steps on alpha. And this alpha might be a single constant for each layer, or maybe this alpha could be a separate value for each channel in your convolution or each output element in your fully connected layer.

但我们可以想象这完全可以正常计算。然后在反向传播过程中，我们会反向传播到这个alpha值，计算损失对alpha的导数，然后也对alpha进行梯度下降步骤。这个alpha可以是每层的一个单一常数，或者这个alpha可以是卷积中每个通道或全连接层中每个输出元素的独立值。

So that's fine, you'll see people work on, you'll see people use that sometimes. But one problem you might have with any of these ReLU functions is that they actually have a kink at zero right, they're actually not differentiable there at all.

这样是可以的，你会看到人们研究这个，有时也会使用它。但所有这些ReLU函数可能存在的一个问题是它们在零点实际上有一个折点，在那里实际上完全不可微。

You might ask what happens at zero? Well it actually doesn't matter what happens at zero because it doesn't happen very often, so you can pretty much choose whatever you want to have it happen at zero and it's probably going to be fine. But in practice usually you just pick one side or the other.

你可能会问在零点会发生什么？实际上零点发生什么并不重要，因为它不常发生，所以你可以基本上选择任何你希望在零点发生的情况，这很可能没问题。但在实践中，通常你只需选择一侧或另一侧。

But one kind of slightly more theoretically grounded non-linearity that you'll see sometimes is the exponential linear unit, and this attempts to fix some of the problems of ReLU. That it's basically like a ReLU but it's smooth and it tends to be more zero-centered.

但你会有时看到的另一种理论上更严谨的非线性函数是指数线性单元（ELU），它试图修复ReLU的一些问题。它基本上像ReLU但是平滑的，并且往往更以零为中心。

So we you can see the mathematical definition of the exponential linear unit here at the bottom of the slide. In the positive regime it just computes the identity function just like ReLU, but in the negative regime it computes this exponential value instead.

你可以在幻灯片底部看到指数线性单元的数学定义。在正值区域，它就像ReLU一样计算恒等函数，但在负值区域，它计算的是这个指数值。

So on the left hand side it kind of looks a little bit like the tail end of a sigmoid, and in fact we also see that it asymptotes to some negative value as we go to the left. This is to avoid this problem of zero gradients that we might have been concerned about with the normal ReLU.

所以在左侧，它看起来有点像sigmoid函数的尾部，事实上我们也看到当向左移动时它会渐近趋近于某个负值。这是为了避免我们可能对普通ReLU担心的零梯度问题。

So this was designed to kind of fix some of these problems with ReLU exactly right, that now because the negative regime actually is non-zero, then you can imagine that this non-linearity might actually have zero-centered outputs. And there's some math in the paper to support that conclusion.

所以这个设计正是为了修复ReLU的一些问题，现在因为负值区域实际上是非零的，那么你可以想象这个非线性函数实际上可能产生以零为中心的输出。论文中有一些数学计算支持这个结论。

The problem here is that the computation still requires this exponential function which is maybe not so good, and it also has this additional hyperparameter alpha that we need to set, although I guess you could try to learn alpha as well and come up with a parametric exponential linear unit or PELU, but I've never actually seen anyone do that in practice.

这里的问题是计算仍然需要这个指数函数，这可能不太好，而且它还有这个额外的超参数alpha需要设置，尽管我猜你也可以尝试学习alpha并提出参数化指数线性单元（PELU），但我从未见过任何人在实践中这样做。

But maybe you could try it, write a paper, who knows. It doesn't stop there, so there was another paper. People love to propose very, I mean I think you should get that we get this idea right now that this non-linearity is kind of this small modular piece of a neural network that you can take out to find new things and run controlled experiments.

但也许你可以尝试一下，写篇论文，谁知道呢。这还没有结束，所以还有另一篇论文。人们喜欢提出非常——我的意思是，我认为你现在应该明白这个想法：非线性函数是神经网络的一个小型模块化组件，你可以将其取出以发现新事物并进行受控实验。

So it's very appealing for a lot of people to try to modify neural networks by coming up with new nonlinearities. So you'll actually see a lot of papers that try to come up with slight variances on them and try to argue for why their ideas are slightly better than anyone became before.

因此，通过提出新的非线性函数来修改神经网络对很多人来说非常有吸引力。所以你实际上会看到很多论文试图提出这些激活函数的微小变体，并论证为什么他们的想法比前人更好。

As a result there's a lot of these things out there that you can find. One that's kind of fun is the SiLU or scaled exponential linear unit. This one is fun because it's just a rescaled version of a ReLU that we just saw on the previous slide. The only difference is that we set alpha equal to this very long seemingly arbitrary constant and we set lambda equal to this very long seemingly arbitrary constant.

因此市面上有很多这样的激活函数可供选择。其中一个比较有趣的是SiLU或缩放指数线性单元。这个函数有趣之处在于它只是我们在上一张幻灯片中看到的ReLU的缩放版本。唯一区别是我们将alpha设置为这个看似随意的长常数，将lambda设置为另一个看似随意的长常数。

The reason that we might want to do this is that if you choose alpha and lambda in this very particular way, then a very deep neural network with this SiLU non-linearity has a kind of self-normalizing property. That as your layer depth goes to infinity, then the statistics of your activations will be well-behaved and even converge to some finite value as the depth of your network trends toward infinity.

我们这样做的原因是，如果你以这种特定方式选择alpha和lambda，那么使用这种SiLU非线性的深度神经网络就具有某种自归一化特性。当你的网络层数趋近于无穷大时，激活的统计特性会表现良好，甚至随着网络深度趋近于无穷大而收敛到某个有限值。

What this means is that if you have very very deep networks with SiLU nonlinearities, then sometimes people can get these things to train very very deep networks even without using batch normalization or other normalization techniques.

这意味着如果你使用SiLU非线性构建非常非常深的网络，有时人们甚至可以在不使用批归一化或其他归一化技术的情况下训练这些非常深的网络。

Now unfortunately in order to understand exactly why this is true, you have to work through 91 pages of math in the appendix of this paper. So if you have a lot of patience you can go through this and figure out exactly why we set the values of these constants to those particular values.

但不幸的是，要确切理解为什么这是真的，你必须研读这篇论文附录中的91页数学推导。所以如果你有足够的耐心，你可以仔细研究并弄清楚为什么我们将这些常数设置为那些特定值。

But I think that's a little kind of fun. But I think the bigger takeaway around a lot of these nonlinearities is that in practice they really don't vary too much in their performance.

但我认为这有点意思。不过我认为关于这些非线性函数更重要的收获是，在实践中它们的性能差异其实不大。

So here's a plot from a paper from another paper that was another of these papers about nonlinearities that actually compared the effects of different nonlinearities on different network architectures on the CIFAR-10 dataset.

这里有一张来自另一篇关于非线性函数论文的图表，该论文实际上比较了不同非线性函数在不同网络架构上对CIFAR-10数据集的影响。

And what's if you look at this plot you see that some of the bars are higher than some of the other bars, but the most important thing to take away from this plot is really just how close all of these things are.

如果你看这张图表，会发现有些柱状图比其他柱状图更高，但最重要的收获是所有这些结果都非常接近。

Something like ReLU on ResNet is giving us 93.8, leaky ReLU is 94.2, soft Plus as a different one is 94.6. So basically all of these things are within a percent of each other in final accuracy on CIFAR-10.

比如在ResNet上使用ReLU得到93.8%，泄漏ReLU是94.2%，soft Plus是94.6%。所以基本上所有这些在CIFAR-10上的最终准确率都在彼此1%的范围内。

And the trends here are just not consistent. So if we look at a ResNet then something called a GELU or a swish non-linearity is gonna slightly outperform a ReLU, but if we look at a DenseNet then ReLU is slightly better or equal to anything else.

而且这些趋势并不一致。如果我们在ResNet上看，像GELU或swish非线性会略微优于ReLU，但如果看DenseNet，ReLU则略微优于或等于其他任何方法。

So the real takeaway I think for nonlinearities is just like don't stress out about it too much, because usually your choice of non-linearity as long as you don't make a stupid choice and choose sigmoid or tanh, if you use any of these more reasonable more modern nonlinearities your network is going to work just fine.

所以我认为关于非线性函数的真正收获是不要过分纠结于此，因为通常只要你没有做出愚蠢的选择（比如选择sigmoid或tanh），而是使用任何这些更合理、更现代的非线性函数，你的网络都能正常工作。

And maybe for your particular problem you might see a variance of maybe one or two percent on your final accuracy depending on which non-linearity you use, but that's going to be very dependent on your data set on your model architecture and on all your other hyper parameter choices.

对于你的特定问题，根据使用的非线性函数不同，最终准确率可能会有1%或2%的差异，但这很大程度上取决于你的数据集、模型架构以及所有其他超参数选择。

So my advice is just don't stress out too much about activation functions. Just in basically don't think too hard just use ReLU it'll work just fine and it'll probably work.

所以我的建议是不要对激活函数太过纠结。基本上不用想太多，直接使用ReLU就行，它会工作得很好，而且很可能就能满足需求。

Now if you're in some situation where you really really must squeeze out that one last percentage point or that last half a percentage point or a tenth of a percentage point of performance, then I think is the time when you should consider swapping out and experimenting with these different nonlinearities.

如果你处于某种情况，真的必须挤出那最后1个百分点、最后0.5个百分点或最后0.1个百分点的性能，那么我认为这时你应该考虑更换并试验这些不同的非线性函数。

So in that case something like a leaky ReLU or an ELU or a SiLU or a GELU or whatever other thing you can think of that rhymes with a LU is probably a reasonable choice to try, but don't expect really too much from it.

在这种情况下，像泄漏ReLU、ELU、SiLU、GELU或其他任何你能想到的以"LU"结尾的函数可能都是值得尝试的合理选择，但不要对它期望过高。

And basically don't use sigmoid or tanh those are terrible ideas your network will not converge and just don't use those.

基本上不要使用sigmoid或tanh，这些都是糟糕的选择，你的网络将无法收敛，所以干脆不要使用它们。

So that's kind of my executive summary on activation functions. Any questions on activation functions before we move on?

这就是我对激活函数的简要总结。在继续之前，关于激活函数有什么问题吗？

The question was what all of these activation functions are monotonic they increase well and why don't we use something like sine or cosine?

问题是所有这些激活函数都是单调递增的，为什么我们不使用像正弦或余弦这样的函数？

Well I actually lied a little bit there's this GELU non-linearity function that I didn't talk about that is actually non monotonic but the reason is that if your activation function is non-monotonic is something like sine or cosine, if it increases and then decreases then there exist multiple values of X that have the same Y and that can be problematic for learning because that kind of destroys information in some way.

其实我有点说谎了，有一个我没讲的GELU非线性函数实际上是非单调的。但原因是如果你的激活函数是非单调的，比如正弦或余弦函数，如果它先增后减，那么就会存在多个X值对应相同的Y值，这对学习来说可能有问题，因为这在某种程度上破坏了信息。

So if your activation function is not invertible that means that it's destroying information and we'd prefer and the reason we wanted activation functions in the first place was not so much to perform useful computation it was more just to add some non-linearity into the system to allow it to represent non-linear functions.

所以如果你的激活函数不可逆，那意味着它在破坏信息，而我们更希望——我们最初需要激活函数的主要原因不是为了执行有用的计算，更多只是为了给系统添加一些非线性，使其能够表示非线性函数。

I have seen people try to show off and train with sine or cosine or something if you use batch normalization and are very careful I think you could probably get that to train but I would not recommend it.

我见过有人为了炫耀而尝试使用正弦或余弦函数进行训练，如果你使用批归一化并且非常小心，我认为你也许能让它训练起来，但我不推荐这样做。

But I think that's a very astute observation and actually the GELU non-linearity is a slightly interesting one to check out so I think you should read that paper if you're interested.

但我认为这是个非常敏锐的观察，实际上GELU非线性是一个稍微有趣的值得研究的函数，所以如果你感兴趣的话，我认为你应该读一下那篇论文。

But there the idea is that they actually interpret the non monotonicity of the activation function as actually a bit of regularization and they view that as the expectation they kind of combine it with something like dropout which we'll talk about later and then show that if you take this expectation combined with some stochastic stuff that is sort of equivalent in some rough expectation way to a non monotonic activation function.

但他们的想法是，他们实际上将激活函数的非单调性解释为某种正则化，并将其视为期望值，他们将其与像dropout这样的方法结合起来（我们稍后会讨论），然后证明如果你采用这种期望结合一些随机因素，在某种粗略的期望意义上等价于一个非单调激活函数。

But in general they're not very widely used and most activation functions you'll see in practice are indeed monotonic.

但总的来说它们使用并不广泛，你在实践中看到的大多数激活函数确实都是单调的。

Any other questions on activation functions? Great just use ReLU.

关于激活函数还有其他问题吗？很好，那就直接用ReLU吧。

So then the next thing we need to talk about is data pre-processing and this is something that you've been doing already in all of your homework assignments already. So what's the idea with that? Well basically the idea is that we want to, before we feed our data into the neural network, we want to perform some pre-processing on it to make it more amenable to efficient training.

那么接下来我们需要讨论的是数据预处理，这是你们在所有作业中已经在做的事情。这样做的目的是什么呢？基本上，我们的想法是在将数据输入神经网络之前，需要对其进行一些预处理，使其更适合高效训练。

As a kind of cartoon here, you can imagine your data cloud of your training set shown here in red on the left, where the X and the Y values are maybe two features of your dataset. So like maybe the red value of one pixel and the blue value of another pixel if you're looking at images.

这里可以打个比方，你可以想象训练集的数据云，如左图红色所示，其中X和Y值可能是数据集的两个特征。比如在图像处理中，可能是一个像素的红色值和另一个像素的蓝色值。

The idea is that your original data is going to be this data cloud which could be very long and skinny and shifted very far away from the origin. In particular, if we're thinking about images, then the way we natively store image data is usually as pixel values between 0 and 255.

关键在于原始数据会形成这种数据云，可能又长又细，且偏离原点很远。特别是对于图像数据，我们通常以0到255之间的像素值来存储原始图像数据。

So our data cloud for raw image data would be kind of located very far away from the origin. Now before we feed the data into the neural network, we want to standardize it in some way and pull it closer to the origin by subtracting the overall mean of the training dataset.

因此原始图像数据的数据云会距离原点很远。在将数据输入神经网络之前，我们需要以某种方式对其进行标准化处理，通过减去训练数据集的整体均值将其拉近原点。

Then we rescale each of the features so that each feature has the same variance, and we can do this by dividing by the standard deviations of each feature computed on the training set.

然后我们重新缩放每个特征，使每个特征具有相同的方差，这可以通过除以在训练集上计算得到的每个特征的标准差来实现。

This idea of why we might want to perform pre-processing in this way also kind of ties back to this discussion we had about the bias problem with sigmoid nonlinearities. Recall if all of the inputs are going to be positive, then all of our gradient directions are going to always be positive or negative.

这种预处理方式的必要性也与我们之前讨论的sigmoid非线性函数的偏置问题相关。回想一下，如果所有输入都是正数，那么所有梯度方向将始终为正或为负。

Similarly, by the same logic, if all of our training data is always positive, then all of our weight updates will also be constrained to have either one sign or another sign. But for training data, we can easily fix this problem by just rescaling everything before we feed it into the neural network.

同样地，按照这个逻辑，如果所有训练数据始终为正，那么所有权重更新也将被限制为具有单一符号。但对于训练数据，我们可以通过在输入神经网络之前重新缩放所有数据来轻松解决这个问题。

For images, it's very common to subtract the mean and divide by the standard deviation. But for other types of data, you might see maybe non-image data, you will sometimes see other types of pre-processing called decorrelation or whitening.

对于图像数据，通常采用减去均值并除以标准差的方法。但对于其他类型的数据，比如非图像数据，有时会看到其他类型的预处理方法，称为去相关或白化处理。

Here the idea is that if we compute the covariance matrix of our data cloud of the entire training set, then we can use that to rotate the data cloud such that the features are uncorrelated. This would be the green data cloud in the middle of the slide.

这里的思路是，如果我们计算整个训练集数据云的协方差矩阵，就可以用它来旋转数据云，使得特征之间不相关。这对应于幻灯片中间显示的绿色数据云。

Now we've basically taken our input data cloud and rotated it, actually moved it to the center of the origin and then rotated it. Another thing you'll sometimes do is then have not only have zero mean unit variance for each feature, but actually perform this zero mean unit variance fixing after decorrelating the data.

现在我们已经基本上将输入数据云进行了旋转，实际上是将它移动到原点中心然后旋转。有时还会做的另一件事是，不仅让每个特征具有零均值和单位方差，而且在数据去相关后实际执行这种零均值单位方差的修正。

That would correspond to this now stretching your data cloud out into this very well-behaved circle at the center of the coordinate axis here on the right shown in blue. If you perform both decorrelation and this normalization, this is often called whitening your input data.

这将对应于将数据云拉伸成这个在坐标轴中心非常规整的圆形，如右图蓝色所示。如果同时执行去相关和这种归一化处理，通常称为输入数据的白化处理。

This is sometimes pretty common to use if you're working on non-vision problems where maybe your inputs are specified as low dimensional vectors in some way. But for image data, this is not so common.

这在处理非视觉问题时相当常见，特别是当输入以某种方式指定为低维向量时。但对于图像数据，这种方法并不那么常见。

Another way that you can think about why data pre-processing is helpful is to think about what happens if we try to learn even a linear classifier on non-standardized data or non-normalized data.

另一种理解数据预处理为何有益的方式是思考在非标准化或非归一化数据上尝试学习线性分类器时会发生什么。

Here on the left, suppose that we have this data cloud that is located very far from the origin, and now we want to find this linear classifier that is going to separate the blue class from the red class.

在左图中，假设我们有一个距离原点很远的数据云，现在想要找到一个能够区分蓝色类和红色类的线性分类器。

Now if our data cut now if we initialize our weight matrix with very small random values as we've typically done, then we can expect that at initialization the boundary that our linear classifier learns is kind of near the origin.

如果我们像通常那样用很小的随机值初始化权重矩阵，那么在初始化时，线性分类器学习到的边界会靠近原点。

Now if our data cloud is very very far away from the origin, then making very very small changes to the values of the weight matrix will result in very drastic changes to the way that that decision boundary cuts through the training data cloud.

如果数据云距离原点非常远，那么对权重矩阵值进行非常微小的改变将导致决策边界切割训练数据云的方式发生剧烈变化。

That means that kind of intuitively if your data is not normalized, then that leads to a more sensitive optimization problem because very small changes in the weight matrix can result in very large changes to the overall classification performance of the system.

这意味着直观上来说，如果数据没有归一化，会导致优化问题更加敏感，因为权重矩阵的微小变化可能导致系统整体分类性能的巨大变化。

In contrast, on the right, if before during training we had normalized our data by moving it to the center, now our entire data cloud also is sitting over the origin. So we can expect this will result in a better conditioned optimization problem.

相比之下，在右图中，如果在训练前通过将数据移动到中心进行了归一化，现在整个数据云也位于原点附近。因此我们可以预期这将导致条件更好的优化问题。

Yeah a question. The question is do people ever use other color spaces other than RGB? I think people do use that sometimes in practice. I've seen that less for image classification.

是的，有个问题。问题是人们是否使用除RGB之外的其他色彩空间？我认为在实践中人们确实有时会使用。在图像分类中我见得比较少。

I've sometimes seen that for more image processing type tasks like super resolution or denoising, and there it's more common I think to feed raw inputs to the network as some other color space.

我有时在更多图像处理类型的任务中看到这种情况，比如超分辨率或去噪，在这些任务中，将其他色彩空间的原始输入馈送到网络中更为常见。

But in practice I think it usually doesn't matter too much what color space you use because in principle the equations for converting from one color space into another color space are usually fairly compact simple equations.

但在实践中，我认为使用什么色彩空间通常不太重要，因为原则上从一个色彩空间转换到另一个色彩空间的方程通常相当简洁简单。

The network could kind of learn in the first couple of layers to perform that kind of conversion implicitly within the first two layers or so if it really wanted to. So in practice you do see it sometimes see people feed input data of different color spaces, but in practice it usually doesn't have massive differences on the final results.

网络确实可以在前几层中学习隐式执行这种转换，如果它真的需要的话。所以在实践中，你确实会看到人们有时会馈送不同色彩空间的输入数据，但在实践中通常对最终结果没有巨大差异。

So then talking about a little bit more concretely about what people actually do in practice for images, there is a couple things that are very common to do in images. One is to compute the mean image on the training set.

那么更具体地谈谈人们在实践中对图像实际做什么，有几件事情在图像处理中非常常见。一是在训练集上计算平均图像。

This for example was done in AlexNet where they write if your training dataset is something like n trained by 32 by 32 by 3, then if you average over the entire training dataset then you get a mean image of 32 by 32 by 3. One thing you can do is subtract the mean image from all your training samples. Another common approach is to subtract the per-channel mean, where you compute the mean RGB color across the entire training dataset and subtract only the mean color from each pixel. This method was used, for example, in training VGG networks.

例如在AlexNet中就采用了这种方法，他们写道如果你的训练数据集是类似n个32x32x3的数据，那么在整个训练数据集上取平均值，你就会得到一个32x32x3的平均图像。一种做法是从所有训练样本中减去均值图像。另一种常见方法是减去每个通道的均值，即计算整个训练数据集的平均RGB颜色，然后从每个像素中仅减去该平均颜色。例如，VGG网络就是采用这种方式进行训练的。

Another common practice is to subtract the per-channel mean and divide by the per-channel standard deviation. This involves computing both the mean RGB color and the standard deviation of the three color channels across the training dataset, resulting in two three-element vectors used for normalization.

另一种常见做法是减去每个通道的均值并除以每个通道的标准差。这需要计算训练数据集中三个颜色通道的平均RGB值和标准差，从而得到两个三元素向量用于归一化处理。

This standard preprocessing method is commonly used for residual networks. Are there any more questions about data preprocessing?

这是残差网络常用的标准预处理方法。关于数据预处理还有其它问题吗？

The question is about training versus testing procedures. When performing preprocessing, you always compute statistics from the training set. This approach simulates real-world deployment scenarios where no training set exists, only actual data for classification.

问题涉及训练与测试流程的区别。进行预处理时，始终基于训练集计算统计量。这种方法模拟了实际部署场景：现实中不存在训练集，只有需要分类的真实数据。

There's no feasible way to continuously recompute statistics from real-world input data, so we consistently apply the training set normalization statistics to the test set.

由于无法持续从实时输入数据重新计算统计量，因此我们统一将训练集的归一化统计量应用于测试集。

Regarding batch normalization and data preprocessing: even with batch normalization, explicit preprocessing remains beneficial. While placing batch normalization as the first layer might function, explicit preprocessing generally yields better performance.

关于批归一化与数据预处理的关系：即使使用批归一化，显式预处理仍然有益。虽然将批归一化作为网络首层可能有效，但显式预处理通常能获得更优性能。

In practice, most practitioners combine explicit preprocessing with batch normalization layers. Now let's discuss weight initialization.

实践中，大多数从业者会同时采用显式预处理和批归一化层。接下来我们讨论权重初始化。

When training neural networks, we must initialize weights somehow. Consider initializing all weights and biases to zero: this would be disastrous because with ReLU or similar nonlinearities, all outputs would become zero and independent of inputs.

训练神经网络时，必须进行权重初始化。假设将所有权重和偏置初始化为零：这将导致灾难性后果，因为使用ReLU等非线性激活函数时，所有输出都会变为零且与输入无关。

Worse yet, all gradients would become zero, completely halting learning. This constant initialization problem is known as lack of symmetry breaking, preventing differentiation between training examples or network neurons.

更严重的是，所有梯度都会变为零，完全终止学习过程。这种常数初始化问题称为对称性破坏缺失，会阻碍训练样本或网络神经元之间的差异化。

With constant initialization, all training examples produce identical gradients, preventing meaningful learning. Instead, we typically initialize with small random numbers, such as zero-mean Gaussian distributions with tunable standard deviation.

使用常数初始化时，所有训练样本产生相同梯度，无法进行有效学习。通常我们改用小随机数初始化，例如采用可调标准差的正态分布。

This random initialization strategy works reasonably well for shallow networks but becomes problematic for deep networks. To understand why, let's examine activation statistics propagation through deep networks.

这种随机初始化策略在浅层网络中效果尚可，但在深层网络中会出现问题。为探究原因，我们需要观察激活值在深度网络中的传播统计特性。

This code snippet demonstrates forward propagation through a six-layer fully connected network with 4096 hidden units and tanh nonlinearity. The activation value histograms reveal concerning patterns.

这段代码展示了六层全连接网络的前向传播过程，该网络包含4096个隐藏单元并使用tanh非线性函数。激活值直方图显示出值得关注的模式。

After the first layer, we observe reasonable value distributions. However, deeper layers show activations collapsing toward zero, which severely impedes learning because zero activations lead to vanishing gradients.

第一层之后可见合理的数值分布。但随着网络加深，激活值逐渐坍缩为零，这会严重阻碍学习，因为零激活值会导致梯度消失。

Recall that weight gradients depend on previous layer activations. When activations approach zero, gradient updates become negligible, preventing effective network training.

回顾权重梯度的计算公式可知，其值取决于前层激活值。当激活值趋近零时，梯度更新变得微乎其微，导致网络无法有效训练。

Perhaps the issue stems from overly small weight initialization. Instead of 0.01 standard deviation, we might try 0.05. However, excessively large weights also cause problems by pushing activations into tanh saturation regions.

或许问题源于过小的权重初始化。我们尝试将标准差从0.01调整为0.05。但过大的权重同样会产生问题，会使激活值进入tanh饱和区。

With larger weight matrices, all activations enter tanh's saturating regimes, as visible in the histograms. This saturation again impedes gradient flow during backpropagation, which means that now the local gradients will again be 0 and everything will be 0, and learning will also not work.

使用较大权重矩阵时，所有激活值都会进入tanh饱和区，这在直方图中清晰可见。这种饱和现象同样会阻碍反向传播中的梯度流动，这意味着局部梯度将再次变为零，所有参数都会归零，学习过程也将无法进行。

So we saw that when we initialize the weights to be too small, then they all collapse to zero, and when we initialize the weights to be too big, then the activations all spread out to these saturating regimes of the non-linearity.

因此我们观察到，当权重初始化过小时，所有参数都会坍缩为零；而当权重初始化过大时，激活值会全部扩散到非线性函数的饱和区域。

So somehow we need to find a value for this weight initialization that is somehow in this Goldilocks zone in between too small and too large.

因此我们需要找到一种权重初始化的方法，使其处于"恰到好处"的区间，既不过小也不过大。

And it turns out that the Goldilocks initialization for these networks, one definition is called the Xavier initialization after the first author of this paper cited at the bottom.

事实证明，这种恰到好处的初始化方法之一被称为Xavier初始化，以论文第一作者的名字命名。

So here rather than setting the standard deviation as a hyperparameter, instead we're going to set the standard deviation to be 1 over the square root of the input dimension of the layer.

这里我们不将标准差设为超参数，而是将其设置为该层输入维度平方根的倒数。

And it turns out that if we make this sort of very special Goldilocks regime for initializing our weights, then we'll get very nicely scaled distributions activations no matter how deep our network goes.

事实证明，如果我们采用这种特殊的"恰到好处"的权重初始化方法，无论网络有多深，我们都能获得尺度良好的分布激活值。

I should also note that this derivation is showing you for a fully connected network, but for a convolutional network, that DN will now be the number of inputs to each neuron, so that will now be the number of input channels times the kernel size times the kernel size if you want to do this exact same thing for convolutional networks.

需要说明的是，这个推导是针对全连接网络的，但对于卷积网络，DN将是每个神经元的输入数量，即输入通道数乘以卷积核尺寸的平方，如果你想对卷积网络应用相同的原理。

So then to derive this Xavier initialization, the trick is that we want to have the variance of the output be equal to the variance of the input.

推导Xavier初始化的关键在于，我们希望输出的方差等于输入的方差。

So the way that we set this up is we imagine that this linear layer is going to compute y equals the matrix multiply of weight matrix W and input activation vector X.

我们这样建立模型：假设这个线性层将计算y等于权重矩阵W与输入激活向量X的矩阵乘积。

Then each scalar element of our next layer, ignoring a bias, will be this inner product between one of the rows of W and the entire input vector X.

那么下一层的每个标量元素（忽略偏置）将是W的某一行与整个输入向量X的内积。

Now if we want to compute the variance of one of these outputs y_i, then if we make a simplifying assumption that all of the X's and all the W's are independent and identically distributed, then we can use the properties of the variance to simplify in this way.

如果我们想要计算某个输出y_i的方差，并且假设所有X和所有W都是独立同分布的，那么我们可以利用方差的性质进行这样的简化。

Then we know that the variance of each one of the elements of the output layer y_i will be equal to DN times the variance of x_i times w_i.

这样我们就知道输出层每个元素y_i的方差将等于DN乘以x_i的方差乘以w_i的方差。

Then we make a couple other simplifying assumptions: we assume that X and W are independent random variables, and when you take the variance of the product of two independent variables, you look up on Wikipedia what to do and you get a formula that looks something like this.

然后我们再做几个简化假设：假设X和W是独立随机变量，当你计算两个独立变量乘积的方差时，查阅维基百科会得到类似这样的公式。

The next assumption is that we make another simplifying assumption that all of the inputs X are 0 mean, which may be reasonable if they're coming out of a batch normalization layer and the previous layer, and we also assume that the W's are also going to be 0 mean, which may be reasonable if we assume that they were initialized from some kind of Gaussian distribution.

下一个假设是：我们进一步假设所有输入X的均值为零，如果它们来自批归一化层和前一层，这可能是合理的；同时我们也假设W的均值也为零，如果它们是从某种高斯分布初始化的，这也可能是合理的。

Then once we have all these assumptions, we can see that if we choose the magical value of variance of standard deviation equal to 1 over, rather if we set the variance of the w_i's to be 1 over DN, then we see that the variance of y_i is going to be equal to the variance of x_i.

在所有这些假设下，我们可以看到，如果我们选择标准差的神奇值为1/√DN，或者更准确地说，如果我们设置w_i的方差为1/DN，那么y_i的方差将等于x_i的方差。

So that motivates this derivation of the Xavier initialization: it's this idea of trying to match variances between the inputs of the layer and the outputs of the layer.

这就构成了Xavier初始化的推导动机：即试图匹配层输入与层输出之间的方差。

And we see that if we set the standard deviations in this very special way, then it will work out.

我们可以看到，如果我们以这种特殊方式设置标准差，那么就能达到预期效果。

But there's a problem: what about ReLU? Right, this whole derivation also hinges on the non-linearity.

但存在一个问题：ReLU激活函数呢？确实，整个推导还依赖于非线性函数。

And that's the reason why I actually use tanh in this experiment, because this derivation is only talking about the linear layer, but that linear will be followed by a non-linearity.

这就是为什么我在这个实验中实际使用tanh的原因，因为这个推导只讨论了线性层，但线性层后面会接一个非线性函数。

For something like tanh, it's symmetric and symmetric around zero, so as long as we match the variances of the linear layer, then things will generally be OK as we move through this center to non-linearity.

对于像tanh这样的函数，它是关于零对称的，因此只要我们匹配了线性层的方差，在通过这个中心点进入非线性函数时，情况通常会是良好的。

But for ReLU, things would be very bad if we do this exact same initialization and now use our deep network to be ReLU instead of tanh.

但对于ReLU，如果我们采用完全相同的初始化方法，并将深度网络改为使用ReLU而不是tanh，情况就会变得非常糟糕。

Then our activations statistics are again going totally out of whack here; these histograms look a little funny because there's a huge spike at zero because ReLU kills everything below zero, but if you kind of zoom in on the non-zero part of these histograms, you'll see that also for a deep network, all the activations are collapsing towards zero again, which will again give us zero local gradients and no learning.

这样我们的激活统计量又会完全失调；这些直方图看起来有点奇怪，因为在零处有一个巨大的尖峰，因为ReLU将所有负值置零，但如果你放大这些直方图的非零部分，你会发现在深度网络中，所有激活值再次坍缩向零，这将再次导致局部梯度为零，无法进行学习。

So to fix this, we need a slightly different initialization for ReLU nonlinearities, and the fix is just to multiply our standard deviation by square root of 2.

为了解决这个问题，我们需要为ReLU非线性函数采用稍有不同的初始化方法，解决方案就是将我们的标准差乘以√2。

So now for Xavier we have standard deviation of square root 1 over DN; now for ReLU we have standard deviation square root 2 over DN.

因此，对于Xavier初始化，我们有标准差为√(1/DN)；而对于ReLU，我们有标准差为√(2/DN)。

Intuitively, that factor of 2 is to deal with the fact that ReLU is killing off half of its inputs and setting them to 0.

直观上，这个因子2是为了处理ReLU将其一半的输入置零的事实。

And this is called a Kaiming initialization after the first author of the paper that introduced it, or sometimes MSRA initialization after Microsoft Research Asia where he worked when writing this paper.

这被称为Kaiming初始化，以提出该方法的论文第一作者命名；有时也称为MSRA初始化，以他撰写论文时工作的微软亚洲研究院命名。

Now this is so then when you're initializing with ReLU nonlinearities, you need to use this MSRA initialization for things to work out well.

因此，当你使用ReLU非线性函数进行初始化时，需要使用这种MSRA初始化方法才能获得良好效果。

And it turns out that actually, remember a couple of lectures ago we talked about VGG and that in 2014 there was no batch normalization and people could not get the VGG to converge without crazy tricks.

事实上，记得几节课前我们讨论过VGG，在2014年还没有批归一化，人们不采用一些复杂技巧就无法使VGG收敛。

Well, it turned out that actually once you have this Kaiming weight initialization scheme, this is sufficient for getting VGG to train from scratch.

事实证明，一旦你有了这种Kaiming权重初始化方案，就足以让VGG从零开始训练。

And actually, the paper that introduced this, their big claim to fame was that they got VGG to train from scratch by simply changing the initialization strategy.

实际上，提出该方法的论文之所以闻名，正是因为他们仅通过改变初始化策略就实现了VGG的从零训练。

But remember we've kind of moved on from VGG now, and we talked in the CNN architectures lecture that the kind of standard baseline architecture you should only consider is a residual network.

但请记住，我们现在已经超越了VGG，在CNN架构讲座中我们谈到，现在应该考虑的标准基线架构是残差网络。

And it turns out that this MSRA or Kaiming initialization is not very useful for residual networks.

事实证明，这种MSRA或Kaiming初始化对于残差网络并不十分有用。

And the way that we can see this is suppose we have a residual network and we've somehow constructed the initialization of those internal convolutions such that the variance of the output matches the variance of the input. Well, if we wrap those layers with a residual connection, now that means that the variance of the output after the residual connection will always be strictly greater because we're adding the input again. So that means that if we were to use this MSRA or Xavier initialization with the residual network, then we would expect the variances of our activations to grow and grow and grow over many layers of the network, and that would be very bad. That would lead to exploding gradients and bad optimization dynamics again.

我们可以这样理解：假设我们有一个残差网络，并且我们以某种方式构建了内部卷积层的初始化，使得输出的方差与输入的方差匹配。如果我们用残差连接包裹这些层，这意味着残差连接后的输出方差总是严格更大的，因为我们再次添加了输入。这意味着如果我们在残差网络中使用MSRA或Xavier初始化，那么我们会预期激活值的方差在网络的多层中不断增长，这将非常糟糕。这会导致梯度爆炸和糟糕的优化动态。

So the solution for residual networks is fairly simple. What we normally do for residual networks is to initialize the first layer with your MSRA initialization and then initialize the last layer of your residual block to be zero, because that means that at initialization this block will compute the identity function and the variances will be again perfectly preserved at initialization. So that's the trick, that's the way that we prefer to initialize residual networks.

因此残差网络的解决方案相当简单。我们通常对残差网络的做法是用MSRA初始化第一层，然后将残差块的最后一层初始化为零，因为这意味着在初始化时，这个块将计算恒等函数，方差在初始化时将再次完美保持。这就是技巧，这就是我们偏好初始化残差网络的方式。

I'd also point out that this whole area of how to initialize your neural networks and the reasons that you might prefer one initialization scheme over another is a super active area of research. You can see papers even from this year that are giving new ways to initialize neural networks.

我还要指出，如何初始化神经网络以及你可能偏好一种初始化方案而非另一种的原因，这是一个非常活跃的研究领域。你甚至可以看到今年发表的论文提出了初始化神经网络的新方法。

So are there any other questions on initialization? Yeah, that's not quite correct. So the idea was that maybe the idea with initialization is we just want to be as close as possible to the global minimum of the loss function, but before we start training we don't know where that minimum is. So instead, the perspective that we normally take on initialization is that we want to initialize in such a way that all the gradients will be well behaved at initialization.

关于初始化还有其他问题吗？是的，那个说法不太正确。有人认为初始化的想法是我们只想尽可能接近损失函数的全局最小值，但在开始训练之前我们不知道最小值在哪里。因此，我们通常对初始化的看法是，我们希望以这样的方式初始化，使得所有梯度在初始化时表现良好。

Because if you choose maybe bad initializations or bad initialization schemes, then you could end up with zero gradients or close to zero gradients right off the bat at the beginning of training, and then nothing will train and you won't make any progress towards the goal. The way I'd like to think about it is that we want to initialize in a place where it's not flat, so we prefer not to initialize in a local minima, and that's the main constraint that we want to take into account when constructing initialization schemes.

因为如果你选择了可能不好的初始化或糟糕的初始化方案，那么你可能在训练一开始就得到零梯度或接近零梯度，然后什么也训练不了，你也不会朝着目标取得任何进展。我喜欢这样思考：我们希望初始化在一个不平坦的地方，所以我们偏好不初始化在局部最小值，这是我们在构建初始化方案时需要考虑的主要约束。

So far we've talked about ways for getting your model to train. We set up the activation function, we set up the initialization, but then once you get your model to train, if you've done a really good job at optimizing it, you might see it start to overfit and it might start to perform better on the test set than it does on the training set, and this would be a bad thing.

到目前为止，我们已经讨论了让模型训练的方法。我们设置了激活函数，设置了初始化，但一旦你的模型开始训练，如果你在优化方面做得非常好，你可能会看到它开始过拟合，并且可能在测试集上的表现比训练集上更好，这将是一件坏事。

So now to overcome this, we need some strategies for regularization. We've already seen one simple scheme for regularization which is to add another term to the loss function. The very common one that you've used on your assignments so far is L2 regularization, also sometimes called weight decay, that just penalizes the L2 norm of your weight matrices. This is very common and this is a very widely used regularizer for deep neural networks as well.

因此，为了克服这个问题，我们需要一些正则化策略。我们已经看到一种简单的正则化方案，即向损失函数添加另一个项。你在作业中经常使用的非常常见的是L2正则化，有时也称为权重衰减，它只是惩罚权重矩阵的L2范数。这非常常见，也是深度神经网络中广泛使用的正则化器。

But there's a whole other host of other regularization schemes that people use in deep learning. One of the most famous is this idea called dropout. So dropout is kind of a funny idea. What we're going to say is that when we add dropout to a neural network, we're going to explicitly add some randomness to the way that the network processes the data.

但深度学习中还使用了大量其他正则化方案。其中最著名的是称为dropout的想法。Dropout是一种有点有趣的想法。我们要说的是，当我们在神经网络中添加dropout时，我们将明确地在网络处理数据的方式中添加一些随机性。

In each forward pass, we're going to randomly set some of the neurons in each layer equal to zero. So we're going to compute a layer, then randomly set some of the neurons to zero, then compute another layer, now randomly set some of those to zero, then compute another layer and so on and so forth. The probability of dropping any individual neuron is a hyperparameter, but a very common choice would be 0.5.

在每次前向传播中，我们将随机将每层中的一些神经元设置为零。因此，我们将计算一层，然后随机将一些神经元设置为零，然后计算另一层，再随机将其中一些设置为零，然后计算另一层，依此类推。丢弃任何单个神经元的概率是一个超参数，但一个非常常见的选择是0.5。

So any individual neuron has probability 1/2. We flip a coin whether or not to keep it or throw it away. Seems crazy right? But it's very simple to implement. This is implementing a two-layer fully connected neural network with dropout, and you can see that the implementation is very simple. We just compute this binary mask after each layer and use that to kill off half of the neurons in each layer after we compute the matrix multiply.

因此，任何单个神经元都有1/2的概率。我们抛硬币决定是保留还是丢弃它。看起来疯狂吧？但实现起来非常简单。这是在实现一个带有dropout的两层全连接神经网络，你可以看到实现非常简单。我们只需在每层之后计算这个二进制掩码，并在计算矩阵乘法后用它来消除每层中一半的神经元。

So then the question is why would you ever possibly want to do this? Well, one interpretation of what dropout is doing is that it forces the network to have a kind of redundant representation. Another way this is phrased is that we want to prevent the co-adaptation of features. So we want to encourage the network in some way to develop representations where different slots in that vector represent maybe different robust ways of recognizing the object.

那么问题是，你为什么可能想要这样做？嗯，对dropout作用的一种解释是，它迫使网络具有一种冗余表示。另一种表述方式是，我们希望防止特征的共同适应。因此，我们希望以某种方式鼓励网络发展表示，其中向量的不同槽位可能代表识别对象的不同鲁棒方式。

For example, if we are building a cat classifier, maybe a bad thing would be for each element of the vector to just learn independently whether it's a cat. But if we add dropout, then maybe we want it to learn more robust representations. Maybe some neurons should learn about ears and some should learn about furry, and different neurons should maybe focus on different high-level aspects of catness such that if we randomly knock out half of these neurons, then it can still robustly recognize the cat even if we mess with its representation.

例如，如果我们正在构建一个猫分类器，可能不好的情况是向量的每个元素只是独立学习它是否是猫。但如果我们添加dropout，那么可能我们希望它学习更鲁棒的表示。可能一些神经元应该学习耳朵，一些应该学习毛茸茸，不同的神经元可能应该关注猫性的不同高级方面，这样如果我们随机敲掉一半这些神经元，那么即使我们干扰其表示，它仍然可以鲁棒地识别猫。

Another interpretation of dropout is that it's effectively training a large ensemble of neural networks that all share weights. Because if you imagine this process of masking out half of the neurons in each layer, what we've effectively done is built a new neural network that is a subnetwork of the original full network. Now at each forward pass, we're going to train a separate subnetwork of this full network, and then somehow we're going to have this very large exponentially large number of subnetworks that all share weights, and then the full network will somehow be some ensemble of this very large number of subnetworks that are all sharing weights.

对dropout的另一种解释是，它实际上是在训练一个共享权重的大型神经网络集合。因为如果你想象这个屏蔽每层一半神经元的过程，我们实际上做的是构建了一个新的神经网络，它是原始完整网络的子网络。现在在每次前向传播中，我们将训练这个完整网络的一个单独子网络，然后以某种方式我们将拥有这个非常大、指数级数量的共享权重的子网络，然后完整网络将以某种方式成为这个非常大数量共享权重的子网络的某种集成。

So these are both admittedly hand-waving explanations of what dropout might be trying to do, and there's a whole host of theory papers that try to get more concrete explanations for why this works, but now a problem with dropout is that it makes the test time operation of the neural network actually random right because we were randomly knocking out half the neurons in each layer at each forward pass. And this seems bad because if you're deploying these networks in practice, you'd like their outputs to be deterministic. You wouldn't want to upload a photo to your web hosting service one day and have it recognized as a cat, and the next day the same photo is recognized as something else. That would be a bad property for your neural networks when they're deployed in practice.

因此，这两种都是对dropout可能试图实现的目标的 admittedly 粗略解释，并且有大量理论论文试图对其工作原理进行更具体的解释，但现在dropout存在一个问题：它使得神经网络在测试时的操作实际上变得随机，因为我们在每次前向传播时随机屏蔽了每层中一半的神经元。这看起来很不理想，因为如果在实际应用中部署这些网络，你希望它们的输出是确定性的。你不希望某天上传照片到网络托管服务时被识别为猫，而第二天同一张照片却被识别为其他东西。这对于实际部署的神经网络来说是个不良特性。

So at test time we want some way to make dropout deterministic, and what we really want to do is kind of average out this randomness. Because now once we've built dropout into our neural network, we can imagine that we're rewriting our neural network to actually take two inputs: one is our actual input image X, and the other is this random mask Z which is some random variable that we are drawing before we run the forward pass of the network. And now the output that our network computes is dependent both on the input data as well as on this random variable.

因此在测试时，我们需要某种方法使dropout变得确定，而我们真正想做的是平均掉这种随机性。因为现在一旦将dropout构建到神经网络中，我们可以想象重写神经网络使其实际接受两个输入：一个是实际输入图像X，另一个是随机掩码Z——这是我们在运行网络前向传播之前抽取的某个随机变量。现在网络计算的输出既依赖于输入数据，也依赖于这个随机变量。

So then in order to make the network deterministic, what we want to do is somehow average out the randomness at test time. And what we can do is then take the test time forward pass be this expectation of averaging out this random variable. So then if we want to compute this analytically, we just want to compute this integral to marginalize out this random variable Z. But in practice actually computing this integral analytically is like we have no idea how to do it - that seems very hard and very intractable for arbitrary neural networks.

因此为了使网络具有确定性，我们需要在测试时以某种方式平均掉随机性。我们可以让测试时的前向传播成为对这个随机变量求平均的期望值。如果我们要解析计算这个值，只需要计算这个积分来边缘化随机变量Z。但实际上解析计算这个积分对我们来说几乎不可能——对于任意神经网络来说，这似乎非常困难且难以处理。

So in practice we will instead think about what happens - how can we think about this integral for only a single neuron? Well for a single neuron that receives two inputs x and y and has these connection strengths w1 and w2 and then produces this single scalar output a, then at the normal forward pass we might imagine doing at test time is to take this inner product of the weight matrix and the two inputs x and y.

因此在实践中，我们会转而考虑单个神经元的情况——如何思考单个神经元的这个积分？对于一个接收两个输入x和y、具有连接强度w1和w2并产生单个标量输出a的神经元，在正常前向传播中，我们可能想象在测试时进行权重矩阵与两个输入x和y的内积。

Now if we're using dropouts then there are four different random masks that we might have drawn during training with equal probability: where we could have kept them both, we could have knocked out x but kept y, knocked out y but kept x, or knocked out both of them. And each of these four options can occur with equal probability. So then in this case we can write out this expectation exactly because we've got exactly four outputs and they all have equal probability.

如果我们使用dropout，那么在训练期间可能有四种不同的随机掩码以相等概率被抽取：保留两个输入、屏蔽x但保留y、屏蔽y但保留x，或者屏蔽两个输入。这四种情况都以相等概率发生。因此在这种情况下，我们可以精确写出这个期望值，因为我们正好有四个输出且它们都具有相等概率。

And what we see is that for this example, this output - this expected output - is actually equal to 1/2 the probability of this normal forward pass. And this turns out to hold in general that if we want to compute the expectation of a single layer with dropout, then all we need to do is multiply by the dropout probability. So simply multiplying by the dropout probability allows us to compute this expectation for a single dropout layer.

我们看到在这个例子中，这个期望输出实际上等于正常前向传播概率的1/2。这通常成立：如果要计算带dropout的单个层的期望值，我们只需要乘以dropout概率。因此简单乘以dropout概率就能计算单个dropout层的期望值。

So that means that at test time we want the output to be equal to the expected input at training time, which means that at test time we want all neurons to be active - we want them all to actually do something. So at test time we'll use all the neurons, but then we'll rescale the output of the layer using this dropout probability that we've used to drop individual neurons.

这意味着在测试时，我们希望输出等于训练时期的期望输入，也就是说在测试时我们希望所有神经元都处于活跃状态——希望它们都能实际发挥作用。因此在测试时我们将使用所有神经元，但会使用之前用于丢弃单个神经元的dropout概率来重新缩放该层的输出。

So this gives us our summary of implementing dropout: it's actually quite straightforward that when implementing dropout during the forward pass during training, we're going to generate these random masks and use those to dropout or zero out random elements of the activation factors. And at test time we'll simply use the probability to rescale the output and have no randomness.

这就给出了实现dropout的总结：实际上相当简单——在训练期间的前向传播中实现dropout时，我们将生成这些随机掩码并用它们来丢弃或清零激活因子的随机元素。而在测试时，我们将简单地使用概率来重新缩放输出，且没有任何随机性。

Now this expectation is exact only for individual layers, and this kind of way of computing expectations is not actually correct if you imagine stacking multiple dropout layers on top of each other. But it seems to work well enough in practice. I'd also like to point out that a slightly more common thing that you'll see for implementing dropout is another variant called inverted dropout that's really fundamentally the same idea - it's just a different implementation of the same thing.

这种期望计算仅对单个层是精确的，如果想象将多个dropout层堆叠在一起，这种计算期望的方式实际上并不正确。但在实践中似乎足够有效。我还想指出，实现dropout时更常见的是一种称为逆dropout的变体，其基本思想完全相同——只是同一事物的不同实现方式。

And the question is: where do we want to do the rescaling? We want to do rescaling during test time or we want to do rescaling during training time? And maybe we would prefer to not do rescaling during test time because during test time we want to really maximize the efficiency of the system because maybe it's gonna run on mobile devices or in servers on lots of images or whatever. So we maybe prefer to pay a little bit of extra cost of training time instead.

问题在于：我们希望在哪里进行重新缩放？是在测试时进行重新缩放，还是在训练时进行重新缩放？我们可能更倾向于不在测试时进行重新缩放，因为在测试时我们想要真正最大化系统效率，可能系统会在移动设备或服务器上处理大量图像等。因此我们可能更愿意在训练时付出一些额外成本。

So in practice a very common thing you'll see with dropout is to generate these random masks at training time, and then actually then like if you're having dropout probability 1/2, then during training time we'll drop half the neurons and then we'll multiply all of the remaining neurons by 2. And then at test time we'll just use all the neurons and use our normal weight matrix. And in this way again the expected value of the output at training time will be equal to the actual output at test time, but it's just a question of where do we put the rescaling - we put it during training time or test time.

因此在实践中，dropout的一个非常常见的做法是在训练时生成这些随机掩码，比如如果dropout概率为1/2，那么在训练时我们将丢弃一半神经元，然后将所有剩余神经元乘以2。而在测试时，我们只需使用所有神经元和正常权重矩阵。通过这种方式，训练时输出的期望值将再次等于测试时的实际输出，但这只是重新缩放放在哪里的问题——是放在训练时还是测试时。

Then the question: there's another question of now that we've got this idea of a dropout layer, where do we actually insert it into our neural network architectures? Well if we remember back to the AlexNet and VGG architectures, we remember that the vast majority of the learnable parameters for those architectures lived in these fully connected layers at the end of the network. And that's indeed the exact place where we tend to put dropout in practice - is in these large fully connected layers at the end of our convolutional neural networks.

那么问题来了：既然我们已经有了dropout层的概念，我们实际应该将其插入到神经网络架构的什么位置？如果我们回顾AlexNet和VGG架构，会记得这些架构中绝大部分可学习参数位于网络末端的全连接层中。这确实是我们实践中倾向于放置dropout的位置——就在卷积神经网络末端的大型全连接层中。

But if you'll remember that as we moved forward in time and looked at more recent architectures, things like ResNet or GoogleNet actually did away with these large fully connected layers and instead use global average pooling instead. So actually for these later network architectures, they did not use dropout at all. But prior to 2014 or so, dropout was really a critical essential piece of getting neural networks to work because it actually helped a lot for reducing overfitting in something like AlexNet or VGG.

但如果你记得，随着时间推移观察更近期的架构，如ResNet或GoogleNet实际上摒弃了这些大型全连接层，转而使用全局平均池化。因此实际上对于这些后期网络架构，它们根本没有使用dropout。但在2014年之前，dropout确实是让神经网络正常工作的关键要素，因为它实际上对减少过拟合有很大帮助，比如在AlexNet或VGG这样的网络中。

They've actually become slightly less important in these more modern architectures like ResNets and so on and so forth.

在更现代的架构如ResNet等网络中，dropout实际上变得不那么重要了。

Now this idea of dropout is actually something of a common pattern that we see repeated a lot in different types of neural network regularization.

现在dropout这个概念实际上是一种常见模式，我们在不同类型的神经网络正则化中经常看到这种模式的重复。

Basically during training we've added some kind of randomness to the system by adding a dependence on some other random source of information, and then at testing we average out the randomness to make a deterministic prediction.

基本上在训练期间，我们通过添加对其他随机信息源的依赖来向系统添加某种随机性，然后在测试时我们平均掉这种随机性以做出确定性预测。

For dropout, this randomness took the source of random masks, but you can see many other types of regularization that use other sorts of randomness instead.

对于dropout，这种随机性来源于随机掩码，但你可以看到许多其他类型的正则化方法使用其他形式的随机性。

We've actually seen another regularizer already in this class that has like this exact same flavor, and that regularizer is batch normalization.

我们实际上已经在本课程中看到了另一个具有完全相同特点的正则化器，那就是批量归一化。

Because if you recall, batch normalization adds randomness during training time because it makes the outputs of each elements in the batch depend on each other elements in the batch.

因为如果你还记得，批量归一化在训练期间添加了随机性，因为它使得批次中每个元素的输出依赖于批次中的其他元素。

During training, for batch normalization we compute these per mini-batch means and standard deviations which means that they depend on which random elements happens to get shuffled into each mini-batch at each iteration of training.

在训练期间，对于批量归一化，我们计算每个小批次的均值和标准差，这意味着它们取决于在每次训练迭代中哪些随机元素恰好被混洗到每个小批次中。

So then batch normalization is adding randomness by depending on which batch we form at training time, and then at test time that averages out this randomness by using these running averages of means and standard deviations and using these fixed values instead.

因此批量归一化通过依赖于我们在训练时形成的批次来添加随机性，然后在测试时通过使用这些运行平均值和标准差并使用这些固定值来平均掉这种随机性。

In fact for these later architectures like residual networks and other types of more modern architectures, batch normalization has somewhat replaced dropout as the main regularizer in these deep neural networks.

事实上，对于这些较新的架构如残差网络和其他类型的现代架构，批量归一化已经在某种程度上取代了dropout，成为这些深度神经网络中的主要正则化器。

So for something like a residual Network, the regularizer is that it's used to train our L2 weight decay and batch normalization and that's it.

因此对于像残差网络这样的架构，使用的正则化器就是L2权重衰减和批量归一化，仅此而已。

That tends to be actually a very useful successful way to successfully train large deep neural networks is actually just relying on the stochasticity of batch normalization.

这实际上是一种非常有用的成功训练大型深度神经网络的方法，就是仅仅依赖于批量归一化的随机性。

But there's some actually there I lied a little bit, there's one other type of thing of one other source of randomness that happens a lot in practice but most people don't refer to it as a type of regularization and that's this notion of data augmentation.

但事实上我在这里有点说谎，还有另一种随机性来源在实践中经常出现，但大多数人并不将其称为一种正则化，那就是数据增强的概念。

So far in this class whenever we've talked about loading and training iterations, we always imagine that we load up our training data loading load up the label for that training data like this picture of a cat and label cat, we're on the image through the network and then compare the predictive label to the true label and then use that to get our loss and compute our gradients.

到目前为止，在本课程中每当我们讨论加载和训练迭代时，我们总是想象我们加载训练数据和对应的标签，比如一张猫的图片和猫的标签，我们将图像通过网络，然后将预测标签与真实标签进行比较，然后使用这个来计算损失和梯度。

But it's actually very common in practice to perform transforms on your data samples before you feed them to the neural network and perform and somehow manipulate or modify the input image in a random way that preserves the label of the data sample.

但实际上在实践中非常常见的是，在将数据样本输入神经网络之前对其执行变换，以某种随机方式操作或修改输入图像，同时保留数据样本的标签。

So for example for images, some common things would be horizontal flips because we as humans know that if we flip the image horizontally then it's still a cat.

例如对于图像，一些常见的操作包括水平翻转，因为我们作为人类知道，如果我们水平翻转图像，它仍然是一只猫。

Other common things would be random crops and scales so at every training iteration we might resize the image to have a random size or take a random crop of the image because we expect that a random crop of a cat image should still be a cat image and should still be recognized as a cat by the neural network.

其他常见的操作包括随机裁剪和缩放，因此在每个训练迭代中，我们可能会将图像调整为随机大小或对图像进行随机裁剪，因为我们期望猫图像的随机裁剪应该仍然是猫图像，并且应该仍然被神经网络识别为猫。

The idea here is that this adds a way this effectively multiplies your training set because we add these data transformations to the model in a way that we know doesn't change the training label.

这里的想法是，这实际上增加了你的训练集，因为我们以我们知道不会改变训练标签的方式向模型添加这些数据变换。

This somehow multiplies your training set for free because now your network is trained on more raw inputs.

这在某种程度上免费地增加了你的训练集，因为现在你的网络在更多的原始输入上进行训练。

But this is again adding randomness at training time so for the answer this example of random cropping and flipping and scaling for something like ResNets the way that they're trained is that at every iteration every training image we pick a random size resize it to a random size then take a random two to four by two to four crop of this randomly resized image.

但这再次在训练时添加了随机性，因此对于像ResNet这样的网络，它们的训练方式是：在每个迭代中，对于每个训练图像，我们选择一个随机大小，将其调整为随机尺寸，然后对这个随机调整大小的图像进行2x2到4x4的随机裁剪。

We just do this random cropping and random resizing and random flipping for each element at every iteration.

我们在每个迭代中对每个元素执行这种随机裁剪、随机调整大小和随机翻转。

But now again this is adding randomness to the network in some way so we want to fitting with this idea of marginalizing out randomness at test time, we want some way to marginalize out this other source of randomness in our neural network.

但现在这再次以某种方式向网络添加了随机性，因此为了符合在测试时边缘化随机性的理念，我们需要某种方法来边缘化神经网络中的这种其他随机性来源。

So then the way this is done in data augmentation is to pick some fixed set of crops or scales to evaluate at a test time.

因此在数据增强中实现这一点的方法是选择一些固定的裁剪或缩放集合在测试时进行评估。

For example in the ResNet paper, they take these five different image scales and then for each scale they evaluate five crops of the image for the four-corners and the center as well as the horizontal flips of all these things and then average the predictions after running each of those different random crops through the network.

例如在ResNet论文中，他们采用五种不同的图像尺度，然后对于每个尺度，他们评估图像的五个裁剪区域（四个角落和中心）以及所有这些的水平翻转，然后在通过网络运行每个不同的随机裁剪后平均预测结果。

Again and again this is a way that we are adding randomness to the network at training time and then averaging out that randomness at test time.

这反复表明了一种方式：我们在训练时向网络添加随机性，然后在测试时平均掉这种随机性。

There's some other other times people play tricks around also randomly jittering the color of your images during training time.

有时人们还会玩一些技巧，比如在训练期间随机抖动图像的颜色。

But in general this idea of data augmentation is a way that you can get creative and add some of your own human expert knowledge to the system because depending on the problem you're trying to solve different types of data augmentation might or might not make sense.

但总的来说，数据增强这个概念是一种你可以发挥创造力并将自己的人类专家知识添加到系统中的方式，因为根据你试图解决的问题，不同类型的数据增强可能合理也可能不合理。

So like for example if you were building a classifier to try to tell right and left hands apart then probably horizontal flipping would not be a good type of data augmentation to use.

例如，如果你正在构建一个分类器来区分左右手，那么水平翻转可能不是一种好的数据增强类型。

But if we want to recognize cats versus dogs then horizontal flipping is maybe very reasonable.

但如果我们想要识别猫和狗，那么水平翻转可能是非常合理的。

Or sometimes I've seen like in like medical imaging context we want to recognize like slides or cells then even performing random rotations is reasonable because depending on that for that source of data you don't really know what orientation it might have come in at.

或者有时我在医学成像环境中看到，我们想要识别切片或细胞，那么甚至执行随机旋转也是合理的，因为对于那种数据源，你并不真正知道它可能以什么方向出现。

So data augmentation is really a place where you can inject some of your own human expert knowledge into the training of the system about what types of transformations do and do not affect the labels that you're trying to predict.

因此数据增强确实是一个你可以将自己的人类专家知识注入系统训练的地方，关于哪些类型的变换会影响你试图预测的标签，哪些不会。

So now we've seen this pattern in regularization of adding randomness at training time and then marginalizing out the randomness at test time. I just want to very quickly go through a couple other examples of this pattern but I don't expect you to know in detail, just to give you a flavor of other ways this has been instantiated. One way is drop connect, which is very similar to drop out but rather than zeroing random activations, instead we're going to zero random weights during every forward pass through the network, and again we'll have some procedure to average out test elasticity at test time.

所以现在我们已经在正则化中看到了这种模式：在训练时添加随机性，然后在测试时边缘化这种随机性。我想快速介绍几个这种模式的其他实例，但不要求大家掌握细节，主要是让大家了解这种模式的其他实现方式。其中一个想法是Drop Connect，这与Dropout非常相似，但不同之处在于我们不是在每次前向传播时将随机激活置零，而是将网络中的随机权重置零，在测试时我们同样会有某种程序来平均测试弹性。

Another idea is this that I find very cute is this notion of fractional max pooling so here what we're going to do is actually randomize the sizes of the receptive fields of our pooling regions inside each of the pooling layers of the neural network so that maybe some neurons will have a two by two portaling region some neurons will have a one by one pooling region and they'll be and every four pass and this is fractional max pooling because this randomness between choosing a one-by-one receptive field and a two-by-two receptive field means you can have like 1.35 pooling in expectation so this is sometimes referred to as fractional max pooling.

另一个我觉得很巧妙的想法是分数最大池化，我们要做的是在神经网络的每个池化层中随机化池化区域的感受野大小，这样有些神经元可能拥有2x2的池化区域，有些神经元可能拥有1x1的池化区域，每次前向传播都会不同。这被称为分数最大池化，因为在选择1x1感受野和2x2感受野之间的随机性意味着在期望值上你可以实现类似1.35的池化效果。

Another crazy one is we can actually build networks deep networks with stochastic death so we can build something like a hundred layer ResNet and then every four pass will use a different subset of the residual blocks during training and then a test time will use all of the blocks so we saw kind of drop out that's dropping weights dropping individual neuron values we saw a drop connect that's dropping individual weight values this is something like drop block it's dropping whole blocks from this deep residual network architecture.

另一个更疯狂的想法是我们可以构建具有随机死亡的深度网络，比如构建一个百层ResNet，在训练时每次前向传播使用不同的残差块子集，而在测试时使用所有块。我们之前看到了Dropout是丢弃权重和单个神经元值，Drop Connect是丢弃单个权重值，而这种类似Drop Block的方法是从深度残差网络架构中丢弃整个块。

Another one that's actually more commonly used is this notion of cutout so here we're simply going to 0 set random image regions of the input image to 0 at every at every four pass during training and then at test time we'll use the whole image instead and again you can see that we're performing some kind of randomness to corrupt the the neural network at training time and then averaging out that one that that randomness at test time.

另一个更常用的方法是Cutout，我们在训练时每次前向传播简单地将输入图像的随机区域置零，而在测试时使用完整图像。你可以看到我们都是在训练时通过某种随机性来干扰神经网络，然后在测试时平均掉这种随机性。

And now this last one is really crazy that I can't believe it works it's called mix up so here with mix up what we're going to do is train on random blends of training images so now rather than training on just a single image at a time we'll form our training samples by taking a cat image and a dog image and then blending them and with a with a random blend weight and then the predicted target should now be like 0.4 cat and 0.6 dog where that that that ran that that that target is now going to be equal to the blend weight and this actually seems totally crazy like how can this possibly work but the reason that it may maybe seems slightly more reasonable is that these blend weights are actually drawn from a beta distribution which we have the the PDF of the beta distribution up there which means that in practice these blend weights are actually very close to zero or very close very close to one so in practice rather than being 0.4 cat and 0.6 dog it's more likely to be like 0.95 cat and 0.5 dog so it's not as crazy as it initially seems.

最后一个方法非常疯狂，我简直不敢相信它能工作，它叫做Mixup。使用Mixup时，我们要在训练图像的随机混合上进行训练，不是每次只训练单张图像，而是通过取一张猫图像和一张狗图像，用随机混合权重进行混合来形成训练样本，预测目标应该是0.4猫和0.6狗，其中目标等于混合权重。这看起来完全疯狂，怎么可能有效？但让它看起来稍微合理的原因是这些混合权重实际上是从贝塔分布中抽取的，这意味着在实践中这些混合权重实际上非常接近0或非常接近1，所以实践中更可能是0.95猫和0.05狗，而不是0.4猫和0.6狗，因此它没有最初看起来那么疯狂。

But then kind of my might your kind of takeaways for which regular answers should you actually use in practice is that you should consider drop out if you have an architecture if you're facing an architecture that has very very large fully connected layers but for but but otherwise drop out is really not used quite so much these days and in practice batch normalization l28 decay and data and data augmentation are the main ways that we are regularizing neural networks in practice today and actually surprisingly these two these two wild ones of cut out and mix up actually end up being fairly useful for small data sets like CFR 10 so I think state of the art and see part n actually uses both of these techniques but for larger data sets like image net then these drop up these cut up cut out and mix up are usually not so helpful.

关于在实践中应该使用哪些正则化方法，我的建议是：如果你的架构包含非常大的全连接层，可以考虑使用Dropout，但除此之外，如今Dropout确实不太常用了。在实践中，批归一化、L2权重衰减和数据增强是我们今天正则化神经网络的主要方式。实际上令人惊讶的是，Cutout和Mixup这两种疯狂的方法最终在像CIFAR-10这样的小数据集上相当有用，我认为最先进的CIFAR模型实际上同时使用了这两种技术，但对于像ImageNet这样更大的数据集，这些方法通常没有那么大的帮助。

So that gives us our summary of part one of a lot of these nitty gritty details about choices you need to make when training neural networks and then on Wednesdays lecture we'll we'll talk about some more of these details that you need to know in order to get your neural networks to Train.

这让我们总结了很多关于训练神经网络时需要做出选择的细节的第一部分，在周三的讲座中，我们将讨论更多你需要知道的细节，以便让你的神经网络成功训练。