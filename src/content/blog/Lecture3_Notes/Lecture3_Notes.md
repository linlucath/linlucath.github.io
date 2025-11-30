---
title: 'Lecture3_Notes'
publishDate: 2025-11-23
description: 'TODO'
tags:
  - TODO
language: 'English'
heroImage: { src: './default.jpg', color: '#D58388' }
---

So welcome back to lecture three. Today we're going to talk about linear classifiers. So a quick recap: in the last lecture we talked about this image classification problem and you'll recall that this was a foundational problem in computer vision where we had to take this input image and then our network or system had to predict a category label from one of a fixed set of categories for the input image. And remember last time we talked about various challenges of this image recognition or image classification problem that we somehow need to build.

欢迎回到第三讲。今天我们将讨论线性分类器。快速回顾一下：在上一讲中我们讨论了图像分类问题，你们会记得这是计算机视觉中的一个基础问题，我们需要获取输入图像，然后我们的网络或系统必须从固定类别集合中为输入图像预测一个类别标签。还记得上次我们讨论了图像识别或图像分类问题的各种挑战，我们需要构建。

Classifiers that can be robust to all these different sorts of variation that can appear in our input data, things like viewpoint changes, illumination changes, deformation, etc. The challenge in building high performance recognition systems is building systems that are robust to all these different changes in the visual input that they need to process. So you would remember also last time we talked about the data-driven approach to overcoming some of these challenges, that rather than trying to write down an explicit function that deals with all of those hairy bits of visual recognition.

能够对我们输入数据中可能出现的各种变化保持鲁棒性的分类器，比如视角变化、光照变化、形变等。构建高性能识别系统的挑战在于构建能够对其需要处理的视觉输入中的所有不同变化保持鲁棒性的系统。所以你们还记得上次我们也讨论了克服其中一些挑战的数据驱动方法，即不是试图写出一个明确的函数来处理视觉识别中所有那些棘手的部分。

Instead, our approach is to collect a big data set that hopefully covers all of the types of visual things that we want to recognize and then to use some kind of learning algorithm to learn from the data how to recognize various text images. It's a concrete example of this pipeline in the last lecture we talked about the K nearest neighbor classifier that was fairly simple that memorized the training data and then output the label at test time that of the image most similar to in the training set to the test data and we saw how.

相反，我们的方法是收集一个大数据集，希望涵盖我们想要识别的所有视觉类型，然后使用某种学习算法从数据中学习如何识别各种文本图像。在上一讲中我们讨论的K最近邻分类器就是这个流程的具体例子，它相当简单，记忆训练数据，然后在测试时输出训练集中与测试数据最相似的图像的标签，我们看到了如何。

This led to ideas of hyper parameters and cross-validation. We went through this entire pipeline of an image classification system in the last lecture. But remember when we left off we said that the K nearest neighbor algorithm was actually not very useful in practice for a couple reasons. One was that it inverted this idea of what is slow and fast that it was very fast at training but very slow to evaluate. The other problem was that it wasn't very perceptually meaningful.

这引出了超参数和交叉验证的概念。我们在上一讲中讨论了图像分类系统的整个流程。但记得我们结束时提到，K最近邻算法实际上在实践中不太有用，原因有几个。一是它颠倒了快慢的概念，训练时非常快但评估时非常慢。另一个问题是它在感知上不太有意义。

that sort of L2 Euclidean or L1 distances on raw pixel values was not a very perceptually meaningful thing to measure. So today we're going to talk about a different sort of classifier that is very different in flavor from the K-nearest neighbors classifier that we talked about before. So today we're going to talk about various types of linear classifiers that we can use to solve this image classification problem.

那种在原始像素值上使用L2欧几里得或L1距离的方法并不是一个在感知上很有意义的衡量方式。所以今天我们将讨论一种不同类型的分类器，其特点与我们之前讨论的K最近邻分类器截然不同。所以今天我们将讨论可以用来解决这个图像分类问题的各种类型的线性分类器。

So linear classifiers might sound kind of simple, but they're actually very important when you're studying neural networks. Because sometimes when you build new neural networks, it's kind of like you want to stack all together your layers as a set of Lego blocks, and one of the most basic blocks that you're going to have in your toolbox when you build these large complicated big neural networks is a linear classifier. So sort of speaking hoarsely, once we move beyond linear classifiers and move to these big complicated neural models, then we'll see that the individual components of those neural network models will look very similar to these linear classifiers that we'll talk about today.

线性分类器可能听起来比较简单，但它们在研究神经网络时实际上非常重要。因为有时候当你构建新的神经网络时，就像要把所有层像乐高积木一样堆叠在一起，而在构建这些大型复杂神经网络时，工具箱中最基本的构建模块之一就是线性分类器。粗略地说，一旦我们超越线性分类器转向这些大型复杂神经模型，我们会发现那些神经网络模型的各个组件看起来与我们今天要讨论的线性分类器非常相似。

And indeed, much of the intuition and technical bits that we'll cover today will carry over completely as we start to move to neural network systems in the next couple of lectures. So as a quick recap, remember that we've been working with this CIFAR-10 dataset. The CIFAR-10 dataset is one of these standard benchmark datasets for image classification that contains 50,000 training images and 10,000 test images, where each of these images is very tiny - it's 32 pixels by 32 pixels, and within each pixel we have three scalar values for the red, blue.

事实上，我们今天要讨论的大部分直觉理解和技术要点，在我们接下来几节课开始转向神经网络系统时，都将完全适用。快速回顾一下，请记住我们一直在使用这个CIFAR-10数据集。CIFAR-10数据集是图像分类的标准基准数据集之一，包含50,000张训练图像和10,000张测试图像，其中每张图像都非常小 - 尺寸为32像素×32像素，在每个像素内我们有三个标量值分别对应红色、蓝色。

And green color channels of the pixels. So the idea of a linear classifier is part of a much broader set of approaches toward building machine learning models. That's the idea of a parametric approach. The idea of a parametric approach is that we're going to take our input image much as we've seen in the previous lecture, but now there's a new component in our system and that's these learnable parameters shown in red at the bottom of the slide.

每个像素的绿色颜色通道。因此线性分类器的概念属于构建机器学习模型的更广泛方法体系的一部分。这就是参数化方法的理念。参数化方法的理念是，我们将像上一节课中看到的那样处理输入图像，但现在我们的系统中有一个新组件，那就是幻灯片底部用红色显示的这些可学习参数。

So then we're going to write this function f which is going to somehow input the image, the pixels of the image, as well as these learnable weights W, and the functional form will somehow end up spinning out ten numbers giving some classification scores for each of the categories that we want the system to be able to recognize. This is a fairly general framework and a fairly general setup, and this idea of a parametric classifier will carry over completely to the neural network systems that we'll talk about.

因此，我们将编写这个函数f，它将以某种方式输入图像、图像的像素以及这些可学习的权重W，而函数形式最终会输出十个数字，为我们希望系统能够识别的每个类别提供一些分类分数。这是一个相当通用的框架和设置，这种参数化分类器的理念将完全延续到我们将要讨论的神经网络系统中。

But we're going to talk about the possibly the simplest instantiation of this parametric classifier pipeline and that's the linear classifier where it has the simplest possible functional form where this F of image and weights W is just going to be a matrix vector multiply between the learnable weights W and the pixels of the image X. To make this a little bit more concrete, remember that the input image for something like CIFAR-10 has is a 32 by 32 by 3 which means that if we count the total number of scalar values that are inside that.

但我们将讨论这个参数化分类器流程可能最简单的实例化，那就是线性分类器，它具有最简单的函数形式，其中这个关于图像和权重W的函数F将只是可学习权重W和图像像素X之间的矩阵向量乘法。为了更具体地说明这一点，请记住像CIFAR-10这样的输入图像是32×32×3的，这意味着如果我们计算其中的标量值总数。

Each of those images we had kind of multiply it out you end up with third with 3072 individual scalar numbers that make up the pixels of that input image. So now so then we will have a weight matrix so then we'll take the pixels of the image and stretch them out into a long vector so this will completely destroy all of the spatial structure in the image and we'll just reorganize all of the data in the input image into a long vector that has 3072 elements.

当我们对这些图像进行乘法运算时，最终会得到3072个单独的标量数值，这些数值构成了输入图像的像素。因此，我们将拥有一个权重矩阵，然后我们将获取图像的像素并将其拉伸成一个长向量，这将完全破坏图像中的所有空间结构，我们只是将输入图像中的所有数据重新组织成一个包含3072个元素的长向量。

And of course we'll need to do this vector application of our image in a consistent way that every time we take an image we always need to convert it into a vector in the consistent same way every time. And once we have chosen some way to flatten our image data into a vector, then our weight matrix will be a two dimensional matrix of shape 10 by 3072 where 10 remember is the number of categories that we want to recognize and 3072 is the number of pixels in the image.

当然，我们需要以一致的方式对图像进行这种向量处理，每次获取图像时，我们都需要以相同一致的方式将其转换为向量。一旦我们选择了某种将图像数据展平为向量的方法，那么我们的权重矩阵将是一个10×3072的二维矩阵，其中10是我们想要识别的类别数量，3072是图像中的像素数量。

And this and when you perform this matrix vector multiplication, the output will be again a vector of size 10 where 10 giving one score for each of the ten categories we want our classifier to recognize. So sometimes you'll also see linear classifiers with a bias term that will be a matrix vector multiply plus an additional bias term B where B is this vector with ten elements giving offsets for one of each of the ten categories that we wish to learn. So this is a fairly straightforward way to think about linear classifiers.

当你执行这个矩阵向量乘法时，输出将再次是一个大小为10的向量，其中10代表我们想要分类器识别的十个类别中每个类别的得分。因此，有时你也会看到带有偏置项的线性分类器，即矩阵向量乘法加上一个额外的偏置项B，其中B是一个包含十个元素的向量，为我们希望学习的十个类别中的每一个提供偏移量。这是思考线性分类器的一种相当直接的方式。

But over the next couple slides I want to dive into what this means in the context of image classification. So first as a concrete example, suppose that we just want to make this super concrete. Suppose that our input image is a 2 by 2 grayscale image, so then it has only 4 pixel values that give the full state of the image. Then we want to stretch the pixels out into a vector form into a column vector with four entries. So here I've just written out the exact values of each of the pixels in this image.

在接下来的几张幻灯片中，我想深入探讨这在图像分类背景下的含义。首先作为一个具体示例，假设我们想要让这一点变得非常具体。假设我们的输入图像是一个2×2的灰度图像，那么它只有4个像素值来完整描述图像状态。然后我们希望将像素展开成向量形式，形成一个包含四个元素的列向量。这里我已经写出了这张图像中每个像素的具体数值。

And then our weight matrix is. And then in this simple example, we will consider classifying only three categories rather than ten, maybe cats, dogs and ships shown in the three with these three corresponding colors. Now in this simple example, the weight matrix W will have shape 3 by 4, where 3 is the number of categories we want to recognize and 4 is the total number of pixels in our input image. And then our bias will again be of shape 3 because this is the number of categories that we want to recognize.

然后我们的权重矩阵是。在这个简单示例中，我们将考虑仅对三个类别进行分类而不是十个，可能是猫、狗和船，用这三种对应的颜色显示。在这个简单示例中，权重矩阵W的形状将是3×4，其中3是我们想要识别的类别数量，4是我们输入图像中的像素总数。然后我们的偏置项形状再次为3，因为这是我们想要识别的类别数量。

