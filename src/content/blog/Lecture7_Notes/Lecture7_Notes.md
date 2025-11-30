---
title: 'Lecture7_Notes'
publishDate: 2025-11-26
description: 'TODO'
tags:
  - TODO
language: 'English'
heroImage: { src: './default.jpg', color: '#D58388' }
---

All right, welcome back to lecture seven. Today we're going to talk about convolutional neural networks, which is finally the major class of models that we'll use to process images going forward.

好的，欢迎回到第七讲。今天我们将讨论卷积神经网络，这最终将成为我们未来处理图像时使用的主要模型类别。

If you'll recall at the last lecture, we were talking about the back propagation algorithm which we could use to compute gradients in arbitrarily complex computational graphs. In particular, we saw that the use of this computational graph data structure made it very easy to compute gradients without having to use tons and tons of paper or lots of white board space or something to derive these complex expressions.

回顾上一讲，我们讨论了反向传播算法，该算法可用于计算任意复杂计算图中的梯度。特别值得注意的是，这种计算图数据结构的使用使得梯度计算变得非常容易，无需使用大量纸张或白板空间来推导这些复杂表达式。

Instead, we were able to compute gradients in expressions of arbitrary complexity by using the back propagation algorithm to walk forward over the graph and then the forward pass to compute the outputs, and then walk backward over the graph in the backward pass to compute the gradients.

相反，我们能够通过使用反向传播算法在图上向前遍历进行前向传播计算输出，然后在图上向后遍历进行反向传播计算梯度，从而处理任意复杂表达式的梯度计算。

We had this local viewpoint of the back propagation algorithm where in order to add a function or use a function inside of a computational graph, we needed to implement this little tiny local operator that would know how to compute its outputs during the forward pass given the inputs, and then in the backward pass would know how to compute the gradients with respect to its inputs given the upstream gradient of the loss with respect to its outputs.

我们采用了反向传播算法的局部视角，为了在计算图中添加或使用函数，我们需要实现这种小型局部操作符，它知道在前向传播中如何根据输入计算输出，然后在反向传播中知道如何根据损失相对于其输出的上游梯度来计算相对于其输入的梯度。

So now all we need to do in order to plug in new types of functions into our computational graphs is just have them conform to this little modular gate API that we talked about last time.

因此，现在为了将新型函数接入我们的计算图，我们只需要让它们符合我们上次讨论的这个小型模块化门API。

At the end of last time we ran into a bit of problem. So far in this class we've talked many times about the linear classifier and now we've hopefully gotten some familiarity with these fully connected neural network classifiers.

上次课程结束时我们遇到了一个小问题。到目前为止，在本课程中我们多次讨论了线性分类器，现在希望大家已经对全连接神经网络分类器有了一定了解。

We saw that especially the fully connected neural network classifier was a very very powerful model that could flexibly represent many different functions, but both of these classifiers had a problem so far and that problem is that neither of these two classifiers would respect the 2D spatial structure of our input images.

我们看到特别是全连接神经网络分类器是一个非常强大的模型，能够灵活表示许多不同的函数，但到目前为止这两种分类器都存在一个问题，即它们都不尊重输入图像的二维空间结构。

If you'll recall, both of these classifiers required us to take our input image that has some spatial structure and has some color about some RGB color values at every point in space and now destroy all of that spatial structure by flattening our images this long vector that we could feed into our linear classifiers or our fully connected networks.

回顾一下，这两种分类器都要求我们获取具有空间结构且在空间每个点都有RGB颜色值的输入图像，然后通过将图像展平为长向量来破坏所有空间结构，以便输入到线性分类器或全连接网络中。

This seems like a problem when we're going to work with image data that somehow whenever you build a machine learning model, it's very useful to build models that somehow take advantage of the structure of the input data.

当我们处理图像数据时，这似乎是个问题，因为构建机器学习模型时，利用输入数据的结构来构建模型总是非常有用的。

Now in order to take advantage of this spatial image structure of our input image data, the solution is relatively simple now that we've built up all this machinery around computational graphs and back propagation.

现在为了利用输入图像数据的空间结构，解决方案相对简单，因为我们已经建立了计算图和反向传播的所有机制。

In order to build neural networks that respect the spatial structure of our input data, all we need to do is define a couple of new operators that know how to operate on images or on spatially structured data.

为了构建尊重输入数据空间结构的神经网络，我们只需要定义几个知道如何操作图像或空间结构化数据的新操作符。

So that's what we're going to talk about today is a couple new operators that we can introduce into our neural networks that will operate on this two-dimensional spatial data.

这就是我们今天要讨论的内容：几个可以引入神经网络的新操作符，它们将操作这种二维空间数据。

In particular, so far when we talk about fully connected networks, we're very familiar with these two basic components of fully connected networks. We have the fully connected layer but gives the fully connected network its name, which is a matrix multiply between the input vector that now produces an output vector.

具体来说，到目前为止当我们讨论全连接网络时，我们非常熟悉全连接网络的这两个基本组件。我们有赋予全连接网络名称的全连接层，它是在输入向量之间进行的矩阵乘法，现在产生输出向量。

Recall the other critical component of these fully connected neural networks was this nonlinear activation function. I'm showing the ReLU activation function here on the slide.

回忆一下这些全连接神经网络的另一个关键组件是非线性激活函数。我在幻灯片上展示的是ReLU激活函数。

Now when we move from fully connected neural networks to convolutional neural networks, we need to introduce a couple extra basic operations that we can use inside the computational graphs or models.

现在当我们从全连接神经网络转向卷积神经网络时，我们需要引入几个额外的可以在计算图或模型内部使用的基本操作。

In particular, so then today we'll talk about three of these operations that let us move from fully connected to convolutional networks. In particular, we talked about convolution layers, pooling layers, and normalization layers.

具体来说，今天我们将讨论三种这样的操作，它们让我们能够从全连接网络转向卷积网络。特别地，我们将讨论卷积层、池化层和归一化层。

So first let's see how we can extend the idea of the fully connected layer, which as you'll recall destroyed all of the spatial information and input, and move on to the convolution layer which will serve a similar role in that it will have learnable weights that will now respect the 2D spatial structure of our inputs.

首先让我们看看如何扩展全连接层的概念，正如您记得的，全连接层破坏了所有空间信息和输入，然后转向卷积层，它将发挥类似作用，但具有可学习权重，并且现在会尊重输入的二维空间结构。

If you'll recall to look for the fully connected layer, one way to look at what it's doing is that during the forward pass it receives some vector. If that vector is perhaps a flattened CIFAR-10 image then it would be a vector of 3072 scalar elements being 32 by 32 by 3.

回顾全连接层，理解其工作方式的一种方法是：在前向传播期间它接收某个向量。如果该向量是展平的CIFAR-10图像，那么它将是一个包含3072个标量元素的向量，即32×32×3。

Now during the forward propagation operation of the fully connected layer, we just simply multiply that vector with a weight matrix to produce an output vector.

在全连接层的前向传播操作期间，我们只需将该向量与权重矩阵相乘以产生输出向量。

Now the convolutional layer is still going to have this flavor of operating on an input using a weight matrix in some way and then producing an output of the same general type.

现在卷积层仍将具有这种特性：以某种方式使用权重矩阵对输入进行操作，然后产生相同一般类型的输出。

In particular, the convolution layer now will input a three-dimensional tensor that is a three-dimensional volume that is no longer a flattened vector.

具体来说，卷积层现在将输入一个三维张量，即三维体积，不再是展平的向量。

So for something like CIFAR-10 image, at the very first layer of a convolutional Network that's operating on a CIFAR-10 image, that input volume would now be a three dimensional tensor of 3 by 32 by 32 where that number three refers is called the channel or depth dimension of the input tensor.

对于像CIFAR-10这样的图像，在操作CIFAR-10图像的卷积网络的第一层，输入体积现在将是一个3×32×32的三维张量，其中数字3被称为输入张量的通道或深度维度。

In the case of a CIFAR-10 image it has three channels: the red, blue and green color channels for the raw input image, and then the other two thirty twos are the height and the width of this three dimensional tensor.

对于CIFAR-10图像，它有三个通道：原始输入图像的红、蓝和绿颜色通道，另外两个32是这个三维张量的高度和宽度。

Now just as our input tensor has some three-dimensional spatial structure, with a convolution layer our weight matrix will also have some kind of three-dimensional spatial structure.

现在正如我们的输入张量具有某种三维空间结构一样，对于卷积层，我们的权重矩阵也将具有某种三维空间结构。

In particular, the weight matrix also called sometimes a filter in the terminology of a convolutional layer will be a little three dimensional chunk. In one example it would be that we might have a convolutional filter of size 3 by 5 by 5.

具体来说，在卷积层的术语中，权重矩阵有时也称为滤波器，将是一个小的三维块。在一个例子中，我们可能有一个大小为3×5×5的卷积滤波器。

Here the idea is that we're going to take this little convolutional filter and we're going to slide it over all spatial positions in the input image to compute another three-dimensional output.

这里的想法是，我们将取这个小卷积滤波器，并在输入图像的所有空间位置上滑动它，以计算另一个三维输出。

But here first notice that there's a constraint here between the shape of the input tensor and the shape of one of these convolutional filters. In particular, the depth dimension of the number of depth channels in the input tensor always has to match the number of depth channels in one of our convolutional filters.

但这里首先要注意输入张量的形状与这些卷积滤波器之一的形状之间存在约束。具体来说，输入张量中深度通道的数量必须始终与我们卷积滤波器之一的深度通道数量匹配。

That is to say that this convolution operation always extends over the full depth of the input tensor. We'll see some examples of convolution operations that relax this I think next lecture, but for the purpose of today you should always consider the convolution as extending over the full depth of the input tensor.

也就是说，这种卷积操作总是延伸覆盖输入张量的整个深度。我想在下一讲中我们会看到一些放松此约束的卷积操作示例，但就今天而言，您应该始终将卷积视为延伸覆盖输入张量的整个深度。

Now in order to compute our output, what we're going to do is take that little 5 by 5 by 3 filter and stick it somewhere inside the input image. Now that 3 by 5 by 5 chunk of filter will then align itself to some little 3 by 5 by 5 chunk of that input sensor.

现在为了计算输出，我们将取那个小的5×5×3滤波器并将其放置在输入图像内部的某个位置。现在这个3×5×5的滤波器块将与输入张量的某个小的3×5×5块对齐。

Then once we've aligned the filter to some spatial position in the input tensor, then we can compute a dot product between the filter and the corresponding elements of the input tensor.

