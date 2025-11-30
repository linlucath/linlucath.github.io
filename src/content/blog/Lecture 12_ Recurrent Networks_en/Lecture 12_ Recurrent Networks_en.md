---
title: 'Lecture 12_ Recurrent Networks_en'
publishDate: 2025-11-30
description: 'TODO'
tags:
  - TODO
language: 'English'
heroImage: { src: './default.jpg', color: '#D58388' }
---

Okay, welcome back. Today we are on lecture twelve, and today we're going to talk about a new species of neural network called recurrent neural networks.

好的，欢迎回来。今天我们进入第十二讲，我们将要讨论一种新型神经网络——循环神经网络。

So before we talk about recurrent neural networks, I wanted to back up and remember we had this slide from a couple lectures ago where we were talking about hardware and software, and this was kind of our TL;DR conclusions about PyTorch versus TensorFlow.

在讨论循环神经网络之前，我想先回顾一下。几讲前我们讨论硬件和软件时有一张幻灯片，那是我们关于PyTorch与TensorFlow的简要结论。

If you'll recall, kind of my biggest gripes with PyTorch as of that lecture were that it didn't have good TPU support for Google's specialized tensor processing unit hardware, and it did not have good support for exporting your trained models onto mobile devices like iOS and Android devices.

大家可能还记得，当时我对PyTorch最大的不满是它对谷歌专用张量处理单元硬件的TPU支持不佳，并且对将训练好的模型导出到iOS和Android等移动设备上的支持也不够好。

Well, since that lecture, PyTorch has actually released a new version 1.3, and two of the biggest features in the new version of PyTorch actually address these two big concerns that I had with PyTorch.

自那讲之后，PyTorch实际上发布了新版本1.3，而新版本中的两大功能正好解决了我对PyTorch的这两个主要担忧。

So PyTorch 1.3 now offers some experimental mobile API that theoretically makes it really easy to export your models and run them on mobile devices, which seems awesome.

PyTorch 1.3现在提供了一些实验性的移动API，理论上可以非常轻松地导出模型并在移动设备上运行，这看起来非常棒。

And there's now also experimental support for running PyTorch code on Google TPUs, which also seems really cool.

同时，现在还实验性地支持在谷歌TPU上运行PyTorch代码，这也非常酷。

So I just thought it's nice to keep you guys up to date when things change in the field of deep learning.

所以我觉得，在深度学习领域情况发生变化时，让大家了解最新进展是很好的。

As you can see, things are changing even within the scope of one semester, and some of our earlier lecture content becomes outdated even just a week or two later sometimes.

如您所见，即使在一个学期内，情况也在不断变化，我们之前的一些讲座内容有时甚至在一两周后就过时了。

But we're gonna - so this is the new PyTorch version 1.3 - we're gonna continue sticking with version 1.2 for the rest of this quarter, unless Colab silently updates to 1.3 on us again, which may happen, I don't know.

但我们将继续在本季度剩余时间内使用1.2版本，除非Colab再次静默更新到1.3，这可能会发生，我也不确定。

So these are really cool new features, but like I said, this was just released, so it's gonna take a little bit of time I think before we see whether or not these new features are really as awesome as they're promised to be.

这些新功能确实很酷，但正如我所说，这是刚刚发布的，所以我认为需要一些时间才能看到这些新功能是否真的像承诺的那样出色。

So then, kind of stepping back to last lecture, remember the last two lectures we've been talking about all these nuts and bolts strategies for how you can actually train your neural networks.

那么，回顾上一讲，记得过去两讲我们一直在讨论所有这些关于如何实际训练神经网络的具体策略。

So we talked in great detail about things like activation functions, data pre-processing, weight initialization, and many other strategies and little details that you need to know in order to train your neural networks.

我们详细讨论了激活函数、数据预处理、权重初始化以及许多其他策略和细节，这些都是训练神经网络需要了解的。

So now, hopefully by this point in the class, you guys are all experts at training deep convolutional neural networks for whatever type of image classification problem I might want to throw at you.

所以现在，希望到课程的这一阶段，你们都已经成为训练深度卷积神经网络的专家，能够应对我可能提出的任何类型的图像分类问题。

So since you are now experts at that problem, it's now the time in the semester when we need to start thinking about new types of problems that we can solve with deep neural networks.

既然你们现在已经是该问题的专家，那么现在是时候开始思考我们可以用深度神经网络解决的新型问题了。

So that brings us to today's topic of recurrent neural networks.

这就引出了我们今天的话题：循环神经网络。

So basically, all the problems, all the applications we've considered of deep neural networks in this class so far, have been what is called a feed-forward network.

基本上，到目前为止我们在本课程中考虑的所有深度神经网络问题和应用，都属于所谓的前馈网络。

Now these feed-forward networks are something that receives some single input at the bottom of the network, like a single image, goes through one or multiple hidden layers, maybe with special fancy layers like convolution or batch normalization, but each layer sort of feeds into the next layer, and at the very end of the network, it's going to output some single output.

这些前馈网络在网络的底部接收某个单一输入，比如单张图像，经过一个或多个隐藏层，可能包含卷积或批量归一化等特殊层，但每一层都会馈入下一层，在网络的最后，它会输出某个单一结果。

The classical example of these kind of feed-forward networks are these image classification networks that we've been working with so far.

这类前馈网络的典型例子就是我们一直在使用的图像分类网络。

Here there's a single input which is the image, and there's a single output which is the category label that we want our network to assign to that image.

这里，单一输入是图像，单一输出是我们希望网络分配给该图像的类别标签。

Now, as we've been, the reason we covered image classification in such detail is because I think it's a really important problem that encapsulates a lot of important features of deep learning.

我们如此详细地介绍图像分类的原因，是因为我认为这是一个非常重要的问题，它包含了深度学习的许多重要特征。

But there's a whole lot of other types of problems that we might imagine wanting to solve using deep neural networks.

但是，我们可能还想要用深度神经网络解决许多其他类型的问题。

So for example, we might want to have problems that are not one-to-one but instead are one-to-many, so where the input is a single input like maybe a single image, and the output is no longer a single label but maybe the output is a sequence.

例如，我们可能遇到的问题不是一对一的，而是一对多的，即输入是单一输入，比如单张图像，而输出不再是单一标签，而可能是一个序列。

An example of this would be a task like image captioning, where we want to have a neural network to look at an image and then write a sequence of words that describe the content of the image in natural language.

这方面的一个例子是图像描述任务，我们希望神经网络观察一张图像，然后用自然语言写出一系列描述图像内容的词语。

Now you can imagine this would be much more general than this single label image classification problem that we've considered so far.

你可以想象，这将比我们迄今为止考虑的单标签图像分类问题通用得多。

Another type of application we might imagine is a many-to-one problem, where now maybe our input is no longer a single item like an image, but maybe our input is a sequence of items.

我们可能设想的另一种应用类型是多对一问题，此时我们的输入可能不再是像图像这样的单一项目，而可能是一个项目序列。

For example, a sequence of video frames that make up a video, and now at the end we then maybe want to assign a label to this sequence of inputs.

例如，构成视频的一系列视频帧，最后我们可能希望为这个输入序列分配一个标签。

Maybe we want to look at the of a video sequence and then say maybe what type of event is happening in that video.

也许我们想观察视频序列，然后判断视频中正在发生什么类型的事件。

So this would be an example of a many-to-one problem because the input is a sequence and the output is a single label.

所以这将是一个多对一问题的例子，因为输入是一个序列，而输出是单一标签。

Of course, you can generalize this. You can imagine problems that are many-to-many that want to input a sequence and then output a sequence.

当然，你可以对此进行推广。你可以想象多对多的问题，即输入一个序列，然后输出一个序列。

An example of this type of problem would be something like machine translation, where we want to have neural networks that can input a sentence in English, so that would be a sequence of words in English, and then output a translation of that sentence to French, which would then be a sequence of words in French.

这类问题的一个例子是机器翻译，我们希望神经网络能够输入一个英语句子，即一个英语单词序列，然后输出该句子的法语翻译，即一个法语单词序列。

And again, and that's sort of on every for passed the network, we might have seek input sequences of different lengths and output sequences of different lengths.

同样，对于通过网络传递的每个序列，我们可能会遇到不同长度的输入序列和不同长度的输出序列。

This would be an example of something we call a many-to-many problem or a sequence-to-sequence problem.

这将是我们称之为多对多问题或序列到序列问题的一个例子。

There's another sort of sequence-to-sequence problem where maybe we want to process an input sequence and then we want to make a decision for each element of that input sequence.

还有另一种序列到序列问题，我们可能希望处理一个输入序列，然后为该输入序列的每个元素做出决策。

This is another example of a type of many-to-many classification problem.

这是多对多分类问题的另一个例子。

So here an example might be processing frames of a video, and rather than making a single classification decision about the entire content of the video, maybe instead we want to make a decision about the content of each frame of the video.

这里的一个例子可能是处理视频帧，我们不是对整个视频内容做出单一分类决策，而是希望对视频的每一帧内容做出决策。

And maybe say that the first three frames were someone dribbling a basketball, the next 10 frames were someone shooting a basketball, the next frame was him missing the shot, and then the next couple frames were him being booed by his team, something like that.

也许可以说前三帧是某人运球，接下来的十帧是某人投篮，再下一帧是他投篮未中，然后接下来的几帧是他被队友喝倒彩，诸如此类。

So maybe we'd like to build this network in a way that can process sequences. You can see that once we have the ability to work not just with single input and single output, but now if we have the ability to process sequences of inputs and sequences of outputs, that allows us to build neural networks that can do much more general types of things.

或许我们希望以能够处理序列的方式构建这个网络。你可以看到，一旦我们不仅能够处理单一输入和单一输出，而且现在如果具备处理输入序列和输出序列的能力，这就能让我们构建能够处理更通用类型任务的神经网络。

Now the general tool that we have in deep learning for working with sequences at the input and the output level is a recurrent neural network. So the rest of the lecture will talk about how whenever you see a problem that involves sequences at the input or sequences at the output, you might consider using some kind of recurrent neural network to solve that problem.

现在我们在深度学习中处理输入和输出层面序列的通用工具是循环神经网络。因此本讲剩余部分将讨论：每当你遇到涉及输入序列或输出序列的问题时，都可以考虑使用某种循环神经网络来解决该问题。

An important point here is that for all these problems we might not know the sequence length ahead of time. For each of these tasks we want to build one neural network that can process sequences of arbitrary length. So we'd like people to use the same video classification network to process a very short video frame or a very long video sequence.

这里一个重要点是，对于所有这些问题，我们可能无法提前知道序列长度。对于每个任务，我们都希望构建一个能够处理任意长度序列的神经网络。因此我们希望人们使用相同的视频分类网络来处理非常短的视频帧或非常长的视频序列。

Then this recurrent neural network will be this very general tool that allows us to process different types of sequences in deep learning problems. But recurrent neural networks are actually useful even for processing non sequential data.

那么循环神经网络将成为这种非常通用的工具，使我们能够在深度学习问题中处理不同类型的序列。但循环神经网络实际上对于处理非序列数据也很有用。

It turns out that sometimes people like to use recurrent neural networks to perform sequential processing of non sequential data. As an example, this is a project from a couple years ago where they were doing our favorite image classification tasks.

事实证明，有时人们喜欢使用循环神经网络来对非序列数据执行顺序处理。举个例子，这是几年前的一个项目，他们当时在进行我们最喜欢的图像分类任务。

Remember image classification has no sequences involved - it's just a single image as input and a single category label as output. But the way that they want to classify images is actually not with a single feed-forward neural network.

记住图像分类不涉及序列——它只是单个图像作为输入，单个类别标签作为输出。但他们想要分类图像的方式实际上不是使用单一的前馈神经网络。

Instead they want to build a neural network that can take multiple glimpses at the image - it maybe wants to look at one part of the image then look at another part of the image. At each point in time, the decision of where to look in the image is conditioned upon all the information that it's extracted at all previous time steps.

相反，他们想要构建一个能够对图像进行多次瞥视的神经网络——它可能想要先看图像的一个部分，然后看另一个部分。在每个时间点，关于在图像中查看哪里的决策都取决于它在所有先前时间步提取的所有信息。

Then after looking at many glimpses in the image, the network finally makes some classification decision about what is the object that it's seeing in the image. This is an example of using sequential processing inside of a neural network even to process non sequential data.

在查看图像的多次瞥视后，网络最终对其在图像中看到的物体做出分类决策。这是在神经网络内部使用顺序处理来处理非序列数据的例子。

This visualization shows that it's doing digit classification and each of these little green squares is one of the glimpses that the neural network is choosing to make to look at one little sub portion of the image in order to make its classification decision.

这个可视化显示它正在进行数字分类，每个小绿方块都是神经网络选择进行的瞥视之一，用于查看图像的一小部分区域以做出分类决策。

Now another example of using sequential processing for non sequential data is doing the inverse and generating images. Here rather than trying to classify, we have some tasks where we want to build neural networks that can generate images of digits.

现在使用顺序处理处理非序列数据的另一个例子是执行相反操作并生成图像。这里不是尝试分类，而是我们有一些任务想要构建能够生成数字图像的神经网络。

The way that it's going to do this is by painting little sequences on the output canvas one time step at a time. At each point in time the neural network will choose where it wants to write and then what it wants to write.

它实现这一点的方式是通过在输出画布上每次一个时间步地绘制小序列。在每个时间点，神经网络将选择它想要写入的位置以及它想要写入的内容。

Then over time those writing decisions will be integrated to allow the network to build up these output images using some kind of sequential processing even though the underlying task is not sequential.

