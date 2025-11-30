---
title: 'MIT_6S184_Flow_Matching_and_Diffusion_Models_-_Lec'
publishDate: 2025-11-30
description: 'TODO'
tags:
  - TODO
language: 'English'
heroImage: { src: './default.jpg', color: '#D58388' }
---

All right, um, so welcome everyone. Hello and welcome to the class Generative AI with Stochastic Differential Equations. As you all know, this is an IP course. It's going to take place in the next two weeks, and we're going to focus on giving you an introduction to flow and diffusion models.

好的，欢迎大家。大家好，欢迎参加《随机微分方程与生成式人工智能》课程。大家都知道，这是一门知识产权课程，将在接下来两周内进行，我们将重点向大家介绍流模型和扩散模型。

Um, I'm Peter. I'm a PhD student here at MIT supervised by Tommy Yakala and Rin Barcel, and I'm co-teaching this class with Ezra. You want to raise your hand? Um, so we did this class together, and you'll see both of us in the next few weeks.

嗯，我是Peter。我是麻省理工学院的博士生，由Tommy Yakala和Rin Barcel指导，这门课是我和Ezra共同教授的。你想举手吗？嗯，我们一起准备了这门课，在接下来的几周里你们会看到我们俩。

At this point, a quick shout out to Tommy who sponsored this class and advised us with the design of the class.

在此，要特别感谢Tommy，他赞助了这门课程，并在课程设计上给予了我们指导。

So, we're all here I guess because we've all witnessed a tremendous revolution of AI systems happening in the last few years, what's commonly called Gen AI in layman's terms.

所以，我想我们大家齐聚于此，是因为我们都见证了在过去几年里人工智能系统发生的巨大革命，也就是通常所说的生成式AI。

For example, you've seen artistic images like from Stable Diffusion or Midjourney. You've seen realistic videos being generated from video generators like Sora from OpenAI, or you've seen things like ChatGPT that you use potentially every day to draft text.

例如，你们见过像Stable Diffusion或Midjourney生成的艺术图像。你们见过由OpenAI的Sora等视频生成器生成的逼真视频，或者你们可能每天都在使用像ChatGPT这样的工具来起草文本。

The really distinguishing feature of these systems is that they're creative. Like previous AI systems were mainly used to predict things, for example, classify an image, but these systems generate new objects.

这些系统真正显著的特点是它们具有创造性。以往的人工智能系统主要用于预测事物，例如对图像进行分类，但这些系统能生成新的对象。

This class, very high level, teaches you algorithms to generate objects. So that's the goal, and we're not focusing on arbitrary algorithms, but we're focusing on what's called flow and diffusion models.

这门课程，从非常高的层面，教授你们生成对象的算法。这就是我们的目标，我们并非关注任意的算法，而是专注于所谓的流模型和扩散模型。

So the specific family of algorithms, like there's a whole family of related frameworks, and all of them basically build state-of-the-art models for generating images, videos, or proteins, and a variety of different modalities.

这个特定的算法家族，包含一系列相关的框架，它们基本上都构建了用于生成图像、视频、蛋白质以及各种不同模态数据的最先进模型。

For example, you've heard potentially of Stable Diffusion or DALL-E which are based on these models. Potentially heard of OpenAI's Sora or Meta's Make-A-Video, and even in a scientific domain, things like AlphaFold Free or RF Diffusion, they're all diffusion models.

例如，你们可能听说过基于这些模型的Stable Diffusion或DALL-E。可能也听说过OpenAI的Sora或Meta的Make-A-Video，甚至在科学领域，像AlphaFold Free或RF Diffusion，它们都是扩散模型。

So these models are really at the core of a lot of generative AI models, and the goal of this class is to make you understand them.

因此，这些模型确实是许多生成式AI模型的核心，本课程的目标就是让你们理解它们。

Flow and diffusion models, given their name, they're based on a bit of theory, and we believe that to understand that, you really need to understand the theory first.

流模型和扩散模型，顾名思义，它们基于一定的理论，我们相信要理解它们，你首先需要理解其理论基础。

So along with this class, you will be taught about ordinary and stochastic differential equations. We want to give you the necessary mathematical toolbox to understand these things.

因此，在本课程中，你们将学习常微分方程和随机微分方程。我们想为你们提供理解这些内容所必需的数学工具箱。

On the other hand, we want you to implement and be able to apply these methods, so that's what's going to be happening in the labs.

另一方面，我们希望你们能够实现并应用这些方法，这将是实验课的内容。

So see this class as has three goals: you learn flow and diffusion models from first principles, you learn the necessary amount of math but only the minimal amount of math to understand them, and third, you learn how to implement and apply these algorithms.

所以，请将本课程视为有三个目标：你们将从基本原理学习流模型和扩散模型，学习理解它们所必需的、但仅是最低限度的数学知识，第三，你们将学习如何实现和应用这些算法。

Quick overview: so we start off today, lecture one today, we're going to talk about sampling. So we're going to formalize what it means to generate an object.

快速概览：我们从今天开始，今天的第一讲，我们将讨论采样。我们将形式化地定义生成一个对象意味着什么。

Next, we're going to construct flow and diffusion models, so we're going to learn what they are.

接下来，我们将构建流模型和扩散模型，也就是学习它们是什么。

The next thing, we're going to learn how to derive training targets for them. So every training algorithm has a certain objective, going to learn what the objective is.

再接下来，我们将学习如何为它们推导训练目标。每个训练算法都有特定的目标函数，我们将学习这个目标函数是什么。

Then we talk about flow matching and score matching, which are the fundamental training algorithms. That's going to be on Thursday, so that's going to conclude week one.

然后我们讨论流匹配和分数匹配，它们是基础的训练算法。这将在周四进行，并以此结束第一周。

And next week, we going to talk about how to build image generators. So we're going to, on the example of things like Stable Diffusion, we're going to learn about network architectures, how to condition on specific prompts.

下周，我们将讨论如何构建图像生成器。我们将以Stable Diffusion等为例，学习网络架构，以及如何根据特定提示进行条件化生成。

We then talk about advanced topics, for example, like alignment, how to deal with complex data types, how to distill a model.

然后我们讨论高级主题，例如对齐、如何处理复杂数据类型、如何进行模型蒸馏。

And then on Thursday, we'll have a guest lecture where we will focus on two new applications of diffusion models.

然后在周四，我们将有一场客座讲座，重点介绍扩散模型的两个新应用。

The one is protein design, where Jason from MIT is going to come give a great talk about how he was really pioneering the design of the diffusion models for these applications.

一个是蛋白质设计，麻省理工学院的Jason将来做一场精彩的演讲，讲述他如何在这些应用中开创性地设计扩散模型。

And also in robotics, where Benjamin Bfield from Toyota Research Institute, who's a manager there who currently leads really the frontier of the diffusion models in the application of robotics.

另一个是机器人技术，丰田研究所的Benjamin Bfield，他是那里的经理，目前正引领着扩散模型在机器人技术应用的前沿。

Great. Okay, so let's start off with section one and think about like what it means to generate something.

很好。好的，让我们从第一部分开始，思考生成某物意味着什么。

So we will see, we going to go from generation to sampling. So the first thing is, how do we represent the objects that we want to generate in a computer?

我们将看到，我们将从生成过渡到采样。所以第一件事是，我们如何在计算机中表示我们想要生成的对象？

So that should not be totally new to you, but let's just recap. So for example, like an image has a certain number of pixels, a certain height and a certain width, and usually have three color channels.

这对你们来说应该不完全陌生，但我们还是回顾一下。例如，一张图像有一定数量的像素，有一定的高度和宽度，通常有三个颜色通道。

Numerically, we represent this object as a vector set that's in the vector space, or it's a vector of length H * W * 3. So usually we can represent that like that numerically in a computer.

在数值上，我们将这个对象表示为向量空间中的一个向量集，或者是一个长度为 H * W * 3 的向量。所以通常我们可以在计算机中这样进行数值表示。

Videos are basically a stream of images. If you have T time frames, each of these represents an image in itself, so you usually can represent this in the computer as a vector of that shape, where like see T times whatever the size of the image was.

视频基本上是一系列图像。如果你有 T 个时间帧，每一帧本身代表一张图像，所以你通常可以在计算机中将其表示为一个该形状的向量，即 T 乘以图像的尺寸。

Let's think about, for example, a molecular structure. Molecular structure, let's say has N atoms, and each atom has three coordinates. Then you would represent that as a vector of that shape, where it's basically each row corresponds to the coordinates of a specific atom.

让我们考虑一下，例如，一个分子结构。假设分子结构有 N 个原子，每个原子有三个坐标。那么你可以将其表示为一个该形状的向量，其中基本上每一行对应一个特定原子的坐标。

So all of this is to say that in this class, we want to represent the object we want to generate as vectors. There's other ways of representing, but in the end of the day, what we want to generate mathematically is just vectors.

所有这些都是为了说明，在本课程中，我们希望将想要生成的对象表示为向量。虽然还有其他表示方式，但归根结底，我们在数学上想要生成的只是向量。

Let's think about what does it mean to successfully generate something. So let's think about it: we have a prompt, a picture of a dog, and let's say you have these four images that you see here on the right.

让我们思考一下成功生成某物意味着什么。假设我们有一个提示词："一张狗的照片"，并且假设你看到右边这里有四张图像。

The first one is just some noise, maybe something you've seen on an old TV. How would you prescribe this? That would be completely useless, right? That has nothing to do with what we actually want.

第一张只是一些噪点，可能像在老式电视上看到的那样。你会如何评价它？那完全没用，对吧？它与我们实际想要的毫无关系。

Let's say this image, it's not a picture of a dog, but at least it looks like something. It looks like an image that we would have done in real life. Okay, so it's bad but better than the previous one.

假设这张图像，它不是一张狗的照片，但至少它看起来像点什么。它看起来像我们在现实生活中会拍到的图像。好吧，所以它不好，但比前一个好。

This one is at least an animal. That's the wrong one, it's a cat. Um, and finally we get a dog.

这张至少是个动物。但不对，是只猫。嗯，最后我们得到了一只狗。

So you can see you rank these images based on how good they align with the prompt, but these are subjective statements. When I say "oh this fits better" or "fits less better", that's not a statement I can mathematically formalize. So the question is how do we formalize this? The language we use, and everybody in general modeling uses this language of probability theory, where we basically introduce this object called the data distribution.

所以你可以看到，你根据这些图像与提示的匹配程度对它们进行排序，但这些是主观判断。当我说"这个更匹配"或"这个不太匹配"时，这并非我能在数学上形式化的陈述。问题在于我们如何将其形式化？我们使用的语言，以及所有建模者通常都使用概率论的语言，我们基本上引入了称为数据分布的这个概念。

It basically converts the statement of how good something is to how likely something is on a specific data set. For example, in this example we could say how likely are we to find this picture on the internet? So let's say we have prompt of the picture of the dog, it would be almost impossible to find a picture like this with this caption right? This would be pretty rare, this would be unlikely. But maybe there was a swap between animals and this would be very likely.

它基本上将"某物有多好"的陈述转换为"某物在特定数据集上的可能性有多大"。例如，在这个例子中，我们可以说我们在互联网上找到这张图片的可能性有多大？假设我们有一张狗图片的提示，几乎不可能找到带有这种标题的图片，对吧？这将非常罕见，不太可能。但如果动物之间发生了互换，这就很可能了。

So we basically translate how good an image is to how likely it is under the data distribution. We translated this subjective statement into a statement of probability theory.

因此我们基本上将图像的质量转化为其在数据分布下的可能性。我们将这个主观陈述转化为概率论的陈述。

So let's think about what this means formally. I just said we have a data distribution that essentially is the distribution of objects that we want to generate. Throughout this class you should keep this notation in your mind: it's called P_data, that's a data distribution.

让我们正式思考这意味着什么。我刚才说过我们有一个数据分布，本质上是我们想要生成的对象的分布。在本课程中，你应该记住这个符号：它被称为P_data，即数据分布。