一旦我们将滤波器与输入张量中的某个空间位置对齐，我们就可以计算滤波器与输入张量相应元素之间的点积。

This is just a dot product just as we've seen in fully connected networks before, but now rather than taking an inner product between a row of a matrix and the entire vector, now it's an inner product between one filter and a little tiny local spatial chunk of the input tensor. In this example, a three by five by five chunk of the input image would result in a dot product of seventy-five elements. It would also be common to add a bias, as with most machine learning models. It's very common to have a bias whenever you have a weight, but for purposes of clarity on the slides we will often omit biases. You should remember that they're usually there.

这就像我们在全连接网络中见过的点积一样，但现在不是在矩阵的一行与整个向量之间取内积，而是在一个滤波器与输入张量的一小块局部空间区域之间取内积。在这个例子中，输入图像中一个3×5×5的区块会产生75个元素的点积。与大多数机器学习模型一样，通常还会添加偏置项。只要有权重就很可能存在偏置项，但为了幻灯片展示的清晰度，我们经常省略偏置项。您应该记住它们通常存在。

By positioning this filter at one position in the input and computing this inner product, we end up computing a single scalar number. That tells us effectively how much this position in the input tensor matches up with this one filter that we've computed. That will give us one element of the output tensor.

通过在输入中的一个位置放置这个滤波器并计算内积，我们最终会计算出一个标量数值。这有效地告诉我们输入张量中的这个位置与我们计算出的这个滤波器的匹配程度。这将为我们提供输出张量的一个元素。

We will repeat this process and take this input filter and slide it around at every possible position in the input tensor. Each one of those positions will result in a single number giving the dot product between the weight tensor and the local chunk of the input at that position.

我们将重复这个过程，取这个输入滤波器并在输入张量的每个可能位置上滑动它。每个位置都会产生一个数值，表示权重张量与该位置输入局部区块之间的点积。

In this example, if we had a 32 by 32 input and a 5 by 5 filter, it's going to result in a 28 by 28 grid of possible positions at which we could stick that filter. For each of those positions, it results in a single number from that dot product.

在这个例子中，如果我们有一个32×32的输入和一个5×5的滤波器，将会产生一个28×28的可能位置网格，我们可以在这些位置上放置该滤波器。对于每个位置，都会从该点积产生一个数值。

The result of convolving this one convolutional filter or kernel with our input tensor will be an output tensor of shape one by 28 by 28. But of course, it's never enough to just have one convolutional filter.

将这个卷积滤波器或核与我们的输入张量进行卷积的结果将是一个形状为1×28×28的输出张量。但当然，仅仅拥有一个卷积滤波器是远远不够的。

In fact, a convolutional layer will always involve convolving the input image with a set or a bank of different filters with different weight values. We can consider convolving our image with a second convolutional filter shown here in green to represent that it has different values of the weights.

实际上，卷积层总是涉及将输入图像与一组具有不同权重值的不同滤波器进行卷积。我们可以考虑用第二个卷积滤波器（此处用绿色表示以显示其具有不同的权重值）与我们的图像进行卷积。

When we convolve with the second green convolutional filter, we perform the exact same operation. We take this three by five by five convolutional filter and we slide it over all positions in the input and compute an inner product at each position.

当我们使用第二个绿色卷积滤波器进行卷积时，我们执行完全相同的操作。我们取这个3×5×5的卷积滤波器，在输入的所有位置上滑动它，并在每个位置计算内积。

This produces a second one by 28 by 28 output plane, giving all the responses of the input image to that second convolutional filter. These 28 by 28 output planes we sometimes refer to as activation maps of the neural network.

这产生了第二个1×28×28的输出平面，给出了输入图像对第二个卷积滤波器的所有响应。这些28×28的输出平面我们有时称为神经网络的激活图。

These are two-dimensional maps showing how much each position in the input responds to each one of our convolutional filters in the layer. We don't have to stop at two filters; in general, a convolutional layer will involve convolving with some arbitrary number of filters that is a hyperparameter you can set.

这些是二维图，显示了输入中每个位置对层中每个卷积滤波器的响应程度。我们不必停留在两个滤波器；通常，卷积层会涉及与任意数量的滤波器进行卷积，这是一个您可以设置的超参数。

In this example, we're showing convolving our input tensor with six convolutional filters. We have six convolutional filters, each of size three by five by five, with three being the number of input channels from the input tensor and five being the spatial size of the filter.

在这个例子中，我们展示了用六个卷积滤波器与输入张量进行卷积。我们有六个卷积滤波器，每个大小为3×5×5，其中3是输入张量的输入通道数，5是滤波器的空间大小。

We can collect all six of our convolutional filters into a single four-dimensional tensor that now has shape six by three by five by five. This four-dimensional tensor has this particular interpretation as being a set of six three-dimensional filters.

我们可以将所有六个卷积滤波器收集到一个单一的四维张量中，其形状为6×3×5×5。这个四维张量具有特定的解释，即作为一组六个三维滤波器。

When we convolve each of those filters with the input image, we get one activation map for each filter. We can concatenate all of those activation maps into a single three-dimensional tensor which in this example has size six by 28 by 28.

当我们用每个滤波器与输入图像进行卷积时，每个滤波器会得到一个激活图。我们可以将所有激活图连接成一个单一的三维张量，在这个例子中大小为6×28×28。

This looks just like another input image because that convolutional layer has taken a three-dimensional tensor with depth dimension three and height and width 32 by 32, and converted it into another three-dimensional tensor where the spatial structure has been preserved.

这看起来就像另一个输入图像，因为卷积层将一个深度维度为3、高度和宽度为32×32的三维张量转换成了另一个三维张量，其中空间结构得到了保留。

All of the computation inside the computational layer always respects the local structure of the image. Convolutional layers always have a bias, so for completeness we're showing it explicitly.

计算层内部的所有计算始终尊重图像的局部结构。卷积层总是有偏置项，因此为了完整性我们明确显示它。

The bias always has one bias term per convolutional filter. In this example with six convolutional filters, the bias is simply a vector of six elements that gives us a constant offset for each of the feature maps in the output.

每个卷积滤波器总是有一个偏置项。在这个有六个卷积滤波器的例子中，偏置只是一个六元素向量，为输出中的每个特征图提供恒定偏移。

The output is then a 28 by 28 grid. There are two useful equivalent ways to think about the output of a convolutional layer.

输出是一个28×28的网格。有两种有用的等效方式来思考卷积层的输出。

One is this notion of activation maps where we can think of these different 28 by 28 maps or slices of the output, where each activation map represents the degree to which the entire input image had responded to one of those filters.

一种是激活图的概念，我们可以将这些不同的28×28图或输出切片视为每个激活图代表整个输入图像对其中一个滤波器的响应程度。

That's one useful way to think about the spatial structure of the output of a convolutional layer. A second way is that it gives us a 28 by 28 grid which corresponds roughly to the same spatial grid of the input tensor.

这是思考卷积层输出空间结构的一种有用方式。第二种方式是它给我们一个28×28的网格，大致对应于输入张量的相同空间网格。

At each position in that spatial grid, the convolution layer computes a feature vector, in this example a six-dimensional feature vector, which tells us something about the structure or the appearance of that input tensor at each position in the spatial grid.

在该空间网格的每个位置，卷积层计算一个特征向量（在这个例子中是一个六维特征向量），它告诉我们关于输入张量在空间网格中每个位置的结构或外观的信息。

Depending on how you're thinking about convolution in different contexts, it's sometimes useful to think of it either as a collection of feature maps or as a grid of feature vectors. It's useful to have both of those concepts in mind when you think about the output of a convolutional layer.

根据您在不同上下文中思考卷积的方式，有时将其视为特征图的集合或特征向量的网格都很有用。在思考卷积层的输出时，记住这两个概念都很有用。

It's very common when actually performing convolution in practice to perform it on batches of images. Rather than operating on just a single three-dimensional tensor giving us a single input image, it'll be common to operate on a batch of three-dimensional tensors.

在实际执行卷积时，通常对图像批次进行操作。不是仅对单个三维张量（给出单个输入图像）进行操作，而是通常对一批三维张量进行操作。

Given a collection of three-dimensional tensors, we can group them into a single four-dimensional tensor where the batch dimension at the beginning corresponds to independent images that we're processing in this convolution layer.

给定一组三维张量，我们可以将它们分组为一个四维张量，其中开头的批次维度对应于我们在此卷积层中处理的独立图像。

The general form of a convolution layer looks something like this: it will receive a four-dimensional tensor as input with shape N for the batch dimension, C_in for the number of channels, and two spatial dimensions H and W that give the spatial size of each input element.

卷积层的一般形式大致如下：它将接收一个四维张量作为输入，形状为N（批次维度）、C_in（通道数）以及两个空间维度H和W，给出每个输入元素的空间大小。

The output will always have the same batch dimension because this convolution layer processes each element in the batch independently. The output will have a channel dimension C_out which might be different than C_in, and some new spatial extent H' and W' which might be different from the spatial extent of the input image.

输出将始终具有相同的批次维度，因为此卷积层独立处理批次中的每个元素。输出将具有通道维度C_out（可能与C_in不同），以及一些新的空间范围H'和W'（可能与输入图像的空间范围不同）。

This convolutional layer takes as input a three-dimensional or four-dimensional tensor and then produces a four-dimensional tensor as output. We can imagine stacking a whole sequence of these convolutional layers end to end.

这个卷积层将三维或四维张量作为输入，然后产生四维张量作为输出。我们可以想象将一系列这样的卷积层端到端堆叠起来。

By doing so, we build up a neural network whose basic elements are now no longer fully connected layers but are instead convolutional layers.

通过这样做，我们构建了一个神经网络，其基本元素不再是全连接层，而是卷积层。

Here's an example of what that might look like for a little cartoon convolutional network with three convolutional layers. Here we're imagining working on CIFAR, so the input image has three channel dimensions for red, green and blue, and 32 height and width spatial dimensions.

这是一个具有三个卷积层的小型卷积网络的示例。我们假设处理的是CIFAR数据集，因此输入图像具有红、绿、蓝三个通道维度，以及32×32的空间维度。

Then we will operate on it with our first convolutional layer. The weight matrix here is six by three by five by five, which you should interpret as a set of six convolutional filters, each of which has an input channel dimension of three to match the input image, and each of which has a local spatial size of five by five.

然后我们使用第一个卷积层对其进行操作。这里的权重矩阵是6×3×5×5，应理解为六个卷积滤波器，每个滤波器的输入通道维度为3以匹配输入图像，每个滤波器的局部空间尺寸为5×5。