然后随着时间的推移，这些写入决策将被整合，使网络能够使用某种顺序处理来构建这些输出图像，即使底层任务不是顺序性的。

Here's one example that I saw on Twitter just last week. The idea is again we're building a neural network that can produce these images which is a non sequential task but using sequential processing.

这是上周我在Twitter上看到的一个例子。这个想法再次是我们在构建一个能够生成这些图像的神经网络，这是一个非顺序性任务，但使用了顺序处理。

What they did is actually integrated the neural network into an oil paint simulator. At every time step the neural network chooses what type of brushstroke it wants to make on this virtual oil paint canvas.

他们所做的实际上是将神经网络集成到油画模拟器中。在每个时间步，神经网络选择它想要在这个虚拟油画画布上制作哪种类型的笔触。

At every time step it's conditioned on what it saw in the previous time step it chooses where to make one of these virtual oil paint brush strokes. Over time it actually builds up these sort of stylized artistic images of faces.

在每个时间步，它都基于在前一个时间步看到的内容来决定在哪里制作这些虚拟油画笔触之一。随着时间的推移，它实际上构建了这些风格化的人脸艺术图像。

These are all examples of where you might imagine using a recurrent neural network. We've seen that recurrent neural networks can be used to both open the door to new types of tasks involving processing sequential data and they can also give us a new way to solve our old types of problems.

这些都是你可能会想到使用循环神经网络的例子。我们已经看到循环神经网络既可以用于开启涉及处理序列数据的新型任务之门，也可以为我们提供解决旧有问题的新方法。

We might want to use sequential processing even for these inherently non sequential tasks. Hopefully this is good enough motivation as to why recurrent neural network is an interesting thing to learn about.

我们可能希望即使对于这些本质上非顺序性的任务也使用顺序处理。希望这足以说明为什么循环神经网络是一个值得学习的有趣主题。

Given that motivation, what is a recurrent neural network and how does it work? The basic intuition behind a recurrent neural network is like I said we're processing sequences.

基于这个动机，什么是循环神经网络，它如何工作？正如我所说，循环神经网络背后的基本直觉是我们在处理序列。

At every time step the recurrent neural network is going to receive some input X shown in red and going to emit some output Y shown in blue. The recurrent neural network will also have some internal hidden state which is some kind of vector.

在每个时间步，循环神经网络将接收一些以红色显示的输入X，并发出一些以蓝色显示的输出Y。循环神经网络还将具有某种内部隐藏状态，这是某种向量。

At every time step the recurrent neural network will use the input of the current time step to update its hidden state using some kind of update formula. Then given the updated hidden state it will then emit its output for the current time step Y.

在每个时间步，循环神经网络将使用当前时间步的输入，通过某种更新公式来更新其隐藏状态。然后根据更新后的隐藏状态，它将发出当前时间步的输出Y。

Concretely, in order to define the architecture of our recurrent neural network we need to write down some kind of recurrence relation. We have this intuition that the network is working on this sequence of hidden states where H sub T is the hidden state at time T which is just going to be some vector. Just like the hidden state activations of the fully connected networks that we've worked with in the past, now we'll write down this recurrence relation F that depends on learnable weights W. This learnable function will take as input the hidden state at the previous time step H_{T-1} as well as the input at the current time step X_T, and it will output the hidden state at the next time step H_T.

具体来说，为了定义循环神经网络的架构，我们需要写下某种递推关系。我们有这样的直觉：网络正在处理这个隐藏状态序列，其中H_T是时间T的隐藏状态，它将是某个向量。就像我们过去处理的全连接网络的隐藏状态激活一样，现在我们将写下这个依赖于可学习权重W的递归关系F。这个可学习函数将接收前一个时间步的隐藏状态H_{T-1}以及当前时间步的输入X_T作为输入，并输出下一个时间步的隐藏状态H_T。

You can imagine that we can write down different types of formulas F_W that cause these inputs and hidden states to be related to each other using different algebraic formulations. The most simple way we can write, and the important critical point, is that we use this same function F_W with the same weights W at every time step in the sequence.

我们可以设想编写不同类型的公式F_W，通过这些公式使用不同的代数形式将这些输入和隐藏状态相互关联。我们能写出的最简单方式，也是关键要点在于，我们在序列的每个时间步都使用具有相同权重W的相同函数F_W。

By doing this we're sharing weights and using the exact same weight matrix to process every point in time and every point in the sequence. This construction allows us to have just a single weight matrix that can now process sequences of arbitrary length because we're using the exact same weight matrix at every time step of the sequence.

通过这种方式，我们实现了权重共享，使用完全相同的权重矩阵来处理时间上的每个点和序列中的每个位置。这种构造使我们仅需一个权重矩阵就能处理任意长度的序列，因为我们在序列的每个时间步都使用完全相同的权重矩阵。

Now with this kind of general definition of a recurrent neural network, we can see our first concrete implementation of a recurrent neural network. This simplest version is sometimes called a vanilla recurrent neural network or sometimes an Elman recurrent neural network after Professor Jeffrey Elman who worked on these some time ago.

基于这种循环神经网络的一般定义，我们现在可以看到循环神经网络的第一个具体实现。这个最简单的版本有时被称为普通循环神经网络，有时被称为埃尔曼循环神经网络，以纪念早期研究此类网络的杰弗里·埃尔曼教授。

Here the hidden state consists of a single vector H_T at every time step. We are going to have to learn two weight matrices: one is W_HH which is going to be multiplied by the hidden state at the previous time step, and the other is W_XH which is going to be multiplied by the input at the current time step.

在这里，隐藏状态由每个时间步的单个向量H_T组成。我们需要学习两个权重矩阵：一个是W_HH，它将与前一时间步的隐藏状态相乘；另一个是W_XH，它将与当前时间步的输入相乘。

What we'll do is we'll take our input at our current time step, multiply it by one weight matrix, take our previous hidden state, multiply it by the other weight matrix, add them together also add a bias term, and then we're going to use the non-linearity tanh and squash them through tanh. After squashing through tanh, this will give us our new hidden state H_T at our new time step.

我们的操作流程是：获取当前时间步的输入，乘以一个权重矩阵；获取前一个隐藏状态，乘以另一个权重矩阵；将两者相加并加上偏置项；然后使用tanh非线性激活函数进行处理。经过tanh函数压缩后，我们就得到了新时间步的新隐藏状态H_T。

Now we can produce our output at this time step Y_T by having another weight matrix that is going to be just a linear transform on that hidden state H_T.

现在我们可以通过另一个权重矩阵来生成当前时间步的输出Y_T，该权重矩阵将仅对隐藏状态H_T进行线性变换。

One way to think about this is another way to think about the processing of a recurrent neural network is to think about the computational graph that we build when we're unrolling this recurrent neural network over time. We can imagine that at the very beginning of processing our sequence, we're going to have some initial input to the first element of the sequence X_1 and we need to get some initial hidden state H_0 from somewhere.

理解这一点的另一种方式是思考循环神经网络的处理过程，即考虑我们在时间维度上展开循环神经网络时构建的计算图。我们可以设想，在处理序列的最开始，我们将有序列第一个元素X_1的初始输入，并且需要从某处获取初始隐藏状态H_0。

To kind of kick off this recurrence, it's very common to either initialize that first hidden state to be all zeros, which is probably one very common thing. Sometimes you'll also see people learn the initial hidden state as another learnable parameter of the network. But those are both kind of implementation details - you can just imagine that this initial hidden state is all zeros and that usually works pretty well.

为了启动这个递归过程，通常会将第一个隐藏状态初始化为全零，这可能是非常常见的做法。有时也会看到人们将初始隐藏状态作为网络的另一个可学习参数。但这些都属于实现细节——你可以假设初始隐藏状态为全零，这通常效果很好。

Then given this initial hidden state and this first element of the sequence, we feed them to this recurrence relation function F_W that will output our first hidden state H_1. Now given our first hidden state, we then feed it again to the same function F_W and slurp in the next element of the sequence X_2 to produce the next hidden state, and so on and so forth.

然后，给定这个初始隐藏状态和序列的第一个元素，我们将它们输入到这个递归关系函数F_W中，该函数将输出我们的第一个隐藏状态H_1。现在有了第一个隐藏状态，我们再次将其输入到相同的函数F_W中，并接收序列的下一个元素X_2，以产生下一个隐藏状态，依此类推。

What's important here is that we're using the exact same weight matrix at every time step of the sequence. In the computational graph, this is manifested as having a single node W for the weight matrix that is then used at every time step in the sequence.

这里重要的是我们在序列的每个时间步都使用完全相同的权重矩阵。在计算图中，这表现为权重矩阵只有一个节点W，然后在序列的每个时间步都被使用。

You can imagine that maybe during back propagation, if you remember the rules for sort of copy nodes in a computational graph, then in the forward pass if we use the same node in multiple parts of the computational graph, then during the backward pass we need to sum. This will be important when you implement recurrent neural networks on assignment 4.

你可以设想在反向传播期间，如果你记得计算图中复制节点的规则，那么在正向传播中如果我们在计算图的多个部分使用相同的节点，那么在反向传播期间我们需要进行求和。当你在作业4中实现循环神经网络时，这一点将非常重要。

You can also see by this design that because we're using the exact same weight matrix at every time step in the sequence, then this one recurrent neural network can process any sequence of arbitrary length. If we receive a sequence of two elements, we'll just unroll this graph for two time steps. If you receive a sequence of 100 elements, we'll unroll this graph for 100 time steps.

通过这种设计你还可以看到，由于我们在序列的每个时间步都使用完全相同的权重矩阵，因此这个循环神经网络可以处理任意长度的序列。如果我们接收到一个包含两个元素的序列，我们只需将这个图展开两个时间步。如果你接收到一个包含100个元素的序列，我们将把这个图展开100个时间步。

No matter what length of sequence we receive, we can use the same recurrent neural network and the same weights to process sequences of arbitrary length. This is kind of the basic operation of a recurrent neural network.

无论我们接收到什么长度的序列，我们都可以使用相同的循环神经网络和相同的权重来处理任意长度的序列。这就是循环神经网络的基本操作。

Remember we saw all these different one-to-many, many-to-many of these different types of sequence tasks that we might want to use. Now we can see how we can use this basic recurrent neural network to implement all of these different types of sequential processing tasks.

记得我们看到了所有这些不同类型的一对多、多对多序列任务，我们可能想要使用这些任务。现在我们可以看到如何使用这个基本的循环神经网络来实现所有这些不同类型的序列处理任务。

Here in the case of many-to-many where we want to receive a sequence of inputs and now we want to make a decision for each point in the sequence, this again might be something like video classification where we want to classify every frame of a video.

在多对多的情况下，我们希望接收一个输入序列，并希望对序列中的每个点做出决策，这可能类似于视频分类，我们希望分类视频的每一帧。

Now we can have another weight matrix maybe W_out or W_Y that's going to produce our output Y at every time step in the sequence. Maybe we have some desired label then to train this thing we might apply a loss function at each time step in the sequence.

现在我们可以有另一个权重矩阵，可能是W_out或W_Y，它将在序列的每个时间步产生我们的输出Y。可能我们有一些期望的标签，然后为了训练这个模型，我们可以在序列的每个时间步应用损失函数。

For example if this was something like video classification and we're making a classification decision at every point in the sequence, then we might have a ground truth label at every point in time and we apply a cross entropy loss to the predictions at every time step to now get a loss per time point per element of the sequence.

例如，如果这是类似视频分类的任务，并且我们在序列的每个点都做出分类决策，那么我们可能在每个时间点都有一个真实标签，并且我们在每个时间步对预测应用交叉熵损失，从而得到序列中每个元素在每个时间点的损失。

To get our final loss function, we would sum together all of these per time step losses, and that would give us our final loss that we could back propagate through.

为了得到最终的损失函数，我们将所有时间步的损失相加，从而得到可以反向传播的最终损失。

Now this would be something like the full computational graph for a many-to-many recurrent neural network that is making one output per time step in our input sequence.

这类似于一个多对多循环神经网络的完整计算图，它在输入序列的每个时间步生成一个输出。

But now, if we were doing a many-to-one situation, this might be something like video classification where we just want to produce a single classification label for the entire video sequence.

但在多对一的情况下，比如视频分类，我们可能只需要为整个视频序列生成一个分类标签。

Then we can just hook up our model to make a single prediction at the very end of the sequence that only operates on the final hidden state of the recurrent neural network.

这时我们可以将模型配置为仅在序列末尾基于循环神经网络的最终隐藏状态进行单次预测。

What you can see is that at this final state of the recurrent neural network, it kind of depends on the entire input sequence.

可以看到，循环神经网络的这个最终状态实际上依赖于整个输入序列。

Hopefully by the time we get to this final hidden state, it encapsulates all the information that the network needs to know about the entire sequence in order to make its classification decision.

理想情况下，当我们到达最终隐藏状态时，它已经包含了网络进行分类决策所需的全部序列信息。

If we're in a many-to-one situation like image captioning, where we want to input a single element like an image and then output a sequence of elements like words to describe the image, then we can also use this recurrent neural network.

在像图像描述这样的多对一场景中，比如输入单个元素（如图像）并输出元素序列（如描述图像的词语序列），我们同样可以使用这种循环神经网络。

Now we pass a single input at the beginning which is our single input X, and then we use this recurrence relation to produce a whole sequence of outputs.

这时我们在开始时传入单个输入X，然后利用循环关系生成完整的输出序列。

There's another very common application of recurrent neural networks which is the so-called sequence to sequence problem, often used in machine translation where you want to process one input sequence and then produce another input sequence where the lengths of the two sequences might be different.

