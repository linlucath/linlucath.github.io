---
title: 'Lecture5_Notes'
publishDate: 2025-11-25
description: 'TODO'
tags:
  - TODO
language: 'English'
heroImage: { src: './default.jpg', color: '#D58388' }
---

So welcome back to lecture five. Today the microphone is actually working, so hopefully that will give you a little bit better audio this time and I don't have to shout quite as loud.

欢迎回到第五讲。今天麦克风终于正常工作了，希望这次能提供更好的音频效果，我也不用那么大声喊叫了。

Today's topic is neural networks. We're finally getting to the meat of the course and we're finally talking about our first deep learning models.

今天的主题是神经网络。我们终于进入了课程的核心内容，开始讨论我们的第一个深度学习模型。

So far we've talked about using linear models as a way to build parametric classifiers. We've talked about using different kinds of loss functions to quantify how happy or unhappy we are with different settings of the weights in our linear classifiers.

到目前为止，我们讨论了使用线性模型构建参数化分类器的方法。我们讨论了使用不同类型的损失函数来量化对线性分类器中不同权重设置的满意程度。

In the last lecture we talked about using stochastic gradient descent or some of its slightly fancier relatives like momentum and Adam and RMSprop for actually minimizing these objective functions and finding values of the weights that satisfy our preferences as specified by the objective function.

在上一讲中，我们讨论了使用随机梯度下降或其更高级的变体（如动量法、Adam和RMSprop）来实际最小化这些目标函数，并找到满足目标函数所指定偏好的权重值。

Today we're going to iterate on point one and we'll step away from linear models for the first time and start to explore neural network based models that will be much more powerful and allow us to classify images with much higher accuracy.

今天我们将重新讨论第一点，首次离开线性模型，开始探索基于神经网络的模型，这些模型将更加强大，能够以更高的准确度对图像进行分类。

But before we talk about neural network based models, I think we should step back and motivate them a little bit. As we've talked about several times already across the semester so far, we've seen that linear classifiers although they're very simple and easy to understand are really quite limited in the types of functions that they can represent.

但在讨论基于神经网络的模型之前，我认为我们应该退一步，稍微探讨一下它们的动机。正如我们在本学期多次讨论的那样，我们已经看到线性分类器虽然非常简单易懂，但在能够表示的函数类型方面确实相当有限。

Their functional power is somehow not as good as we would like them to be. We saw this from the geometric viewpoint recall that from this geometric viewpoint of a linear classifier, we saw a linear classifier as kind of drawing high dimensional hyperplanes to carve up this high dimensional Euclidean space into two chunks.

With situations like that on the left it's just impossible for a linear classifier to possibly carve up the space in a way that separates the green points from the blue points. When we thought about linear classifiers from the visual viewpoint we had this notion of linear classifiers learning just a single template per class and that therefore they were unable to represent multiple modes of the same object category. For example, we saw that in the horse template it kind of blends the horse looking to the left and the horse looking to the right. And this is somewhat of a representational shortcoming in the linear classification model.

对于左侧所示的情况，线性分类器根本不可能以分离绿色点和蓝色点的方式来划分空间。当我们从视觉角度思考线性分类器时，我们形成了这样的概念：线性分类器每个类别只学习一个模板，因此它们无法表示同一对象类别的多种模式。例如，我们在马匹模板中看到它混合了向左看的马和向右看的马。这在一定程度上是线性分类模型中的表征缺陷。

Well before moving to neural networks, I think we should discuss a different way to overcome this limitation of linear classifiers. And that's the notion of feature transforms. So here the idea with feature transforms is that we will take our original data which is given to us in some native input original space on the left.

在转向神经网络之前，我认为我们应该讨论另一种克服线性分类器局限性的方法。这就是特征变换的概念。特征变换的核心思想是：我们将获取左侧原始输入空间中的初始数据。

And then we will apply some cleverly chosen mathematical transformation to the input data to now transform it in a way that will hopefully be more amenable to classification. So as an example here on the left we maybe with our human intuition we think that maybe transforming this data from cartesian into polar coordinates would be a better representation of this data for the purpose of classification.

然后我们将对输入数据应用经过巧妙选择的数学变换，使其更适用于分类任务。例如在左侧，凭借人类直觉我们可能会认为将数据从笛卡尔坐标系转换到极坐标系能为此数据的分类提供更好的表示方式。

So we can imagine writing down this feature transform which simply converts the cartesian representation of our data into this polar representation of our data. And after we apply this feature transformation to this input data, it now lives in some new space that we call a feature space that is defined by the mathematical form of the feature transform that we've chosen.

我们可以设想写出这个特征变换，它简单地将数据的笛卡尔表示转换为极坐标表示。当我们对输入数据应用此特征变换后，数据就存在于某个新空间中——我们称之为特征空间，该空间由我们选择的特征变换的数学形式定义。

But what's particularly useful about this feature transform for this problem is that now after transforming this input data set from cartesian to polar, we can see that in polar coordinates this data set actually becomes linearly separable. So now we could imagine training a linear classifier not on the original input data space but instead training a linear classifier on the feature space representation of our data.

But this feature transformation is particularly useful for this problem in that: after converting the input dataset from Cartesian coordinates to polar coordinates, we can see that in the polar coordinate system the dataset actually becomes linearly separable. Therefore we can now consider training a linear classifier not in the original input data space, but in the feature space representation of the data.

And then if we imagine porting this linear decision boundary in the feature space back into the original space on the left, we can see that a linear classifier or a linear decision boundary in the feature space corresponds to some kind of non-linear decision boundary or non-linear classifier in the original space. And so by cleverly choosing a feature transform that suits the properties of your data, it may be possible to overcome some of the limitations that we've seen with linear classifiers so far.

但此特征变换对该问题的特别用处在于：将输入数据集从笛卡尔坐标转换到极坐标后，我们可以看到在极坐标系中该数据集实际上变得线性可分。因此现在我们可以设想不在原始输入数据空间，而是在数据的特征空间表示上训练线性分类器。

接着如果我们设想将特征空间中的线性决策边界映射回左侧的原始空间，我们会发现特征空间中的线性分类器或线性决策边界对应着原始空间中的某种非线性决策边界或非线性分类器。因此通过巧妙选择适合数据特性的特征变换，我们或许能够克服目前线性分类器存在的一些局限性。

For this particular example of transforming from cartesian to polar seems kind of trivial, but in general when you think about applying feature transforms more broadly, you have to think very carefully about the structure of the data that you're working with and think about what types of functional transformations you might consider applying to your input data that might make it more amenable to linear classification downstream.

虽然从笛卡尔坐标到极坐标的这个具体变换示例看起来很简单，但一般来说，在更广泛地应用特征变换时，你需要仔细考虑所处理数据的结构，思考应该对输入数据应用哪些类型的函数变换，才能使其更适合后续的线性分类。

This is not just a hypothetical thing. This notion of feature transforms was very broadly used in computer vision and even still is in some sub domains.

这不仅仅是一个假设性的概念。特征变换这一概念在计算机视觉领域曾被广泛应用，甚至在某些子领域中至今仍在沿用。

One example of a feature transform that we might use in computer vision is a notion of a color histogram. So here we can imagine taking each pixel, dividing the color space the RGB spectrum of color space into some number of discrete bins, and then for each pixel in our input image we could assign where it is in the bin representation of our color space.

我们在计算机视觉中可能使用的一个特征变换示例是颜色直方图的概念。我们可以设想将每个像素点的颜色空间RGB光谱划分为若干离散区间，然后为输入图像中的每个像素分配其在颜色空间区间表示中的位置。

Now the feature representation could be some kind of normalized histogram over what colors happen to appear in the image. This color histogram representation of an input image somehow throws away all the spatial information about images and only cares about what types of colors are present in the image.

现在的特征表示可以是图像中出现颜色的某种归一化直方图。这种输入图像的颜色直方图表示在某种程度上丢弃了所有关于图像的空间信息，只关注图像中存在哪些类型的颜色。

So you might imagine that a color histogram representation might for example be more spatially invariant. Suppose that we had maybe a car image like a red car on a brown background and all of our car images were of this nature, but the car might appear in different locations in the image.

因此你可能会认为颜色直方图表示可能具有更好的空间不变性。假设我们有一张红色汽车在棕色背景上的图像，我们所有的汽车图像都具有这种特性，但汽车可能出现在图像中的不同位置。

Then a linear classifier might have a hard time dealing with that kind of representation, but a color histogram representation would always represent it as a bunch of red and a bunch of brown no matter where in exactly in the image the car might be located or the frog in this case.

那么线性分类器可能难以处理这种表示，但颜色直方图表示总是会将其表示为大量红色和大量棕色，无论汽车（或本例中的青蛙）在图像中的确切位置如何。

So this is one fairly simple feature representation that you might imagine applying to input images. Another feature representation in images that was very widely used is the so-called histogram of oriented gradient approach. I don't want to talk about in too much detail. The basic idea is that it's somewhat dual to the color histogram approach. So in the color histogram approach we saw that it threw away all the spatial information and it didn't care about textures or locations.

这是你可以考虑应用于输入图像的一个相当简单的特征表示。图像中另一个曾被广泛使用的特征表示是所谓的定向梯度直方图方法。我不想对此进行过多详细讨论。基本思路是它与颜色直方图方法存在某种对偶关系。在颜色直方图方法中，我们看到它丢弃了所有空间信息，不关心纹理或位置。

It only cared about what colors were present in the image. The histogram of oriented gradients representation does something of the opposite. It throws away all of the color information because it only cares about local edge orientations and strengths.

它只关心图像中存在哪些颜色。而方向梯度直方图表示法则做了相反的事情。它丢弃了所有颜色信息，因为它只关注局部边缘的方向和强度。

Instead it tells us something about the local orientations of edges and the local strengths of edges at every position in the input image. So the histogram of oriented gradients representation could tell us that for example in the red region there is a fairly strong diagonal edges in that region.

相反，它告诉我们输入图像中每个位置的边缘局部方向和边缘局部强度信息。因此方向梯度直方图表示法可以告诉我们，例如在红色区域存在相当强的对角线边缘。

In the blue region around the frog's eyes there are edges in all different kinds of directions. And in the yellow region in the upper right corner of the image there's very little edge information at all because it's a blurry background.

在青蛙眼睛周围的蓝色区域，存在各种不同方向的边缘。而在图像右上角的黄色区域，由于是模糊的背景，几乎没有任何边缘信息。

Very photographic and beautiful so this histogram of oriented gradients representation is somehow dual to the notion of the color histogram representation that we saw before. And this was very widely used in computer vision for tasks like object detection and many other tasks in the mid to late 2000s.

非常具有摄影美感，因此这种方向梯度直方图表示法在某种程度上与我们之前看到的颜色直方图表示法形成对偶。这种方法在2000年代中后期被广泛应用于计算机视觉中的目标检测等任务。

But one interesting feature of both of these feature representations is that they sort of require the practitioner to just think about what is the right qualities of their data that they want to capture with the feature representation. And requires the practitioner to think ahead of time about how to design the right types of feature transforms.