So then we will convolve each of those five by five filters with the input to get our 28 by 28 spatially sized output, and the depth channel of that output will now be 6 for those six filters in that first convolutional layer.

然后我们将这些5×5滤波器与输入进行卷积，得到28×28空间尺寸的输出，该输出的深度通道现在为6，对应第一个卷积层中的六个滤波器。

Then we can repeat the process. Now we've just got another three dimensional tensor we can pass it on to another convolution operation. For the second convolution operation, we can see the weight matrix has shape ten by six by three by three, which again means that we have ten convolutional filters, each of those convolutional filters has a depth dimension of six to match that input tensor, and the spatial size of these filters is now three by three.

然后我们可以重复这个过程。现在我们得到另一个三维张量，可以将其传递给另一个卷积操作。对于第二个卷积操作，我们可以看到权重矩阵的形状为10×6×3×3，这再次意味着我们有十个卷积滤波器，每个滤波器的深度维度为6以匹配输入张量，这些滤波器的空间尺寸现在是3×3。

That would produce another output, and then we keep stacking more and more convolutions on top of each other.

这将产生另一个输出，然后我们继续层层堆叠更多的卷积层。

Just as we use this terminology of hidden layers when we were talking about fully connected networks, we can use the exact same terminology with respect to these convolutional networks. So here this would be a 3-layer convolutional network with our input in red, our first hidden layer in blue, and our second hidden layer in green.

正如我们在讨论全连接网络时使用隐藏层这一术语一样，我们可以对卷积网络使用完全相同的术语。因此，这将是一个3层卷积网络，输入层用红色表示，第一个隐藏层用蓝色表示，第二个隐藏层用绿色表示。

But there's actually a problem with this convolutional network that I've written down on this slide. Can anyone spot what might be a bad thing about this particular design?

但实际上我在幻灯片上写的这个卷积网络存在一个问题。有人能发现这个特定设计的缺陷吗？

What happens if we stack multiple convolution layers directly on top of each other? Well, each convolution operation is itself a linear operator, so when we stack one convolution directly with another convolution, it actually is equivalent to another convolution.

如果我们直接将多个卷积层堆叠在一起会发生什么？每个卷积操作本身都是线性算子，因此当我们直接将一个卷积与另一个卷积堆叠时，实际上等同于另一个卷积。

Just as you might recall from the example of a fully connected network, if we had tried to build a fully connected network by stacking two fully connected layers directly on top of each other, then it had the same representational power as just a single fully connected layer.

正如您可能从全连接网络的例子中回忆起的，如果我们试图通过直接将两个全连接层堆叠在一起来构建全连接网络，那么它具有与单个全连接层相同的表示能力。

The same thing happens with convolution layers because they are also linear operators. So if we stack two convolutional layers directly on top of each other, the result still has the same representational capacity as another convolutional layer, although perhaps with a different filter size or a different number of channel dimensions, but it's still a convolutional layer.

卷积层也会发生同样的情况，因为它们也是线性算子。因此，如果我们直接将两个卷积层堆叠在一起，结果仍然具有与另一个卷积层相同的表示能力，尽管可能具有不同的滤波器大小或不同的通道维度数量，但它仍然是一个卷积层。

To overcome this problem, we use the exact same solution that we saw with the fully connected networks, which is that in between each of our linear convolution operations, we need to insert some kind of nonlinear activation function.

为了克服这个问题，我们使用与全连接网络完全相同的解决方案，即在每个线性卷积操作之间，我们需要插入某种非线性激活函数。

We will very commonly use the ReLU activation function that operates element-wise on each element of this three dimensional tensor, just as we did in the exact same way for our fully connected networks.

我们将非常常用ReLU激活函数，它对这个三维张量的每个元素进行逐元素操作，就像我们对全连接网络所做的那样。

The question was why are there five bias terms for the first convolutional layer, and the answer is because I have a typo on the slide. So thank you for pointing that out. That was supposed to have six bias terms for each of the six filters in that first convolutional layer.

问题是为什么第一个卷积层有五个偏置项，答案是幻灯片上有一个拼写错误。感谢您指出这一点。第一个卷积层中的六个滤波器应该各有六个偏置项。

Hopefully that means that it's been very clear what these layers are supposed to do.

希望这已经清楚地说明了这些层应该做什么。

Another question you might ask is that as you recall from our study of linear classifiers and fully connected neural networks, we always were able to somehow visually inspect the weights that were learned at the first layer of the network.

您可能会问的另一个问题是，正如您从线性分类器和全连接神经网络的研究中回忆起的，我们总是能够以某种方式直观检查网络第一层学习到的权重。

So we might ask the same question for a convolutional network: is there some way that we can visually inspect or visually interpret what the weights at that first layer of the convolutional neural network are?

因此，我们可能会对卷积网络提出同样的问题：是否有某种方法可以直观检查或直观解释卷积神经网络第一层的权重是什么？

The linear classifier had this interpretation of learning a bank of templates, one template per class, and this was expanded with our fully connected neural networks but now learned a set of templates in the first layer which were not tied to any particular class.

线性分类器具有学习一组模板的解释，每个类别一个模板，这在我们全连接神经网络中得到了扩展，但现在在第一层学习了一组不绑定到任何特定类别的模板。

Each of the templates in the fully connected network expanded extended over the full size of the input image, so the fully connected network in the first layer learned this bank of templates each having the same size as the input image.

全连接网络中的每个模板都扩展到输入图像的完整尺寸，因此全连接网络在第一层学习了这组模板，每个模板都具有与输入图像相同的大小。

The convolutional network has a very similar interpretation, except that now rather than learning a set of templates that are the same size as the full input image, instead now it's learning a set of templates that are small and local in size.

卷积网络具有非常相似的解释，不同之处在于现在不是学习与完整输入图像大小相同的一组模板，而是学习一组尺寸较小且局部的模板。

Here I'm showing some learned templates from AlexNet trained on ImageNet. In the first layer of AlexNet, it actually has an 11 by 11 convolution with 64 filters, so we can visualize each of those 64 filters as a little 11 by 11 RGB image.

这里我展示了一些在ImageNet上训练的AlexNet学习到的模板。在AlexNet的第一层，它实际上有一个11×11的卷积，具有64个滤波器，因此我们可以将这64个滤波器中的每一个可视化为一个小的11×11 RGB图像。

Then we can get some sense for what these filters are learning. These filters from AlexNet are very typical of what you tend to expect to learn in the first layer of a convolutional network.

然后我们可以了解这些滤波器在学习什么。来自AlexNet的这些滤波器非常典型，是您期望在卷积网络第一层学习到的内容。

We can see that many of these filters learn something like an oriented edge detector that they learn maybe edges that detect maybe vertical, horizontal edges or vertical edges in different orientations and with different frequencies.

我们可以看到，许多这些滤波器学习类似定向边缘检测器的东西，它们可能学习检测垂直、水平边缘或不同方向和不同频率的垂直边缘。

They look sort of like local edge detectors or local wavelets. Another thing that's very common to see in convolutional network filters is these opposing colors.

它们看起来有点像局部边缘检测器或局部小波。在卷积网络滤波器中另一个非常常见的是这些对立的颜色。

We can see that some of these filters have like a green blob next to a red blob, and that's somehow looking for opposing colors in a particular orientation in the image.

我们可以看到，一些滤波器有绿色斑点旁边是红色斑点，这在一定程度上是在图像中特定方向上寻找对立的颜色。

The interpretation of what is the feature map after we apply this first convolution operation: each of the activation maps in that 3D output tensor gives the degree to which each position in the input image responds to each of these 64 filters.

应用第一个卷积操作后特征图的解释：该3D输出张量中的每个激活图给出了输入图像中每个位置对这些64个滤波器中的每一个的响应程度。

Equivalently, when we had this viewpoint of the output of the convolution as a grid of feature vectors, then that tells us that for AlexNet it gives us a 64-dimensional feature vector at every position in the input image.

等价地，当我们从特征向量网格的角度来看卷积输出时，那么这告诉我们对于AlexNet，它在输入图像的每个位置提供了一个64维特征向量。

The elements of that feature vector correspond to the degree to which the corresponding chunk of the input matches up with each of these templates that's learned in the first layer.

该特征向量的元素对应于输入图像的相应块与第一层学习的这些模板中的每一个的匹配程度。

If you'll recall back to Hubel and Wiesel experimenting on the cat, they found that the cat visual system tended to respond to these local regions of edges, local patterns in the visual field of the cat's eyes.

如果您回想一下Hubel和Wiesel在猫身上进行的实验，他们发现猫的视觉系统倾向于响应这些局部边缘区域，即猫眼视野中的局部模式。

That's kind of a similar effect that's going on with these learned convolutional filters in the first layer of a convolutional network.

这与卷积网络第一层中这些学习到的卷积滤波器所产生的影响有些相似。

Then we can dive in and look a little bit more detail at the exact spatial dimensions of a convolution operation. Here we have an input image that I've kind of transposed and dropped the depth dimension, so now the depth dimension is going into the screen and I'm hiding it because that's not relevant when you're thinking about the spatial dimensions.

然后我们可以深入探讨卷积操作的确切空间维度。这里我有一个输入图像，我已经转置并去掉了深度维度，所以现在深度维度进入屏幕，我隐藏了它，因为当您考虑空间维度时这不相关。

Here we have an input image of spatial size seven by seven and we imagine convolving with a convolutional filter of size three by three.

这里我们有一个空间尺寸为7×7的输入图像，我们想象与一个尺寸为3×3的卷积滤波器进行卷积。

To see what spatial size the output should be, we just need to count the number of spots that we can drop down that fill in this input image. We've got one, two, three, four, five, so that means that there were five positions that we could have dropped a 3x3 filter into a 7x7 image, which means that the spatial size of our output should be five by five.

要了解输出的空间尺寸应该是多少，我们只需要计算可以放入此输入图像的位置数量。我们有1、2、3、4、5个位置，这意味着我们可以在7×7图像中放置3×3滤波器的位置有五个，这意味着我们输出的空间尺寸应该是5×5。

In general, if our input has some size W and the filter has a kernel size of K, then the size of the output will be W minus K plus 1. The idea is that we've been right that the number of positions we can drop the filter is actually less than the number of positions in the input because it bumps up against the edge of the input along the edges and corners of the input image.

一般来说，如果我们的输入具有某个尺寸W且滤波器核大小为K，那么输出尺寸将是W减K加1。这是因为我们能够放置滤波器的位置数量实际上少于输入中的位置数量，因为滤波器会沿着输入图像的边缘和角落碰到边界。