循环神经网络另一个常见应用是序列到序列问题，通常用于机器翻译，即处理一个输入序列后生成另一个长度可能不同的输入序列。

This might be something like we input a sequence of words in English and then output a sequence of words in French, giving a translation of the sentence.

例如输入英语单词序列，输出法语句子翻译。

An English sentence does not always have the same number of words as its corresponding translation in French, so it's important that we are able to build recurrent neural networks that can process an input sequence of one length and produce an output sequence of another length.

由于英语句子与其法语翻译的单词数量并不总是相同，因此构建能够处理不同长度输入输出序列的循环神经网络至关重要。

The way that we implement this is the so-called sequence to sequence recurrent neural network architecture, which basically takes a many-to-one recurrent neural network and feeds it directly into another one-to-many recurrent neural network.

我们通过序列到序列循环神经网络架构实现这一点，该架构本质上是将一个多对一循环神经网络直接连接到另一个一对多循环神经网络。

Here we have one recurrent neural network called the encoder that will receive our input sequence, like an English sentence, and process that input sequence one element at a time.

这里我们有一个称为编码器的循环神经网络，它接收输入序列（如英语句子），并逐元素处理该输入序列。

The entire content of that sequence will be summarized in the hidden vector predicted at the very end of the sequence.

整个序列的内容将被汇总到序列末尾预测得到的隐藏向量中。

We can take that hidden vector at the end of the encoder sequence and feed it as a single input to the second recurrent neural network called the decoder.

我们可以将编码器序列末尾的隐藏向量作为单个输入，馈送到第二个称为解码器的循环神经网络中。

This second decoder network is a one-to-many network because it receives the single vector output from the first network and produces a variable length sequence as output.

这第二个解码器网络是一对多网络，因为它接收来自第一个网络的单个向量输出，并生成可变长度的输出序列。

From this computational graph you can see that we're using different weight matrices in the encoder and the decoder, which is pretty common in these sequence to sequence models.

从这个计算图中可以看到，我们在编码器和解码器中使用了不同的权重矩阵，这在序列到序列模型中非常常见。

One problem is that the number of output tokens might be different from the number of input tokens, so we might want to process the English sentence and then output the French sentence where the number of words in the output might be different from the number of words in the input.

一个关键问题是输出标记的数量可能与输入标记不同，比如处理英语句子后输出法语句子时，输出单词数可能与输入单词数不同。

You might imagine we just use the same weight matrix for both the encoder and the decoder, which would be like processing the whole sequence where we give an input for the first K time steps, then for the last K time steps we don't give it any input at all and just expect it to produce an output.

你可能会设想在编码器和解码器中使用相同的权重矩阵，就像处理完整序列时在前K个时间步提供输入，而在后K个时间步不提供任何输入仅期望其产生输出。

People do that sometimes, but the question of how we know how many tokens we need in the second one is a detail we'll get to in a couple slides.

虽然有时人们确实这样做，但关于如何确定第二个网络所需标记数量的问题，我们将在后续幻灯片中详细讨论。

As a more concrete example, we can talk about the so-called language modeling task where we want to build a recurrent neural network that processes an infinite stream of input data and at every point in time tries to predict the next character in the sequence.

作为一个更具体的例子，我们可以讨论语言建模任务：构建一个处理无限输入数据流的循环神经网络，在每个时间点尝试预测序列中的下一个字符。

This is called a language model because it allows the neural network to score the probability of any sequence being part of the language that it's learning.

这被称为语言模型，因为它使神经网络能够评估任何序列属于其所学语言的概率。

The way we set this up is we'll typically have some fixed set of vocabulary that the network knows about, like the letters H, E, L, O in this case.

我们通常会让网络了解一个固定的词汇表，比如这个例子中的字母H、E、L、O。

We need to choose this vocabulary at the beginning of training.

这个词汇表需要在训练开始时确定。

We process the training sequence "hello" by converting each character into a one-hot vector.

我们通过将每个字符转换为one-hot向量来处理训练序列"hello"。

Given our vocabulary of size four for H-E-L-L-O, we convert the letter H into the vector [1,0,0,0] because it has a 1 for the first slot in the vector corresponding to the first element of our vocabulary.

在包含H-E-L-L-O的四元素词汇表中，我们将字母H转换为向量[1,0,0,0]，因为它在向量中对应词汇表首元素的位置为1。

We process our input sequence in these one-hot vectors, giving us a sequence of vectors that we can feed to the recurrent neural network.

我们通过这些one-hot向量处理输入序列，得到可以馈送到循环神经网络的向量序列。

We can use our recurrent neural network to process the sequence of input vectors and produce a sequence of hidden states.

我们可以使用循环神经网络处理输入向量序列并生成隐藏状态序列。

At every time step, we can use our output matrix to predict a distribution over the elements in our vocabulary.

在每个时间步，我们可以使用输出矩阵预测词汇表元素的概率分布。

Because the task of the network is to predict at every point in time the next element in the input sequence, you can see that after it receives the first element H, it tries to predict the next element. So then that would be a cross entropy classification loss at that point at that time step in the sequence.

由于网络的任务是在每个时间点预测输入序列中的下一个元素，可以看到在接收到第一个元素H后，模型试图预测下一个元素。这会在序列的该时间步产生交叉熵分类损失。

Then after, once it receives the first two input characters H and E, it needs to predict L which is the third character in the sequence, and so on and so forth.

随后，当模型接收到前两个输入字符H和E后，它需要预测序列中的第三个字符L，依此类推。

So then you can see that the target outputs in this sequence are equal to the target inputs, just kind of offset by 1.

因此可以看出，该序列中的目标输出等于目标输入，只是偏移了一个位置。

So then once you've got a trained language model - that's kind of what a language model looks like during training - you got your input sequence, you shift the output input sequences, and then try to predict the next character at every time step of processing.

当获得训练好的语言模型后——这就是语言模型在训练期间的大致形态——你拥有输入序列，将输出输入序列偏移，然后在处理的每个时间步尝试预测下一个字符。

But now once you've got a trained language model, you can actually do something really cool with it, and you can use your trained language model to generate new text that is in the style of the text that it was trained on.

但一旦获得训练好的语言模型，你实际上可以用它做一些非常酷的事情，即使用训练好的语言模型生成符合其训练文本风格的新文本。

So as an example of what that might look like, given our trained language model that we've now trained on a set of sequences, then what we can do is we can feed it some initial seed token like the letter H.

举例说明这种情况，假设我们已在序列集上训练好语言模型，那么我们可以输入一些初始种子标记，比如字母H。

And now what we want to do is have the recurrent neural network generate new text conditioned on this initial seed token.

现在我们希望循环神经网络基于这个初始种子标记生成新文本。

So then the way that this works is we give it our input token H, we give it the same one-hot encoding, we go through one layer, we unroll one tick of this recurrent neural network sequence, and then get this distribution of predictions for the next character at that time.

其工作原理是：我们输入标记H，采用相同的独热编码，经过一个层，展开这个循环神经网络序列的一个时间步，然后获得当前时刻对下一个字符的预测分布。

And now because our model has predicted a distribution of what characters it thinks should happen at the next time step, what we can do is sample from that distribution to just give a new invented character for what the model thinks is probable at the next time step.

由于模型已预测了下一个时间步可能出现的字符分布，我们可以从该分布中采样，生成模型认为在下一时间步可能出现的虚构字符。

And then we could, after we take that sample character, we can take the sampled output from the first time step of the network and feed it back as input in the next time step of the network.

在获取采样字符后，我们可以将网络第一个时间步的采样输出作为输入反馈到网络的下一个时间步。

Then we have this sampled character E that we can feed back as input at the next time step, and then go through another layer of processing of this recurrent neural network.

这样我们就有了采样字符E，可将其作为输入反馈至下一时间步，然后经过循环神经网络的又一层处理。

So again compute the next hidden state, compute a new distribution of predicted outputs, and then gives us a new distribution over what the model thinks the yet next character should be.

再次计算下一个隐藏状态，计算新的预测输出分布，从而获得模型认为再下一个字符应该是什么的新分布。

And then you can imagine repeating this process over and over again.

可以想象重复这一过程周而复始地进行。

So then given your trained language model, you can seed it with some initial token, and then just have it generate new tokens that it thinks are likely to follow that initial token that you give it.

因此，给定训练好的语言模型，你可以用某个初始标记作为种子，然后让它生成认为可能跟随该初始标记的新标记。

So it's kind of a little ugly detail is that so far we've talked about encoding our input sequence as a set of one-hot vectors.

有个不太优雅的细节是：目前我们讨论的是将输入序列编码为一组独热向量。

And if you think about what happens in this first layer in this first layer of the recurrent neural network, remember in this vanilla neural network we were taking our input vector and then multiplying it with our weight matrix.

如果思考循环神经网络第一层中发生的情况，请记得在这个普通神经网络中，我们获取输入向量后将其与权重矩阵相乘。

Well if our input vector is just a one-hot vector with a 1 in one slot and zeros in all other slots, but actually that matrix multiplication is kind of trivial because if we were to take matrix multiplied by a one-hot vector, then what it does is it just extracts one of the columns of the vector.

如果输入向量仅是某个位置为1、其余位置为零的独热向量，实际上这种矩阵乘法有些平凡，因为若将矩阵与独热向量相乘，其结果仅是提取向量的某一列。

So actually you don't need to implement that with a matrix multiplication routine, you can implement that much more efficiently with an operator that just simply extracts out rows of a weight matrix.

实际上无需通过矩阵乘法例程实现此操作，用仅提取权重矩阵各行的运算符来实现会高效得多。

For that reason, it's very common to actually insert another layer in between the input to the network and the recurrent neural network called an embedding layer that does exactly this.

因此，通常会在网络输入和循环神经网络之间插入另一个称为嵌入层的层，专门执行此操作。

So here the embedding now at the input, our sequence will be encoded as a set of one-hot vectors, and now the embedding layer will just perform this one-hot sparse matrix multiply implicitly.

这样在输入处，我们的序列将被编码为一组独热向量，而嵌入层将隐式执行这种独热稀疏矩阵乘法。

So effectively this embedding layer just learns a separate each row of the column of the embedding matrix corresponds to an embedding vector corresponding to each element in our vocabulary.

实际上，嵌入层仅学习独立的嵌入向量，嵌入矩阵的每列行对应词汇表中每个元素的嵌入向量。

So in a very common design for these recurrent neural networks is actually to have this separate embedding layer that happens between the raw inputs - the raw input one-hot sequence - and before passing to our recurrent neural network that computes these sequence of hidden states.

在这些循环神经网络非常常见的设计中，实际上会在原始输入（原始输入的独热序列）与传递至计算隐藏状态序列的循环神经网络之间设置独立的嵌入层。

So now to train these things, we've sort of seen this example of a computational graph using recurrent neural networks already, and what we saw is that in order to train one of these recurrent neural networks we need to kind of unroll this computational graph over time.

现在要训练这些模型，我们已经看过使用循环神经网络的计算图示例，观察到要训练这类循环神经网络，我们需要随时间展开这个计算图。

And then at every time point in the sequence we give rise to some loss per time step, then these get summed over the entire length of the sequence to give a single loss.

序列中的每个时间点都会产生每个时间步的损失，这些损失沿整个序列长度求和得到单个总损失。

So what was this kind of mean? This is sometimes given a fancy name called back propagation through time, because during the forward pass we're kind of stepping forward through time through a sequence, and then during a backward pass we're back propagating backwards in time through this sequence that we had unrolled during the forward pass.

这意味着什么？这有时被赋予一个 fancy 的名称：随时间反向传播，因为在正向传播期间我们沿时间步进通过序列，而在反向传播期间则沿时间反向传播通过我们在正向传播期间展开的序列。

But now one problem with this back propagation through time algorithm is that if we want to work on very very long sequences and train on very very long sequences, then this is going to take an enormous amount of memory.

但随时间反向传播算法的一个问题是：如果我们要处理非常长的序列并在非常长的序列上训练，这将占用巨大的内存量。

Because say we want to train on sequences that are like a million characters long, well then you need to unroll this computational graph for a million time steps - that's probably not going to fit in your GPU memory.

因为假设我们要训练百万字符长的序列，那么需要展开计算图至百万个时间步——这很可能无法放入GPU内存。

So in practice, when we're training recurrent neural networks and especially recurrent neural network language models on very very long sequences, sometimes we use an alternative approximate algorithm called a truncated back propagation through time.

因此实践中，当在非常长的序列上训练循环神经网络（尤其是循环神经网络语言模型）时，有时会使用另一种近似算法：截断随时间反向传播。

So here the idea is that we want to sort of approximate the training of this network on this full possibly infinite sequence, but then what we'll do is we'll take some subset of the sequence maybe like the initial the first ten tokens or the first hundred tokens of the sequence.

其思想是：我们希望近似模拟网络在完整（可能无限）序列上的训练，但实际操作是取序列的某个子集，可能是初始的前十个标记或前一百个标记。

Then we'll unroll the forward pass of the network for that short prefix of the sequence and then compute a loss only for that first chunk of the sequence and then back propagate through the initial chunk of the sequence and make an update on the weights. Now what we'll do is we'll actually record the hidden weight, the values of the hidden states from the end of this initial chunk of the sequence. Then we'll receive the second chunk of the sequence and we'll pass in these recorded hidden weights that we remembered when processing the first chunk. Then we'll unroll a second chunk of the sequence like the next hundred characters in our possibly million character sequence.