Now let's think back about how we represent probability distributions. The usual representation, the one we are going to use here, is one of a probability density. Basically says that P_data goes from the vector space R^D, which is our space of objects, and gives you a non-negative number. So given an object set, it gives you a likelihood or probability of how likely that object is.

现在让我们回顾一下如何表示概率分布。通常的表示方法，也是我们将在这里使用的，是概率密度。基本上说P_data从向量空间R^D（我们的对象空间）映射，并给出一个非负数。因此给定一个对象集，它会给出该对象的可能性或概率。

I should stress we don't know this probability density, but we just postulate it as a distribution they want to sample from. So what does it mean to generate something? It means to sample from this distribution, to roll the dice and get samples from distribution out.

我应该强调我们不知道这个概率密度，但我们只是假设它作为我们想要从中采样的分布。那么生成某物意味着什么？意味着从这个分布中采样，掷骰子并从分布中获得样本。

And hopefully if we done this right, what we will get is an actual good image. So an image of a dog, or at least most of the time we should get that good. But we don't just want to do that somehow because we're doing machine learning, we need some sort of like data right?

如果我们做得正确，我们希望得到的是一个真正的好图像。比如一张狗的图片，或者至少大多数时候我们应该得到这样的好结果。但我们不仅仅想以某种方式做到这一点，因为我们在进行机器学习，我们需要某种数据，对吧？

So the question is like how does the data set fit into this picture? Let's think about our a few examples. For images we could just use public available images from the internet, we could just pull them and like collect the data set. For videos we could just use something like YouTube. For protein structures we could use something like the Protein Data Bank, just generally scientific data that's been gathered over last few decades.

那么问题是数据集如何融入这个图景？让我们考虑几个例子。对于图像，我们可以直接使用互联网上公开可用的图像，我们可以直接获取它们并收集数据集。对于视频，我们可以使用类似YouTube的东西。对于蛋白质结构，我们可以使用蛋白质数据库等，基本上是过去几十年收集的科学数据。

What's a data set? A data set consists of a finite numbers of samples from that data distribution. So we just denote this as set one to set n, which are just samples from the data distribution, and we're going to use that to train our model.

什么是数据集？数据集由来自该数据分布的有限数量的样本组成。因此我们将其表示为集合1到集合n，这些只是来自数据分布的样本，我们将使用这些样本来训练我们的模型。

All right, so let's think about something like the case I just described to you was what's commonly called unconditional generation. So usually we have one model for, let's say, a fixed prompt. For example that prompt could be dog, and it basically means that you always get the same image from the same model right? We would always get images of dogs, they would likely look a little different, but at the end of the day they would all be images of dogs.

好的，让我们思考一下我刚才描述的情况，这通常被称为无条件生成。因此通常我们有一个模型用于，比如说，固定的提示。例如，该提示可能是"狗"，这基本上意味着你总是从同一个模型得到相同的图像，对吧？我们总是会得到狗的图片，它们可能看起来有些不同，但最终它们都是狗的图片。

What we more want is you want to condition whatever we want to generate on a certain variable that throughout this class is going to be called y. So for example, y could be dog, could be cat, could be landscape, could be much more complicated object. It could be a room full of MIT students listening to a diffusion class, so something like it could be a much longer text.

我们更想要的是将我们想要生成的任何内容以某个变量为条件，这个变量在本课程中将被称为y。例如，y可以是狗、猫、风景，或者更复杂的对象。可能是一个坐满MIT学生听扩散课程的教室，所以它可能是一段更长的文本。

So we want to have this variable y basically condition our generation. And for this we introduce this object called the conditional data distribution, which we denote as P dot (uh dot stands for a placeholder) given y. So it basically means given this prompt y, what's the distribution of the data given this prompt?

因此我们想要这个变量y作为我们生成的条件。为此，我们引入了称为条件数据分布的这个对象，我们将其表示为给定y的P点（点代表占位符）。所以它基本上意味着给定这个提示y，给定这个提示的数据分布是什么？

And conditional generation essentially means that sampling from this conditional distribution. So we sample an object set from P_data given Y. And it's important to note that this is the case we're going to focus on first off, because like most general of modeling frameworks start with that and then they translate it to this case.

条件生成本质上意味着从这个条件分布中采样。因此我们从给定Y的P_data中采样一个对象集。需要注意的是，这是我们首先将关注的情况，因为大多数通用建模框架都从这里开始，然后将其转化到这种情况。

So this is the case we're ultimately interested in, but we want to first focus on this case. So just keep that in mind that where we're starting and but where we're going.

因此这是我们最终感兴趣的情况，但我们想首先关注这种情况。所以请记住我们从哪里开始，以及我们要去哪里。

All right good, so let's think about a generative model. So given its name, a generative model tries to generate something, which we've just learned means sampling from the distribution. So in other words, a generative model tries to generate samples from the distribution.

好的，让我们思考一下生成模型。顾名思义，生成模型试图生成某些东西，我们刚刚了解到这意味着从分布中采样。换句话说，生成模型试图从分布中生成样本。

And usually what we have is what's called an initial distribution. We call this here as P_init, it's an important notation as well that we use throughout this class, which is essentially the initial distribution. And in the default case, you could always think about P_init as a Gaussian, so as a standard Gaussian of zero mean and diagonal identity matrix as covariance matrix.

通常我们拥有的是所谓的初始分布。我们在这里称之为P_init，这也是我们在整个课程中使用的重要符号，它本质上是初始分布。在默认情况下，你总是可以将P_init视为高斯分布，即均值为零、协方差矩阵为对角单位矩阵的标准高斯分布。

So that's what's here on the top right. And the idea of a generative model essentially start with an initial distribution. So you sample from a Gaussian which visually would just look like white noise for an image, and what you get out is you get an image of a dog.

这就是右上角显示的内容。生成模型的概念基本上从初始分布开始。因此你从高斯分布中采样，在视觉上对于图像来说就像白噪声，而你得到的结果是一张狗的图片。

So essentially in the language of probability theory, a generative model converts an initial distribution into a data distribution. And the goal of this class is to teach you how to do that, and not so somehow but with flows and diffusion models, and that's what we're going to talk about in a second.

因此本质上，在概率论的语言中，生成模型将初始分布转换为数据分布。本课程的目标是教你如何做到这一点，不是以某种方式，而是通过流模型和扩散模型，这就是我们接下来要讨论的内容。

All right, I'll stop here real quick. Are there any questions? Good, cool. All right, so let's dive right in.

好的，我在这里稍微停一下。有什么问题吗？很好，很棒。那么让我们直接开始吧。

So as I said, we want to build generative models with flows and diffusion, and the goal of this section is going to be to define what that is and how we sample from it. At this point a quick note: I'm going to write now on the blackboard. There's lecture notes online which is a self-contained summary of what I'm going to tell you here.

如我所说，我们想要用流模型和扩散模型构建生成模型，本节的目标将是定义这是什么以及我们如何从中采样。在此快速说明：我现在要在黑板上写字。网上有讲义，它包含了我将要在这里讲述内容的独立总结。

So you're very invited to like take notes along, but I also want to just as a disclaimer use selection notes, that's what I would do. Thanks, all right cool, so let's get started.

非常欢迎你们做笔记，但我也想作为免责声明使用精选笔记，这就是我会做的。谢谢，好的很棒，让我们开始吧。

We want to talk about flow and diffusion models. Throughout this class, we will always have a flow component and a diffusion component. It's much like the most important thing is you understand the flow component because it's really the basis for the other part. So I would almost say like the flow component is the one that's really core to understand. So if you struggle, make sure you understand the flow component and then maybe revisit the diffusion component.

我们将讨论流模型和扩散模型。在本课程中，我们将始终包含流组件和扩散组件。最重要的是理解流组件，因为它实际上是其他部分的基础。我几乎可以说流组件才是真正需要理解的核心内容。因此如果你遇到困难，请确保先理解流组件，然后再回顾扩散组件。

Okay, so let's start off with thinking about what's the fundamental object of a flow, and that's what's a trajectory. So I'll first define what a trajectory is. The trajectory is what intuitively think a trajectory is basically is a function of time. In this class, we mainly going to focus on time from 0 to 1, and it essentially given a time point T gives you a vector XT. So you're basically saying for every time point T, I'm getting a vector XT out.

好的，让我们从思考流的基本对象开始，那就是轨迹。首先我将定义什么是轨迹。直观上来说，轨迹本质上是一个时间函数。在本课程中，我们主要关注从0到1的时间范围，它本质上是在给定时间点T时给出一个向量XT。也就是说对于每个时间点T，我们都能获得一个向量XT。

A way to think about this is essentially in two dimensions. For example, you would have a point and you're basically moving through space, and that's basically your trajectory XT. The other fundamental object is a vector field, and vector field we commonly denote here as U, and it has a spatial component and a time component. So we have a vector x a time T, and it returns a vector UT of X.

理解这个概念可以从二维空间来考虑。例如，你有一个点，它基本上在空间中移动，这就是你的轨迹XT。另一个基本对象是向量场，我们通常用U表示，它具有空间分量和时间分量。因此我们有一个向量x和时间T，它返回一个向量UT(x)。

Visually, you should think about a vector field as something that at every point in space gives you direction. So like let's say you start like at this point, it gives you direction here. At this point, it gives you direction here. It basically gives you directions at every point, and you basically ideas later to follow along this.

从视觉上来说，你应该将向量场理解为在空间中每个点都给出方向的东西。比如你从这个点开始，它在这里给你方向；在那个点，它在那里给你方向。它基本上在每个点都给出方向，而你后续基本上会沿着这些方向移动。

So let's define now and that's very fundamental what an ordinary differential equation is. An ODE which stands for ordinary differential equation, which you might have also heard before, is basically describes conditions on a trajectory. The first thing we have is that a trajectory should start at a specific point which is called the initial condition. So x0 should like capital X0 should start at lowercase x0 which is what's called the initial condition.

现在让我们定义什么是常微分方程，这是非常基础的概念。ODE代表常微分方程，你可能之前也听说过，它基本上描述了轨迹的条件。首先我们要求轨迹应该从一个特定点开始，这称为初始条件。因此大写X0应该从小写x0开始，这就是所谓的初始条件。

Then we have the condition that we're basically saying that we want to follow along the vector field that describes the direction. So let's say we started here at x0 and we basically follow along the time vectors across time. You should keep in mind that these direction change over time because the vector field here is time dependent. But basically you should think about it's basically following along the direction specified by a vector field.

然后我们还有一个条件，即我们基本上希望沿着描述方向的向量场移动。假设我们从x0开始，基本上随着时间沿着时间向量移动。需要注意的是这些方向会随时间变化，因为这里的向量场是时间相关的。但基本上你可以理解为它沿着向量场指定的方向移动。

So how do we describe this? You're basically saying that the derivative or the velocity of the trajectory is given by UT at the location where XT currently is. So says the direction of XT is given by the location of X and then specified by UT, and that's what's called an ODE or combined with the initial condition it's called an ODE.

那么我们如何描述这个呢？基本上就是说轨迹的导数或速度由当前位置XT处的UT给出。也就是说XT的方向由X的位置决定，然后由UT指定，这就是所谓的ODE，或者与初始条件结合时称为ODE。

Maybe some of us you have heard this. I mean ODEs are fundamental in mechanics, in engineering, in physics. So I'm sure you have heard this in some form or another before. But this term is less common and that's called the flow. So a flow is essentially a collection of trajectories that follow the ODE essentially is we gather a lot of solutions for different initial conditions and then we gather them all in one function and call that a flow.

可能你们中有些人听说过这个。我的意思是，ODE在力学、工程和物理学中都是基础概念。所以我确信你们之前以某种形式听说过这个。但流这个术语不太常见，它本质上是一组遵循ODE的轨迹的集合，也就是说我们收集了许多不同初始条件的解，然后将它们全部汇集到一个函数中，称之为流。

So let's define what a flow is. We commonly call this S and like before it has a spatial and a time component, and then given an initial condition x0 and T, it maps it to phi T x0. And what we're basically saying is that for every initial condition x0, I want this to be a solution to my ODE which essentially as the following condition.