So then we'll perform this vector matrix multiplication and we'll output the specter of scores getting one score for each category we want to recognize. So when you look at the problem in this way you can start to recognize a little bit of structure in how we're breaking up this image classification problem. If you remember the way that matrix vector multiplication works you know you take the vector and you kind of multiple take inner products along each row the matrix you recognize you realize that each row of this matrix corresponds to one of the categories that our classifier wants.

然后我们将执行这个向量矩阵乘法，并输出得分向量，为我们想要识别的每个类别获得一个分数。当你以这种方式看待问题时，你就能开始认识到我们在分解这个图像分类问题时的一些结构。如果你记得矩阵向量乘法的工作原理，你知道你需要取向量并沿着矩阵的每一行进行内积运算，你会认识到这个矩阵的每一行对应着我们分类器想要的一个类别。

To recognize so I think it's useful to think about linear classifiers in a couple of different equivalent ways. When you think and by using different viewpoints to think about linear classifiers, it can make certain properties of them very obvious and or not obvious. So having different ways to think about a linear classifier can help you understand it more intuitively. So the first idea that the first way I like to think of linear classifiers is what I call the algebraic viewpoint which is exactly this idea of a linear classifier as a.

我认为以几种不同的等效方式来思考线性分类器是很有用的。当你使用不同视角来思考线性分类器时，某些特性会变得非常明显或不明显。因此，用不同方式思考线性分类器可以帮助你更直观地理解它。所以我喜欢思考线性分类器的第一种方式就是我称之为代数视角的观点，这正是将线性分类器视为一个。

Matrix vector multiply plus a vector offset. If you think about the algebraic viewpoint of a linear classifier, you reckon there's a couple features or facts about linear classifiers that immediately become obvious. One is that we can equivalently do what sometimes is referred to as the bias trick that eliminates the bias as a separate learnable parameter and instead incorporates the bias directly into the weight matrix W. The way that we do this is that we can augment our input image with the vector representation of our input image.

矩阵向量乘法加上向量偏移。如果你考虑线性分类器的代数视角，你会发现线性分类器有几个特征或事实会立即变得明显。其中之一是我们可以等效地执行所谓的偏置技巧，该技巧将偏置作为单独可学习参数消除，而是直接将偏置纳入权重矩阵W中。我们实现这一点的方式是通过用输入图像的向量表示来增强我们的输入图像。

With an additional constant one at the end of the vector and then augment our weight matrix with an additional column corresponding to that that will now perform the exact same computation as the W X plus B formulation that we saw before. So that's kind of a nice feature and this bias trick is quite common to use when your input data has a native vector form. So it's nice to be aware of as you think about building different types of machine learning systems.

在向量末尾添加一个额外的常数1，然后用对应的额外列来扩展我们的权重矩阵，这样就能执行与我们之前看到的WX+B公式完全相同的计算。这是一个相当不错的特性，当你的输入数据具有原生向量形式时，这种偏置技巧相当常用。因此，在考虑构建不同类型的机器学习系统时，了解这一点是很有用的。

But in fact in computer vision this bias trick is less common to use in practice because it doesn't carry over so nicely as we move from linear classifiers to convolutions later on and furthermore it's nice sometimes to separate the weight and the bias into separate parameters so we can treat them differently and how they're initialized or regularize or other things like that but nevertheless this bias trick is a fairly nice thing to be aware of for linear classifiers and it's totally obvious when you think about it when you think about linear classifiers through this lens of the algebraic viewpoint another another thing that's very obvious when you think

但实际上在计算机视觉中，这种偏置技巧在实践中不太常用，因为当我们从线性分类器转向卷积时，它不能很好地延续，而且有时将权重和偏置分成单独的参数是很好的，这样我们可以以不同的方式处理它们，比如在初始化、正则化或其他方面。尽管如此，对于线性分类器来说，这种偏置技巧是相当值得了解的，当你通过代数视角的镜头来思考线性分类器时，这一点是完全显而易见的。

Another thing that's very obvious when you think about linear classifiers in this algebraic way is that the predictions are linear. So what this means is that in a simple example, if we ignore the bias and we imagine scaling our whole input image by some constant C, then we could just pull that constant out of the linear classifier and that means that the predictions of the model will also be scaled by that scalar value C. So if you think about images, that means that if we have some input image on the left with some original image.

当你以这种代数方式思考线性分类器时，另一个非常明显的事情是预测是线性的。这意味着在一个简单的例子中，如果我们忽略偏置并想象将整个输入图像按某个常数C进行缩放，那么我们可以直接将这个常数从线性分类器中提取出来，这意味着模型的预测也将按该标量值C进行缩放。所以如果你考虑图像，这意味着如果我们左边有一些原始图像的输入图像。

Set with some predicted cat classifier scores from a linear classifier, then if we were to modify the image by sort of desaturating it by multiplying all the pixels by some constant one half, then all of the predicted category scores from the classifier would all be cut in half as well. So this is maybe a bug maybe a feature, but it feels kind of weird for linear classifiers to behave in this way on image data because you might think that just by scaling down all the pixels by a constant value we as humans have.

假设我们有一组来自线性分类器的预测猫分类器分数，如果我们通过将所有像素乘以某个常数（如1/2）来降低图像饱和度，那么分类器预测的所有类别分数也将减半。这可能是缺陷也可能是特性，但线性分类器在图像数据上以这种方式表现确实有些奇怪，因为您可能认为仅仅将所有像素按恒定值缩放，我们人类仍然可以。

We can still recognize this as a cat just as easily, but somehow it's a bit unintuitive that just scaling down all the pixels changes the predictive scores from the classifier. So that's a kind of a weird feature of linear classifiers that may or may not be important depending on exactly what loss function used to train these. So we'll talk about that a bit later. So that's the algebraic viewpoint that I like to think about for linear classifiers.

我们仍然可以同样轻松地将其识别为猫，但不知何故，仅仅缩小所有像素就会改变分类器的预测分数，这有点不太直观。所以这是线性分类器的一个奇怪特性，它可能重要也可能不重要，具体取决于用于训练这些分类器的损失函数。所以我们稍后会讨论这一点。这就是我喜欢思考线性分类器的代数观点。

But there's a very, we can reformulate this computation in an equivalent but slightly different way that will give us a slightly different way to think about exactly what image linear classifiers are doing in the context of image data. So remember from the algebraic viewpoint of a matrix vector multiply, we saw that the classification score that's predicted for each category is the result of an inner product between the vector representation of the image and one of the rows of the matrix. In this algebraic viewpoint, recall that we had taken the pixel values of our input image and stretch them out into a column vector.

但是我们可以用一种等效但略有不同的方式来重新表述这个计算，这将给我们提供一个稍微不同的视角来思考图像线性分类器在图像数据上下文中的具体工作原理。从矩阵向量乘法的代数观点来看，我们看到每个类别的预测分类分数是图像向量表示与矩阵某一行之间内积的结果。在这个代数观点中，回忆一下，我们获取输入图像的像素值并将其拉伸成一个列向量。

And then when we took this inner product we ended up with an inner product of these rows in the matrix and the column of the stretched out version of the image. Well rather than stretching out the image into a column vector, we can instead think about reshaping the rows of that matrix into the same shape as the input image. Then we get a system that looks something like this on the right. So here we've taken each of the rows of the matrix and reshaped them to have this same two-by-two shape as the image that we're trying to classify. And now then we've broken up these rows of the matrix into these four different sort of columns in the diagram here. And now the weight and now the bias vector has then been broken up into these three separate elements that we split along the columns. So then when we think about linear classifiers in this way, it lets us interpret their behavior in a slightly different and slightly perhaps more intuitive way.

然后当我们进行这个内积运算时，最终得到的是矩阵中这些行与图像拉伸版本列向量的内积。与其将图像拉伸成列向量，我们可以考虑将矩阵的行重塑成与输入图像相同的形状。这样我们就得到了右侧所示的系统。这里我们将矩阵的每一行重塑成与我们试图分类的图像相同的2x2形状。现在我们将矩阵的这些行分解成图中所示的四种不同列。同时权重和偏置向量也被分解成我们沿列分割的三个独立元素。因此当我们以这种方式思考线性分类器时，它让我们能够以略有不同且可能更直观的方式来解释它们的行为。

And now then we've broken up these rows of the matrix into these four different sort of columns in the diagram here. And now the weight and now the bias vector has then been broken up into these three separate elements that we split along the columns. So then when we think about linear classifiers in this way, it lets us interpret their behavior in a slightly different and slightly perhaps more intuitive way.

现在我们将矩阵的这些行分解成图中所示的四种不同列。同时权重和偏置向量也被分解成我们沿列分割的三个独立元素。因此当我们以这种方式思考线性分类器时，它让我们能够以略有不同且可能更直观的方式来解释它们的行为。

So that's what I like to call the visual viewpoint of linear classifiers. Because now that we've taken each of these rows of the weight matrix and stretched them out to have the same shape as the image, what we can then do is try to visualize each of the rows of that matrix as an image itself. This interpretation of a linear classifier looks kind of like template matching right because now the classifier is learning one image template per category that we want to recognize.

这就是我称之为线性分类器可视化视角的原因。因为现在我们已经将权重矩阵的每一行拉伸成与图像相同的形状，接下来我们可以尝试将矩阵的每一行本身可视化为图像。这种线性分类器的解释看起来有点像模板匹配，因为现在分类器正在为我们想要识别的每个类别学习一个图像模板。

And each of these templates is then to produce the category score for the template. We simply match up the template for the class with the pixels of the image by computing an inner product between them. You might remember that if you have two vectors that are maybe of unit norm, then they achieve their maximum when they're all lined up, which sort of fits with this idea of template matching.

这些模板中的每一个都会生成该类别的得分分数。我们只需通过计算模板与图像像素之间的内积来将类别模板与图像进行匹配。你可能记得，如果有两个可能是单位范数的向量，当它们完全对齐时，它们的内积会达到最大值，这与模板匹配的概念相吻合。

And now it's really interesting if you then buy by visualizing these templates learned from the classifier as images themselves. You get a bit more intuition about exactly what this linear classifier is looking for in images when it tries to recognize the different categories. So for example on the bottom left you can see that this plane category is maybe looking for some kind of a blob in the middle and it's generally looking for blue images. So any images that have a lot of blue in them are going to be very highly received with very high scores for the plane class using these particular weight matrix for a linear classifier. Similarly the deer class is kind of this green blobby background with kind of a brown.

现在真的很有趣，如果你将通过分类器学习到的这些模板可视化为图像本身，你会对线性分类器在尝试识别不同类别时在图像中寻找什么有更直观的理解。例如在左下角，你可以看到这个飞机类别可能是在寻找中间的某种斑点，并且通常寻找蓝色图像。因此，使用线性分类器的这些特定权重矩阵，任何含有大量蓝色的图像都会为飞机类别获得非常高的分数。类似地，鹿类别是这种带有棕色的绿色斑点背景。

Blob in the middle but it's maybe the deer so that's again gives us some more intuition about what the linear classifier is looking at. And one thing that's kind of interesting from this viewpoint is that it becomes clear that even though we told the classifier that we wanted to recognize object categories like planes and dogs and deer, in fact it's using a lot more evidence from the input image than just the object itself and it's in fact relying very strongly on the context cues from image.

中间的斑点可能是鹿，这再次让我们对线性分类器在观察什么有了更多直观理解。从这个角度来看，一个有趣的事情变得清晰：尽管我们告诉分类器我们想要识别飞机、狗和鹿等物体类别，但实际上它使用的输入图像证据远不止物体本身，而是非常依赖图像中的上下文线索。