但这两种特征表示法的一个有趣特点是，它们都要求实践者思考希望通过特征表示捕获数据的哪些正确特性。并要求实践者提前考虑如何设计正确类型的特征变换。

Well there are there exists other types of feature transform methods that are somehow data driven that are actually driven by the data that we see in our training set. That drive the the one canonical example of a data-driven feature transform is the so-called bag of visual words representation. Here the idea is that we have some large training set of images so that and then from our training set of images we're going to extract a large set of random patches of various scales and sizes and perhaps aspect ratios cropping out random bits of all of our training images.

实际上还存在其他类型的特征变换方法，这些方法在某种程度上是数据驱动的，实际上是由我们在训练集中看到的数据驱动的。数据驱动特征变换的一个典型例子就是所谓的视觉词袋表示法。这里的思路是我们拥有一个大型图像训练集，然后从这个训练集中，我们将提取大量不同尺度、尺寸和可能纵横比的随机图像块，从所有训练图像中裁剪出随机片段。

We will cluster all of those random patches of our training images to get what is called a code book or a set of visual words that can represent what types of features tend to appear in our images.

我们将对这些训练图像的所有随机块进行聚类，得到所谓的代码本或视觉词汇集，用以表示图像中经常出现的特征类型。

The idea here being that if there are common types of structures in your images that appear in many many images in your training set then you will hopefully learn some kind of visual word representation that can capture or recognize each of the many of those common features that appear in your training set.

这个想法的核心是，如果图像中存在某些常见结构类型，并且在训练集的许多图像中反复出现，那么你就有望学习到某种能够捕捉或识别这些常见特征的视觉词汇表示。

After this step one of building your code book of visual words there is some step two that encodes your image using the learned code book of visual words.

在构建视觉词汇代码本的第一步之后，第二步是使用学习到的视觉词汇代码本对图像进行编码。

Here we will take this code book of visual words that is cluster centers of all the local image patches and compute some kind of histogram representation for each input image to say how much does each of those visual words appear in that input image representation.

这里我们将使用这个视觉词汇代码本（即所有局部图像块的聚类中心），并为每个输入图像计算某种直方图表示，以说明每个视觉词汇在该输入图像表示中出现的频率。

This you can imagine is quite a powerful type of feature representation because it does not require the practitioner to fully specify the functional form of the feature representation.

你可以想象这是一种相当强大的特征表示方法，因为它不需要实践者完全指定特征表示的函数形式。

It instead somehow allows the features to be that are used in the feature representation the visual code book words in this case somehow they're able to be learned from the training data to better fit the problem at hand.

相反，它允许特征表示中使用的特征（在此情况下是视觉代码本词汇）能够从训练数据中学习，从而更好地适应当前问题。

This is maybe a bit more flexible than some of the other feature representations that we saw previously but another common trick with image features is that you don't have to settle for one.

这可能比我们之前看到的一些其他特征表示方法更加灵活，但图像特征的另一个常见技巧是你不必局限于单一特征表示。

You can imagine having a bunch of different feature representations of your input image and then concatenating them all together into one long feature vector.

你可以想象拥有输入图像的多种不同特征表示，然后将它们全部连接成一个长特征向量。

You might concatenate a color histogram representation at the top with some bag of words representation in the middle with some histogram of oriented gradients representation at the bottom to get some kind of long long high dimensional feature vector that describes your image in various that were different parts of the feature representation.

You can connect color histogram representations at the top, bag-of-words representations in the middle, and histogram of oriented gradients representations at the bottom to obtain a long, high-dimensional feature vector that describes your image from multiple aspects through different parts of the feature representation. Now capture maybe different types of information from the input image like color or edges or whatnot, and this idea of combining multiple types of feature representations was very widely used in computer vision in the late 2000s and early 2010s. As a canonical example of that, the winner of the 2011 ImageNet challenge used some kind of complicated feature representation.

你可以在顶部连接颜色直方图表示，中间连接词袋表示，底部连接方向梯度直方图表示，从而得到一个长长的、高维的特征向量，通过特征表示的不同部分从多个方面描述你的图像。现在可能从输入图像中捕获不同类型的信息，比如颜色或边缘等等，而这种结合多种特征表示类型的想法在2000年代末和2010年代初被广泛应用于计算机视觉领域。一个典型例子是2011年ImageNet挑战赛的获胜者使用了某种复杂的特征表示。

This was the state of the art in large scale image classification as of 2011, which was of course the year right before the AlexNet architecture made deep learning very popular across all parts of computer vision. But if we look back at this 2011 ImageNet winner, we can see that they first took some low-level feature extraction where they extracted a bunch of patches from the images.

这是2011年大规模图像分类的最先进技术，当然这一年正好是在AlexNet架构使深度学习在计算机视觉各个领域变得非常流行之前。但回顾这个2011年ImageNet获胜者，我们可以看到他们首先进行了一些低级特征提取，从图像中提取了许多图像块。

They extracted SIFT and color representation, reduced the dimensionality of those using PCA, and applied something called Fischer vector encoding to compress and get another layer of features on top of those original features. They did some compression step and after that they ended up with some kind of feature representation upon which they could learn linear SVMs to finally classify.

他们提取了SIFT和颜色表示，使用PCA降低了这些特征的维度，并应用了称为Fisher向量编码的方法来压缩并在原始特征之上获得另一层特征。他们进行了一些压缩步骤，之后得到某种特征表示，在此基础上他们可以学习线性支持向量机来进行最终分类。

So this pipeline of coming up with feature representations for images can perform reasonably well in some contexts. But one way I like to think about feature extraction is something like the following diagram here on the top.

因此这种为图像创建特征表示的流程在某些情况下可以表现得相当好。但我喜欢将特征提取理解为类似顶部图示的方式。

Here we imagine if we have some kind of machine learning image classification pipeline that's built with feature extraction. Basically what we've done is we've decomposed our system into two parts: one is the feature extractor, and the second is the learnable model that will actually operate on top of that feature representation or that feature space.

Here we imagine if we have some kind of machine learning image classification pipeline built on feature extraction. Basically what we do is break the system into two parts: one is the feature extractor, and the second is the learnable model that actually operates on top of that feature representation or feature space.

Typically this feature extraction stage is not necessarily going to automatically tune itself in order to directly maximize the classification performance of the overall system. Instead, when we learn a linear classifier on top of a fixed feature representation, we end up with this very large complicated system that goes from raw image pixels all the way to our final classification scores. But only a small part of that system at the very end is actually tuning itself or tuning its weights in response to trying to maximize the classification accuracy of our system.

这里我们想象如果我们有某种基于特征提取构建的机器学习图像分类流程。基本上我们所做的是将系统分解为两个部分：一个是特征提取器，第二个是在该特征表示或特征空间之上实际运行的可学习模型。

通常这个特征提取阶段不一定会自动调整自身以直接最大化整个系统的分类性能。相反，当我们在固定特征表示之上学习线性分类器时，我们最终会得到这个非常庞大复杂的系统，从原始图像像素一直延伸到我们的最终分类分数。但只有该系统最末端的一小部分实际上在进行自我调整或调整其权重，以响应我们系统分类准确率最大化的目标。

In contrast, well clearly here we might want to do something better which is to somehow automatically tune all parts of the system to maximize the performance on image classification tasks.

相比之下，我们显然希望采取更好的方案——通过某种方式自动调整系统的所有组成部分，以最大化图像分类任务的性能。

And that's one motivation to think about what a neural network is doing when we build a deep neural network system for image classification.

这正是思考神经网络工作原理的一个动机——当我们构建用于图像分类的深度神经网络系统时。

What we're doing is building a single end-to-end pipeline that on the left-hand side takes in the raw pixels of the image and on the right hand side is predicting these classification scores or classification probabilities.

我们构建的是一个端到端的流程：左侧接收图像的原始像素，右侧输出分类得分或分类概率。

Then during the process of training we will tune not only that final layer that's using some linear classifier, we will tune the entire system all parts of the system jointly in order to maximize the performance of classification or whatever other end task we're considering.

在训练过程中，我们不仅会调整使用线性分类器的最终层，还将联合调整整个系统的所有组成部分，以最大化分类性能或我们考虑的任何其他最终任务。

And then from this point of view it seems that a neural network based system is really not so different from these large deep feature representation systems.

从这个角度来看，基于神经网络的系统与这些大型深度特征表示系统并没有本质区别。

Basically the change is that a neural network is somehow jointly learning both a feature representation and a linear classifier on top of that feature representation in such a way to maximize the classification performance of our system.

本质上改变在于：神经网络通过某种方式同时学习特征表示和基于该特征表示的线性分类器，从而最大化我们系统的分类性能。

With this introduction we can finally talk about a concrete example of what exactly a neural network might look like.

通过以上介绍，我们终于可以讨论神经网络具体示例的样貌。

So far in this class of course we're very familiar with this linear classifier right given our input data which is stored in a giant column vector x.

在本课程中，我们当然非常熟悉线性分类器——给定存储在巨型列向量x中的输入数据。

Then our linear classifier is going to have a learnable weight matrix w of size number of input dimensions by number of categories and the linear classifier is this matrix vector multiply between the input data and the learnable weight matrix.

我们的线性分类器将具有一个可学习的权重矩阵w，其尺寸为输入维度数乘以类别数，而线性分类器就是输入数据与可学习权重矩阵之间的矩阵向量乘法。

Now we've generalized this system. Rather than have we still represent our input data with a single long column vector, where we stretched out the raw pixel values of all the parts of the image into one big vector.

现在我们已经将这个系统进行了泛化。我们仍然使用一个长的列向量来表示输入数据，将图像所有部分的原始像素值展开成一个大向量。

But now we have two learnable weight matrices. One of these learnable weight matrices is W1, which is now shaped number of input dimensions by H. H is called the hidden size of the neural network.

但现在我们有两个可学习的权重矩阵。其中一个可学习的权重矩阵是W1，其形状为输入维度数乘以H。H被称为神经网络的隐藏层大小。

We first do a matrix vector multiply between our input data X and our first learnable weight matrix W to produce some new vector of dimension H. Then we apply this element-wise maximum function upon this vector of size H.

我们首先在输入数据X和第一个可学习权重矩阵W之间进行矩阵向量乘法，生成一个维度为H的新向量。然后我们在这个大小为H的向量上应用逐元素最大值函数。

Finally we perform a second matrix vector multiply between this H dimensional vector and our second learnable weight matrix W2, which is of size H by C where C is the number of output channels.

最后我们在这个H维向量和第二个可学习权重矩阵W2之间执行第二次矩阵向量乘法，W2的大小为H乘以C，其中C是输出通道的数量。

In practice each of these matrix vector multiplies will typically also have an associated bias term that will also add a learnable bias whenever we do a matrix vector multiply. But writing down the bias clutters the notation quite a lot so in practice you'll see people often omit bias terms and equations when they actually do indeed use them when training their system.

在实践中，每次矩阵向量乘法通常还会有一个相关的偏置项，在进行矩阵向量乘法时会添加一个可学习的偏置。但写下偏置会使符号表示变得相当混乱，因此在实际中你会看到人们经常在方程中省略偏置项，尽管他们在训练系统时确实使用了偏置。