Now this seems like potentially a problem. This means that every time we perform a convolution operation in this way, every convolution operation is going to reduce the spatial dimensions of our input tensor. So then that puts some constraints on the depth of the networks that we might be able to train.

这看起来可能是个问题。这意味着每次我们以这种方式执行卷积操作时，都会减少输入张量的空间维度。这就对我们可能训练的网络深度施加了一些限制。

For example, if we use a three by three convolution, we're going to lose two pixels of resolution every time we do a convolution, which puts an upper bound on the number of layers that we could potentially put in our network because eventually the spatial size of our image would just evaporate away to nothing if we used enough convolutional layers.

例如，如果我们使用3x3卷积，每次卷积都会损失两个像素的分辨率，这就为我们可能在网络中放置的层数设定了上限，因为如果我们使用足够多的卷积层，最终图像的空间尺寸将会完全消失。

And that seems like a problem. We don't want the number of layers in our model to be constrained by this evaporative nature of the convolution operation. So to fix that, we often introduce padding around the borders of the image before we apply the convolution operation.

这似乎是个问题。我们不希望模型中的层数受到卷积操作这种"蒸发"特性的限制。为了解决这个问题，我们通常在应用卷积操作之前在图像边界周围引入填充。

So here's an example where we're applying a padding of one, which means that before we perform the convolution operation, we add an extra layer of pixels around the border of the image and fill them all with zeros. This is called zero padding.

这里有一个应用填充为1的例子，这意味着在执行卷积操作之前，我们在图像边界周围添加一层额外的像素，并用零填充它们。这被称为零填充。

You might imagine there's different strategies you might use for how you might pad out the input. For example, you might think to maybe pull the nearest neighbor value from the border of the image, or you might imagine a circular padding value, or other schemes like that.

你可能会想到填充输入的不同策略。例如，你可能会考虑从图像边界提取最近邻值，或者想象循环填充值，或者其他类似的方案。

But it turns out in practice, the most common thing that we do when training convolutional neural networks for padding is simply to add zeros. It's simple, it's easy, and it seems to work quite well.

但实践证明，在训练卷积神经网络时，最常用的填充方法就是简单地添加零。这种方法简单、容易实现，而且效果相当好。

So now this introduces an additional hyperparameter into a convolution layer. So now when we're building a convolution layer, we need to choose both the filter size and the number of filters in the layer, and also the amount of padding that we're going to apply inside the convolutional layer.

现在这为卷积层引入了一个额外的超参数。因此，在构建卷积层时，我们需要同时选择滤波器大小和层中滤波器数量，以及将在卷积层内部应用的填充量。

And now once we've generalized our convolution layer in this way to accept padding, then the output size now becomes W minus K plus 1 plus 2P, where P is the padding value.

现在，当我们以这种方式推广卷积层以接受填充后，输出尺寸就变成了W减K加1加2P，其中P是填充值。

A very common way to set that hyperparameter P is to set it equal to the kernel size minus 1 over 2. That means that suppose we're doing a 3x3 convolution, then we pad with 1. If we do a 5x5 convolution, we pad with 2 on each side.

设置这个超参数P的一个非常常见的方法是将其设为核大小减1除以2。这意味着如果我们进行3x3卷积，我们就填充1；如果我们进行5x5卷积，我们在每边填充2。

And that is called same padding because it means that when we apply the convolution, the output will now have the exact same spatial size as the input.

这被称为相同填充，因为当我们应用卷积时，输出将具有与输入完全相同的空间尺寸。

So even though padding is an extra hyperparameter technically that you can play around with, the most common thing to do for padding is actually same padding. This just makes it easier to reason about the spatial sizes because it means that the spatial size tends not to change when we perform our convolution operation.

因此，尽管填充在技术上是一个可以调整的额外超参数，但最常用的填充方法实际上是相同填充。这使得空间尺寸的推理更加容易，因为这意味着在执行卷积操作时空间尺寸往往不会改变。

Now another thing, another useful way to think about what the convolution is doing is this notion of a receptive field. So here we're showing an input grid, I'm showing our input image, and then the output grid, the spatial grid of the output after performing a convolution operation.

现在另一个方面，思考卷积作用的另一个有用方式是感受野的概念。这里我们展示了一个输入网格，我展示了我们的输入图像，然后是输出网格，即执行卷积操作后输出的空间网格。

And recall that we had this interpretation of the convolution as taking our convolutional filter matrix and sliding it around and taking inner product, sliding it around every position of the input.

回想一下，我们对卷积的解释是：取我们的卷积滤波器矩阵，在输入图像的每个位置滑动并计算内积。

What this means is that each spatial position in the output image now depends only on a local region of the input image. In particular, for a 3x3 convolution here, one element of that output tensor now only depends on a 3x3 region in the input tensor.

这意味着输出图像中的每个空间位置现在仅依赖于输入图像的一个局部区域。具体来说，对于这里的3x3卷积，输出张量的一个元素现在仅依赖于输入张量中的一个3x3区域。

And this 3x3 region is often called the receptive field of the convolution, or the receptive field of the value in the output tensor.

这个3x3区域通常被称为卷积的感受野，或输出张量中值的感受野。

This is relatively a straightforward and easy thing to think about in the context of one convolution layer, but it's also interesting to think about what happens to these receptive fields as we start stacking convolution layers together.

在单个卷积层的背景下，这是一个相对简单易懂的概念，但当我们开始堆叠卷积层时，思考这些感受野会发生什么变化也很有趣。

So what we can see is that here we're showing a stack of three convolution layers, and now on the very right hand side we see that one element in the very rightmost output tensor depends on a three by three region in the second-to-last activation map.

我们可以看到，这里展示了三个卷积层的堆叠，在最右侧我们看到最右边输出张量中的一个元素依赖于倒数第二个激活映射中的一个3x3区域。

But each of those elements in that second-to-last activation map in turn depends on a three by three region in the second activation map, which in turn depends on a three by three region in the beginning activation map.

但是倒数第二个激活映射中的每个元素又依赖于第二个激活映射中的一个3x3区域，而该区域又依赖于初始激活映射中的一个3x3区域。

So that means that transitively, this green region in the final output actually depends on a fairly large spatial region in the input tensor on the far left.

这意味着传递性地，最终输出中的这个绿色区域实际上依赖于最左侧输入张量中一个相当大的空间区域。

So in this example with three by three convolutions, you can sort of visually see that when we stack two three by three convolutions one after another, then the output of those two three by three convolutions now depends on a five by five spatial region in the original input.

因此，在这个使用3x3卷积的例子中，你可以直观地看到，当我们连续堆叠两个3x3卷积时，这两个3x3卷积的输出现在依赖于原始输入中的一个5x5空间区域。

And if we stack three three by three convolutions, it now depends on a seven by seven region in the input feature map.

如果我们堆叠三个3x3卷积，它现在依赖于输入特征映射中的一个7x7区域。

So then this is another... but then the term receptive field is sometimes overloaded a little bit to mean maybe two different things.

所以这是另一个方面...但感受野这个术语有时会被稍微重载，可能表示两种不同的含义。

So then the receptive field of a neuron in the previous layer would be equal to the kernel size of the convolution, and sometimes you'll also talk about the receptive field of an activation all the way in the original image, which is the spatial size in the input image that has the potential to affect the value of that neuron after however many convolution layers.

因此，前一层中神经元的感受野将等于卷积的核大小，有时你也会讨论一直到原始图像中激活的感受野，这是输入图像中的空间尺寸，有可能在经过若干卷积层后影响该神经元的值。

And what we can see from this diagram is that as we stack convolution layers on top of each other, the effective receptive fields in the input is going to grow linearly with the number of convolution layers that we add.

从这张图中我们可以看到，当我们将卷积层堆叠在一起时，输入中的有效感受野将随着我们添加的卷积层数量线性增长。

But now this is maybe a slight problem because suppose we want to work with very very high resolution images, maybe we will want to work with 1024 by 1024 images.

但现在这可能是一个小问题，因为假设我们想要处理非常高分辨率的图像，比如1024x1024的图像。

Now that means that in order for the values in that output tensor to actually have the ability to see a large region in that high resolution input image, the only way that we can do that is by stacking up a very very very large number of convolution layers.

这意味着为了让输出张量中的值实际上能够看到高分辨率输入图像中的大区域，我们唯一的方法就是堆叠非常非常大量的卷积层。

So then if maybe we have a 1024 by 1024 image, then every 3x3 convolution only adds two pixels to the receptive field, so we would need something like 500 convolutions in order to allow those final output features to depend on the full input image.

因此，如果我们有一个1024x1024的图像，那么每个3x3卷积只会给感受野增加两个像素，所以我们需要大约500个卷积层才能让最终输出特征依赖于整个输入图像。

And having these large receptive fields in the input image maybe seems like a good idea because the neural network needs to be able to get some global context about what is the full image that it's looking at.

在输入图像中拥有这些大感受野似乎是个好主意，因为神经网络需要能够获取关于它正在查看的完整图像的全局上下文。

So then a solution to this problem is to add another hyperparameter to our convolution operation.

因此，解决这个问题的方法是为我们的卷积操作添加另一个超参数。

Yeah, was there... yeah, the question is: it's the zero padding, it doesn't seem like it's adding any information to the network. Well, it's actually not meant to add any information, it's more about notational convenience to prevent the features from shrinking inside the network.

是的，有个问题...关于零填充，它似乎没有为网络添加任何信息。实际上，它本意并不是要添加任何信息，更多的是为了符号上的便利，防止特征在网络内部收缩。

Although there is actually a type of implicit way the zero padding does actually add some information to the network, and that's that it actually breaks translation invariance.

尽管零填充实际上确实以一种隐式的方式为网络添加了一些信息，那就是它实际上打破了平移不变性。

So with a convolution, it should be translation invariant, right? If you imagine shifting the whole image... translation equivariance to be more technical, if we shift the entire image then the output should shift correspondingly.

对于卷积来说，它应该是平移不变的，对吧？如果你想象移动整个图像...更准确地说应该是平移等变性，如果我们移动整个图像，那么输出应该相应地移动。

But once you add a convolution layer, it actually gives the network the ability to kind of count out from the border. You could imagine it learns a convolutional filter that looks for that row of zeros to know where it is in the input image. So actually I think that adding zero padding in this way somehow breaks the translational equivariance of the convolution operation and gives the network some latent or implicit ability to know exactly where it is in the input image. I don't know if that's a bug or a feature, but it's something that the zero padding actually is adding to the representational power of the network.

