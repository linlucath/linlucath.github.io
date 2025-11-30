---
title: 'Lecture 11_ Training Neural Networks II_en'
publishDate: 2025-11-30
description: 'TODO'
tags:
  - TODO
language: 'English'
heroImage: { src: './default.jpg', color: '#D58388' }
---

All right, welcome back to Lecture 11. Today we're gonna continue our discussion about all the little nitty-gritty tips and tricks that you need to train neural networks. I apologize I couldn't come up with a better title for this lecture. At last lecture they just end up being kind of a bit of potpourri of a lot of little things that you need that I think you need to know about training neural networks, but sort of had a hard time putting them into a good theme or a good title other than that.

好的，欢迎回到第11讲。今天我们将继续讨论训练神经网络所需的各种细节技巧和窍门。很抱歉我没能为这次讲座想出一个更好的标题。上次讲座最终变成了各种训练神经网络所需小知识的大杂烩，我很难将它们归入一个恰当的主题或标题。

To kind of recap last lecture and also this lecture, like I said, it's been a bit of a potpourri of all a lot of different topics that you need to know about about how to train their own networks. So the last time we talked we focused on some of these sort of one-time setup choices about the architecture and whatnot but you need to make before you start training.

回顾一下上节课和本节课的内容，正如我所说，这就像是训练自有网络所需各种不同主题的大杂烩。上次我们讨论时，我们重点讨论了一些在开始训练之前需要做出的架构等方面的一次性设置选择。

So recall last time we talked about activation functions and we had a lot to say about the different activation functions, but at the end we just decided to stick with ReLU. We talked about data pre-processing and we finally explained the mystery behind those mean subtraction lines that have been appearing on your homework assignments so far. We talked about weight initialization and then saw how we can use this Xavier or Kaiming initialization rules to force our activations to be initialized to have good distributions over many layers of a deep network.

记得上次我们讨论了激活函数，我们对不同的激活函数有很多讨论，但最后我们决定坚持使用ReLU。我们讨论了数据预处理，最终解释了那些在你们作业中一直出现的均值减去行的奥秘。我们讨论了权重初始化，然后看到了如何使用Xavier或Kaiming初始化规则来强制我们的激活在深度网络的许多层中初始化时具有良好的分布。

And this was kind of a trade-off between too small where things would collapse to zero or too large where things would explode. And then these these last two points I think we went a little bit fast last time in lecture so if anyone have any had any lingering questions about any of these points this would be your in to ask those.

这是一种权衡，介于太小会导致一切坍缩为零和太大会导致一切爆炸之间。最后这两点我认为上次讲座我们讲得有点快，所以如果有人对其中任何一点还有疑问，现在就是提问的机会。

But then remember we also talked last time about data augmentation which was this technique by which we can artificially multiply the size of our training set by performing random transformations on our training data before we feed it directly into the network. And again we saw that data augmentation was a way that you can inject priors about your own knowledge about the structure of your data into the training procedure of your neural network and that you could imagine inventing different types of data augmentation for different types of tasks.

但请记住，我们上次还讨论了数据增强，这是一种技术，通过在将训练数据直接输入网络之前对其执行随机变换，我们可以人为地增加训练集的大小。我们再次看到，数据增强是一种将您对自己数据结构知识的先验注入神经网络训练过程的方法，并且您可以想象为不同类型的任务发明不同类型的数据增强。

Then we also saw this very general concept of regularization where so far in the in your homework assignments you've seen something like L2 regularization where we add an explicit term and or explicit additional term on to our loss function that for example penalizes the norm of the weight matrix. But last time we saw this much more general class of regularizers that are commonly used in neural networks whereby in the forward pass we somehow inject some kind of noise to mess up the processing of the neural network in some way and then in the back in during testing then we somehow marginalize out or average out that bit of noise.

然后我们还看到了正则化这个非常通用的概念，到目前为止在你们的作业中，你们已经见过像L2正则化这样的东西，我们在损失函数中添加一个显式项或额外的显式项，例如惩罚权重矩阵的范数。但上次我们看到了在神经网络中常用的更通用的一类正则化器，在前向传播过程中，我们以某种方式注入某种噪声来干扰神经网络的处理，然后在测试期间的后向过程中，我们以某种方式边缘化或平均掉那点噪声。

As examples of this sort of paradigm of regularization we talked about things like dropouts, fractional pooling, drop connect, stochastic depth, and these crazy ones like cut out and mix up that are actually used in practice quite a bit. So I realize we went a little bit fast especially around regularization toward the end so I just wanted to make sure there were no lingering questions about any of these topics before we move on to new stuff.

作为这类正则化范式的例子，我们讨论了诸如丢弃法、分数池化、随机连接、随机深度，以及像CutOut和MixUp这些在实践中相当常用的疯狂方法。我意识到我们讲得有点快，尤其是在最后关于正则化的部分，所以我想在继续新内容之前，确保大家对任何这些主题都没有遗留问题。

Ok very good. So then that was kind of the stuff that we talked about last time and today we're gonna talk move on to some other interesting topics about bits and pieces about how you train neural networks in practice. In particular, we're going to talk about things that you need to worry about during the process of training your model and getting your model to train that's sort of setting learning rate schedules and how to choose hyper parameters.

好的，很好。那么这些就是我们上次讨论的内容，今天我们将继续讨论其他一些关于在实践中如何训练神经网络的零散有趣话题。特别是，我们将讨论在训练模型和让模型成功训练的过程中你需要担心的事情，比如设置学习率调度以及如何选择超参数。

I know this has been a very frustrating procedure so for some of you and then some additional points that you might want to think about after you've successfully trained your model. Those are questions about maybe model ensemble incurring and how to scale up your model to train on maybe whole data center levels of compute.

我知道这对你们中的一些人来说是一个非常令人沮丧的过程，然后还有一些在您成功训练模型后可能想要考虑的额外要点。这些问题可能涉及模型集成，以及如何扩展您的模型以在可能达到整个数据中心级别的计算资源上进行训练。

So the first of these topics is learning rate schedules. So I think at this point you've we've seen many different optimization algorithms we've seen things like vanilla SGD, SGD with momentum, Adagrad, RMSprop, Adam and all of these have some kind of hyper parameter that's called a learning rate. And usually this learning rate hyper parameter is probably the most important hyper parameter that you need to set for most deep learning models.

所以第一个主题是学习率调度。我认为到目前为止，我们已经看到了许多不同的优化算法，比如普通SGD、带动量的SGD、Adagrad、RMSprop、Adam，所有这些都有某种称为学习率的超参数。通常，这个学习率超参数可能是您为大多数深度学习模型需要设置的最重要的超参数。

And at this point you've had the chance to use SGD for a variety of different types of models and hopefully you've started to get some some intuition about what happens when you set different values of a learning rate with different optimizers. So here on the left is a little bit of a cartoon picture of what you can sometimes expect might happen with optimization as you set different types of learning rates.

此时，您已经有机会在各种不同类型的模型上使用SGD，并且希望您已经开始对使用不同优化器设置不同学习率值时会发生什么有了一些直觉。左边这里有一幅简图，展示了设置不同类型的学习率时，优化过程中有时可能发生的情况。

So here for example in yellow if you set the learning rate too high then often things will just explode immediately and the loss will escape to infinity or you'll get NaNs and it will very quickly go very wrong very fast. In contrast if you set something like in blue a very low learning rate then you'll see that learning tends to proceed very very slowly and this is good because you make progress things don't explode to infinity but it might take a while to train for your loss actually you drop to very low values.

例如，这里黄色的线，如果你设置的学习率太高，那么通常情况会立即爆炸，损失会趋向无穷大，或者你会得到NaN，并且会非常快地变得非常糟糕。相反，如果你设置像蓝色那样非常低的学习率，那么你会看到学习往往进行得非常非常缓慢，这很好，因为你在取得进展，情况不会爆炸到无穷大，但可能需要一段时间来训练，实际上你的损失会下降到非常低的值。

And in contrast something like in green might be learning rate which is high but not so high that you explode to infinity and there you see that in contrast to the blue learning rate setting a higher learning rate might actually converge to a value faster but it might actually not converge to as low of a loss value. And what we kind of like is something like the red curve here which is some sort of ideal good learning rate that you see it makes a quick progress towards this areas of low loss while also not exploding to infinity and also actually training reasonably quickly.

相比之下，像绿色那样的学习率可能较高，但又没有高到让你爆炸到无穷大，在那里你可以看到，与蓝色学习率设置相比，更高的学习率实际上可能更快地收敛到一个值，但它实际上可能不会收敛到那么低的损失值。而我们比较喜欢的是像这里的红色曲线那样，这是某种理想的好学习率，你可以看到它快速向低损失区域进展，同时也不会爆炸到无穷大，并且实际上训练速度也相当快。

So obviously if we can we would prefer to choose this red learning rate but if that's not always possible we have a sort of a question here which is that if we can't find that one perfect learning rate that's going to work for us then what are we supposed to do how are we supposed to trade-off between these seemingly sub optimal choices and this is a bit of a trick question it turns out because we don't actually have to make one choice for the learning rate. In fact, it's very common to somehow choose all of them. The basic idea here is to start with a relatively high learning rate, which might look something like the green curve, allowing our optimization to make quick progress in the first iterations towards areas of low loss.

显然，如果可能，我们宁愿选择这个红色的学习率，但如果这并不总是可能，我们这里就有一个问题：如果我们找不到那个对我们有效的完美学习率，那么我们应该怎么做？我们该如何在这些看似次优的选择之间进行权衡？结果证明这是一个有点技巧性的问题，因为我们实际上不必为学习率做出单一选择。事实上，通常的做法是以某种方式选择所有学习率。基本思路是从相对较高的学习率开始，这可能类似于绿色曲线，使我们的优化过程在最初几次迭代中能够快速向低损失区域推进。

Then over time, when the green curve starts plateauing, we want to reduce the learning rate and continue training with lower learning rates, like the blue learning rate. This approach hopefully gives us the best of both worlds - making quick progress at the beginning by starting with a high learning rate, and converging to very low loss values at the end of training by lowering it over time.

随着时间的推移，当绿色曲线开始趋于平缓时，我们希望降低学习率并以较低的学习率（如蓝色学习率）继续训练。这种方法有望让我们两全其美——通过以高学习率开始实现快速进展，并通过随时间降低学习率在训练结束时收敛到非常低的损失值。

But this has been vague and not very specific so far. I just said we're going to start with a high learning rate and end with a low learning rate. What concretely is that going to look like? This process of changing the learning rate during training is called learning rate scheduling, and there are several different forms commonly used when training deep neural network models.

但这到目前为止还比较模糊且不够具体。我只是说我们将以高学习率开始并以低学习率结束。具体来说这会是什么样子？在训练过程中改变学习率的这个过程被称为学习率调度，在训练深度神经网络模型时通常使用几种不同的形式。

The most commonly used learning rate schedule is the step schedule. With this schedule, we begin training with some relatively high learning rate, like 10^-1 for residual networks, and at certain chosen points during optimization, we suddenly jump to a brand new lower learning rate.

最常用的学习率调度是阶梯式调度。使用这种调度，我们以相对较高的学习率开始训练，比如残差网络使用10^-1，在优化过程中的特定选定点，我们会突然跳转到一个全新的较低学习率。

For residual networks, the typical schedule is to start with a learning rate of 0.1, then after 30 epochs of training, suddenly drop the learning rate to 0.01 and continue training for another 30 epochs. Then again drop the learning rate after 60 epochs and after 90 epochs - basically after every 30 epochs of training, we drop the overall learning rate by a factor of ten.

对于残差网络，典型的调度是以0.1的学习率开始，然后在训练30个周期后，突然将学习率降至0.01并继续训练30个周期。然后在60个周期和90个周期后再次降低学习率——基本上每训练30个周期，我们就将整体学习率降低十倍。

When you look at a training curve using step learning rate decay, you see a characteristic curve of loss as a function of time. In the first phase of training during the first 30 epochs with relatively high learning rate, we make very quick progress from high initial loss values towards lower loss values.

当您查看使用阶梯式学习率衰减的训练曲线时，您会看到损失随时间变化的特征曲线。在使用相对较高学习率的前30个周期的第一阶段训练中，我们从较高的初始损失值向较低损失值取得了非常快速的进展。

But after about 30 epochs, this quick progress steadies off and we're no longer making fast progress. At this moment when we decay the learning rate by a factor of 10, we see another new exponential pattern begin - once we drop the learning rate again, it decays and then plateaus, and when we reduce the learning rate again at 60 epochs, it decays quickly and plateaus again.

但大约30个周期后，这种快速进展趋于稳定，我们不再取得快速进展。当我们以10倍系数衰减学习率时，我们看到另一个新的指数模式开始——一旦我们再次降低学习率，它会衰减然后趋于平缓，当我们在60个周期再次降低学习率时，它会快速衰减然后再次趋于平缓。

This is a very characteristic shape of learning curves you'll see when models are trained using step learning rate schedule. One problem with this schedule is that it introduces many new hyperparameters into our model training.

这是使用阶梯式学习率调度训练模型时您会看到的学习曲线的典型形状。这种调度的一个问题是它给我们的模型训练引入了许多新的超参数。