然后对序列的短前缀展开网络的正向传播，仅计算该首段序列的损失，然后通过序列的初始块进行反向传播并对权重进行更新。现在我们将记录隐藏权重，即序列初始块末尾的隐藏状态值。然后接收序列的第二个块，并传入处理第一个块时记录的这些隐藏权重。接着展开序列的第二个块，比如我们可能长达百万字符序列中的接下来一百个字符。

Then we'll unroll this next hundred characters of the sequence, compute a loss for the second chunk, and then back propagate not through the entire sequence but instead back propagate only through the second chunk of the sequence. This will compute gradients of this loss with respect to our weight matrix, then we can make an update on the weight matrix and continue.

然后展开序列的下一个百字符，计算第二个块的损失，接着不是通过整个序列而是仅通过序列的第二个块进行反向传播。这将计算该损失相对于权重矩阵的梯度，然后我们可以更新权重矩阵并继续。

Next we would take the next chunk of the sequence, remember what the hidden state was from passing the second chunk, and then use that recorded hidden states to continue unrolling the sequence forward in time. Then again make a truncated back propagation through time and another weight update.

接下来我们将获取序列的下一个块，记住通过第二个块时的隐藏状态，然后使用记录的隐藏状态继续在时间上向前展开序列。接着再次进行截断时间反向传播和另一次权重更新。

What this back propagation through time algorithm does is basically the forward pass because we're always carrying the hidden information forward throughout forever perfectly through these remembered hidden states. The forward pass is still sort of processing an infinite but potentially infinite sequence, but now we're only back propagating through small chunks of the sequence at a time.

这种时间反向传播算法的基本工作原理是：前向传播时我们始终通过记忆的隐藏状态完美地向前传递隐藏信息。前向传播仍然在处理一个无限但可能无限的序列，但现在我们每次只对序列的小块进行反向传播。

This drastically reduces the amount of stuff that we need to keep in GPU memory. So this trick of truncated back propagation through time makes it feasible to train recurrent neural networks on even infinite sequences even though you only have finite amounts of GPU memory.

这极大地减少了我们需要保存在GPU内存中的数据量。因此，这种截断时间反向传播的技巧使得即使在GPU内存有限的情况下，也能训练循环神经网络处理无限序列。

All this sounds really complicated but in practice you can implement this whole idea of training with truncated back propagation through time. The question is for this truncated back propagation through time, how do you set the h0 for passing the second chunk? You would use the final hidden state when processing the first chunk.

这一切听起来很复杂，但在实践中你可以实现这种使用截断时间反向传播的训练方法。问题是对于这种截断时间反向传播，如何设置传递第二个块的初始隐藏状态h0？你应该使用处理第一个块时的最终隐藏状态。

When passing the first chunk we'll have this final hidden state, and then we'll just pass that final hidden state from the first chunk will become the initial hidden state when processing the second chunk. That's the trick that means that it's sort of processing everything forward in time potentially infinitely because it's carrying all this information forward in time through the hidden states, but then we're only back propagating through finite chunks of the sequence at a time.

处理第一个块时我们会得到最终隐藏状态，然后这个来自第一个块的最终隐藏状态将成为处理第二个块时的初始隐藏状态。这就是技巧所在，意味着它可以在时间上无限地向前处理所有信息，因为它通过隐藏状态在时间上向前传递所有信息，但我们每次只对序列的有限块进行反向传播。

When doing truncated back propagation through time, usually you'll go forward through a chunk, backward through a chunk, update the weight matrix, then you'll copy this hidden state over, go forward backward update, and then copy this one forward for backward update. Every time you do back propagation through some portion of the sequence, that will compute derivative of that chunk loss with respect to the weight matrix, then you can use that to make a weight update on the weights of the network.

进行截断时间反向传播时，通常你会前向通过一个块，反向通过一个块，更新权重矩阵，然后复制这个隐藏状态，再进行前向-反向-更新，接着复制这个状态进行下一个前向-反向-更新。每次对序列的某部分进行反向传播时，都会计算该块损失相对于权重矩阵的导数，然后你可以用它来更新网络权重。

The idea is that with this truncated back propagation through time, once you process one chunk of data you can throw it away, evict it from the memory of your computer. Then all the information about that sequence that's needed for the rest of training is stored in that final hidden state of the recurrent neural network at the end of processing the chunk.

其理念是，使用这种截断时间反向传播，一旦处理完一个数据块，你就可以丢弃它，从计算机内存中清除它。然后该序列在后续训练中所需的所有信息都存储在循环神经网络在处理该块结束时的最终隐藏状态中。

All this sounds maybe kind of complicated but in fact you can implement this whole process of truncated back propagation through time for training recurrent neural network language models and then sampling from them to generate new text. You can do it in like 112 lines of Python, and this is no PyTorch so no autograd, this is doing all the gradients manually. I did a version of this in PyTorch and then once you have PyTorch you can do it about like 40 or 50 lines.

这一切听起来可能有点复杂，但实际上你可以实现这种截断时间反向传播的整个过程，用于训练循环神经网络语言模型，然后从中采样生成新文本。你可以用大约112行Python代码实现，而且这不使用PyTorch，没有自动求导，所有梯度都是手动计算的。我用PyTorch做了一个版本，一旦使用PyTorch，你可以在大约40到50行代码内完成。

So it's actually not a ton of code to actually do this stuff. Now what's fun is that once you've implemented these things, you can have fun and just sort of train recurrent neural network language models on different types of data and then use them to generate new texts to kind of get an insight to what types of stuff these networks are learning when we train a language model on text.

所以实际上实现这些东西并不需要大量代码。有趣的是，一旦你实现了这些功能，你就可以在不同类型的数据上训练循环神经网络语言模型，然后用它们生成新文本，从而深入了解当我们训练文本语言模型时这些网络在学习什么类型的内容。

For example what we can do is download the entire works of William Shakespeare, concatenate them into a giant text file, and then that's a very very long sequence of characters. Then we can train a recurrent neural network that processes the entire works of William Shakespeare and tries to predict the next character given the previous hundred characters or something like that.

例如，我们可以下载威廉·莎士比亚的全部作品，将它们连接成一个巨大的文本文件，这就形成了一个非常非常长的字符序列。然后我们可以训练一个循环神经网络，它处理莎士比亚的全部作品，并尝试根据前一百个字符预测下一个字符。

Just train a recurrent neural network whose entire purpose in life is to predict next character from works of William Shakespeare. Then once we train this thing, we can sample from the trained model. After the first couple of iterations it doesn't look like it's doing too good.

训练一个循环神经网络，其全部生命目标就是从莎士比亚作品中预测下一个字符。然后一旦训练完成，我们可以从训练好的模型中采样。在最初几次迭代后，它看起来效果并不太好。

What we're doing is we're sampling what the network thinks is the next character and then feeding that sample back to the network as the next input, and then repeating this process to generate new data. At first this thing is basically generating garbage because it's fairly random weights.

我们做的是采样网络认为的下一个字符，然后将该采样作为下一个输入反馈给网络，然后重复这个过程以生成新数据。最初这东西基本上生成的是垃圾，因为权重相当随机。

If you train this thing a little bit longer then it starts to recognize some structure in the text. So it makes things that look like words and put spaces in there and maybe put some quotes, but if you actually read it it's still garbage.

如果你训练这个东西时间稍长一些，它就会开始识别文本中的某些结构。所以它会生成看起来像单词的东西，并在其中加入空格，可能还会加一些引号，但如果你实际阅读，它仍然是垃圾。

We train a little bit more and now I think it almost looks like sentences. There's some spelling errors but it says something like after fall ooh such that the hall for a Princeville smoky. So it's like starting to say something.

我们再训练一会儿，现在我认为它几乎看起来像句子了。有一些拼写错误，但它会说类似"after fall ooh such that the hall for a Princeville smoky"这样的话。所以它像是开始说些什么了。

But you train it even longer and now it starts to get really really good and starts to generate text that looks fairly realistic. So now it says why do what they day replied Natasha and wishing to himself the fact the Princess Mary was easier. So you know the grammar is not perfect but this does looked kind of like real English.

但如果你训练更长时间，它就会开始变得非常好，并开始生成看起来相当逼真的文本。所以现在它会说"why do what they day replied Natasha and wishing to himself the fact the Princess Mary was easier"。你知道语法并不完美，但这确实看起来有点像真正的英语。

Now we train this thing for a very long time and sample longer sequences and it generates very plausible looking Shakespeare text. So you can see these look like stage directions: Pan drape Andreas alas, I think he shall come approached in the day with little strain would be attained into being never fed and who is but a chain and subjects of his death I should not sleep. So this sounds very dramatic, it sounds very much like Shakespeare, but unfortunately it's still gibberish.

现在我们长时间训练这个东西，并采样更长的序列，它会生成看起来非常可信的莎士比亚风格文本。所以你可以看到这些看起来像舞台指示：潘·德雷普·安德烈亚斯，唉，我想他会在白天轻松到来，几乎不费力气就能达成存在却从未被滋养，他不过是一条锁链，是他死亡的臣民，我实在无法安眠。这听起来非常戏剧化，很像莎士比亚的风格，但不幸的是，它仍然是胡言乱语。

Now you can actually go further and imagine training these things on different types of data. So this was the entire concatenated works of William Shakespeare. Years ago I did this one: have you anyone ever taken an abstract algebra course or an algebraic geometry course? Well, that's this sort of very abstract part of mathematics. It turns out there's an open source textbook for algebraic geometry that's something like many many thousands of pages written in LaTeX.

实际上，你可以更进一步，想象用不同类型的数据来训练这些模型。这是威廉·莎士比亚全部作品的拼接。多年前我做过一个实验：有没有人上过抽象代数或代数几何课程？嗯，那是数学中非常抽象的部分。结果发现有一本开源的代数几何教材，大约有数千页，是用LaTeX编写的。

So what I did is I downloaded the entire LaTeX source code of the several thousand page algebraic geometry textbook and then trained a recurrent neural network to generate the next character of LaTeX source code given the previous hundred characters on this entire source code of this algebraic geometry textbook. Then you sample fake math that the neural recurrent neural network is just inventing out of the weights that it's trained. Unfortunately, it tends not to compile, so it's not so good at producing exactly grammatically correct LaTeX source code, but you imagine you can manually fix some compile errors and then you can actually get this thing to compile.

所以我下载了这本数千页代数几何教材的完整LaTeX源代码，然后训练了一个循环神经网络，根据这本代数几何教材整个源代码的前一百个字符来生成LaTeX源代码的下一个字符。然后你采样出假的数学内容，这些是神经循环神经网络仅仅根据它训练得到的权重凭空发明的。不幸的是，它往往无法编译，所以它不太擅长生成语法完全正确的LaTeX源代码，但你可以想象手动修复一些编译错误，然后实际上可以让这个东西编译通过。

So now these are examples of generated text from our current neural network that was trained on this algebraic geometry textbook. So you can see that it's like this kind of looks like abstract math right? It's like I'm having lemmas, it's having proofs, it even put the little square at the end of the proof when it's done proving things. It tries to refer to previous lemmas that may or may not have been proven elsewhere in this text, and it's kind of like very adversarial and kind of rude in the way that some math books are. So like the proof is see discussion in sheaves of sets, so like clearly you should have a reference back somewhere else in this text work in order to understand this proof.

所以现在这些是我们当前神经网络生成的文本示例，该网络是在这本代数几何教材上训练的。你可以看到它看起来有点像抽象数学，对吧？比如它有引理，有证明，甚至在证明结束时还加了一个小方块。它试图引用之前的引理，这些引理可能在这篇文本的其他地方被证明过，也可能没有，而且它的方式有点对抗性，有点粗鲁，就像一些数学书那样。比如证明中写着“参见集合层的讨论”，所以显然你应该在这篇文本的其他地方有参考文献才能理解这个证明。

We can look at some more in algebraic geometry you actually have these cool commutative diagrams that people draw. They show relationships within different mathematical spaces that are generated with some LaTeX source code, some LaTeX package, and the recurrent neural network attempts to generate commutative diagrams to explain the proofs that it's generating that are also nonsense.

我们可以再看一些代数几何中的内容，实际上人们会画这些很酷的交换图。它们展示了不同数学空间之间的关系，这些图是用一些LaTeX源代码、一些LaTeX包生成的，而循环神经网络试图生成交换图来解释它正在生成的证明，这些证明同样也是无意义的。

And actually one of my favorite examples of this is actually on this page as well if you look at the up top left it says proof omitted which is definitely something that you'll see in math books sometimes.

实际上，我最喜欢的例子之一也在这页上，如果你看左上角，它写着“证明省略”，这绝对是你在数学书中有时会看到的东西。

So we can go further on this. So what's another really basically at this point you've got this idea that once you've got these character level recurrent neural network language models you can train them on basically any kind of data you can imagine.

所以我们可以在这方面更进一步。那么另一个真正基本的是，此时你已经有了这个想法：一旦你有了这些字符级循环神经网络语言模型，你基本上可以用你能想象的任何类型的数据来训练它们。

So we also at one point we download the entire current source code of the Linux kernel and trained a recurrent neural network language model to predict this to this model the C source code of a Linux kernel. Many of what you can do is you can sample from this and just generate invented C source code and this looks like pretty reasonable right? Like if you're not looking at this thing carefully this leg definitely looks like it could be real a kernel source code.

所以我们还在某个时间点下载了整个Linux内核的当前源代码，并训练了一个循环神经网络语言模型来预测这个模型的Linux内核C源代码。你能做的很多事情之一就是从中采样，仅仅生成发明的C源代码，而这看起来相当合理，对吧？比如如果你不仔细看这个东西，这段代码看起来肯定像是真正的内核源代码。