那么让我们来定义什么是流。我们通常称之为S，和之前一样，它具有空间和时间分量，然后在给定初始条件x0和时间T时，它映射到phi T x0。我们基本上是说对于每个初始条件x0，我希望这是我的ODE的解，这本质上满足以下条件。

It means the initial condition says phi 0 of x0 is just x0 which is essentially this condition. So we're starting at x0 and the other component is that the time derivative of S T of x0 is specified by this equation. So it's specified by S T of x0 and UT.

这意味着初始条件表明phi 0(x0)就是x0，这本质上就是这个条件。所以我们从x0开始，另一个条件是S T(x0)的时间导数由这个方程指定。也就是说它由S T(x0)和UT指定。

All right, just to recap. So a flow is essentially collection of solutions to an ODE for a lot of initial conditions. So maybe like as a diagram so an ODE is defined by a vector field. So a vector field defines an ODE. A trajectory is a solution to this ODE, and then a flow a flow is a collection of trajectories for various initial conditions.

好的，简单回顾一下。流本质上是许多初始条件下ODE解的集合。也许可以这样图示：ODE由向量场定义，所以向量场定义了ODE。轨迹是这个ODE的一个解，而流是各种初始条件下轨迹的集合。

So a vector field defines an ODE in this format. A trajectory XT is a solution to this ODE, and a flow defines a collection of solutions to that for various initial conditions. Good cool, let's look at an example.

所以向量场以这种形式定义ODE。轨迹XT是这个ODE的一个解，而流定义了各种初始条件下这些解的集合。很好，让我们看一个例子。

So what you see here is an example of a flow and that's a visualization that takes a bit of time to get used to. So in blue you see the vector field and then in red you see for every grid point that's an initial condition and you see how this initial condition moves through space. So the grids how they get deformed, it's basically a visualization of the flow.

这里你看到的是一个流的示例，这是一个需要一些时间才能习惯的可视化。蓝色部分显示的是向量场，红色部分显示的是每个网格点（即初始条件），你可以看到这些初始条件如何在空间中移动。网格如何变形，这基本上就是流的可视化。

So basically it says now that's a flow map and every time it there's a new phi T for different T and it kind of moves through time. Good all right, so the first thing we should ask ourselves if we see an equation is always does the solution exist and if yes is it unique.

基本上这就是说现在有一个流映射，每次对于不同的T都有一个新的phi T，它随着时间移动。好的，那么当我们看到一个方程时，首先要问自己的是解是否存在，如果存在，是否唯一。

And we can answer this question with yes under certain regularity condition. So if the vector field UT is continuously differentiable with bounded derivatives, then a unique solution to this ODE exists. There's a bit more general condition which is when the vector field is Lipschitz. If that's not a term for you, it doesn't really matter in this case.

我们可以在一定的正则性条件下肯定地回答这个问题。如果向量场UT是连续可微且导数有界，那么这个ODE存在唯一解。还有一个更一般的条件是向量场满足Lipschitz条件。如果你不熟悉这个术语，在这种情况下并不重要。

The key takeaway is in all practical interests of machine learning, unique solution to this ODE exists. Actually in most classes you've ever taken that already was implicitly assumed. I'm just going to resay it again that there are certain nice conditions you need to assume, but these conditions are usually the case in the case of machine learning.

关键要点是，在机器学习的所有实际应用中，这个ODE都存在唯一解。实际上，在你们上过的大多数课程中，这已经被隐含地假设了。我要再次说明，你需要假设某些良好的条件，但这些条件在机器学习的情况下通常都成立。

If this would be a math class, you would prove this. Maybe you've seen this before, it's called the Picard iteration. I'm not going to do this of course, just as a for your information. Good cool, let's think about an example.

如果这是数学课，你会证明这个。也许你以前见过这个，它叫做皮卡迭代法。我当然不会在这里证明，只是供你参考。很好，让我们考虑一个例子。

A very simple example is a linear vector field. So UT of X would just be minus theta times X. So you just multiply a vector x with a scalar, and that scalar we assume to be bigger than zero. So we take minus theta times x, and the claim I'm going to make is that the flow is given by this function here, so some exponentially decreasing term times x0. Visualize it's here on the right for different initial conditions, so you should see here the y-axis starts at different initial conditions and it exponentially decays towards zero.

一个非常简单的例子是线性向量场。因此UT(x)就是负θ乘以X。也就是说你将向量x与一个标量相乘，我们假设这个标量大于零。所以我们取负θ乘以x，并且我主张这个流由这里的函数给出，即某个指数递减项乘以x0。在右侧可以看到不同初始条件下的可视化效果，这里y轴从不同的初始条件开始，然后指数衰减趋近于零。

So I make the claim that this is the flow. So what do I need to check? I need to check that the initial condition holds, so y of T at zero should be exponential of zero which is one, so this is just x0 so it starts at x0. So the initial condition is fulfilled.

因此我主张这就是流。那么我需要验证什么？我需要验证初始条件是否成立，所以y在时间零处的值应为e^0等于1，因此这就是x0，所以它从x0开始。这样初始条件就满足了。

And to show that the ODE is fulfilled, what do we do? We just plug it into the equation. So to take the time derivative of it and then we can use the chain rule and plug in the definitions again. In lecture notes this is explained in more detail, but you show that basically you see that the first and the last equation is essentially this condition, so you see that the ODE is fulfilled and this is the flow that we want.

为了证明ODE成立，我们该怎么做？我们只需将其代入方程。对其求时间导数，然后使用链式法则并再次代入定义。在讲义中有更详细的解释，但基本上你会看到第一个和最后一个方程本质上就是这个条件，因此可以看出ODE成立，这就是我们想要的流。

So unfortunately in most cases it's not that easy. You cannot just find by hand a solution to an ODE, and what we do is we need to simulate it. Most of the time in this class we're going to use a method called the Euler method, and it's basically the idea that you just simply go into the direction of the vector field for small time steps at every time.

遗憾的是在大多数情况下并没有这么简单。你无法手动找到ODE的解，我们需要进行模拟。本课程大多数时候我们会使用欧拉方法，其基本思想是在每个时间点沿着向量场的方向进行小步长移动。

So let's think about how what this means: we get a vector field UT, we get an initial condition X zero, and get a certain number of steps. Here I assume that we just simulated until time one. You also do use another time, start at time zero. The step size is just one divided by the number of steps.

让我们思考这意味着什么：我们有一个向量场UT，一个初始条件X0，以及一定数量的步数。这里我假设我们只模拟到时间1。你也可以使用其他时间，从时间零开始。步长就是1除以步数。

We set the initial condition and then we just at every time step XT plus h updates the previous time step plus h times UT of XT. We just basically get every time go a small direction into the direction of the vector field. At the end we just return a trajectory, so that's numerical simulation of an ODE that many of you had potentially have seen before.

我们设置初始条件，然后在每个时间步XT+h更新为前一个时间步加上h乘以UT(XT)。基本上就是每次沿着向量场方向移动一小步。最后我们返回一条轨迹，这就是许多同学可能之前见过的ODE数值模拟。

So let's now use this for machine learning. Now the question is where does the machine learning come in? And here we want to use a flow model which basically leverages what I've just told you.

现在让我们将其应用于机器学习。问题在于机器学习在何处介入？这里我们想要使用流模型，它基本上利用了我刚才讲述的内容。

So what was our goal? I told you that a generative model converts an initial distribution P init and does something I haven't specified yet, and what we get out is the data distribution. And given what I told you, it will not surprise you that we want to do this transformation with an ODE.

我们的目标是什么？我告诉过你们生成模型将初始分布P init转换并执行某些我尚未指定的操作，最终得到数据分布。根据我所讲的内容，我们要用ODE进行这个转换应该不会让你们感到意外。

So what we're going to do is we're going to start off with the initial distribution, we simulate an ODE, and then we get a data distribution hopefully out.

所以我们要做的是从初始分布开始，模拟一个ODE，然后希望最终得到数据分布。

So let's start with a neural network. The idea is basically that really what defines this ODE is a vector field, so let's just make the vector field a neural network. So we have a vector field UT theta which is, as I just defined, a function of time and space, and theta are the parameters.

让我们从神经网络开始。基本思想是真正定义这个ODE的是向量场，因此我们让向量场成为一个神经网络。所以我们有一个向量场UT theta，正如我刚才定义的，它是时间和空间的函数，而theta是参数。

So I'm not telling you what these parameters are at this point because it doesn't really matter. All it matters is that it's a function that is a vector field that has some parameters that we can optimize over. We will talk later about architectures; at this point it's not yet that relevant.

此刻我不具体说明这些参数是什么，因为这并不重要。重要的是它是一个具有可优化参数的向量场函数。我们稍后会讨论架构问题，此刻这还不那么相关。

You set P init so P init is at this point a distribution. You could think about it as a Gaussian distribution. Like for the first few classes you can always think about P init as a Gaussian distribution.

你设置P init，此时P init是一个分布。你可以将其视为高斯分布。就像在前几节课中，你总可以将P init视为高斯分布。

So we start with something very simple where we have full access to some knowledge, and then we want to end up with some abstract object that we have not yet don't know yet. So that is more a specification of a goal less so of a definition.

所以我们从非常简单的、完全掌握某些知识的情况开始，然后希望最终得到某个尚未知晓的抽象对象。这更像是对目标的说明而非定义。

So we want to build something that does that. It's a distribution so you can sample from it. For example, you could sample a Gaussian distribution from it, so some Gaussian noise. If you think about it as a probability density, you're right in the sense that given a vector it gives you a non-negative number that specifies the likelihood of that point.

因此我们想要构建实现这一目标的系统。这是一个分布，所以你可以从中采样。例如，你可以从中采样高斯分布，即一些高斯噪声。如果你将其视为概率密度，你的理解是正确的：给定一个向量，它会给出一个非负数来指定该点的似然值。

So now what we want to do is like an ODE is something deterministic. So at this point we will not be able to generate a whole distribution yet, but what we're basically doing is making the initial condition random now.

现在我们要做的是：ODE是确定性的。因此此刻我们还不能生成整个分布，但我们基本上是在让初始条件变得随机。

So what we're saying is x0 is sampled from P init. Thank you for that question because it basically means I told you P init is something we know, something we specify, something we design. So in this sense we could just, if it would be a normal distribution, you would go to a certain Python package like PyTorch and just sample, click on it, give me a sample for a normal distribution. So that's something we can easily do.

所以我们说x0是从P init中采样的。感谢这个问题，因为它基本上意味着我告诉过你们P init是我们已知、指定和设计的东西。因此从这个意义上说，如果它是正态分布，你可以直接使用某个Python包如PyTorch进行采样，点击它，给我一个正态分布的样本。这是我们可以轻松完成的事情。

And now what we do is essentially we have an ODE that is like before, so the time derivative of the trajectory is given by UT theta of XT, which means we're simulating this. And what is our goal? And that's going to be really what the fusion and flow models are all about is to say that the endpoint X1 has the distribution P data.

现在我们基本上做的是拥有一个如前所述的ODE，轨迹的时间导数由UT theta(XT)给出，这意味着我们在模拟这个过程。我们的目标是什么？这实际上正是融合模型和流模型的核心：让终点X1具有分布P data。

So let's think a bit quickly about a toy example. So we said we want to convert an initial distribution into a data distribution, and now this is as I said a toy example. So we have here now visualized a 2D distribution, so we think about a distribution as a density function. You basically see here the contour lines of that distribution.

让我们快速思考一个简单示例。我们说过想要将初始分布转换为数据分布，正如我所说这是一个简单示例。这里我们可视化了一个二维分布，我们将分布视为密度函数。你基本上可以在这里看到该分布的等高线。

And we start off with some Gaussian noise which is visualized here now in 2D. If you would do this in higher dimensions you would just get some like image like that which we call X zero. So we initialize and then our goal is to transform it to this data distribution with a vector field.

我们从一些高斯噪声开始，这里以二维形式可视化。如果在更高维度进行，你会得到类似图像的输出，我们称之为X0。我们进行初始化，然后目标是通过向量场将其转换到这个数据分布。