但当你添加卷积层时，它实际上赋予了网络从边界开始计数的能力。你可以想象它学习了一个卷积滤波器，通过寻找零值行来确定在输入图像中的位置。因此我认为这种零填充方式在某种程度上打破了卷积操作的平移等变性，并赋予网络某种潜在或隐式能力来精确定位其在输入图像中的位置。我不确定这是缺陷还是特性，但零填充确实增强了网络的表示能力。

But then back to this problem we had: in order to achieve these very large receptive fields, we need to stack many many convolution layers. Now we can overcome that by adding another hyperparameter to our convolution called stride. So now we go back to our example of a seven by seven input with a three by three convolutional filter, but now we want it to have a stride of two. That means that rather than placing the convolutional filter at every possible position in the input image, instead we're going to place it every two possible positions in the input image.

但回到我们之前遇到的问题：为了实现这些非常大的感受野，我们需要堆叠许多卷积层。现在我们可以通过为卷积添加另一个称为步长的超参数来解决这个问题。现在我们回到七乘七输入与三乘三卷积滤波器的例子，但这次我们要求步长为二。这意味着我们不会将卷积滤波器放置在输入图像的每个可能位置，而是每两个可能位置放置一次。

So then the output: our first placement is still in the upper left-hand corner, and now we actually skip over one potential position to place the filter because our stride is two, and now we place it again. So then with a stride of two, we can see that there are only three positions in the input where we can place the convolutional filter, which means that when we use a stride of two then our output is now quite a lot spatially downsampled.

因此输出结果如下：我们的首次放置仍然在左上角，现在由于步长为二，我们实际上跳过一个可能的滤波器放置位置，然后再次放置。所以当使用步长为二时，我们可以看到输入中只有三个位置可以放置卷积滤波器，这意味着使用步长为二时，我们的输出在空间上会大幅下采样。

Once we add stride to the network, that means it can build up receptive fields much more quickly because effectively adding a layer with stride of two effectively doubles the receptive field at that layer in the network. So now for a little bit of this more general formulation of how to compute the output shape or size of a convolution: if our input has size W, our filter has size K, our padding is P and we have a stride of S, then we have this expression for computing the size of the output.

当我们为网络添加步长后，意味着它可以更快地构建感受野，因为具有步长为二的层实际上会使该层的感受野翻倍。现在关于计算卷积输出形状或尺寸的更通用公式：如果输入尺寸为W，滤波器尺寸为K，填充为P，步长为S，那么我们可以使用这个表达式来计算输出尺寸。

You might ask here where you can see that the size is dividing by the stride, and you might wonder what happens if that integer is not divisible by the stride. Well that's kind of implementation dependent, but usually you just truncate it or round down or round up. I don't know, it depends on the application, but usually we don't do that and usually we set up our convolutional layers in such a way that the stride always divides that expression that we're dividing W minus K plus two P.

你可能会注意到尺寸需要除以步长，并想知道如果该整数不能被步长整除会发生什么。这实际上取决于具体实现，但通常你会直接截断、向下取整或向上取整。这取决于具体应用，但通常我们不这样做，而是以步长总能整除W减K加二P这个表达式的方式来设置卷积层。

So now here's a little recap of all of that. Here's an example of a convolution that you might use in a CNN network with our input volume of size three channels 32 by 32 spatial size, and now we might imagine a convolutional filter with ten filters of five by five with stride one and pad two.

现在对以上内容做个小结。这是一个你可能在CNN网络中使用的卷积示例：输入体积为三个通道、32乘32空间尺寸，现在我们设想一个具有十个滤波器的卷积层，滤波器尺寸为五乘五，步长为一，填充为二。

So what we've given these settings for the convolutional layer, what should the output size of this tensor be after the convolution? Well here we can apply the formula on the previous slide, and it turns out that in this case the spatial size is the same because we're using stride of one and same padding, and the number of channel dimensions is equal to the number of filters, so now the output size is 10 by 32 by 32.

那么给定这些卷积层设置，卷积后这个张量的输出尺寸应该是多少？这里我们可以应用前一页的公式，结果表明在这种情况下空间尺寸保持不变，因为我们使用步长为一和等宽填充，通道维度数量等于滤波器数量，因此输出尺寸为10乘32乘32。

Now as a recap, what would be the number of learnable parameters in this layer? Well we have our format: we have now ten convolutional filters, each of those filters has size three by five by five, and each of those filters has an associated bias. So that means each of the filters has 76 trainable parameters, since we've got ten filters we've got 760 total parameters.

现在回顾一下，该层可学习参数的数量是多少？根据我们的格式：我们有十个卷积滤波器，每个滤波器的尺寸为三乘五乘五，每个滤波器都有一个关联偏置。这意味着每个滤波器有76个可训练参数，由于我们有十个滤波器，总共有760个参数。

Now another question: how many multiply-adds does this convolution operation take to compute? Well to compute this, you can think that the output tensor has shape 10 by 32 by 32, and in order to compute each of those elements at the output, we need to compute an inner product between two tensors each of shape three by five by five.

另一个问题：这个卷积操作需要多少次乘加计算？要计算这个，你可以认为输出张量的形状是10乘32乘32，为了计算输出中的每个元素，我们需要计算两个形状均为三乘五乘五的张量之间的内积。

So then if you multiply all that together, you can see that this convolution operation takes quite a lot of multiply-add operations in order to compute its output. And also by the way, this is something that sometimes trips people up, but one thing that's actually used sometimes is a one by one convolution where the kernel size is just one by one.

因此如果将所有这些相乘，你会发现这个卷积操作需要相当多的乘加运算来计算其输出。顺便提一下，这有时会让人困惑，但实际中有时会使用一乘一卷积，其核尺寸仅为一乘一。

This seems kind of weird, but it actually makes perfect sense. So for a one by one convolution, we might have in this case a spatial input tensor with 32 channels and 56 by 56 in the spatial dimension, and now a one by one convolution would have a convolutional kernel with 32 filters, each of those filters would be one-by-one in spatial size and extend over the full 32 channels of the input depth.

这看起来有点奇怪，但实际上完全合理。对于一乘一卷积，在这种情况下我们可能有一个空间输入张量，具有32个通道和56乘56的空间维度，现在一乘一卷积将具有32个滤波器的卷积核，每个滤波器在空间尺寸上为一乘一，并覆盖输入深度的全部32个通道。

What this basically means is that we're doing independent dot products. And then remember we had this interpretation of these three dimensional tensors as being a grid of feature vectors. Well when you apply a one-by-one convolution, that basically looks like a linear layer that operates independently on each of the feature vectors in our three dimensional grid.

这基本上意味着我们正在进行独立的点积运算。记得我们曾将这些三维张量解释为特征向量的网格。当你应用一乘一卷积时，它基本上就像一个线性层，独立地作用于我们三维网格中的每个特征向量。

Because of that interpretation, sometimes you might see neural network structures where we have a one by one convolution and then a ReLU and another one by one convolution and another ReLU, some sequence of one by one convolutions and ReLUs, and that's sometimes called a network in network structure because it's effectively a fully connected neural network that operates independently on each of the feature vectors that appear at every position in space.

基于这种解释，有时你可能会看到神经网络结构包含一乘一卷积、ReLU激活、另一个一乘一卷积和另一个ReLU，形成一系列一乘一卷积和ReLU的组合，这有时被称为"网络中的网络"结构，因为它实际上是一个全连接神经网络，独立地作用于空间中每个位置出现的特征向量。

So this seems like kind of a weird thing to do, but actually you'll see it in practice used sometimes. So then the recap of a convolution is that it takes this three-dimensional input, it has hyperparameters: the kernel size, in general you might imagine actually non-square kernels, they do show up sometimes but the overwhelming majority is for kernels to be square, a number of filters, padding and stride.

所以这看起来有点奇怪，但实际上你会在实践中看到它被使用。那么对卷积的总结是：它接收三维输入，具有超参数：核尺寸（通常你可能会想到非方形核，它们确实有时会出现，但绝大多数情况下核是方形的）、滤波器数量、填充和步长。

We have a four-dimensional weight matrix and a single bias factor, and then it produces an output of three-dimensional output according to this particular formula. Then a couple very common settings to see with convolutions, because there are a lot of hyperparameters here, would be to set... it's very common to use square filters, there's some applications where you might see non-square filters.

我们有一个四维权重矩阵和单个偏置因子，然后根据这个特定公式产生三维输出。关于卷积的一些非常常见的设置（因为这里有很多超参数）包括：使用方形滤波器非常常见，在某些应用中你可能会看到非方形滤波器。

It's very common to use same padding so that the output has the same size as the input. And a couple very common patterns in convolution would be: it's very common to use like a three by three stride one convolution or a five by five or one by one stride one convolution. You'll see these types of configurations very commonly.

使用等宽填充以使输出尺寸与输入相同非常常见。卷积中一些非常常见的模式包括：三乘三步长为一的卷积、五乘五或一乘一步长为一的卷积非常常见。你会经常看到这些类型的配置。

It's also very common to see a convolution layer with three by three kernels, padding of one and a stride of two that then is a sort of a spatial down sampling by a factor of two using a convolution layer. So these are all settings that you'll see very commonly in convolution.

同样非常常见的是使用三乘三核、填充为一、步长为二的卷积层，这实际上是通过卷积层实现空间下采样两倍。这些都是你在卷积中会经常看到的设置。

Yeah the question is would it be preferable to use a one by one convolution instead of a fully connected layer? And I think those have slightly different interpretations. A one by one convolution has the interpretation of changing the number of channel dimensions in our three dimensional tensor, whereas a fully connected layer has the interpretation of flattening that whole tensor into one and then producing a vector output.

是的，问题是用一乘一卷积代替全连接层是否更可取？我认为这两者有着略微不同的解释。一乘一卷积的解释是改变我们三维张量中的通道维度数量，而全连接层的解释是将整个张量展平为一个向量，然后产生向量输出。

So that fully connected layer is for cases where you want to destroy the spatial structure in the input, so it would be common to see that at the end of the network when you need to produce category scores, whereas the one-by-one convolutions are very common when you need to have a convolutional 3-dimensional chunk of activation match up with something else that expects a different input number of channels, it's very common to use a one-by-one convolution to adapt or change the number of input channels. They're often used as a kind of adapter inside of a neural network.

因此全连接层适用于你想要破坏输入中空间结构的情况，所以通常在网络末端需要产生类别分数时会看到它，而一乘一卷积在需要让三维卷积激活块与期望不同输入通道数的组件匹配时非常常见，通常使用1x1卷积来调整或改变输入通道数。它们经常被用作神经网络内部的一种适配器。