You know it's saying like static void do command struck SEC Phi M void star pointer it puts the bracket it indents it puts like int column equals 32 left shift left shift command of two like it even puts comments like free our user page pointer to place to place camera if all - so the comments don't really make sense but it knows that you're supposed to put comments. It also knows that you're supposed to recite this copyright notice at the top of files so when you sample from this thing of it outputs this copyright notice.

你知道它写着像 static void do command struck SEC Phi M void star pointer，它放括号，它缩进，它放像 int column equals 32 left shift left shift command of two，它甚至放注释像 free our user page pointer to place to place camera if all - 所以注释并不真正有意义，但它知道你应该放注释。它也知道你应该在文件顶部复述这个版权声明，所以当你从这个东西采样时，它输出这个版权声明。

It also kind of knows the general structure of C source code files so after the copyright notice you can see it's having a lot of includes like includes Linux k XC h so includes a bunch of headers has includes a bunch of other headers it defines some macros it defines some constants and then after all of that it starts defining functions.

它也有点知道C源代码文件的一般结构，所以在版权声明之后，你可以看到它有很多include，比如include Linux k XC h，所以include了一堆头文件，include了一堆其他头文件，它定义了一些宏，定义了一些常量，然后在所有这一切之后，它开始定义函数。

So you can see that this thing and just by doing this very simple task of trying to predict the next character then our recurrent neural network language model has somehow been able to capture a lot of structure in this a relatively complex data of this C source code of the Linux kernel.

所以你可以看到这个东西，仅仅通过做预测下一个字符这个非常简单的任务，我们的循环神经网络语言模型就 somehow 能够在这个相对复杂的Linux内核C源代码数据中捕捉到很多结构。

So then one thing you might you one question you might want to ask is how is it doing this? What kinds of representations are these recurrent neural networks learning from these data that we're training them on?

那么你可能想问的一个问题是，它是如何做到这一点的？这些循环神经网络从我们训练它们的数据中学到了什么样的表示？

Well there was a paper from Karpathy, Johnson, and Fei-Fei a couple years ago that attempted to answer some question like that. So here the idea is that we wanted to try to gain some interpretability into what these recurrent neural network language models were learning on when we trained them on different different types of sequence data sets.

嗯，几年前Karpathy、Johnson和Fei-Fei有一篇论文试图回答类似的问题。所以这里的想法是，我们想尝试获得一些可解释性，了解这些循环神经网络语言模型当我们在不同类型序列数据集上训练它们时，它们在学习什么。

So here what the methodology here is that we take our recurrent neural network and we unroll it for many time steps and we just make a skit to perform this prediction task of predicting the next character. So then in the in the process of predicting the next character these recurrent neural networks are going to generate this sequence of hidden states one hidden state for each character of input and then trying to generate that character at output.

所以这里的方法是，我们取我们的循环神经网络，将其在多个时间步上展开，我们只是做一个简短的表演来执行这个预测下一个字符的任务。那么在预测下一个字符的过程中，这些循环神经网络将生成这个隐藏状态序列，每个输入字符对应一个隐藏状态，然后试图在输出端生成那个字符。

So then what we can ask is we can ask what do the different dimensions of those hidden state capture? So for example what we can do is look at maybe look at dimension like 56 of those hidden states and now because that's going to be the output of @nh because of that's because it has that non-linearity then we know that each element of that hidden state vector will be a real number between negative 1 and 1.

那么我们可以问的是，我们可以问这些隐藏状态的不同维度捕捉了什么？例如，我们可以做的是，也许看看那些隐藏状态的维度，比如56，现在因为那将是@nh的输出，因为它有那个非线性，所以我们知道那个隐藏状态向量的每个元素将是一个介于负1和1之间的实数。

So what we can do is take the activation of element 56 and that recurrent neural network hidden state and then use the value of that hidden state which is between 0 & 1 to color the text on which the network was processing and that can give us some sense for what different elements of that hidden state when they light up when processing text.

所以我们可以做的是，取元素56的激活和那个循环神经网络隐藏状态，然后用那个介于0和1之间的隐藏状态值来给网络正在处理的文本上色，这可以让我们对那个隐藏状态的不同元素在处理文本时亮起时代表什么有一些感觉。

So here's an example of a not very interpretable result. So basically what we've done is when we trained our neural network to process this Linux kernel data set and then we asked it to predict the next character, now at every time step we've chosen like element 56 so there are currently all network hidden state and then we use the value of the hidden state at each character to color the text that it's processing. Is this visualization clear? So when one of the characters is colored red, that means that the value of that cell is very high, close to positive one. And when it's blue, it means it's very low, close to negative one.

所以这里是一个不太可解释的结果的例子。基本上我们所做的是，当我们训练我们的神经网络来处理这个Linux内核数据集，然后我们要求它预测下一个字符，现在在每个时间步我们都选择了像元素56这样的东西，所以当前有所有网络隐藏状态，然后我们使用每个字符处的隐藏状态值来为正在处理的文本着色。这个可视化效果清楚吗？当某个字符显示为红色时，意味着该单元格的值非常高，接近正一。而当显示为蓝色时，则意味着该值非常低，接近负一。

So then what you can do is look at these different hidden cell states and try to get some intuition for what they might be looking for. A lot of them look like this and you have no idea what they're looking for, they just look totally random. But sometimes you get some very interpretable cells in these recurrent neural networks.

这样你就可以观察这些不同的隐藏单元格状态，并尝试理解它们可能在寻找什么模式。很多状态看起来像这样，你完全不知道它们在寻找什么，看起来完全是随机的。但有时在这些循环神经网络中会出现一些非常可解释的单元格。

Here's an example where we trained on some Tolstoy text and then we test the recurrent neural network. We found that one of these cells is actually looking for quotes. What you can see is that this one particular cell of the recurrent neural network is all blue, which means it's all off, and then once it hits a quote, that one cell flips all the way on and turns all the way red, and that remains red all the way until the end quote when it flips all the way back to blue.

这里有一个例子，我们在一些托尔斯泰的文本上进行了训练，然后测试循环神经网络。我们发现其中一个单元格实际上在寻找引号。你可以看到，循环神经网络的这个特定单元格全是蓝色，意味着它处于关闭状态，然后一旦遇到引号，这个单元格就会完全激活变成红色，并一直保持红色直到引号结束，这时它又会完全变回蓝色。

What that gives us is the intuition that somehow this recurrent neural network has learned this kind of binary switch that keeps track of whether or not we are currently inside of a quote. We found another cell that tracks where we are in the current line. For example, after we hit a carriage return, then it resets to negative one and then it slowly increases over the course of a line.

这给我们的启示是，这个循环神经网络以某种方式学会了这种二进制开关，用于跟踪我们当前是否处于引号内部。我们还发现了另一个单元格，它跟踪我们在当前行中的位置。例如，当我们遇到回车符后，它会重置为负一，然后在一行中缓慢增加。

Because this dataset always had line breaks at certain characters, after we get about 80 characters then it knows we have to have a new line and then we reset that cell back to blue. When training on the Linux source code, we found a cell that tracked the conditions inside if statements, which was very interesting.

因为这个数据集总是在特定字符处有换行符，当我们达到大约80个字符时，它就知道需要换行，然后我们将该单元格重置为蓝色。在Linux源代码上训练时，我们发现了一个跟踪if语句内部条件的单元格，这非常有趣。

We also found another one that was checking whether or not we were inside a comment inside the Linux source code, and we found ones that were also tracking what is our indentation level inside the code. Basically, I thought this was really cool.

我们还发现了另一个检查我们是否处于Linux源代码注释内部的单元格，以及一些跟踪代码中缩进级别的单元格。基本上，我认为这真的很酷。

This means that even though we're just training these neural networks to do this seemingly stupid task of trying to predict the next character, then somehow in the process of simply trying to predict the next character in sequences, the recurrent neural network learns all of these features inside its hidden state that detect all these different types of structures in the data that it is trying to process.

这意味着，尽管我们只是训练这些神经网络来完成预测下一个字符这个看似愚蠢的任务，但在简单地尝试预测序列中下一个字符的过程中，循环神经网络在其隐藏状态中学会了所有这些特征，能够检测出它正在处理的数据中所有这些不同类型的结构。

So I thought that was a really cool result that gives us some insight into what types of things these recurrent neural network language models are learning. Now as an example back to computer vision, one thing that we can use these type of recurrent neural network language models for is the task of image captioning.

所以我认为这是一个非常酷的结果，它让我们深入了解了这些循环神经网络语言模型正在学习什么类型的内容。现在回到计算机视觉的例子，我们可以使用这类循环神经网络语言模型来完成图像描述的任务。

Here what we want to do is we want to input an image. This is an example of a one-to-many problem. So we're going to input an image, feed it to a convolutional network that you guys are like all experts on now, to extract features about the image, and then pass the features of that convolutional network to a recurrent neural network language model that will now generate words one at a time to describe the content of that image.

这里我们要做的是输入一张图像。这是一个一对多问题的例子。我们将输入一张图像，将其馈送到卷积网络（你们现在应该都是这方面的专家了）以提取图像特征，然后将该卷积网络的特征传递给循环神经网络语言模型，该模型将逐个生成单词来描述该图像的内容。

Then we can train this thing if you had a dataset of images and associated captions, then you could train this thing using normal gradient descent. To kind of concretely look at what this looks like, this is an example of transfer learning.

然后我们可以训练这个系统，如果你有一个包含图像和相关描述的数据集，那么你可以使用正常的梯度下降来训练它。具体来看这是什么样子，这是一个迁移学习的例子。

So we're going to step one: download a CNN model that had been pre-trained for classification on ImageNet. Then we're going to chop off the last two layers of that network. Now here we actually want to operate on sequences of finite length, so unlike the language modeling case we're kind of mostly concerned with operating on these like infinite streams of data and doing truncated back propagation through time.

所以第一步：下载一个在ImageNet上预训练用于分类的CNN模型。然后我们将截掉该网络的最后两层。现在这里我们实际上要在有限长度的序列上操作，因此与语言建模情况不同，我们主要关注的是在无限数据流上操作并通过时间进行截断反向传播。

But now in image captioning we actually want to focus on sequences that have some actual start and actual end. So then what we always do is we start the first element of the sequence is always a special token called start, which just means like this is the start of a new sentence that we want you to generate.

但现在在图像描述中，我们实际上要关注具有实际开始和实际结束的序列。所以我们总是这样做：序列的第一个元素始终是一个称为"start"的特殊标记，这仅仅意味着这是我们要你生成的新句子的开始。

But now we need to somehow connect the data from the convolutional neural network into the recurrent neural network. The way that we do this is we slightly modify the recurrence formula that we use for producing hidden states of the recurrent neural network.

但现在我们需要以某种方式将来自卷积神经网络的数据连接到循环神经网络。我们这样做的方式是稍微修改用于产生循环神经网络隐藏状态的递推公式。

Recall that previously we had seen this recurrent neural network that applies a linear transform to the input, a linear transform to the previous hidden state, and then squashes through tanh to give the next hidden state. Well now to incorporate the image data, we're gonna have three inputs at each time step of the neural network.

回想一下，之前我们见过这种循环神经网络，它对输入应用线性变换，对先前的隐藏状态应用线性变换，然后通过tanh激活函数来给出下一个隐藏状态。现在为了整合图像数据，我们在神经网络的每个时间步将有三个输入。

We're going to have the current element of the sequence that we're processing, we're going to have the previous hidden state, and we're also going to have this feature that was extracted from the top of this convolutional neural network pre-trained on ImageNet.

我们将有正在处理的序列当前元素，我们将有先前的隐藏状态，我们还将有这个从在ImageNet上预训练的卷积神经网络顶部提取的特征。

So now given these three inputs, we will apply a separate weight or linear projection to each of these three different inputs, add them together, and again squash through tanh. So now you can see that we've modified the recurrence relation of this recurrent neural network that allows it to incorporate this additional type of information which is the feature vector coming out of the image.

所以现在给定这三个输入，我们将对这三个不同的输入分别应用单独的权重或线性投影，将它们加在一起，然后再次通过tanh激活函数。现在你可以看到我们已经修改了这个循环神经网络的递推关系，使其能够整合这种额外类型的信息，即来自图像的特征向量。

After that then things proceed very much like they did in the language modeling case. So then what we do is in the test time case, this is going to predict a distribution over the tokens or the words in our vocabulary. We sample from that distribution to get the first word, in this example "man".

之后的过程与语言建模情况非常相似。在测试时，我们会预测词汇表中标记或单词的分布。我们从该分布中采样得到第一个单词，在这个例子中是"man"。

We pass that back to be processed by the recurrent neural network as the next element of the input sequence, pass it again and then sample the next word, and then this repeats. So this would be "man in straw hat". And then here's the answer to your question: a special token called stop or end. So whatever we're processing sequences of finite length, it's very common to add these two extra special tokens into the vocabulary: one called the start token that we put at the beginning of every sequence, and one called the end token that we insert. Then during training, we force the network to predict the end token at the end of every sequence, and during testing, once the network chooses to sample the end token, then we stop sampling and that's the end of the output that we generate.

我们将其传回由循环神经网络处理，作为输入序列的下一个元素，再次传递，然后采样下一个单词，这个过程重复进行。所以这将是"戴草帽的男人"。然后这是你问题的答案：一个称为"stop"或"end"的特殊标记。当我们处理有限长度序列时，通常会在词汇表中添加两个特殊标记：一个称为起始标记，我们将其置于每个序列的开头；另一个称为结束标记，我们将其插入序列末尾。在训练过程中，我们强制网络在每个序列末尾预测结束标记；在测试过程中，一旦网络选择采样结束标记，我们就停止采样，这就是我们生成输出的终点。