And the first two like three classes is basically like going to teach you how to do this, and the outcome of this is going to be exactly in this toy example this. So I told you that in red you see a flow, and in this case this flow is simulated with a neural network and it converts as you see initial distribution in this case into some data distribution, and you see that at the end, so now we're ending up with the distribution that we wanted. And of course, we're going to do this in much higher dimensions with, like you know, high resolution images later, but for this, the mathematics, the model is always the same. So there's no real difference in the sense like what it actually does.

前两三节课基本上会教你如何实现这一点，在这个简单示例中的结果将完全如图所示。我告诉过你们红色显示的是一个流，在这个案例中这个流是通过神经网络模拟的，正如你所见它将初始分布在这种情况下转化为某种数据分布，最终你会看到我们得到了想要的分布。当然，我们后续会在更高维度上进行这种操作，比如处理高分辨率图像，但就数学原理而言，模型始终是相同的。因此在实际功能上并没有本质区别。

So how to generate objects with a flow model? So what are we going to do? Oh yeah, question: the red grid is the flow. So you could think about the red grid as initial conditions. Let's think about like the bottom left point of this plot. So this one, it just basically over time moves with this left grid point. That's basically how it moves through time, and basically it warps this distribution into one that we want to have.

那么如何使用流模型生成对象？我们要做什么？哦对了，问题：红色网格代表流。你可以将红色网格视为初始条件。让我们考虑这个图表左下角的点。这个点会随着时间沿着左侧网格点移动，这就是它随时间移动的方式，本质上它将这个分布扭曲成我们想要的分布。

I'm not told you how to do this yet. This is our goal right? So like at this point, we've just randomly initialized the model and it's just nonsense, but like this is where we want to go. Um, oh yeah, your question was what the vector field is. So the one in blue, the red one is the flow, the blue one is the vector field. Okay cool.

我还没有告诉你们具体如何实现。这是我们的目标对吧？在现阶段，我们只是随机初始化了模型，结果毫无意义，但这就是我们想要达到的方向。嗯，对了，你刚才问的是向量场是什么。蓝色代表向量场，红色代表流。好的，很好。

So now, this might seem not so hard or might seem obvious because I'm just restating something I've said before, but this is important because you're going to need to implement this. And that's basically how we generate objects with a flow model. So if I would now give you a flow model, let's say meta movie generator that's a flow model, you would know now how to sample from this model.

现在这可能看起来不难或显而易见，因为我只是在重复之前说过的话，但这很重要，因为你们需要实现它。这基本上就是我们用流模型生成对象的方式。如果我现在给你们一个流模型，比如元电影生成器这样的流模型，你们现在就知道如何从这个模型中采样了。

It's basically we're initializing randomly with our initial distribution, we draw a Gaussian, and essentially what we're doing is we're looping over our Euler scheme. So basically at every time step, take a direction that's specified by our neural network. So we basically every time we evaluate the neural network at a point XT, we get some vector out of it, and we update our previous step with this vector field.

基本上我们使用初始分布进行随机初始化，抽取一个高斯分布，本质上我们是在循环执行欧拉方案。在每个时间步，取由神经网络指定的方向。每次我们在XT点评估神经网络时，会得到一个向量，然后用这个向量场更新前一步的状态。

That's the sampling algorithm. The sampling algorithm is rather simple, the training is going to be much harder, but the sampling is basically that. And what we return is the final time point. Maybe that's important to note that like that's what we're going to end up with. Good cool, that's it for flow models.

这就是采样算法。采样算法相当简单，训练会困难得多，但采样基本上就是这样。我们返回的是最终时间点。也许需要重点注意的是，这就是我们最终会得到的结果。很好，关于流模型就讲到这里。

And now we're going to go to the diffusion models. I'm going to pause briefly to make sure that we have no questions. All good, you can always ask questions, just raise your hand if you want.

现在我们将转向扩散模型。我稍作暂停，确保没有疑问。没问题，你们随时可以提问，只需举手示意。

But sure, question? It's the direction it gives you basically the change. It's the velocity that you have at a specific place and a specific time. Good cool. Yeah, another question for that vector field? So you mean what this notation means? Oh yes, I guess I haven't told you yet.

当然，有问题吗？它给出的方向本质上就是变化量。这是你在特定位置和特定时间具有的速度。很好。对那个向量场还有问题？你是问这个符号表示什么意思？哦是的，我猜我还没告诉你们。

All I'm saying is that for every parameter, you can think about a parameter as a vector of like let's say length K, but K doesn't really matter at this point. It's just some numbers that specify how I transform my input to output, which in this case means specifies the vector field.

我要说的是，对于每个参数，你可以将其视为一个长度为K的向量，但K在这里并不重要。这些数字只是规定了如何将输入转换为输出，在这种情况下就是指定了向量场。

And there's going to be a diversity of different architectures. It's just that this architecture usually differs by modality: for images, for videos, for proteins, all of this will have different neural network architectures. So we're going to talk about this once we talk about specific applications.

会有各种不同的架构。只是这种架构通常因模态而异：对于图像、视频、蛋白质，这些都会有不同的神经网络架构。我们将在讨论具体应用时再详细讲解。

Yeah, so you ask about boundary conditions. So a boundary condition is usually something that exists for partial differential equations or PDEs. In this case, an ODE is also a partial differential equation, but a boundary condition is the initial condition. It says the boundary at this point would be the time like Time Zero, which is the boundary of the time length line would be x0 equal x0.

是的，你问到了边界条件。边界条件通常存在于偏微分方程中。在这种情况下，常微分方程也是一种偏微分方程，但边界条件就是初始条件。它表示这个点的边界就是时间零点，即时间长度线的边界，也就是x0等于x0。

Yes yes, oh you mean why the red? So your question is why does the red is not defined under the whole space anymore? Because so what we're starting off with is with red grids populated across the entire space, but they then change over time. So there's still going to be a value that you can evaluate, but the initial points that have been specified by this red grid have moved in the meantime.

是的，哦你是问为什么红色不再定义在整个空间上？因为我们开始时红色网格遍布整个空间，但它们随后会随时间变化。所以你仍然可以评估某个值，但由这个红色网格指定的初始点在此期间已经移动了。

I would not say that's something I would not say that's a large F well. Like um um yes yeah, there's infinite like you say that definition the set where this function is defined is infinite in that sense. Well you know in a computer you have numerical precision right, there's like limits, but for this purposes like it's fine everywhere.

我不会说这是...嗯，是的，如你所说，从定义上看这个函数定义域是无限的。你知道在计算机中数值精度是有限的，但就这个目的而言，它在各处都是良好的。

Cool okay, so let's go to the diffusion models. And the diffusion models essentially extend the ideas that we just discussed but to stochastic differential equations. So to do that, we first need to define what a stochastic differential equation is, which is going to be most of the work now that we have to do.

好的，那么让我们开始讨论扩散模型。扩散模型本质上将我们刚刚讨论的思想扩展到了随机微分方程。为此，我们首先需要定义什么是随机微分方程，这将是现在我们需要完成的主要工作。

Ah yes yes, that's a great question actually. So what we are specifying: the neural network, the model like your PyTorch model or whatever you want to implement, is going to be a vector field. So in that sense, I don't quite like the terminology flow model because the neural network itself is the vector field. That's something important to keep in mind.

啊是的，这实际上是个很好的问题。我们指定的是：神经网络，比如你的PyTorch模型或任何你想实现的模型，将是一个向量场。从这个意义上说，我不太喜欢"流模型"这个术语，因为神经网络本身就是向量场。这是需要牢记的重要一点。

The flow is the map that we implicitly get by simulating this ODE. So like I told you before that a flow is basically a function that solves this ODE. So the flow map would be abstractly defined as whatever output map like output x0 I map to phi theta of x0. Now what phi theta is is essentially the flow that corresponds to this ODE.

流是我们通过模拟这个常微分方程隐式得到的映射。正如我之前告诉你们的，流基本上就是解这个常微分方程的函数。因此流映射可以抽象地定义为任意输出映射，比如将输入x0映射到φθ(x0)。这里的φθ本质上就是对应这个常微分方程的流。

So that's a flow model in that sense. We've not represented this as a neural network, we've just represented an algorithm which is this algorithm I've just showed you that implicitly computes this map. Yeah great question because there are also models that explicitly parameterize the flow but less popular nowadays because it's a bit hard to do so.

从这个意义上说，这就是流模型。我们并没有将其表示为神经网络，只是表示了一个算法，就是我刚才展示的隐式计算这个映射的算法。是的，很好的问题，因为也有显式参数化流的模型，但现在不太流行，因为这样做比较困难。

Good cool, so the diffusion models think about they use stochastic differential equations and we basically do the same game as before. So we're going to now think about what a trajectory is and why, because diffusion models are not inherently deterministic like ODEs. Their solutions are not trajectories but random trajectories, and the language that we use for random trajectories is that of a stochastic process.

很好，那么扩散模型考虑使用随机微分方程，我们基本上进行与之前相同的操作。现在我们要思考什么是轨迹以及为什么，因为扩散模型不像常微分方程那样本质上是确定性的。它们的解不是轨迹而是随机轨迹，我们用于描述随机轨迹的语言是随机过程。

So you should think about a stochastic process basically as a random trajectory. What's the stochastic process? We have a random variable XT where in this case T is bigger than zero and in most cases is going to be from 0 to 1. So you can have any time convention here, but most of the time you're going to think from 0 to 1. Essentially we have the same thing as before - we have a trajectory X that is a function of time that goes from time to Rd and maps a time step T to XT which is a vector. But in this case we can draw samples from X, so X itself is random.

因此你应该将随机过程基本理解为一个随机轨迹。什么是随机过程？我们有一个随机变量XT，其中T大于零，在大多数情况下是从0到1。你可以使用任何时间约定，但大多数时候你会考虑从0到1。本质上我们拥有与之前相同的东西 - 我们有一个轨迹X，它是时间的函数，从时间映射到Rd，并将时间步长T映射到向量XT。但在这种情况下我们可以从X中抽取样本，因此X本身是随机的。

Okay, so stochastic process is a random trajectory - that's important to keep in mind. How to visualize this? Same as before, we start off at some point and then we go through space which would be a trajectory that would be one draw of that stochastic process. Now I take another draw that might look slightly different. So stochastic process if I do the same trajectory twice, I might get something different. It's important to keep in mind there's no one trajectory we're following here. The collection of these trajectories are more like how likely they are - that's a stochastic process.

好的，随机过程是一个随机轨迹 - 记住这一点很重要。如何可视化？和之前一样，我们从某个点开始，然后穿过空间，这将成为该随机过程的一次抽取轨迹。现在我进行另一次抽取，可能看起来略有不同。因此，如果我两次执行相同的轨迹，随机过程可能会得到不同的结果。重要的是要记住，这里我们并不遵循单一轨迹。这些轨迹的集合更像是它们的可能性程度 - 这就是随机过程。

All right, so the other ingredient we need is what's called a vector field. I don't need to define this anymore - I've just done it. It's the same thing as for ODE, so the key ingredient will be the same which is a vector field U that looks exactly like before - it has a spatial component and a time component and it returns a vector. This is basically what we've done before, so vector field we've already seen.

好的，我们需要的另一个要素是所谓的向量场。我不需要再定义它了 - 刚刚已经完成。它与常微分方程中的相同，因此关键要素将是一样的，即向量场U，看起来和之前完全一样 - 它具有空间分量和时间分量，并返回一个向量。这基本上是我们之前做过的，所以向量场我们已经见过。

And there's now though a second component and that's new - that's what's called a diffusion coefficient. What's the diffusion coefficient? In this class we always going to call it Sigma and it's being a function of time. So it's like for every time it gives you a value and it gives you a value that's a real number. So for every time you get a value Sigma T, and that's what's called the diffusion coefficient. You can make this also more complex, but at least in machine learning most of the time this object is like this.