I've wanted to indoctrinate you to this confusing bit of notation even from our first slide on neural networks. We can generalize this notion of neural networks beyond two weight matrices.

我想从一开始介绍神经网络时就让你熟悉这个令人困惑的符号表示。我们可以将神经网络的概念推广到两个权重矩阵之外。

We can generalize to any number of weight matrices where at each stage what we're going to do is take our current input vector, apply a matrix multiply and add a hidden bias, apply this element-wise maximum function, and then repeat until we've applied all of our learnable weight matrices in the system.

We'll often see this type of system drawn pictorially with a diagram kind of like the following where on the left we have a map. So here we imagine data flowing through this neural network based system from left to right, and on the left is our input data which is this column vector containing all of our pixels of our input image X in the middle we have.

我们可以推广到任意数量的权重矩阵，在每个阶段我们要做的是获取当前输入向量，应用矩阵乘法并添加隐藏偏置，应用这个逐元素最大值函数，然后重复这个过程，直到我们应用了系统中所有可学习的权重矩阵。

我们经常会看到这类系统用类似下面的示意图来表示，左边有一个地图。这里我们想象数据从左到右流经这个基于神经网络的系统，左边是我们的输入数据，即包含输入图像X所有像素的列向量，在中间我们有。

In the middle we have this hidden vector h which is maybe of size 100 elements in this hidden vector in this example, and then on the right are our final score vectors giving 10 classification scores for 10 categories here.

中间是这个隐藏向量h，在这个例子中可能包含100个元素，右边则是我们的最终得分向量，这里给出10个类别的10个分类得分。

We imagine these weight matrices as living in between each of these multiple layers of our neural network, where we can interpret these weight matrices as somehow telling us how much each element of the previous layer influences each element of the next layer.

我们将这些权重矩阵视为存在于神经网络各层之间，可以将其解释为能够告诉我们前一层每个元素对下一层每个元素的影响程度。

For example, if we look at element i comma j of the first learnable weight matrix w1, then this is a scalar value of the weight matrix that tells us how much is the element h i in the hidden layer influenced by the input element xj in the input.

例如，如果我们查看第一个可学习权重矩阵w1的元素i,j，那么这个权重矩阵的标量值告诉我们隐藏层中的元素hi受输入中元素xj影响的程度。

You can look at a similar representation, a similar interpretation of the second weight matrix w2, showing how much does each element of the hidden vector affect each element of the output scores.

你可以对第二个权重矩阵w2采用类似的表示和解释，显示隐藏向量的每个元素对输出得分中每个元素的影响程度。

Because these are dense general matrices, we can recognize that in this particular structure here we see that each element of the input x affects each element of the hidden dimension of hidden vector h, and similarly each element of the hidden dimension vector h affects each and every element of our final score vector s.

由于这些是密集的通用矩阵，我们可以认识到在这种特定结构中，输入x的每个元素都会影响隐藏向量h的隐藏维度中的每个元素，同样地，隐藏维度向量h的每个元素也会影响最终得分向量s的每个元素。

Because of this dense connectivity pattern, this type of neural network is typically called a fully connected network because the units in each layer of the network are all connected to one another, so it's fully connected.

由于这种密集的连接模式，这类神经网络通常被称为全连接网络，因为网络中每一层的单元都相互连接，所以它是全连接的。

This type of structure is also sometimes called a multi-layer perceptron or MLP. This is in reference way back to the perceptron learning algorithm that we talked about in the first lecture.

这种结构有时也被称为多层感知机或MLP。这是参考我们在第一讲中讨论过的感知机学习算法。

I think it's maybe a strange bit of terminology, but you'll definitely see people refer to neural networks of this type as MLPs, so you should be aware of that bit of notation.

我认为这可能有点奇怪的术语，但你肯定会看到人们将这类神经网络称为MLP，所以你应该了解这个符号表示。

Now we can think we can add a little bit of extra interpretation to what exactly these neural networks are computing. So if you'll recall in the linear classification case, oh sorry was there a question that's a great question uh the question you asked what was the purpose of the max I'll get back to that in about four slides so hold on and then ask again.

现在我们可以思考为这些神经网络具体在计算什么添加一些额外的解释。如果您还记得线性分类器的情况，哦抱歉刚才是有问题吗？这个问题问得很好，您问最大值函数的目的是什么，我将在约四张幻灯片后回答这个问题，请稍等然后再次提问。

So one way that we can think about this neural network based classifier is in contrast to a linear classifier.

因此，我们可以将这种基于神经网络的分类器与线性分类器进行对比思考。

So if we'll as we'll recall the linear classifier we interpreted as learning this set of one templates per class and then the scores were then the inner product between each of our large templates and the input image that somehow said how much does each of those templates each how much does our input image match each of the templates.

如果我们回顾线性分类器，我们将其解释为学习每个类别的一组模板，然后得分就是每个大型模板与输入图像之间的内积，这某种程度上表明了输入图像与每个模板的匹配程度。

Well now we can interpret the weight matrix the first weight matrix of the neural network in a very similar way where somehow now the first layer of the neural network the weight matrix in that first layer w1 also learns a set of templates and one way that we can interpret the values in the hidden layer is is as how much does each how much does each of our learned templates respond to the input image x.

现在我们可以用非常类似的方式解释神经网络的第一个权重矩阵，即神经网络的第一层中的权重矩阵w1也学习了一组模板，我们可以将隐藏层中的值解释为每个学习到的模板对输入图像x的响应程度。

And here we can see that most of these images are not most of these templates are not very interpretable you can't really always tell what's going on with these templates but there is definitely some kind of discernible spatial structure that these temp that these first layer features or first layer templates have learned in this two-layer neural network system.

在这里我们可以看到，大多数这些图像（模板）并不具有很强的可解释性，你并不总能明确理解这些模板的含义，但这些第一层特征或第一层模板在这个两层神经网络系统中确实学习到了某种可辨别的空间结构。

But sometimes you get lucky and sometimes you do get some beautiful layers in this first layer so i don't know if you noticed but uh maybe i've looked at these too long but for these two examples here they actually look to me like one is a horse facing one way and the other is a horse facing the other way.

但有时你会很幸运，在第一层中确实会出现一些漂亮的层，我不知道你们是否注意到了，也许我看这些太久了，但这里的这两个例子在我看来实际上像是一匹马朝一个方向，另一匹马朝另一个方向。

So now this this is like finally overcoming this two-headed force problem that's been plaguing us for the past couple weeks as we with linear classifiers and then of course the second layer of the neural network is then somehow predicting its classification scores by recombining the by having another weighted recombination of the responses of the input image these templates.

So what that means is that the neural network could then hopefully somehow finally recognize multiple types or multiple subsets of a category where it could recognize both the left-facing horse using one template and the right-facing horse using another template and then use the weights in the second layer to recombine the information from both of those two templates.

这意味着神经网络有望最终识别一个类别中的多种类型或多个子集，它可以使用一个模板识别朝左的马，使用另一个模板识别朝右的马，然后使用第二层的权重来重新组合这两个模板的信息。

But this is sometimes called a distributed representation because, really, this two facing horses example is really quite rare.

但这有时被称为分布式表示，因为实际上这个两匹相向马匹的例子确实相当罕见。

The more common case is that most of the time the features or the image templates that we learn in the first layer of the neural network are not very human interpretable.

更常见的情况是，大多数时候我们在神经网络第一层学习到的特征或图像模板并不太容易被人类理解。

Instead they have maybe some kind of spatial structure, and then there's this notion of the neural network using a so-called distributed representation to represent the images.

相反，它们可能具有某种空间结构，然后存在神经网络使用所谓的分布式表示来表示图像的概念。

Where somehow by having some kind of linear combination of each of these templates, the network represents something about the image but we can't, it's not super interpretable to us what exactly those different templates are trying to capture.

通过以某种方式对这些模板进行线性组合，网络表示了图像的某些信息，但我们无法非常清楚地解释这些不同模板究竟试图捕捉什么。

Oh yeah, was there a question? So the question is there seems to be some repeated structures in these templates.

哦对了，刚才是有问题吗？问题是这些模板中似乎存在一些重复的结构。

A lot of them have this structure of like a red blob and then a blue blob, I don't know, that's what I decided to learn.

很多模板都有这种结构，比如一个红色斑点然后一个蓝色斑点，我不知道，这就是它决定学习的内容。

That's kind of the mystery and the magic of neural networks, you don't always know what exactly types of features they're going to learn but they're going to learn something that's going to maximize the classification accuracy.

这有点像是神经网络的神秘和魔力所在，你并不总是知道它们会学习什么类型的特征，但它们会学习一些能够最大化分类准确率的东西。

My intuition here is that those are representing some kind of car because I think there's a lot of cars in the CIFAR-10 dataset.

我的直觉是这些特征代表了某种汽车，因为我认为CIFAR-10数据集中有很多汽车图像。

Another really common pattern that you'll often see in the learned first layer features of neural networks is that they often represent oriented edges, just as we saw in the Hubel and Wiesel example.

另一个在神经网络学习到的第一层特征中经常看到的常见模式是，它们通常表示定向边缘，就像我们在休伯尔和威泽尔的例子中看到的那样。

When they were investigating the human or mammalian visual system by presenting different visual stimuli and see how human neurons or cat neurons activate, then we know that many of our own neurons in our visual system end up being sensitive to oriented edges.

当他们通过呈现不同的视觉刺激来研究人类或哺乳动物的视觉系统，并观察人类神经元或猫神经元如何激活时，我们了解到我们视觉系统中的许多神经元最终对定向边缘敏感。

And with these neural network systems, you often get a similar type, you tend to learn similar types of features in many cases where you learn either oriented edges or opposing colors.

So in this case the blue and the red is maybe some kind of opposing color scheme and you can imagine that by recombining many of these features, it can represent something about the structure of the image.

所以在这种情况下，蓝色和红色可能是某种对比色方案，你可以想象，通过重新组合这些特征中的许多元素，它能够表示图像的某些结构信息。

Yeah, so the question is: is there some risk of redundancy that maybe we learned multiple filters to represent the same thing? And that's definitely a possibility.

确实，问题在于是否存在冗余风险——我们可能学习了多个滤波器来表示相同特征？这绝对是一种可能性。

But there exists some networks, some techniques actually for network pruning that I think we'll talk about later in the semester, whereby you can first train a neural network that's maybe big and represents a lot of stuff, and then as a second post-processing step you can try to prune out the redundant representations that have been learned by the network.

但存在一些网络架构和剪枝技术——我想我们会在学期后期讨论——你可以先训练一个可能较大、能表示很多特征的神经网络，然后作为后处理步骤，尝试剪枝掉网络学习到的冗余表示。

So that's something that people sometimes do in practice, but we're not going to cover in this lecture.

这是实践中人们有时会采用的方法，但本次课程不会涉及。

Now with this notion of neural networks, you can definitely imagine generalizing this to many, many layers as we've already seen.

基于神经网络的这一概念，你完全可以想象将其推广到非常多层的结构，正如我们已经看到的。

And for a bit of notation here, the depth of the neural network is usually the number of layers that it contains, and when we count layers we usually count the number of weight matrices.