Did that answer your question about how we know when to stop? Good. There was a question here: yes, the question is what's the difference between blue and purple? So here these three inputs: green is the input of the sequence of the current time step, so that would be like one of these input tokens "start man in straw hat" that would be the X at the current timestamp. The H at the current time step is the blue thing - that's the previous hidden state from the previous time step of the sequence, which would be something like H when we're trying to predict h2 then H would be H1 which is the hidden state at the previous time step. And now the purple is the feature vector coming from the convolutional neural network, which we're calling V, which is going to be a single vector that we extract once from the convolutional network and then pass it to each of the time steps of the recurrent neural network.

这回答了您关于如何知道何时停止的问题吗？很好。这里有个问题：蓝色和紫色有什么区别？这三个输入中：绿色是当前时间步的序列输入，就像这些输入标记中的"戴草帽的男人开始"这样的标记，就是当前时间戳的X。当前时间步的H是蓝色的部分——这是序列前一个时间步的隐藏状态，比如当我们试图预测h2时，H就是H1，即前一个时间步的隐藏状态。而紫色是来自卷积神经网络的特征向量，我们称之为V，这是一个我们从卷积网络中提取一次的单一向量，然后将其传递给循环神经网络的每个时间步。

So with that, then for each of these three inputs we have a separate associated weight matrix with width dimensions such that they can be added in a way that doesn't crash. Does that clarify a little bit? OK.

因此，对于这三个输入中的每一个，我们都有一个独立的关联权重矩阵，其宽度维度使得它们能够以不冲突的方式相加。这样解释清楚一些了吗？好的。

So then once we've got this, it's fun to look at some results for these things. You know, this computer got to look at images, we got to look at results and have a little fun. So sometimes when you train this thing on a data set of images and associated captions, sometimes these image captioning models seem to produce really shockingly good descriptions of images.

当我们完成这些后，看看这些模型的结果是很有趣的。你知道，计算机能够查看图像，我们能够查看结果并享受一些乐趣。有时当你在图像和相关标题的数据集上训练这些模型时，这些图像描述模型似乎能产生令人惊讶的优秀图像描述。

So here the one at the upper left it says "a cat sitting on a suitcase on the floor" which is like pretty good - that's like a lot more detail than we were able to get out of our previous image classification models that just output a single label. Or maybe in the upper right it says "a white teddy bear sitting in the grass" that looks pretty correct. At the bottom it says "two people walking on the beach with surfboards", "a tennis player in action on the court". So it's like giving us these really non-trivial descriptions that seem really exciting.

比如左上角的这张图，它描述为"一只猫坐在地板上的行李箱上"，这相当不错——比我们之前只能输出单个标签的图像分类模型提供了更多细节。或者右上角写着"一只白色泰迪熊坐在草地上"，看起来相当准确。底部写着"两个人拿着冲浪板在海滩上行走"，"网球选手在球场上比赛"。这些描述都非常有意义，令人兴奋。

So when these first papers came out that were first doing these image captioning results, they got people really excited because for the first time these networks were saying very non-trivial things about the images that they were looking at. They were no longer just single labels like dog or cat or truck.

因此，当第一批实现这些图像描述结果的论文发表时，人们非常兴奋，因为这是第一次这些网络能够对它们看到的图像说出非常有意义的内容。它们不再仅仅是像"狗"、"猫"或"卡车"这样的单一标签。

But these image captioning models actually it turns out are not that smart, and it's actually really instructive to look at the cases where they fail as well. So here's an example: if we feed this image to a trained image caption model, it says "a woman is holding a cat in her hand" which I think it says that because somehow the texture of the woman's coat maybe looks kind of like the texture of cat fur that it would have seen in the training set.

但事实证明这些图像描述模型实际上并不那么聪明，查看它们失败的案例实际上非常有启发性。这里有一个例子：如果我们把这张图像输入到训练好的图像描述模型中，它会说"一个女人手里抱着一只猫"，我认为它这样说是因为某种程度上这位女士外套的纹理可能看起来像它在训练集中见过的猫毛纹理。

Or here if we look at this, it says "a person holding a computer mouse on a desk" well that's because this data set came out before iPhones were prevalent, so whenever someone was holding something near a desk it was always a computer mouse. Another cell phone here it says "a woman is standing on a beach holding a surfboard" which is like completely wrong, but the data set where this was trained has a lot of images of people holding surfboards on beaches.

或者看这里，它说"一个人拿着电脑鼠标在桌子上"，这是因为这个数据集出现在iPhone普及之前，所以每当有人在桌子附近拿着什么东西时，总是电脑鼠标。另一张手机图片它说"一个女人站在海滩上拿着冲浪板"，这完全错误，但训练这个模型的数据集有很多人在海滩上拿着冲浪板的图像。

So basically whatever it sees someone standing near water, it just wants to say it's a person holding a surfboard on the beach, even if that's not actually what's in the image at all. We have a similar problem in this example: so this is a spiderweb kind of in a branch and it says "a bird is perched on a tree branch" again maybe it's just sort of copying whatever it saw - a tree branch there was always a bird there, so whenever it sees that branch just wants to say a bird perched on a tree branch even if that's actually not what it says at all.

基本上，无论何时它看到有人站在水边，它就想说这是一个人在海滩上拿着冲浪板，即使这根本不是图像中的实际内容。在这个例子中我们有类似的问题：这是一个树枝上的蜘蛛网，它却说"一只鸟栖息在树枝上"，也许它只是在复制它看到的东西——有树枝的地方总是有鸟，所以每当它看到树枝就想说一只鸟栖息在树枝上，即使实际上根本不是这样。

Maybe one more example: here it says "a man in a baseball uniform throwing a ball". So now this one I think it's really interesting right because it knows it's a man in baseball uniform but it kind of gets confused about exactly what action is happening in the scene. But when we look at this, we have this human understanding of like physics and we know that there's no way he could be throwing a ball from that position, so we know that he's probably sliding in there to try to catch the ball.

也许再看一个例子：这里它说"一个穿棒球服的男人在扔球"。这个例子我觉得很有趣，因为它知道这是一个穿棒球服的男人，但对场景中具体发生了什么动作有些混淆。但当我们看这个时，我们有人类对物理的理解，我们知道他不可能从那个位置扔球，所以我们知道他可能是在滑垒试图接球。

But that kind of fine-grain distinction is just something that's completely lost on this model. So I think these image captioning models are pretty exciting but they're actually like still pretty dumb, and they're pretty far from solving this computer vision task. And I think that's really clear when you look at these failure modes.

但这种细微的区分对这个模型来说完全无法理解。所以我认为这些图像描述模型虽然令人兴奋，但实际上仍然相当笨拙，距离解决这个计算机视觉任务还很远。当你看到这些失败案例时，这一点就非常清楚了。

By the way, these image captioning - you'll have fun, you'll get to implement your own image captioning model on the fourth homework assignment which will be out shortly after the midterm.

顺便说一下，这些图像描述——你们会很有趣，你们将在第四次作业中实现自己的图像描述模型，这个作业将在期中考试后不久发布。

So now another thing we need to talk about is gradient flow through these recurrent neural networks. So here is a little diagram that kind of illustrates pictorially what's going on in this vanilla or Elman recurrent neural network that we've been considering so far.

现在我们需要讨论的另一件事是通过这些循环神经网络的梯度流。这里有一个小图表，形象地展示了我们一直在考虑的普通或Elman循环神经网络中发生的情况。

So here is showing the processing for one time step of the recurrent neural network. So here we have the input XT coming in at the bottom, we have the previous hidden state HT coming in at the left, then you can imagine that these are concatenated and then have this linear transform by the weight matrix and then squash this tanh non-linearity.

这里展示了循环神经网络一个时间步的处理过程。底部输入的是XT，左侧输入的是前一个隐藏状态HT，然后你可以想象这些被连接起来，然后通过权重矩阵进行线性变换，再通过tanh非线性函数进行压缩。

So now you should be able to recognize some problems about what's going to happen during the gradients in the backward pass of this model. If we imagine what happens during the backward pass at this model, then during back propagation we're going to receive the derivative of the loss with respect to the output hidden state at HT, and we want to compute and we need to back propagate through this little RNN cell to compute the gradient of the loss with respect to the input hidden state HT minus 1.

现在你应该能够认识到在这个模型的反向传播过程中梯度会出现的一些问题。如果我们想象在这个模型的反向传播过程中会发生什么，那么在反向传播期间，我们将接收到损失相对于输出隐藏状态HT的导数，我们需要通过这个小型RNN单元进行反向传播，以计算关于输入隐藏状态HT减1的损失梯度。

Now there are two really bad things happening in this back propagation. One is that we're back propagating through an H non-linearity, and we told you repeatedly that tanh nonlinearities were really bad and you should not have used them. So that already seems like a potentially bad setup, but you know these RNNs were invented in the 90s - they didn't really know better back then, so maybe we can excuse that.

在反向传播过程中存在两个严重问题。首先是我们正在通过H非线性函数进行反向传播，我们曾多次强调tanh非线性函数效果很差，本不应使用。这已经构成了潜在问题，但考虑到这些RNN是在90年代发明的，当时人们认知有限，或许可以谅解这一点。

But another big problem that happens when back propagating through this recurrent neural network is that when we back propagate through this matrix multiply stage, during back propagation it's going to cause us to multiply by the transpose of that weight matrix. Because when you back propagate through a matrix multiplication, you're going to multiply by the transpose of that weight matrix.

但通过这个循环神经网络进行反向传播时的另一个大问题是，当我们通过矩阵乘法阶段进行反向传播时，实际上会导致我们乘以该权重矩阵的转置。因为通过矩阵乘法进行反向传播时，你会乘以该权重矩阵的转置。

Now think about what happens when we have not just a single recurrent neural network cell, but now we're unrolling this recurrent neural network for many, many time steps. You can see that as the gradient flows backward through this entire sequence, every time we flow through this recurrent neural network cell, it's going to multiply the upstream gradient by this weight matrix.

现在考虑当我们不仅有单个循环神经网络单元，而是将这个循环神经网络展开为许多时间步长时会发生什么。你可以看到当梯度向后流过整个序列时，每次通过这个循环神经网络单元，都会将上游梯度乘以这个权重矩阵。

So during back propagation, we're going to take our gradient and just multiply it over and over and over again by this exact same weight matrix W transpose. This is basically really bad. Suppose here I'm only showing four in the slide, but imagine we're unrolling the sequence for like 100 or 200 or thousand time steps.

因此在反向传播过程中，我们将反复不断地用完全相同的权重矩阵W转置来乘以我们的梯度。这基本上非常糟糕。假设我在幻灯片中只展示了四个步骤，但想象一下我们将序列展开为100、200甚至1000个时间步长。

Then during back propagation, we're multiplying by the same matrix a thousand times. Now that can go really bad in two ways. One is that if the matrix is too big as measured by its largest singular value, then multiplying by the same matrix repeatedly is going to cause it to just blow up and explode to infinity.

那么在反向传播过程中，我们将同一矩阵乘以一千次。这可能在两个方面变得非常糟糕。首先，如果矩阵的最大奇异值过大，那么重复乘以同一矩阵将导致梯度爆炸至无穷大。

On the other hand, if that weight matrix is somehow too small as measured by its smallest singular value being less than one, then those gradients will just tend to shrink and disappear and vanish towards zero during back propagation.

另一方面，如果权重矩阵的最小奇异值小于1而显得过小，那么这些梯度在反向传播过程中就会不断收缩、消失并趋近于零。

So basically we're caught on this knife edge where if the singular value is just a little bit greater than one, then our gradients will explode to infinity. If our singular value is just a little bit less than 1, then our gradients will vanish to 0, and we'll have either this exploding gradient problem or this vanishing gradient problem as they're called.

基本上我们陷入了两难境地：如果奇异值略大于1，梯度就会爆炸至无穷大；如果奇异值略小于1，梯度就会消失为0，这就是所谓的梯度爆炸问题或梯度消失问题。

The only way we can get this thing to train is if somehow we arranged for our weight matrix to have all its singular values exactly 1, and that's the only way we're going to be able to get stable training out of this kind of network over very, very long sequences. So that seems like a problem.

要让这种网络能够训练的唯一方法是，设法让权重矩阵的所有奇异值恰好等于1，这是我们在非常长的序列上获得稳定训练的唯一途径。这显然是个问题。

There is one kind of a hack that people sometimes use to deal with this exploding gradient problem called gradient clipping. Here what we're doing is we're not using the true gradient when we're doing back propagation.

人们有时会使用一种称为梯度裁剪的技巧来处理梯度爆炸问题。这里我们的做法是在进行反向传播时不使用真实的梯度。

After we compute the gradient of the loss with respect to the hidden state, we check the Euclidean norm of that vector because the gradient of the loss with respect to the hidden state is just a vector. Then if the Euclidean norm of that local gradient is too high, we just clip it down and cause it to be smaller and we continue back propagation.

在计算了关于隐藏状态的损失梯度后，我们检查该向量的欧几里得范数，因为关于隐藏状态的损失梯度只是一个向量。如果该局部梯度的欧几里得范数过高，我们就将其裁剪并使其变小，然后继续反向传播。

So now basically what we're doing with this idea of gradient clipping is that we're computing the wrong gradients on purpose that will hopefully not explode at least. This is like kind of a horrible dirty hack - it means you're not actually computing the true gradients of the loss with respect to the model weights anymore, but this is a heuristic that people sometimes use in practice to overcome this exploding gradient problem.