但现在有第二个组成部分，这是新的 - 这就是所谓的扩散系数。什么是扩散系数？在这门课中我们总是称它为Sigma，它是一个时间的函数。因此对于每个时间点，它都会给你一个值，这个值是一个实数。所以对于每个时间点，你都会得到一个值Sigma T，这就是所谓的扩散系数。你可以让它更复杂，但至少在机器学习中，大多数情况下这个对象都是这样的。

The idea of the diffusion coefficient is basically we're injecting randomness or stochasticity into our ODE. That's basically what the role of this diffusion coefficient is. I should say that this is bigger, this is non-negative. So if Sigma would be zero, we would not inject any noise, we would not inject any randomness. If Sigma is very large, we inject more noise.

扩散系数的概念基本上是我们正在向常微分方程中注入随机性或随机性。这基本上就是这个扩散系数的作用。我应该说这个更大，这是非负的。因此，如果Sigma为零，我们就不会注入任何噪声，不会注入任何随机性。如果Sigma非常大，我们会注入更多噪声。

And now I'm going to write down what a stochastic differential equation is. It's not going to be clear exactly what it means, but I at least want to introduce the notation. The same way as before we will have an initial condition, so we say that our trajectory or in this case stochastic process is going to start off at x0.

现在我要写下什么是随机微分方程。它的确切含义可能不太清楚，但至少我想介绍一下这个符号。和之前一样，我们将有一个初始条件，所以我们说我们的轨迹，或者在这种情况下是随机过程，将从x0开始。

But now something different comes in. Maybe I should say you could also make this random, so you could also make the initial condition random. Again in this case just going to have a fixed initial condition. But this condition just says stochastic process always for every draw of X will start at x0.

但现在出现了一些不同的东西。也许我应该说，你也可以让它随机化，所以你也可以让初始条件随机化。再次说明，在这种情况下只是使用固定的初始条件。但这个条件只是说明随机过程对于X的每次抽取都将从x0开始。

And now the SDE has - sorry, stochastic differential equation is a bit long, people call it SDE. So whenever I say SDE I mean stochastic differential equation. And the notation that we use here is - let me make this very clear - we're going to use what's called the dX notation, that's the symbolic notation.

现在SDE有 - 抱歉，随机微分方程有点长，人们称之为SDE。所以每当我提到SDE时，我指的是随机微分方程。我们这里使用的符号是 - 让我说清楚 - 我们将使用所谓的dX符号，这是符号表示法。

And it basically says that the change of XT in time is given by vector field. So the change of XT time is given by the change of a vector field plus the change of something that we don't know what it is yet, but I still want to write it down.

它基本上说明XT随时间的变化是由向量场给出的。因此XT时间的变化是由向量场的变化加上我们还不知道是什么的东西的变化给出的，但我仍然想把它写下来。

So what does this notation say? For now I can just tell you what it intuitively means. The change of XT is given by the change of a vector field, so we're going to the direction of a vector field, plus we're injecting some stochastic component now. So like you can also think about like injecting noise into this equation.

那么这个符号表示什么？现在我只能告诉你它的直观含义。XT的变化是由向量场的变化给出的，所以我们正朝着向量场的方向前进，再加上我们现在正在注入一些随机成分。所以你也可以想象成向这个方程中注入噪声。

And the first component is essentially an ODE, and we're going to see this in a second. But basically you can think about it as injecting going into direction of a vector field, inject it with some noise.

第一个分量本质上是一个常微分方程，我们马上就会看到这一点。但基本上你可以把它想象成朝着向量场的方向前进，并注入一些噪声。

And in this equation there's a few things that you probably haven't seen before. The W here is what's called a Brownian motion and we're going to get to know this object in a second. And we denote this as W.

在这个方程中有一些你可能以前没见过的东西。这里的W就是所谓的布朗运动，我们马上就会了解这个对象。我们将其表示为W。

Does somebody know why we call it W? Yes, so the answer is why we call it W is because it's also called a Wiener process, and Wiener was mathematician at MIT. You actually still see some of his pictures at the math department hanging around. There's also I think a bust - there's like you see pictures of him still around here. So he was studying that process, it's also called a Wiener process, and that's why we call it with a W.

有人知道我们为什么称它为W吗？是的，答案是我们称它为W是因为它也被称为维纳过程，维纳是麻省理工学院的数学家。你实际上仍然可以在数学系看到一些他的照片挂在那里。我想还有一个半身像 - 你仍然可以在这里看到他的照片。所以他研究了这个过程，它也被称为维纳过程，这就是为什么我们用W来称呼它。

So now what I want to do is I want to explain what this equation means go step by step. Any questions at this point? I guess I haven't answered all questions yet, so I'll keep going for now.

所以现在我想做的是逐步解释这个方程的含义。在这一点上有什么问题吗？我想我还没有回答所有问题，所以我现在继续。

Okay, let's see. Brownian motion - so you should think about a Brownian motion as something like a random walk. So it's basically you've seen maybe like a random walk that goes at every step a bit forth and back randomly with equal chance, and it's basically a continuous equivalent of that but now in a continuous space and continuous time.

好的，让我们看看。布朗运动 - 所以你应该将布朗运动想象成类似随机游走的东西。所以基本上你可能见过像随机游走这样的东西，每一步都以相等的概率随机前进和后退，而这基本上是它的连续等价物，但现在是在连续空间和连续时间中。

So let's see how we can define this. So it's a stochastic process we just learned what that is - it gives you random trajectories. In this case the time can go infinitely long, we don't have to stop at time one.

那么让我们看看如何定义这个。所以它是一个随机过程，我们刚刚了解了那是什么 - 它给你随机轨迹。在这种情况下，时间可以无限长，我们不必在时间1停止。

And it has several conditions. So the first one is that we initialize it at zero, so W0 is just zero. So this Brownian motion will always start at zero, which you can see here on the plot on the right - it starts at time zero, it's at time zero it's at zero.

它有几个条件。第一个是我们在零处初始化它，所以W0就是零。因此这个布朗运动总是从零开始，你可以在右边的图上看到这一点 - 它在时间零开始，在时间零时它处于零。

The second thing is that what we call it has Gaussian increments. What does that mean? It means that WT minus WS, which is what's called an increment, has distribution that's specified by normal distribution and it has this form.

第二件事是它被称为具有高斯增量。这是什么意思？这意味着WT减去WS（这被称为增量）具有由正态分布指定的分布，并且具有这种形式。

So what I took here is I took two arbitrary time points s and t, t is after s, and I say what's the difference starting from s to the next time point t? And we're basically saying that this has a normal distribution like a Gaussian distribution, but with a variance that basically increases linearly in time.

所以我在这里取的是两个任意时间点s和t，t在s之后，然后我问从时间点s到下一个时间点t有什么区别？我们基本上是说这具有正态分布，即高斯分布，但其方差基本上随时间线性增加。

So let's think about this: if T would be equal to S, then it would have zero variance because we're just where we are. The further we go in time, the more insecurity there is, the more the variance of this distribution increases.

让我们思考一下：如果T等于S，那么方差将为零，因为我们就在当前位置。时间推进得越远，不确定性就越大，该分布的方差也就越大。

Another important thing is that it has independent increments, and that might be a bit of an unusual condition if you haven't seen this before.

另一个重要特性是它具有独立增量，如果你之前没接触过这个概念，可能会觉得有点不寻常。

So let's define it: I will take a range of increments over time and say that they are independent. Independent means that they're stochastically independent considered as random variables.

那么我们来定义一下：我将取一段时间内的增量范围，并说明它们是独立的。独立意味着作为随机变量，它们在随机意义上是独立的。

You could also say that they have no information between these points. It's a bit stronger condition than having no information, basically saying they are stochastically independent random variables.

你也可以说这些点之间没有信息关联。这比单纯没有信息关联的条件更强，基本上是说它们是随机独立的随机变量。

If you've done probability theory, you will understand what that means. If not, don't worry about it - we will see how to simulate this in a second.

如果你学过概率论，你会理解这意味着什么。如果没学过也不用担心 - 我们马上就会看到如何模拟这个过程。

And I should make the condition that these time points are increasing, so T0, T1 to TN are increasing in time.

我还应该说明这些时间点是递增的，即从T0到T1再到TN是随时间递增的。

If you're taking a machine learning class, maybe you've heard of a Gaussian process - that's a term that's commonly used, and that's a specific Gaussian process.

如果你正在上机器学习课程，可能听说过高斯过程 - 这是一个常用术语，而布朗运动就是一个特定的高斯过程。

Brownian motion is a specific Gaussian process. It looks a bit different than the ones you've seen because it's not smooth, but it's a specific process.

布朗运动是一个特定的高斯过程。它看起来与你见过的其他过程有些不同，因为它不平滑，但它是一个特定的过程。

You've heard of a kernel from a Gaussian process - what I'm doing is I'm specifying a specific kernel of Brownian motion of a Gaussian process.

你听说过高斯过程的核函数 - 我现在做的就是指定高斯过程中布朗运动的一个特定核函数。

Great question - same as the arbitrary stochastic process: for every draw of W and for every time point, it's going to be a vector in R^D. This is a vector-valued object.

很好的问题 - 与任意随机过程相同：对于W的每次抽样和每个时间点，它将是R^D空间中的一个向量。这是一个向量值对象。

Here I plotted it for dimension one or D equals 1, but it can be generally defined for arbitrary dimensions.

这里我绘制的是维度为一或D等于1的情况，但它可以普遍定义为任意维度。

With this, we're basically ready to now understand what this other equation means. I call this the dXt notation - that's not a technical term, that's something I pointed out.

有了这些，我们基本上准备好理解另一个方程的含义了。我称之为dXt表示法 - 这不是一个技术术语，而是我指出的一个概念。

It basically tries to describe what the symbolic notation is that this SDE describes - like what the heck does this dXt mean?

它基本上试图描述这个随机微分方程的符号表示法是什么 - 比如这个dXt到底是什么意思？

First of all, a few fun facts: Brownian motion has continuous paths actually - that's one of the conditions.

首先，一些有趣的事实：布朗运动实际上具有连续路径 - 这是条件之一。

You can see that if you would draw these paths, you could draw them without ever lifting your pen, which intuitively means what a continuous function means.

你可以看到，如果你绘制这些路径，你可以不抬笔地连续绘制，这直观地表明了连续函数的含义。

But these paths are infinitely long, so if I would draw them, I would stand here for an eternity because that would never stop.

但这些路径是无限长的，所以如果我要绘制它们，我会永远站在这里，因为绘制永远不会停止。

On the one hand, that's just a cool fun fact about it, which kind of resembles what's called a fractal - maybe you've seen this before.

一方面，这只是一个有趣的事实，它有点像所谓的分形 - 也许你以前见过这个。

But these really unique properties about these paths make it differentiable nowhere, so you cannot take derivatives of this object.

但这些路径的真正独特性质使其处处不可微，因此你无法对这个对象求导。

Why am I saying this? Because so far we were studying differential equations which rely on taking derivatives.

我为什么这么说？因为到目前为止，我们一直在研究依赖于求导的微分方程。

We want to use Brownian motion for specifying a differential equation, but we suddenly cannot use derivatives anymore because this object has no derivatives.

我们想用布朗运动来指定微分方程，但我们突然不能再使用导数了，因为这个对象没有导数。

The dynamics are inherently stochastic, so that's why the first thing we need to do is we need to basically describe an ODE in a form that does not rely on derivatives - that's step number one.

动力学本质上是随机的，所以这就是为什么我们需要做的第一件事是以不依赖于导数的形式描述一个常微分方程 - 这是第一步。

Let's do that: we saw that ODE is given by something like this - the time derivative of a trajectory Xt is given by Ut of Xt.

让我们开始吧：我们看到常微分方程是这样给出的 - 轨迹Xt的时间导数由Xt的Ut给出。

Now what I make the claim is that this is equivalent to the following formula: that Xt plus h is given by Xt plus h times Ut of Xt plus some error term or remainder term, what I call Rt of h here.

现在我主张这等价于以下公式：Xt加h等于Xt加上h乘以Xt的Ut，再加上某个误差项或余项，我在这里称之为h的Rt。