So right, if you for example imagined putting in an image that had a car in a forest, that would be kind of confusing for a linear classifier because the forest background might be very green and then would achieve very high scores according to the deer classifier where the car in the middle might match up more to the car template. So in some kind of image with objects in unusual contexts, it would be very likely that a linear classifier would completely fail to properly recognize those objects and that becomes very obvious when you think.

所以，如果你想象输入一张森林中有汽车的图像，这对线性分类器来说会有些困惑，因为森林背景可能非常绿，然后根据鹿分类器会获得非常高的分数，而中间的汽车可能更匹配汽车模板。因此，在具有不寻常背景物体的图像中，线性分类器很可能会完全无法正确识别这些物体，这一点在你思考时会变得非常明显。

About the visual viewpoint of these linear classifiers, so another sort of failure mode of linear classifiers that becomes clear when you think about this visual viewpoint is that of mode splitting. So our linear classifier is only able to learn one template per category, but there's a problem: what happens if we have categories that might appear in different types of ways? As a concrete example, think about horses. So if you go and look at the CIFAR-10 dataset, which maybe you might have done if you started working on the first homework assignment.

从线性分类器的视觉角度来看，当你思考这种视觉视角时，另一种线性分类器的失败模式变得清晰，那就是模式分裂。我们的线性分类器每个类别只能学习一个模板，但存在一个问题：如果我们的类别可能以不同方式出现会发生什么？举个具体例子，想想马。如果你去看CIFAR-10数据集，可能你在开始做第一个作业时已经看过了。

Then you'll see that horses on C part n are sometimes looking to the left and they're sometimes looking to the right and they're sometimes looking dead on. Now if we have horses that are looking in different directions then the visual appearance of the images of horses looking in different directions will be very different. But unfortunately the linear classifier has no way to disentangle its representation and no way to separately learn templates for horses that are looking in different directions.

你会看到C部分中的马有时向左看，有时向右看，有时直视前方。如果我们有朝向不同方向的马，那么这些不同方向马的图像视觉外观会有很大差异。但不幸的是，线性分类器无法解耦其表示，也无法为不同方向的马分别学习模板。

So in fact if you look at this example of a learned template of a horse from this one particular linear classifier, you can kind of see that it actually has two heads. If you look at the horse here, he has kind of a brown blob in the middle and green on the bottom which you might expect, but now there's a black blob on the left and a black blob on the right.

事实上，如果你观察这个特定线性分类器学习到的马模板示例，你会看到它实际上有两个头。如果你看这里的马，中间有一个棕色的斑块，底部是绿色的，这符合预期，但现在左边有一个黑色斑块，右边也有一个黑色斑块。

Which might occur. So then this is the linear classifier trying to do the best that it can to match horses looking in different directions using only a single template that it has the ability to learn. This is also somewhat visible in the car example. You can see that the car template doesn't actually look anything like a car. It just kind of looks like a red blob and a windshield. Again, if the car template might have this funny shape because it's trying to use a single template to cover all possible appearances of cars that you might see in the data set. This also gives us a sense that maybe see if our tent has a lot of red cars because the car template that's learned is red.

这种情况可能会出现。因此，线性分类器只能尽其所能，使用它能够学习的单一模板来匹配不同方向的马。这在汽车示例中也可见一斑。你可以看到汽车模板实际上看起来根本不像汽车，它只是像一个红色斑块和挡风玻璃。同样，汽车模板可能具有这种奇怪的形状，因为它试图使用单一模板来覆盖数据集中可能出现的所有汽车外观。这也让我们意识到，也许是因为我们数据集中有很多红色汽车，所以学习到的汽车模板是红色的。

And maybe if we try to recognize green cars or blue cars then the classifier might fail. All of these type of failure modes become very obvious when you think about the linear classifier from this visual viewpoint. So another third way that we can think about linear classifiers is what I like to call the geometric viewpoint. So here we can imagine drawing a plot where on the x axis we pick out a single pixel in the image.

如果我们尝试识别绿色汽车或蓝色汽车，分类器可能会失败。当你从这个视觉角度思考线性分类器时，所有这些类型的故障模式变得非常明显。所以我们思考线性分类器的第三种方式是我称之为几何视角的方法。这里我们可以想象绘制一个图表，在x轴上我们选取图像中的单个像素。

And now we draw a plot where the x-axis is the value of the pixel and the y-axis is the value of the classifier as that pixel changes. Maybe as we keep all the other pixels in the image fixed, and now because this linear classifier is a linear function, then clearly the classifier score must vary linearly as we change any of the individual values in the image.

现在我们绘制一个图表，其中x轴是像素值，y轴是分类器的值，随着该像素的变化而变化。当我们保持图像中所有其他像素不变时，由于线性分类器是一个线性函数，那么显然当我们改变图像中任何单个像素值时，分类器得分必须线性变化。

So this is not very interesting when you think about this example with only a single pixel, so we can instead try to broaden this viewpoint and incorporate multiple pixels simultaneously. Then we can imagine drawing a plot where the x-axis is maybe one pixel in the image and the y-axis is a second pixel in the image. Because I can't really draw three dimensional plots on PowerPoint, you have to live with some kind of a contour plot. So here then we could draw a line where the car score is equal to one half, and you can see that this level set of the car score forms a line in this pixel space.

所以当你思考这个只涉及单个像素的例子时，这并不十分有趣，因此我们可以尝试扩展这个视角并同时纳入多个像素。然后我们可以想象绘制一个图表，其中x轴可能是图像中的一个像素，y轴是图像中的第二个像素。因为我无法在PowerPoint上真正绘制三维图表，你只能接受某种等高线图。那么在这里我们可以画一条线，其中汽车得分等于二分之一，你可以看到汽车得分的这个等值线在这个像素空间中形成了一条线。

And then the court, because this is linear, the car score will increase linearly along a direction in this pixel space which is orthogonal to this line. Tying this back to the template view, the learned car template will lie somewhere along this line which is orthogonal to the level set of the car score. Similarly, for all the scores for all the different categories that we're trying to recognize, we'll end up having different lines with different level sets and different templates.

由于这是线性的，汽车得分会沿着像素空间中垂直于该直线的方向线性增加。将其与模板视图联系起来，学习到的汽车模板将位于垂直于汽车得分等值线的这条线上。类似地，对于我们试图识别的所有不同类别的所有得分，最终我们将得到具有不同等值线和不同模板的不同直线。

The cart and the template, the learned templates for those categories orthogonal to the level sets in this pixel space. Now of course, looking at only two pixel images like we're doing in this example is not very intuitive, but you can imagine that this viewpoint would extend to higher dimensions as well. So here the idea is that we imagine a linear classifier as taking the whole space of images as this very, very high dimensional Euclidean space, and now within that Euclidean space we have different hyperplanes that are trying to, one hyperplane per category that we want to.

汽车和模板，这些类别的学习模板在像素空间中垂直于等值线。当然，像我们在这个例子中这样只观察两个像素的图像并不十分直观，但你可以想象这个视角也会扩展到更高维度。所以这里的想法是，我们想象一个线性分类器将整个图像空间视为一个非常高维的欧几里得空间，现在在这个欧几里得空间内，我们有不同的超平面试图为每个我们想要分类的类别建立一个超平面。

Recognize each of those hyperplanes for each of the categories we want to recognize. Each of those hyperplanes for each of the categories we want to recognize are now cutting this high dimensional Euclidean space into two half spaces along this level set. That's this third viewpoint on linear classifiers which is of one hyperplane per class cutting up this high dimensional Euclidean space of pixels. This geometric viewpoint is a very useful way to think about linear classifiers.

为我们想要识别的每个类别识别每个超平面。为我们想要识别的每个类别的每个超平面现在正沿着这个等值线将这个高维欧几里得空间切割成两个半空间。这是线性分类器的第三种视角，即每个类别一个超平面，切割这个高维像素欧几里得空间。这种几何视角是思考线性分类器的一个非常有用的方法。

But again I would caution you that geometry gets really weird in high dimensions so we unfortunately are cursed to live in a low dimensional three-dimensional universe so all of our physical intuition about how geometry behaves is really shaped by these very low number of dimensions and that's kind of unfortunate because the way that geometry the Euclidean geometry behaves in very high dimensions can be very non-intuitive to do this the two low dimensions to our low dimensional experience so well I think that this this geometric viewpoint is kind of useful sometimes it's very easy to be led astray by geometric intuition because we happen to have all our intuition built on low.

但我要再次提醒你，几何在高维度中会变得非常奇怪。不幸的是，我们注定生活在低维的三维宇宙中，因此我们对几何行为的所有物理直觉实际上都是由这些极低维度塑造的，这有点不幸，因为欧几里得几何在极高维度中的行为方式可能与我们低维经验中的二维低维度非常不直观。所以我认为这种几何视角有时很有用，但也很容易被几何直觉误导，因为我们所有的直觉都建立在低维基础上。

Dimensional spaces but nevertheless the geometric viewpoint does let us get some other ideas about what kinds of things a linear classifier can and cannot recognize. So then based on this geometric viewpoint we can try to write out different types of cases or different kinds of classification settings that would be difficult or impossible for a linear classifier to properly recognize. So here the idea is that we've colored this two-dimensional pixel space with red and blue corresponding to different categories that we want the classifier to try to recognize.

尽管是维度空间，但几何视角确实让我们对线性分类器能够和无法识别的事物有了其他认识。基于这种几何视角，我们可以尝试列出不同类型的情况或不同种类的分类设置，这些对于线性分类器来说难以或无法正确识别。这里的想法是，我们用红色和蓝色给这个二维像素空间着色，对应我们希望分类器尝试识别的不同类别。

And these are all three cases that are completely impossible for a linear classifier to recognize. So on the left we have this case of red and blue in the first and third quadrants having one category and the second and fourth quadrants being of a different category. And then if you think about it, there's no way that we can draw a single hyperplane that can divide the red and the blue here. So that's a case that is just impossible for linear classifiers to recognize. Another case that's completely impossible for linear classifiers is this case on.

这些都是线性分类器完全无法识别的三种情况。在左侧，我们看到第一和第三象限的红色和蓝色属于一个类别，而第二和第四象限属于不同类别。仔细思考后，我们会发现无法绘制一个单一的超平面来分割这里的红色和蓝色区域。因此这种情况是线性分类器完全无法识别的。线性分类器完全无法识别的另一种情况是右侧的这个案例。

The right which is very interesting of three modes. So here we've got the blue category. There's maybe three distinct patches and parts in regions in pixel space that correspond to possibly different visual appearances of the category we wish to recognize. Then if we have these different disjoint regions in pixel space corresponding to a single category, again you can see there's no way for a single line to perfectly carve up the red and the blue regions.

右侧这个三模态的例子非常有趣。这里我们看到了蓝色类别。在像素空间中可能存在三个不同的斑块和区域部分，对应着我们希望识别的类别可能具有的不同视觉外观。然后，如果这些在像素空间中互不相连的区域对应着同一个类别，你再次可以看到，单条直线无法完美地划分红色和蓝色区域。

So this right example of these three modes is, I think, similar to what we saw in the visual example of maybe the horse looking in different directions. You can imagine maybe in this high dimensional pixel space there's some region of space corresponding to horses looking right and a completely separate region of space corresponding to horses looking in a different direction. And again, with this geometric viewpoint of hyperplanes cutting out high dimensional spaces, it again becomes clear that it's very difficult for a linear classifier to carve up classes that have completely separate modes.