Not only do we need to choose regularization and initial learning rate like in previous models, we also need to choose at which iterations to decay the learning rate and what new learning rates to choose at those iterations. This gives us many choices, so properly tuning step decay schedules can take considerable trial and error.

我们不仅需要像以前模型那样选择正则化和初始学习率，还需要选择在哪些迭代中衰减学习率以及在这些迭代中选择什么新的学习率。这给了我们很多选择，因此正确调整阶梯衰减调度可能需要大量的试错。

In practice, people usually look at learning curves and let things train with high learning rate for quite a long time to get a sense of when the model tends to plateau. Sometimes papers mention using heuristics where they keep training until loss plateaus or validation accuracy plateaus, then decay the learning rate.

在实践中，人们通常会观察学习曲线，让模型以高学习率训练相当长的时间，以了解模型何时趋于稳定。有时论文会提到使用启发式方法，即持续训练直到损失趋于平缓或验证准确率趋于平缓，然后衰减学习率。

This usually means they're using some kind of heuristic step decay schedule. However, if you're starting a new problem without much time to experiment with different decay schedules, step decay can be tricky because it introduces so many new things to tune.

这通常意味着他们使用某种启发式阶梯衰减调度。然而，如果您开始一个新问题而没有太多时间尝试不同的衰减调度，阶梯衰减可能会比较棘手，因为它引入了太多需要调整的新参数。

To overcome some shortcomings of step decay, another learning rate schedule has become trendy in recent years - the cosine learning rate decay schedule. Rather than choosing particular iterations to decay the learning rate, we write down a formula ahead of time that tells us the learning rate at every epoch as a function of epoch number.

为了克服阶梯衰减的一些缺点，近年来另一种学习率调度变得流行——余弦学习率衰减调度。我们不是选择特定的迭代来衰减学习率，而是提前写出一个公式，告诉我们每个周期的学习率作为周期数的函数。

Then we only need to choose the functional form - the shape of the curve along which the learning rate decays. One very popular form is the half-wave cosine learning rate schedule, where from the plot we can see the learning rate starts at some high value and decays according to half a period of a cosine wave.

然后我们只需要选择函数形式——学习率衰减的曲线形状。一个非常流行的形式是半波余弦学习率调度，从图中我们可以看到学习率从某个高值开始，并按照半个余弦波周期衰减。

This means we start with some initial high value of learning rate, and towards the end of training our learning rate will decay all the way to zero as this wave of the cosine is all the way to zero.

这意味着我们以某个较高的初始学习率开始，在训练接近尾声时，我们的学习率将逐渐衰减至零，就像余弦波的振幅最终归零一样。

Now this cosine learning rate schedule is very appealing because it has many fewer hyperparameters than the step decay schedule.

这种余弦学习率调度方案非常吸引人，因为它比阶梯式衰减方案的超参数数量要少得多。

In particular the cosine learning rate decay schedule only has two hyperparameters that we need to choose: one is the initial learning rate on this alpha zero here in the equation, and the other is the number of epochs that we're going to use to train the model which is this capital T.

具体来说，余弦学习率衰减方案只需要我们选择两个超参数：一个是方程中的初始学习率α0，另一个是训练模型的周期数，即这个大写的T。

But what's particularly appealing about this cosine learning rate schedule is that it actually doesn't introduce any new hyperparameters when training the model because whenever we're training the model we always need to choose some initial learning rate and we always need to choose some number of iterations that we're going to train.

但余弦学习率调度方案特别吸引人的地方在于，它实际上不会在训练模型时引入任何新的超参数，因为无论何时训练模型，我们总是需要选择某个初始学习率，也总是需要选择训练迭代次数。

So when using cosine learning rate schedule it doesn't introduce any new hyperparameters, it just gives additional interpretation or additional meaning to some of these other hyperparameters that we already were having to choose anyway before.

因此使用余弦学习率调度方案不会引入新的超参数，它只是为我们原本就需要选择的那些超参数赋予了额外的解释或意义。

And so that tends to make the cosine schedules a lot more easier to tune compared to step decay schedules.

这使得余弦调度方案相比阶梯式衰减方案更容易调整。

The general rule of thumb with cosine schedules is just training longer tends to work better.

使用余弦调度方案的一般经验法则是：训练时间越长效果越好。

So in practice the only thing you really need to tune is that initial learning rate and then sort of come to grips with how long you're willing to wait for your model to train.

因此在实践中，你真正需要调整的只有初始学习率，然后就是决定愿意等待模型训练多长时间。

So that I think those are some reasons why this cosine learning rate schedule has become reasonably popular in the last couple of years.

我认为这些就是余弦学习率调度方案在过去几年中变得相当受欢迎的一些原因。

And here I put some citations on the slide of some reasonably high profile papers from the last year or two that have used this cosine learning rate schedule.

这里我在幻灯片上列出了一些引用，来自过去一两年中使用这种余弦学习率调度方案的重要论文。

But this cosine shape is just one of many shapes that you might imagine using for decaying learning rates over time.

但余弦形状只是你可以设想的随时间衰减学习率的多种形状之一。

So another decay schedule that people sometimes use is a simple linear decay: again we're going to start with some initial learning rate and then decay it to zero over the course of training, but rather than following this cosine decay this linear decay learning schedule instead will simply decay the learning rate linearly over time.

人们有时使用的另一种衰减方案是简单的线性衰减：同样从某个初始学习率开始，然后在训练过程中衰减到零，但与余弦衰减不同，线性衰减学习方案会随时间线性降低学习率。

And that seems to work well for many problems.

这种方案似乎对许多问题都效果良好。

I should point out that I think there has not been super good studies that really compare these different schedules head-to-head so I can't really tell you concretely when cosine is going to be better or linear is going to work better.

我应该指出，我认为目前还没有非常好的研究来直接比较这些不同的调度方案，所以我无法具体告诉你什么时候余弦方案更好，或者线性方案效果更佳。

I think what most people do in practice is they build upon some prior work and then they sort of adopt whatever type of schedule happen to be used in the prior work that they're building upon.

我认为大多数人在实践中会基于先前的工作，然后采用他们正在构建的工作中恰好使用的任何类型的调度方案。

So what this means is that you'll see different areas of deep learning tends to end up using different types of learning rate schedules.

这意味着你会看到深度学习的不同领域往往最终使用不同类型的学习率调度方案。

But it's not really clear to me that that's because they're intrinsically better for that area it's often I think just because they want to have a fair comparison with whatever a piece of work came beforehand.

但我不太确定这是因为它们本质上更适合该领域，我认为通常只是因为希望与之前的任何工作保持公平比较。

So with that kind of in mind if you kind of look at these citations and what type of problem you'll see that a lot of computer vision type projects are often using this cosine learning rate decay schedule whereas this linear learning rate schedule is often used for large-scale natural language processing instead that are also trained using deep neural networks.

考虑到这一点，如果你查看这些引用和问题类型，会发现许多计算机视觉类项目经常使用余弦学习率衰减方案，而线性学习率调度方案则常用于使用深度神经网络训练的大规模自然语言处理。

And again I think that's maybe not something fundamental about vision versus natural language I think it's more a function of what paper what how different researchers in different areas have proceeded upon the paths.

我再次认为这可能不是视觉与自然语言处理之间的根本差异，而更多是不同领域的研究人员沿着不同路径发展的结果。

So then another learning rate schedule you'll sometimes see is this inverse square root schedule that sort of decays the learning rate across a different functional form but again it has this interpretation of starting out high and then ending up low.

你有时会看到的另一种学习率调度方案是反平方根调度，它以不同的函数形式衰减学习率，但同样具有从高开始最终降低的特性。

Now this inverse square root schedule I'm only putting it in here because it was used by one very high-profile paper in 2017 but it's like I've actually seen it used compared to be less compared to linear decay schedules and the cosine learning rate and the cosine decay schedules.

我在这里提到反平方根调度方案，只是因为2017年有一篇非常重要的论文使用了它，但实际上我看到它的使用频率低于线性衰减方案和余弦学习率衰减方案。

And I think the potential pitfall with this inverse square root schedule is that the model actually spends very little time at that initial high learning rate.

我认为反平方根调度方案的潜在缺陷是模型在初始高学习率状态下花费的时间非常少。

So with this inverse square root schedule you can see that the learning rate very quickly drops off from its high initial value and then spends a lot of time at these lower later values.

使用反平方根调度方案时，你可以看到学习率从初始高值迅速下降，然后在后续较低值上花费大量时间。

And if we compare that with the linear or the cosine schedule then we see that in contrast with these other schedules that are a bit more popular models tend to spend more time at those initial higher learning rates.

如果将其与线性或余弦调度方案进行比较，我们会发现与这些更受欢迎的调度方案相比，模型倾向于在初始较高学习率上花费更多时间。

And then I think with all this talk about different learning rate schedules I think I need to point out another very probably the most common real learning rate schedule is just the constant schedule.

在讨论了这么多不同的学习率调度方案后，我认为需要指出另一种可能是最常见的实际学习率调度方案：恒定调度方案。

And this one surprisingly actually works quite well for a lot of problems.

令人惊讶的是，这种方案实际上对许多问题都相当有效。

So here you know we simply set some initial learning rate and then keep that same learning rate through the entire course of training.

这里我们只需设置某个初始学习率，然后在整个训练过程中保持相同的学习率。

And this is actually what I recommend people do in practice until they have some reason to do otherwise.

这实际上是我建议人们在实践中采用的方法，除非他们有理由采用其他方案。

Where I see people mess up a lot of times when they're starting new deep learning projects is to fiddle with learning rate schedules too early in the process.

我看到很多人在开始新的深度学习项目时犯的错误是过早地调整学习率调度方案。

And typically changing learning rate schedules should be something that you do rather far along into the process of developing your model and getting it to work.

通常，改变学习率调度方案应该是在模型开发和使其工作的过程中相当靠后的阶段才进行的操作。

You can usually get things to work reasonably well just using a constant learning rate schedule and then the difference between performance with a constant schedule versus performance with one of these other more complicated schedules is usually not the difference between your model working and not working.

通常使用恒定学习率调度方案就能获得相当不错的效果，恒定调度方案与其他更复杂调度方案之间的性能差异通常不会决定模型能否工作。

Usually moving from constant to some more complicated schedule well maybe make things work a couple percent better so it's important if you're really trying to push for the state of the art on some problem.

从恒定调度方案转向更复杂的调度方案可能只会使性能提升几个百分点，因此如果你真的试图在某个问题上达到最先进水平，这很重要。

But if your goal is just to get something to work as quickly as possible with this little mess as possible then I think constant learning rates are actually a pretty good choice.

但如果你的目标只是尽可能快速、简洁地让某个东西工作起来，那么我认为恒定学习率实际上是一个相当不错的选择。

Although I should also point out that there is a bit of complication between learning rates and the optimizer that you choose.

尽管我也应该指出，学习率与你选择的优化器之间存在一些复杂关系。

So when using stochastic gradient descent with momentum then I think using some kind of learning rate decay schedule is fairly important but if we're using one of these more complicated optimizers like RMSprop or Adam, then you can go farther using only a constant learning rate. So that's kind of my caveat is that especially if you're using something like Adam, then you can actually get pretty far using just a constant learning rate.

因此当使用带动量的随机梯度下降时，我认为使用某种学习率衰减方案相当重要，但如果我们使用RMSprop或Adam这类更复杂的优化器，那么仅使用恒定学习率就能取得更好的效果。这就是我要提醒的一点：特别是当你使用Adam这样的优化器时，实际上仅靠恒定学习率就能取得相当不错的进展。

So any questions about these learning rate schedules before we move on to some other topic? Yeah, so the question is sometimes you'll train for a long time, loss will be going down, you'll be very happy because you think you trained a good model, and then all of a sudden loss will go up, things will explode and you'll become sad.

在我们转向其他话题之前，关于这些学习率调度策略还有什么问题吗？好的，问题是：有时候长时间训练后损失函数会持续下降，你会很高兴以为训练出了一个好模型，但突然之间损失值会上升，一切都会失控，然后你就会很沮丧。

I think it's really hard to say anything general about that case. I think there's a lot of reasons why something like that can go wrong. One case where I've run into a similar problem is if you forget the torch.zero_grad that I warned you guys multiple times about a couple lectures ago. Maybe that was not applicable to your case, but then if you're accidentally accumulating gradients over many iterations, then things will tend to work for a while but then after some point then gradients will explode and things will go wrong.

我认为这种情况很难给出通用解释。出现这种问题的原因有很多。我之前遇到过的一个类似问题是忘记调用torch.zero_grad——几节课前我多次提醒过大家这一点。可能这不适用于你的情况，但如果你意外地在多次迭代中累积梯度，那么模型可能会正常工作一段时间，但在某个时间点后梯度会爆炸，一切都会出问题。

I think things can also blow up depending on the problem type that you're working on. You might also see bad training dynamics so for something like different types of generative models or especially different types of reinforcement learning problems, then you'll often see very troubling behavior with these learning curves.