关于符号表示：神经网络的深度通常指其包含的层数，而我们计算层数时通常统计的是权重矩阵的数量。

So our two layer network would have two learnable weight matrices. This would be a six-layer network because it has six learnable weight matrices.

因此我们的两层网络将有两个可学习的权重矩阵。而具有六个可学习权重矩阵的则称为六层网络。

Then the width of the network would be the number of units or the dimension of each of those hidden representations.

网络的宽度则是指单元数量或每个隐藏表示的维度。

In practice, you could imagine that each layer might in principle have a different feature dimension at each of those hidden layers, but in practice what's more common is to set the same width throughout every part of the network just as a bit of convention.

实践中，原则上每个隐藏层可以具有不同的特征维度，但更常见的做法是遵循惯例，在整个网络的各个部分设置相同的宽度。

Then we had this very astute question a couple minutes ago about like what is this funny max doing hanging out in this neural network equation?

几分钟前我们遇到了一个非常敏锐的问题：这个奇怪的max函数在神经网络方程中扮演什么角色？

Well this turns out to be quite an important feature or quite an important component of the neural network.

事实证明这是神经网络中一个非常重要的特性或组件。

This function max of zero and the input takes an element-wise maximum between zero and the input, which means that we input a vector and then anything negative we throw away and set it to zero instead.

This function seems simple, but it's so important and so widely used that it's given the special name ReLU for a rectified linear unit. We have a plot of this beautiful function here on the left. You can see that if the argument is positive, there's nothing. If the argument is negative, we return 0 instead. This max(0, input) function will take the maximum between zero and input element-wise, which means that after we input a vector, all negative values will be discarded and set to zero.

这个函数看似简单，但它非常重要且应用广泛，因此被赋予了一个特殊名称——ReLU（修正线性单元）。我们在左侧绘制了这个优美函数的图像。可以看到，如果参数为正数，函数值不变；如果参数为负数，则返回0。这个max(0, input)函数会逐元素取零和输入之间的最大值，这意味着我们输入一个向量后，所有负值都会被丢弃并设为零。

This function we imagine as being sandwiched in between our two learnable weight matrices in the neural network. This function is called the activation function of the neural network and it's actually completely critical to the functioning of the neural network.

我们将这个函数想象成夹在神经网络中两个可学习权重矩阵之间的部分。这个函数被称为神经网络的激活函数，它对神经网络的运行实际上至关重要。

As a thought exercise, you should think to yourself what if we built a neural network system with the following structure that had no activation function in between the two weight matrices. What if we took our input vector multiplied by our first weight matrix W1 and then multiplied by our second weight matrix W2? What would be wrong with this type of neural network-based approach?

作为思考练习，你应该设想一下：如果我们构建一个在两层权重矩阵之间没有激活函数的神经网络系统会怎样？如果我们让输入向量先乘以第一个权重矩阵W1，再乘以第二个权重矩阵W2，这种基于神经网络的方法会存在什么问题？

Yeah exactly, it's still a linear classifier because we know that matrix multiplication is associative. You could imagine grouping those two matrix multiplies together and this just devolves back into a linear classifier. So without some kind of non-linear function between our two matrix multiplies, we have no additional representational power beyond a linear classifier.

没错，这仍然是一个线性分类器，因为我们知道矩阵乘法具有结合律。你可以将这两个矩阵乘法组合在一起，这样就会退化为线性分类器。因此，如果在两个矩阵乘法之间没有某种非线性函数，我们就无法获得超越线性分类器的表达能力。

So the choice of activation function and the presence of an activation function is absolutely critical to the functioning of the neural network. I could point out that sometimes these networks with no activation function are called deep linear networks and they're sometimes studied in the optimization community.

因此，激活函数的选择及其存在对神经网络的运行至关重要。需要指出的是，有时这些没有激活函数的网络被称为深度线性网络，优化领域的研究人员有时会研究它们。

Even if their representational power is the same as a linear classifier, the optimization dynamics are actually much more complex than that of a linear classifier. So sometimes people do actually study these in the theoretical context in the context of optimization.

尽管它们的表达能力与线性分类器相同，但优化动态实际上比线性分类器复杂得多。因此，在优化理论背景下，人们确实会研究这些网络。

In this first example of a neural network, we've chosen this activation function to be this ReLU function. But there's a whole menagerie of different activation functions that people out there work with.

在我们这个神经网络的第一个示例中，我们选择ReLU作为激活函数。但实际上研究人员使用的激活函数种类繁多。Sometimes very common one that was used maybe before the mid-2000s was this sigmoid activation function that starts off at zero and then ramps up to one. And there's a whole zoo of these things that people work with sometimes. Sometimes I think we'll talk about the choices behind these for different reasons for why you might choose one over another in a later lecture.

在2000年代中期之前，一个非常常用的激活函数是S型函数，它从零开始逐渐上升到一。研究人员使用的这类激活函数确实种类繁多。有时我想我们会在后续课程中讨论这些选择背后的原因，解释为什么你可能选择其中一个而不是另一个。

But the TL;DR is that ReLU was a pretty good default choice, and for most applications sticking with ReLU you probably can't go wrong.

但简而言之，ReLU是一个相当不错的默认选择，对于大多数应用来说，坚持使用ReLU你可能不会出错。

So this is definitely the most widely used activation function in deep learning today, and you should probably consider using this for most of your deep learning applications.

因此这绝对是当今深度学习中使用最广泛的激活函数，你应该考虑在大多数深度学习应用中使用它。

Now I also want to point out that this neural network system is actually super simple to implement.

现在我还想指出，这个神经网络系统实际上实现起来非常简单。

We can actually train a full neural network system in just 20 lines of code.

我们实际上可以用仅仅20行代码训练一个完整的神经网络系统。

Here I'm doing NumPy instead of PyTorch because I can't give away your homework right.

这里我使用NumPy而不是PyTorch，因为我不应该泄露你们的作业答案。

And here what we're doing is that the first couple lines are setting up some random data and some random weights.

这里我们做的是，前几行代码设置了一些随机数据和随机权重。

The second bit of lines is doing this forward pass that we call where we compute the score function as a function of the input.

接下来的几行代码执行我们称之为前向传播的过程，在这里我们计算作为输入函数的得分函数。

We've also used a sigmoid non-linearity here as well which is that first line up top here we compute the gradients with respect to our weight matrix and here we take our gradient descent step.

我们还在这里使用了sigmoid非线性激活函数，也就是顶部的第一行，我们计算关于权重矩阵的梯度，然后在这里执行梯度下降步骤。

So you can see that this neural network system is actually quite simple to implement.

所以你可以看到这个神经网络系统实际上实现起来相当简单。

And if you actually copy paste this code and run it in your terminal, you'll be training your own neural network in 20 lines of Python.

如果你实际复制粘贴这段代码并在终端中运行它，你将用20行Python代码训练自己的神经网络。

So that's pretty exciting now when we talk about neural networks there's one word that people often get hung up on and that's this word neural.

所以这相当令人兴奋，当我们谈论神经网络时，有一个词人们常常纠结，那就是"神经"这个词。

So I think we could not have a course on deep learning without at least acknowledging the presence of the word neural in our neural network models.

所以我认为如果没有至少承认"神经"这个词在我们神经网络模型中的存在，我们就不能开设深度学习课程。

So we have to talk about that a little bit but I'm not a neuroscientist by any stretch of the imagination.

因此我们必须稍微讨论一下这个问题，但我绝不是神经科学家。

So please I expect that I could say something slightly wrong with respect to neuroscience but I'll do my best and please don't actually ask me too many hard questions about neuroscience.

So I'm not a neuroscientist but I gotta talk about this somehow. So please understand I might say some things wrong in neuroscience, but I'll do my best, please don't really ask me too many difficult questions about neuroscience.

所以我不是神经科学家，但我必须设法讨论这个问题。所以请理解我可能在神经科学方面会说错一些东西，但我会尽力而为，请不要真的问我太多关于神经科学的难题。

But the basic idea here is that our brains are amazing biological organisms. The basic building block of our brain is this little cell called the neuron.

这里的基本概念是，我们的大脑是奇妙的生物有机体。大脑的基本构建单元是一种叫做神经元的小细胞。

Neurons look something like this if you search them on Google. They have a couple important components in these cells.

如果你在谷歌上搜索神经元，它们看起来大概是这个样子。这些细胞中有几个重要组成部分。

One is they have a cell body in the middle where all of the juice is happening. Then they've got this long terminal out to the right called the axon.

一个是它们中间有细胞体，所有重要活动都在这里进行。然后它们右侧有一个叫做轴突的长末端。

This neuron will be sending electrical impulses out from the cell body down the axon. These neurons are all connected in giant networks where these axons are sending electrical signals to other neurons.

神经元会从细胞体发出电脉冲，沿着轴突传递。这些神经元通过巨大的网络相互连接，轴突将电信号发送给其他神经元。

The electrical signals from axons are received by these other little protrusions from the cell body called dendrites. The gap between the dendrite of one neuron and the axon of the other neuron is called the synapse.

来自轴突的电信号被细胞体上其他叫做树突的小突起接收。一个神经元的树突与另一个神经元的轴突之间的间隙被称为突触。

This neuron in the middle is going to collect electrical impulses coming down the axons of all the incoming neurons on the left. Those electrical impulses will be modulated by these synaptic connections between the dendrites and the axons.

中间的神经元将收集来自左侧所有输入神经元轴突传来的电脉冲。这些电脉冲将通过树突和轴突之间的突触连接进行调节。

Based on the rate and transformation of the electrical signals, this neuron will send down some other electrical signal downstream to the other neurons. One abstraction we can think about these neurons is by representing them using a firing rate.

基于电信号的频率和转换，这个神经元将向下游的其他神经元发送其他电信号。我们可以通过使用放电频率来表示这些神经元，这是一种抽象方式。

These neurons are maybe firing electrical impulses at some rate. The rate at which our electrical impulses fire is some kind of non-linear function of the rate of all of the input connections from all of the input neurons on the left.

We can imagine having this very simple mathematical model of a neuron. The cell body then collects all of these incoming signals from all of the neurons on the left and sums up all of the firing rates of all of the incoming neurons on the left. Then based on this sum of all the firing rates coming in on the left, we apply some kind of non-linear function.

我们可以想象有一个非常简单的神经元数学模型。细胞体收集来自左侧所有神经元的输入信号，并汇总左侧所有传入神经元的放电频率。然后基于左侧传入的所有放电频率之和，我们应用某种非线性函数。

Maybe a sigmoid, maybe a ReLU, maybe some other kind of nonlinear function that now computes the firing rate of this neuron as a function of the firing rates of all the input neurons. And then this is now the firing rate that this neuron will send off to other neurons downstream in other parts of the network.

可能是S型函数，可能是线性整流函数，也可能是其他类型的非线性函数，用于根据所有输入神经元的放电频率计算该神经元的放电频率。然后这个放电频率将被该神经元发送给网络中其他部分的下游神经元。

This is basically where the similarities end. I think this is a very crude approximation on the right here. This is basically what we're doing in our neural network systems that we use today.