Which basically, if you take the limit to zero, goes to zero. So what I'm saying is that a trajectory has this ODE is equivalent to that the next time step for some small h is this previous time step plus h times in the direction of this vector field plus some reminder term that I can ignore.

基本上，如果你取极限趋近于零，这个项就会趋近于零。所以我说的是，具有这个常微分方程的轨迹等价于：对于某个小的h，下一个时间步等于前一个时间步加上h乘以这个向量场方向，再加上一些我可以忽略的余项。

Maybe you've seen this in other notation - some people write something like for this error term write something like small o of h or capital O of h squared.

也许你在其他记法中见过这个 - 有些人把这个误差项写成小o(h)或大O(h²)。

Think about this as a Taylor approximation - is that clear or shall I derive that?

可以把这个看作泰勒近似 - 这样清楚吗，或者需要我推导一下？

I can quickly write it down: the answer why that is is that we have to think back about how derivatives were defined in the first place.

我可以快速写下来：之所以如此，是因为我们需要回顾导数最初是如何定义的。

What does it mean to take the derivative of a trajectory Xt? If you think back to your multivariate calculus class, let's say you have a function like this big function, and how do we define derivative at a point T?

对轨迹Xt求导意味着什么？如果你回想一下多元微积分课程，假设你有这样一个函数，我们如何定义在点T处的导数？

We're taking the next time step t plus h, we're taking Xt plus h, taking Xt, and we're taking the difference between these two, and then take this what's called differential quotient, and then basically take the limit for that to zero.

我们取下一个时间步t加h，取Xt加h，取Xt，然后取这两者之间的差值，接着取所谓的差商，然后基本上取这个差商趋近于零的极限。

What we are saying is that this is equivalent to the limit of h goes to zero of (Xt plus h minus Xt) divided by h is equal to Ut of Xt.

我们说的是，这等价于当h趋近于零时，(Xt加h减去Xt)除以h的极限等于Xt的Ut。

So far, what I've done here is I've used the definition of a derivative - it's usually not something you use every day, but that's how a derivative is defined.

到目前为止，我在这里所做的是使用了导数的定义 - 这通常不是你日常使用的东西，但这就是导数的定义方式。

What we can do with that is we can say this is equivalent to (Xt plus h minus Xt) divided by h equals Ut of Xt plus an error term Rt of h, which basically means that whatever this term is essentially goes to zero.

我们可以由此说这等价于(Xt加h减去Xt)除以h等于Xt的Ut加上误差项h的Rt，这基本上意味着无论这个项是什么，它最终都会趋近于零。

Now what I do is essentially do some algebra: I'm just taking the h here, multiplying that with h, and then adding Xt, and that's the equation we end up with.

现在我要做的就是进行一些代数运算：我取这里的h，乘以h，然后加上Xt，这就是我们最终得到的方程。

From this equation to this equation, it's just algebra - I'm multiplying with h, I'm adding up Xt. All right, if this is not entirely clear, it doesn't matter because what's going to matter is what's going to come now. This is more like motivation for what's about to come.

从这个方程到这个方程，只是代数运算 - 我乘以h，加上Xt。好的，如果这部分内容不完全清晰也没关系，因为真正重要的是接下来要讲的内容。这更像是为即将展开的内容所做的铺垫。

All right, question. Oh, I'm still in the case for ODEs, but that's a great point. It's going to be a random, but for now, it's going to be a deterministic term because we're still on the case of ODE. But you get your eye is good because it's going to be random. R is a term I don't specify. I'm just saying whatever that is, it's just sometimes something that goes to zero when I make age go to zero.

好的，有问题。哦，我还在讨论常微分方程的情况，但这一点提得很好。它最终会是随机的，但目前它是一个确定性项，因为我们仍在讨论常微分方程的情形。但你的眼光很准，因为它最终会是随机的。R 是一个我不具体说明的项。我只是说，无论它是什么，它只是有时会在我让 h 趋近于零时也趋近于零的东西。

So this is like if for very small age, it's going to be essentially zero. All I'm saying is that's an error term that you can effectively ignore. That's basically what I say. Good, cool.

所以这就像是，对于非常小的 h，它基本上就是零。我所说的全部意思是，这是一个你实际上可以忽略的误差项。这基本上就是我的意思。好的，很好。

Okay, so now we're going to make this random, and now we're going to do the same thing as before. I'm going to write down now this notation that I've used for the SDE.

好的，现在我们要让它变得随机，并且我们要做和之前一样的事情。我现在要写下这个我用于表示随机微分方程的符号。

So what I've done now instead of writing down ODE, I've wrote this SDE down, and I'm doing now the same thing when injecting noise into this formula. This formula we cannot generalize really because it's like derivatives. We don't know how to do this for stochastic processes in this form, but we know how to do this for like this is a form that does not rely on derivatives.

所以我现在做的是，不再写常微分方程，而是写下这个随机微分方程，并且当向这个公式中注入噪声时，我现在在做同样的事情。这个公式我们实际上无法推广，因为它涉及到导数。我们不知道如何以这种形式对随机过程做这件事，但我们知道如何处理这种不依赖于导数的形式。

Right, I told you we need something that does not rely on derivatives. That's something that I'm not telling you what a derivative here is. I'm just saying that like the next time step is specified by this plus some negligible error term.

是的，我告诉过你我们需要不依赖于导数的方法。这里我并没有告诉你导数是什么。我只是说，下一个时间步是由这个加上一些可忽略的误差项来指定的。

And now I can make this random. What I can do is I can say XT plus h is given by XT plus h times UT XT plus, and now that's the new term that I want to highlight in color, Sigma T. And now the Brownian motion comes in, WT plus h minus WT, and I still need the error term plus h times error term.

现在我可以让它变得随机。我能做的是，我可以说 XT+h 由 XT 加上 h 乘以 UT(XT) 给出，再加上——这是我想用颜色高亮显示的新项——Sigma T。现在布朗运动登场了，WT+h 减去 WT，并且我仍然需要误差项，加上 h 乘以误差项。

And great point, this thing is going to be random now. So what does it mean for this to have order zero? It means that its standard deviation goes to zero. We need to... this is a random object. Cannot deal with the fact that it's... oop... um, so what's written here is that the square expected square norm and square root of that is going to zero.

说得好，现在这个东西将是随机的。那么它为零阶是什么意思呢？意思是它的标准差趋近于零。我们需要……这是一个随机对象。不能处理它是……呃……嗯，所以这里写的是，期望平方范数的平方根趋近于零。

So let's recap. We knew what an ODE is that we specified with derivatives. We found a formula that is not relying on derivatives but that relies on just going a step forward in time H that's rather small, and we can specify that up to an error term Rth the right-hand side.

让我们回顾一下。我们知道用导数定义的常微分方程是什么。我们找到了一个不依赖于导数，而是依赖于在时间上向前迈出一小步 H 的公式，并且我们可以在一个误差项 R 的精度内指定这个公式的右边。

We can make random that's something we have. We can inject noise. The idea of this notation is essentially to add noise to this equation.

我们可以让它随机化，这是我们能做到的。我们可以注入噪声。这种符号表示法的本质思想就是给这个方程添加噪声。

So we say that the next time step is the previous time step plus some H given going to this vector field plus some increment from a Brownian motion, which is effectively noise that we scale now by the diffusion coefficient Sigma T.

所以我们说，下一个时间步是前一个时间步加上某个 H 乘以这个向量场，再加上来自布朗运动的某个增量，这实际上就是噪声，我们现在用扩散系数 Sigma T 对其进行缩放。

So the more if Sigma T is zero, this is just zero. And what is... what if Sigma T is zero? What have I written down? An ODE exactly. So like if Sigma T is zero, that's just that, and we've learned that this is equivalent to this, which is an ODE, right?

所以，如果 Sigma T 是零，那么这项就是零。那么……如果 Sigma T 是零呢？我写下了什么？正是一个常微分方程。所以如果 Sigma T 是零，那就只剩下这个，并且我们已经知道这等价于这个，也就是一个常微分方程，对吧？

So for Sigma T bigger than zero, I've added a term that I told you is a Brownian motion, and I'm basically adding this on top of that plus some negligible error term.

所以对于 Sigma T 大于零的情况，我添加了一个我告诉过你是布朗运动的项，我基本上是在那之上加上这个，再加上一些可忽略的误差项。

Okay, and that's essentially what this notation means. That's how you should think about it. I'll show you, to be honest with you, most of the time I think about actually an SDE as something how I would simulate it, and I will show you that in a second.

好的，这基本上就是这个符号表示法的含义。这就是你应该如何理解它。老实说，我会告诉你，大多数时候我实际上是把随机微分方程看作是我会如何模拟它的东西，我稍后会展示给你看。

But this is basically you could think about this as a Taylor approximation. It's not really a Taylor approximation of this thing, but this equivalent statement in the same way that this is equivalent to this. This is equivalent to this.

但这基本上你可以把它看作是一种泰勒近似。它并不真正是这个东西的泰勒近似，但这种等价陈述的方式，就像这个等价于这个一样。这个等价于这个。

Maybe let's compare quickly about like ODEs and SDE again. So we've seen ODEs have trajectories as solutions. What's the solution of an SDE? It's a stochastic process. It's a random trajectory.

也许让我们再快速比较一下常微分方程和随机微分方程。我们已经看到常微分方程的解是轨迹。随机微分方程的解是什么？它是一个随机过程。是一条随机轨迹。

To define an ODE, we need a vector field. To define an SDE, we need a vector field and a diffusion coefficient, so we need an extra component.

要定义一个常微分方程，我们需要一个向量场。要定义一个随机微分方程，我们需要一个向量场和一个扩散系数，所以我们需要一个额外的组成部分。

An ODE is basically defined by a derivative and an initial condition, so it says that the derivative of XT is given by the specified vector field. The SDE also has an initial condition, and it says that the direction of XT or small incremental changes of XT are given by small directions in the step of a vector field plus small noise or steps added from a Brownian motion like small increments add.

常微分方程基本上是由一个导数和一个初始条件定义的，所以它说 XT 的导数由指定的向量场给出。随机微分方程也有一个初始条件，并且它说 XT 的方向或 XT 的微小增量变化是由向量场步长中的微小方向加上来自布朗运动的微小噪声或步长给出的，就像微小增量的叠加。

So we're injecting stochastic noise in that, which essentially says that we impose a condition on the stochastic process which basically says that this holds.

所以我们是在其中注入随机噪声，这实质上意味着我们对随机过程施加了一个条件，基本上就是说这个等式成立。

I've not told you that this thing exists. I've not constructed it to you. Just saying this is condition that we impose. Question? Yes, yes. At this point, I've just brought you done math. I didn't give you an algorithm yet.

我还没有告诉你这个东西存在。我还没有为你构造它。只是说这是我们施加的一个条件。有问题吗？是的，是的。在这一点上，我只是带你完成了数学部分。我还没有给你一个算法。

So the question is like how do we sample from this, right? And that's a great thing for the next step. Thank you. So, basically, what is the distribution of this object?

所以问题是我们如何从中采样，对吧？这是下一步要讨论的好问题。谢谢。那么，基本上，这个对象的分布是什么？

I wrote this down. Let's look, let's look back. The distribution of an increment what I use here is given by a Gaussian variable with variance of the difference in time. The difference in time of these two things is H, so this object has distribution has a Gaussian distribution with variance H or a covariance matrix like H times the identity matrix.

我把它写下来了。让我们看看，让我们回顾一下。我这里使用的增量的分布是由一个具有时间差方差的高斯变量给出的。这两者之间的时间差是 H，所以这个对象的分布是一个方差为 H 的高斯分布，或者说协方差矩阵是 H 乘以单位矩阵。

So to sample from this, you would sample from this distribution, which is something we know how to do. Okay, I'll show you an algorithm in a second.

所以要从中采样，你需要从这个分布中采样，这是我们知道如何做的事情。好的，我稍后会给你看一个算法。

So now we do the same exercise that we did for ODEs for SDEs. So if I show you this equation and I'm claiming I'm imposing a conditional something, I should tell you, does it have a solution and is that solution unique?