So far we've talked about two-dimensional convolution because we had the notion of this convolutional kernel. We have three-dimensional input and a convolutional kernel that we moved around at every position in that 2D space.

到目前为止我们讨论的都是二维卷积，因为我们有卷积核的概念。我们拥有三维输入和一个在二维空间每个位置移动的卷积核。

But there are other types of convolutions that you'll see used out there sometimes. You might imagine a one-dimensional convolution where our input would be two-dimensional with a channel dimension and one spatial dimension, and the weight matrix would be three-dimensional.

但还有其他类型的卷积有时也会被使用。你可以想象一维卷积，其输入是二维的，包含通道维度和一个空间维度，而权重矩阵则是三维的。

We would have a bank of filters with C_out filters, and each individual filter would extend the full depth dimension with kernel size K. This has the interpretation of positioning this filter at every position in 1D space and sliding it over the input.

我们会有一组包含C_out个滤波器的滤波器组，每个独立滤波器都会扩展到完整的深度维度，核大小为K。这可以理解为在1D空间的每个位置放置这个滤波器并在输入上滑动。

These 1D convolutions are sometimes used to process textual data that might occur as a sequence, or to process audio data like audio waveforms that you want to process with a convolutional network.

这些1D卷积有时用于处理文本序列数据，或者用于处理音频数据，比如想要用卷积网络处理的音频波形。

We can go the other way and sometimes see three-dimensional convolutions. With 3D convolution, each element of the batch will be a four-dimensional tensor.

我们还可以向另一个方向扩展，有时会看到三维卷积。在3D卷积中，批处理的每个元素都是一个四维张量。

The filter is then a five-dimensional thing where the kernel has three spatial dimensions extending over the full number of feature dimensions of the input vector, and we have a collection of those things so it becomes a five-dimensional weight matrix.

滤波器是一个五维结构，其中核具有三个空间维度，扩展到输入向量的全部特征维度，我们有一组这样的滤波器，因此形成了一个五维权重矩阵。

Each of those three-dimensional filters gets slid at every position in 3D space over that 3D input tensor. These 3D convolutions are sometimes used to process point cloud data or other types of 3D data where data actually lives in some native 3D space.

每个三维滤波器在3D输入张量的3D空间每个位置上滑动。这些3D卷积有时用于处理点云数据或其他类型的3D数据，其中数据实际上存在于某些原生3D空间中。

We've seen that convolution layers come with quite a lot of hyperparameters to set their inputs, outputs, padding, strides and whatnot. In PyTorch, you can have all these settings to change different settings in the convolutional layer, and of course you'll find 1D and 2D convolutions as well.

我们已经看到卷积层带有相当多的超参数来设置其输入、输出、填充、步长等。在PyTorch中，你可以通过所有这些设置来改变卷积层中的不同参数，当然你也会找到1D和2D卷积。

That brings us to our next layer that's a key ingredient of a convolutional network, which is a pooling layer. Pooling layers are a way to downsample inside your neural network in a way that does not involve any learnable parameters.

这引出了我们下一个层，它是卷积网络的关键组成部分——池化层。池化层是在神经网络内部进行下采样的一种方式，不涉及任何可学习参数。

We have already seen that we can spatially downsample our inputs in a convolutional network by using a convolution layer with a stride greater than one. Another way to downsample our spatial dimensions would be to use a pooling layer.

我们已经看到可以通过使用步长大于1的卷积层在卷积网络中空间下采样输入。另一种下采样空间维度的方法是使用池化层。

Here it involves no learnable parameters, we just have hyperparameters which would be the kernel size. The way this pooling layer functions is very similar to a convolutional layer where we operate on local receptive fields in the input tensor.

这里不涉及可学习参数，我们只有超参数，即核大小。池化层的运作方式与卷积层非常相似，我们在输入张量的局部感受野上操作。

Within each of those local regions we will have some pooling function which is some way to collapse those set of input values into one output value. We apply this operation at every slice of our input tensor that results in spatially downsampled output.

在每个局部区域内，我们将使用某种池化函数，将那些输入值集合压缩为一个输出值。我们在输入张量的每个切片上应用此操作，从而产生空间下采样的输出。

One very common way to set up pooling would be an example of two-by-two max pooling with a stride of two. This means we consider carving up our input tensor into spatial regions each of size 2x2, and we stride those spatial regions by 2 pixels each unit.

设置池化的一种非常常见的方法是使用步长为2的2x2最大池化。这意味着我们将输入张量划分为每个大小为2x2的空间区域，并且每个单元移动2个像素来滑动这些空间区域。

When the stride and kernel size are equal, the pooling regions will be non-overlapping. It's very common to use this setting of stride equal to kernel size for pooling.

当步长和核大小相等时，池化区域将不重叠。通常使用步长等于核大小的设置进行池化。

If we have a four-by-four spatial input, it gets carved up into two-by-two regions. Within each of those 2x2 regions we compute a single output number that summarizes the values within that spatial region.

如果我们有一个4x4的空间输入，它会被划分为2x2的区域。在每个2x2区域内，我们计算一个单一的输出数字来汇总该空间区域内的值。

When we use max pooling, we use the max function to compute that one number. Within that 4x4 spatial region we pick the biggest one, and that ends up as the corresponding bin in the output.

当我们使用最大池化时，我们使用最大值函数来计算该数字。在4x4空间区域内，我们选择最大的值，这最终成为输出中对应的单元。

One reason we might prefer pooling over strided convolution is that it doesn't involve any learnable parameters. A second reason is that it introduces some amount of invariance to translation, especially in the case of max pooling.

我们可能更喜欢池化而不是步进卷积的一个原因是它不涉及任何可学习参数。第二个原因是它引入了一定程度的平移不变性，特别是在最大池化的情况下。

Because the max operation selects the largest value within each region, if the input image had moved around a little bit, it's conceivable that the max value might not have changed within the region even if the exact position of something would have changed slightly.

因为最大操作选择每个区域内的最大值，如果输入图像稍有移动，即使图像中某些内容的确切位置发生了轻微变化，区域内的最大值也可能没有改变。

What that means is that when we use max pooling, it introduces some amount of translational invariance to the model, which might be useful for different types of problems.

这意味着当我们使用最大池化时，它给模型引入了一定程度的平移不变性，这可能对不同类型的问题有用。

Pooling has a very similar structure to convolution with stride and kernel size parameters, but rather than taking inner products, we apply some kind of fixed pooling function within each receptive field to compute the output values.

池化具有与卷积非常相似的结构，包含步长和核大小参数，但我们不是进行内积运算，而是在每个感受野内应用某种固定的池化函数来计算输出值。

Now we've got fully connected layers, activation functions, convolutional layers and pooling layers. Given all of these, we can now build a classical convolutional network.

现在我们有了全连接层、激活函数、卷积层和池化层。有了所有这些组件，我们现在可以构建一个经典的卷积网络。

A convolutional network is some kind of neural network that is a composition or combination of all these different operations. You've got a lot of freedom in how you might choose to hook these things up because there are a lot of hyperparameters and different types of layers.

A very classical design that you'll see in a convolutional network is that we'll have some amount of convolutional layers followed by pooling layers, followed by some number of fully connected layers. That's a very classical design that you'll see in convolutional networks.

卷积网络中一个非常经典的设计是：我们会先有一定数量的卷积层，接着是池化层，然后是若干全连接层。这是你在卷积网络中会看到的非常经典的设计。

There's a concrete example of that classical CNN design we can look at: the LeNet-5 network from Yann LeCun that was used for character recognition back in 1998. With the LeNet-5, it takes as input a single grayscale image that is 28 by 28 in spatial size, and because it's grayscale there's only one input channel which is the intensity of each pixel.

这个经典CNN设计的一个具体例子是Yann LeCun在1998年用于字符识别的LeNet-5网络。LeNet-5的输入是一个空间尺寸为28×28的灰度图像，由于是灰度图，只有一个输入通道，即每个像素的强度值。

The first thing is a convolutional layer with 20 convolutional filters each of 5x5 spatial size. Here they're not using same padding, so the output after the convolutional layer is 20 by 28 by 28. After the convolution, we put a ReLU non-linearity right after a convolution layer, which is pretty common.

首先是卷积层，包含20个5×5空间尺寸的卷积滤波器。这里没有使用相同填充，因此卷积层后的输出尺寸为20×28×28。卷积之后，我们立即应用ReLU非线性激活，这在卷积层后很常见。

The next thing would be to apply 2x2 stride 2 max pooling that halves the spatial size of that tensor, moving it from 20 by 28 by 28 down to 20 by 14 by 14. Then we apply another convolutional layer that has 50 filters, so our output after that second convolutional layer and its corresponding ReLU would be 50 depth dimensions and 14 by 14 spatial dimensions.

接下来应用步长为2的2×2最大池化，将张量的空间尺寸减半，从20×28×28降至20×14×14。然后我们应用另一个包含50个滤波器的卷积层，因此第二个卷积层及其对应的ReLU后的输出将是50个深度维度和14×14的空间维度。

Then we have another max pooling that again halves the spatial dimensions, putting us down to 50 by 7 by 7. Before we go into these fully connected layers, we have this flatten operation that takes this three dimensional tensor and flattens it out into a vector, like we've done in fully connected networks.

接着是另一个最大池化，再次将空间维度减半，降至50×7×7。在进入全连接层之前，我们有一个展平操作，将这个三维张量展平成一个向量，就像我们在全连接网络中所做的那样。

This flattens out the 50 by 7 by 7 tensor into a single vector of size 2450. Then we have a fully connected layer with 500 output channels followed by another ReLU that converts us to a vector of size 500.

这将50×7×7的张量展平成一个大小为2450的向量。然后我们有一个500个输出通道的全连接层，接着是另一个ReLU，将我们转换为一个大小为500的向量。

At the end we have another fully connected layer to produce 10 class scores because we want digit classification and recognize digits from zero to nine. One thing you can notice about this classical CNN design is that as we go through the network we tend to have the spatial size decreasing either through pooling layers or through strided convolution layers, and the number of filters is increasing.

最后我们还有另一个全连接层来产生10个类别分数，因为我们想要进行数字分类并识别0到9的数字。关于这个经典CNN设计，你可以注意到的一点是，随着我们通过网络，空间尺寸通过池化层或步进卷积层而减小，而滤波器数量在增加。

This means that as the spatial size decreases the depth increases, so somehow the total volume is always sometimes preserved exactly - we're just squeezing it down this way and stretching it out this way. That's a very common paradigm to see in convolutional networks.