我认为问题的爆发也可能取决于你正在处理的问题类型。你可能会遇到糟糕的训练动态，比如对于不同类型的生成模型，特别是不同类型的强化学习问题，你经常会看到这些学习曲线出现非常棘手的行为。

Although if we're kind of a standard well-behaved classification problem, if the loss is kind of blowing up after a long periods of training, usually that indicates that makes me suspect some kind of a bug, either bad hyperparameters or some bug in the way data is being loaded, or maybe you're training on some kind of corrupt data.

虽然如果我们处理的是标准、表现良好的分类问题，在长时间训练后损失函数突然爆炸，这通常让我怀疑存在某种错误，可能是超参数设置不当，或是数据加载方式存在错误，或者你可能正在使用某种损坏的数据进行训练。

Actually that I've seen that as a problem sometimes where maybe one sample in your training set is actually corrupted. One common failure mode is like you know Flickr actually takes users can upload images to Flickr or different photo sharing sites and then later decide to remove those images, and then many datasets are constructed not by distributing actual JPEG files but by distributing links to Flickr images.

实际上我确实见过这样的问题：有时候训练集中的某个样本实际上已经损坏。一个常见的故障模式是：你知道Flickr允许用户上传图片到Flickr或其他照片分享网站，但后来用户决定删除这些图片，而许多数据集并不是通过分发实际的JPEG文件构建的，而是通过分发Flickr图片链接构建的。

So then if you go and naively try to download all the links in the dataset, you'll sometimes try to download some images that have been removed by users and then those will end up with some kind of default corrupted JPEG file, and then during the training process maybe it's a zero with some non-trivial label and then you'll explode when you try to train with a mini-batch that includes one of these corrupted image files.

因此，如果你直接尝试下载数据集中的所有链接，有时会尝试下载一些已被用户删除的图片，这些图片最终会变成某种默认的损坏JPEG文件。然后在训练过程中，可能这个样本带有非平凡标签但数据全为零，当你尝试使用包含这些损坏图像文件的小批量数据进行训练时，就会发生爆炸。

So sometimes data corruption in your training set can cause things to explode all of a sudden, but I think there's really no general answer, you need to dig in more to the specifics of your problem.

所以有时候训练集中的数据损坏会导致突然爆炸，但我认为确实没有通用答案，你需要更深入地研究你问题的具体情况。

Ok, but then another strategy. Yeah, so the question is about adaptive learning rates, so that would be something like Adagrad or RMSprop or Adam are examples of adaptive learning rate mechanisms, and they're... I think things can still blow up, things can still go wrong, sometimes they tend to make things a bit more robust but they definitely still don't solve all your optimization problems.

好的，那么另一种策略。是的，问题是关于自适应学习率的，比如Adagrad、RMSprop或Adam都是自适应学习率机制的示例。我认为即使使用这些方法，仍然可能发生爆炸，仍然可能出错，虽然它们往往能使训练更加稳健，但绝对无法解决所有的优化问题。

The question is like oh maybe I want to write some heuristic that will look at the loss curve and then determine for itself when the learning rate will drop. I've seen people do this but I recommend against it because I think you can get yourself into trouble too easily doing that.

问题是：也许我想编写一些启发式方法，通过观察损失曲线来自行决定何时降低学习率。我见过有人这样做，但我建议不要这样做，因为我认为这样做很容易陷入麻烦。

I think it's very easy to try to code up some clever solution that will try to smartly choose when to drop the learning rate, but there's a lot of corner cases that are difficult to account for. For example, if you look at these curves, they're actually very noisy.

我认为很容易尝试编写一些聪明的解决方案来智能选择降低学习率的时机，但有很多难以处理的边界情况。例如，如果你观察这些曲线，它们实际上非常嘈杂。

So here I'm actually plotting a scatter plot with each dot being the loss at a particular iteration, but it's so noisy that to get any signal you need to take some kind of a moving average over the training iterations.

这里我实际上绘制了一个散点图，每个点代表特定迭代的损失值，但噪声太大，为了获得任何有效信号，你需要对训练迭代进行某种移动平均。

So then they just end up being a lot of now sort of meta hyperparameters that go into these heuristics about when to decay the learning rates, and I think that you're just again you're just setting yourself up for trouble there and you're better off just looking at the loss curves and trying to make some expert determination there.

这样最终会产生大量元超参数用于这些关于何时衰减学习率的启发式方法，我认为你只是在自找麻烦，更好的做法是直接观察损失曲线并尝试做出专家判断。

Yeah, that's correct. So the dark is actually a bunch of circles that represent the loss of each iteration, but they're extremely noisy. So these circles end up actually having a pretty huge variance between iterations on the individual training losses.

是的，正确。这些黑点实际上是代表每次迭代损失值的一堆圆圈，但它们极其嘈杂。这些圆圈在单个训练损失的迭代之间实际上存在相当大的方差。

So whenever I plot these things, I always like to plot both those losses at every iteration to get some general sense of the variance, but then I also like to plot a moving average. This is usually a moving average of a window size of like a hundred or a thousand iterations that tells the moving average of the losses over some window, and that gives you a sense of both the overall variance of the training as well as the longer-term trends.

因此，每当我绘制这些图表时，我总是喜欢同时绘制每次迭代的损失值以获得方差的总体感觉，但我也喜欢绘制移动平均线。这通常是窗口大小为100或1000次迭代的移动平均，显示某个窗口内损失值的移动平均，这让你既能了解训练的整体方差，也能把握长期趋势。

So I think that's very useful, a very useful thing when plotting losses to help you debug. And absolutely point out that this is kind of a very fairly characteristic image that you get when training with a cosine schedule that it has a very funny shape, and if you're used to seeing plots like this with the step decay then the first time you train with a cosine schedule you get very surprised.

所以我认为这在绘制损失图表时非常有用，能帮助你进行调试。绝对要指出的是，使用余弦调度训练时得到的这种图像具有相当典型的特征——形状非常奇特，如果你习惯了步进衰减的图表，那么第一次使用余弦调度训练时会感到非常惊讶。

So this is something I've started to do recently and it's always concerning to me when I see these very weird-looking plots. But I think another thing that you should always be doing that really helps you to choose how long you should train is this notion of early stopping.

这是我最近开始采用的方法，当我看到这些看起来非常奇怪的图表时总是会担心。但我认为另一个你应该始终采用的方法，能真正帮助你决定应该训练多长时间的概念，就是早停法。

And I think this is a good mechanism to also go back to this exploding process during training. So here the idea is that whenever you train neural networks, you want to look at two to the really three curves: one is the training loss as a function of iteration which is here on the left, and if things are healthy you should see this kind of decaying exponentially in some way.

我认为这也是回顾训练过程中爆炸现象的一个好机制。这里的理念是：每当你训练神经网络时，你实际上需要关注三条曲线：一条是作为迭代函数的训练损失（在左侧），如果情况健康，你应该看到它以某种方式呈指数衰减。

But what you should also always be looking at is the training accuracy of your network, maybe that you check every epoch or so, as well as the accuracy both on the training set as well as the validation set, and the looking at these curves can give you some other sense of the health of your network throughout the training process.

但你还应该始终关注网络的训练准确率，可能每个周期检查一次，以及训练集和验证集上的准确率，观察这些曲线可以让你对网络在整个训练过程中的健康状况有其他的了解。

So then what you'll typically do is you want to stop training. You want to pick the net, the checkpoint of the model during training where you had the highest validation accuracy. So what you'll typically do is you'll typically set some number of max iterations or max epochs that you're going to train for, and then just let that thing train for that batch number epochs. But every epoch or every five epochs or ten epochs, you should always check the training and validation set accuracies, and then save the model parameters at those points to disk.

因此，你通常会想要停止训练，选择在训练过程中验证准确率最高的模型检查点。具体来说，你会设置最大迭代次数或训练周期数，然后让模型完成指定周期的训练。但每个周期、每五个周期或每十个周期，你都应该检查训练集和验证集的准确率，并将该时间点的模型参数保存到磁盘。

And then after the model finishes training, you can plot these curves and then select the checkpoint. Just select the point in time at which your model performed the best on the validation set, and then that's the checkpoint you should actually use when running the model in practice.

在模型完成训练后，你可以绘制这些曲线并选择检查点。只需选择模型在验证集上表现最佳的时间点，这就是实际运行模型时应该使用的检查点。

So if you do a process like this, then if the model happened to blow up late in the training process, then it's maybe not such a big deal. You can just look at this curve and then pull one of the model checkpoints from the point in training before the model blew up.

因此，如果采用这样的流程，即使模型在训练后期出现发散问题，影响也不会太大。你只需查看这些曲线，然后从模型发散前的训练时间点提取一个模型检查点即可。

So this is a really useful skill, a really useful heuristic on how to train your networks and how to select which checkpoint to use at the end of the day. This is something I really encourage people to use pretty much all the time whenever you're training deep networks.

这是一个非常实用的技巧，关于如何训练网络以及最终选择哪个检查点的有效启发式方法。我强烈建议大家在训练深度网络时始终采用这种方法。

So then that kind of leads into a bit of a larger discussion of how are we supposed to go about choosing hyperparameters for our neural networks. Well, one thing that you'll commonly see people do is this notion of a grid search.

这就引出了一个更广泛的讨论：我们该如何为神经网络选择超参数。通常人们会采用网格搜索的方法。

So here what we're going to do is select some set of hyperparameters that we care to tune, and then for each of those hyperparameters we'll select some set of values that we want to evaluate. For that hyperparameter, and often many of these hyperparameters you should be searching in kind of a log-linear space rather than a linear space.

具体做法是：选择一组需要调优的超参数，然后为每个超参数选定一组待评估的数值。对于大多数超参数，应该在log线性空间而非线性空间中进行搜索。

So then for example we might want to evaluate here for examples of the learning rate for different learning rates that are kind of spaced out in a log-linear way, and also test out for different values of regularization strengths that again are spaced sort of log linearly.

例如，我们可以评估按log线性间隔分布的不同学习率，同时测试同样按log线性间隔分布的正则化强度值。

And then given four values of the weight decay and four values of the learning rate, then that gives rise to 16 combinations. And if you have enough GPUs, just try them all and see which one works best, and that actually is a fairly reasonable strategy that people sometimes do in practice.

假设权重衰减有四个取值，学习率也有四个取值，就会产生16种组合。如果你有足够的GPU资源，可以全部尝试并找出最佳组合，这实际上是实践中相当合理的策略。

But the problem is that this strategy requires a number of GPUs which is exponential in the number of hyperparameters that you want to tune. So this very quickly gets very infeasible very quickly.

但问题在于，这种策略所需的GPU数量会随着待调优超参数的数量呈指数级增长，因此很快就会变得不可行。

So another strategy that sometimes people employ instead is random search rather than grid search. And here again we're going to select the procedure is much the same: we're going to select some set of hyperparameters that we want to tune.

因此人们有时会采用另一种策略：随机搜索而非网格搜索。这里的流程大致相同：选择一组需要调优的超参数。

Now rather than selecting values that we want to try for each of those hyperparameters, instead we're going to select some ranges of those hyperparameters along which we want to search.

不同于为每个超参数选定具体数值，我们会为这些超参数设定搜索范围。

And now we're going to, during each time we train our model, we're going to select a random value for each of those type of parameters that fall within that range.

在每次训练模型时，我们会为每个超参数在其范围内随机选择一个数值。

And again for like a learning rate and a weight decay, you'll often want to search in a log-linear space, whereas for other types of hyperparameters like maybe with the network or model size or dropout probability, sometimes you'll see a linear rather than log-linear spacing.

同样地，对于学习率和权重衰减这类超参数，通常需要在log线性空间搜索，而对于网络结构、模型大小或dropout概率等其他超参数，有时会采用线性而非log线性间隔。

And it kind of depends on what the hyperparameter is as to whether it should be linear or log-linear. But now the idea is that with this random search idea, we set these ranges for different hyperparameters, and then during each training run we draw a random value for our hyperparameter and then just let it go.

具体采用线性还是log线性取决于超参数的类型。随机搜索的核心思想是：为不同超参数设定范围，在每次训练运行时随机抽取超参数值进行训练。

And then however many trials of your network you can afford to train, you train that many, and then whatever happens to work best at the end, that gives you some that that's the hyperparameters that you use.

根据你能够承受的训练次数进行多次试验，最终选择表现最佳的那组超参数作为最终配置。

And there's been some, maybe if you think about grid search versus random search, you know they're formatted very similarly on the slide so you should think that so maybe you might think that they're very similar in how they perform.

关于网格搜索与随机搜索的比较，虽然它们在幻灯片上的呈现形式很相似，可能会让人以为它们的性能表现也相近。

But there's actually a fairly strong argument for using random search instead of grid search that comes from this 2010 paper. And the idea here is that if you have a lot of hyperparameters and you don't really know which hyperparameters are important.

但实际上，2010年的一篇论文提出了支持使用随机搜索而非网格搜索的有力论据。核心观点是：当存在大量超参数且你无法确定哪些超参数重要时，

If you have a lot of hyperparameters you need to tune, probably some of those hyperparameters are going to be very important for model performance, whereas other of those hyperparameters maybe it didn't really matter what value you were going to set - they were all anything in that range would have been fine.