所以现在我们为随机微分方程做我们为常微分方程做的同样的练习。所以，如果我给你看这个方程，并且我声称我正在施加一个条件，我应该告诉你，它是否有解，并且这个解是否唯一？

So and basically the same thing as before holds. So if the vector field UT is continuously differentiable with bounded derivatives, then a unique solution to this SDE exists.

基本上，和之前一样的情况成立。所以如果向量场 UT 是连续可微的并且导数有界，那么这个随机微分方程就存在唯一解。

More generally again, if you know what a Lipschitz condition is, means that this SDE has unique solution if this vector field is Lipschitz.

再次更一般地说，如果你知道利普希茨条件是什么，意思是如果这个向量场是利普希茨连续的，那么这个随机微分方程就有唯一解。

The key takeaway is that in the cases of practical interest for our purposes, there will be unique solutions to this equation. So there will be exactly one stochastic process that satisfies this condition, or in other words, satisfies this stochastic differential equation. So you can assume that whatever you write down in such a form will have a unique solution.

关键要点在于，对于我们实际关注的情况，这个方程将存在唯一解。因此将恰好有一个随机过程满足这个条件，或者说满足这个随机微分方程。所以你可以假设任何以这种形式写出的方程都会存在唯一解。

If this was a stochastic calculus class, which it is not, we would try to construct this now explicitly. There's a thing called stochastic integrals, and you can construct this in the same way you construct Riemann integrals with Riemann sums. You can find something like Euler sums and then construct this explicitly. For the purpose of this class, this is like an equivalent statement I just haven't shown you yet that I can have a solution to this.

如果这是一门随机微积分课程（实际上并不是），我们现在会尝试显式地构造这个解。有一种叫做随机积分的东西，你可以用构造黎曼积分与黎曼和的相同方式来构造它。你可以找到类似欧拉和的方法，然后显式地构造这个解。对于本课程的目的来说，这就像一个等效的陈述，我还没有向你们展示我可以得到这个方程的解。

Good, cool. So thank you for the question about simulation. We now need to think about something we can plug into a computer: how do we sample from this? We know that this is a normal distribution with variance H, and we know how to sample from it. So now let's iterate that and arrive at an algorithm that's called the Euler-Maruyama method, essentially an extension of the Euler method that we've seen before. So it basically describes how to sample from an SDE.

很好，很酷。感谢关于模拟的问题。我们现在需要考虑一些可以输入计算机的东西：我们如何从中采样？我们知道这是一个方差为H的正态分布，并且我们知道如何从中采样。现在让我们迭代这个过程，得到一个叫做欧拉-丸山方法的算法，这基本上是我们之前见过的欧拉方法的扩展。因此它基本上描述了如何从随机微分方程中采样。

What have we given? We've given a vector field UT, we have a certain number of steps, and we have a diffusion coefficient Sigma T. We start at time equal zero and we want to simulate until time one, so our step size will be basically one divided by the number of steps. We set the initial condition x0 to b0, so we're imposing the condition we have the initial condition I have written above here.

我们已知什么？我们已知一个向量场UT，一定数量的步数，以及一个扩散系数Sigma T。我们从时间零开始，想要模拟到时间一，所以我们的步长基本上是一除以步数。我们将初始条件x0设为b0，这样我们就施加了我上面写出的初始条件。

Then we loop over the number of steps. We draw a sample from a Gaussian standard Gaussian, so basically you could call some Python package if you do this in Python. And you say the next time step XT plus h is given by the previous time step plus h times UT of X. Until that time it's just ODE simulation, and now what we're doing is we're adding some noise.

然后我们循环遍历步数。我们从高斯标准正态分布中抽取一个样本，所以基本上如果你用Python做这个，你可以调用一些Python包。然后你说下一个时间步XT加h等于前一个时间步加上h乘以UT(X)。在此之前这只是常微分方程模拟，而现在我们正在添加一些噪声。

So we're adding, we're scaling the standard Gaussian with square root of H. Why square root of H? Because we want to have its variance to be H. If I scale a variable with variance one with square root of H, its variance will be H. Variance scales quadratically, that's why we need square root of H. And then we need to scale it by the diffusion coefficient Sigma T.

所以我们添加噪声时，用根号H来缩放标准高斯分布。为什么是根号H？因为我们希望它的方差是H。如果我用根号H来缩放一个方差为一的变量，它的方差就会变成H。方差是按平方比例缩放的，这就是为什么我们需要根号H。然后我们需要用扩散系数Sigma T来缩放它。

Then we loop over this over many time steps to arrive at the final trajectory that we can then return, and you'll implement this in the lab. So if you have any questions, best to ask now.

然后我们循环这个过程多次时间步，得到最终轨迹并返回，你们将在实验中实现这个。所以如果你们有任何问题，最好现在提问。

Yes, to make this completely rigorous, if you would take a stochastic calculus class, what you need to do first of all is you need to show that a Brownian motion exists. There's a diversity of different construction methods, that's not trivial. And the second thing is then you need this construction that I mentioned with the Euler Riemann sums. You need to basically explicitly construct stochastic integral, and then you show that this basically fulfills that condition that the stochastic process exists.

是的，为了使这完全严谨，如果你上一门随机微积分课程，首先你需要证明布朗运动存在。有各种不同的构造方法，这并不简单。第二件事是你需要我提到的带有欧拉黎曼和的构造。你需要基本上显式地构造随机积分，然后你证明这基本上满足随机过程存在的条件。

So you construct it explicitly with a Brownian motion, and then you have these Riemann sums or Euler sums, and then you need to define what it means to converge in this space of random trajectories. This is not trivial, and then you arrive at something that fulfills this condition.

所以你用布朗运动显式地构造它，然后你有这些黎曼和或欧拉和，然后你需要定义在这个随机轨迹空间中收敛意味着什么。这并不简单，然后你得到满足这个条件的东西。

Sigma T can be a function of X actually. I've written a paper on this, so you can do this. You can also learn it, but we want to teach you here the state of the art that's currently at the frontier and leave out some exotic stuff, so that's why we're not doing this. But great point, we can make it state dependent.

Sigma T实际上可以是X的函数。我写过一篇关于这个的论文，所以你可以这样做。你也可以学习它，但我们想在这里教你们当前处于前沿的最新技术，并省略一些特殊的内容，这就是为什么我们不这样做。但很好的观点，我们可以让它成为状态依赖的。

Yes, great question. So I said that we need to add this increment, and I'm telling you that the increment has variance H times identity matrix, and I sampled here something with variance one. So if I scale it with square root of H, its variance will scale with square root H squared, which will be H. So that's a bit intuitive at first, why square root of H, but because we want to have the variance increasing linear in time, not the standard deviation.

是的，很好的问题。我说过我们需要添加这个增量，我告诉你们这个增量具有方差H乘以单位矩阵，而我在这里采样的是方差为一的东西。所以如果我用根号H来缩放它，它的方差将按根号H的平方比例缩放，也就是H。所以一开始这有点直观，为什么是根号H，但因为我们希望方差随时间线性增加，而不是标准差。

Yes, yes, yes. Okay, so question is what does it mean to be a unique solution? So you're right, there's many many different trajectories, but I told you the stochastic process describes the distribution of these trajectories. So now to actually formally define this, what I would need to do is we need to define what's the distribution over trajectories. For that you need measure theory, you need to define a whole bunch of technical things.

是的，是的，是的。好的，问题是唯一解意味着什么？你说得对，存在许多不同的轨迹，但我告诉过你们随机过程描述了这些轨迹的分布。所以现在要正式定义这个，我需要做的是定义轨迹上的分布是什么。为此你需要测度论，你需要定义一大堆技术性的东西。

So I think it's not necessarily needed for actually doing machine learning, but you need to define that and then you say that these distributions that you then define over this space of trajectories would be the same. So as you can imagine, it's not quite... I could spend a whole class on this, which I'm not going to.

所以我认为对于实际进行机器学习来说并不一定需要，但你需要定义它，然后你说你在这个轨迹空间上定义的这些分布将是相同的。所以你可以想象，这并不完全...我可以花一整门课来讲这个，但我不会这样做。

Cool, okay. But the way to think about this is the way I think about this really: when I think about SDEs I think about something like in this format. I think about I go to the next step in the direction of the vector field while adding a bit of noise, and as soon as my vector field is nice enough, this thing has unique solution.

好的，很酷。但思考这个问题的方式就是我真正思考的方式：当我思考随机微分方程时，我想到的是这种格式的东西。我想到的是我沿着向量场的方向前进一步，同时添加一点噪声，只要我的向量场足够好，这个东西就有唯一解。

Good, cool. Let's consider an example. So I've showed you before the example of a linear vector field, and now we're looking at an equivalent construction of that but for stochastic differential equations. So what I'm doing is I'm saying my vector field is minus theta of the state XT, so we've seen this before, but now we're saying we're adding a constant diffusion coefficient so Sigma is just a constant in time.

很好，很酷。让我们考虑一个例子。我之前给你们展示过线性向量场的例子，现在我们在看一个等效的构造，但是针对随机微分方程的。所以我做的是说我的向量场是负theta乘以状态XT，这个我们之前见过，但现在我们说我们添加一个常数扩散系数，所以Sigma只是一个时间上的常数。

That's going to be all, every time I'm going to inject the same amount of noise, and that's called an Ornstein-Uhlenbeck process. You might have seen this, I think it's Danish scientist who found this, and it basically describes this linear ODE, this exponential decay that we've seen before, but now with some noise injected.

这将是一直如此，每次我将注入相同量的噪声，这被称为奥恩斯坦-乌伦贝克过程。你们可能见过这个，我想是丹麦科学家发现的，它基本上描述了这个线性常微分方程，我们之前见过的指数衰减，但现在注入了一些噪声。

So if we have zero noise, if Sigma is zero, we just have an ODE right? So if this Sigma is zero, we've learned already that the SDE just basically reverts back to the ODE case, and this is the plot I've showed you earlier.

所以如果我们没有噪声，如果Sigma为零，我们就只有一个常微分方程，对吧？我们已经知道当Sigma为零时，随机微分方程基本上就回到了常微分方程的情况，这就是我之前给你们看的图。

And now you see if I increase the diffusion coefficient over time, you see there's more and more stochasticity that I'm injecting into this process. You go from left to right, so you get more and more noise. It's going to be more and more random, and that's basically what this process describes is kind of this exponential convergence to something.

现在可以看到，当我随时间增加扩散系数时，这个过程会注入越来越多的随机性。从左到右移动时，噪声会越来越多，过程会变得越来越随机，这基本上描述了指数级收敛到某个状态的过程。

I'm just going to state that now to a normal distribution with that's specified with variance specified by these two parameters. You'll actually implement this in the lab, so you'll see these plots appearing.

我现在要说明的是，这会收敛到一个正态分布，其方差由这两个参数指定。你们将在实验中实际实现这个过程，会看到这些图表出现。

Good, cool. All right, so now we're finally ready to define what a diffusion model is.

很好。那么现在我们终于可以定义什么是扩散模型了。

What I'm going to do I'm going to overwrite the definition of a flow model because the changes are pretty minimal. So I'm going to make the changes in green.

我准备覆盖流模型的定义，因为改动非常小。我会用绿色标出这些改动。

So what is a diffusion model? I told you that we want to do generative modeling, which means we want to convert an initial distribution that you should think about as a Gaussian into a data distribution.

那么什么是扩散模型？我之前说过我们要进行生成建模，这意味着我们需要将初始分布（可以理解为高斯分布）转换为数据分布。

A flow model leads us with an ODE, and now we're just going to do the same thing but with an SDE.

流模型使用常微分方程，而现在我们要做同样的事情，但使用随机微分方程。

So the goal is to use stochastic differential equation to convert a simple distribution something like a Gaussian into a complex distribution.

所以目标是使用随机微分方程将简单分布（比如高斯分布）转换为复杂分布。

In the same way as before, the key ingredient will be the neural network, and the neural network will be the vector field.