这意味着随着空间尺寸的减小，深度在增加，因此总容量在某种程度上总是被保持 - 我们只是这样压缩它，又这样拉伸它。这是卷积网络中非常常见的模式。

But now there's a problem with this classical design. We've seen that it's very common to stack up convolutional layers, ReLUs, pooling layers, and you can imagine writing down networks that are arbitrarily deep and arbitrarily big. You'll be excited about training deep networks on big data, but you'll run into a problem that if you use this very classical design of a CNN, you'll find that it's very difficult to get networks to converge once they become very deep.

但现在这个经典设计存在一个问题。我们已经看到堆叠卷积层、ReLU、池化层很常见，你可以想象写出任意深度和任意大小的网络。你会对在大数据上训练深度网络感到兴奋，但会遇到一个问题：如果使用这种非常经典的CNN设计，你会发现一旦网络变得非常深，就很难让网络收敛。

To overcome that, a more recent innovation is that we add some kind of normalization layer inside the network that makes it easier for us to train very deep networks. The most common of these is called batch normalization.

为了克服这个问题，一个较新的创新是在网络内部添加某种归一化层，使我们更容易训练非常深的网络。其中最常见的是批量归一化。

The idea is that we want to receive the outputs from some previous layer and we want to normalize those outputs in some way so that they have a zero mean and unit variance distribution. If you read the original paper, they say that it reduces something called internal covariate shift, which is not well understood exactly what that is.

其思想是我们希望接收来自前一层的输出，并以某种方式对这些输出进行归一化，使它们具有零均值和单位方差分布。如果你阅读原始论文，他们会说这减少了所谓的内部协变量偏移，但对此的确切含义理解并不深入。

The rough idea is that when you're training a deep neural network, each layer is looking at the outputs of the previous layer. Because all these different weight matrices are training simultaneously, as the weight matrix from the previous layer changes over the course of optimization, the distribution of outputs that the next layer will see is also going to change over the process of optimization.

大致思想是，当你训练深度神经网络时，每一层都在查看前一层的输出。由于所有这些不同的权重矩阵同时训练，随着前一层权重矩阵在优化过程中发生变化，下一层将看到的输出分布在优化过程中也会发生变化。

Somehow the fact that this second layer sees a changing distribution of inputs over the process of training might be bad for optimization in some way. This is the very coarse non-rigorous idea of what they mean by internal covariate shift.

某种程度上，第二层在训练过程中看到不断变化的输入分布可能对优化不利。这是他们对内部协变量偏移含义的非常粗略、非严格的想法。

To overcome this potential problem of internal covariate shift, the idea is that we want to standardize all of the layers to all fit some target distribution. In particular, we want to force the outputs from every layer to be distributed with zero mean and unit variance.

为了克服内部协变量偏移这个潜在问题，其思想是我们希望标准化所有层，使它们都符合某个目标分布。具体来说，我们希望强制每一层的输出都具有零均值和单位方差的分布。

This means that the next layer consuming those activations is then hopefully seeing inputs that are from a more stationary distribution over the process of training, and this can hopefully stabilize or accelerate the optimization process of these deep networks in some way.

这意味着消耗这些激活的下一层有望在训练过程中看到来自更稳定分布的输入，这有望以某种方式稳定或加速这些深度网络的优化过程。

How exactly can we do this? We know that given a set of samples from some distribution, we can normalize them by subtracting off the mean and dividing by the standard deviation. It turns out that computing and subtracting the mean and dividing by the standard deviation is itself a differentiable function.

我们究竟如何做到这一点？我们知道，给定来自某个分布的一组样本，我们可以通过减去均值并除以标准差来对它们进行归一化。事实证明，计算并减去均值再除以标准差本身就是一个可微函数。

From the idea of computational graphs, when you have a differentiable function you can just slide it as a layer into your neural network. So what we will do with batch normalization is just insert a layer into the network whose purpose is to convert the inputs to have this more standardized distribution.

根据计算图的思想，当你有一个可微函数时，你可以将其作为一层插入到神经网络中。因此，对于批量归一化，我们只需在网络中插入一个层，其目的是将输入转换为具有这种更标准化分布的形式。

More concretely, you can imagine a fully connected version of batch normalization that receives an input of size N for the batch dimension and size D for the vector dimension. It's a batch of N vectors each of size D, and now what we're going to do is along each element of the vector dimension we want to compute an empirical mean over the batch dimension. Then we use the different samples in the batch to compute what is the average value for each slot in that vector. So then we simply compute the empirical mean over the batch dimension to get this vector mu of size D.

更具体地说，你可以想象一个全连接版本的批量归一化，它接收批次维度大小为N、向量维度大小为D的输入。这是一批N个向量，每个大小为D，现在我们要做的是沿着向量维度的每个元素计算批次维度上的经验均值。我们使用批次中的不同样本来计算该向量中每个位置的平均值，然后只需在批次维度上计算经验均值，即可得到这个大小为D的向量μ。

Then given that, we can look and remember the expression for computing the standard deviation of variance. We know that we can compute the standard deviation in this way, which will then give us the standard deviation of variants per channel for each of those D slots and our input, again averaging over the batch dimension.

基于此，我们可以回顾并记住计算方差标准差的表达式。我们知道可以通过这种方式计算标准差，从而得到每个D个位置和输入中每个通道的变体标准差，同样是在批次维度上进行平均。

Finally, we can normalize to give us 0 mean unit variance by subtracting the empirical mean and dividing by the empirical standard deviation. Of course, you'll notice that in the denominator we have this plus epsilon term that's to avoid dividing by 0, and that's a small constant hyperparameter that people don't usually adjust too much.

最后，我们可以通过减去经验均值并除以经验标准差来进行归一化，从而得到零均值单位方差。当然，你会注意到分母中有一个加epsilon项，这是为了避免除以零，它是一个小的常数超参数，通常人们不会过多调整。

Now there's a slight problem: we said we wanted to make our inputs unit mean 0 variance, which might be good for optimization, but that's actually quite a stiff constraint to place on the network to force all these layers to always exactly fit this unit normal distribution.

现在存在一个小问题：我们说过希望输入具有单位均值零方差，这可能对优化有好处，但实际上这对网络施加了相当严格的约束，强制所有层始终完全符合这种单位正态分布。

In practice, it's common to add an additional operation after this normalization where we add learnable scale and shift parameters gamma and beta to the network. Each of these will now be vectors of dimension D that we will take our normalized outputs in the X hat that now have 0 mean unit variance, then we'll add back in this learnable bias and multiply it by this learnable scale parameter.

在实践中，通常在此归一化之后添加一个额外操作，即向网络中添加可学习的缩放和偏移参数γ和β。这些参数现在都是维度为D的向量，我们将对现在具有零均值单位方差的归一化输出X hat应用这些参数，然后重新添加这个可学习的偏置并乘以这个可学习的缩放参数。

These parameters basically allow the network to learn for itself what means and variances it wants to see in each of those elements of the vector. In particular, if the network now has the capacity to learn gamma equals Sigma and beta equals mu, it would require the batch normalization layer to recover the identity function in expectation.

这些参数基本上允许网络自行学习它希望在该向量的每个元素中看到的均值和方差。特别地，如果网络现在有能力学习γ等于Σ和β等于μ，这将要求批归一化层在期望中恢复恒等函数。

But now there's a problem with batch normalization which is the batch part. This mu and Sigma were computed by averaging over the batch dimension of our input tensors, which is something we haven't seen before in any neural network operation discussed thus far.

但现在批归一化存在一个问题，即"批"的部分。这个μ和Σ是通过在输入张量的批次维度上平均计算得到的，这在我们迄今为止讨论的任何神经网络操作中都是前所未见的。

So far, whenever we've had a batch of inputs, all our operations always worked independently on every element in the batch. This meant we could put whatever we wanted into a batch, and having a picture of a cat in the same batch as a picture of a dog would not cause them to have different classification scores.

到目前为止，每当我们有一个输入批次时，所有操作总是独立地对批次中的每个元素进行处理。这意味着我们可以将任何内容放入批次中，并且将猫的图片与狗的图片放在同一批次中不会导致它们具有不同的分类分数。

Now with batch normalization, that's no longer the case. The outputs produced for each element in the batch now depend on every other element in the batch, which is a very bad property to have at test time.

现在有了批归一化，情况不再如此。批次中每个元素的输出现在依赖于批次中的其他所有元素，这在测试时是一个非常糟糕的特性。

Suppose you're running a web service and want to compute what users are uploading at every point in time. It would be really bad if your network's predictions depended on what different users happen to be uploading at the same time.

假设您正在运行一个Web服务，并希望计算用户在每个时间点上载的内容。如果您的网络预测依赖于不同用户恰好在同一时间上载的内容，那将非常糟糕。

In batch normalization, we address this by having the layer operate differently during training and testing. During training, the batch normalization layer takes empirical means and standard deviations over the batch of data it sees.

在批归一化中，我们通过让该层在训练和测试期间以不同方式操作来解决这个问题。在训练期间，批归一化层对其看到的数据批次采用经验均值和标准差。

During testing, it will not compute empirically over the batch. Instead, over the course of training, we keep track of some running exponential mean of all those mu vectors and Sigma vectors seen during training, which become fixed scalars representing the average mu and average Sigma seen over training.

在测试期间，它不会在批次上经验性地计算。相反，在训练过程中，我们会跟踪所有在训练期间看到的μ向量和Σ向量的运行指数均值，这些将成为固定的标量，代表训练过程中看到的平均μ和平均Σ。

Then at test time, rather than using empirical means over the batch, we use those constant mu and Sigma values, which are the average means and standard deviations over the course of training. This allows us to recover independence among elements in the batch at test time.

然后在测试时，我们不使用批次上的经验均值，而是使用那些恒定的μ和Σ值，这些是训练过程中的平均均值和标准差。这使我们能够在测试时恢复批次中元素之间的独立性。

There's actually another nice property of using these running means and variances at test time: if mu and Sigma are actually constants, then the batch normalization operation becomes a linear operation.

实际上，在测试时使用这些运行均值和方差还有另一个很好的特性：如果μ和Σ实际上是常数，那么批归一化操作就变成了线性操作。

If you look at it, we have our X, we're subtracting a constant and dividing by a constant, then for the normalization step and the scale and shift step, we're multiplying by a learned weight and shifting by a learned weight.

如果你仔细观察，我们有我们的X，我们减去一个常数并除以一个常数，然后在归一化步骤以及缩放和偏移步骤中，我们乘以一个学习到的权重并加上一个学习到的权重。