在需要调优的众多超参数中，可能只有部分对模型性能至关重要，而其他超参数的具体取值影响不大——在合理范围内任意取值都可以。

But ahead of time before you train models, you might not know which hyperparameter is in which category. But usually it's the case that some hyperparameters matter and some hyperparameters don't matter.

但在训练模型之前，你往往无法预知哪些超参数属于哪一类。通常情况是部分超参数重要，部分不重要。

But now the idea is that if you are using a grid search, then we were always going to evaluate exactly the same grid of parameters on the left. So what this means is that in this sort of cartoon picture, the parameter on the horizontal axis ends up being very important for optimizing for getting good performance.

关键在于：如果使用网格搜索，我们总是会评估完全相同的参数网格。这意味着在这个示意图中，水平轴对应的参数对于获得良好性能至关重要。

Because you can see that this sort of distribution that we draw on the top of the grid is sort of the marginal distribution of model performance as a function of that hyperparameter value. So you can see that in this toy example, this horizontal hyperparameter is very important for getting very good performance because if we go far to the left there's low performance and there's kind of a small sweet spot in the middle that gives us very high model performance.

因为网格上方的分布展示了模型性能随该超参数变化的边际分布。在这个示例中，水平超参数对获得优异性能非常关键：向左移动会导致性能下降，而中间的一个小区域能带来极高的模型性能。

In contrast, in this sort of cartoon picture the vertical hyperparameter is maybe not so important for model performance. And you can see we've also drawn this sort of orange marginal distribution on the left-hand side of the plot that shows that no matter which value of this vertical hyperparameter we choose, things are going to perform about the same.

相比之下，示意图中的垂直超参数对模型性能可能不太重要。图中左侧的橙色边际分布显示，无论选择哪个垂直超参数值，性能表现都大致相同。

And now the problem is that if we do a grid search, we are not gaining as much information as we could from each trial of the model that we train. This is because we're going to try the same values of the important type of parameter many times repeated for each value of the unimportant parameter.

现在的问题是，如果我们进行网格搜索，我们无法从训练的每个模型试验中获得尽可能多的信息。这是因为对于不重要参数的每个取值，我们都要重复多次尝试重要参数的相同数值。

What that means is that for this important parameter distribution, we're only going to get three samples in this cartoon example. So that means that maybe we don't have enough information to properly tune the right value of the right setting of that important parameter.

这意味着对于这个重要参数的分布，在这个示意图例子中我们只能获得三个样本。因此我们可能没有足够的信息来正确调整这个重要参数的合适设置值。

In contrast, if you're going to use random search on the right, every trial that we run is going to have random hyperparameters for both the vertical and the horizontal hyperparameters. What this means is that when we plot these marginal distributions of model performance as a function of each hyperparameter, we end up getting more samples for each hyperparameter individually because the points on the grid don't align perfectly vertically or horizontally.

相比之下，如果使用右侧所示的随机搜索，我们运行的每个试验都会为垂直和水平超参数都分配随机值。这意味着当我们绘制模型性能作为每个超参数函数的边际分布时，我们最终能为每个超参数单独获得更多样本，因为网格上的点不会完美地垂直或水平对齐。

So what that means is that in a situation like this where one hyperparameter is important and the other parameter is unimportant, we end up using the multiple repeated samples of the unimportant hyperparameter in order to give us more effective samples of the important type of parameter.

这意味着在这样一个场景中，当一个超参数重要而另一个参数不重要时，我们最终利用不重要超参数的多次重复样本来为我们提供重要类型参数的更多有效样本。

Then if you look at these four though for the right-hand plot giving this random grid search, we see we end up with many samples of this important type of parameter that allows us to sample more points on this curve and hopefully find a better value overall.

那么如果你观察右侧图表中这四点所展示的随机网格搜索，我们会发现最终获得了这种重要类型参数的许多样本，这使我们能够在这条曲线上采样更多点，从而有望找到整体更好的值。

So if you're in a setting where you need to search randomly or private over hyperparameters, then usually using some kind of a random search is much more important than using some kind of a grid search.

因此，如果你需要在超参数上进行随机搜索或私有搜索的场景中，通常使用某种随机搜索比使用某种网格搜索要重要得多。

So here this slide is showing you a cartoon picture of some kind of idealized situation of what might happen with a random search. But here's an example of an actual random search that I did at a project at Facebook where we could use a lot of GPUs.

这张幻灯片展示的是随机搜索可能发生的某种理想化情况的示意图。但这里有一个我在Facebook项目中进行的实际随机搜索示例，当时我们可以使用大量GPU。

Here these plots we were evaluating the learning rate and the regularization strength for three different categories of models: a feed-forward model, a residual model, and a different sort of model architecture called DARTS. The details of what that is is not important for this purpose.

在这些图表中，我们评估了三种不同类型模型的学习率和正则化强度：前馈模型、残差模型，以及一种称为DARTS的不同模型架构。具体细节对此目的并不重要。

What you can see is that each point on these plots is a different model that we trained and the plot is quite dense as we trained a lot of models. The color of the point gives you the overall performance of the model at the end of training.

你可以看到这些图表上的每个点代表我们训练的不同模型，由于我们训练了大量模型，图表相当密集。点的颜色表示训练结束时模型的整体性能。

By looking at plots like this, you can get some sense of the interactions between different learning rates that you might come across.

通过观察这样的图表，你可以对不同学习率之间可能存在的相互作用有所了解。

What you can see here is that the x-axis is learning rate and the y-axis is the regularization strength along a log scale. What we can see is that there is some kind of non-trivial interaction between these two parameters but there's this kind of sweet spot or sweet river in the middle of good learning rates for each regularization strength and vice versa.

这里可以看到x轴是学习率，y轴是对数尺度上的正则化强度。我们可以看到这两个参数之间存在某种非平凡的相互作用，但对于每个正则化强度都存在一个最佳学习率的甜点区或甜带，反之亦然。

Yes, the question is should can you use gradient descent to learn the hyperparameters? Yes you can and I think that's a really cool area of research that I really enjoy and I think it's really creative and really interesting.

是的，问题是能否使用梯度下降来学习超参数？是的，可以。我认为这是一个非常酷的研究领域，我真的很喜欢，觉得它非常有创造性和趣味性。

There's many different approaches to that that I think are slightly beyond the scope of this lecture. But to give you a flavor of what that looks like, I think that's actually a beautiful situation where the software systems that we end up building to help us solve our problems end up giving rise to new mathematical solutions as well.

这方面有很多不同的方法，我认为略微超出了本讲座的范围。但为了让你们了解这是什么样子，我认为这实际上是一个美妙的情况：我们最终构建来帮助解决问题的软件系统也催生了新的数学解决方案。

What I mean by that is when you have something like PyTorch, it's very easy to back propagate through arbitrary Python code and your optimization loop is again just another bit of Python code.

我的意思是，当你使用像PyTorch这样的工具时，通过任意Python代码进行反向传播非常容易，而你的优化循环也只是另一段Python代码。

So in principle it's very easy in PyTorch to write code which will allow you to back propagate through an inner loop and an outer loop. In the inner loop you're going to run optimization over your model parameters but you'll actually back propagate through that entire inner loop in order to compute gradients of the final model performance on the initial values of the hyperparameters.

因此原则上，在PyTorch中编写代码非常容易，可以让你通过内循环和外循环进行反向传播。在内循环中，你将运行模型参数的优化，但实际上你会通过整个内循环进行反向传播，以计算最终模型性能对超参数初始值的梯度。

Then in this outer loop you'll use gradient to learn the hyperparameters. There's a bunch of really cool papers along this direction that I would love to find some way to sneak into one of these lectures.

然后在这个外循环中，你将使用梯度来学习超参数。这个方向有很多非常酷的论文，我很想找机会在某个讲座中介绍。

There's one paper I love where they learn not only the learning rates but they also use similar idea to learn the training data. Because now if you can back propagate through the learning process, we can actually learn the optimal training set that will cause the trained model to work well on the validation set.

有一篇我喜欢的论文，他们不仅学习学习率，还使用类似的思想来学习训练数据。因为现在如果你能通过学习过程进行反向传播，我们实际上可以学习最优训练集，使得训练出的模型在验证集上表现良好。

Oh so this is like very crazy and very fun to read papers in this direction. So yes you can, but it's actually not commonly used in practice. These things are super computationally expensive and people only at this point in time employ them for relatively small toy problems to show off that they can.

哦，这非常疯狂，阅读这个方向的论文非常有趣。所以是的，你可以这样做，但实际上在实践中并不常用。这些方法计算成本极高，目前人们只在相对较小的玩具问题上使用它们来展示能力。

For very large scale problems, these kind of automatic methods of learning hyperparameters via gradient descent are really not very practical to scale up to very large scale problems.

对于非常大规模的问题，这种通过梯度下降自动学习超参数的方法实际上很难扩展到非常大规模的问题。

The color scale is error rate, but it looks like the color bar on the right doesn't quite match up to actual colors in the plot so I apologize for that. But clearly the point where we put the red dot or the values we ended up choosing for the paper got to be the highest one.

颜色标度是错误率，但看起来右侧的颜色条与图表中的实际颜色不太匹配，我为此道歉。但很明显，我们放置红点的位置或最终为论文选择的数值达到了最高点。

You can see this dark purple means it's working well and then moving towards yellow is things not working well. I think there's probably some transparency issue between the actual color bar in the plot maybe I need to talk to my co-author about that.

你可以看到深紫色表示效果良好，而向黄色移动则表示效果不佳。我认为图表中实际颜色条可能存在一些透明度问题，也许我需要和我的合著者讨论这个问题。

So this is a good strategy to choose hyperparameters if you happen to be working at Facebook or Google or another tech company and have access to a lot of GPU resources. But if that's not the case then you need to be a little bit smarter in the way that you choose hyperparameters.

如果你恰好在Facebook、Google或其他科技公司工作，并且能够访问大量GPU资源，这是一个选择超参数的好策略。但如果不是这种情况，那么你需要在选择超参数的方式上更聪明一些。

But I think you should not despair. It's usually possible in my experience to choose pretty good hyperparameters for your problem without this massive hyperparameter search. So this is the procedure that I usually go about choosing hyper parameters when I don't have access to a very large GPU cluster. Step one is that you implement your model - actually you need to write some code first. Once you're done with that, then you should always be checking your initial loss. As we talked about multiple times so far, usually by the structure of the loss function that you're using, you can compute analytically what sort of initial loss you expect at random initialization. For something like cross entropy loss, it should be like minus log of the number of classes. So your first step after implementing your model is you turn off weight decay and just check this loss at initialization - that only takes one iteration, it's very cheap to do very fast. If that loss is wrong, you know you have a bug and you should go back and fix the bug.

但我认为你不应该绝望。根据我的经验，通常可以在不进行这种大规模超参数搜索的情况下为你的问题选择相当好的超参数。这是我在没有大型GPU集群时通常采用的超参数选择流程。第一步是实现你的模型——实际上你需要先编写一些代码。完成之后，你应该始终检查初始损失值。正如我们多次讨论过的，通常根据你使用的损失函数结构，你可以通过分析计算得出在随机初始化时期望的初始损失值。对于交叉熵损失这样的函数，它应该类似于类别数的负对数。因此，在实现模型后的第一步是关闭权重衰减，仅检查初始化时的损失值——这只需要一次迭代，成本极低且速度很快。如果损失值不正确，说明存在错误，你应该返回修复这个错误。

The next step is to try to overfit a very small sample of your training data. The idea here is that you want to take something like between one and maybe five to ten mini batches of data - a very tiny sample of your training set - and try to overfit this to a hundred percent. When you're doing this, you always want to turn off the regularization, and your goal is just to overfit the training data. When you're doing that, what you need to do is fiddle with the precise architecture of your model - maybe the number of layers and the size of each layer, play with the learning rate, play with the method of weight initialization.

第二步是尝试过拟合训练数据的一个极小样本。这里的思路是选取大约一到五个或十个迷你批次的数据——即训练集中非常小的样本——并尝试将其过拟合至百分之百准确率。在进行此操作时，你应当始终关闭正则化，目标仅仅是过拟合训练数据。在这个过程中，你需要调整模型的精确架构——可能是层数和每层的大小，尝试不同的学习率，试验不同的权重初始化方法。

When you play around with these different hyper parameters, you should be able to usually get whatever model you're working on to achieve 100 percent accuracy on this very tiny sample of the training set within a very small amount of time. Your goal here is that you should be able to overfit in something like five minutes of training. Because you're working on this very small sample training set and the training times are very short, it allows you to interactively play around with different values of these settings to find settings that cause you to overfit very quickly. The point of this is to just make sure that you don't have any bugs in your optimization loop.

当你调整这些不同的超参数时，通常应该能够让你正在处理的任何模型在极短时间内对这个极小的训练集样本达到100%的准确率。你的目标是能够在约五分钟的训练时间内实现过拟合。由于你处理的是非常小的训练样本且训练时间极短，这使你可以交互式地尝试不同的设置值，以找到能够快速过拟合的配置。这样做的目的是确保你的优化循环中没有任何错误。