我认为右侧这个三模态的例子，类似于我们在视觉示例中看到的马朝不同方向看的情况。你可以想象，在这个高维像素空间中，可能存在某个区域对应着朝右看的马，而另一个完全独立的区域对应着朝不同方向看的马。再次地，通过超平面切割高维空间的几何视角，我们可以清楚地看到线性分类器很难划分具有完全分离模态的类别。

Of appearance and this also ties back to the historical context that we saw in the first lecture. If you remember in the first lecture last week we talked about this historical context of different types of machine learning algorithms people have built over the years. One of these very first machine learning algorithms that got people very excited was the perceptron. All of a sudden there was this machine that could learn from data, it could learn to recognize digits and characters and got people really excited.

外观问题这也与我们第一堂课中看到的历史背景相关联。如果你还记得上周的第一堂课，我们讨论了人们多年来构建的不同类型机器学习算法的历史背景。其中最早让人们对感到兴奋的机器学习算法之一是感知机。突然间出现了这样一台能够从数据中学习的机器，它能够识别数字和字符，这让人们非常兴奋。

But it had this, but now if we were to, if you were to look back at the exact math of the perceptron now we would recognize it as a linear classifier. And because the perceptron was a linear classifier, there's a lot of things that it was just fundamentally unable to recognize. The most famous example was the XOR function which is shown here, which where we have the green as one category and the blue is a different category. So because the linear, because the perceptron was a linear model, there was no way that it could carve up these input, these red and green regions.

但它有这样的问题，但现在如果我们回顾感知机的具体数学原理，我们会认识到它是一个线性分类器。由于感知机是一个线性分类器，有很多东西它从根本上就无法识别。最著名的例子就是这里展示的异或函数，其中绿色代表一个类别，蓝色代表另一个类别。由于感知机是一个线性模型，它无法用一条直线划分这些红色和绿色区域。

Red and sorry green and blue regions with a single line and therefore there was no way that the perceptron could learn the XOR function. So that's kind of a nice bit of historical context about why the geometric viewpoint was historically useful for having people think about how machine learning algorithms could operate. So now to this point we've talked about linear classifiers as this fairly simple model of a matrix vector multiply and we've seen how even though this is a fairly simple equation to write now if you unpack it and think about it in different ways some of the shortcomings of its representation abilities become clearer as we think about it from these different viewpoints.

无法用一条直线划分这些红色和绿色区域，因此感知机无法学习异或函数。这是一个很好的历史背景，说明了为什么几何视角在历史上对人们思考机器学习算法如何运作很有帮助。到目前为止，我们已经讨论了线性分类器作为矩阵向量相乘的相当简单的模型，并且我们已经看到，尽管这是一个相当简单的方程，但当你从不同角度解构和思考它时，其表示能力的一些缺点就会变得更加清晰。

So is there any questions about these different viewpoints of linear classifiers so far? Okay, so then basically where we are now is that once we have a linear classifier we're able to predict scores right given any value of the weight matrix W we can perform this matrix vector multiply on an input image to now spit out a vector of scores for the classes that we want to carry.

到目前为止，关于线性分类器的这些不同视角，大家有什么问题吗？好的，那么我们现在的处境是：一旦我们有了一个线性分类器，我们就能够预测分数，给定权重矩阵W的任何值，我们可以在输入图像上执行这个矩阵向量乘法，从而为我们想要处理的类别输出一个分数向量。

That we want to recognize so as an example here we've got three images and ten categories for CIFAR-10. So for any particular value of the weight matrix W we can run the classifier and get these vectors of scores but this has told us nothing about how we actually select the weight matrix W and we've not said anything about the learning process by which this matrix W is selected or learned from data.

我们想要识别的类别，举个例子，这里我们有三个图像和CIFAR-10的十个类别。因此对于权重矩阵W的任何特定值，我们可以运行分类器并获得这些分数向量，但这并没有告诉我们如何实际选择权重矩阵W，我们也没有讨论过这个矩阵W是如何从数据中选择或学习的学习过程。

So that so now in order to actually write down linear and actually in order to actually implement linear classifiers we need to talk about two more things. One is we need to use the idea of a loss function to quantify how good any particular value of W is and that's what we'll talk about for the rest of this lecture. And then in the next lecture we'll talk about optimization which is the process by which we try to search using our training data to search over all possible values of W and arrive at one that works quite well for our data. So a little bit more formally a loss function is some way to tell how good our.

因此，为了实际编写并实现线性分类器，我们需要讨论另外两个问题。首先，我们需要利用损失函数的概念来量化任意特定W值的好坏程度，这将是本讲剩余部分要讨论的内容。然后在下一讲中，我们将讨论优化过程，即如何利用训练数据在所有可能的W值中进行搜索，最终找到一个对我们的数据效果良好的W值。更正式地说，损失函数是某种能够衡量我们的分类器在数据上表现好坏的方法。

Classifier is doing on our data with the interpretation that a high loss means we're doing bad and a low loss means that we're doing good and the goal the goal whole goal of machine learning is to write down loss both well okay that's a little bit reductive but one way to one way that we can think about ma'sha'allah - Murel network systems is writing down loss functions that try to capture intuitive ideas about what types of models are good or when models are working well and when models are not working well.

分类器在我们的数据上的表现可以这样理解：高损失意味着表现不佳，低损失意味着表现良好。机器学习的目标就是恰当地定义损失函数，好吧，这样说可能有点简化，但我们可以这样理解ma'sha'allah - Murel网络系统：通过编写损失函数来捕捉关于哪些模型类型是好的、模型何时表现良好以及何时表现不佳的直观概念。

And then once we have this quantitative way to evaluate models, we try to find models that perform well. As a bit of terminology, this term of a loss function will also sometimes be called an objective function or a cost function in other literature. Because people can never agree on names, sometimes people will talk about the negative of a loss function instead. So what loss function is something you want to minimize, sometimes people want to maximize something instead, and it's the thing we care about.

然后，一旦我们有了这种定量评估模型的方法，我们就会尝试寻找表现良好的模型。作为术语说明，损失函数在其他文献中有时也被称为目标函数或成本函数。由于人们永远无法在命名上达成一致，有时人们会讨论损失函数的负数形式。因此损失函数是你想要最小化的东西，而有时人们想要最大化某些东西，这才是我们关心的内容。

If that if we want to right now in our model by maximizing a function then it'll typically be called something like a reward function, profit function, utility function, or fitness function. Each subfield has their own names and bits of terminology but they're all the same idea. It's just a way to quantify what your model is doing well and when your model is not doing well. Then a bit more formally, the way that we'll usually think about this is we have some data set of examples where each input is a vector X and each output is a label Y.

如果在我们当前的模型中想要最大化某个函数，那么它通常会被称作奖励函数、利润函数、效用函数或适应度函数等。每个子领域都有各自的名称和术语，但它们都是相同的概念。这只是一种量化模型表现好坏的方法。更正式地说，我们通常这样理解：我们有一个包含多个样本的数据集，其中每个输入是一个向量X，每个输出是一个标签Y。

In the image classification case, X will be these images of fixed size and Y will be an integer giving the label. It will be an integer indexing into the categories that we care to recognize. Now the loss for a single example will often be written as Li, and it will take in f of Xi and W, which will be the predictions of our model on a data point Xi. The loss function will then assign a score of badness between the prediction and the ground truth or true label Yi.

在图像分类的情况下，X将是这些固定尺寸的图像，Y将是一个整数，表示标签。它将是一个整数，索引到我们关心的识别类别中。现在，单个样本的损失通常写作Li，它将输入f(Xi, W)，这是我们的模型在数据点Xi上的预测。损失函数随后会在预测值与真实值或真实标签Yi之间分配一个不良程度的评分。

And then the loss over the entire data set will simply be the average of all the losses of the individual examples in the data set. So then this is kind of the idea of a loss function in the abstract and the first concrete loss function. And then you can imagine that as we try to tackle different tasks in machine learning, we need to write down different types of loss functions for each different task that we want to try to solve. And even when we're focused on a single task, we can often write down different.

整个数据集的损失将简单地是数据集中所有单个样本损失的平均值。这就是损失函数在抽象层面以及第一个具体损失函数的概念。可以想象，当我们尝试处理机器学习中的不同任务时，我们需要为每个想要解决的不同任务写下不同类型的损失函数。即使我们专注于单个任务，也经常可以写出不同的。

Types of loss functions that encapsulate different types of preferences over when models are going to be good and when models are going to be bad. So as a first example of a loss function, I want to talk about the multi-class SVM loss for the infer image classification or really for classification more generally. So here the idea of the multi-class SVM loss is quite intuitive. What it basically says is that the score of the correct class should be a lot higher than all of the scores assigns to all of the incorrect classes right.

能够体现模型在不同情况下表现优劣偏好的各类损失函数。作为损失函数的第一个示例，我想讨论多类SVM损失，它适用于图像分类或更广义的分类任务。多类SVM损失的概念相当直观，其核心思想是正确类别的得分应当远高于所有错误类别所获的得分。

That's kind of an intuitive statement that if we want to use this classifier to actually predict and recognize images, then at the end of the day we don't care about the predicted scores. We want to assign a single label to each image that we want to classify. In order to do that, it seems reasonable that we want our classifier to assign high scores to the right category and low scores to all the other categories. Now the multi-class SVM loss is one particular way to make this intuition concrete.

这是一个直观的陈述：如果我们想使用这个分类器来实际预测和识别图像，那么最终我们并不关心预测得分。我们希望为每个要分类的图像分配一个单一标签。为了实现这一点，我们希望分类器为正确类别分配高分，为所有其他类别分配低分，这似乎是合理的。现在，多类SVM损失是将这种直觉具体化的一种特定方法。

So what exactly the multi-class SVM loss computes is that we can draw a plot here where the x axis is going to be the score for the correct class for the example we're considering and the y axis will be the loss for that individual data point that we're trying to classify. Then in addition to keeping track of the score of the correct class, we also want to keep track of the highest score assigned to all other categories that we care to recognize.

多类SVM损失的具体计算方式是：我们可以在此绘制一个图表，其中x轴代表我们正在考虑样本的正确类别得分，y轴代表我们试图分类的单个数据点的损失值。除了跟踪正确类别的得分外，我们还需要跟踪所有其他待识别类别中的最高得分。

So maybe if we were classifying an image whose correct class is cat, then the x-axis would be cat score and then this particular dot would be the highest score assigned to all of the other categories in the classifier. Then the multi-class SVM loss looks like the following: it's going to decrease linearly and once the score of the correct class is more than some margin over the second highest score among all the incorrect classes, well that will give us zero loss.

假设我们正在对一张正确类别为猫的图像进行分类，那么x轴将代表猫类别的得分，而这个特定点将代表分类器中所有其他类别被分配的最高得分。多类SVM损失的表现如下：它会线性递减，一旦正确类别的得分超过所有错误类别中第二高得分某个边界值时，损失值就会归零。

And I'll call that low loss means of a good classifier. Then moving to the left you can see that as the score for the correct class becomes close or even higher than the score to the highest incorrect class, then the loss we assigned to that example will increase linearly. This type of loss function that has a general shape of kind of a linear region and then a zero region comes up a lot in different contexts in machine learning. This is often called a hinge loss because it looks kind of like a door hinge that can open and close.

我将其称为低损失意味着一个好的分类器。然后向左移动可以看到，当正确类别的得分接近甚至高于最高错误类别的得分时，我们分配给该样本的损失将线性增加。这种具有线性区域和零区域一般形状的损失函数在机器学习的不同情境中经常出现。这通常被称为铰链损失，因为它看起来有点像可以开关的门铰链。