这基本上就是相似之处的终点。我认为右侧展示的是一个非常粗略的近似。这基本上就是我们在当今使用的神经网络系统中正在做的事情。

As you can see there is some crude approximation to biological neurons, but there are many differences between biological neurons and artificial neurons and artificial neural networks. So you shouldn't get too hung up on these similarities.

正如你所看到的，这与生物神经元存在一些粗略的近似，但生物神经元与人工神经元及人工神经网络之间存在诸多差异。因此你不应过分纠结于这些相似之处。

One bit of dissimilarity is that biological neurons tend to be organized into very complex networks that could be highly irregular. They could even loop back around and have one neuron kind of loop back and send signals back to itself running around in time.

一个不同之处在于，生物神经元倾向于组织成非常复杂的网络，这些网络可能高度不规则。它们甚至可能形成回路，让某个神经元循环并将信号及时发送回自身。

So there can be very complex topological structures of neurons in real mammalian brains. In contrast, when we work with an artificial neural network based systems, we typically organize our neurons into layers.

因此真实哺乳动物大脑中的神经元可能具有非常复杂的拓扑结构。相比之下，当我们使用基于人工神经网络的系统时，我们通常将神经元组织成层。

These layers are something of an artificial construct that allows us to perform all of these multiplications and sums all jointly using efficient matrix and vector operations. So this notion of a layer is something of an abstraction to represent this big soup of neurons that might exist in a real mammalian brain.

这些层是一种人工构造，使我们能够使用高效的矩阵和向量运算同时执行所有这些乘法和求和。因此层的概念是一种抽象，用于表示可能存在于真实哺乳动物大脑中的大量神经元。

But people are getting creative and people are starting to explore artificial neural network-based systems with very crazy or even random connectivity patterns as well. And that can actually sometimes work in some cases.

But people are getting creative and starting to explore artificial neural network systems with very crazy or even random connection patterns. So one of some of my colleagues at Facebook AI Research had this paper last year where they trained neural networks with these connectivity patterns on the right that look totally nuts. But they actually train and get near state-of-the-art performance. So I guess random connections can, if you're careful, can sometimes work in these artificial systems as well. But the general story here is that you should be extremely careful with your brain analogies. Even though that word neural is hanging out in the term in the name of neural networks, I think that it's really something of a historical term at this point.

但人们正在发挥创造力，开始探索具有非常疯狂甚至随机连接模式的基于人工神经网络的系统。我在Facebook人工智能研究部门的一些同事去年发表了一篇论文，他们用右侧这些看起来完全疯狂的连接模式训练神经网络。但它们实际上能够训练并获得接近最先进的性能。所以我认为随机连接如果处理得当，有时也能在这些人工系统中发挥作用。但总体而言，你应该对大脑类比保持极度谨慎。尽管"神经"这个词出现在神经网络这个术语中，但我认为它现在更多是一个历史遗留的称谓。

You should not take too much significance between analogies between our artificial systems and actual biological neurons. So in particular, there's just maybe a couple caveats here. In our artificial systems, our neurons are kind of all the same kind.

你不应该过分看重人工系统与实际生物神经元之间的类比。特别要注意几个注意事项：在我们的人工系统中，所有神经元基本上都是同一类型的。

In a real brain you might imagine there's different types of neurons that have different specialized function. You can imagine that our neurons could perform inside an individual neuron it could perform very complex non-linear computations that are not well modeled by our simple activation functions.

而在真实大脑中，你可能会想象存在不同类型的神经元，它们具有不同的专门功能。你可以想象我们的神经元在单个神经元内部可能执行非常复杂的非线性计算，这些计算无法用我们简单的激活函数很好地建模。

You can also remember that we modeled these neurons in terms of a single scalar firing rate and that was our main abstraction that we used to represent the activity level of a neuron in the brain. And that was a very very coarse abstraction of what might actually be going on inside and between neurons.

你还要记得我们是用单一标量放电率来建模这些神经元的，这是我们用来表示大脑神经元活动水平的主要抽象方法。这是对神经元内部和神经元之间实际发生情况非常粗糙的抽象。

So this simple idea of a firing rate it might be too coarse to represent what's truly going on. So that's pretty much all I want to say about brains for the semester.

因此这种简单的放电率概念可能过于粗糙，无法代表真实发生的情况。这就是我这学期关于大脑想说的全部内容。

So with that let's go back to math and let's go back to engineering that I actually do know a little bit about hopefully. So then one question is if we're not going to take these brain analogies with too much seriousness, then why actually should we choose neural networks as a powerful image classification system or as a powerful function approximation system more broadly.

那么接下来让我们回到数学，回到我确实希望稍微了解一些的工程领域。那么问题来了：如果我们不打算过分认真对待这些大脑类比，那么我们为什么实际上应该选择神经网络作为强大的图像分类系统，或者更广泛地说，作为强大的函数逼近系统呢？

Well one we've already seen sort of coarsely how neural networks can represent multiple templates in their first layer and then recombine them in the second layer. But I think another interesting way to think about and see why neural networks are such a powerful system is through this notion of space warping.

On one hand, we have roughly seen how neural networks represent multiple templates in their first layer and then recombine them in the second layer. But I think another interesting way to understand why neural networks are such powerful systems is through the concept of spatial distortion.

So here we want to think about this geometric viewpoint of a linear classifier. Remember with a linear classifier when we thought about them geometrically we thought about our data points as all living in this high dimensional space, and then each row of our linear classifier gave rise to some plane in feature space in our input space.

一方面我们已经粗略地看到神经网络如何在其第一层表示多个模板，然后在第二层重新组合它们。但我认为另一个有趣的方式来思考并理解为什么神经网络是如此强大的系统，是通过空间扭曲这个概念。

因此这里我们想要考虑线性分类器的几何视角。记得当我们从几何角度思考线性分类器时，我们考虑的是关于我们的数据点都存在于这个高维空间中，线性分类器的每一行都会在我们的输入特征空间中产生某个平面。

We could imagine that these lines are the values at which you have a dot product of zero with your weight matrix, so each of these lines would be score of zero for that class and then they would increase linearly as we go perpendicular to the plane or line.

我们可以想象这些线代表与权重矩阵点积为零的值，因此每条线对应该类别的零分，然后随着我们垂直于平面或线的方向移动，分数会线性增加。

Previously we had always thought about this in terms of predicting classification scores, but another way we can think about what's going on here is as warping the input space.

之前我们总是从预测分类得分的角度来思考这个问题，但另一种理解方式是将其视为对输入空间的扭曲变换。

What we can imagine doing is that we're taking our input space which has features x1 and x2 and transforming it into another feature space that has coordinates h1 and h2 for the two dimensions of our two-dimensional hidden unit here.

我们可以想象的是，我们获取具有特征x1和x2的输入空间，并将其转换为另一个特征空间，该空间具有我们二维隐藏单元的两个维度坐标h1和h2。

If we have a linear transform, you can imagine that all of these regions of space are somehow deforming in a linear way with these two-dimensional linear transform we got these two rows and we've got these two lines in our input space.

如果我们进行线性变换，你可以想象所有这些空间区域都以某种线性方式变形，通过这个二维线性变换，我们在输入空间中得到了这两条线。

Those two lines in our input space divided up into four regions, and each of those four regions get transformed into the four quadrants in this transformed output space where now the space is going to get kind of rotated and transformed like this.

输入空间中的这两条线将空间划分为四个区域，每个区域都被转换到这个变换后输出空间的四个象限中，此时空间会以这种方式旋转和变换。

All four of these quadrants will get transformed in this linear way when we think about a linear transformation acting geometrically on the space.

当我们考虑线性变换在几何上作用于空间时，所有这四个象限都会以这种线性方式被变换。

Now we can then think about what happens when we try to train a linear classifier on top of a linear transform.

现在我们可以思考当我们在线性变换之上训练线性分类器时会发生什么。

If we try to train a linear classifier on top of a linear transform, then you can imagine we've got this cloud of data points here on the left where blue are maybe images of one category and orange are images of another category.

Once we apply this linear transform that linearly warps the space, we transform our input data into some new representation.

一旦我们应用这种线性变换来扭曲空间时，我们将输入数据转换为某种新的表示形式。

But now because this feature transform was only modifying the input space linearly, we can see that even though we've transformed the space, the points are still not linearly separable in this new transformed output space.

但由于这种特征变换只是线性地修改输入空间，我们可以看到即使变换了空间，在这个新的变换输出空间中数据点仍然不是线性可分的。

So somehow applying a linear classifier on top of a linear feature transformation is not going to increase the representational power of our model.

因此，在线性特征变换之上应用线性分类器并不会增加我们模型的表示能力。

We will still be unable to separate this particular data set linearly if we use a linear feature transform.

如果我们使用线性特征变换，我们仍然无法线性分离这个特定的数据集。

Now let's think about what happens once we apply this ReLU function.

现在让我们思考一下应用ReLU函数后会发生什么。

What happens is that in our input space we still have these four lines corresponding to the two rows of the weight matrix, and they still carve up our input space into these four quadrants.

在我们的输入空间中，我们仍然有这四条线对应于权重矩阵的两行，它们仍然将我们的输入空间划分为这四个象限。

But now because of this non-linear ReLU function, the way in which these four quadrants get transformed will all be different.

但由于这种非线性的ReLU函数，这四个象限被转换的方式都将不同。

In this first region A that corresponds to the positive quadrant in the input, because it corresponds to both directions of increase for the red and the green lines, that will transform just as it did in the previous linear transformed case.

在第一个区域A中，它对应于输入的正象限，因为它对应于红色和绿色线的两个增加方向，它将像之前的线性变换情况一样进行转换。

This quadrant A in the input space will be linearly warped into the quadrant A in the transformed feature space.

输入空间中的这个象限A将被线性扭曲到变换特征空间中的象限A。

But things get very interesting when we imagine quadrant B.

但当我们考虑象限B时，情况变得非常有趣。

Quadrant B in the input space corresponds to a positive value for the red feature but corresponds to a negative value for the green feature.

输入空间中的象限B对应于红色特征的正值，但对应于绿色特征的负值。

If you'll recall based on the structure of the ReLU, when the feature is positive it will be left alone and when the input is negative it will be set to zero.

如果您根据ReLU的结构回忆，当特征为正时它将保持不变，当输入为负时它将被设置为零。

What this means is that because all of the points in this quadrant have a negative value for their green feature, that means that their green feature will be set to zero.

这意味着由于该象限中的所有点在其绿色特征上都具有负值，这意味着它们的绿色特征将被设置为零。

What geometrically happens in this case is that the entire B quadrant in the input space is now collapsed onto the positive H2 axis in our transformed feature space.

在几何上发生的情况是，输入空间中的整个B象限现在被折叠到我们变换特征空间中的正H2轴上。

This is now very dramatically different from what's going on in the linear classification setup.

A similar thing happens with quadrant D. It's now collapsed onto the positive H1 axis because of the Rayleigh function.

类似的情况发生在象限D上。由于瑞利函数的作用，它现在坍缩到了正H1轴上。

Now quadrant C is really tight for space because all of quadrant C is now packed into the origin in our transform feature space.