If you can't overfit 10 training examples or 10 batches of data, then you have no hope in actually fitting the training set for real. It's surprising how often you can catch bugs in your optimization setup or in your model architecture choices just in this stage. These training loops run very fast, so you can do this interactively on a single GPU in most cases. In step two, we don't care about generalization to the validation set at all - we're just trying to debug the optimization process on a small training set.

如果你无法过拟合10个训练样本或10批数据，那么你实际上就没有希望真正拟合整个训练集。令人惊讶的是，仅在这个阶段你就能经常发现优化设置或模型架构选择中的错误。这些训练循环运行非常快，因此在大多数情况下你可以在单个GPU上交互式地进行操作。在第二步中，我们完全不在乎对验证集的泛化能力——我们只是尝试在小训练集上调试优化过程。

Once we've succeeded at step two and are able to overfit a very small amount of data, then we want to take the architecture from the previous step and use all of the training data. Now your goal is to find a learning rate that will cause the loss to start to go down quickly on your whole training set. Hopefully from step two, you found that your code is correct, your optimization loop is correct, and you found a model architecture that you believe is sufficient for modeling your data.

一旦我们在第二步取得成功并能够过拟合极小量数据后，我们就要采用前一步的架构并使用全部训练数据。现在你的目标是找到一个能使损失在整个训练集上快速下降的学习率。希望从第二步中，你已经确认代码正确、优化循环正确，并找到了一个你认为足以建模数据的模型架构。

In step three, you're going to take all of those architectural parameters and copy them over, and you're just going to fiddle with the learning rate on the entire training set. The learning rate is the only parameter you'll change. In changing the learning rate, your goal is to make the loss drop significantly within the first hundred iterations of training or so. For most problems that are set up properly, you'll usually see some very high initial loss at the beginning and tend to see some kind of exponential decrease in loss within the first hundred to thousand iterations of training - that's empirically true across a wide variety of problems and neural network architectures.

在第三步中，你将采用所有这些架构参数并复制它们，然后仅在整个训练集上调整学习率。学习率是你唯一要改变的参数。在调整学习率时，你的目标是让损失在训练的前一百次迭代左右显著下降。对于大多数设置得当的问题，你通常会在开始时看到较高的初始损失，并倾向于在训练的前一百到一千次迭代中看到某种指数级的损失下降——这在各种问题和神经网络架构中都是经验性的真理。

At this stage, because you're only caring to train for something like a hundred or a thousand iterations, you can typically do this interactively. Just choose learning rates, look at the learning curve, and based on what the plots look like, go back and choose new learning rates and work interactively until you can find a setting of the learning rate that causes things to actually start to converge within the first 100 or so iterations.

在这个阶段，由于你只关心大约一百或一千次迭代的训练，你通常可以交互式地进行操作。只需选择学习率，观察学习曲线，然后根据图表的表现返回选择新的学习率，交互式地工作，直到找到能在前100次左右迭代中使模型真正开始收敛的学习率设置。

Now at this point we're in relatively good shape. We've got an architecture that we know has the potential to model our data because it can overfit a couple training samples, and we know our optimization is in a pretty good state because we know the loss is starting to go down at the beginning of training. So now step four is to set up a very coarse hyper parameter grid - maybe like a very small number of models. Maybe you choose two different values of learning rates and two different values of regularization strength, or just choose a very tiny hyper parameter grid to evaluate that is somewhere in the neighborhood of all the choices you have made up to this point.

现在我们已经处于相对良好的状态。我们拥有一个我们知道有潜力建模数据的架构，因为它能够过拟合几个训练样本，而且我们知道优化状态相当不错，因为我们知道损失在训练开始时就开始下降。因此第四步是建立一个非常粗糙的超参数网格——可能只包含极少数的模型。也许你选择两个不同的学习率值和两个不同的正则化强度值，或者只是选择一个非常小的超参数网格来评估，这个网格应该位于你到目前为止所有选择的邻近范围内。

Hopefully by following these previous steps, all of these hyper parameter choices within this very small grid will end up being somewhat reasonable. You will not have any catastrophically bad models within this very tiny hyper parameter grid. After you have this coarse grid, then you train on the full training set for something like one to five or one to ten epochs, and that should be enough to give you some sense of the generalization performance of your model beyond the training set and actually see it start to look at how it performs on the validation set.

希望通过遵循前面的步骤，这个极小网格内的所有超参数选择最终都会相对合理。在这个极小的超参数网格内，你不会出现任何灾难性的糟糕模型。在有了这个粗糙网格后，你在完整训练集上训练大约一到五个或十个周期，这应该足以让你了解模型在训练集之外的泛化性能，并实际开始观察它在验证集上的表现。

This is something that you probably cannot do interactively, but at this point you've got enough familiarity with your model that you know all of these choices should hopefully work. You set up this tiny hyper parameter grid depending on how many models you can afford to train in parallel, you can step back and return after a coffee break, a night of sleep, or a week's vacation depending on how long your model takes to train. Then you can see how well these models performed.

这可能无法交互式完成，但此时你已经对模型足够熟悉，知道所有这些选择应该都能正常工作。你根据能够并行训练的模型数量来设置这个微小的超参数网格，然后可以在咖啡休息时间、睡一晚或休假一周后重新评估，具体取决于模型训练所需的时间。最后查看这些模型的表现情况。

After going to step four, you enter this iterative loop of examining the results from your previous small hyperparameter grid. Then you adjust your hyperparameter grid and go back to train for longer periods.

完成第四步后，您将进入这个迭代循环：检查先前小型超参数网格的结果，调整超参数网格，然后返回进行更长时间的训练。

At this point, you're in an interactive process where each iteration might take hours to days depending on your model's training time. Throughout this process, you're always monitoring the learning curves and using them to determine how to adjust your hyperparameter grid moving forward.

此时您处于交互式流程中，每次迭代可能需要数小时到数天，具体取决于模型训练时间。在整个过程中，您需要持续观察学习曲线，并据此决定如何调整后续的超参数网格。

When I say look at learning curves, I mean we need to examine these plots I mentioned earlier. You should always plot the training loss on the left - I prefer to show both raw accuracies as scatter plots and moving average losses as line plots. On the right, I always plot training and validation accuracies and check them regularly.

当我说查看学习曲线时，指的是需要检查我之前提到的这些图表。您应该总是在左侧绘制训练损失 - 我喜欢同时显示原始准确率的散点图和损失移动平均值的线图。在右侧，我总是绘制训练和验证准确率并定期检查。

By looking at these learning curves, you can usually gain insight into what might be going right or wrong with your model. Here I'll show you some schematic diagrams of different learning curve shapes you should become familiar with.

通过观察这些学习曲线，您通常能够了解模型可能存在的问题或优势。这里我将向您展示几种应该熟悉的不同学习曲线形状示意图。

One situation is when your learning curve looks very flat at the beginning and then makes a sharp initial drop. If you see this shape, it probably means your initialization was bad because no progress was made at the start of training, so you should adjust your initialization and try again.

一种情况是学习曲线开始时非常平坦，然后突然急剧下降。如果看到这种形状，很可能意味着初始化效果不佳，因为训练初期没有进展，因此您应该调整初始化设置并重新尝试。

Another problem is when we see a loss that makes good progress but then plateaus after a while. When you see a loss curve like this, you should consider some kind of learning rate decay because your learning rate might be too high, and around the plateau point might be where you should introduce learning rate decay.

另一个问题是损失函数初期进展良好但随后出现平台期。当看到这样的损失曲线时，您应该考虑采用某种学习率衰减策略，因为您的学习率可能过高，平台期出现的时间点可能正是应该引入学习率衰减的时机。

Conversely, if you introduce learning rate decay too early in the model development process, you might see a learning curve where the model was making good progress, then at the decay point the loss made a small drop and completely plateaued afterward. This usually means you decayed too early.

相反，如果在模型开发过程中过早引入学习率衰减，您可能会看到这样的学习曲线：模型原本进展良好，但在衰减点损失仅小幅下降后完全停滞。这通常意味着衰减时机过早。

These were all shapes of the moving average of training losses, but you can also gain intuition by looking at plots of training and validation accuracy over time. One characteristic shape is when they make exponential increase initially and then slowly increase linearly over time.

以上都是训练损失移动平均值的形状，但通过观察训练和验证准确率随时间变化的图表也能获得启发。一个典型特征是初期呈指数增长，随后随时间线性缓慢上升。

If you see a shape where there's a non-trivial but healthy gap between train and validation, and both are continuing to increase, this means things are going well and you just need to train longer because the curves are still rising.

如果您看到训练和验证之间存在显著但合理的差距，且两者都在持续上升，这表明情况良好，您只需要延长训练时间，因为曲线仍在上升。

A plot like this means something very bad is going on - this is a characteristic plot of overfitting. Here performance on the training set continues to increase but validation performance has either plateaued or decreased. This large and increasing gap indicates overfitting.

这样的图表意味着出现了严重问题 - 这是过拟合的典型表现。训练集性能持续提升但验证集性能却停滞不前甚至下降。这种不断扩大的差距表明存在过拟合。

When you see a learning curve like this, you need to either increase regularization strength, collect more training data, or in rare cases decrease model size or capacity. This is the characteristic shape of overfitting.

当看到这样的学习曲线时，您需要增强正则化强度、收集更多训练数据，或在少数情况下减小模型规模或容量。这是过拟合的典型特征。

In contrast, if you see a plot where training and validation performance are almost identical, you might think this is good because there's no overfitting. But usually this is a bad sign - it typically means you're underfitting the data.

相反，如果看到训练和验证性能几乎完全相同的图表，您可能认为这是好事因为没有过拟合。但这通常是个不良信号 - 往往意味着您对数据欠拟合。

In fact, you would have been better off increasing model capacity or decreasing regularization, which would tend to give better overall performance on the validation set even if that results in a larger gap. This is counterintuitive but actually represents an unhealthy learning curve indicating underfitting.

实际上，增加模型容量或减少正则化会更好，即使这会扩大差距，但往往能在验证集上获得更好的整体性能。这虽然违反直觉，但确实是表示欠拟合的不健康学习曲线。

Any questions about these learning curves? The question is whether you can also tell by looking at absolute accuracies if they're particularly low. Yes, that's also definitely a good indicator to know if you're underfitting the data.

关于这些学习曲线还有问题吗？问题是通过观察绝对准确率是否特别低也能判断情况。是的，这确实也是判断是否欠拟合的良好指标。

But this requires prior knowledge about what constitutes reasonable accuracy on the dataset, which you might not have when approaching a new problem. This is empirical - what we want is a model that performs best on unseen data. It happens I mean I think there's not a strong theoretical reason for this but the empirical fact is that usually when you find a model that achieves the best accuracy on the unseen data then there typically is some kind of a non-trivial gap between performance on the training and performance on the validation set and that's kind of an empirical fact I think there's not really great theory that I can point to you for that problem for that uh for that observation.

但这需要事先了解数据集的合理准确率标准，而处理新问题时可能缺乏这种认知。这属于经验性判断 - 我们最终需要的是在未见数据上表现最佳的模型。我认为这并没有很强的理论依据，但经验事实是：当你找到一个在未见数据上达到最佳准确率的模型时，训练集表现和验证集表现之间通常存在显著差距。这算是一个经验事实，对于这个问题或观察，我确实无法向你指出什么完善的理论。

So then this brings us to our final close our final step in this type of parameter training policy you look at these lost curves and then based on your intuitions about how things are going that should gives you some sense about how to adjust your grids based on looking at these lost pots that we just looked at then you go to step 5 and loop until you run into your paper submission deadline and you have no more time to train models.

这就引出了我们参数训练策略的最后一步：观察这些损失曲线，根据你对训练进展的直觉判断，这应该能让你对如何调整参数有个大致概念。基于我们刚才看到的这些损失图，然后进入第5步并循环执行，直到你遇到论文提交截止日期，没有更多时间训练模型为止。

So basically what I like to think about when you're tuning these things is that you're some kind of a DJ tuning all these little knobs about your learning rate strength and your hyper parameters strength and your dropout and your model architecture and then hopefully if you tune all these knobs in just the right way you'll end up making beautiful music in the form of a model that works really well and unseen data.

基本上，我喜欢把参数调优想象成DJ打碟：你就像在调节各种旋钮，包括学习率强度、超参数强度、dropout和模型架构等。如果所有这些旋钮都调节得当，你最终就能创作出美妙的音乐——也就是一个在未见数据上表现优异的模型。

And in order to do that it's often very helpful to set up some kind of a cross validation command center where you need where you can look at very like you need to train large numbers of models in parallel and then look at these learning curves in parallel and then use this as a way to get some idea about what sets of hyper parameters tend to be working well or tend to not be working well.

为此，建立一个交叉验证指挥中心通常很有帮助：你需要并行训练大量模型，然后同时观察这些学习曲线，借此了解哪些超参数组合效果较好，哪些效果不佳。

Now back in the day before things like tensorflow this was a pain in the butt and you had we had to like write custom web code in order to visualize these learning curves and like learn JavaScript plotting frameworks to plot these things or set up your own custom jupiter notebooks for plotting these things and you could end up spending a lot of time just on the infrastructure of looking at the results of your experiments but now with things like tensor board you a lot of that work has been done for you so it's usually a lot more seamless nowadays to set up these kind of cross validation command centers as they as you will.