This type of loss function with a linear region and then a zero region comes up a lot in different contexts in machine learning and this is often called a hinge loss because it looks kind of like a door hinge that can open and close. So we can write down the same intuition mathematically like the following: given a single data example X I image and Y I label, then the SVM loss has the form where we sum over each of the category labels not including the correct label Y I. The sum here goes over all category labels but excludes the correct class and takes the max of 0 and the class we're looping over minus the correct class plus 1.

这种具有线性区域和零区域的损失函数在机器学习的不同情境中经常出现，通常被称为铰链损失，因为它看起来有点像可以开关的门铰链。我们可以将相同的直觉用数学方式表达如下：给定单个数据样本X I图像和Y I标签，SVM损失的形式是对不包括正确标签Y I在内的每个类别标签进行求和。这里的求和遍历所有类别标签但排除正确类别，并取0与当前循环类别减去正确类别加1的最大值。

And if you kind of think about the different cases about what can be higher and what can be lower, you can see that this corresponds to on the right corresponds to two cases. One is that if the correct class is more than one greater than the incorrect class, then we achieve a loss of 0 for that class right. So basically what this is saying is that we're summing over all the classes that we want to recognize and we're going to assign a sort of a mini loss per class per incorrect category.

如果你思考关于什么可能更高、什么可能更低的不同情况，你会看到这对应于右侧的两种情况。一种是如果正确类别比错误类别高出超过1分，那么该类别对应的损失为0。所以这基本上是在说，我们对所有想要识别的类别进行求和，并为每个错误类别分配一个小型损失。

And now if the incorrect category is greater than 1 less than the correct class, then we take some loss. Whereas if the correct class is more than 1 greater than the incorrect class, then we get 0 loss for that class example pair. Then we loop over all the other classes that we care to recognize. So because that's a little bit hard to wrap your head around, we can look at a more concrete example.

现在如果错误类别比正确类别高出超过1分，那么我们会产生一些损失。而如果正确类别比错误类别高出超过1分，那么该类别样本对的损失为0。然后我们遍历所有其他需要识别的类别。因为这有点难以理解，我们可以看一个更具体的例子。

So here we're imagining a data set of three images. Hopefully you can recognize as expert human visualizers that these are cats, cars and frogs. Now we're imagining some particular setting of the weight matrix W that causes our classifier to spit out these scores for these images. Given these scores and these images, we can compute the SVM loss as follows. First we want in order to compute the loss for the cat example, then we need to loop over all the incorrect classes of all the incorrect categories so we skip the cat category.

这里我们想象一个包含三张图像的数据集。希望作为专业的人类视觉识别专家，你能认出这些是猫、汽车和青蛙。现在我们想象权重矩阵W的某个特定设置，导致我们的分类器为这些图像输出这些分数。给定这些分数和这些图像，我们可以按如下方式计算SVM损失。首先为了计算猫样本的损失，我们需要遍历所有错误类别的所有错误类别，因此我们跳过猫类别。

And now for the car category we compute max of zero five point one is the car score minus 3.2 is the cat score plus one is the margin and that gives us a score for that thing of 2.9. And now for the car category we see that then we see that cat is more than 1 greater than frog then the Frog score so then we achieve zero loss for the for the crab for the category of frog and the overall loss for the cat example is 2 for this cat image is 2.9.

现在对于汽车类别，我们计算最大值：0，汽车分数5.1减去猫分数3.2加上边界值1，得到该项分数为2.9。对于汽车类别，我们看到猫比青蛙高出超过1分，因此青蛙类别获得零损失，这个猫样本的总体损失为2.9。

We can do something similar for the car image. Here because the correct category of this image is car and the score we're currently assigning to it is 4.9, and 4.9 is more than one greater than all of the scores assigned to the incorrect categories, we achieve a loss of zero for this example. You can imagine doing the same computation for the frog example. Here we get a lot of loss because we've assigned a very low score to the frog category.

我们可以对汽车图像进行类似处理。由于该图像的正确类别是汽车，我们当前分配给它的分数是4.9，而4.9比分配给错误类别的所有分数都高出超过1分，因此这个样本的损失为零。你可以想象对青蛙样本进行相同的计算。这里我们会得到很大的损失，因为我们给青蛙类别分配了非常低的分数。

And then to compute the loss over the full data set we just take an average over the loss over the examples. So now a couple questions first think about what happens if the loss what happens to the to this loss if the if some of the predicted scores for the car image were to change a little bit well in this case because the car image is achieving zero loss overall if we a met and and the predicted car score it's a lot greater than any of the other scores assigned to the incorrect classes you can see that if we were to

然后计算整个数据集的损失，我们只需对样本损失取平均值。现在有几个问题：首先考虑如果汽车图像的某些预测分数发生微小变化，损失会如何变化？在这种情况下，由于汽车图像总体实现了零损失，如果我们满足条件且预测的汽车分数远高于分配给错误类别的任何其他分数，我们可以看到如果我们要

If we change the predicted scores of this example by a little bit, then we would still achieve zero loss. That's one interesting property of the multi-class SVM loss: once an example is correctly classified, then changing the predicted scores of that example just a little bit doesn't really affect the loss anymore. So another question is: what's the maximum and minimum possible values for this loss on a single example? The minimum loss is zero.

如果我们稍微改变这个样本的预测分数，我们仍然会得到零损失。这是多类SVM损失的一个有趣特性：一旦一个样本被正确分类，那么稍微改变该样本的预测分数就不会再影响损失了。那么另一个问题是：对于单个样本，这个损失的最大值和最小值可能是什么？最小损失是零。

So we achieved the minimum loss when the correct category has a score much higher than all the incorrect categories and the maximum loss is infinite and that happens when the correct category has a very very low score that's much smaller than all the other predicted scores. So then another question: if all of the scores if we had a linear classifier that was randomly initialized, the weight matrix has not been learned at all then and if the values of the weight matrix are all small random values then maybe we would expect at initialization when we first start the

当正确类别的分数远高于所有错误类别时，我们实现了最小损失，而最大损失是无限的，这种情况发生在正确类别的分数非常非常低，远小于所有其他预测分数时。那么另一个问题是：如果我们有一个随机初始化的线性分类器，权重矩阵完全未经学习，且所有权重矩阵的值都是小的随机值，那么在我们刚开始初始化时，我们可能会预期

Learning process that all of the predicted scores for the linear classifier would also be small random values for each of the categories. So in this case if all of the predicted scores are small random values, that approximately what loss would we expect to see from the SVM classifier? I heard they heard zero, that's actually not correct. Small? So when I say it okay maybe this was not a bit not very precise, so maybe that was my fault for asking an imprecise question. But maybe if all of this so maybe.

学习过程中，线性分类器的所有预测分数对于每个类别来说也都是小的随机值。在这种情况下，如果所有预测分数都是小的随机值，我们预期从SVM分类器中看到大约多少损失？我听到有人说零，这实际上是不正确的。小？所以当我说好吧，可能这有点不太精确，所以可能是我问了一个不精确的问题的错。但也许如果所有这些，所以也许。

If we're going to draw on each of the scores from some Gaussian distribution with maybe a standard deviation of like 0.001 something very very small, then in that case if all of the predicted scores would then be small random values so then the expected difference between the correct category and any of the incorrect categories would be approximately zero. So then if you imagine turning through this loss computation we would get like small value minus small value is approximately zero and then this overall and then plus 1 would give max of 0 & 1 so then we would achieve a loss of.

如果我们从某个高斯分布中抽取每个分数，可能标准差约为0.001这样非常小的值，那么在这种情况下，如果所有预测分数都是小的随机值，那么正确类别与任何错误类别之间的预期差异将近似为零。因此，如果你想象通过这个损失计算过程，我们会得到小值减去小值约等于零，然后整体加上1会得到0和1的最大值，这样我们就会实现一个损失值。

1 / incorrect category which and again because this sum is looping over all the incorrect categories then in this case we would expect to see a loss of approximately C minus 1 where C is the number of categories that we're trying to recognize. Now this might seem like kind of a stupid question to ask but it's actually a really useful debugging technique whenever you're implementing a neural network or other kind of learning based system you should think about what type of loss do you expect to see if all of the scores.

1/错误类别，并且由于这个求和是在所有错误类别上循环，那么在这种情况下，我们预期会看到大约C减1的损失，其中C是我们试图识别的类别数量。现在这可能看起来像是一个愚蠢的问题，但实际上这是一个非常有用的调试技巧，当你实现神经网络或其他基于学习的系统时，你应该思考如果所有分数都是小的随机值，你预期会看到什么样的损失。

Are approximately random and then when you start training your system if you actually see a loss which is very different from what you expect then probably have a bug somewhere so this might have seemed like a contrived question but it's actually a very useful debugging technique to go through this exercise of thinking about what kind of loss would you expect to see with small random values whenever you go and implement a new loss function or start training the new loss function.

如果所有分数都大致是随机值，那么当你开始训练系统时，如果实际看到的损失与你预期的相差甚远，那么很可能某处存在错误。这看起来可能像是个刻意设计的问题，但实际上这是个非常有用的调试技巧——每当你实施新的损失函数或开始训练新损失函数时，通过这个思考练习来预期在小随机值情况下会看到何种损失。

So then another question is that we saw in this formulation of the SVM loss that we're summing over all of the incorrect categories only. So what would happen if we were to sum over all of the categories including the correct category? Would this represent the same preference over classifiers or would this represent some other type of preference over weight matrices? Well in this case, we would just expect all of the scores to be inflated by one right because this would be adding an extra term to the sum.

那么另一个问题是，我们在SVM损失的公式中看到，我们只对所有错误类别进行求和。那么如果我们要对所有类别（包括正确类别）进行求和会发生什么？这表示对分类器的相同偏好，还是表示对权重矩阵的其他类型偏好？在这种情况下，我们预期所有分数都会增加1，因为这会向求和添加一个额外的项。

Sorry okay yes yes. Then we would then we expect all the predicted losses to just go up by a constant one because we add an extra value to the sum which was syi minus syi which would be zero plus one x is 0 1 1 is 1 so we just add 1 to all the losses. So this would express the same preference over classifiers because all the losses would be inflated by a value of 1 but the relative assignment would not change our order about whether we would prefer one weight matrix over another.

抱歉好的，是的。那么我们会预期所有预测损失都会增加一个常数1，因为我们在求和中添加了一个额外值，即syi减去syi本应为零，加上1乘以0等于1，所以我们在所有损失上都加1。因此这将表示对分类器的相同偏好，因为所有损失都会增加1，但相对分配不会改变我们对权重矩阵的偏好顺序。

All over and over because all the losses would just be inflated by one. It's done another question. What would happen if, rather than using a sum, we used a mean over categories instead of a sum? So here then all of them, all of the computed losses, would just be multiplied by a factor of 1 over C minus 1 and again, because that's a monotonic transform, this would express the exact same preference over weight matrices. So the values of loss would change that we see when we're training, but exactly the preference over weight matrices would.

一遍又一遍，因为所有损失只会增加1。这引出了另一个问题。如果我们不使用求和，而是使用类别的平均值代替求和，会发生什么？那么这里所有的计算损失只会乘以1除以C减1的因子，同样因为这是一个单调变换，这将表示对权重矩阵的完全相同的偏好。所以我们在训练时看到的损失值会改变，但对权重矩阵的偏好完全会。