现在象限C的空间非常拥挤，因为在我们的变换特征空间中，整个象限C都被压缩到了原点。

Now that we've seen what happens with quadrants, let's go back to this example of the data cloud.

既然我们已经看到了象限的变化情况，让我们回到这个数据云的例子。

We saw that transforming this data cloud linearly resulted in this linearly transformed data space.

我们看到线性变换这个数据云产生了这个线性变换后的数据空间。

But now when we imagine applying this ReLU non-linearity to this feature representation, we can see something very interesting has happened in the transform feature space.

但现在当我们想象将ReLU非线性应用于这个特征表示时，我们可以看到在变换特征空间中发生了非常有趣的事情。

What has happened is that now our yellow and blue points have become linearly separable in this transformed feature space.

发生的情况是，现在我们的黄色和蓝色点在这个变换后的特征空间中变得线性可分了。

In particular, if we were to train a linear model on top of this feature space H, we would be able to properly separate the blue and the yellow points.

具体来说，如果我们在特征空间H上训练一个线性模型，我们将能够正确分离蓝色和黄色点。

Now if we imagine porting this decision boundary from the output feature space back into the input space, we would see that decision boundary now corresponds to some non-linear decision boundary in the input feature space.

现在如果我们想象将这个决策边界从输出特征空间映射回输入空间，我们会看到该决策边界现在对应于输入特征空间中的某个非线性决策边界。

So we have this interpretation of these ReLU based neural networks as being like multiple linear classifiers all folding the space onto itself linearly and then applying linear classifiers on top of that folded transformed collapsed version of the space.

因此，我们对这些基于ReLU的神经网络有这样的理解：它们就像多个线性分类器，都以线性方式折叠空间，然后在这个折叠变换后的坍缩版本空间上应用线性分类器。

In this example we've seen a fully connected network with two dimensions in the hidden unit and that allowed us to fold the space over twice, or collapse two of the dimensions in the output space.

在这个例子中，我们看到了一个隐藏单元具有二维的全连接网络，这使我们能够将空间折叠两次，或者说在输出空间中坍缩两个维度。

If we are to increase and use neural network based systems with more and more dimensions in the hidden layer, you can imagine that now in the feature space we're drawing more and more lines in the original input space to divide it into two regions and then folding them back on itself.

If we use neural network-based systems with increasingly more dimensions in the hidden layer, you can imagine that in the feature space, we draw more and more lines in the original input space to divide it into two regions, and then fold them back onto themselves. This leads to an ever more complicated collapsed representation in the feature space such that linear decision boundaries in that complicated feature space now correspond to very complex non-linear decision boundaries in the original input space.

如果我们在隐藏层中使用具有越来越多维度的基于神经网络的系统，你可以想象在特征空间中，我们在原始输入空间中绘制越来越多的线将其分成两个区域，然后将它们折叠回自身。这导致特征空间中出现越来越复杂的坍缩表示，使得该复杂特征空间中的线性决策边界现在对应于原始输入空间中非常复杂的非线性决策边界。

In general from this example here we can see that by using more and more units in the hidden layer we end up with decision boundaries that are more and more complex.

从这个例子中我们可以看到，通过在隐藏层使用越来越多的单元，我们最终会得到越来越复杂的决策边界。

Well remember we talked last time about regularization and how regularization is a way of controlling the complexity of your model.

记得我们上次讨论过正则化，以及正则化是如何控制模型复杂度的一种方法。

When you see an image like this you might think that oh this model on the right is way too complex it's got a much too wiggly decision boundary it's very likely to overfit my training data.

当你看到这样的图像时，你可能会认为右边的模型过于复杂，它的决策边界过于曲折，很可能会过拟合我的训练数据。

So you might be tempted to try to regularize your neural network based model by reducing the number of dimensions in the hidden layer but in general that's actually not a great idea.

因此你可能会试图通过减少隐藏层的维度来正则化你的神经网络模型，但通常这不是一个好主意。

In general what we want to do is to regularize your neural network model using some kind of tunable regularization parameter rather than using the size of the network itself as a regularizer.

通常我们想要做的是使用某种可调节的正则化参数来正则化你的神经网络模型，而不是使用网络本身的大小作为正则化器。

In this example we can see that using the same number of hidden units then by increasing this strength of l2 regularization just by increasing the strength of l2 regularization we've been able to smooth out the decision boundary that the network has learned between the categories.

在这个例子中我们可以看到，使用相同数量的隐藏单元，然后通过增加L2正则化的强度，仅仅通过增加L2正则化的强度，我们就能够平滑网络在类别之间学习到的决策边界。

All of these examples on the last two slides were generated by this online web demo where you can go and train neural networks in real time in your browser and see these decision boundaries fly around in real time.

前两张幻灯片上的所有例子都是由这个在线网络演示生成的，你可以在浏览器中实时训练神经网络，并实时观察这些决策边界的变化。

So I would really encourage you to check her out check out and play with that to gain some intuition about this notion of neural networks as transforming feature spaces.

因此我强烈建议你去查看并尝试使用它，以获得关于神经网络作为特征空间转换器这一概念的直观理解。

Now with this idea of neural networks as transforming feature spaces and now that we've seen these very very complex decision boundaries that can be learned by neural network systems we might have an intuition that these neural network systems are very powerful and can represent very very large categories of functions much more so than the linear classifiers we've previously considered.

We now have the concept of neural networks as feature space transformers, and we have seen that neural network systems can learn very complex decision boundaries. We might intuitively think that these neural network systems are very powerful and can represent a broader class of functions than the linear classifiers we previously considered. We can actually formalize this a little bit so there's a property called universal approximation which is that a neural network system with one hidden layer can approximate any function from Rn to Rm. Of course, because when you say any function, people who have taken real analysis will get on your back. So there's a lot of technical caveats to this statement.

现在有了神经网络作为特征空间转换器的概念，并且我们已经看到了神经网络系统可以学习的非常复杂的决策边界，我们可能会直觉地认为这些神经网络系统非常强大，能够表示比我们之前考虑的线性分类器更广泛的函数类别。我们实际上可以稍微形式化这一点，有一个称为通用逼近的性质，即具有一个隐藏层的神经网络系统可以逼近从Rn到Rm的任何函数。当然，因为当你说任意函数时，学过实分析的人会挑你的毛病。所以这个声明有很多技术上的注意事项。

Something like, oh, maybe it's only a compact subset of the input space. Maybe it's a continuous function. What do we mean by arbitrary precision?

比如，可能只是输入空间的紧子集。可能是一个连续函数。我们所说的任意精度是什么意思？

But like this isn't a real analysis class, so we'll just ignore all those details and just say that neural networks can learn to approximate any continuous function on a bounded input space.

但这又不是实分析课，所以我们忽略所有这些细节，只说神经网络可以学习逼近有界输入空间上的任何连续函数。

To kind of get at intuition for how a ReLU based system can learn to approximate any function, we can think about algebraically how a ReLU based neural network is computing its outputs.

为了直观理解基于ReLU的系统如何学习逼近任意函数，我们可以从代数角度思考基于ReLU的神经网络如何计算其输出。

So let's consider a simple example of a ReLU based fully connected two-layer neural network that takes as input a single real number and produces as output a single real number.

让我们考虑一个简单的基于ReLU的全连接两层神经网络示例，它输入一个实数，输出一个实数。

Now the hidden layer in this neural network has maybe three units here, and we've got our weight matrix of the first layer. We got a weight matrix the first layer which is just a vector in this case because our input is one.

现在这个神经网络的隐藏层可能有三个单元，我们有了第一层的权重矩阵。在这种情况下，由于我们的输入是一维的，第一层的权重矩阵只是一个向量。

And our weight matrix at the second layer is also a vector because our output is only a single dimension.

第二层的权重矩阵也是一个向量，因为我们的输出只有单一维度。

Then we can write down how what is the functional form of each of these hidden layer activations.

然后我们可以写出每个隐藏层激活的函数形式。

We can see that the first hidden unit is equal to the max of 0 and w1 times x minus the bias b1. Here I'm putting the bias in because it's actually important for this example.

我们可以看到第一个隐藏单元等于0和w1乘以x减去偏置b1的最大值。这里我加入了偏置，因为它对这个例子很重要。

And this then it's a similar structure for all three of the units in the hidden layer.

然后隐藏层中所有三个单元都有类似的结构。

And then the output value y is then a linear combination of these hidden layer values. In this case, I've written the second weight matrix as u, so the final output value y is u1 times h1 plus u2 times h2 and you get the idea.

But now we can actually reorganize this output a little bit and we can write down this output value y as a scalar value u in the second weight matrix times this maximum of zero and an element in the first weight matrix times the input minus a bias.

现在我们可以稍微重新组织这个输出，并将输出值y写作来自第二个权重矩阵的标量值u的函数，乘以零的w与第一权重矩阵中元素乘以输入减去偏置的最大值。

The final output y then decomposes into the sum of these three different terms and what each of these three different terms looks like is some kind of shifted or scaled or translated version of the Rayleigh function.

最终输出y随后分解为这三个不同项的和，而每个项看起来都像是某种经过平移、缩放或变换的瑞利函数版本。

Here we can then flip this Rayleigh function to the left or the right depending on the sign of this w1 element in the first weight matrix we can determine the point between the flat region and the linear region.

这里我们可以根据第一权重矩阵中w1元素的正负来决定将瑞利函数向左或向右翻转，从而确定平坦区域与线性区域之间的转折点。

The point where it changes between flat to linear is given by the bias terms bi and the slope of the non-flat part is given by the ratio of the terms in the second weight matrix and the terms in the first weight matrix.

从平坦到线性变化的转折点由偏置项bi决定，而非平坦部分的斜率则由第二权重矩阵中的项与第一权重矩阵中的项之比决定。

So now we have this notion of this type of neural network system as computing its output as some kind of sum of all of these kind of shifted Rayleigh functions.

因此我们现在形成了这样的概念：这类神经网络系统通过将所有平移后的瑞利函数进行某种求和来计算其输出。

If we're clever with the way we do the shifting we can actually build up approximations to any function you can imagine and our strategy here is to build something called a bump function.

如果我们能巧妙地进行平移操作，实际上可以构建出任意函数的近似，而我们的策略是构建所谓的凸起函数。

This bump function will be flat over all of the input and then once we get to some chosen value s1 it will increase linearly up to some second chosen value t.

这个凸起函数将在整个输入范围内保持平坦，直到达到某个选定值s1时开始线性上升，直至第二个选定值t。

It will remain flat at t until we get to a second value s3 and then once we hit s3 it will decrease linearly back down to zero at s4 and then it will be zero again for the rest of forever.

在t值处保持平坦直到达到第三个值s3，一旦达到s3就开始线性下降，在s4处降回零值，此后将永远保持为零。

You can kind of imagine that because we've written down our neural network system and we've seen that the output is now a linear combination of these kind of shifted and scaled Rayleigh functions.

你可以这样理解：由于我们已经构建了神经网络系统，并且看到输出现在是这些平移和缩放后的瑞利函数的线性组合。

You can imagine building up this bump function by having a weighted sum of four different hidden units in particular.