在TensorFlow等工具出现之前，这非常麻烦：我们必须编写自定义网页代码来可视化这些学习曲线，学习JavaScript绘图框架来绘制图表，或者设置自定义的Jupyter笔记本。你可能要花费大量时间仅仅在构建实验结果查看的基础设施上。但现在有了TensorBoard等工具，大部分工作都已为你完成，因此如今建立这类交叉验证指挥中心通常要顺畅得多。

Another there's another cut there's some other heuristics that you can sometimes look at that can help you diagnose things that are going wrong and training so for example one thing you sometimes like to do is look at the ratio between the values of the weights of your network and the and the magnitudes of the updates that you're making onto those same ways so that would be you know your gradients times their learning rates gives you the overall delta that you're going to use update each value of the way that each iteration and generally speaking you want to AP's the value between the absolute value of the weight and the absolute value of the update for each of the scalar weights in your network to be not too large typically if you're making updates that are of the same order of magnitude or larger in order in larger in magnitude then the weight value itself that's usually some kind of a sign that something bad is happening.

还有一些其他启发式方法可以帮助诊断训练中出现的问题。例如，有时可以查看网络权重值与对应更新量级之间的比率——即梯度乘以学习率得到的总体增量，用于每次迭代更新每个权重值。通常来说，你希望网络中每个标量权重的绝对值与更新绝对值之间的比率不要过大。如果你做的更新量级与权重值本身相当或更大，这通常表明出现了问题。

So looking at these ratios between weight update magnitudes and weight magnitudes is sometimes some heuristic that people look at and practice or maybe looking at other kinds of statistics of the gradient and magnitudes or magnitude is something that can sometimes help you debug problems that are going wrong during training so that gives us to looking at these training dynamics and hopefully if you follow this simple step a seven step procedure that I've outlined you'll be able to train really good models even if you don't have access to a giant GPU cluster.

因此，观察权重更新量级与权重量级之间的比率是人们常用的一种启发式方法，或者查看梯度的其他统计量和量级有时也能帮助调试训练中出现的问题。这让我们得以审视训练动态。希望如果你遵循我概述的这个简单七步流程，即使没有大型GPU集群，也能训练出非常优秀的模型。

But then the question happens is that after your after you've successfully trained some models then what can you do after that now well now things get interesting so one thing that you often want to do is to get a little bit better on your train on your on your final test set and it turns out there's a very hip simple heuristic that apply is almost across the board for getting a slightly better performance on basically whatever problem you're considering and that is that you train something like n independent models however many you can afford to train and then rather than using one of them instead you just use all of them at test time.

但问题来了：成功训练出一些模型后，接下来能做什么？这时事情就变得有趣了。通常你会想在最终测试集上获得更好表现。事实证明，有一个非常时髦简单的启发式方法几乎适用于所有问题：训练n个独立模型（数量取决于你的承受能力），然后在测试时不是使用其中一个，而是使用全部模型。

So that means for each sample in your test set you run it through each of your trained models to get the predictions from each of your trained models and then you average the predictions across all of your trained models for something the exact mechanism of averaging kind of depends on the exact problem you're trying to solve but for something like image classification you could for example take an average of the probability distributions that are output from each of the models because the average of probably distribution is still a probability distribution and typically when you take an ensemble of a bunch of different models you end up getting about one or two percent better on your final test case on your final test set.

这意味着对于测试集中的每个样本，你都要通过所有训练好的模型运行它，获得每个模型的预测结果，然后对所有模型的预测结果进行平均。具体的平均机制取决于你要解决的具体问题。例如对于图像分类，你可以对每个模型输出的概率分布取平均，因为概率分布的平均仍然是概率分布。通常当你集成多个不同模型时，最终测试集上的表现会提高约1-2%。

So that is pretty standard no matter what the model architecture is or how many models you're honest not well more is usually better but what tasks you're working on what data set you're working what's your underlying CNN architecture typically you get about one to two percent better when you ensemble some some set up models together so if you're really trying to squeeze out that last bit of juice then this is a very common trick that you'll see people use.

这是相当标准的做法，无论模型架构如何，或者你训练了多少个模型（通常越多越好），无论你处理什么任务、使用什么数据集、底层CNN架构如何，当你集成多个模型时，通常能获得约1-2%的提升。所以如果你真的想榨取最后一点性能，这是人们常用的技巧。

One kind of cute idea is that rather than training multiple independent models sometimes you can get away with saving multiple check points of one model during training and then actually average the results of those different check points during training and that can also give you some some improved performance.

一个巧妙的想法是：与其训练多个独立模型，有时你可以保存单个模型在训练过程中的多个检查点，然后对这些不同检查点的结果进行平均，这也能带来一定的性能提升。

And then one trick there is actually to Train with a very very bizarre learning rate schedule that is actually periodic so this is like not super mainstream but it's so crazy I wanted to point it out the idea is that your learning rate schedule is now actually periodic but it starts high goes low goes high again goes low again goes high again goes low again and then the the check points that you take during training to form your kind of ensemble are the values of the model weights that were at the the very low point of each of those points in the learning rate schedule that's kind of a cute idea that you might see people use sometimes. Another idea is to keep a running average of the model weights that you see during training, and this is called Polyak averaging. It's used actually pretty commonly in some large-scale generative models.

还有一个技巧是使用非常奇特、实际上是周期性的学习率调度。这不算主流方法，但非常疯狂，我想特别指出：你的学习率调度现在是周期性的，从高到低，再到高，再到低，如此循环。你在训练过程中为形成集成而采集的检查点，是学习率调度中每个周期最低点时的模型权重值。这是个挺有趣的想法，有时你会看到人们使用它。另一个想法是保持训练过程中所见模型权重的运行平均值，这被称为Polyak平均法。实际上这种方法在一些大规模生成模型中相当常用。

So here, remember like in batch normalization, we're always keeping this running exponential average of the means and the variances of our features, and then during testing we use those exponentially running means and standard deviations for batch normalization during test time. Well, it turns out you can actually do the same thing with the model weights.

这里要记得，就像在批归一化中，我们始终保持着特征均值和方差的运行指数平均值，然后在测试期间使用这些指数运行均值和标准差进行批归一化。实际上，你可以对模型权重做同样的事情。

So then rather than using the model weights that result from any one iteration of gradient descent, instead you can take an exponentially running average of the model weights that you see during training, and then actually use this exponential running average of the model weights at test time instead. This can have the effect of helping you to smooth out some of this iteration-to-iteration variation in the model that happens during training.

因此，与其使用梯度下降任何一次迭代产生的模型权重，不如采用训练过程中所见模型权重的指数运行平均值，然后在测试时实际使用这个模型权重的指数运行平均值。这有助于平滑训练过程中模型在迭代之间产生的某些变化。

If you go back and look at these loss plots, remember there was a lot of variation in the loss between individual iterations of the model. By applying this kind of averaging to the model weights themselves, it can help to average out some of that noise that happens between individual iterations of SGD.

如果你回头看看这些损失曲线，记得模型在单个迭代之间的损失存在很大变化。通过对模型权重本身应用这种平均方法，有助于平均掉SGD在单个迭代之间发生的某些噪声。

So those are ways that you can squeeze out just a little bit of extra juice on whatever is the original task you were trying to solve. But sometimes we actually want to use one trained model to help us solve a totally different task, and that is an extremely powerful tool that has become super mainstream in computer vision over the past several years.

这些方法可以让你在试图解决的原始任务上再榨取一点额外的性能。但有时我们实际上想要使用一个训练好的模型来帮助解决完全不同的任务，这是一个极其强大的工具，在过去几年中已成为计算机视觉领域的主流方法。

That's basically the problem of transfer learning. So here the idea is that there's kind of a myth that goes around when training CNNs: the myth is that you'll often see people say that you need very, very large training sets if you want to successfully use deep learning for your problem. But I think this is actually false, and I'd like to bust this myth.

这基本上就是迁移学习的问题。这里有个关于训练CNN的迷思：你经常会听到人们说，如果想成功将深度学习用于你的问题，就需要非常大的训练集。但我认为这实际上是错误的，我想打破这个迷思。

The idea is that I think if you utilize transfer learning, you can actually get away with using deep learning for a lot of problems even in cases where you do not have access to a very large training set. For this reason, transfer learning has become a critical part of pretty much all mainstream computer vision.

我认为如果你利用迁移学习，即使在没有大型训练集的情况下，实际上也可以将深度学习用于许多问题。因此，迁移学习已成为几乎所有主流计算机视觉方法的关键组成部分。

The basic idea is that step one is train a convolutional neural network model on ImageNet or some other very large-scale image classification dataset, and make that part work as well as you possibly can using all the tricks that we've outlined. Then step two is to realize that we don't actually care about ImageNet classification performance; instead we might care about classification performance on some other smaller dataset or some other task entirely.

基本思路是：第一步在ImageNet或其他大型图像分类数据集上训练卷积神经网络模型，使用我们概述的所有技巧尽可能让这部分工作得最好。然后第二步是意识到我们实际上并不关心ImageNet的分类性能；相反，我们可能关心其他更小数据集或完全其他任务的分类性能。

Well here the idea is that we will take our trained network from image data and then remove the last fully connected layer. Recall that for example the last fully connected layer in something like an AlexNet takes us from these 4096-dimensional features into this 1000-dimensional vector of class scores.

这里的思路是：我们将从图像数据中获取训练好的网络，然后移除最后一个全连接层。回想一下，例如AlexNet中的最后一个全连接层将这些4096维特征转换为1000维的类别得分向量。

So in fact this last layer in the network ends up being tied to the category identities of the categories on which the model was trained. But now what we can do is simply throw away that last layer and delete it from the network, and just use those 4096-dimensional vectors at the second-to-last layer of the network as some kind of general feature representation of our images.

实际上，网络中的这最后一层最终与模型训练所用类别的身份标识绑定在一起。但现在我们可以简单地丢弃最后一层，将其从网络中删除，只使用网络倒数第二层的4096维向量作为图像的某种通用特征表示。

Then you can just freeze the whole weights of the network and just use those extracted features as a feature vector that represents your images. What people found out starting in about 2013 to 2014 was that this seemingly simple idea allows you to get very good performance on many, many computer vision problems.

然后你可以冻结整个网络的权重，只使用这些提取的特征作为代表你图像的特征向量。人们从2013到2014年左右发现，这个看似简单的想法可以让你在许多计算机视觉问题上获得非常好的性能。

For example, there was another dataset that was called Caltech 101 that was unsurprisingly 101 object categories, but overall a lot smaller in size than ImageNet. Here what we're showing is the red curve: the x-axis shows the number of training samples on Caltech 101 that we're using per category, and the y-axis shows the classification performance on this Caltech 101 dataset.

例如，还有另一个名为Caltech 101的数据集，不出所料包含101个对象类别，但总体规模比ImageNet小得多。这里我们展示的是红色曲线：x轴显示我们在Caltech 101上每类使用的训练样本数量，y轴显示在这个Caltech 101数据集上的分类性能。

Now the red curve was this prior pre-deep learning method that was the state of the art on Caltech 101, that was very particularly designed feature extraction pipeline for this particular dataset. Now the blue and the green curves show this very simple procedure of taking an AlexNet that was pre-trained on ImageNet and then using the second-to-last layer of AlexNet features as this predefined feature vector.

红色曲线是深度学习之前的方法，当时在Caltech 101上是最先进的，专门为这个特定数据集设计了特征提取流程。而蓝色和绿色曲线显示了这个非常简单的过程：取一个在ImageNet上预训练的AlexNet，然后使用AlexNet倒数第二层的特征作为预定义的特征向量。

Then simply training a logistic regression or SVM in this case - so they trained either a logistic regression or a support vector machine, which are simple linear models that work on top of this fixed 4096-dimensional feature vector that was extracted from our pre-trained model.

然后简单地训练逻辑回归或SVM - 他们训练了逻辑回归或支持向量机，这些是在从预训练模型提取的固定4096维特征向量之上工作的简单线性模型。

What they found is that using this very simple procedure of training a linear model on top of these pre-extracted feature vectors, they were able to significantly outperform the state of the art in this dataset. In particular, they were able to get non-trivial performance even using something like five to ten samples per class on this new dataset.

他们发现，使用这种在预提取特征向量之上训练线性模型的非常简单的方法，能够显著超越该数据集的最先进水平。特别是，即使在这个新数据集上每类只使用五到十个样本，他们也能获得不错的性能。

So this actually is very common, and if you use an ImageNet pre-trained model to extract features, then you can tend to get reasonably good performance on many downstream tasks even when you don't have a very large training set. This is definitely not particular to Caltech 101.

这实际上非常常见，如果你使用ImageNet预训练模型提取特征，那么即使没有非常大的训练集，也往往能在许多下游任务中获得相当好的性能。这绝对不仅限于Caltech 101。

We saw that this was also similar on this other bird classification dataset at that time. So here DPD and Pio and POF poof were existing methods that were very particularly tuned for this task of recognizing birds.