So another question: what if we use some other type of formulation? What if we took a square? What if we put a square over this max value? This would now express quite a different preference. This would actually be quite different. So this would change all of the scores in a nonlinear way, and this would cause the preference over weight matrices that we're expressing with our loss function to change in a non-trivial way. So this would no longer be called this. You can no longer call this a multi-class SVM loss.

那么另一个问题：如果我们使用其他类型的公式会怎样？如果我们取平方呢？如果我们在这个最大值上加平方呢？这将表示相当不同的偏好。这实际上会非常不同。因此这将以非线性方式改变所有分数，这将导致我们通过损失函数表达的权重矩阵偏好发生显著变化。因此这不再能被称为多类SVM损失。

Because this would now be expressing a different set of preferences over our weight matrices. So then now another question: what happened if we found some weight matrix W that caused the overall SVM loss to be zero? If we happen to find such an example with zero loss, would it be unique? So here it would not be right because if we would take our weight matrix and multiply all by two, then we would still get a loss of zero.

因为这现在将表达对我们权重矩阵的不同偏好。那么现在另一个问题：如果我们找到某个使整体SVM损失为零的权重矩阵W会发生什么？如果我们恰好找到这样一个零损失的例子，它会是唯一的吗？这里答案是否定的，因为如果我们将权重矩阵全部乘以二，我们仍然会得到零损失。

And we can see that by working through one of these examples that if the loss was zero that meant that the score for the correct category was more than one greater than all the scores for the incorrect categories. So if we multiply the weight matrix by two then all of the predicted scores will also go up by a factor of two because the classifier is linear, which will mean that now all of the predicted scores for the correct categories will be more than two greater than all of.

通过分析这些例子我们可以看到，如果损失为零，意味着正确类别的分数比所有错误类别的分数高出至少一。因此如果我们将权重矩阵乘以二，由于分类器是线性的，所有预测分数也会增加两倍，这意味着现在所有正确类别的预测分数将比所有错误类别的分数高出至少两倍。

The scores of the incorrect categories so we'll still be over the margin and we'll still get zero loss. So now that leads to an interesting question: now that it's possible that we can have two different weight matrices that achieve the exact same loss, then how can we possibly express preferences over these weight matrices? Because in this case we found two different weight matrices that achieve the same loss on the training data, so in order to distinguish them we need some other mechanism.

错误类别的分数仍然会超过边界，我们仍然会得到零损失。这就引出了一个有趣的问题：既然我们可能有两个不同的权重矩阵实现完全相同的损失，那么我们如何表达对这些权重矩阵的偏好呢？因为在这种情况下，我们发现了两个在训练数据上实现相同损失的不同权重矩阵，所以为了区分它们，我们需要其他机制。

Additional mechanism beyond the training set laws in order to express our preference or preferences over classifiers. So this is an idea, this is one idea called regularization. So regularization is something, some piece that you add to the objective function or the overall learning objective that is fighting against the training data, what is performing well on the training data. So far we've seen this overall loss as the average loss of all the examples on the training set. So this is usually called the data loss which is somehow measuring how good are the models predictions on the training data and it's very common to add an.

除了训练集规则之外的额外机制，用于表达我们对分类器的偏好。这就是一个概念，一个称为正则化的概念。正则化是你添加到目标函数或整体学习目标中的某个部分，它与训练数据相抗衡，与在训练数据上表现良好的部分相抗衡。到目前为止，我们将整体损失视为训练集上所有样本的平均损失。这通常称为数据损失，它在某种程度上衡量模型在训练数据上的预测效果，并且通常会在其中添加一个。

Additional term to our overall loss function that does something else that might not depend on the data. This is called a regularization term that serves a couple different purposes: one is to prevent model, one is to express preference. So here the second term is called a regularization term and you'll see that it does not involve the training data. This is meant to prevent the model from doing too well on the training data, basically to give the model something else to do other than just try to fit the training.

在我们整体损失函数中添加的额外项，它执行其他可能不依赖于数据的操作。这被称为正则化项，它有几个不同的目的：一个是防止模型过拟合，一个是表达偏好。所以这里的第二项被称为正则化项，你会看到它不涉及训练数据。这是为了防止模型在训练数据上表现得太好，基本上给模型提供除了仅仅尝试拟合训练数据之外的其他任务。

Data and here these different types of regularization will often come with some kind of hyper parameter usually called lambda in terms of regular for regularizer that will be some hyper parameter controlling the trade-off between how well the model is supposed to fit the data versus how well is the model supposed to achieve this regularization loss. So a couple very common examples of regularization that are typically used for linear models are L2 regularization which is the overall norm of the weight matrix W.

数据在这里，这些不同类型的正则化通常会带有某种超参数，通常称为lambda，用于正则化器，这将是一个超参数，控制模型应该对数据的拟合程度与模型应该达到的正则化损失之间的权衡。所以一些通常用于线性模型的非常常见的正则化例子包括L2正则化，它是权重矩阵W的整体范数。

The L1 regularizer, and we can sometimes use an L1 regularizer which is the sum of the absolute values of all the elements in this weight matrix W. Sometimes you'll see what's called an elastic net in statistics literature which is a combination of the L1 and L2 regularizers. So all of these types of regularizers will also be used in neural networks, but as we move to neural network models we'll also see other types of regularizers such as dropout, batch normalization, and more recent things like cut out and mix up, stochastic gap.

L1正则化器，我们有时可以使用L1正则化器，它是权重矩阵W中所有元素绝对值的总和。有时在统计学文献中会看到所谓的弹性网络，它是L1和L2正则化器的组合。所有这些类型的正则化器也将在神经网络中使用，但随着我们转向神经网络模型，我们还会看到其他类型的正则化器，如dropout、批量归一化，以及更近期的技术如cut out和mix up、随机间隙。

There are a lot of interesting regularizers that people use for neural networks, but the basic idea of why we might want to use regularizers is somehow threefold in my thinking. One is that adding some additional term to the loss beyond the data loss allows us to express our preferences over different types of models when those different types of models are not distinguished by their training accuracy, and this can sometimes be a way that we can inject some of our own human prior knowledge into the types of classifiers that we would like to learn.

人们为神经网络使用了许多有趣的正则化器，但在我看来，我们可能想要使用正则化器的基本原因有三方面。一是在数据损失之外向损失函数添加额外项，使我们能够在不同类型模型无法通过训练准确率区分时表达我们对这些模型的偏好，这有时可以成为将我们自己的人类先验知识注入我们希望学习的分类器类型的一种方式。

A second is to avoid what we call overfitting. So overfitting is a bad problem in machine learning. This happens when you build a model that works really really well on your training data but it actually performs very poorly on unseen data. And this is here this is a point where machine learning is quite distinct from something like optimization right in optimization we typically have an objective function and our whole goal is just to find the bottom of the objective function but in machine learning we often don't really want to do that at all because the end of the day we want.

第二个原因是为了避免我们所说的过拟合。过拟合是机器学习中一个严重的问题。当你构建的模型在训练数据上表现非常好，但在未见过的数据上实际表现很差时，就会发生这种情况。这一点正是机器学习与优化等领域的显著区别所在——在优化中，我们通常有一个目标函数，我们的全部目标就是找到目标函数的最低点，但在机器学习中，我们往往根本不想这样做，因为最终我们想要。

To build a system that performs well on unseen data, so finding a model that gets the best possible performance on the training data might be working actually against us in some ways and might result in models that do not work well on unseen data. And then there's another kind of technical bit is that if we're using gradient-based optimizers then adding an extra term of adding this extra regularization term can sort of add extra curvature to the overall objective landscape and that can maybe sometimes help the optimization process.

构建一个在未见数据上表现良好的系统，因此寻找在训练数据上获得最佳性能的模型实际上可能在某些方面对我们不利，并可能导致模型在未见数据上表现不佳。另一个技术细节是，如果我们使用基于梯度的优化器，那么添加这个额外的正则化项可以为整体目标地形增加额外的曲率，这可能有时有助于优化过程。

So the idea of regularization is that we can express preferences over different types of classifiers that we want a model to learn. Here's an example where we have an input vector X that has all ones, and now we consider two different weight matrices W1 and W2. Now imagine that we're in some kind of linear classification or linear regression setting, then the prediction of a linear model with this input X and either of these two weight matrices will be one, because the inner product of the vector X and either of these two matrices is 1.

正则化的核心思想是，我们可以对我们希望模型学习的不同类型分类器表达偏好。这里有一个例子：我们有一个全为1的输入向量X，现在我们考虑两个不同的权重矩阵W1和W2。假设我们处于某种线性分类或线性回归场景中，那么使用这个输入X和任意一个权重矩阵的线性模型预测结果都将为1，因为向量X与这两个矩阵中任意一个的内积都是1。

Which means that if we were solely going by something like a data loss then the loss would have no way to distinguish these two different values of the weight matrix and they would be preferred equally. But if we were to add an L2 regularization term to this model and to our loss function then this allows us to express an additional preference to tell the model which of these two we would prefer.

这意味着如果我们仅依赖数据损失之类的指标，那么损失函数将无法区分这两个不同的权重矩阵值，它们将被同等偏好。但如果我们在模型和损失函数中添加L2正则化项，这就能让我们表达额外偏好，告诉模型我们更倾向于这两个中的哪一个。

The vector X and either of these two matrices is 1, which means that if we were solely going by something like a data loss then the loss would have no way to distinguish these two different values of the weight matrix and they would be preferred equally. But if we were to add an L2 regularization term to this model and to our loss function then this allows us to express an additional preference to tell the model which of these two we would prefer. So here we add this L2 regularization term then we see that if you imagine computing the L2 norm of the W1 vector then its L2 norm is 1 whereas the L2 norm of the second vector is 1/4 squared is 1/16 and we got four of those so the overall L2 norm is 1/4. So the weight matrix W2 would be preferred if we add in this L2 regularization and what's and here this is very interesting right because what this is one way to think about what an L2 regularizer is.

向量X与这两个矩阵中任意一个的内积都是1，这意味着如果我们仅依赖数据损失之类的指标，那么损失函数将无法区分这两个不同的权重矩阵值，它们将被同等偏好。但如果我们在模型和损失函数中添加L2正则化项，这就能让我们表达额外偏好，告诉模型我们更倾向于这两个中的哪一个。因此当我们添加这个L2正则化项后，可以看到计算W1向量的L2范数结果为1，而第二个向量的L2范数是1/4的平方等于1/16，由于有四个这样的值，所以总体L2范数为1/4。因此如果我们加入这个L2正则化，权重矩阵W2会更受青睐，这一点非常有趣，因为这是理解L2正则化器的一种方式。

When you have two different options that compute the same value on the input, you could either choose to spread out your weight matrix to use all of the available input features, or you could concentrate all of your weights on exactly one input feature. When you're using an L2 regularizer, you're giving the model this extra hint that you would prefer that it used all available features where possible, even if using a single feature would have achieved the same result.

当你在输入上计算相同值的两种不同选择时，你可以选择分散权重矩阵以使用所有可用输入特征，或者将所有权重集中在一个输入特征上。当你使用L2正则化器时，你实际上是在给模型一个额外提示：你更希望它尽可能使用所有可用特征，即使使用单一特征也能达到相同结果。

So this could be useful maybe if you believe that individual features might be noisy and that you have maybe a lot of features that all could be correlated and you want to tell the model to use all of available features. Something like L1 regularization tends to express the opposite preference where in L1 regularization it tells the model to prefer to put all of your weight on a single feature where it possible. So it's kind of interesting that these different regularizers allow us to give the model extra hints about what types of classifiers we'd like them to learn.