We can imagine that we can imagine writing down the slope of these two regimes by using a simple slope calculation and once we have computed the slope of these two parts of the bump then we can approximate this first part of the bump with one linear unit that's doing one ReLU like this, and now we can then deal with the second kink in the bump function by adding in a second unit that has the following form.

我们可以设想通过简单的斜率计算来确定这两个区域的斜率，一旦计算出凸起函数这两个部分的斜率，我们就可以用一个像这样的线性单元执行一个ReLU来近似凸起函数的第一部分，然后通过添加具有以下形式的第二个单元来处理凸函数中的第二个转折点。

Then we can deal with the third bit in the bump by without with a fourth ship scaled and shifted and flipped ReLU. We can complete out our bump with a with a force with a fourth scale and shifted Rayleigh.

然后我们可以通过使用第四个缩放、平移和翻转的ReLU来处理凸函数中的第三部分。我们可以通过使用第四个缩放和平移的Rayleigh函数来完成凸函数的构建。

So now with this formulation you can see that but with a combination of four Rayleigh functions with a that is using a neural network with four hidden layers we can represent exactly this bump function.

通过这种表述可以看到，使用四个Rayleigh函数的组合——即使用具有四个隐藏层的神经网络——我们可以精确表示这个凸函数。

Where the exact location of the bump, the slopes of the lines and the height of the bump are all controllable by the weights in the first and the second layer.

凸函数的精确位置、线的斜率和凸函数的高度都可以通过第一层和第二层中的权重进行控制。

And now if we have not just hit a neural network with four hidden units but instead have a neural network with like 8 or 12 or 16 hidden units then we can imagine using each group of four hidden units to compute a separate bump.

如果我们不仅使用具有四个隐藏单元的神经网络，而是使用具有8、12或16个隐藏单元的神经网络，那么我们可以设想使用每组四个隐藏单元来计算一个独立的凸函数。

Then we can then the overall function that would be computed by this neural network we could set it up in a way so that it's a composition of some that it's a sum of these different bumps located arbitrary positions and arbitrary heights over the input.

然后我们可以设置这个神经网络计算的整体函数，使其成为这些位于输入上任意位置和任意高度的不同凸函数的和。

And once we have this this freedom to position bumps wherever we want then we can position the bumps in such a way that they perfectly approximate any type of continuous function that we want over this domain.

一旦我们拥有这种将凸函数放置在任何位置的自由，我们就可以通过放置凸函数来完美逼近该域上我们想要的任何类型的连续函数。

And now you can imagine that to increase the fidelity of our representation we need to make the bumps narrower and maybe reduce the gap between the bumps and things like that.

现在可以想象，为了提高我们表示的保真度，我们需要使凸函数更窄，并可能减少凸函数之间的间隙等。

So then you can imagine that we can add more and more bumps to get better and better approximation to any underlying function by using wider and wider and wider neural networks.

So with this interpretation you kind of get the sense that a two-layer neural network is actually good enough for computing any kind of continuous function with the big caveat that in order to approximate those functions within ever increasing fidelity we need to use networks that have more and more units in that middle hidden layer. And then there's many questions you might ask about this universal approximation setup.

通过这种解释，你会感觉到两层神经网络实际上足以计算任何类型的连续函数，但有一个重要注意事项：为了以不断提高的保真度逼近这些函数，我们需要使用在那个中间隐藏层中设置越来越多单元的网络。关于这个通用近似定理的设定，你可能会提出很多问题。

Because this was not really a formal proof, this was something of a sketch of a proof. You might ask about what's going on with the gaps between the bumps.

因为这并不是一个严格的证明，更像是一个证明的草图。你可能会问这些凸起之间的间隙是怎么回事。

You might ask about how would you deal with nonlinearities other than ReLU. You might also wonder how you could extend this analysis to higher dimensional functions and not just one variable in and one variable out.

你可能会问如何处理ReLU以外的非线性函数。你可能还想知道如何将这个分析扩展到更高维度的函数，而不仅仅是单输入单输出的情况。

I think we don't have time to talk about that in this lecture. But if you're interested in those questions, I suggest you look at this chapter by Michael Nielsen's book on deep learning that talks about universal approximation in a bit more detail.

我认为在这节课中没有时间讨论这些。但如果你对这些问题感兴趣，我建议你阅读Michael Nielsen深度学习书籍中关于通用近似定理的章节，其中有更详细的讨论。

So this is really cool right? Basically we've shown that with a neural network we can learn any kind of function no matter what that function might look like as long as it's continuous and blah blah blah.

这真的很酷，对吧？基本上我们已经证明，只要函数是连续的等等，无论函数看起来如何，我们都可以用神经网络学习任何类型的函数。

But this is then much more clearly a much more powerful type of representation than we could do with linear classifiers. But we need a bit of reality check here.

但这显然是一种比线性分类器更强大的表示方式。不过我们需要对此保持一点现实态度。

So you need to realize that this universal approximation construction is really a mathematical construction to show that in principle neural networks could potentially have the capacity to represent any function if you happen to set up the weights in exactly this right way.

你需要认识到，这个通用近似定理的构造实际上是一个数学构造，它表明原则上神经网络有可能表示任何函数，前提是你恰好以这种正确的方式设置了权重。

But in practice if we try to train neural networks to do this single variable regression type problem, they don't actually learn these bump representations at all.

但在实践中，如果我们尝试训练神经网络来解决这种单变量回归类型的问题，它们实际上根本不会学习这些凸起表示。

If we actually try to, for example here I used a neural network with something like I think it was 16 hidden units to try to learn a sine function. And you can see that it learns to fit the sine function pretty well but it didn't actually use bumps at all.

如果我们实际尝试，例如这里我使用了一个大约有16个隐藏单元的神经网络来尝试学习正弦函数。你可以看到它学会了很好地拟合正弦函数，但根本没有使用凸起。

It kind of used its linear ReLU units and scaled and shifted them around in some kind of way that ends up fitting the sine function very well but doesn't actually use this bump construction that we use in the proof of universal approximation.

它使用了线性ReLU单元，并以某种方式对它们进行缩放和移动，最终很好地拟合了正弦函数，但实际上并没有使用我们在通用近似定理证明中使用的这种凸起构造。

Um so I think sometimes you see so I think this result of universal approximation is really cool and really interesting and it gives us hope that neural networks are indeed a very powerful class of models that can flexibly represent a lot of different functions. But you should not put too much stock or too much faith in this universal approximation result because as we've seen universal approximation just tells us that there exists some setting of the weights that lets neural networks compute very complicated functions but it leaves a lot of questions unanswered.

嗯，我认为有时候你会看到，我觉得这个万能逼近定理的结果真的很酷也很有趣，它给了我们希望，神经网络确实是一类非常强大的模型，能够灵活地表示许多不同的函数。但你不应该对这个万能逼近定理的结果寄予太多期望或信心，因为正如我们所看到的，万能逼近定理只是告诉我们存在某种权重设置可以让神经网络计算非常复杂的函数，但它留下了很多未解答的问题。

For example it does not tell us how easy it is to actually learn those values of the weights to learn arbitrary functions. It tells us nothing about the learning procedure that we need to go through to set those weights nor does it tell us anything about how much data we actually need in order to properly approximate any function.

例如，它没有告诉我们实际学习这些权重值来学习任意函数有多容易。它没有告诉我们设置这些权重需要经历的学习过程，也没有告诉我们实际需要多少数据才能正确逼近任何函数。

So this result of universal approximation is really interesting but you should not take it as the end-all proof that neural networks are the best type of model ever because if you'll remember back to k-nearest neighbors back in lecture two we saw also that k-nearest neighbor also had this universal approximation property. Um so just uh just having universal approximation is not a strong enough property that that's not really where the magic is in neural networks because even something like k n is universal is universal.

所以这个万能逼近定理的结果确实很有趣，但你不应该把它当作证明神经网络是有史以来最佳模型的终极证据，因为如果你还记得第二讲中的k近邻算法，我们也看到k近邻同样具有这种万能逼近特性。嗯，所以仅仅拥有万能逼近特性并不是足够强大的特性，这实际上不是神经网络的神奇之处，因为即使是像k近邻这样的算法也具有万能逼近特性。

So now we've talked about a lot of good reasons about why why and how neural networks can be more powerful and more flexibly represent functions compared to linear models but we have not really talked about the optimization process right. Universal approximation tells us that there exists values of the weights that are diffic that can represent lots of functions but it doesn't tell them tell us how to find them well.

So now we have discussed many good reasons why and how neural networks are more powerful and flexible than linear models in representing functions, but we haven't really discussed the optimization process. The universal approximation theorem tells us that there exist weight values that can represent many functions, but it doesn't tell us how to find those weight values.

There is a question you might ask is how can we know whether neural networks or other types of machine learning models will actually converge to solutions that are useful or to globally optimal solutions or other things like that. Well one type of mathematical tool that we often use in optimization to talk about notions of optimality and convergence of optimization problems is the notion of a convex function.

A convex function is one that's going to take an input vector and return a single scalar.

所以现在我们已经讨论了很多关于为什么以及如何神经网络比线性模型更强大、更灵活地表示函数的好理由，但我们还没有真正讨论优化过程。万能逼近定理告诉我们存在能够表示许多函数的权重值，但它没有告诉我们如何找到这些权重值。

有一个问题你可能会问，那就是我们如何知道神经网络或其他类型的机器学习模型是否会实际收敛到有用的解或全局最优解或其他类似的东西。嗯，我们在优化中经常使用的一种数学工具来讨论最优性和优化问题的收敛性概念就是凸函数的概念。

凸函数是指接受输入向量并返回单个标量的函数。

You could imagine this is something like the loss function of a neural network based system, where the input is going to be the setting of the weight matrix and the output is going to be the scalar loss that tells us how well that weight matrix is doing.

可以将其想象为基于神经网络的系统的损失函数，输入是权重矩阵的设置，输出是标量损失，告诉我们该权重矩阵的表现如何。

A function is said to be convex if it satisfies this particular inequality constraint, which I think is better understood visually.

如果一个函数满足这个特定的不等式约束，我们就称其为凸函数，我认为通过视觉方式能更好地理解这一点。

So if we imagine what this inequality constraint is doing in this concrete example of f of x equals x squared, what it says is that when we take two points in the input x1 and x2 and we look at linear combinations of those points and then feed linear combinations of those points back to the function itself, then that should be less than something on the right hand side.

以f(x)等于x平方的具体例子来看这个不等式约束的作用，它表明当我们取输入中的两个点x1和x2，观察这些点的线性组合，然后将这些点的线性组合反馈给函数本身时，结果应该小于右侧的某个值。

The thing on the right hand side is basically referring to this chunk of the curve which is this chunk of the function f between the two points x1 and x2.

右侧的内容基本上指的是曲线中x1和x2两点之间的函数f片段。

The thing on the right is saying that is a secant line where if we compute the value of the function at x1 and the value of the function at x2, then the right hand side of this equation is going to look at all linear combinations of those two values of the functions computed at the end points x1 and x2.