我们看到这在当时的另一个鸟类分类数据集上也有类似情况。这里DPD、Pio和POF poof是专门为识别鸟类任务调整的现有方法。

What they found is that by simply training a logistic regression on these pre-extracted features from an AlexNet, then they were able to outperform these previous methods. And if they were incorporating the AlexNet features into the previous method, they were able to get an even larger boost.

他们发现，只需在AlexNet的这些预提取特征上训练逻辑回归，就能超越这些先前的方法。如果将AlexNet特征整合到先前的方法中，他们能够获得更大的提升。

This was simply swapping out the AlexNet features for whatever else that previous method was doing in their learning process. And this applies across not just Caltech 101 and birds - this tends to apply across a very large set of image classification problems. So there was another paper that benchmarked this idea of extracting features and then training linear models on top of them for a whole suite of different image classification problems. Here you can see they got good performance on objects, scenes, birds, flowers, human attributes and object attributes. In all cases they were able to outperform the previous state of the art on those data sets.

这只是将AlexNet特征替换掉先前方法在学习过程中所做的任何其他操作。这不仅适用于Caltech 101和鸟类 - 这往往适用于非常广泛的图像分类问题集。另一篇论文对这种提取特征并在其上训练线性模型的方法进行了一系列不同图像分类问题的基准测试。可以看到，该方法在物体、场景、鸟类、花卉、人类属性和物体属性等任务上都取得了良好性能。在所有情况下，该方法都超越了这些数据集上先前的最优结果。

What's astounding here is that each of those blue bars, which represents the previous state of the art on one of those data sets, was typically a completely independent method that had been tuned independently for that one particular data set. Here they were able to use this very simple procedure that utilizes fine-tuning and outperform all of these different methods using one simple procedure of extracting features from a pre-trained model on ImageNet and then simply training linear models on top of those for the downstream tasks.

令人惊讶的是，每个蓝色条代表先前在某个数据集上的最优结果，通常都是完全独立的方法，专门为特定数据集单独调优。而他们能够使用这种非常简单的流程，通过微调技术，仅用单一方法——从ImageNet预训练模型中提取特征，然后为下游任务简单训练线性模型——就超越了所有这些不同的方法。

This applies not only to image classification but it turns out that this idea of utilizing features from a pre-trained network applies to a wide variety of image computer vision problems as well. In that same paper, I guess beating one two three four five six state-of-the-art methods wasn't enough for them - they also benchmarked a set of image retrieval tasks.

这不仅适用于图像分类，事实证明，利用预训练网络特征的思想也适用于各种计算机视觉问题。在同一篇论文中，我想击败六个最优方法对他们来说还不够——他们还对一系列图像检索任务进行了基准测试。

The details are not super important of how these works, but basically the setup is that you for example get an image of some building at Oxford and your task is to, based on the picture of that one building, retrieve other images from a database that are other images of the same building, maybe from a different viewpoint or different time of day or something like that. This is some kind of search by image - if you upload an image to Google and then search for similar images.

这些工作的具体细节不是特别重要，但基本设置是：例如你得到牛津某座建筑物的图像，你的任务是基于该建筑物的图片，从数据库中检索出同一建筑物的其他图像，可能是不同视角或不同时间拍摄的。这类似于图像搜索——如果你向Google上传图像然后搜索相似图像。

Again here, the idea is that we're going to extract these features from our models that are pre-trained on ImageNet, and then we will simply use some kind of simple nearest neighbor procedure to perform this image retrieval task. Again by using simple nearest neighbors on top of these pre-trained feature vectors from ImageNet, they were able to outperform a large number of previous methods on a large set of image retrieval datasets.

同样地，这里的思路是从ImageNet预训练的模型中提取这些特征，然后我们只需使用某种简单的最近邻程序来执行此图像检索任务。通过在ImageNet预训练特征向量上使用简单最近邻方法，他们再次在大量图像检索数据集上超越了众多先前的方法。

This is probably the simplest example of transfer learning tasks where we simply extract feature vectors and then use them out of the box for either a linear model or a retrieval baseline.

这可能是迁移学习任务中最简单的例子，我们只需提取特征向量，然后直接用于线性模型或检索基线。

For this paper in particular, they had another trick in their bag which was actually applying data augmentation to the raw images before extracting the feature vectors. They found that this was actually pretty important for beating these pipeline methods. Again this data augmentation is fairly simple and straightforward - these are mostly random scales, flips and crops that we kind of talked about in a previous lecture.

特别是对于这篇论文，他们还有另一个技巧，就是在提取特征向量之前对原始图像应用数据增强。他们发现这对于击败这些流水线方法实际上相当重要。同样，这种数据增强相当简单直接——主要是我们在之前讲座中讨论过的随机缩放、翻转和裁剪。

Then they take an ensemble over many different test time, taking ensemble over many different random data augmentations for the data point. But again it's sort of a fairly simple straightforward procedure that is very the same across all the data sets.

然后他们在多个不同的测试时间进行集成，对数据点进行多种不同随机数据增强的集成。但这仍然是一个相当简单直接的过程，在所有数据集上都非常相同。

That's kind of the simplest procedure where we simply use the pre-trained network and just use it to extract feature vectors out of the box and plug those feature vectors into some other algorithm. But if your dataset is a little bit larger, you can often do better than that using this procedure we call fine-tuning.

这是最简单的方法，我们只需使用预训练网络直接提取特征向量，并将这些特征向量插入其他算法。但如果你的数据集稍大一些，使用我们称为微调的流程通常可以获得更好的效果。

Here the idea is that we will take our model which has been pre-trained on something like ImageNet, and then maybe throw away the last layer and reinitialize the last layer to be maybe a new layer that pertains to the classification categories on our new data set. Now we will continue training the entire model for our new classification data set.

这里的思路是，我们将获取在ImageNet等数据集上预训练的模型，然后可能丢弃最后一层，并重新初始化最后一层为适合新数据集分类类别的新层。现在我们将继续为新的分类数据集训练整个模型。

Rather than just using it as a fixed feature extractor, we'll actually backpropagate into the model and continue updating the weights of the model to continue improving performance on this downstream task. Here there's a couple tricks and tips: one is that sometimes you often need to reduce the learning rates a lot when you're doing this fine-tuning.

而不仅仅是将其用作固定特征提取器，我们实际上会反向传播到模型中，并继续更新模型的权重，以持续提高下游任务的性能。这里有一些技巧和提示：一是进行微调时通常需要大幅降低学习率。

Another trick is that sometimes you might want to first start from extracting features and then getting a linear model to converge on top of features, and then after you do that go back and fine-tune the whole model. Those are maybe some tips and tricks on this fine-tuning that you might do in practice.

另一个技巧是，有时你可能希望首先从提取特征开始，然后在特征之上让线性模型收敛，完成后再返回微调整个模型。这些可能是你在实践中进行微调时可以使用的一些技巧。

It turns out that this procedure of fine-tuning can actually give pretty substantial gains in performance on a lot of tasks. For this task of object detection that we'll talk about in more detail in a few lectures, you don't need to know how this works or what this number on the vertical axis is - just higher is better and 100 is perfect.

事实证明，这种微调流程实际上可以在许多任务上带来相当显著的性能提升。对于我们将在后续讲座中详细讨论的目标检测任务，你不需要了解其工作原理或纵轴数字的含义——只需知道越高越好，100是完美值。

What this means is that the blue bar is using some kind of transfer learning for object detection on two different data sets. This blue bar was using fixed feature vectors where we just freeze the entire network and use it as a fixed feature extractor. The orange bars are where we actually continue training the entire neural network model on the new data set.

这意味着蓝色条表示在两种不同数据集上使用某种迁移学习进行目标检测。蓝色条使用的是固定特征向量方法，即冻结整个网络并将其用作固定特征提取器。橙色条表示我们实际上在新数据集上继续训练整个神经网络模型。

What we can see is that by fine-tuning, things are working a lot better than this - this gives us a huge boost over just using the network as a feature extractor. Another point here is that the architecture that you use matters. I told you that step one was to train a model on ImageNet - well it turns out that the model that you train matters a lot.

我们可以看到，通过微调，效果比这种方法好得多——相比仅将网络用作特征提取器，这给我们带来了巨大提升。另一点是，你使用的架构很重要。我告诉过你们第一步是在ImageNet上训练模型——事实证明，你训练的模型非常重要。

In general, models that work better on ImageNet tend to also work better for many other computer vision problems. This is why I think this is the reason why many people in computer vision and basically everyone in computer vision knows the exact relative order of all the models on ImageNet.

一般来说，在ImageNet上表现更好的模型往往在其他许多计算机视觉问题上也表现更好。这就是为什么我认为计算机视觉领域的许多人，基本上所有从业者，都知道所有模型在ImageNet上的确切相对排名。

The reason for that is not because they're obsessed with the ImageNet challenge - it's instead because models that work better on ImageNet for many years tended to also work better on basically every other problem that you tried. There was a period in time when basically you would just take the best, the latest and greatest model that worked the best on ImageNet and just apply it to whatever your problem at hand was, and things would get better.

原因并不是因为他们痴迷于ImageNet挑战赛——而是因为多年来在ImageNet上表现更好的模型，基本上在你尝试的任何其他问题上也往往表现更好。有一段时间，你只需取在ImageNet上表现最好的最新最优模型，直接应用于你手头的任何问题，效果就会变得更好。

An example from that comes from again this object detection problem. So it's very difficult to find papers that make controlled comparisons between different ImageNet models. This object detection comparison is not perfect, but it's the closest I could find.

这方面的例子再次来自这个目标检测问题。因此很难找到对不同ImageNet模型进行受控比较的论文。这个目标检测的对比并不完美，但已是我能找到的最接近的对比。

Here again the y-axis is the performance on this object detection task. Zero is as terrible, 100 is perfect. Starting around 2011, the best state-of-the-art pre-deep learning was getting something like 5 on this task.

这里的纵轴表示目标检测任务的性能表现。0分代表极差，100分代表完美。大约从2011年开始，深度学习之前的最先进技术在这个任务上只能得到5分左右。

Now when we use an object detection method with AlexNet we got 15, and then using the exact same object detection method but just swapping out AlexNet for VGG and doing everything else the same gave us a boost from 15 to 19.

当我们使用AlexNet的目标检测方法时得到了15分，然后使用完全相同的目标检测方法，只是将AlexNet替换为VGG，其他所有条件保持不变，我们的得分就从15分提升到了19分。

That's just the result of using a more powerful bigger network that works better on ImageNet, also gave us improvements on this new task.

这仅仅是使用了在ImageNet上表现更好的更强大、更大型网络的结果，同时也让我们在这个新任务上获得了改进。

Then if you compare this 29 and 36, again this is the exact same object detection method, but one is using VGG and the other is using a 50-layer ResNet.

然后如果你比较这个29分和36分，这同样是使用完全相同的目标检测方法，但一个使用VGG，另一个使用50层ResNet。

Again the jump from VGG to ResNet gave huge gains in performance, not just on ImageNet but also on a ton of downstream tasks.

从VGG到ResNet的转变再次带来了巨大的性能提升，不仅在ImageNet上，也在大量下游任务中。

This trend basically continued that models that work better on ImageNet tended to give you gains nearly for free with very little effort on a wide range of downstream tasks in computer vision.

这个趋势基本上持续存在：在ImageNet上表现更好的模型往往能让你几乎不费吹灰之力就在广泛的计算机视觉下游任务中获得提升。

So then the quick guide of how to approach transfer learning with CNNs is I'd like to think about this little two-by-two matrix where you can think about your problem falling into one of these buckets.

因此关于如何使用CNN进行迁移学习的快速指南是，我喜欢考虑这个2x2矩阵，你可以将你的问题归入其中某个类别。

One is whether your dataset is very similar to ImageNet, and the other is how much data you have in this new dataset.

一个维度是你的数据集是否与ImageNet非常相似，另一个维度是你在这个新数据集中拥有多少数据。

If your dataset is very similar to ImageNet, that is it tends to contain objects that look kind of like ImageNet objects, then even if you have very little data like maybe tens to hundreds per category, then applying some kind of linear classifier on top of your pre-trained features tends to work quite well.

如果你的数据集与ImageNet非常相似，即它倾向于包含看起来有点像ImageNet对象的物体，那么即使你只有很少的数据（比如每类几十到几百个样本），在你的预训练特征之上应用某种线性分类器通常效果很好。

If you have a fairly large amount of data, maybe hundreds to thousands of samples per category, then fine-tuning your ImageNet model on your new dataset tends to work quite well.

如果你有相当大量的数据，可能每类数百到数千个样本，那么在你的新数据集上微调你的ImageNet模型通常效果很好。

Now if your dataset is fairly different from ImageNet, and similar and different is not well defined in this context I admit, but if your dataset is fairly different from ImageNet but you have a lot of data, often you can still initialize from an ImageNet model and fine-tune and get good performance.

如果你的数据集与ImageNet相当不同（我承认相似和不同在这个语境中没有明确定义），但如果你有大量数据，通常你仍然可以从ImageNet模型初始化并进行微调，获得良好性能。