这可能很有用，如果你认为单个特征可能存在噪声，并且你有很多可能相互关联的特征，你希望告诉模型使用所有可用特征。像L1正则化则倾向于表达相反的偏好，在L1正则化中，它告诉模型尽可能将所有权重集中在单个特征上。因此有趣的是，这些不同的正则化器让我们能够给模型提供额外提示，告诉它们我们希望学习什么类型的分类器。

That is completely separate from their performance on the training data. I said the second really interesting piece of regularization is to prefer simpler models in order to avoid overfitting. Here we can imagine we're building some model that is receiving a scalar input X and is predicting a scalar output Y. We've got some noisy training data well specified by these blue points. We could imagine fitting two different models to this training data. Maybe the model f1 is this blue curve that goes and perfectly fits all of the training points.

这与它们在训练数据上的表现完全无关。我说正则化的第二个真正有趣之处在于倾向于更简单的模型以避免过拟合。这里我们可以想象我们正在构建一个接收标量输入X并预测标量输出Y的模型。我们有一些由这些蓝点很好指定的噪声训练数据。我们可以想象用两个不同的模型来拟合这些训练数据。也许模型f1是这条蓝色曲线，它完美地拟合了所有训练点。

Whereas the model f2 is this green curve that does not perfectly fit all the training points, but somehow the model f2 curve is simpler because it's a line and not a big wiggly polynomial. So it might be that given our human intuition about the problem, we might have reason to believe that a line might be a more generalizable solution to the task at hand. Indeed, if we were to imagine collecting a couple more data points that are also kind of noisy data points that fall roughly along a line.

而模型f2是这条绿色曲线，它不能完美拟合所有训练点，但模型f2曲线更简单，因为它是一条直线而不是一个大幅波动的多项式。因此根据我们对问题的人类直觉，我们可能有理由相信直线可能是当前任务更具泛化性的解决方案。确实，如果我们想象再收集几个同样有噪声且大致沿直线分布的数据点。

Then you can see that the blue curve f1 might achieve very bad predictions on unseen data while the simpler green curve f2 might achieve better predictions on unseen data. Of course I need to point out that we've been talking about linear models and people always complain that this slide has a model definitely not linear on it, so it's just a cartoon to express the idea of preferring simpler models with regularization. The kind of takeaway here is that regularization is really important when you're building machine learning systems and that you should basically always.

那么你可以看到蓝色曲线f1可能对未见数据做出非常糟糕的预测，而更简单的绿色曲线f2可能对未见数据做出更好的预测。当然我需要指出我们一直在讨论线性模型，人们总是抱怨这张幻灯片上的模型绝对不是线性的，所以这只是一个卡通图来表达通过正则化偏好更简单模型的概念。这里的要点是正则化在构建机器学习系统时非常重要，你应该基本上总是。

Incorporate some form of regularization into whatever machine learning system you're trying to build. So here now we've seen this idea of a linear classifier, we've seen the notion of a loss function with it, we saw a concrete example of the loss function being via the multi-class SVM loss, and now we've talked about regularization as a way to prefer one type of classifier over another. Well another way that you can tell the model how you can give the model your preferences about the types of functions you'd like it to learn is by using different types.

在你尝试构建的任何机器学习系统中都应加入某种形式的正则化。至此我们已经了解了线性分类器的概念，认识了损失函数的概念，通过多类SVM损失看到了损失函数的具体示例，并且讨论了正则化作为偏好某类分类器的方法。另一种你可以向模型传达你希望它学习何种函数偏好的方式是通过使用不同类型的。

So far we have seen the multi-class SVM loss, but another very commonly used loss and perhaps the most commonly used loss when training neural networks is the so-called cross-entropy loss or multinomial logistic regression. This comes by a lot of names, so you will see a lot of names for this, but it all means the same thing. Here the intuition is that we would like to remember that so far we have not really given much interpretation to the scores that are being spit out by our linear model.

到目前为止我们已经了解了多类SVM损失，但另一个非常常用、或许在训练神经网络时最常用的损失是所谓的交叉熵损失或多类逻辑回归。它有很多不同的名称，所以你会看到很多叫法，但都指的是同一个概念。这里的直观理解是，我们希望记住到目前为止我们并没有真正对我们线性模型输出的分数给出太多解释。

We just said that we had an input X, we had a weight matrix W, it was somehow spinning out some collection of scores. But the multi-class SVM loss did not really give any interpretation to those scores other than telling that the score of the correct class should be higher than the score of all the other classes. Well now as we move to the cross-entropy loss, we are motivated by a different goal: we want to give some interpretation to the scores that the model is predicting.

我们刚才提到我们有一个输入X，一个权重矩阵W，它会产生一系列分数。但多类SVM损失并没有真正对这些分数给出任何解释，除了要求正确类别的分数应该高于所有其他类别的分数。现在当我们转向交叉熵损失时，我们的动机有所不同：我们希望对模型预测的分数给出某种解释。

So with the cross-entropy loss, what we want to do is to try to find a way to have some probabilistic interpretation of the scores that are being predicted by the model. We'd like to find a way to take this arbitrary vector of scores and interpret it as a probability distribution over all the categories that we're trying to recognize. The way that we do that is with this particular function called softmax that has this functional form here.

对于交叉熵损失，我们想要做的是尝试找到一种方法，对模型预测的分数给出某种概率解释。我们希望找到一种方法，将这个任意的分数向量解释为我们在尝试识别的所有类别上的概率分布。我们实现这一点的方法是使用这个称为softmax的特殊函数，它具有这里展示的函数形式。

But basically what we want to do is we're going to take the raw scores predicted by the classifier, and these raw scores are sometimes called unnormalized log probabilities or logits. You'll see these terms thrown around, and we'll take these raw scores and run them through an exponential function. So we'll take e to the power of each individual score and apply this element-wise from the score vector. The interpretation is that we know that probability distributions are supposed to be non-negative in all their slots, and the output of exponential is also non-negative, so this is a way.

但基本上我们要做的是获取分类器预测的原始分数，这些原始分数有时被称为未归一化的对数概率或logits。你会经常看到这些术语，我们将把这些原始分数通过指数函数进行处理。因此我们将对每个单独的分数取e的幂次，并逐个元素地应用于分数向量。这样解释是因为我们知道概率分布在所有位置都应该是非负的，而指数函数的输出也是非负的，所以这是一种方法。

To force our outputs to now be non-negative, and these are sometimes called unnormalized probabilities. That name unnormalized probabilities is very suggestive. It should tell you that the next thing we want to do is to normalize. So indeed then what we want to do is take the sum over all unnormalized probabilities and divide each of the unnormalized probabilities by the sum. Then after this operation, now we have a vector each element of which is nonzero and which sums to 1.

为了强制我们的输出变为非负值，这些有时被称为未归一化概率。未归一化概率这个名称非常有提示性，它应该告诉你我们接下来要做的是归一化。因此实际上我们要做的是计算所有未归一化概率的总和，然后用这个总和除以每个未归一化概率。经过这个操作后，现在我们得到了一个向量，其中每个元素都非零且总和为1。

So now this vector we can interpret as a probability distribution over all the classes that we're trying to recognize. This combination of taking exponential and then dividing by the sum of the exponentials is called the softmax function and this gets used in a lot of different places in machine learning. The reason it's called softmax is because it's a differentiable approximation to the max function. So if you were to look at this raw score vector the max would be this middle slot 5.1.

现在我们可以将这个向量解释为我们试图识别的所有类别的概率分布。这种先取指数然后用指数之和进行归一化的组合被称为softmax函数，它在机器学习的许多不同领域都有应用。它被称为softmax的原因是因为它是max函数的可微分近似。所以如果你观察这个原始分数向量，最大值应该是中间这个位置的5.1。

So you can imagine a version of the max function which output the vector 0 1 0 that had a 0 in all the non-max slots but a 1 in the slot of the max element. But that would be a non-differentiable function which would have 0 derivative almost everywhere, so we would not like to use that when training neural networks. Whereas this softmax function is now a soft differentiable approximation to that hard max function where you can see that now the maximum value of the unnormalized log probabilities was 5.1.

所以你可以想象一个max函数的版本，它输出向量0 1 0，在所有非最大位置都是0，但在最大元素的位置是1。但这将是一个不可微分的函数，几乎处处导数为0，因此我们在训练神经网络时不愿意使用它。而这个softmax函数现在是对那个硬max函数的软可微分近似，你可以看到未归一化对数概率的最大值是5.1。

So then that ended up as the largest element of the normalizer of the final probability distribution of 0 etc. This softmax function gets used in a lot of places in different types of neural network models whenever you want to compute the max of something but you also want it to be differentiable. So that's a very useful function and a very useful tool to have in your toolbox when you're building different types of differentiable neural network systems.

因此最终它成为了归一化后最终概率分布中最大的元素等等。这个softmax函数在很多不同类型的神经网络模型中都有应用，当你想要计算某个东西的最大值但又希望它是可微分的时候。所以这是一个非常有用的函数，也是你在构建各种可微分神经网络系统时工具箱中非常有用的工具。

But with that long aside, basically what we've done is we've taken this raw set of score vectors and we've now converted it into a probability distribution. Given that probability distribution, we now need to compute a loss for this element. The way that we do that is by taking the opposite of the log of the probability assigned to the correct category. In this case, the correct category should be cat. The probability assigned to the correct category is zero point one three, and then the minus log of that would be two point zero four. So the loss that we assigned to this example when training with.

经过这段长篇旁白，我们基本上所做的是：我们获取了这组原始得分向量，并将其转换为概率分布。有了这个概率分布，我们现在需要计算这个元素的损失。我们通过取正确类别概率的对数的相反数来实现这一点。在这种情况下，正确类别应该是猫。分配给正确类别的概率是0.13，然后其负对数将是2.04。因此，我们在训练时分配给这个示例的损失。

A with a cross entropy loss would be two point zero four. So then this operation of taking the minus log of the correct class maybe seems a bit arbitrary, but the reason that we take this particular form is because it's an instance of maximum likelihood estimation that I don't want to go into the details up here. But if you've taken something like a es 4 4 4 5 or 5 4 5 you would have talked about that in detail maybe excruciating detail, but the basic intuition behind why this is maybe a...

使用交叉熵损失的情况下，这个值将是2.04。因此，对正确类别取负对数的操作可能看起来有点随意，但我们采用这种特定形式的原因是因为它是最大似然估计的一个实例，我不想在这里详细讨论。但如果你学过类似es 4445或545的课程，你可能已经详细讨论过这个问题，也许是极其详细的讨论，但为什么这可能是一个...的基本直觉

A with a cross entropy loss would be two point zero four. So then this operation of taking the minus log of the correct class maybe seems a bit arbitrary, but the reason that we take this particular form is because it's an instance of maximum likelihood estimation that I don't want to go into the details up here. But if you've taken something like a es 4 4 4 5 or 5 4 5 you would have talked about that in detail maybe excruciating detail, but the basic intuition behind why this is maybe a reasonable loss to talk about is because we can imagine you can basically say that our model has now predicted some probability distribution over the categories and there exists some ground truth or correct probability distribution that we would have liked it to predict. Now the correct probability distribution would have had a 1 would have assigned all the probability mass onto the correct class, so the target probability distribution in this case would have had a 1 in the first thought 0 and all the others, and now we want to have some function that compares probability distributions.