因此，通过梯度裁剪的理念，我们实际上是在故意计算错误的梯度，希望至少不会发生爆炸。这就像一种糟糕的临时解决方案——意味着你不再计算损失关于模型权重的真实梯度，但这是人们在实践中有时用来克服梯度爆炸问题的启发式方法。

Now the other problem is what do we do how do we deal with these vanishing gradients and how do we avoid this problem of singular values being very very small. Well here kind of the basic thing that people do is they throw away this architecture and they use a different flavor of recurrent neural network instead.

另一个问题是我们如何处理这些梯度消失现象，以及如何避免奇异值过小的问题。这里人们的基本做法是抛弃这种架构，转而使用另一种类型的循环神经网络。

So far we've been talking about this vanilla recurrent neural network, but there's another very common variant people use instead called long short-term memory or LSTM. LSTM is a very common acronym you should get to know very well.

到目前为止我们讨论的都是普通循环神经网络，但人们使用的另一种常见变体称为长短期记忆网络或LSTM。LSTM是一个非常常见的缩写词，你应该非常熟悉。

This is a slightly complicated and confusing looking functional form, and it's not really clear at the outset when you first see these equations like what's going on or why this is solving any problems whatsoever.

这是一个看起来稍微复杂和令人困惑的函数形式，当你第一次看到这些方程时，并不清楚发生了什么或为什么这能解决任何问题。

But basically the intuition of this LSTM is that rather than keeping a single hidden vector at every time step, instead we're going to keep two different hidden vectors at every time step. One is called CT the cell state, and the other is HT the hidden state.

但基本上LSTM的直觉是，不是在每个时间步保留单个隐藏向量，而是在每个时间步保留两个不同的隐藏向量。一个称为CT细胞状态，另一个称为HT隐藏状态。

Then at every time step we're going to use the previous hidden state and the current input to compute four different gate values: I, F, O and G. Those will be used to compute the updated cell state and then also be used to compute the updated hidden state.

然后在每个时间步，我们将使用先前的隐藏状态和当前输入来计算四个不同的门值：I、F、O和G。这些将用于计算更新的细胞状态，然后也用于计算更新的隐藏状态。

I also think it's interesting to point out that this paper actually was published in 1997 that proposed this LSTM architecture. Maybe for the first 10 years it came out it wasn't very well known, and then starting around about 2013 or 2014 people sort of rediscovered this LSTM architecture and it became very very popular again starting around that time.

我还想指出，提出这种LSTM架构的论文实际上发表于1997年。在它问世的最初10年里并不太为人所知，然后从2013或2014年左右开始，人们重新发现了这种LSTM架构，从那时起它再次变得非常流行。

Nowadays this LSTM architecture is one of the most commonly used recurrent neural network architectures used to process sequences in practice.

如今，这种LSTM架构是在实践中用于处理序列的最常用的循环神经网络架构之一。

To unpack a little bit more about what this thing is actually doing, let's look at it this way. Here what we're doing is that at every time step we're receiving some input XT and we are also receiving the previous hidden state HT minus 1.

为了进一步理解这个东西实际上在做什么，让我们这样来看。这里我们在每个时间步接收某个输入XT，同时也接收先前的隐藏状态HT减1。

Just like the recurrent neural network, we're going to concatenate the input vector XT and the previous vector HT minus 1, concatenate them and then multiply them by some weight matrix. But now in the recurrent neural network, in the vanilla or Elman recurrent neural network case, the output of this matrix multiplication basically was the next hidden state up to a non-linearity. However for the LSTM instead, what we did was the output of this matrix multiplication we're going to carve up into four different vectors, each of size H where H is the number of elements in the hidden unit.

就像循环神经网络一样，我们将连接输入向量XT和先前的向量HT减1，将它们连接起来然后乘以某个权重矩阵。但在循环神经网络中，在普通或Elman循环神经网络的情况下，这个矩阵乘法的输出基本上就是经过非线性变换后的下一个隐藏状态。然而对于LSTM，我们将这个矩阵乘法的输出分割成四个不同的向量，每个向量的大小为H，其中H是隐藏单元中的元素数量。

These will be called these four gates: the input gate, the forget gate, the output gate, and this other one I don't know what to call it. I just called the gate gate because I can't think of a better name. But the intuition here is that now rather than directly predicting the output from this matrix multiply, instead we predict these four gates and then we use these four gates to update the cell state and update the hidden state.

这四个向量被称为四个门：输入门、遗忘门、输出门，以及另一个我不知道该怎么命名的门。我暂时称之为"门控门"，因为我想不出更好的名字。这里的直观理解是，现在我们不是直接从矩阵乘法预测输出，而是预测这四个门，然后用这四个门来更新细胞状态和隐藏状态。

So you'll notice that these four gates each go through different nonlinearities. The input, forget and output gates all go through a sigmoid nonlinearity, which means they're all going to be between zero and one. And now the gate gate goes through a tanh non-linearity, which means it's between minus 1 and 1.

你会注意到这四个门分别经过不同的非线性变换。输入门、遗忘门和输出门都经过sigmoid非线性变换，这意味着它们的值都在0到1之间。而门控门经过tanh非线性变换，这意味着它的值在-1到1之间。

So now if you look at this region in CT, you can see that in order to compute the next cell state what we do is we take the previous cell state C t minus 1 and we multiply it element-wise by the forget gate. So now the forget gate remember is all elements between 0 and 1, so then the forget gate has the interpretation that it tells us for each element of the cell state do we want to reset it to 0 or continue propagating that element of the cell state forward in time.

现在如果你观察CT这个区域，你会看到为了计算下一个细胞状态，我们取前一个细胞状态C t减1，然后将其与遗忘门进行逐元素相乘。遗忘门的所有元素都在0到1之间，因此遗忘门的解释是：对于细胞状态的每个元素，它告诉我们是要将其重置为0，还是继续在时间上向前传播该细胞状态元素。

That's how we use the forget gate. And then we add then ER and then we also add on the element wise product of the input gate and the gate gate. So now the gate gate is kind of remember between negative 1 and 1, so that's kind of what we want to write into the cell state at every point every element of the cell state.

这就是我们使用遗忘门的方式。然后我们加上ER，再加上输入门和门控门的逐元素乘积。门控门的值在-1到1之间，这基本上就是我们想要写入细胞状态每个点的内容。

And now the input gate is again between 0 & 1 because it's a sigmoid that we element multiply element wise with the gate gate. So in kind of the entry into the interpretation is that the gate gate tells us at every point in a cell we can either add 1 or subtract 1, and the input gate tells us how much do we actually want to add or subtract at every point in the cell.

输入门的值又在0到1之间，因为它是一个sigmoid函数，我们将其与门控门进行逐元素相乘。这样解释的话，门控门告诉我们在细胞的每个点上我们可以加1或减1，而输入门告诉我们在细胞的每个点上我们实际想要加减多少。

So that's how we use the input, forget and gate gates. And now to compute the final output state, we're going to do is take the cell state squash it through a tanh non-linearity and then multiply element-wise by the output gate.

这就是我们使用输入门、遗忘门和门控门的方式。现在为了计算最终的输出状态，我们要做的是取细胞状态，通过tanh非线性函数进行压缩，然后与输出门进行逐元素相乘。

So now the interpretation here is that the cell state is this kind of internal hidden state that's internal to the processing of the LSTM, and the LSTM can choose to reveal parts of its cell state at every time step as modulated by the output gate. So then the output gate tells us how much of each element of the cell do we want to reveal and put in place into the hidden state.

这里的解释是，细胞状态是LSTM处理过程中内部的一种隐藏状态，LSTM可以选择在每个时间步通过输出门的调节来揭示其细胞状态的部分内容。因此输出门告诉我们想要揭示细胞的每个元素的多少，并将其放入隐藏状态中。

Because we could put we couldn't write we put could put the out part if we put some element of the output gate to 0, then it would be sort of hiding that element of the cell and keeping it as kind of a private variable internal to the LSTM.

因为如果我们将输出门的某个元素设置为0，那么它就会隐藏细胞的该元素，并将其作为LSTM内部的私有变量保存。

So there's kind of a tradition in explaining LSTM's that you've got to have a number of very confusing diagrams to try to explain them, so here's mine. So one way that you can look at the processing of a single LSTM is that we receive from the left and from we received two things from the previous time step: we were to receive the previous cell state CT minus 1 and the previous hidden state H minus one.

在解释LSTM时有一个传统，就是必须用一些非常令人困惑的图表来解释它们，这是我的版本。看待单个LSTM处理的一种方式是：我们从左侧接收来自前一个时间步的两个输入：前一个细胞状态CT减1和前一个隐藏状态H减1。

And now we concatenate the previous hidden state and the current input XT, we multiply them by this weight matrix W, and then we divide them up into these four gates. And then we use these four gates to compute the cell state, the next cell states C T that we're going to pass along to the next time step, as well as produce the next hidden state HT that will pass on to the next time step.

然后我们将前一个隐藏状态和当前输入XT连接起来，用权重矩阵W乘以它们，然后将它们分成这四个门。接着我们使用这四个门计算细胞状态，即下一个细胞状态C T，我们将把它传递给下一个时间步，同时生成下一个隐藏状态HT，它也将传递给下一个时间步。

And now what's interesting about looking at the LSTM this way is that it gives us some different perspective on gradient flow through the LSTM, especially compared to the vanilla RNN. So now if you imagine back propagating from the next cell state C T back to the previous cell state CT minus one, now you can see that this is a fairly friendly gradient pathway.

以这种方式观察LSTM的有趣之处在于，它为我们提供了关于梯度在LSTM中流动的不同视角，特别是与普通RNN相比。现在如果你想象从下一个细胞状态C T反向传播到前一个细胞状态CT减1，你会发现这是一个相当友好的梯度路径。

Because when we back propagate this then we first back propagate through a sum node. So then what I remember when we back propagate through a sum node then what happens is that we copy the gradients, so back propagating through a sum is very friendly so that's not gonna kill the information.

因为当我们反向传播时，首先经过一个求和节点。我记得当我们通过求和节点反向传播时，会发生梯度复制，所以通过求和节点的反向传播非常友好，不会破坏信息。

So you first hit this sum node and that's just going to distribute the gradients down to the inner parts of the LSTM as well as backward in time to the previous cell state. And then we back propagate through this element wise multiplication with the forget gate.

所以你首先遇到这个求和节点，它只是将梯度分布到LSTM的内部部分，以及时间上向后到前一个细胞状态。然后我们通过这个与遗忘门的逐元素乘法进行反向传播。

So now this has the potential to destroy information, but this is not directly back propagating through a sigmoid non-linearity. That from the perspective of computing the derivative with respect to the previous cell state, this is just multiplies just back propagating through this element wise constant multiply, we were multiplying by a constant between zero and one.

这有可能破坏信息，但这不是直接通过sigmoid非线性函数进行反向传播。从计算关于前一个细胞状态的导数的角度来看，这只是通过逐元素常数乘法进行反向传播，我们乘以的是一个0到1之间的常数。

So now this again has the potential to destroy information if that forget gate is very close to zero, but if the forget gate is very close to one then back propagating backwards to the next cell state or to the previous cell state is basically not going to destroy any information.

所以如果遗忘门非常接近0，这有可能破坏信息，但如果遗忘门非常接近1，那么向后传播到下一个细胞状态或前一个细胞状态基本上不会破坏任何信息。

So now what you'll notice is that when we back propagate from the next cell state to the previous cell state, then we are not back propagating through any nonlinearities and we're also not back propagating through any matrix multiplies. So this top level pathway through the LSTM is now a very friendly pathway for gradients to propagate backwards during the backward pass.

所以你会注意到，当我们从下一个细胞状态反向传播到前一个细胞状态时，我们没有通过任何非线性函数进行反向传播，也没有通过任何矩阵乘法进行反向传播。因此通过LSTM的这条顶层路径在反向传播过程中为梯度向后传播提供了一个非常友好的路径。

So now if you imagine kind of chaining together these multiple LSTM cells one after to process along sequence, then this upper pathway through all the cell states kind of forms this uninterrupted gradient superhighway along which the model can easily pass information backwards through time, even through many, many time steps. So this form, because now we see that this kind of funny formulation of the LSTM, basically the whole point of it is to achieve these better dynamics of the gradient flow during a backward pass compared to the vanilla RNN.

所以现在如果你想象将这些多个LSTM单元一个接一个地连接起来处理序列，那么通过所有细胞状态的这条上层路径就形成了一条不间断的梯度高速公路，沿着这条路径，模型可以非常轻松地将信息在时间上向后传递，甚至跨越很多很多时间步长。因此这种形式，因为我们现在看到LSTM这种看似有趣的表述，其根本目的就是为了在反向传播过程中实现比普通RNN更好的梯度流动态。

Yeah, yeah. So the question is, we still do need to backpropagate through the h_t through to the weights, so that could potentially give us some problems. But kind of the hope here is that for the vanilla RNN, our only source of gradient is coming through this very long, long set of time dependencies. So then if our information gets very diluted across these many, many time steps, then we can't learn.

是的，是的。所以问题是，我们仍然需要通过h_t反向传播到权重，这可能会给我们带来一些问题。但这里的希望在于，对于普通RNN，我们唯一的梯度来源是通过这个非常长的时间依赖链。因此，如果我们的信息在这么多时间步长中被严重稀释，那么我们就无法学习。