Now the danger zone is right here where you have a very small dataset and the nature of that data is somehow very different from the types of images you see in ImageNet. If you have a small number of samples per class, then something like pre-training and fine-tuning is very, very effective. If you have larger data sets, if your data set size is larger, then you can consider not only fine-tuning the whole model or perhaps also training a brand new model from scratch. But the caveat here is that even in cases where you have large data sets, I think pre-training and fine-tuning is still an extremely effective recipe in computer vision because it makes things trained a lot faster. So even in cases where you do have a fairly large data set at the end of the day for the task you care about, then initializing your model from a model pre-trained on ImageNet tends to make things train much, much faster, so it's very, very useful to do in practice.

危险区域就在这里：当你有一个非常小的数据集，而且这些数据的性质在某种程度上与你ImageNet中看到的图像类型不同。如果每个类别的样本数量较少，那么预训练和微调这类方法会非常有效。如果拥有更大的数据集，你不仅可以考虑对整个模型进行微调，或许还可以从头开始训练一个全新的模型。但需要注意的是，即使是在拥有大型数据集的情况下，我认为预训练和微调在计算机视觉领域仍然是极其有效的方案，因为它能大幅加速训练过程。因此，即使你最终针对所关注的任务拥有相当大的数据集，使用在ImageNet上预训练的模型来初始化你的模型，通常也能使训练速度大大加快，这在实践中非常有用。

I'd like to go, I think this one this part is less critical, so if there's any sort of questions about transfer learning, I'd rather take those here. Alright, so then I guess if you can bear with me for just like two minutes, we can blast through this stuff because you don't have enough GPUs to do this anyway.

我想继续，不过这部分内容相对不那么关键，所以如果对迁移学习有任何问题，我宁愿在这里解答。好的，那么如果大家能再忍受我两分钟，我们可以快速过一遍这些内容，反正你们也没有足够的GPU来做这个。

But basically, a couple lectures ago we talked about this notion of moving from single device to entire sort of rack scale or data center scale machine learning. So the question is how do you actually do that? So one idea is that we'll split our model across GPUs. Maybe one thing you could imagine is you partition put some of the layers on one GPU, some of the layers on other GPUs, and the players that I'm not on GPU. This turns out to be a really bad idea because your GPUs will spend a lot of time waiting. That only one in this kind of scheme only one GPU will be executing at a time, so it'll be a very inefficient use of your resources.

基本上，几堂课之前我们讨论过从单设备扩展到整个机架规模或数据中心规模的机器学习这个概念。那么问题是如何实际操作？一种想法是将模型拆分到多个GPU上。你可能设想的一种方式是，将部分层放在一个GPU上，其他层放在另外的GPU上，而剩下的部分不在GPU上。结果证明这是一个非常糟糕的想法，因为你的GPU会花费大量时间等待。在这种方案中，每次只有一个GPU在执行计算，因此资源利用效率极低。

Another idea that you'll see sometimes, by the way this is called model parallelism because the idea is that you're splitting up your model to run different parts of your model on different devices. Another thing that's another flavor of model parallelism that you'll see is to use different parallel branches, use the split up your model into multiple parallel branches, and then run those different parallel branches on different GPU devices. And this is the type of mechanism that was actually used by the original AlexNet paper because if you recall, the poor guy only had GPUs with only three gigabytes of memory, so this was really critical for training AlexNet at that time.

你会看到的另一种想法，顺便说一下，这被称为模型并行，因为其核心思想是将模型拆分，以便在不同的设备上运行模型的不同部分。你会看到的模型并行的另一种形式是使用不同的并行分支，将模型分割成多个并行分支，然后在不同的GPU设备上运行这些不同的并行分支。这正是原始AlexNet论文实际采用的机制，因为如果你还记得，那位可怜的研究者当时只有3GB显存的GPU，所以这对当时训练AlexNet至关重要。

But this ends up being a fairly inefficient way of parallelizing across GPUs as well because it requires synchronizing, it requires a lot of communication between GPUs, and in particular it requires communicating the activations and the gradients of the loss with respect to those activations within the forward and backward passes. So this tends to be fairly expensive to do.

但这最终成为一种在GPU间并行效率相当低的方式，因为它需要同步，需要在GPU之间进行大量通信，特别是需要在前向传播和反向传播过程中传递激活值以及损失相对于这些激活值的梯度。因此，这样做往往成本相当高。

So instead, the way that people, the typical way that people instead parallelize across multiple GPU devices is this idea called data parallelism. So here what we're going to do is take your batch of n images, and then if we're training on two GPU devices, we'll replicate the model on each of those two GPU devices and run a smaller mini-batch of n over two images on each of your independent GPUs.

因此，取而代之，人们通常采用的跨多个GPU设备并行的方法是所谓的数据并行。这里我们要做的是，取一批包含n张图像的批次，然后如果我们在两个GPU设备上训练，我们会在每个GPU设备上复制模型，并在每个独立的GPU上运行一个包含n/2张图像的较小迷你批次。

And now after you split if you split across the batch dimension, now your GPUs can perform most of their processing completely independently, and there's much, much less need for communication across the GPUs. So now these two GPUs can run forward pass completely independently, compute loss completely independently, compute gradient with respect to the parameters again without any communication between GPUs.

现在，如果你沿着批次维度进行分割，那么你的GPU可以完全独立地执行大部分处理，GPU之间的通信需求大大减少。因此，现在这两个GPU可以完全独立地运行前向传播，完全独立地计算损失，再次完全独立地计算相对于参数的梯度，而GPU之间无需任何通信。

And the only point where you need to communicate is at the end of each forward and backward pass where these GPUs now need to communicate the gradient of the loss with respect to the parameters and sum those gradients across all the GPU devices in order to make a gradient step. So this is the way that people typically take to make use of multiple GPU devices these days.

唯一需要通信的时刻是在每次前向和反向传播结束时，此时这些GPU需要交换损失相对于参数的梯度，并在所有GPU设备上对这些梯度求和，以便执行梯度下降步骤。这就是当今人们通常利用多个GPU设备的方式。

And this approach actually scales very well to very, very large numbers of GPUs. So you can imagine splitting this not just across two GPUs in your desktop but across eight GPUs on a big server or maybe across hundreds of GPUs in a data center. And again, the idea is the same that will take our batch of data, split it evenly across the GPUs, do independent forward and backward passes on each device, and the only point of communication is where we need to sum the gradients across the different elements or the different devices.

这种方法实际上可以很好地扩展到非常大量的GPU。所以你可以想象，不仅仅是拆分到你台式机上的两个GPU，还可以拆分到大型服务器上的八个GPU，或者数据中心里的数百个GPU。同样，其思想是相同的：我们将数据批次均匀分割到各个GPU上，在每个设备上执行独立的前向和反向传播，唯一的通信点是我们需要跨不同元素或不同设备对梯度进行求和。

Another variant you'll somewhat sometimes see is where you may be combined model parallelism within each server and then do data parallelism across different servers in a data center. You can imagine this requires having a fairly large number of GPUs to play with.

你有时会看到的另一种变体是，在每个服务器内部结合使用模型并行，然后在数据中心的不同服务器之间进行数据并行。可以想象，这需要拥有相当多数量的GPU可供使用。

But then basically this idea of training splitting, you're utilizing multiple devices by splitting across the batch dimension, works really well, but it requires you to be able to train with very large mini-batches.

但基本上，这种通过沿批次维度分割来利用多个设备的训练分割思想效果非常好，但它要求你能够使用非常大的迷你批次进行训练。

So then basically the goal here is suppose you've got a model that works really well on a single GPU, and now we want to scale up to train that same model on a very, very large number of GPUs. And the goal here is usually that we want to reduce the overall training time of that model. So rather than training for a very long time on one GPU, instead we'd like to train for a very short amount of time on that larger set of GPUs.

那么，这里的基本目标是：假设你有一个在单个GPU上运行良好的模型，现在我们想扩展到在非常大量的GPU上训练同一个模型。这里的目标通常是我们希望减少该模型的总训练时间。因此，与其在一个GPU上训练很长时间，我们更愿意在那一大组GPU上训练很短的时间。

So then hopefully the total number of epochs, the total number of times that we see each element in the training set, should be the same, but we will instead form larger mini-batches and make fewer overall gradient descent steps.

因此，希望总的训练周期数，即我们在训练集中看到每个样本的总次数，保持不变，但我们将使用更大的迷你批次，并执行更少的总体梯度下降步骤。

And basically, the one really important trick to do this ends up being fairly straightforward, and it turns out that a very simple heuristic that works really well for this large batch training is to scale the learning rate. So suppose that you've got a model that trains really well on one GPU using a learning rate alpha and a batch size of n, then you can usually train on K GPUs with a batch size of K times n per GPU and a learning rate of K times alpha.

基本上，实现这一目标的一个非常重要的技巧最终是相当直接的，事实证明，一个对这种大批量训练非常有效的简单启发式方法是按比例缩放学习率。假设你有一个模型，在单个GPU上使用学习率α和批次大小n训练效果很好，那么你通常可以在K个GPU上进行训练，每个GPU的批次大小为K乘以n，学习率为K乘以α。

So basically, you scale the number of devices by K, you scale the batch size by K, you also scale the learning rate by K, and this is the most important trick for getting things to work with very large batches.

所以基本上，设备数量按K倍缩放，批次大小按K倍缩放，学习率也按K倍缩放，这是让模型在非常大批次下正常工作的最重要技巧。

The other trick is that when you imagine our batches are very, very large and we're maybe trading up to 1,000 GPUs, then that learning rate is going to be very large because now we're training like 1000 times whatever our old learning rate was.

另一个技巧是，当你想象我们的批次非常大，可能要用到上千个GPU时，学习率会变得非常大，因为现在的训练速度相当于旧学习率的1000倍。

The problem is that this very large learning rate can sometimes explode in the very first iteration of training. So what people do is they often use a learning rate warm-up schedule where they'll start the learning rate from zero and then gradually increase it over the first maybe thousand or five thousand iterations of training.

问题在于这种极大的学习率有时会在训练的第一轮迭代中就爆炸。因此人们通常采用学习率预热方案，从零开始逐步提升学习率，这个过程可能持续最初的一千到五千次迭代。

At that point they'll continue with whatever other learning rate schedule they would have used originally. There's a couple other tricks that you need to use to get this to actually work, and I would recommend you check out this paper if you're interested in them.

之后他们会继续采用原本计划的其他学习率调整方案。要让这种方法真正奏效还需要一些其他技巧，如果你感兴趣的话我推荐你查阅这篇论文。

The basic idea is that once you get all these tricks to work, people were able to train models on ImageNet very quickly using a very large number of GPUs.

基本思路是，一旦掌握了所有这些技巧，人们就能使用大量GPU非常快速地训练ImageNet模型。

This relatively well-known paper was able to train models on ImageNet in just one hour. The secret is that they used a batch size of 8,192 and distributed this across 256 GPUs.

这篇比较知名的论文实现了在一小时内完成ImageNet模型训练。秘诀在于他们使用了8,192的批次大小，并将其分布在256个GPU上。

They had kind of a clickbait title for the paper called "ImageNet in One Hour". The problem is that once you write a paper called "ImageNet in One Hour", you are just begging for people to try to come and beat you.

论文采用了类似"一小时完成ImageNet训练"这样的吸睛标题。问题在于，一旦你发表了这样的论文，就等于在邀请别人来超越你。

After this paper came out, it seemed like all the big industrial labs were just tripping over themselves to try to compete and see how many hardware resources they could throw at this problem and how fast they could train on ImageNet.

这篇论文发表后，各大工业实验室似乎都争相参与竞争，试图看自己能投入多少硬件资源来解决这个问题，以及能在ImageNet上实现多快的训练速度。

After this paper came out they got one hour of training on 256 GPUs. Not long after, another paper trained with a batch size of 12,000 - it was a group from Intel pushing their new Knights Landing devices that was kind of like a GPU, and they achieved training in 39 minutes.

这篇论文发表后，他们在256个GPU上实现了一小时训练。不久后，另一篇论文使用了12,000的批次大小——这是英特尔的一个团队在推广他们类似GPU的新型Knights Landing设备，最终实现了39分钟的训练时长。

Then another group at Intel came out - they trained with a batch size of 16,000 on 1,600 Xeon CPUs and achieved training in 31 minutes.

随后英特尔的另一个团队出现——他们在1,600个至强CPU上使用16,000的批次大小，实现了31分钟的训练时长。

Then another group came out and they got it to train in 15 minutes, but they trained with a batch size of 32,000 on a thousand GPUs.

接着又有一个团队实现了15分钟的训练时长，但他们在上千个GPU上使用了32,000的批次大小。

If you look, this is kind of linear right? If you compare this original ImageNet in one hour, then you can train four times as fast with four times as big a batch size on four times as many devices.

观察这些数据，这几乎是线性增长的对吧？与最初的一小时ImageNet训练相比，使用四倍的批次大小和四倍的设备数量，就能实现四倍的训练速度。

In practice, if you read papers from big industrial labs, they'll often use these tricks in practice.

实际上，如果你阅读大型工业实验室的论文，会发现他们经常在实践中运用这些技巧。