使用交叉熵损失的情况下，这个值将是2.04。因此，对正确类别取负对数的操作可能看起来有点随意，但我们采用这种特定形式的原因是因为它是最大似然估计的一个实例，我不想在这里详细讨论。但如果你学过类似es 4445或545的课程，你可能已经详细讨论过这个问题，也许是极其详细的讨论，但为什么这可能是一个合理的损失函数的基本直觉是：我们可以想象，我们的模型现在已经预测了类别上的某种概率分布，并且存在某种我们希望它预测的基本事实或正确概率分布。正确的概率分布应该将所有的概率质量都分配给正确的类别，所以在这种情况下，目标概率分布应该在第一个位置为1，其他位置为0，现在我们想要有某种函数来比较概率分布。

So if you take information theory then there's a lot of nice mathematical reasons why this particular functional form called the Kullback-Leibler divergence is often used as a way to measure differences between probability distributions. And now if you imagine using this Kullback-Leibler divergence to compute the difference between this predicted probability distribution in the green box and this target probability distribution in the purple box, then if you work out the math you'll see that it comes out to be the negative log of the probability assigned to the class.

如果你学习信息论，就会有很多很好的数学理由来解释为什么这种称为Kullback-Leibler散度的特定函数形式经常被用来衡量概率分布之间的差异。现在，如果你想象使用这个Kullback-Leibler散度来计算绿色框中预测的概率分布和紫色框中目标概率分布之间的差异，那么如果你进行数学计算，你会发现结果就是分配给该类别的概率的负对数。

And this is called, and this is, there's another, I mean information theory has all these nice little ways to manipulate probabilities that are all related to each other. There's another thing called a cross entropy, which is a slightly different way of measuring differences between probability distributions that is the entropy of one plus the KL divergence of the two. The reason that this loss function is often called the cross entropy loss is because it's monotonically related to the cross entropy between the two probabilities of distributions.

这被称为，而且，还有另一个，我是说信息论有所有这些相互关联的巧妙处理概率的方法。还有另一个叫做交叉熵的东西，它是一种稍微不同的衡量概率分布之间差异的方法，即一个分布的熵加上两个分布的KL散度。这个损失函数通常被称为交叉熵损失的原因是因为它与两个概率分布之间的交叉熵单调相关。

So if we sum this up, the cross entropy loss is maximizing the probability of the correct class using this particular log formulation. Then we can ask a couple questions about this loss, just like we did for the multi-class SVM loss. First, what's the minimum and maximum possible loss for an example when we're using the cross entropy loss? The minimum loss would be 0 and the maximum loss would be infinity.

综上所述，交叉熵损失通过这种特定的对数公式来最大化正确类别的概率。然后我们可以像处理多类SVM损失那样，对这个问题提出几个疑问。首先，使用交叉熵损失时，单个样本可能的最小和最大损失是多少？最小损失为0，最大损失为无穷大。

But what's interesting here is that with the SVM loss it was actually possible to achieve the minimum because remember with the SVM loss we could achieve a loss of 0 by just having the correct class be a lot higher than all the other classes. But with the cross entropy loss, the only possible way that we could actually achieve a loss of 0 would be if our target probability distribution was actually one hot and if our predicted and target probability distributions were actually the same.

但这里有趣的是，使用SVM损失实际上有可能达到最小值，因为记得使用SVM损失时，我们只需让正确类别的分数远高于所有其他类别就能实现0损失。但对于交叉熵损失，我们实际上能够实现0损失的唯一方式是，如果我们的目标概率分布实际上是独热编码，并且我们的预测和目标概率分布完全相同。

But because our predicted probability distribution is being printed as being predicted through the softmax function, there's no actual practical way we can ever actually achieve zero loss. So another question, remember we've got the same debugging trick that we use for SVMs, that if all of our scores are going to be small random values, then what loss would we expect to see? Well, in this case, if all of our scores were small random values that were about the same, then we would expect to predict a uniform distribution as we run our predicted scores to the softmax function, so our predicted probability distribution would be uniform over C categories.

但由于我们的预测概率分布是通过softmax函数输出的，实际上我们无法真正实现零损失。那么另一个问题是，还记得我们用于SVM的调试技巧吗？如果所有分数都是较小的随机值，我们预期会看到怎样的损失？在这种情况下，如果所有分数都是大致相同的较小随机值，那么当我们把预测分数输入softmax函数时，预期会得到均匀分布，因此我们的预测概率分布将在C个类别上呈现均匀分布。

Which means we would have probability of 1 over C in each of the C slots. When we predict the minus log of the correct class, it would be minus log of 1 over C, or log of the number of categories. This is a number you should again be very familiar with. If we're training on the CIFAR-10 dataset, then you should know that natural log of 10 is about 2.3 because that's the loss you should expect to see at the beginning of training.

这意味着在C个类别中的每个位置，我们都会有1/C的概率。当我们预测正确类别的负对数时，结果将是负的1/C的对数，或者说是类别数量的对数。这个数字你应该再次非常熟悉。如果我们在CIFAR-10数据集上进行训练，那么你应该知道10的自然对数约为2.3，因为这是你在训练初期预期会看到的损失值。

Training so when you implement a linear classifier with a cross-entropy loss on CIFAR-10 and you don't see something about near 2.3 at the beginning, that means that you've done something very wrong and you have a bug. This is also a useful number to know because if during the training process you ever see losses that are much much higher than 2.3 with a 10 category problem, that means something has gone very wrong during the optimization because now your classifier is doing worse than random.

因此，当你在CIFAR-10上实现带有交叉熵损失的线性分类器时，如果开始时没有看到接近2.3的数值，这意味着你出现了严重错误并存在程序缺陷。这也是一个需要了解的有用数值，因为在训练过程中，对于10个类别的问题，如果你看到的损失值远高于2.3，这意味着优化过程中出现了严重问题，因为你的分类器表现比随机猜测还要差。

So, sort of practically speaking, when you're training a model with a cross entropy loss, it's always useful to have in the back of your mind what is the log of the number of categories and then kind of use that as a way to benchmark whether you've implemented things properly or whether the model has totally blown up and is now predicting something much worse than random. Okay, so then we've talked about two different types of losses: one being cross entropy and one being the multi-class SVM, and it's interesting to think about what happens.

那么，从实际角度来说，当你使用交叉熵损失训练模型时，心里要始终记住类别数量的对数值是多少，然后将其作为基准来衡量你是否正确实现了模型，或者模型是否完全崩溃并做出了比随机猜测更差的预测。好的，我们已经讨论了两种不同类型的损失函数：一种是交叉熵，另一种是多类SVM，思考它们会发生什么变化是很有趣的。

In what happened, how do we compare these two different losses and how these two different losses would behave on the same data? So let's assume that we've got some data set of three examples in three categories like we've been thinking about so far, and assume our predicted categories are the ground truth category is category 0 for each of these examples, and our classifier has predicted these through these set of scores on the left. So then what would be the cross entropy loss in this situation and what would be the SVM loss in this situation?

那么，在实际应用中，我们如何比较这两种不同的损失函数，以及它们在同一数据上会如何表现？假设我们有一个包含三个类别三个样本的数据集，就像我们之前一直讨论的那样，并假设每个样本的真实类别都是类别0，而我们的分类器通过左侧这组分值进行了预测。那么在这种情况下，交叉熵损失会是多少，SVM损失又会是多少？

In what happened, how do we compare these two different losses and how these two different losses would behave on the same data? So let's assume that we've got some data set of three examples in three categories like we've been thinking about so far, and assume our predicted categories are the ground truth category is category 0 for each of these examples, and our classifier has predicted these through these set of scores on the left. So then what would be the cross entropy loss in this situation? Well in this case, the SVM loss is easy because we can see that the ground truth category scores of 10 are at least one greater than all the incorrect scores, so the SVM loss would get zero here in this situation and the cross-entropy loss would be some value that's greater than zero. That I definitely can't compute all those logs in my head, but the difference is that this is kind of pointing to the same point we saw that before whereas with the SVM loss it's very.

那么，在实际应用中，我们如何比较这两种不同的损失函数，以及它们在同一数据上会如何表现？假设我们有一个包含三个类别三个样本的数据集，就像我们之前一直讨论的那样，并假设每个样本的真实类别都是类别0，而我们的分类器通过左侧这组分值进行了预测。那么在这种情况下，交叉熵损失会是多少？在这种情况下，SVM损失很容易计算，因为我们可以看到真实类别得分10至少比所有错误类别得分高1分，所以SVM损失在这里会是零，而交叉熵损失会是某个大于零的值。我肯定无法心算所有这些对数，但区别在于这指向了我们之前看到的相同点，而对于SVM损失来说，这是非常。

Easy and very possible to actually achieve zero loss, whereas for the cross entropy you'll never get zero loss. So then what happens to each loss if I slightly change the scores of the last data point? All right, so this last data point has a predicted score of 10 for the correct category and a predicted score of minus 100 for the two incorrect categories. So in this case the SVM loss won't care. The SVM is already giving zero loss to this example.

实际上很容易实现零损失，而对于交叉熵来说你永远无法得到零损失。那么如果我稍微改变最后一个数据点的分值，每种损失会如何变化？好的，这最后一个数据点在正确类别上的预测得分是10，在两个错误类别上的预测得分是负100。在这种情况下，SVM损失不会在意，SVM已经给这个样本零损失了。

And if we change it just a little bit, then it doesn't really care. But the cross-entropy loss on the other hand is never satisfied for this particular example. It's already doing a really good job at classifying an example because the correct score is way way way higher than all the incorrect scores. But the cross-entropy loss doesn't care. The cross-entropy loss always wants to continue pushing these farther and farther apart and continue pushing the predicted score of the correct class up to positive infinity and keep pushing all the scores of all the incorrect classes down.

如果我们只是稍微改变一下，它并不在意。但另一方面，对于这个特定样本，交叉熵损失永远不会满足。它已经在分类样本方面做得非常好了，因为正确类别的得分远远高于所有错误类别的得分。但交叉熵损失并不在意。交叉熵损失总是希望继续将这些分数推得越来越远，继续将正确类别的预测得分推向正无穷，并将所有错误类别的得分不断压低。

With cross entropy, you just keep training forever and it'll just continue trying to separate those scores more and more. Then we get a similar intuition if you think about doubling the score of the correct class from 10 to 20. Again, the cross entropy loss will decrease where the SVM loss will still be zero. To recap what we talked about today, we introduced this notion of linear classifiers as this matrix multiply and a vector. We talked about these three different viewpoints to think about what linear.

对于交叉熵，你会一直训练下去，它会继续不断地尝试将这些分数分得越来越开。然后我们得到类似的直觉，如果你考虑将正确类别的分数从10加倍到20。交叉熵损失会再次减少，而SVM损失仍然为零。总结一下我们今天讨论的内容，我们引入了线性分类器的概念，即矩阵乘法和向量。我们讨论了三种不同的视角来思考线性。

Classifiers are doing and saw how these different viewpoints can have different implications what we're thinking about and we saw the idea of a loss function to quantify our unhappiness with the present performance of our classifier. But now the next question of course is how will we actually go about finding the best W once we've written down our preferences and for that well you can come back next time and we'll talk about optimization.

分类器的作用，并看到了这些不同视角如何对我们思考的内容产生不同的影响，我们引入了损失函数的概念来量化对分类器当前性能的不满。但现在下一个问题当然是，一旦我们写下了我们的偏好，我们实际上要如何找到最佳的W，为此你可以下次再来，我们将讨论优化。