与之前相同，关键组成部分仍然是神经网络，而神经网络将作为向量场。

So the same way as before that has not changed, and then we add a diffusion coefficient. We add that here, and it's basically like sigma like T in the same way as I had before.

所以这部分与之前相比没有变化，然后我们添加扩散系数。我们在这里添加，基本上就像我之前使用的σ(t)一样。

The diffusion coefficient is just basically what I wrote here, and it's important to note that most of the time that's fixed.

扩散系数基本上就是我这里写的内容，需要注意的是大多数情况下这是固定的。

So we're not going to learn this, there's not going to be new network for this, we're just going to have a formula for this.

所以我们不会学习这个参数，不会为此创建新的网络，我们只需要一个公式来表示它。

So you don't have to really have to worry about the sigma T at this point, you can actually more like later choose it the way you want it, which is what we want to see.

所以现阶段你们不必担心σ(t)，实际上你们可以在之后按照需要选择它，这正是我们想要看到的。

So the diffusion coefficient is not going to be learned, we're still going to learn the vector field, it's very important to note.

因此扩散系数不会被学习，我们仍然会学习向量场，这一点非常重要。

So really the core ingredient is still going to be the vector field, the same way as before.

所以核心组成部分仍然和之前一样是向量场。

We're going to randomly initialize the model, but now we're going to simulate an SDE.

我们将随机初始化模型，但现在我们要模拟随机微分方程。

So what we're going to do instead of simulating this ODE, we're simulating the same way an SDE.

所以我们要做的是不再模拟这个常微分方程，而是以同样的方式模拟随机微分方程。

So what have I changed? I've changed the introduction. Instead of on top of having a neural network that represents a vector field, I also have a diffusion coefficient.

那么我改变了什么？我修改了介绍部分。除了表示向量场的神经网络外，我还添加了扩散系数。

In the same way as before, I'm initializing with my initial distribution p init, and then I'm simulating an SDE in this way which we know how to do.

与之前相同，我用初始分布p_init进行初始化，然后以我们知道的方式模拟随机微分方程。

And the goal is that we end up with the data distribution, which is the distribution of object that we want to generate.

目标是最终得到数据分布，即我们想要生成对象的分布。

Great question. So the question is like you're asking what's the effect of adding this term.

很好的问题。这个问题是关于添加这个项会产生什么影响。

First of all I should say that that's why I said the flow component is the most important to understand that many state of the art models are just flow models.

首先我要说，这就是为什么我说流组件是最重要的，许多最先进的模型实际上就是流模型。

It's the really crucial part is the flow part, and then you can add stochasticity by this.

最关键的部分是流部分，然后你可以通过这个添加随机性。

And there's a bit of it's both an empirical and a theoretical question.

这既是一个经验问题也是一个理论问题。

So most people just empirically choose this Sigma T in a way that serves best. There's a bit of a trade-off is that you explore more if you inject more noise.

所以大多数人只是凭经验选择这个σ(t)以达到最佳效果。这里存在一个权衡：如果注入更多噪声，你可以探索更多空间。

In some sense you move more through space, but also if you do it too much you get some numerical errors that you might incur and then not generate as nice objects.

在某种意义上你可以在空间中移动更多，但如果做得太过，可能会产生一些数值误差，从而无法生成那么好的对象。

Great question. So why are we doing Brownian motion right? Like it seems kind of arbitrary.

很好的问题。那么我们为什么要使用布朗运动呢？这看起来有点随意。

So there is several answers to this question. First of all, a Gaussian distribution is something nice.

这个问题有几个答案。首先，高斯分布是一种很好的分布。

Like we know from the central limit theorem for example that Gaussian distributions are somehow universal, and we want to have a process that has Gaussian increments.

比如我们从中心极限定理知道高斯分布在某种程度上是普适的，我们想要一个具有高斯增量的过程。

So Gaussian is something we want most of the time anyway, so Gaussian is nice.

所以大多数时候我们确实想要高斯分布，高斯分布很好。

But it's actually more than that. It's pretty universal any Markov process that is continuous in time and has continuous trajectories actually will be represented by an SDE in this form.

但实际上不仅如此。它相当普适，任何时间连续且具有连续轨迹的马尔可夫过程实际上都可以用这种形式的随机微分方程表示。

So like you can impose relatively weak assumption and reduce it to this stochastic differential equation.

所以你可以施加相对较弱的假设，将其简化为这种随机微分方程。

It's like whenever you have non-Gaussian increments that are not in this format, you're going to have things like jump around in space, they're not going to be continuous and stuff like that, so it's going to be significantly more ugly.

就像当你使用非高斯增量且不符合这种格式时，会出现空间中的跳跃等现象，它们不会是连续的，因此会变得明显更加复杂。

So Brownian motion in that sense, the way I think about Brownian motion like I think about a Gaussian distribution as something really universal for distributions, and a Brownian motion I think about it as this equivalent for stochastic processes.

所以在这个意义上，布朗运动就像我认为高斯分布对于分布来说是真正普适的一样，我认为布朗运动对于随机过程来说是等效的普适概念。

You could think about general Markov processes. That's correct. It does not reduce like this.

你可以考虑一般的马尔可夫过程。这是正确的。它不会这样简化。

I actually wrote a paper on generalizing this to Markov processes, but I'm here to teach you the state of the art and not the exotic research.

我实际上写过一篇关于将其推广到马尔可夫过程的论文，但我在这里是教你们最先进的技术，而不是奇异的研究。

But you can make this more general, but then as I said you will have jump processes, you will have something that's not continuous, it's going to be more ugly, more difficult.

但你可以使其更通用，但正如我所说，你将会遇到跳跃过程，会遇到不连续的东西，这会更加复杂，更加困难。

Yes, so other you're saying if you look at this as a Gaussian process. So like a Gaussian process, now I'm lacking space.

是的，所以你是说如果将其视为高斯过程。就像高斯过程，现在我没有足够空间了。

So usually this is a detour you don't need to understand this, but Gaussian process is usually specified for some sort of mean function and the kernel function K.

通常这是一个题外话，你不需要理解，但高斯过程通常由某种均值函数和核函数K来指定。

So like if you've taken a class like K is what's like what's a Gaussian kernel? This is a specific kernel.

就像如果你上过相关课程，K是什么？什么是高斯核？这是一个特定的核函数。

So there's other kernels that would make the function smoother called the RBF kernel for example.

所以还有其他核函数可以使函数更平滑，例如称为RBF核的核函数。

There is should just have these are just smooth trajectories, so this would reduce to the ODE case.

这些只是平滑的轨迹，所以这会简化为常微分方程的情况。

They would be random but they're smooth, that simplifies as you've not done anything new.

它们会是随机的但是平滑的，这简化了问题，因为你没有做任何新的事情。

And there's other stuff that you could do like white noise. There are other things you could do, but then it wouldn't even be continuous. That would be completely different. You could make other choices - yes, it's just that if they're smooth functions, you could basically... They're smooth functions, so then it says you haven't done anything new. That's true, but we've also done this with the initial condition of the ODE. I'm not saying it's not possible, I'm just saying why it wouldn't be qualitatively that different from what people have done.

还有其他你可以做的事情，比如白噪声。你还可以做其他尝试，但那样甚至无法保持连续性。那将会是完全不同的情况。你可以做出其他选择——没错，关键在于如果它们是平滑函数，你基本上可以...既然它们是平滑函数，那就说明你并没有做出任何创新。这确实如此，但我们也已经通过ODE的初始条件实现了这一点。我并不是说这不可能，我只是在解释为什么它在性质上不会与人们已经完成的工作有太大差异。

Cool, okay. So we're almost at the end. Let's think about how we sample from a diffusion model. This algorithm is essentially the same as before, with J.S. saying we're using the Euler-Maruyama method. We initialize the point randomly now, not deterministically anymore. At every time point, we do the same thing as before: we draw a Gaussian sample and take a small step in the direction of the vector field, plus adding some Gaussian noise scaled by the square root of h and the diffusion coefficient sigma_t.

很好，我们即将结束。现在让我们思考如何从扩散模型中采样。这个算法本质上与之前相同，正如J.S.所说，我们使用的是欧拉-丸山方法。我们现在随机初始化点，不再采用确定性方式。在每个时间点，我们执行与之前相同的操作：抽取一个高斯样本，沿着向量场方向前进一小步，同时加入由h的平方根和扩散系数σ_t缩放的高斯噪声。

You're going to implement this in lab one. Important to note: we're always only interested in the final time point X1 - that's what we want to produce.

你们将在实验一中实现这个。需要重点注意的是：我们始终只关注最终时间点X1——这才是我们想要生成的结果。

At this point we're at the end. Did you want to say something? You want to grab the mic? Yeah, so let's just finish by talking about some of the logistics of this class.

现在我们来到最后部分。你刚才想说什么吗？想要发言？好的，让我们最后谈谈这门课的一些具体安排。

What do you have to do to pass this class? You have to come to the lectures, or if you can't make it, you can watch the recordings which I think will get posted with about a day delay, and do the labs. We want you to pass the class - I think you know, you have to give us an excuse to not pass you. But really, it's not anything super complicated: come to lecture, pay attention, do the labs, and you should be fine.

要通过这门课需要做什么？你需要参加讲座，如果无法到场，可以观看录播（我认为会延迟一天左右发布），并完成实验。我们希望你们都能通过——要知道，你们得给我们个理由才能不让你们通过。但说实话，这并不复杂：听课、认真听讲、完成实验，这样就没问题。

Tony L at Piazza - we do not have a plan, there's currently no Piazza. You can email us - if we get like 100 emails, maybe we'll think about it. For now, there's no Piazza.

关于Piazza平台的Tony L问题——我们目前没有计划，现在还没有设置Piazza论坛。你们可以给我们发邮件——如果收到大约100封邮件，我们可能会考虑。但目前还没有Piazza。

This is described on the website in our Logistics section. There are 25 points in the class: lecture 10 points, labs five points each, 15 points total, total of 25 points. You need 18 points to pass, I think we have written down right now.

这在网站的后勤部分有详细说明。课程总共25分：讲座参与10分，实验每个5分共15分，总计25分。通过需要18分，这个标准我们已经明确记录。

In terms of resources for this class, the central backbone - your main point of reference - should be the Leon notes you can find on the website. These are self-contained in a way that the slides are meant to accompany the lecture, and in that sense, they might be harder to view standalone. But the notes are complete and elaborate on many of the points that Pierre went into depth about, and many of the questions that people ask you can find answers there.

关于本课程资源，核心参考资料应该是网站上可找到的Leon笔记。这些笔记自成体系，而幻灯片主要是配合讲座使用，因此单独阅读幻灯片可能较难理解。但笔记内容完整，对Pierre深入讲解的许多要点进行了详细阐述，你们能在其中找到很多人提出问题的答案。

Secondly, office hours - come to office hours to ask questions about lectures. I think they're primarily intended for help with the labs.

其次，答疑时间——欢迎来答疑时间询问讲座相关问题。我认为答疑时间主要目的是为实验提供帮助。

I think with that, I'll just say that we're releasing lab one today - it was actually released at like 11:00 p.m. last night. It's available on our website - you can go to diffusion.c.m., the website up there, you can open up the lab. We are basically providing a link to GitHub, so you can download it from GitHub, you can open it up in your favorite Jupyter notebook environment like Colab, fill out the assignment, export it to a PDF and upload it to Gradescope.

说到这里，我要通知大家我们今天发布实验一——其实昨晚11点左右已经发布了。它可以在我们的网站上找到——你们可以访问diffusion.c.m.这个网站，打开实验。我们基本上提供的是GitHub链接，所以你可以从GitHub下载，在你喜欢的Jupyter笔记本环境（比如Colab）中打开，完成作业，导出为PDF并上传到Gradescope。

This is due on Friday at 5:00 p.m. Try and start it as soon as possible. I think we have office hours tomorrow at 3:30... 3 p.m., okay, my bad, 3 p.m.

提交截止时间是周五下午5点。请尽快开始。我记得明天答疑时间是3:30...不对是下午3点，抱歉，是下午3点。