右侧表明这是一条割线，如果我们计算函数在x1处的值和函数在x2处的值，那么该方程的右侧将考察端点x1和x2处计算得到的这两个函数值的所有线性组合。

What this property of convexity is saying is that whenever you take two points in the input domain, then the secant line between the two points always lies above the function itself.

凸性的这一特性表明，当你在输入域中取任意两点时，这两点之间的割线始终位于函数本身之上。

With that kind of geometric interpretation of convexity, you can clearly see that this quadratic function is indeed convex because if we imagine taking any two points in the input and imagine drawing a secant line between any two of those points in the input, then that secant line will always lie above the function itself.

Through this geometric interpretation of convexity, you can clearly see that this quadratic function is indeed convex, because if we imagine taking any two points in the input and drawing a secant line between them, this secant line will always lie above the function itself. You can prove this formally but I think it's quite intuitive when you look at it visually. At least in contrast, something like f of x equals cosine of x is not convex because we can draw these two points and find a secant line where the curve lies above the secant line of the function itself.

通过这种凸性的几何解释，你可以清楚地看到这个二次函数确实是凸的，因为如果我们想象在输入中取任意两点，并在它们之间画一条割线，那么这条割线将始终位于函数本身之上。你可以正式证明这一点，但我认为通过视觉观察会非常直观。至少相比之下，像f(x)等于cos(x)这样的函数不是凸的，因为我们可以画出这两点并找到一条割线，使得曲线位于函数本身割线的上方。

Something like cosine is not convex, and the intuition about convexity more generally for higher dimensional functions is that a convex function is somehow a high dimensional analog or high dimensional generalization of a bowl shaped function.

像余弦这样的函数不是凸函数，而对于高维函数凸性的更一般直觉是，凸函数在某种程度上是碗形函数的高维类比或高维推广。

Because if it's kind of a bowl, if you take now a secant line between any two points, it's always the secant is always going to lie above the function itself, and that always kind of gives us some kind of general bowl shaped function.

因为如果它有点像碗的形状，那么你在任意两点之间取一条割线，这条割线总是位于函数本身之上，这总是给我们某种广义的碗形函数的感觉。

Now convex functions have a lot of beautiful and elegant and amazing mathematical properties, so that if you want to know more about it, you can take an entire course about it is IOE or Math 663.

凸函数具有许多优美、精致且令人惊叹的数学性质，所以如果你想了解更多，你可以选修关于它的完整课程，比如IOE或数学663。

Clearly you should not expect to learn everything about convex functions or convex optimization in this one little chunk of a lecture, but for the purposes of this class, the course intuition that you should know is that convex functions are roughly bowl shaped and amazingly convex functions can actually be optimized efficiently.

显然，你不应期望在这短短的一节课中学会关于凸函数或凸优化的一切，但就本课程的目的而言，你应该掌握的课程直觉是：凸函数大致呈碗状，并且令人惊奇的是，凸函数实际上可以被高效地优化。

If you take this class and work through an entire semester of math, then you will be able to write down formal theorems about when you can actually have optimization algorithms that provably converge to the global minimum, and you can prove that convex functions have global minimum and that local minima are global minima and that things actually converge and it actually works.

如果你选修这门课并完成一整个学期的数学学习，那么你将能够写下形式定理，说明何时你实际上可以拥有可证明收敛到全局最小值的优化算法，并且你可以证明凸函数具有全局最小值，局部最小值就是全局最小值，并且算法确实会收敛且有效。

So that's amazing, but it takes actually quite a lot of mathematical machinery to work up to those results, but the takeaway is that also in practice convex optimization problems are quite easy to solve in practice and they also tend not to depend on initialization right because the convex property these convex functions are nicely bowl shaped so you can get convergence guarantees of the form like no matter where you start you're always going to find the bottom somehow.

所以这很神奇，但实际达到这些结果需要相当多的数学工具，但要点是，在实践中凸优化问题也相当容易解决，而且它们往往不依赖于初始化，因为凸函数的这种凸性质使它们呈良好的碗状，因此你可以得到形式的收敛保证，即无论你从哪里开始，你总能以某种方式找到底部。

So the course into it the very coarse intuition that you should take away is that convex functions are easy and robust to optimize and we have theoretical guarantees about optimizing convex functions. Now one reason that we've spent so much time talking about linear classifiers and linear models is that the optimization problems that we end up optimizing when we try to fit linear models to our data are convex optimization problems.

所以课程中你应该带走的最粗略的直觉是：凸函数易于优化且稳健，并且我们在优化凸函数方面有理论保证。我们花费大量时间讨论线性分类器和线性模型的一个原因是，当我们尝试将线性模型拟合到数据时，最终需要优化的这些优化问题都是凸优化问题。

When we try to train linear models we can actually write down formal theoretical guarantees about the convergence of those training runs in the context of linear models.

当我们尝试训练线性模型时，我们实际上可以写下关于这些训练过程收敛性的正式理论保证。

So this is actually a reason why people sometimes prefer linear models over neural network based models is if you actually want some kind of formal guarantee about the convergence of the system.

因此这实际上是人们有时更喜欢线性模型而非基于神经网络模型的一个原因——如果你确实想要关于系统收敛性的某种形式保证。

Now no such guarantees exist for neural network-based systems unfortunately empirically what we can do is kind of slice through different slices of the lost surface of a neural network based model to kind of get some intuitive picture of what these lost surfaces of neural networks might look like.

不幸的是，基于神经网络的系统不存在这样的保证，我们经验上能做的是对基于神经网络的模型的损失表面进行不同切面的切片，以获得对这些神经网络损失表面可能样貌的直观认识。

So here what I've done is I've built a five-layer multilayer ReLU network and then I've picked a single element of the weight matrix in the first layer of the weight of the ReLU network and then I'm computing on the x-axis are different values of that one element of the weight matrix and the y-axis are the values of the loss function as we change one element of the weight matrix inside this deep ReLU based neural network system.

这里我构建了一个五层多层ReLU网络，然后选取了ReLU网络第一层权重矩阵中的单个元素，在x轴上计算该权重矩阵元素的不同的值，y轴则是当我们改变这个基于ReLU的深度神经网络系统内部权重矩阵的一个元素时损失函数的值。

What you should think about this as being is that our loss surface when we optimize these high dimensional neural networks are these very very high dimensional loss surfaces that we can't visualize because we live only in three dimensions but what this is visualizing is now a one-dimensional slice through this very high dimensional loss surface.

你应该这样理解：当我们优化这些高维神经网络时，我们的损失表面是这些非常高维的损失表面，由于我们只生活在三维空间而无法可视化，但这里可视化的是这个极高维损失表面的一维切片。

What we can see here is that sometimes we get slices of loss surfaces of neural networks that actually do look sort of convex or bowl like but other times we get this very non-convex slices of our loss surfaces in neural network systems.

So this is another loss surface through a different part of the same ReLU based architecture and this looks very very much non-convex or you can get even wilder than that we can get chunks of the lost surface that have this. This is like adversarial to gradient-based learning right. If you imagine trying to optimize this type of a loss surface with gradient descent, you have to like climb up this hay up this thing and you'll get stuck in this very deep hill that needs to climb out somehow.

这是通过相同基于ReLU架构不同部分的另一个损失表面，这个看起来非常非常非凸，或者你甚至可能得到比这更复杂的——我们可以得到具有这种特性的损失表面片段。这就像是对基于梯度的学习具有对抗性。如果你想象尝试用梯度下降优化这种类型的损失表面，你必须像爬干草堆一样爬上去，然后会陷入这个需要设法爬出来的非常深的山谷中。

Like this is like bad news for gradient based optimization. And you can get these very very wild types of slices of lost surfaces when we try to train deep neural networks.

这对基于梯度的优化来说是个坏消息。当我们尝试训练深度神经网络时，可能会得到这些非常不规则的损失表面切片。

So the takeaway here is that neural networks rely on non-convex optimization. That in general the optimization problems that we are trying to solve when we optimize neural network systems and fit them to our data, we're trying to solve a non-convex optimization problem.

所以这里的要点是神经网络依赖于非凸优化。通常来说，当我们优化神经网络系统并将其拟合到我们的数据时，我们试图解决的是一个非凸优化问题。

And this is like terrible news theoretically. We basically have no theoretical guarantees about convergence. We basically have no theoretical guarantees about anything but empirically it seems to work anyway which is somewhat surprising.

这在理论上是相当糟糕的消息。我们基本上没有关于收敛的理论保证。我们基本上没有任何理论保证，但经验上它似乎仍然有效，这有些令人惊讶。

So this is an extremely active area of research of trying to characterize the theoretical properties of the optimization problems that arise from training neural network systems. But right and I think there's maybe some promising results or promising progress on this, but the story is far from complete.

所以这是一个极其活跃的研究领域，试图描述训练神经网络系统所产生的优化问题的理论特性。我认为在这方面可能有一些有希望的结果或进展，但故事还远未完成。

And I think we still as a community do not fully understand the theoretical properties of these optimization problems. But it seems to work anyway so I guess we'll do it right that's kind of the takeaway here.

我认为作为学术界，我们仍然没有完全理解这些优化问题的理论特性。但无论如何它似乎有效，所以我想我们会继续这样做，这就是这里的要点。

And hopefully the theory will catch up eventually at some point. So the summary then of what we talked about today is that we saw this notion of feature transforms and how by combining a feature transform with a linear model we could end up with much more complicated decision boundaries.

希望理论最终会在某个时候跟上。那么我们今天讨论的总结是，我们看到了特征变换的概念，以及如何通过将特征变换与线性模型结合，我们可以得到更复杂的决策边界。

And we saw a neural network system as kind of jointly learning a feature transform and this linear model. We talked about these two layer neural network systems and saw how they use these distributed representations to reshuffle different template values to more powerfully represent visual features compared to linear models.

We viewed the neural network system as jointly learning feature transformations and this linear model. We discussed two-layer neural network systems and saw how they use distributed representations to recombine different template values, representing visual features more powerfully than linear models. We talked a little bit about brains but I don't want to talk about that too much. And then we talked about these kind of nice interesting properties of these fully connected networks the notion of space warping of a universal approximation and this bad property of non-convexity then of course there's a big open problem for us to consider.

我们将神经网络系统视为联合学习特征变换和这个线性模型。我们讨论了两层神经网络系统，并看到了它们如何使用这些分布式表示来重新组合不同的模板值，以比线性模型更强大地表示视觉特征。我们稍微谈到了大脑，但我不想过多讨论这个。然后我们讨论了这些全连接网络的一些有趣特性，包括空间扭曲的概念、通用逼近定理以及这种非凸性的不良特性，这自然引出了一个需要我们思考的重大开放性问题。

Which is how do we actually compute gradients in these big neural network based systems?

即我们如何在这些基于神经网络的大型系统中实际计算梯度？

I think it's not just working it out on paper is not going to scale as we move to very complicated systems and very big and complicated models.

我认为仅仅在纸上计算是无法适应我们转向非常复杂的系统以及庞大复杂模型的规模要求的。

So to learn how to do that we'll cover the back propagation algorithm in the next lecture.

因此为了学习如何实现这一点，我们将在下一讲中介绍反向传播算法。