But now for the LSTM, there's always going to be some pathway along which information is kind of preserved through the backward pass. So you're correct that when we backpropagate into the weights, then we are going to backpropagate through a matrix multiply and through these nonlinearities. But there exists a pathway along which we do not have to backpropagate through any matrix multiplies or any nonlinearities, so the hope is that that would be enough to keep the flow of information going during the learning process.

但现在对于LSTM，总会存在某些路径，信息在反向传播过程中得以保留。所以你是对的，当我们反向传播到权重时，我们将通过矩阵乘法和这些非线性函数进行反向传播。但存在一条路径，我们不必通过任何矩阵乘法或任何非线性函数进行反向传播，因此希望这足以在学习过程中保持信息流动。

Yeah, yeah. So the solution still has some h_t at the end, and usually for an LSTM, the cell state is usually considered kind of a private variable to the LSTM. And then usually you use the hidden state h_t to either predict, to do whatever prediction you want on the output of the LSTM.

是的，是的。所以解决方案最后仍然有h_t，通常对于LSTM，细胞状态通常被认为是LSTM的一种私有变量。然后你通常使用隐藏状态h_t来在LSTM的输出上进行你想要的任何预测。

It's now also this design of the LSTM as sort of giving us this uninterrupted gradient superhighway should remind you of another architecture that we've already seen in this class. Can anyone guess? Yeah, the ResNet. So remember that in the residual networks, we had this problem of training very, very deep convolutional neural networks with perhaps hundreds of layers.

现在LSTM的这种设计，就像为我们提供了一条不间断的梯度高速公路，这应该让你想起我们在这门课中已经见过的另一种架构。有人能猜到吗？是的，ResNet。所以请记住，在残差网络中，我们遇到了训练可能具有数百层的非常非常深的卷积神经网络的问题。

And there we saw that by adding these additive skip connections between layers in a deep convolutional network, then it gave us very good gradient flow across many, many, many layers. And it's basically the same idea with the LSTM, that we've got these additive connections that are now giving us this uninterrupted gradient flow, not across many, many layers, but many, many time steps in time.

在那里我们看到，通过在深度卷积网络的层之间添加这些加性跳跃连接，它为我们提供了跨越很多很多层的非常好的梯度流。而LSTM基本上也是同样的想法，我们有了这些加性连接，现在为我们提供了这种不间断的梯度流，不是跨越很多很多层，而是跨越时间上的很多很多时间步长。

So I think the LSTM and the ResNet actually share a lot of intuition. And as a fun pointer, there's another kind of a thing called the Highway Network that actually came out right before ResNet that looks even more like an LSTM that all kind of cement these connections a little bit more. So you can check that out if you're interested in those connections.

所以我认为LSTM和ResNet实际上共享很多直觉。作为一个有趣的提示，还有另一种叫做 Highway Network 的东西，实际上在ResNet之前出现，看起来甚至更像LSTM，所有这些都更加巩固了这些连接。所以如果你对那些连接感兴趣，可以查一下。

Any questions about this LSTM? If we move on to something else? Yeah, yeah. The question is like, how do you possibly come up with this? Well, it's called research. So I mean it's this iterative process of you have some idea that maybe I have this idea that I think if I do this thing then things will improve, and then I try it and it doesn't work.

对LSTM有什么问题吗？我们继续讲别的？是的，是的。问题是，你怎么可能想出这个？嗯，这叫做研究。我的意思是，这是一个迭代的过程，你有一些想法，也许我有这个想法，认为如果我做这件事，事情就会改善，然后我尝试了，但它不奏效。

Then I have another idea and I try it and it doesn't work, and I have another idea and I try and it doesn't work, and then eventually you can have an idea, or maybe not you but somebody is gonna come up with an idea. Sorry, I don't mean you specifically, I mean kind of you generically or me generically, right? Any individual person, you know, it'd be troubling.

然后我有另一个想法，我尝试了，但它不奏效，然后我有另一个想法，我尝试了，但它不奏效，然后最终你可能会有一个想法，或者也许不是你，但有人会想出一个想法。抱歉，我不是特指你，我是泛指你或我，对吧？任何个人，你知道，这会是令人困扰的。

But then as a community, hopefully over time, eventually someone will come up with an idea that actually works well that then gets adopted by the community. And if you look at the development of the LSTM, actually it got more complex over time. So kind of it, I actually this would be kind of fun to go to read this history of papers and see exactly how it developed.

但然后作为一个社区，希望随着时间的推移，最终有人会想出一个真正有效的想法，然后被社区采纳。如果你看看LSTM的发展，实际上它随着时间的推移变得越来越复杂。所以，实际上去读这些论文的历史，看看它究竟是如何发展的，会很有趣。

But kind of it, they start with one thing and then they make it a little bit more complicated, it works better, and you kind of iteratively refine these things over long periods of time. But yeah, if I knew how to come up with things as impactful as the LSTM, like oh man, that'd be awesome. I wish. Okay, any other questions on LSTM?

但大致上，他们从一件事开始，然后让它变得更复杂一点，它工作得更好，然后你在很长一段时间内迭代地改进这些东西。但是，是的，如果我知道如何想出像LSTM这样有影响力的东西，哦，那太棒了。我希望。好了，关于LSTM还有其他问题吗？

Okay, so then so far we've talked about single layer RNN. So this is something I just want to briefly mention right that we've got this process, the sequence of inputs we process, we produce this sequence of hidden layers, and we use this sequence of hidden vectors to produce a sequence of outputs.

好的，那么到目前为止我们已经讨论了单层RNN。所以这是我想要简要提及的事情，我们有了这个过程，我们处理输入序列，我们产生这个隐藏层序列，我们使用这个隐藏向量序列来产生一个输出序列。

Well, we've seen sort of from processing images that more layers is often better and more layers often allows us to achieve models perform better on whatever task we want to use. So clearly we'd like to have some way to apply this intuition of stacking layers also to recurrent neural networks.

嗯，我们从处理图像中已经看到，更多的层通常更好，更多的层通常允许我们实现的模型在我们想要使用的任何任务上表现更好。所以很明显，我们希望能够将这种堆叠层的直觉也应用到递归神经网络上。

So we can do that by just applying another recurrent neural network on top of the sequence of hidden states that are produced by a first recurrent neural network. So this is called a multi-layer or deep recurrent neural network.

所以我们可以通过在第一个递归神经网络产生的隐藏状态序列之上应用另一个递归神经网络来实现这一点。所以这被称为多层或深度递归神经网络。

So here the idea is we've got one recurrent neural network that processes our raw input sequence and then produces the sequence of hidden states. And now that sequence of hidden states is just treated as the input sequence to another recurrent neural network that then produces its own second sequence of hidden states.

所以这里的想法是，我们有一个递归神经网络处理我们的原始输入序列，然后产生隐藏状态序列。现在这个隐藏状态序列就被当作另一个递归神经网络的输入序列，然后它产生自己的第二个隐藏状态序列。

And you know, you don't have to stop with two, you can stack these things as far as your GPU memory will take you or as far as much space you have on the slide in this case. But you can imagine stacking these recurrent neural networks to multiple to many, many layers.

你知道，你不必止步于两层，你可以堆叠这些网络，只要你的GPU内存允许，或者在这种情况下，只要幻灯片上有足够的空间。但你可以想象将这些递归神经网络堆叠到多层，很多很多层。

I think in practice for recurrent neural networks, it's actually, you know, you'll often see improvements from like maybe up to the three or five layers or so. But I think in recurrent neural networks, it's really not so common to have these extremely, extremely deep models like we do for convolutional networks.

我认为在实践中对于递归神经网络，实际上，你知道，你通常会看到改进，可能最多到三层或五层左右。但我认为在递归神经网络中，拥有像卷积神经网络那样极其深的模型真的不那么常见。

Yeah, it was our question. Yeah, so the question is, do we use the same weight matrix for these different layers or different weight matrix for these different layers? And usually you would use different weight matrices for these different layers.

是的，这是我们的问题。是的，所以问题是，我们对这些不同的层使用相同的权重矩阵，还是对不同的层使用不同的权重矩阵？通常你会对这些不同的层使用不同的权重矩阵。

So this is kind of, so you should think of each of these layers of RNN is kind of like the layers in our convolutional network. So then at every layer in a convolutional network, we typically use different weight matrices.

所以这有点，所以你应该把RNN的每一层都看作类似于我们卷积网络中的层。那么在卷积网络的每一层，我们通常使用不同的权重矩阵。

And very similarly, we'll often use different weight matrices at every layer in these deep recurrent neural networks. So then kind of going back to this. Another question about how people come up with these things: there are actually other different architectures. I mean, this is something that you can imagine people play around with a lot, but it's very appealing to think, "Oh, maybe I can just write down a better equation for a current neural network and we'll just make all of our problems go away."

并且非常相似，我们通常会在这些深度递归神经网络的每一层使用不同的权重矩阵。那么，大致回到这个。关于人们如何构思这些架构的另一个问题：实际上还存在其他不同的架构。我的意思是，你可以想象人们在这方面进行了大量尝试，但那种"也许我只需为现有神经网络写出更好的方程就能解决所有问题"的想法确实非常诱人。

So you'll see a lot of papers that try to play around with the exact architecture and the exact update rules of different recurrent neural networks. There are like a ton of papers about this that you'll see.

因此你会看到很多论文试图探索不同循环神经网络的具体架构和更新规则。关于这方面的论文数量众多，你会经常看到。

One that I want to point out and highlight is this one on the left called the gated recurrent unit, which looks kind of like a simplified version of an LSTM. I don't want to go into the details here, but it has this similar interpretation of using additive connections to improve the gradient flow.

我想特别指出并强调的是左边这个称为门控循环单元的架构，它看起来像是LSTM的简化版本。这里我不想深入细节，但它同样采用了加法连接来改善梯度流的类似设计理念。

As compute power has gotten cheaper and cheaper, people have also started to take brute-force approaches to this as well. There was a paper from a couple years ago that I liked, where they used evolutionary search on the space of update formulas and did a brute-force search over tens of thousands of update formulas, where each update formula then gives rise to different recurrent neural network architecture.

随着计算能力变得越来越廉价，人们也开始采用暴力搜索的方法。我喜欢几年前的一篇论文，他们在更新公式的空间上使用进化搜索，对数以万计的更新公式进行了暴力搜索，每个更新公式都会产生不同的循环神经网络架构。

So rather than trying to come up with it yourself, you just have some algorithm that automatically searches through update formulas and then tries these different update formulas on different tasks. What they found is that, here are examples of three of the update formulas that this paper found, but the general takeaway is that nothing really works that much better than an LSTM, so let's just stick with that.

因此，与其自己尝试设计，不如让算法自动搜索更新公式，然后在不同任务上测试这些不同的更新公式。他们发现的结果是，这里展示了该论文发现的三个更新公式示例，但总体结论是没有任何方法真正比LSTM表现好很多，所以我们还是继续使用它吧。

I think maybe the interpretation here is that there's actually a lot of different update formulas that would result in similar performance across a large number of tasks, and then maybe the LSTM just happens to be the one that people discovered first and has a nice historical precedent, so people continue using it.

我认为这里的解释可能是，实际上有很多不同的更新公式在大量任务上都能产生相似的性能，而LSTM可能恰好是人们最早发现的，并且具有良好的历史传承，所以人们继续使用它。

We also talked a couple lectures ago about this area of neural architecture search, where you train one neural network to produce the architecture of another neural network. We saw that in the context of convolutional networks, and it turns out people have also applied similar ideas to recurrent neural network architectures as well.

我们在几节课前也讨论过神经架构搜索这个领域，即训练一个神经网络来生成另一个神经网络的架构。我们在卷积网络的背景下看到过这个技术，事实证明人们也将类似的想法应用到了循环神经网络架构中。

Here there was a paper from Google where they actually have one recurrent neural network predict the architecture of the recurrent neural network cell encoded as a sequence, and then train that thing on hundreds of GPUs for a month. Eventually at the end of training, you get this learned architecture on the right that worked a little bit better than LSTM.

这里有一篇来自谷歌的论文，他们实际上让一个循环神经网络预测被编码为序列的循环神经网络单元架构，然后在数百个GPU上训练了一个月。最终在训练结束时，你得到了右边这个学习到的架构，它的表现比LSTM稍好一些。

But I think the takeaway from a lot of these papers is that even these LSTM and GRU architectures that we already have are actually pretty good in practice, and even if they're not perfectly optimal, they tend to perform well across a wide variety of problems.

但我认为从这些论文中得到的关键信息是，即使是我们现有的这些LSTM和GRU架构，在实践中实际上已经相当不错了，即使它们不是完全最优的，也往往能在各种问题上表现良好。

So then a summary today is that we introduced this whole new flavor of neural networks called recurrent neural networks. I hope I convinced you that they're cool and interesting and let you solve a lot of new types of problems.

那么今天的总结是，我们介绍了一种全新类型的神经网络——循环神经网络。我希望我已经让你们相信它们既酷又有趣，并且能帮助你们解决许多新型问题。

We saw in particular how this LSTM improves the gradient flow compared to these vanilla recurrent neural networks, and we finally saw this image captioning example that let us build neural networks that write natural language descriptions of images that you'll get to do on your fourth assignment.

我们特别看到了LSTM相比普通循环神经网络如何改善梯度流，最后我们还看到了图像描述生成的例子，这让我们能够构建为图像编写自然语言描述的神经网络，这将是你们第四次作业的内容。

But before we get to the fourth assignment, coming on next time will be the midterm, so hopefully that will be fun on Monday and I'll see you there.

但在我们进行第四次作业之前，下次将是期中考试，希望周一的考试会很有趣，我们到时见。