So at test time, the batch normalization operator becomes a linear operator, which means that if we have a convolutional network design with convolution followed by batch normalization, we know that two linear operators can be fused into one linear operator.

因此在测试时，批归一化算子变成了线性算子，这意味着如果我们有一个卷积网络设计，其中卷积后面跟着批归一化，我们知道两个线性算子可以融合成一个线性算子。

What's very common in practice is to perform that fusion at inference time and fuse these running means, running standard deviations, and learned scale and shift parameters into the previous convolution operator in the network.

在实践中非常常见的是在推理时执行这种融合，并将这些运行均值、运行标准差以及学习到的缩放和偏移参数融合到网络中的前一个卷积算子中。

This means batch normalization becomes free at test time with zero computational overhead because we can fuse it into the previous linear operator. This is a very nice property of batch normalization that makes it very practical.

这意味着批归一化在测试时变得免费，具有零计算开销，因为我们可以将其融合到前一个线性算子中。这是批归一化的一个非常好的特性，使其非常实用。

We've seen batch normalization in the context of fully connected networks. It's also very common to use batch normalization in convolutional networks.

我们已经在全连接网络的背景下看到了批归一化。在卷积网络中使用批归一化也非常常见。

In the context of fully connected networks, we had input X of size n by D and were averaging over the batch dimension to produce empirical means of size 1 by D, then applied scale and shift to produce the output.

在全连接网络的背景下，我们有大小为n×D的输入X，并在批次维度上进行平均以产生大小为1×D的经验均值，然后应用缩放和偏移来产生输出。

For convolutional networks, it looks very similar except now rather than averaging only over the batch dimension, we average over the batch dimension as well as both spatial dimensions of the input.

对于卷积网络，它看起来非常相似，只是现在不再仅仅在批次维度上平均，我们还在批次维度以及输入的两个空间维度上进行平均。

This means our mean and standard deviation will be vectors of size C, and our learned scale and shifts will also be vectors of size C. We can compute these outputs using the broadcast functionality you're familiar with in PyTorch.

这意味着我们的均值和标准差将是大小为C的向量，我们学习到的缩放和偏移也将是大小为C的向量。我们可以使用您在PyTorch中熟悉的广播功能来计算这些输出。

It's very common to add batch normalization in your networks directly after a convolutional or fully connected layer. What's very nice empirically about batch normalization is that it makes your network train a lot faster.

在卷积层或全连接层之后直接向网络中添加批归一化是非常常见的。从经验上看，批归一化的一个非常好的特性是它使您的网络训练速度快得多。

Here's a plot from the paper that introduced batch normalization: the black dashed line is the baseline network with only convolution and pooling and no batch normalization, while the red dashed line simply adds batch normalization after all convolution layers with no other changes to the network architecture.

这是引入批归一化的论文中的一张图：黑色虚线是仅包含卷积和池化而没有批归一化的基线网络，而红色虚线仅在所有卷积层之后添加批归一化，对网络架构没有其他更改。

You can see that simply by adding batch normalization to this model, it trains much faster. Another nice property is that empirically, when training networks with batch normalization, you can tend to increase learning rates much higher without diverging during training.

您可以看到，仅仅通过向该模型添加批归一化，它的训练速度就快得多。另一个很好的特性是，从经验上看，当使用批归一化训练网络时，您可以显著提高学习率而不会在训练期间发散。

Then this other blue solid line is the same network with higher learning rates during training and batch normalization. And you can see that by combining batch normalization in the model and higher learning rates, we're able to train this deep network much, much faster. And this is actually a very robust finding that occurs across many, many different convolutional network architectures.

然后这条蓝色实线是相同的网络，在训练期间具有更高的学习率并使用了批归一化。通过结合模型中的批量归一化和更高的学习率，我们能够更快地训练这个深度网络。这实际上是一个非常稳健的发现，出现在许多不同的卷积网络架构中。

But there are some big downsides in batch normalization. One is that it's really not very well understood theoretically. There's this kind of hand-waving around internal covariant shift, but I think there's not really a clear understanding of exactly why it helps optimization in the way that it seems to.

但批量归一化存在一些重大缺点。一是它在理论上并没有得到很好的理解。虽然有人提出了内部协变量偏移的概念，但我认为对于它究竟为何能如此有效地帮助优化，并没有真正清晰的理解。

And another problem with batch normalization is that it actually does something different between training time and test time. And that seems like a problem that actually is a source of bugs in many, many applications.

批量归一化的另一个问题是它在训练时和测试时的行为实际上有所不同。这似乎是一个问题，实际上是许多应用中错误的来源。

Like myself personally on multiple different research projects, I've gotten bitten by this fact that batch normalization does different things during training and test time. Sometimes it's just like a bug in your code and you forget to flip the mode between train and test, and then you're very sad.

就像我个人在多个不同的研究项目中，都曾被批量归一化在训练和测试时行为不同这个问题困扰过。有时这只是代码中的一个错误，你忘记在训练和测试模式之间切换，然后你会非常沮丧。

Or sometimes if your data is somehow imbalanced in a way that it might actually be inappropriate for your model to be forcing this normalization constraint on your data.

或者有时如果你的数据在某种程度上不平衡，可能实际上不适合你的模型对数据施加这种归一化约束。

So for problems like image classification with balanced image classes, maybe this unit variance normalization is appropriate. But for other types of models where you expect very imbalanced inputs or very imbalanced data sets, that can actually be a big problem.

因此对于具有平衡图像类别的图像分类问题，也许这种单位方差归一化是合适的。但对于其他类型的模型，当你期望输入非常不平衡或数据集非常不平衡时，这实际上可能是一个大问题。

And I've been bitten by batch normalization multiple times. But for a class of feed-forward convolutional networks that it was set to work on, it works really, really well.

我已经多次被批量归一化困扰过。但对于它被设计用于处理的前馈卷积网络类别，它的效果确实非常好。

Now one variant on batch normalization that you'll sometimes see is called layer normalization. So we said that one of the problems with batch normalization is that it behaves differently during training time and test time, and that's maybe bad for a lot of reasons.

现在你会看到批量归一化的一个变体叫做层归一化。我们说过批量归一化的一个问题是在训练时和测试时行为不同，这出于很多原因可能都不好。

You want your network to do the same thing at training and testing. During training it was trained to do this thing, and now at test time if you swap out the way one of the layers functions, then the rest of the model wasn't trained to operate productively with that layer in another mode.

你希望你的网络在训练和测试时做同样的事情。在训练时它被训练做这件事，现在在测试时如果你改变某一层的功能方式，那么模型的其余部分就没有被训练来与另一种模式下的该层高效协作。

So in general, we generally prefer layers that operate the same way at training and test time. So one variant of batch normalization that's been proposed that operates the same at training and test time is called layer normalization.

因此总的来说，我们通常更喜欢在训练和测试时以相同方式运行的层。所以有人提出了一个在训练和测试时行为相同的批量归一化变体，称为层归一化。

And here the idea is very similar: we're still going to compute some means and standard deviations and do this empirical normalization. But the difference is that rather than computing the average over the batch dimension, instead we'll compute the average over the feature dimension.

这里的想法非常相似：我们仍然要计算一些均值和标准差，并进行这种经验归一化。但区别在于，我们不是计算批次维度上的平均值，而是计算特征维度上的平均值。

And now this normalization no longer depends on the other elements in the batch, so we can use the same operation at training and test time. And this layer normalization operator is actually used fairly commonly in recurrent neural networks and transformers that we'll talk about in later lectures.

现在这种归一化不再依赖于批次中的其他元素，因此我们可以在训练和测试时使用相同的操作。这种层归一化操作实际上在循环神经网络和Transformer中相当常用，我们将在后面的讲座中讨论这些。

Another kind of equivalent version to layer normalization that you'll see in images is called instance normalization. Here rather than averaging over the batch and spatial dimensions, we average only over the spatial dimensions.

你在图像中会看到的另一种与层归一化等效的版本称为实例归一化。这里我们不是对批次和空间维度求平均，而是仅对空间维度求平均。

And then again, because our means and standard deviations don't depend on the batch dimension, we could do the same thing at training and test time.

同样，因为我们的均值和标准差不依赖于批次维度，我们可以在训练和测试时做同样的事情。

There's this beautiful figure that kind of gives us some intuition about the relationship between these different types of normalization. If we have a tensor with a batch dimension, a channel dimension, and some spatial dimensions, then you can see that batch normalization averages only over the batch and spatial dimensions.

有一个漂亮的图表可以让我们对这些不同类型归一化之间的关系有一些直观理解。如果我们有一个具有批次维度、通道维度和一些空间维度的张量，那么你可以看到批量归一化仅对批次和空间维度求平均。

Layer normalization is averaging over the spatial and channel dimensions, and instance normalization is averaging only over the spatial dimensions.

层归一化是对空间和通道维度求平均，而实例归一化仅对空间维度求平均。

And because there's an empty slot on the slide, you should expect that there's another type of normalization that was proposed in this paper called group normalization a year or so ago.

因为幻灯片上有一个空位，你应该预料到还有一种归一化类型，大约一年前在这篇论文中提出，称为组归一化。

And here the idea is that you split the channel dimension into some number of groups, and you normalize over different subsets of the channel dimension. And that actually tends to work quite well in some applications like object detection.

这里的想法是将通道维度分成若干组，然后对通道维度的不同子集进行归一化。这实际上在某些应用中效果很好，比如目标检测。

So now we've seen these different components of a convolutional network, and you might be wondering: do you have a lot of freedom here in how you can recombine these things?

现在我们已经看到了卷积网络的不同组件，你可能会想：在如何重新组合这些东西方面，你有多大的自由度？

I've kind of given you a set of ingredients that you can use to build neural network models that are aware of the 2D structure of images, but I haven't really told you any best principles about how actually to combine them and go about building neural networks that actually work well.

我已经给了你一组可以用来构建能够感知图像2D结构的神经网络模型的要素，但我还没有真正告诉你任何关于如何实际组合它们并构建真正有效的神经网络的最佳原则。

So that will be the topic of next week's lecture. So come back and actually learn how to build neural networks.

所以这将是下周讲座的主题。请回来真正学习如何构建神经网络。

I have a lot of other stuff here that we're not going to be able to get to. Maybe that'll come some other time. So let's skip to here, and the problem is how do we actually build these things in a way that makes sense?

我这里还有很多其他内容我们无法涉及。也许以后会有机会讨论。所以让我们跳到这里，问题是我们如何以有意义的方式实际构建这些东西？

And then we'll talk about that in next week's lecture.

然后我们将在下周的讲座中讨论这个问题。