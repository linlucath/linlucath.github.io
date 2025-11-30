---
title: 'Lecture6_Notes'
publishDate: 2025-11-26
description: 'TODO'
tags:
  - TODO
language: 'English'
heroImage: { src: './default.jpg', color: '#D58388' }
---

So welcome back to the class. We're up to lecture six and today we're going to talk about back propagation.

欢迎回到课堂。我们进行到第六讲，今天我们将讨论反向传播。

Where we are in this class is that last time we talked about neural networks. We saw that neural networks are a very powerful class of classifiers that let us do much more powerful computation than had been possible with the linear classifiers we have been considering so far.

我们在课程中的进度是，上次我们讨论了神经网络。我们看到神经网络是一类非常强大的分类器，使我们能够进行比迄今为止考虑的线性分类器更强大的计算。

You'll recall that neural networks had this fairly simple functional form of a matrix multiply with element-wise non-linearity that we called an activation function. Together with another matrix multiply, we could chain these things together to get really deep neural networks.

您会记得神经网络具有相当简单的函数形式：矩阵乘法与逐元素非线性（我们称之为激活函数）相结合。再加上另一个矩阵乘法，我们可以将这些组件链接在一起形成真正的深度神经网络。

We saw this notion of space warping to demonstrate the way in which neural networks were much more powerful than linear classifiers. Neural networks are now able to have these nonlinear decision boundaries in input space.

我们看到了空间扭曲的概念，用以演示神经网络比线性分类器强大得多的方式。神经网络现在能够在输入空间中具有这些非线性决策边界。

We also talked about neural networks as the universal approximator to give another notion in which neural networks are a very powerful class of functions. We also saw this notion of non-convexity that neural networks, despite their powerful ability to represent many functions, resulted in non-convex optimization problems that have very few theoretical guarantees.

我们还讨论了神经网络作为通用逼近器，这提供了另一个视角说明神经网络是一类非常强大的函数。我们也看到了非凸性的概念，即神经网络尽管具有表示许多函数的强大能力，却导致了非凸优化问题，这些问题几乎没有理论保证。

Now we were then left with a bit of a problem at the end of the last lecture. We have the ability to write down these very complicated expressions that describe loss functions that we want to minimize using stochastic gradient descent in order to train our classifiers.

现在我们在上一讲结束时留下了一个问题。我们能够写出这些非常复杂的表达式来描述损失函数，我们希望使用随机梯度下降来最小化这些损失函数，以便训练我们的分类器。

The problem here is how do we actually go about computing gradients in these models. We know that we can write down arbitrary loss functions and if we can find some way to compute the gradient of a loss with respect to all the weight matrices of a model, then we can use the optimization algorithm as we talked about a few lectures ago.

The topic of today's lecture is how do we actually go about computing these gradients for arbitrarily complex types of neural networks or other types of functions. The first strategy that you might be familiar with that you might try to adopt if you just attack this problem naively is to just derive the gradients on paper.

今天讲座的主题是我们实际上如何计算这些任意复杂类型的神经网络或其他类型函数的梯度。如果你只是简单地处理这个问题，可能熟悉并尝试采用的第一个策略就是在纸上推导梯度。

You know that we can write down these score functions, these loss functions and expand out the loss function on paper as an equation with many terms. Here I've expanded out the SVM loss function with a linear classifier.

我们知道可以写下这些评分函数、这些损失函数，并在纸上将损失函数展开为包含多项的方程。这里我展开了使用线性分类器的SVM损失函数。

One strategy you might go about doing this is you just write it all down on paper, you expand out all the terms and you end up with a giant equation that represent the loss as a function of your data and the weights of your model. Then if you're very familiar with the rules of matrix calculus, you could imagine trying to churn through this and just compute expressions on paper for all the learnable weight matrices that appear in the model.

你可能采用的一种策略就是把所有内容都写在纸上，展开所有项，最终得到一个巨大的方程，将损失表示为数据和模型权重的函数。然后如果你非常熟悉矩阵微积分的规则，你可以想象尝试通过这个方程进行推导，在纸上为模型中出现的所有可学习权重矩阵计算表达式。

This turns out to not be a very scalable solution. I apologize if anyone actually attempted this for the second assignment. If you did go this route and try to compute these weight matrices on paper for the second assignment, you will have noticed some of the potential shortcomings of this approach.

事实证明这不是一个非常可扩展的解决方案。如果有人真的在第二次作业中尝试了这种方法，我表示歉意。如果你确实走了这条路，并尝试在纸上计算第二次作业的这些权重矩阵，你会注意到这种方法的一些潜在缺点。

One is that it's extremely tedious. You likely needed quite a lot of paper to get this thing right as you are working with loss functions like the cross entropy loss function or the SVM loss. Another problem is that it's not very feasible for complex models.

其一是极其繁琐。当你处理交叉熵损失函数或SVM损失等损失函数时，你可能需要相当多的纸张才能正确完成。另一个问题是对于复杂模型来说不太可行。

For something like a linear model I think you can probably get by with this approach, but as we scale to much more complex models, this approach of writing down gradients and deriving them on paper will just not scale to more complex models. And a final somewhat subtle problem with deriving everything on paper is that it does not lead to a modular design.

对于线性模型这样的简单情况，我认为你或许可以通过这种方法应付，但随着我们扩展到更复杂的模型，这种在纸上写下梯度并进行推导的方法将无法适应更复杂的模型。在纸上推导所有内容的最后一个有些微妙的问题是它不会产生模块化设计。

Now suppose that once you've derived your loss function for a linear classifier with an SVM loss, and now tomorrow you want to derive the gradients for a linear classifier with a softmax loss, or a two layered neural network with a softmax loss or a five layer neural network with an SVM loss or any other kind of combination of losses and architectures and regularizers that you might imagine.

假设你已经推导出了使用SVM损失的线性分类器的损失函数，而明天你想要推导使用softmax损失的线性分类器的梯度，无论是带有softmax损失的双层神经网络，还是带有SVM损失的五层神经网络，或是您能想象到的任何损失函数、架构和正则化器的组合。

If you were deriving these things from scratch on paper for every combination of loss function and architecture, you would have to derive everything from scratch every time.

如果您要为每种损失函数和架构组合在纸上从头推导这些内容，每次都必须完全重新推导。

And in practical situations, it's much nicer to have some modular approach where you can just swap in and slot out different types of models and architectures and loss functions that will allow you to iterate much more quickly as you try to find models that work well.

在实际应用中，采用模块化方法会更好，您可以直接替换不同类型的模型、架构和损失函数，这样在寻找有效模型时能够更快地进行迭代。

The approach that we tend to take in deep learning is actually, you may have guessed, not deriving gradients on paper.

我们在深度学习中通常采用的方法，您可能已经猜到，并不是在纸上推导梯度。

And since we're computer scientists, we'd like to try to find data structures and algorithms that can help us solve tedious problems.

由于我们是计算机科学家，我们希望找到能够帮助我们解决繁琐问题的数据结构和算法。

The data structure that we use to help us solve this problem of computing gradients is called a computational graph.

我们用来帮助解决梯度计算问题的数据结构称为计算图。

Now a computational graph is a directed graph that represents the computation we were performing inside our model.

计算图是一种有向图，表示我们在模型内部执行的计算。

Here on the Left we can see the inputs to the model the data X and the labels Y maybe not in this graph but maybe here we have the data X and the learnable weights W coming in as nodes on the left of the graph.

在左侧我们可以看到模型的输入数据X和标签Y，可能不在这个图中，但这里我们有数据X和可学习权重W作为节点出现在图的左侧。

And now as we proceed from left to right in this graph we see nodes that represent bits of fundamental computation that we want to perform in the process of computing this function.

当我们从左到右遍历这个图时，我们看到代表基本计算单元的节点，这些是我们在计算该函数过程中想要执行的操作。

So we see that there's this blue node that represents the matrix multiplication between the input X and the weight matrix W.

我们看到有一个蓝色节点代表输入X和权重矩阵W之间的矩阵乘法。

We have this red node that represents our hinge loss if we're using an SVM classifier.

如果使用SVM分类器，我们有一个红色节点代表我们的合页损失。

We have this green node that represents the regularization term in our model.

我们有一个绿色节点代表模型中的正则化项。

We have a sum that represents the sum of the data loss and the regularization loss and then finally on the right we have the output of the computational graph which is the scalar loss L that we want to compute when training our model.

我们有一个求和节点代表数据损失和正则化损失的总和，最后在右侧我们有计算图的输出，即训练模型时想要计算的标量损失L。

Now this computational graph formalism when applied to something like a linear model might seem a little bit silly and a little bit trivial. Because in a linear model, as we said, there's only a couple operations that we need to perform in order to compute the loss. The formalism of writing the bound as a graph might seem a little bit like a little bit of overkill.

现在这种计算图形式化方法当应用于线性模型时可能看起来有点简单甚至微不足道。因为在线性模型中，正如我们所说，计算损失只需要执行几个操作。用计算图的形式来表示这个过程可能显得有点小题大做。

But this will become critical as we move to more complex and larger models. For example, something like AlexNet is a deep convolutional neural network with seven convolutional layers. A non-linearity and regularizer is at every layers and a loss function at the end.

但当我们转向更复杂、更大的模型时，这种方法就变得至关重要。例如，AlexNet是一个深度卷积神经网络，包含七个卷积层。每一层都有非线性和正则化处理，最后是损失函数。

Where now the images are coming in at the top and it's going through many many layers of processing. Our final scalar loss is coming out at the bottom. Something like this you probably do not want to derive the gradients on paper.

图像从顶部输入，经过很多层的处理。最终标量损失从底部输出。对于这样的模型，你可能不想在纸上手动推导梯度。

Instead you really want to use this computational graphic formalism. Track the data structure to build up a data structure that represents all of the computation that the model will perform in order to compute the loss.

相反，你确实需要使用这种计算图形式化方法。跟踪数据结构，构建一个代表模型为计算损失而执行的所有计算的数据结构。

This will and this these things can get arbitrarily crazy. So here's an example of a model called a neural Turing machine. If you remember your theory of intro of theory to computation class, you remember that a Turing machine is this a formalized model of computation.

这些计算图可能会变得极其复杂。这里有一个神经图灵机模型的例子。如果你还记得计算理论导论课程，你会记得图灵机是一种形式化的计算模型。

Well it turns a couple years ago some folks wrote a neural network that is kind of like a soft differentiable approximation to the Turing machines that you learn in this intro the computation class. The here on the screen we're showing the computational graph that arises from this differentiable neural Turing machine.

几年前，一些研究人员开发了一种神经网络，它类似于你在计算导论课上学到的图灵机的软可微分近似。屏幕上显示的是这个可微分神经图灵机产生的计算图。

Extend you can see that it's very big and complex. And you definitely want don't want to compute gradients in this model by hand. You really want to rely on the computational graph formalism to compute gradients for you.

可以看到它非常庞大和复杂。你绝对不想在这个模型中手动计算梯度。你确实需要依赖计算图形式化方法来为你计算梯度。

But actually it gets even worse than this. Because for the neural Turing machine, this is showing only one time step of the model. And in practice this model gets unrolled over many time steps as a kind of recurrent Network.

但实际上情况比这更复杂。因为对于神经图灵机，这只是显示了模型的一个时间步。实际上，这种模型会作为循环网络在多个时间步上展开。

So you can see that this this you bury once you get into these very complex models. You very quickly get computational graphs that are much much too large even to fit on a slide, so you definitely want to use some kind of direct graph traversal algorithms to help us automatically compute gradients for us on top of this computational graph structure.

所以你可以看到，一旦你进入这些非常复杂的模型，你很快就会得到大到连幻灯片都放不下的计算图，因此你肯定需要使用某种直接图遍历算法，在这个计算图结构之上帮助我们自动计算梯度。

Hopefully this has motivated why it's really going to be critical for us to use computational graphs in order to compute gradients in our big complex neural network models.

希望这已经说明了为什么在大型复杂神经网络模型中，使用计算图来计算梯度对我们来说确实至关重要。

Now that we've got this motivation, let's actually see a concrete example of how we can use a computational graph to help us compute gradients in a little tiny neural network model here.

既然我们已经有了这个动机，现在让我们实际看一个具体例子，看看如何在这里的一个小型神经网络模型中使用计算图来帮助我们计算梯度。

In order to actually fit an example on a slide, we're having to use a very trivial computation, but as you've seen in real models we'll be doing much more complicated processing.

为了能在幻灯片上展示一个示例，我们不得不使用非常简单的计算，但正如你在实际模型中看到的，我们将进行更复杂的处理。

Here we're showing a very simple function of three scalar variables X, Y and Z where the output is computed by adding X and Y and then multiplied by Z to compute the loss.

这里我们展示了一个非常简单的三个标量变量X、Y和Z的函数，输出是通过将X和Y相加，然后乘以Z来计算损失。

This is maybe a weird loss function, a weird learning problem that doesn't make sense, but hopefully this simple example will help us walk through exactly what it means to compute gradients in a computational graph.

这可能是一个奇怪的损失函数，一个没有意义的学习问题，但希望这个简单示例能帮助我们逐步理解在计算图中计算梯度的确切含义。

By the way, this back propagation is the algorithm that we use for computing gradients in a computational graph.

顺便说一下，反向传播是我们在计算图中用于计算梯度的算法。

Now suppose that we want to evaluate this function at a particular point in the input space, say X is minus 2, Y is 5 and Z is minus 4.

现在假设我们想要在输入空间的特定点评估这个函数，比如X等于-2，Y等于5，Z等于-4。

The first step in using this computational graph is called the forward pass.

使用这个计算图的第一步称为前向传播。

In the forward pass, we will proceed with preceding computation from left to right and we will perform all of the operations specified by the nodes of the graph in order to compute the output values from the input values.

在前向传播中，我们将从左到右进行前向计算，并执行图中节点指定的所有操作，以便从输入值计算输出值。

In this example, we'll simply add X and Y and we'll get this intermediate that we're going to give the name of Q.

在这个例子中，我们只需将X和Y相加，就会得到一个中间结果，我们将其命名为Q。

Then to compute the final output value F, we're going to multiply Q by the input value Z.

然后为了计算最终输出值F，我们将Q乘以输入值Z。

By running the forward pass of this graph, we'll end up computing our final output value of minus 12 in this case. Now in the backward pass our goal is to compute all of the gradients, the derivatives of all of the inputs.

通过运行这个图的前向传播，我们最终将计算出本例中的最终输出值-12。在反向传播过程中，我们的目标是计算所有输入的梯度，即所有输入的导数。

So in this case our output was F, so we want to compute the derivatives DF/DX, DF/DY and DF/DZ which are the three inputs that appear on the left side of the graph.

因此在这种情况下，我们的输出是F，所以我们需要计算导数DF/DX、DF/DY和DF/DZ，这些是出现在图左侧的三个输入。

We'll proceed from left to right because this is back propagation, so it needs to proceed backward compared to the forward pass.

我们将从左到右进行处理，因为这是反向传播，所以需要与前向传播方向相反。

We always start with the base case, so in the base case on the right we want to compute the derivative of F with respect to F.

我们总是从基本情况开始，所以在右侧的基本情况中，我们需要计算F关于F的导数。

Anyone got an idea what that ought to be? Yeah, that's trivial, that's one because if we change F a little bit, then F is gonna change by the same amount so the derivative is 1.

有人知道这应该是什么吗？是的，这很简单，就是1，因为如果我们稍微改变F，那么F会以相同量变化，所以导数为1。

When we're computing back derivatives or using back propagation in a graph, we often write a little diagram like this where we show the values that are computed at each node above the corresponding line, and then we'll write the gradients or the derivatives below the corresponding line during the backward pass.

当我们在图中计算反向导数或使用反向传播时，我们经常写这样的小图：在对应线上方显示每个节点计算的值，然后在反向传播期间在对应线下方写出梯度或导数。

Now the second step is we need to compute the derivative of F with respect to Z, and in order to do this we know we can look at this little intermediate computation.

现在第二步是计算F关于Z的导数，为了做到这一点，我们知道可以查看这个小的中间计算。

We know that F was Q times Z, so we know that the derivative of F with respect to Z should just be Q.

我们知道F等于Q乘以Z，所以我们知道F关于Z的导数应该就是Q。

Then we can go back and look into the computational graph and look up what the value of Q was, in this case it was three.

然后我们可以回到计算图中查找Q的值，在这个例子中它是3。

So now the derivative of F with respect to Z in this little piece of the graph is now going to be three, and that's now we've got one of our three gradients that we needed to compute.

所以现在图中这一小部分的F关于Z的导数将是3，这样我们就得到了需要计算的三个梯度中的一个。

The next piece we need to compute the derivative of F with respect to Q.

接下来我们需要计算F关于Q的导数。

You can see that we're kind of marching backward in an opposite topologically sorted version of the graph, and in order to compute the gradient of derivative of F with respect to Q, we again know that F is Q Z. So this local derivative should be Z.

你可以看到我们正在以图的反向拓扑排序方式向后推进，为了计算F对Q的梯度导数，我们再次知道F等于Q乘以Z。因此这个局部导数应该是Z。

We can look up the value of Z from the forward pass of the graph and compute the derivative as minus 4.

我们可以从图的前向传播中查找Z的值，并将导数计算为负4。

We can consider continued proceeding to the left now. We want to compute the derivative of F with respect to Y.

现在我们可以考虑继续向左推进。我们想要计算F对Y的导数。

And here things get a little bit interesting because now we need to remember the chain rule from calculus. Because here the value Y is not directly connected to the output value F.

这里情况变得有点意思，因为现在我们需要回忆微积分中的链式法则。因为这里的Y值并不直接与输出值F相连。

So in order to compute the derivative of F with respect to Y, we need to take into account the influence of Y on the intermediate variable Q.

因此为了计算F对Y的导数，我们需要考虑Y对中间变量Q的影响。

Then the chain of the single variable chain rule from calculus tells us that DF dy is equal to D of DQ dy times DF DQ.

然后单变量链式法则告诉我们，DF dy等于DQ dy乘以DF DQ。

And this is very intuitive right. The idea is that if Y changes by a little bit, then Q is going to change by some a little bit DQ dy.

这非常直观。其思想是如果Y发生微小变化，那么Q将会发生微小变化DQ dy。

And then if Q changes, then f is going to change some little bit which is the other derivative.

然后如果Q发生变化，那么f将会发生微小变化，这就是另一个导数。

So then to take into account these two effects, we need to multiply them.

因此为了考虑这两种影响，我们需要将它们相乘。

Now here in this case, in the context of neural networks, these three different terms in this equation have particular names that we'll use over and over again.

现在在这种情况下，在神经网络的背景下，这个方程中的三个不同项有着特定的名称，我们将反复使用这些名称。

So this left-hand term the f dy will often call the downstream gradient because this is the value of the derivative that we're computing at this step in the process.

所以左边这个项f dy通常被称为下游梯度，因为这是我们在当前步骤计算的导数值。

This value DQ dy is going to be called the local gradient because this is the local effect of how much this value of Y affects this next intermediate output Q.

这个值DQ dy将被称为局部梯度，因为这是Y值对下一个中间输出Q的局部影响。

And this value d f dy is going to be called the upstream gradient because this is the effect where because network will kind of a match zooming in on this little piece of the graph around Y and the upstream gradient tells us how much does the output of this piece of the graph affect the final output at the very end of the graph. Then of course the chain rule tells us that to get the downstream gradient, we just need to multiply this local upstream derivatives.

而这个值d f dy将被称为上游梯度，因为这是网络在Y周围这一小块图上放大的效果，上游梯度告诉我们图的这一部分的输出对图最末端最终输出的影响程度。当然，链式法则告诉我们，要获得下游梯度，我们只需要将这些局部上游导数相乘。

Here we know that the local gradient in this case is just one because Q is equal to X plus Y. So when we multiply these two together, we know that the derivative of F with respect to Y is the same as the derivative of F with respect to Q.

我们知道在这种情况下局部梯度正好为1，因为Q等于X加Y。因此当我们将这两者相乘时，我们知道F对Y的导数与F对Q的导数相同。

So we get our downstream gradient of minus four. Now it's very similar when we want to compute this other thing. We again need to multiply the upstream and local gradients.

这样我们就得到了负四的下游梯度。现在当我们计算另一个量时情况非常相似。我们再次需要将上游梯度和局部梯度相乘。

The local gradient is one because this was a simple addition. We compute our final output value so here you can see in this relatively simple example how we can use computational graphs to help us mechanize the process of computing derivatives in very complex functions.

局部梯度为1，因为这是一个简单的加法运算。我们计算最终输出值，因此在这个相对简单的例子中，你可以看到如何使用计算图来帮助我们机械化地计算复杂函数的导数。

During the forward pass we're going to compute everything from left to right. Then the backwards pass we're going to step backwards through the graph and compute these little derivatives at every point.

在前向传播过程中，我们将从左到右计算所有内容。然后在反向传播过程中，我们将沿着图向后逐步计算每个点的这些微小导数。

This way of thinking about computing gradients is very useful because it's modular. One way to think about it is we can zoom in on one little node inside this computational graph.

这种计算梯度的思维方式非常有用，因为它是模块化的。一种理解方式是我们可以聚焦于计算图中的单个小节点。

What's really great about this mechanism for using back propagation to compute gradients is that each little piece of the graph does not need to know or care about the rest of the graph.

使用反向传播计算梯度的机制最棒的地方在于，图的每个小部分都不需要了解或关心图的其余部分。

We can just perform local processing within each node. Then by aggregating all this local processing we can end up computing these global derivatives throughout the entire graph.

我们只需在每个节点内执行局部处理。然后通过聚合所有这些局部处理，我们最终能够计算整个图中的这些全局导数。

If we step through this exact same process in the context of a single local node, it looks something like this. For each node in the graph we're computing some little local function f.

If we step through exactly the same process in the context of a single local node, it looks like this. For each node in the graph, we are computing some small local function f. This local function f takes two inputs x and y. During the forward pass, we apply the local function to compute the local output Z.

如果我们在单个局部节点的背景下逐步执行完全相同的过程，它看起来是这样的。对于图中的每个节点，我们都在计算某个小的局部函数f。这个局部函数f接收两个输入x和y。在前向传播过程中，我们应用局部函数来计算局部输出Z。

Now that's what forward operation of this little independent node. After the forward operation of this node runs, this output Z will be passed off to some other part of the graph and that might be reused by other nodes in multiple arbitrary complex ways.

这就是这个独立小节点的前向操作。该节点完成前向操作后，输出Z将被传递给计算图的其他部分，并可能被其他节点以多种复杂方式重复使用。

We don't know and we don't care from the perspective of this one node. We just know that we computed an output and we passed it on to someone else.

从该节点的角度来看，我们不知道也不关心这些细节。我们只知道计算了一个输出并将其传递给了其他节点。

Then at the end of the process, eventually somehow at the end of the graph, someone far away from us will compute some final loss L. Then back propagation will start and somewhere outside of us will pass these gradients back to us outside of the purview of this little node.

然后在过程结束时，最终在计算图的末端，远离我们的某个节点将计算最终损失L。接着反向传播开始，我们外部的某个部分将在该小节点范围之外将这些梯度传递回来。

Eventually this back propagation process will hit this one node that we care about. This one node will receive a message from upstream in the graph which tells us the derivative of the loss with respect to Z.

最终反向传播过程将到达我们关心的这个节点。该节点将从计算图的上游接收一个消息，告诉我们损失相对于Z的导数。

That is how much does this loss, which may be very far away from this node, change if we change the local output of our node by a little bit. That's exactly what this upstream gradient dL/dZ tells us.

这表示如果我们稍微改变节点的局部输出，这个可能距离该节点非常遥远的损失会变化多少。这正是上游梯度dL/dZ告诉我们的信息。

Now at this point we can compute the local gradients that are internal to this node. These tell us for each output of the node how much each output is affected by each input of the node.

此时我们可以计算该节点内部的局部梯度。这些梯度告诉我们节点的每个输出受每个输入影响的程度。

Now this node can simply compute the downstream gradients by multiplying the local gradients and the upstream gradients. These downstream gradients then get passed along to other nodes backwards in the graph.

现在该节点可以通过将局部梯度与上游梯度相乘来简单计算下游梯度。这些下游梯度随后被反向传递给计算图中的其他节点。

Again this node doesn't need to know or care exactly how those downstream gradients will be used elsewhere in the graph. They're simply used somewhere and by the end when this final back propagation process terminates, we'll be left with having computed the gradients of the loss with respect to all of the original inputs of the graph. And this we were able to compute this local and global property without really reasoning at all about the global structure of the function we were trying to compute. It only required us to think about locally what's going on inside each node of the graph and then have some data structure to track how all those nodes are connected together.

同样，该节点不需要知道或关心这些下游梯度将在计算图的其他地方如何被使用。它们只是被用在某处，当最终的反向传播过程结束时，我们将得到损失相对于计算图所有原始输入的梯度。我们能够计算这个局部和全局属性，而无需真正推理我们试图计算的函数的全局结构。它只需要我们局部思考图中每个节点内部的情况，然后使用某种数据结构来跟踪所有这些节点是如何连接在一起的。

So hopefully this is going to be a big improvement what we're trying to derive those big gradient expressions on paper. Here's another example of running computational graph here this should look something like a logistic classifier you kind of care about these equations.

因此希望这将是一个重大改进，我们试图在纸上推导那些复杂的梯度表达式。这是运行计算图的另一个示例，这里看起来应该类似于逻辑分类器，你会关心这些方程。

But the details of exactly what the functions are computing for the purpose of this lecture are somewhat irrelevant we just care about computing gradients and arbitrarily complex functions. But here on the Left we're showing we've got a function that takes for what 1 2 3 4 5 inputs our 5 inputs are W 0 X 0 W 1 X 1 W 2.

但就本讲座目的而言，函数具体计算什么的细节有些无关紧要，我们只关心计算梯度和任意复杂函数。但在左边我们展示了一个函数，它接受1 2 3 4 5个输入，我们的5个输入是W 0 X 0 W 1 X 1 W 2。

And now in the forward pass we're going to compute this inner product between these first two elements of the weight and the first two elements of the X and then we're going to compute this bias term to add W 2. Then we're going to compute some kind of e to the minus something on this computational graph.

现在在前向传播中，我们将计算权重的前两个元素和X的前两个元素之间的内积，然后计算这个偏置项来加上W 2。接着我们将在这个计算图上计算某种e的负指数运算。

And here the computation will proceed much as we saw in a previous example in the forward pass we'll compute the outputs of the graph by evaluating the forward function for each of these nodes and this will end up computing this final scalar output value on the right.

这里的计算过程与我们之前看到的例子类似，在前向传播中，我们将通过评估每个节点的前向函数来计算图的输出，最终将计算出右侧这个最终的标量输出值。

And then during the backward pass we'll iteratively think about how it will it early multiply the upstream gradient by the local gradient at each node in the graph to compute the downstream gradients. So we'll always start with this base case base case of derivative of output with respect to itself is always 1.

然后在反向传播过程中，我们将迭代思考如何将上游梯度乘以图中每个节点的局部梯度来计算下游梯度。因此我们总是从这个基本情况开始：输出相对于自身的导数总是1。

Next we'll look at this 1 over X. We know that the local gradient of 1 over X is minus 1 over x squared, which gives us the local gradient we can multiply these to get the downstream gradients. We can step through again - adding a constant has local gradient of 1 so we can easily pass those gradients backward. We can compute the local gradient of this exponential function which is trivial.

接下来我们来看这个1/X。我们知道1/X的局部梯度是-1/x²，这给出了我们可以相乘得到下游梯度的局部梯度。我们可以再次逐步分析 - 添加常数的局部梯度为1，因此我们可以轻松地将这些梯度向后传递。我们可以计算这个指数函数的局部梯度，这很简单。

We can step through again. The adding a constant has local gradient of 1, so we can easily pass those gradients backward. We can compute the local gradient of this exponential function which is trivial, so that just lets us easily compute downstream gradient.

我们可以再次逐步分析。添加常数的局部梯度为1，因此我们可以轻松地反向传递这些梯度。我们可以计算这个指数函数的局部梯度，这很简单，因此我们可以轻松计算下游梯度。

This kind of process steps backward one node at a time, where at each point we're just computing these local gradients and then multiplying the upstream and local gradients.

这种过程一次向后移动一个节点，在每个点我们只是计算这些局部梯度，然后将上游梯度和局部梯度相乘。

But what's really interesting about this particular computational graph is that there's multiple ways in which we could have thought to structure this computation. In this graph as I've written it, I've written it out in terms of very basic, very primitive, very fundamental arithmetic operators of addition and multiplication, exponentiation, division, adding a constant.

但这个特定计算图真正有趣的地方在于，我们可以用多种方式来构建这个计算结构。在我写的这个图中，我使用了非常基础、非常原始、非常基本的算术运算符来表示，包括加法、乘法、指数运算、除法、添加常数等。

I have broken down this computation into its barest fundamental pieces of arithmetic primitives. As you saw by looking at this graph, breaking everything down into the barest arithmetic primitives at every graph ends up a little bit tedious.

我将这个计算分解成了最基本的算术原语。正如你通过观察这个图所看到的，将所有内容都分解为最基本的算术原语最终会变得有些繁琐。

Sometimes we actually have our freedom to define for ourselves what the types of primitive operations we want to use in our graph. In this example, we've kind of broken everything down into the basic primitives, but we also have the freedom to define arbitrary new types of nodes that can internally compute more complicated functions.

实际上，我们有时可以自由定义要在图中使用的原始操作类型。在这个例子中，我们已经将所有内容分解为基本原语，但我们也可以自由定义能够内部计算更复杂函数的任意新类型节点。

An example of why this might be useful is that this little chunk of the graph that I've outlined in blue locally independently computes this so-called sigmoid function, that is 1 over 1 plus e to the minus its argument.

这种方法可能有用的一个例子是，我用蓝色勾勒出的这个小图块在局部独立计算了所谓的sigmoid函数，即1除以1加上e的负参数次方。

This sigmoid function shows up all the time in machine learning. We've seen it in the context of binary cross-entropy when you're doing a two class logistic regression. This also shows up in many other contexts.

sigmoid函数在机器学习中经常出现。我们在进行二类逻辑回归时的二元交叉熵上下文中见过它。它也在许多其他上下文中出现。

What's kind of nice is that we have the freedom as the designers of this little graph language to pick primitive graph elements that will be useful or easy to compute in the backpropagation process.

比较好的是，作为这个小图语言的设计者，我们可以自由选择在反向传播过程中有用或易于计算的原始图元素。

In particular, we can choose primitive functions to assign to nodes in the graph such that the local gradients become easy to compute. This turns out to be the case for the sigmoid function.

So the sigmoid function, if you kind of work through some math on paper, you can see that the sigmoid function actually has a very simple functional form when computing the derivative with respect to its input.

因此，如果你在纸上进行一些数学推导，会发现sigmoid函数对其输入求导时，其局部梯度实际上具有非常简单的函数形式。

That the local gradient of the sigmoid function is simply equal to the output of the sigmoid function multiplied by 1 minus the output of the sigmoid function. What this means is that we can very easily compute the local gradient of this entire blue chunk of the graph without storing this whole intermediate chunk of the graph.

sigmoid函数的局部梯度简单地等于sigmoid函数的输出乘以1减去sigmoid函数的输出。这意味着我们可以非常容易地计算整个蓝色图块的局部梯度，而无需存储这个完整的中间图块。

And this is an example of ours ourselves of the graph designers cleverly choosing the primitives that we want to use in our graph language in such a way that will make it easy or more efficient to compute the derivatives during the backwards pass. So this is definitely something you should consider doing.

这是图设计者巧妙选择图语言中基本单元的一个例子，通过这种方式使得在反向传播过程中计算导数变得更容易或更高效。所以这绝对是你应该考虑做的事情。

And now if we were to imagine kind of an aggregate, you could imagine an equivalent version of this graph which collapsed this whole blue box onto a single node that would then receive the upstream gradients on the right and now compute the local gradient using this expression we've derived at the bottom of the slide.

现在如果我们想象一种聚合，你可以想象这个图的等效版本，它将整个蓝色框折叠成一个单一节点，该节点将在右侧接收上游梯度，并使用我们在幻灯片底部推导出的这个表达式计算局部梯度。

And then immediately returned the downstream gradient on the Left kind of skipping over all of that intermediate computation inside the box. So this idea of defining more complex primitives to use in our graphs is something that we'll use quite a lot in general in order to make our computational graphs have them either be more efficient or have more semantic meaning.

然后立即在左侧返回下游梯度，跳过了框内的所有中间计算。因此，在图中定义更复杂基本单元的这个想法，我们通常会大量使用，以使我们的计算图要么更高效，要么具有更多语义含义。

Another thing we can start to notice when we look at these computational graphs is that there's some patterns that become apparent when you look at the patterns of how information propagates forward during the forward pass and then during the backward pass.

当我们观察这些计算图时，我们开始注意到的另一件事是，当你观察信息在前向传播和反向传播过程中如何传播的模式时，会出现一些明显的模式。

What I sometimes think about this is like a little circuit that during the forward pass we're kind of flowing information forward from the input to the output and then during the backward pass we're kind of flowing information backward from the loss through each of these intermediate nodes backwards to the original parameters of the model for which we wanted to compute gradients. And when you have this kind of circuit interpretation of these computational graphs, you start to notice some patterns about how gradient flow works. There is some duality between how information flows during the forward and backward passes.

我有时将其想象成一个小电路，在前向传播过程中，我们让信息从输入流向输出，然后在反向传播过程中，我们让信息从损失函数通过每个中间节点向后流动，回到我们想要计算梯度的模型原始参数。当您对计算图进行这种电路式解读时，您会开始注意到梯度流动的一些模式。在前向传播和反向传播过程中，信息流动方式存在某种对偶性。

The simplest example of this is that this add gate or add function acts as a gradient distributor during the backward pass. If we have a little function which locally computes the output as the sum of its two inputs, maybe here seven is three plus four, then during the backward pass we know that the derivative of X plus y with respect to X is 1 and derivative of X plus y over y is also 1.

最简单的例子是加法门或加法函数在反向传播过程中充当梯度分配器。如果我们有一个局部计算输出为两个输入之和的小函数，比如这里的7等于3加4，那么在反向传播过程中，我们知道X加y对X的导数为1，X加y对y的导数也为1。

The local gradients are both 1, so that means the downstream gradients for both inputs are both equal to the upstream gradient. This would generalize to a sum with an arbitrary number of terms.

局部梯度都是1，这意味着两个输入的下游梯度都等于上游梯度。这一规律可以推广到任意数量项的求和运算。

What this means is that during the backward pass when you have a sum node, that sum node is going to distribute and copy those gradients from the upstream into the downstream. This is kind of a nice intuition about what's happening when you have addition inside of your model.

这意味着在反向传播过程中，当存在求和节点时，该节点会将梯度从上游分配并复制到下游。这很好地解释了模型内部进行加法运算时发生的情况。

Now kind of dual to the sum node is the copy node. This is kind of a trivial node that during its input maybe receive some input and then has two output values that are both equal to identical copies of the input.

与求和节点形成对偶的是复制节点。这是一个简单的节点，在其输入阶段可能接收某个输入，然后产生两个输出值，这两个输出值都是输入值的完全相同副本。

This seems like maybe a stupid operation at first glance. Why would you ever introduce such an operation in your graph? Well you might want to do this if you want to use one term of your model in multiple places downstream in the graph.

乍看之下这似乎是个愚蠢的操作。为什么要在图中引入这样的操作？如果您希望在图中多个下游位置使用模型的某个项，您可能就需要这样做。

For example, you might imagine in a regularization setting we actually want to use each of our weight matrices in two ways in our model. We want to use the weight matrix one to compute scores in the main branch of the model and second we need to use the weight matrix to compute our regularization term like L2 or L1 regularization.

例如，在正则化设置中，我们实际上希望以两种方式使用模型中的每个权重矩阵。我们希望在模型的主分支中使用权重矩阵来计算得分，同时需要使用权重矩阵来计算正则化项，如L2或L1正则化。

So in order to use our weight matrix in two downstream parts in the graph, we might imagine inserting a copy node somewhere in the graph. That now makes two identical copies of the weight matrix that can be used in different parts of the graph. The important bit is that even when we've produced these two copies, because they may have been used in different ways, we might end up computing different gradients with respect to the two copies.

因此，为了在图中两个下游部分使用我们的权重矩阵，我们可能会考虑在图中某处插入一个复制节点。现在创建了两个相同的权重矩阵副本，可以在计算图的不同部分使用。关键在于即使我们生成了这两个副本，由于它们可能以不同方式被使用，我们最终可能会针对这两个副本计算出不同的梯度。

During the backward pass, the upstream gradients that the copy node receives might be different for the two outputs that it's produced. But during the backward pass, we simply need to sum those two gradients, which shows that the add gate and the copy gate are somehow dual.

在反向传播过程中，复制节点接收到的上游梯度可能因其产生的两个输出而不同。但在反向传播过程中，我们只需要将这两个梯度相加，这表明加法门和复制门在某种程度上是对偶的。

The add gate forward operation is kind of the same as the copy gate backward operation and vice versa. So these two operations are somehow dual to each other.

加法门的前向操作类似于复制门的反向操作，反之亦然。因此这两种操作在某种程度上是相互对偶的。

Another funny thing that's going on is the multiplication. You can think of this as a kind of swap multiplier because the derivative of XY with respect to X is Y and derivative of XY with respect to Y is X. This means the local gradient for one of the inputs is the other input, and the local gradient for the second input is the first input.

另一个有趣的现象是乘法运算。你可以将其视为一种交换乘法器，因为XY对X的导数是Y，XY对Y的导数是X。这意味着其中一个输入的局部梯度是另一个输入，而第二个输入的局部梯度是第一个输入。

When we compute the downstream gradient, the downstream gradient is equal to the upstream gradients times the other input. This has a funny implication if you think about now we have a multiplication inside your model - it's gonna mix the gradients all up in some kind of a funny way.

当我们计算下游梯度时，下游梯度等于上游梯度乘以另一个输入。如果你考虑到模型内部存在乘法运算，这会产生有趣的影响——它会以某种有趣的方式混合所有梯度。

Because during the backward pass of a multiplication gate also involves multiplication, you can see that you're going to end up with some very large products in the backward pass. You can imagine this might be a problem in certain types of models.

由于乘法门的反向传播过程也涉及乘法运算，你会发现在反向传播中最终会出现一些非常大的乘积。可以想象在某些类型的模型中这可能是个问题。

Another one that you might see a lot is a max gate. So here the max gate is gonna take two scalar inputs and return the maximum of the two inputs. Here what does that function look like? That looks kind of like a ReLU function a little bit. And you can imagine that for the input that was indeed the maximum, the local gradient was one and for the input which was not the maximum, the local gradient was zero.

另一个你可能会经常看到的是最大值门。最大值门接收两个标量输入并返回两个输入中的较大值。这个函数看起来像什么？它看起来有点像ReLU函数。可以想象，对于确实是最大值的输入，其局部梯度为1；而对于非最大值的输入，其局部梯度为0。

So this has the interpretation that during the backward pass, a max gate acts as a gradient router that it's going to take the upstream gradient and route that upstream gradient towards the one input that happens to be the max.

这意味着在反向传播过程中，最大值门充当梯度路由器的角色，它会获取上游梯度并将其路由到恰好是最大值的那个输入。

The downstream gradients of all the other inputs that were not the max are all going to be set to zero.

所有其他非最大值输入的下游梯度都将被设置为零。

So then you can imagine that if we had a model that was taking a max of like many many many things, then during the backward pass we'll end up with a gradient that is mostly zero.

因此可以想象，如果我们有一个需要对大量数值取最大值的模型，那么在反向传播过程中，我们最终会得到一个大部分为零的梯度。

So you can imagine maybe that's a problem for getting good gradient flow throughout the entire model, so maybe we might not prefer to use max for that reason.

可以想象这可能会影响梯度在整个模型中的良好流动，因此我们可能不太愿意使用最大值函数。

So these are all obviously sort of trivial mathematical expressions, but it's sort of interesting to think about how these trivial derivatives of scalar functions actually can have non-trivial consequences for the way in which gradients tend to flow through these giant neural network models.

这些显然都是相当简单的数学表达式，但思考这些标量函数的简单导数如何对梯度在大型神经网络模型中的流动方式产生非平凡的影响，这确实很有趣。

So now that we've hopefully gotten a bit of intuition about what is back propagation and how can it help us automate the process of computing gradients in big models, I think it's helpful for you guys to talk about how you actually might implement this stuff in code.

既然我们已经对反向传播是什么以及它如何帮助我们自动化计算大型模型中的梯度有了一些直观理解，我认为现在讨论如何在代码中实际实现这些内容对你们很有帮助。

Because that's something you have to do in your homework, so hopefully you will get good at that.

因为这是你们作业中必须完成的内容，希望你们能在这方面做得很好。

I think there are at least two major ways in which I tend to think about implementing backpropagation.

我认为至少有两种主要方式来实现反向传播。

The first is what I call a flat implementation of backpropagation, so here the idea is we're gonna write a single Python function that computes the entire computational graph.

第一种是我称之为反向传播的扁平实现，其思路是编写一个单独的Python函数来计算整个计算图。

Maybe this Python function is computing a linear classifier on an input, so mini-batch of data and your weights, and then it computes the loss on that mini batch of data.

这个Python函数可能是在输入上计算线性分类器，处理小批量数据和权重，然后计算该小批量数据的损失。

Sound familiar from homework to hopefully those of you who looked at it.

And now you're asked to compute a single function that is going to both compute the loss and compute the derivative of the loss with respect to each of those weight matrices.

现在你需要编写一个单一函数，既要计算损失值，又要计算损失相对于每个权重矩阵的导数。

Well the way you would - I mean one thing you can do is kind of go to town on paper and kind of like you do your derivatives and it's a mess and hopefully you'll eventually pass the gradient check.

你可以采用的方式——我的意思是，一种做法是在纸上大展身手，进行各种导数计算，过程会很混乱，希望最终能通过梯度检查。

But you can try to structure this computation in a much simpler way that I think makes writing this different this backward pass code actually very simple in this case.

但你可以尝试以更简单的方式组织这个计算过程，我认为这样编写反向传播代码实际上会变得非常简单。

So here we'll have as an example we're going to have this little computational graph on the Left which is the sigmoid example from a couple slides ago.

这里我们以左侧这个小计算图为例，这是几页幻灯片前展示的sigmoid函数示例。

We're going to input our two weights W0 W1 or 2 inputs X0 X1 and our bias term W to the forward pass of our code is simply going to apply this AB mult add a multiply these things and compute our long L.

我们将输入两个权重W0、W1或两个输入X0、X1，以及偏置项W，前向传播代码只需应用这个AB乘法加法运算，将这些数值相乘并计算我们的损失L。

Now the backward pass code is going to be right after the forward pass code and the trick is that the backward pass code is going to look like a reversed version of the forward pass code.

反向传播代码将紧随前向传播代码之后，关键在于反向传播代码看起来像是前向传播代码的逆序版本。

What do I mean by that well the you can see that the very first thing we do in the backward pass is compute this trivial base case of grad of output respect to itself as one you might you probably should actually omit this line and your actual implementation but I wanted to be super pedagogical here.

我的意思是，你可以看到在反向传播中我们首先计算这个简单的基础情况：输出相对于自身的梯度为1，在实际实现中你可能应该省略这一行，但这里我想保持教学上的完整性。

But here you can see that this first line in the backward pass code corresponds to this really rightmost thing in the computational graph.

但这里你可以看到反向传播代码的第一行对应着计算图中最右侧的元素。

Now this second line in the backward pass code corresponds to back propagate back back propagating through this sigmoid function and you can see that the back propagation lined the sigmoid function corresponds to the last line of the forward pass.

反向传播代码的第二行对应着通过这个sigmoid函数进行反向传播，你可以看到sigmoid函数的反向传播行对应着前向传播的最后一行。

And here what we notice is that in the forward pass the sigmoid function was taking as input s3 and returning as output L and now the corresponding line in the bit of backward implementation kind of inverts that around a little bit here the backward pass takes as input grad_L and produces as output grad_s3 so you can see that there's a 1:1 correspondence between this line and the forward pass and this line in the backward pass.

这里我们注意到，在前向传播中sigmoid函数以s3作为输入，返回L作为输出，而在反向传播实现中对应的行则稍微反转了这个关系，反向传播以grad_L作为输入，产生grad_s3作为输出，因此你可以看到这一行与前向传播中的这一行以及反向传播中的这一行存在一一对应的关系。

And now the inputs and the outputs are somehow swapped between these two corresponding lines.

现在输入和输出在这两条对应行之间以某种方式互换了位置。

Then this correspondence continues that in the second to last line in our forward we wanted to add these two things s2 and W2.

这种对应关系延续到前向传播倒数第二行，我们想要将s2和W2这两个值相加。

And now because this was an operation with two inputs it gives rise to two lines in the output in the corresponding backward code.

由于这是一个双输入操作，它在对应的反向传播代码中产生了两个输出行。

And again we can see that we have this intuition of the add gate as a gradient distributor.

我们再次看到加法门作为梯度分配器的直观理解。

So you can see that we're simply distributing or copying the gradient to the two inputs in each two lines.

可以看到我们只是简单地将梯度分配到两个输入，在每两行中进行复制。

A similar thing happens with this third to last line in the forward.

类似的情况发生在前向传播倒数第三行。

And now very similar things happen in this fourth to last line in the forward.

现在前向传播倒数第四行也发生了非常相似的情况。

But again now we've got a multiplication gate we've got this interpretation of this local gradient add multiply swapper.

但这次我们遇到的是乘法门，我们将其理解为局部梯度的加法乘法交换器。

And then finally we've got this final output that does it the same thing it's also a multiplication.

最后我们得到这个最终输出，它执行相同的操作，也是一个乘法运算。

Now this is kind of amazing right we actually wrote a correct implementation of backpropagation without writing out any math.

这确实很神奇，我们实际上在没有写出任何数学公式的情况下，就实现了正确的反向传播。

We didn't write down any equations on paper all we had to do is think about transfer we wrote the code for our broadcast and then we just transformed in our mind the code that we wrote in the forward pass we just transformed it to generate the code for the backward pass.

我们没有在纸上写下任何方程，我们只需要思考转换过程，我们编写了广播代码，然后在脑海中将前向传播代码转换为生成反向传播代码。

This is the way that you should actually go about doing homework two if you have not completed those assignments yet.

如果你还没有完成作业二，这实际上就是你该采用的方法。

And this will make your life much much easier when it comes to computing gradients.

这将使你在计算梯度时变得轻松很多。

It turns out that if once you get enough practice with this you almost never need to do math on paper in order to write gradient code.

事实证明，一旦你在这方面获得足够练习，你几乎永远不需要在纸上做数学计算来编写梯度代码。

You simply look at the code that you wrote for the forward pass and you just invert it using all these little local rules that you pick up over time.

你只需要查看为前向传播编写的代码，然后使用你随时间积累的所有这些小局部规则来反转它。

So this idea of flat back propagation that you implement by inverting the code from the forward pass is something that you should do in assignment two.

因此，通过反转前向传播代码来实现的扁平反向传播思想，是你在作业二中应该采用的方法。

So for example for the SVM you could imagine that we might compute the scores, compute margins, compute the data loss.

例如对于支持向量机，你可以想象我们可能会计算得分、计算边界、计算数据损失。

And now in the backward pass you'll just do all of those things in reverse, except the exact operation that occurs during each line in backward pass will be this local computation which is the multiplication of the upstream gradient and the local gradient in order to compute the downstream gradient.

而在反向传播过程中，你只需按相反顺序执行所有这些操作，只是反向传播中每行发生的具体操作将是这种局部计算——即上游梯度与局部梯度的相乘，以计算下游梯度。

And you can see that you can do this for an SVM, you can do this for a two layer neural network.

你可以看到这种方法既适用于支持向量机，也适用于两层神经网络。

I would highly recommend that you become familiar with this way of transforming your forward pass code in order to compute the backward pass.

我强烈建议你熟悉这种通过转换前向传播代码来计算反向传播的方法。

But of course this way of doing flat back propagation is really useful when you just need to write a gradient function, but it kind of fails this modularity test because with this version of flat back propagation, if we change the model or we change the activation function, we change the loss, we change the regularizer, we're gonna have to rewrite our code and that's going to be a little bit painful and annoying.

当然，这种扁平反向传播方法在只需编写梯度函数时非常有用，但它在模块化测试方面有所不足，因为使用这种扁平反向传播版本时，如果我们改变模型、激活函数、损失函数或正则化器，就不得不重写代码，这会有些繁琐和恼人。

So there's a second way which is kind of like the more industrial-strength way to implement back propagation which is to use a more modular API and this fits very much with this idea that we saw a local computation around nodes.

因此还有第二种方法，更像是工业级强度的反向传播实现方式，即使用更模块化的API，这与我们看到的围绕节点的局部计算理念非常契合。

Here with this modular implementation of backpropagation we will typically define some kind of computational graph object and this computational graph object will be able to do forward and backward passes throughout the entire graph by doing some topological sort operation on all of the nodes of the graph and then calling these little forward operations on each node during a forward pass and then calling the corresponding backwards operations on each node during the backward pass.

采用这种模块化反向传播实现时，我们通常会定义某种计算图对象，该对象能够通过对图中所有节点进行拓扑排序操作，在前向传播期间调用每个节点的小型前向操作，在反向传播期间调用每个节点的相应反向操作，从而在整个图中执行前向和反向传播。

Now this piece of code that I'm showing you here is just pseudocode like this is not actually real code it could have typos I don't know but this one this one actually is real code so in PyTorch you can actually define your own functions using this API by sub-classing torch.autograd.Function, now you are defining your own little computational node object which represents a node in a computation graph. Now you can see that this object defines two functions: forward and backward.

现在我在这里展示的这段代码只是伪代码，并非实际代码，可能包含拼写错误，但后面这个实际上是真实代码，在PyTorch中你确实可以使用这个API通过子类化 torch.autograd.Function 来定义自己的函数，你现在正在定义自己的小型计算节点对象，该对象代表计算图中的一个节点。现在你可以看到这个对象定义了两个函数：前向传播和反向传播。

Forward takes three inputs. The interesting ones are x and y, which correspond to the input values that this node will receive during the forward pass, and these will be specified as torch tensors. In this case we're just working with scalars, so this would be torch scalars. In this case we also received this context object that we can use to stash arbitrary bits of information that we want to remember for the backward pass.

前向传播接收三个输入。其中重要的是x和y，它们对应着在前向传播过程中该节点将接收的输入值，这些将被指定为torch张量。在这个例子中我们只处理标量，所以这里应该是torch标量。在这种情况下我们还接收这个上下文对象，我们可以用它来存储任意信息，这些信息我们希望记住以便在反向传播时使用。

You can see that in the forward pass we're simply defining this output Z equals X plus y and returning Z. These are all operations on torch tensors that you got familiar with in the first two assignments.

你可以看到在前向传播中，我们只是简单地定义输出Z等于X加y并返回Z。这些都是你在前两个作业中熟悉的torch张量操作。

Now in the backward pass we rewrite this function called backward that receives that same context object from the forward pass. So we can use that context object to pop off any stuff that we needed to remember from the forward pass in order to compute our derivatives.

现在在反向传播中，我们重写这个名为backward的函数，它从前向传播接收相同的上下文对象。这样我们就可以使用该上下文对象来取出我们需要从前向传播记住的任何信息，以便计算我们的导数。

In this case we need to remember x and y, and now we also receive grad Z which is this upstream gradient also stored in a torch tensor. Now internally we locally compute this product of the local gradient and the upstream gradient to compute our downstream gradients, which are the derivatives of the two inputs, and we simply return those.

在这种情况下我们需要记住x和y，现在我们还接收grad Z，这是上游梯度，同样存储在torch张量中。现在我们在内部本地计算局部梯度和上游梯度的乘积，以计算我们的下游梯度，也就是两个输入的导数，然后我们简单地返回这些梯度。

Now this is like real torch code that you could use to implement your own scalar sum of two scalars. So that's how actually our add is already implemented in torch, so I don't recommend you actually use this implementation.

现在这就像真实的torch代码，你可以用它来实现自己的两个标量的标量和。实际上这就是我们的加法操作在torch中的实现方式，所以我不建议你实际使用这个实现。

But if for some reason you did want to define your own arbitrary function in torch and define the forward and backward passes of your new operation, this is how you can actually do it inside pytorch. Now if you look into a like deep in the guts of the pytorch code base, basically what is pytorch is this autograd engine and a ton of these little functions that define paired forward and backward functions. So here I'm showing you is just like, there's a lot of files in here.

但如果出于某种原因，你确实想在torch中定义自己的任意函数，并定义新操作的前向传播和反向传播，这就是你可以在pytorch内部实际实现的方式。现在如果你深入查看pytorch代码库的内部，基本上pytorch就是这个自动梯度引擎和大量定义配对的前向传播和反向传播函数的小函数。这里我向你们展示的是，这里面有很多文件。

This is somewhere on the PyTorch GitHub repo, and if we zoom into one of these files, this is actually the implementation of one of many implementations of sigmoid.

这是在PyTorch GitHub仓库的某个位置，如果我们放大其中一个文件，这实际上是众多sigmoid函数实现中的一个具体实现。

It's actually deep inside the guts of PyTorch work somewhere, so you can see that here we are defining the forward pass of sigmoid using like deep C++ or C or something deep in the guts of PyTorch that's computing the forward pass of the sigmoid layer.

它实际上深藏在PyTorch的内部代码中，所以你可以看到这里我们正在定义sigmoid的前向传播，使用的是深层的C++或C语言代码，这些代码深埋在PyTorch内部，用于计算sigmoid层的前向传播。

Unfortunately it calls into this other function which is defined somewhere else, it's a bit of spaghetti code if you actually ever looked at the backend of PyTorch, but we can ignore that.

不幸的是，它调用了另一个在别处定义的函数，如果你真的看过PyTorch的后端代码，会发现这有点像意大利面条式代码，但我们可以忽略这一点。

Now there's the second paired function which is THNN sigmoid update grad input which computes the backward pass, and it does some boilerplate like unpack tensors and do some checking of input.

现在有第二个配对函数叫做THNN sigmoid update grad input，它计算反向传播，并执行一些样板代码，比如解包张量和检查输入。

This is a real industrial strength code base, but now there's this critical line where you can see that it actually is right here is where PyTorch is actually computing the backward pass of the sigmoid layer like deep inside C, like nested inside some macros and like some crazy stuff going on.

这是一个真正的工业级代码库，但现在有一个关键行，你可以看到这里正是PyTorch实际计算sigmoid层反向传播的地方，深藏在C语言代码中，嵌套在一些宏里面，看起来有些复杂。

But basically what is PyTorch is these paired forward and backward functions that can then be chained together into these big computational graphs.

但基本上PyTorch就是这些配对的前向和反向函数，它们可以被串联起来形成这些大的计算图。

So basically up to this point we've really only talked about the notion of using back propagation and computational graphs using scalars, which is really easy and intuitive if you kind of remember everything from single variable calculus.

所以基本上到目前为止，我们只讨论了使用标量的反向传播和计算图的概念，如果你还记得单变量微积分的所有内容，这确实很容易和直观。

But in practice we often want to work with vector valued functions or functions that operate on vectors or matrices or tensors of arbitrary dimension, so we also need to think about what does it mean to do back propagation in computational graphs with vector or tensor valued inputs as well.

但在实践中，我们经常需要处理向量值函数，或者操作于向量、矩阵或任意维度张量的函数，因此我们还需要思考在具有向量或张量值输入的计算图中进行反向传播意味着什么。

Well here we need to recap a little bit some of these different flavors of multivariable derivatives.

那么这里我们需要稍微回顾一下多元导数的不同类型。

Well, you remember the normal single variable derivative. Given a function that takes a single scalar input and produces a scalar output, the derivative of the output with respect to the input tells us this local linear approximation. If we change the input by a little bit, then how much does the output change?

我们回顾一下常规的单变量导数。给定一个接收单个标量输入并生成标量输出的函数，输出相对于输入的导数告诉我们这种局部线性近似关系。如果我们稍微改变输入，输出会变化多少？

We've got this familiar gradient operation. A gradient is the type of derivative that is appropriate when our function takes a vector as an input and produces a scalar as an output. The gradient dy/dx in this case is then a vector of the same size as the input, where each element of the gradient vector says how much does the output change if the corresponding element of the input changes by a little bit.

我们熟悉梯度运算。梯度是一种适用于函数接收向量输入并生成标量输出的导数类型。在这种情况下，dy/dx 梯度是一个与输入尺寸相同的向量，其中梯度向量的每个元素表示：如果输入的对应元素发生微小变化，输出会变化多少。

So it's a vector of these sort of classical single value derivatives. Now the generalization is that inputs a vector and outputs a vector, possibly of a different dimension. These things all have different names but they're all basically the same idea - they're all derivatives, and this one is called a Jacobian.

因此它是这类经典单值导数的向量。现在的推广是：输入一个向量并输出一个向量，可能具有不同维度。这些概念都有不同名称，但基本思想相同——它们都是导数，而这个被称为雅可比矩阵。

Now the Jacobian is a matrix where it has a number of elements which is n times M if those are the two dimensions of our input and our output. The idea of the Jacobian is that it says for each element of the input and for each element of the output, how much does changing one of the elements of the input affects that element of the output.

雅可比矩阵是一个具有 n×M 个元素的矩阵，其中 n 和 M 分别是输入和输出的维度。雅可比矩阵的概念是：对于输入的每个元素和输出的每个元素，它表示改变输入的某个元素会影响输出的那个元素的程度。

Because we've got M input elements and N output elements, we need M times N scalar values in order to represent all those possible effects of inputs on all the outputs. So then now suppose we've got the same picture that's kind of right.

因为我们有 M 个输入元素和 N 个输出元素，我们需要 M×N 个标量值来表示所有输入对所有输出的可能影响。那么现在假设我们有相同的正确图示。

We know that we don't really need to think about graphs as a whole. We only need to think about how does back propagation work for one node at a time. So then what does it mean to do back propagation in this vector-valued case, kind of zooming in on one node again?

我们知道实际上不需要将计算图视为整体来考虑。我们只需要考虑反向传播如何一次在一个节点上工作。那么在这种向量值情况下进行反向传播意味着什么，再次聚焦于单个节点？

Well here we've got now our little function f. It is inputting two vector values: X is now a vector of dimension DX, Y is now a vector with DY elements, and we're producing a vector output Z with DZ elements. This is our forward path - it's very easy, things are happening.

这里我们的小函数 f 接收两个向量值：X 现在是维度为 DX 的向量，Y 现在是具有 DY 个元素的向量，我们生成具有 DZ 个元素的向量输出 Z。这是我们的前向路径——非常简单，事情正在发生。

Eventually we receive this gradient map from upstream. Now this upstream gradient now in this vector valued case, it's important to remember that the loss we compute is always a scalar. No matter whether we're working with vectors or tensors or whatever, this final loss we compute at the end of the graph is always going to be a scalar.

最终我们从上游接收这个梯度图。现在这个上游梯度在向量值的情况下，重要的是要记住我们计算的损失始终是一个标量。无论我们处理的是向量、张量还是其他数据类型，我们在计算图末端计算的最终损失始终是一个标量。

And now this upstream gradient that we receive is going to be the gradient of that - the derivative of the loss with respect to our outputs. So that's going to tell us for each of the outputs from this node: if we were to change each of our outputs by a little bit, how much would they affect the loss way down to the right of the graph at the very end of our computation.

现在我们接收的上游梯度将是该损失的梯度——即损失相对于我们输出的导数。这将告诉我们对于该节点的每个输出：如果我们稍微改变每个输出，它们会对计算图最右端、我们计算最终结果的损失产生多大影响。

Now the local ingredients in this case become these Jacobian matrices. Because in this case now our function is a vector valued function that takes two vectors as input and produces one vector as output. So now our local derivatives become these Jacobian matrices that again tell us for each output element from this node how much is it affected by changing each input element of this node.

在这种情况下，局部成分变成了这些雅可比矩阵。因为现在我们的函数是一个向量值函数，它接受两个向量作为输入并产生一个向量作为输出。因此现在我们的局部导数变成了这些雅可比矩阵，它们再次告诉我们对于该节点的每个输出元素，改变该节点的每个输入元素会对它产生多大影响。

Now during the backward pass, the downstream gradients that we want are always going to be the derivative of the loss with respect to the inputs. And now the derivative of loss with respect to a vector input is again going to be a vector of the same size as the inputs.

在反向传播过程中，我们想要的下游梯度始终是损失相对于输入的导数。现在，损失相对于向量输入的导数将再次成为与输入大小相同的向量。

So the downstream gradient that we produce for X is going to be dL/dX which will be a vector of the same size of X. The downstream gradient we produce for Y will be dL/dY which is a vector of the same size as Y.

因此我们为X生成的下游梯度将是dL/dX，它将是一个与X大小相同的向量。我们为Y生成的下游梯度将是dL/dY，这是一个与Y大小相同的向量。

And now to actually produce these downstream gradients, we know we need to multiply the local and upstream gradients. But now that we're working with vectors, it's not a scalar multiplication anymore - this becomes a matrix vector product where the local gradient is now this Jacobian matrix and the upstream gradient is this gradient vector. The downstream gradient is a gradient vector. The way that we produced this downstream gradient is doing a matrix vector multiply between the upstream gradient vector and the local Jacobian matrix in such a way that the shapes work out. If you're ever confused about this, I always just recommend writing out the shapes of all the things and hopefully that will help you clarify what's going on.

现在要实际生成这些下游梯度，我们知道需要将局部梯度和上游梯度相乘。但由于我们现在处理的是向量，这不再是标量乘法——这变成了矩阵向量乘积，其中局部梯度现在是这个雅可比矩阵，而上游梯度是这个梯度向量。下游梯度是一个梯度向量。我们生成这个下游梯度的方法是通过上游梯度向量与局部雅可比矩阵进行矩阵向量乘法，使得维度能够匹配。如果你对此感到困惑，我总是建议写出所有内容的维度，希望这能帮助你理清当前的情况。

As a concrete example of doing back propagation with vectors, we can imagine what this looks like for the ReLU function. For the ReLU function, remember that it's an element-wise max of zero where we clip everything below zero. Given an example input vector X as [1, -2, 3, -1], applying the ReLU function to this vector would replace all the negative values by zero so our output Y would be the vector [1, 0, 3, 0].

作为使用向量进行反向传播的具体示例，我们可以想象ReLU函数的情况。对于ReLU函数，请记住它是一个逐元素的最大零操作，我们将所有低于零的值裁剪为零。给定一个示例输入向量X为[1, -2, 3, -1]，对该向量应用ReLU函数会将所有负值替换为零，因此我们的输出Y将是向量[1, 0, 3, 0]。

This ReLU function, this kind of vector valued ReLU function, is one little computational node embedded somewhere in our graph. Eventually we will be returned this upstream gradient which tells us how much the final loss changes if any of our little outputs from our ReLU function changed. These could be arbitrary values positive or negative, we don't know or care how they're computed, they're just handed to us by the automatic differentiation engine.

这个ReLU函数，这种向量值的ReLU函数，是嵌入在我们计算图中某处的一个小型计算节点。最终我们将收到这个上游梯度，它告诉我们如果ReLU函数的任何小输出发生变化，最终损失会改变多少。这些可能是任意的正值或负值，我们不知道也不关心它们是如何计算的，它们只是由自动微分引擎传递给我们的。

This Jacobian matrix tells us for each input of our local function how does each output of our local function change. What we can start to notice here is that the Jacobian matrix of this element-wise ReLU function has some special structure. Because this is an element-wise function, we know that the first output of the function depends only on the first input and the second output depends only on the second input.

这个雅可比矩阵告诉我们对于局部函数的每个输入，局部函数的每个输出如何变化。我们在这里可以开始注意到，这个逐元素ReLU函数的雅可比矩阵具有一些特殊结构。因为这是一个逐元素函数，我们知道函数的第一个输出仅取决于第一个输入，第二个输出仅取决于第二个输入。

In particular, the first input does not affect the second output or the third output or the fourth output. Each input only affects its corresponding output in the same position in the vector. What does that structure look like in a Jacobian matrix? That means that the Jacobian matrix is diagonal because the off-diagonal elements of the Jacobian tell us how element I of the input affects element J of the output where I and J are not equal.

具体来说，第一个输入不会影响第二个输出、第三个输出或第四个输出。每个输入仅影响向量中相同位置的对应输出。这种结构在雅可比矩阵中是什么样子？这意味着雅可比矩阵是对角矩阵，因为雅可比矩阵的非对角元素告诉我们输入的第I个元素如何影响输出的第J个元素，其中I和J不相等。

So for this element-wise function, all those off-diagonal elements of the Jacobian are zero. Now for the on-diagonal elements, that's the fact that becomes this scalar value derivative, which tells us how much does the ReLU change as a function of changing the input. So then for the positive value of inputs, they'll have a local gradient of 1 on the diagonal, and for the negative value for the negative inputs, they'll have a local gradient or the local derivative of 0 on the diagonal.

因此对于这个逐元素函数，雅可比矩阵的所有非对角元素都为零。而对于对角元素，这就是变成标量值导数的事实，它告诉我们ReLU随着输入改变而变化的程度。因此对于正值输入，它们在主对角线上具有局部梯度1；对于负值输入，它们在主对角线上具有局部梯度或局部导数0。

Unless UK and then this this by by working through this and lets us form this full Jacobian matrix for the input. And now remember to compute the downstream gradient, we need to compute this matrix vector multiply between the upstream gradient vector and the local Jacobian matrix.

除非英国，然后通过逐步推导这个过程，我们可以形成输入的完整雅可比矩阵。现在要记住计算下游梯度，我们需要计算上游梯度向量与局部雅可比矩阵之间的矩阵向量乘法。

So you can compute that thing offline and then the art then we can produce this downstream gradient vector that will be passed to the other nodes that were feeding into us as input. And now we start to realize something interesting is that this is actually in this case for this ReLU function we saw that the input that the Jacobian matrix was sparse, that the Jacobian matrix had a lot of zeroes in it.

因此你可以离线计算这个结果，然后通过技巧我们可以生成这个下游梯度向量，它将传递给作为我们输入的其他节点。现在我们开始意识到一个有趣的现象：对于这个ReLU函数，我们看到雅可比矩阵是稀疏的，雅可比矩阵中有很多零元素。

And this isn't this is actually the common case for most of the functions that we use in deep learning. In general most of the local Jacobian matrices that we use are going to be very very very sparse. So in practice we will almost never explicitly form which is this Jacobian matrix and we will almost never explicitly perform this matrix vector multiply between the Jacobian and the upstream gradient.

这实际上是我们深度学习中使用的多数函数的常见情况。一般来说，我们使用的大多数局部雅可比矩阵都将非常非常稀疏。因此在实践中，我们几乎从不显式构建这个雅可比矩阵，也几乎从不显式执行雅可比矩阵与上游梯度之间的矩阵向量乘法。

So you can imagine that for this really example it's may be fine to form the Jacobian for a for a vector of four inputs. But what if our inputs was like a mini batch of a thousand of 128 elements and each of all those elements was a vector of like 4096 dimensions.

所以你可以想象，对于这个简单示例，为四个输入的向量构建雅可比矩阵可能没问题。但如果我们的输入是一个包含1000个128元素的小批量，并且每个元素都是类似4096维的向量呢？

Now this Jacobian matrix would be like super super gigantic and super super sparse like only the diagonal would be nonzero. So in general actually explicitly forming those matrices would be super wasteful and explicitly performing that multiplication with a general matrix multiply function would be super inefficient.

现在这个雅可比矩阵将变得极其巨大且极其稀疏，只有主对角线会是非零的。因此一般来说，显式构建这些矩阵实际上会非常浪费，使用通用矩阵乘法函数显式执行该乘法会非常低效。

So really the big trick in back prop is figuring out a way to express these Jacobian vector multiplies in an efficient implicit way. And for the example of ReLU, this is trivial because we know that ReLU has this structure where the output where the diagonal entries are either 1 or 0. So this means that for the real output, we can compute this local gradient. We can compute this downstream gradient by either passing on the upstream gradient or killing the upstream gradient and clipping it to 0, depending on the sign of the corresponding value of the input.

因此反向传播的真正诀窍在于找到一种方法，以高效的隐式方式表达这些雅可比向量乘法。对于ReLU的例子，这很简单，因为我们知道ReLU具有这种结构，其中输出中对角线元素为1或0。这意味着对于实际输出，我们可以计算这个局部梯度。我们可以通过传递上游梯度或将上游梯度置零并裁剪为0来计算这个下游梯度，具体取决于输入对应值的符号。

The way that you should think about this expression is that this is a very efficient implementation of an implicit multiplication between this large sparse local Jacobian and this upstream gradient vector. Is this clear?

理解这个表达式的方式是：这是大型稀疏局部雅可比矩阵与上游梯度向量之间隐式乘法的高效实现。清楚了吗？

So then now this is talking about vectors, but of course we need to work with tensors of rank greater than 1. We need to work with matrices and 3-dimensional tensors and four dimensional tensors and like arbitrary things. Now the picture is very much the same.

虽然这里讨论的是向量，但当然我们需要处理秩大于1的张量。我们需要处理矩阵、三维张量、四维张量等任意维度的张量。现在的情况非常相似。

To understand how to work with back propagation with tensors of arbitrary dimensions, we have very much the same picture that now we have our local function like the identity function that inputs two values x and y. In this case they're going to be matrices. X will be a matrix of size DX by MX, Y will be a matrix of size DY by MY, and the output will also be a matrix.

要理解如何对任意维度张量进行反向传播，我们采用非常相似的思路：现在我们的局部函数（比如恒等函数）输入两个值x和y。在这种情况下它们将是矩阵。X是大小为DX乘MX的矩阵，Y是大小为DY乘MY的矩阵，输出也将是一个矩阵。

Now remember the loss is still a scalar, and all the gradients of the loss with respect to something will always be a tensor of the same shape as something, which will tell us how does the final downstream loss change as we vary each of the independent elements of that input of that tensor.

请记住损失函数仍然是标量，损失相对于某个参数的梯度始终是与该参数形状相同的张量，这将告诉我们当改变该张量输入中每个独立元素时，最终下游损失如何变化。

So then when we finally receive this upstream gradient, it will be a matrix now of size DZ by MZ, which will tell us again how if we change any of the elements of our output, how much will that loss change.

因此当我们最终接收到这个上游梯度时，它将是一个大小为DZ乘MZ的矩阵，这将再次告诉我们如果改变输出中的任何元素，损失会变化多少。

And now these local Jacobian matrices get very interesting in this sort of tensor case, because recall these Jacobian matrices need to be able to tell us for each scalar element of the input how much does each scalar element of the output change, which means that the Jacobian matrix is now a kind of like generalized matrix type of thing. So the number of elements of these local Jacobian matrices is maybe like DX times MX times DZ times NZ for those local Jacobian matrix between X and Z.

现在在这种张量情况下，这些局部雅可比矩阵变得非常有趣，因为回想一下这些雅可比矩阵需要能够告诉我们：对于输入的每个标量元素，输出的每个标量元素会变化多少，这意味着雅可比矩阵现在是一种广义的矩阵类型。这些局部雅可比矩阵的元素数量大约是DX乘以MX乘以DZ乘以NZ，针对X和Z之间的那些局部雅可比矩阵。

And I often think about grouping the dimensions of the Jacobian matrix where we have one group of dimensions corresponding to the shape of the input and one group of dimensions corresponding to the shape of the output.

我经常考虑将雅可比矩阵的维度分组，其中一组维度对应输入的形状，另一组维度对应输出的形状。

In that way we get this Jacobian matrix as this very high rank tensor that has the product of the sizes of the input and the output.

通过这种方式，我们得到这个雅可比矩阵作为一个非常高阶的张量，它具有输入和输出大小的乘积。

Now the downstream gradient proceeds in much the same way that the downstream gradients are then going to be again tensors of the same shape as the inputs giving us these downstream gradients.

现在下游梯度的处理方式大致相同，下游梯度将再次成为与输入相同形状的张量，从而得到这些下游梯度。

In order to compute them we still need to do a kind of matrix vector multiply between these local Jacobians and these upstream gradients.

为了计算它们，我们仍然需要在局部雅可比矩阵和上游梯度之间进行一种矩阵向量乘法。

The problem is that now we need to think of these as a kind of generalized form of a matrix product or a general right where now the local Jacobian matrix you can imagine kind of flattening these two groups of dimensions corresponding to the input and the output.

问题在于现在我们需要将这些视为矩阵乘积的一种广义形式，你可以想象将局部雅可比矩阵中对应输入和输出的这两组维度展平。

They would actually give us a literal matrix you could imagine flattening the output to be a high dimensional vector and flattening the input to be a high dimensional vector.

实际上它们会给我们一个真正的矩阵，你可以想象将输出展平为高维向量，将输入展平为高维向量。

Then that kind of results that then you can after flattening actually literally perform a matrix vector multiply and get these demonstrating gradients.

然后这种结果使得在展平后你实际上可以执行矩阵向量乘法并获得这些演示梯度。

But whenever I think about these like higher dimensional implicit Jacobian matrix vector multiplies between like super high dimensional tensors like my brain ends up exploding and it becomes very difficult to actually think about how to write down an expression for implicitly computing this like giant sparse matrix vector multiply thing like it's a mess.

但每当我思考这些高维隐式雅可比矩阵向量乘法，在超高维张量之间进行时，我的大脑就会爆炸，实际上很难思考如何写出隐式计算这个巨大稀疏矩阵向量乘法的表达式，这简直是一团糟。

So to help you out I'm going to work through a strategy that you can use to help you implement these types of operations without actually thinking about really high dimensional high rank tensors.

所以为了帮助你，我将详细讲解一个策略，你可以使用这个策略来实现这些类型的操作，而无需真正考虑真正高维高阶的张量。

So for that, we're going to work through a concrete example of deriving the backpropagation expression for the case of matrix multiplication. This is going to be a super pedagogical but general strategy that you can apply for deriving these back propagation operations for arbitrary functions of tensors with complex shapes.

为此，我们将通过一个具体示例来推导矩阵乘法情况下的反向传播表达式。这将是一个极具教学意义且通用的策略，你可以将其应用于推导任意张量函数（即使形状复杂）的反向传播操作。

Here we're doing a matrix vector multiplication between an input X which is a matrix of size n by D, and I've written out a concrete example of a matrix of size 2 by 3. We have a weight matrix of size D by M, and again we have a three by four concrete example. Now our little computational node is going to do matrix multiplication and produce this output Y.

这里我们进行输入X（一个n×D大小的矩阵）与权重矩阵（D×M大小）之间的矩阵向量乘法。我写出了一个2×3矩阵的具体示例，以及一个3×4的权重矩阵示例。现在我们的计算节点将执行矩阵乘法并产生输出Y。

During the backward pass, we're going to receive this upstream gradient from somewhere out in the graph. It's going to tell us how much each element of Y affects that final loss L, and again these can be arbitrary values. Our goal is to compute this downstream gradient, which is how much each of our inputs affects the loss.

在反向传播过程中，我们将从计算图中的某处接收这个上游梯度。它会告诉我们Y的每个元素对最终损失L的影响程度，这些值可以是任意的。我们的目标是计算这个下游梯度，即我们的每个输入对损失的影响程度。

Now if you imagine what are the sizes of these Jacobians, they're going to be pretty big. For a real neural network we might have like n is 64 and D might be 4096. If you multiply those out, each of those Jacobians is going to be like 256 gigabytes of memory in fp32, and the biggest GPU you can buy today has like 48 gigabytes of memory.

现在想象一下这些雅可比矩阵的大小，它们会非常庞大。对于一个真实的神经网络，我们可能有n=64，D=4096。如果计算这些乘积，每个雅可比矩阵在fp32精度下将占用约256GB内存，而目前市场上最大的GPU只有约48GB内存。

So basically the whole trick here is to find a way to express this computation without explicitly forming the Jacobian and without explicitly doing that matrix vector multiply. You need to find a way to do that implicitly, and the way that you can think about doing this is kind of element wise on the input. So what you can think about doing is think about one element of the input, think about x11. Now we want to think about how what is gonna happen just thinking about x11, and now we can compute a slice of the local gradient.

因此，基本技巧在于找到一种方法来表示这个计算，而无需显式构建雅可比矩阵，也无需显式执行矩阵向量乘法。你需要找到一种隐式实现的方法，可以考虑按输入元素逐个处理的方式来实现。你可以这样思考：考虑输入中的一个元素，比如x11。现在我们要思考仅针对x11会发生什么，这样我们就能计算局部梯度的切片。

This local gradient was this like really big thing of the shape of the input times the full shape of the output. But this slice of the local gradient, what I mean by that is the derivative of the output matrix Y with respect to the single scalar input element x11.

这个局部梯度原本是输入形状乘以完整输出形状的巨大张量。但这里说的局部梯度切片，我指的是输出矩阵Y对单个标量输入元素x11的导数。

Now because this is the derivative of a matrix by a scalar, it's going to be an object of the same shape as that matrix, telling us how much does each element of the output Y get affected by that one scalar element of the input X.

由于这是矩阵对标量的导数，它将是一个与矩阵同形状的对象，告诉我们输出Y的每个元素受输入X中那个标量元素影响的程度。

Now to make matters further, then we can think how what is this first element of this local gradient slice. This first element of this local gradient slice tells us how much is y11 affected by X11.

进一步来说，我们可以思考这个局部梯度切片的第一个元素是什么。这个局部梯度切片的第一个元素告诉我们y11受X11影响的程度。

Well we know that matrix multiplication in order to compute that value y11, what we computed would join nature's multiplication was an inner product between the first row of X in the first column of W.

我们知道在矩阵乘法中，为了计算y11的值，我们计算的是X的第一行与W的第一列的内积。

So then we can write out this expression on the bottom here for how we actually computed y11, and then we can imagine computing the derivative of y11 with respect to X11.

因此我们可以在底部写出实际计算y11的表达式，然后可以想象计算y11对X11的导数。

We can see that all of these other terms fall away and the only part that matters is this first term of X11 times W11, and we know how to take derivatives of product of two scalars.

我们可以看到所有其他项都消失了，唯一重要的是X11乘以W11这一项，而且我们知道如何计算两个标量乘积的导数。

So we know that that local gradient, that piece of the slice of the local gradient is now just equal to W11, so in this case is going to be 3.

所以我们知道那个局部梯度，即局部梯度切片的这一部分现在就等于W11，在这个例子中就是3。

Now we can repeat this process for the second element of this local gradient slice, and now the story is very much the same.

现在我们可以对局部梯度切片的第二个元素重复这个过程，情况非常相似。

This second piece of the local gradient slice says how much does this blue element of the input affect this purple element in the output, and again, this purple elements in the output was an inner product in the first row of X and the first column of Y.

局部梯度切片的第二部分说明了输入的蓝色元素对输出的紫色元素影响程度，再次强调，输出中的紫色元素是X的第一行和Y的第一列的内积。

So then again it looks like this inner product, and again all but one term vanishes. And it sees then we see that the gradient we just pick out this value of W 1 1, so the second element of the slice is 2, so it's just going to copy all one of those elements of the weight matrix.

所以再次看起来像是这个内积，并且再次除了一个项外其他项都消失了。然后我们看到梯度，我们只选取W 1 1的值，因此切片的第二个元素是2，所以它只是复制权重矩阵中的那些元素之一。

I'm not gonna bore you with the next two, but basically at this point you should see the pattern that this first row of the local gradient slice is just copying over this first row of the weight matrix W.

接下来的两个我就不赘述了，但基本上在这一点上你应该看到模式，即局部梯度切片的第一行只是复制权重矩阵W的第一行。

Now what about the second row of the local gradient slice? Well, the second row of the local gradient slice is going to ask the question: how much does this blue element of the input affect the purple element of the output?

那么局部梯度切片的第二行呢？局部梯度切片的第二行将要问这个问题：输入的蓝色元素对输出的紫色元素有多大影响？

Well remember now, the purple element of the output is computed by an inner product between the second row of X and first column of Y. But what you'll notice is that it doesn't involve that x11 term at all, so here the local gradient is zero for this little chunk of the local gradient slice.

现在请记住，输出的紫色元素是通过X的第二行和Y的第一列的内积计算的。但你会注意到它根本不涉及x11项，因此在这里，对于这一小块局部梯度切片，局部梯度为零。

And now you can expect that this pattern will repeat for all of the other elements in the second row of the local gradient slice.

现在你可以预期，这种模式将在局部梯度切片第二行的所有其他元素中重复出现。

So then now through that like excessively verbose explanation, we've finally computed this local gradient slice. And now we're ready to compute the downstream gradient.

因此，通过那个过于冗长的解释，我们终于计算出了这个局部梯度切片。现在我们已经准备好计算下游梯度。

So we can finally now compute this blue element of the downstream gradient by computing an inner product between this local gradient slice and the full upstream gradient.

所以我们最终可以通过计算这个局部梯度切片和完整上游梯度之间的内积，来计算下游梯度的这个蓝色元素。

And now this will tell us how much does this one element of the input affect the final loss at the very end of the thing.

现在这将告诉我们输入的这一个元素对最终损失的影响有多大。

So then you can see that this local gradient slice now ends up being this inner product between the local gradient slice and the upstream gradient. But because the local gradient slice was copying one row of the weight matrix and the rest of it was zeros, what this means is that this element of the input of the downstream gradient is really an inner product between the first row of the weight matrix W and the first row of the upstream gradient DL dy.

因此你可以看到，这个局部梯度切片最终变成了局部梯度切片和上游梯度之间的这个内积。但由于局部梯度切片复制了权重矩阵的一行，其余部分为零，这意味着下游梯度输入的这个元素实际上是权重矩阵W的第一行与上游梯度DL dy的第一行之间的内积。

So now at this point we can kind of like throw away this local gradient slice and forget about it, and we could realize that we only needed to look at that one row of weight matrix on the one row of upstream gradient to compute the downstream grades.

因此现在我们可以丢弃这个局部梯度切片并忘记它，我们意识到只需要查看权重矩阵的某一行和上游梯度的某一行就能计算下游梯度。

And now we could imagine doing the same thing for another element of the input, and we could go through this whole same song and dance to compute this local gradient slice but kind of reasoning one element at a time.

现在我们可以想象对输入的另一个元素进行同样的操作，通过同样的步骤计算这个局部梯度切片，但每次只推理一个元素。

Now you can see that if we were to pick the local gradient slice for x_3, that is this like bottom right element of the input X, now our local gradient slice has the same kind of structure that this local gradient slice is now copying one of the rows from the weight matrix and it is zero everywhere else.

现在可以看到，如果我们选择x_3的局部梯度切片，即输入X的右下角元素，我们的局部梯度切片具有相同的结构——它复制了权重矩阵的某一行，其余位置都为零。

So then when we take this inner product, we see that this other element of the downstream gradient is again an inner product in one of the rows of the weight matrix and one of the rows with the upstream gradient.

因此当我们进行这个内积运算时，会发现下游梯度的另一个元素同样是权重矩阵某一行与上游梯度某一行之间的内积。

And now you could kind of work through this with some complicated indexing expressions on paper, but it ends up that you kind of get this general expression that now you can kind of jump and see that for any individual element of the downstream gradient, it ends up being an inner product between one of the rows of the weight matrix and one of the rows of the upstream gradient.

虽然可以通过复杂的索引表达式在纸上推导，但最终会得到一个通用表达式：对于下游梯度的任何单个元素，它都是权重矩阵某一行与上游梯度某一行之间的内积。

Once you realize this relationship, you don't actually have to form that upstream gradient that local gradient slice at all, and we can compute all of these inner products between the rows of the weight matrix and the rows of the upstream gradient compute them all at once using the single matrix product between DL dy the upstream gradient and W transpose the transpose. People get confused about this sometimes and people look at this expression and think that we are somehow forming the Jacobian here.

一旦认识到这个关系，就完全不需要构建那个上游梯度的局部梯度切片，我们可以通过上游梯度DL dy与权重矩阵转置W^T的单个矩阵乘积，一次性计算出所有权重矩阵行与上游梯度行之间的所有内积。人们有时会对这一点感到困惑，看到这个表达式会认为我们在这里以某种方式构建雅可比矩阵。

We are not forming the Jacobian here. What this expression is doing is that by taking this matrix product between the option gradient and the weight matrix, this is actually an implicit matrix vector multiplication between this very large high dimensional sparse Jacobian and the upstream gradient.

我们并没有在这里构建雅可比矩阵。这个表达式的作用是通过计算选项梯度与权重矩阵之间的矩阵乘积，实际上是在执行这个非常高维稀疏雅可比矩阵与上游梯度之间的隐式矩阵向量乘法。

Even though it looks like a matrix product, this is actually not that. This is not explicitly the Jacobian ties depth ingredient. This is somehow an exhibition way to compute on that sparse sparse product.

尽管它看起来像矩阵乘积，但实际上并非如此。这不是显式的雅可比矩阵关联深度成分。这某种程度上是一种计算该稀疏乘积的展示方法。

And by the way, a really easy mnemonic to remember this expression is that it's the only way the shapes can work out. So you know that when you compute a product of two things, then the derivative should involve the upstream gradient and because product is a gradient swapper, it should involve the other input value.

顺便说一下，记住这个表达式的一个非常简单的方法是：这是唯一能使形状匹配的方式。当计算两个事物的乘积时，导数应包含上游梯度，并且由于乘积是梯度交换器，它还应包含另一个输入值。

So then in order to compute the downstream gradient for X, we know it has to involve the upstream gradient and we know it has to involve W. And then like there's only one way to multiply them that results in shapes of the same shape as X.

因此，为了计算X的下游梯度，我们知道它必须包含上游梯度，也必须包含W。然后只有一种乘法方式能得到与X相同形状的结果。

So that's the protip is like yeah this is a actual trick to remember matrix multiplication just match up the shapes. And it turns out that the exact same heuristic also works for the other input right.

所以这个专业技巧就是：这实际上是记住矩阵乘法的诀窍——只需匹配形状。事实证明，完全相同的启发式方法也适用于另一个输入。

So if we want to compute now the ldw again, we know it has to involve you upstream gradient has to involve the other input and there's only one way to match up this effect this product in a way that makes the shapes work out.

因此，如果我们现在要再次计算ldw，我们知道它必须包含上游梯度，必须包含另一个输入，并且只有一种方式能匹配这个乘积效应，使形状匹配成功。

So this is a super easy way to remember how to compute these things. So another view of back propagation is that we have this long chain of functions we've got like F 1 and F 2 and F 3 and F 4 and we eventually produced this scalar loss L.

所以这是记住如何计算这些内容的超简单方法。反向传播的另一个视角是：我们有一个很长的函数链，比如F1、F2、F3和F4，最终我们产生了这个标量损失L。

And now by the multivariate chain rule, we know we can expand out this gradient expression and write DL DX 0 as this product of all of these Jacobian matrices which are the intermediate Jacobian matrices and this final gradient matrix gradient vector on the very far right and we also know that matrix vector products are all associative.

现在根据多元链式法则，我们知道可以展开这个梯度表达式，将DL/DX0写成所有这些雅可比矩阵（即中间雅可比矩阵）与这个最右侧的最终梯度矩阵梯度向量的乘积，而且我们也知道矩阵向量乘积都是可结合的。

So we in principle could choose to perform this multiplication of all these Jacobian matrices and this final vector in any grouping that makes sense.

因此原则上我们可以选择以任何有意义的分组方式来执行这些雅可比矩阵和最终向量的乘法运算。

And what's happening in back propagation is that we've chosen the particular grouping of computing these products right to left.

而在反向传播中，我们选择了从右到左计算这些乘积的特定分组方式。

What's really nice about computing these products right to left is that we never have to do any matrix matrix multiplication because if we compute these products right to left then we only ever end up having to do matrix vector multiplication which is much more efficient.

从右到左计算这些乘积的好处在于，我们永远不需要进行任何矩阵与矩阵的乘法，因为如果从右到左计算这些乘积，我们最终只需要进行矩阵向量乘法，这要高效得多。

But this whole thing hinges on right this is a very nice algorithm but for that to work out it means that we always need to be computing a final scalar loss at the very end and this algorithm only works for computing the derivatives of that final scalar loss with respect to everything else in the graph.

但这一切都基于一个前提：虽然这是个很好的算法，但要让其奏效，意味着我们始终需要在最后计算一个最终标量损失，而且该算法仅适用于计算该最终标量损失相对于图中所有其他元素的导数。

And there might be other situations where you might want to do something else what if for instance you want to compute the derivative of a scalar input and get the derivatives of everything in the graph with respect to a single scalar input.

可能还存在其他情况需要不同的处理方式，例如想要计算标量输入的导数，并获取图中所有元素相对于单个标量输入的导数。

Now that might that corresponds to a different version oh and by the way this back propagation algorithm because of this method of multiplying the Jacobian matrices in this right-to-left way um this is sometimes referred to as reverse mode automatic differentiation.

这种情况可能对应着不同的版本。顺便说一下，由于这种从右到左相乘雅可比矩阵的方法，这个反向传播算法有时被称为反向模式自动微分。

So fancy as this is saying fancy sounding name and from the name you know there's explicitly calling it out reverse mode and there should be forward mode as well and it turns out there is.

虽然这个名字听起来很花哨，但从名称中你可以明确看出这是反向模式，应该还有正向模式，而事实上确实存在。

So the forward mode automatic differentiation is for this slightly off other case where we want we have a scalar input value and now we want to compute the derivative of that scalar input value with respect to everything else in a graph.

正向模式自动微分适用于这种略有不同的情况：当我们有一个标量输入值，想要计算该标量输入值相对于图中所有其他元素的导数。

And then you can see that if we kind of work it will think about it in this view point of vectors and jacobians then again we can multiply these things in any way but if we perform the multiplication left to right then we again get kind of only major expected multipliers. And by the way, you might ask why I would want to do this.

然后你会发现，如果我们从向量和雅可比矩阵的角度来思考，我们同样可以用任何方式相乘这些元素，但如果我们从左到右执行乘法，那么我们最终又只会得到主要预期乘数。顺便提一下，你可能会问为什么要这样做。

In machine learning, we always want to compute the derivatives. We always have a loss at the end, and we want to compute derivatives with respect to that loss in order to do gradient descent.

在机器学习中，我们总是需要计算导数。我们最终总会有损失函数，需要计算相对于该损失的导数来进行梯度下降。

Well, I know it's hard to believe, but there's actually more to the world than machine learning. Sometimes it's useful to have computer systems that can automatically compute gradients for us when we're doing things that are not minimizing a loss function.

虽然这很难相信，但世界不仅仅只有机器学习。有时在进行非最小化损失函数的任务时，拥有能自动计算梯度的计算机系统也很有用。

An example here might be we have some physical simulation. Input A is maybe like a scalar parameter giving the gravity or the friction or something of that physical simulation.

例如我们可能有某个物理模拟。输入A可能是一个标量参数，用于设定重力、摩擦力或该物理模拟的其他参数。

Now we want to compute how much would all of the outputs of the simulation change had one of those scalar inputs changed. So controlling gravity or friction or something changed.

现在我们想要计算，如果某个标量输入发生变化，模拟的所有输出会发生多大变化。比如控制重力或摩擦力等参数发生变化时的影响。

Because this kind of idea of automatic differentiation is generally useful far beyond machine learning. It's really useful anytime you want to compute derivatives for any kind of scientific computing application.

因为自动微分这种思想的应用范围远不止机器学习。在任何需要为科学计算应用计算导数的情况下都非常有用。

But the downside is that forward mode differentiation is not implemented by PyTorch and TensorFlow and all the other big frameworks. So unfortunately even though it has these really cool applications in scientific computing and whatnot, it's not super easy to use because they don't implement forward mode automatic differentiation.

但缺点是前向模式微分并未被PyTorch、TensorFlow等主流框架实现。因此尽管它在科学计算等领域有很酷的应用，但由于这些框架未实现前向模式自动微分，使用起来并不方便。

They've had issues about this open up on GitHub. You can look it up like they released but it still hasn't been merged in.

在GitHub上有相关的开放问题。你可以查看这些已发布但尚未合并的议题。

But thankfully there's a clever algebraic trick you can actually do to compute forward mode gradients using two back propagation operations. There's a link to that here - it's a super clever piece of algebra that I'd really encourage you to check out if you ever happen to find yourself wanting to compute forward mode gradients in a deep learning framework.

但幸运的是，有一个巧妙的代数技巧，实际上可以通过两次反向传播操作来计算前向模式梯度。这里有一个链接 - 这是一个非常聪明的代数方法，如果你需要在深度学习框架中计算前向模式梯度，我强烈建议你了解一下。

Now another kind of really useful trick that we can use once we have this viewpoint of back propagation is kind of multiplying vectors and tensors and Jacobians and vectors and what not. Is we can actually use this same back propagation algorithm to compute not only gradients but also higher-order derivatives as well. So as an example here, we're showing a very amended computational graph where we have an input X of a vector of size d1 that goes through f1 to produce another intermediate vector of size d0 d1.

现在我们有了反向传播的这种视角后，另一个非常有用的技巧是处理向量、张量、雅可比矩阵和向量等的乘法运算。实际上我们可以使用相同的反向传播算法，不仅计算梯度，还能计算高阶导数。举个例子，这里我们展示了一个经过修改的计算图，其中我们有一个大小为d1的向量输入X，它通过f1处理后生成另一个大小为d0 d1的中间向量。

Then we go for f2 to produce a scalar loss, and now what if so far we've always talked about first derivatives? We always talked about gradients and Jacobians and normal derivatives, but in this case the second derivative of the loss with respect to the input x0 is now a matrix that tells us all of these second derivatives.

然后我们通过f2得到标量损失。到目前为止我们一直讨论的都是一阶导数——梯度、雅可比矩阵和普通导数，但这里损失函数对输入x0的二阶导数现在是一个矩阵，它能告诉我们所有这些二阶导数的信息。

Well, that's if we were to kind of like if we were to change x0 by a little bit like change one element of x1 by a little bit and change another element of x1 by a little bit simultaneously, and kind of how much is the loss changing, or equivalently if we were to change one of the elements of x1 then how fast would the gradient change is another way to think about this Jacobian matrix or sorry this Hessian matrix.

也就是说，如果我们稍微改变x0——比如同时稍微改变x1的一个元素和另一个元素——损失函数会如何变化；或者等价地说，如果我们改变x1的某个元素，梯度变化的速度有多快，这是理解这个雅可比矩阵（抱歉，应该是海森矩阵）的另一种方式。

See these things are easy to get mixed up very carefully: Hessian matrix the second derivative, Jacobian matrix is first derivative, Jacobian matrix is vector and vector out, Hessian matrix is vector in scalar out simple.

这些概念很容易混淆，需要非常小心：海森矩阵是二阶导数，雅可比矩阵是一阶导数，雅可比矩阵是向量到向量的映射，海森矩阵是向量到标量的简单映射。

But it turns out sometimes you might want to compute elements in your computational graph that are a function of this Hessian matrix.

但事实证明，有时你可能需要计算计算图中作为这个海森矩阵函数的元素。

So as an example, it would be a Hessian vector multiplied say we will have this Hessian matrix that we want to compute the Hessian is a matrix, we have a vector and we want to compute this Hessian vector product.

举个例子，比如海森向量乘积——假设我们有一个想要计算的海森矩阵（海森是一个矩阵），我们有一个向量，我们想要计算这个海森向量乘积。

Why would you ever want to do this? It turns out there's reasons, for instance there's an iterative algorithm to approximate the singular values of a matrix using kind of these nature expected products.

为什么要这样做呢？事实证明这是有原因的，例如存在一种迭代算法，可以利用这类自然期望乘积来近似矩阵的奇异值。

So for example, you could of what if you wanted to compute the some kind of second order information about the singular values of the optimization landscape then you might want to compute Hessian vector products to approximate those singular values.

比如说，如果你想要计算关于优化景观奇异值的某种二阶信息，那么你可能需要计算海森向量乘积来近似这些奇异值。

And it turns out through a bit of clever algebraic transform, you know derivatives are linear and gradients are linear. Linear functions are amazing so we can actually rewrite this second derivative of this nature as the derivative of the inner product between the gradient and the vector.

通过一些巧妙的代数变换，我们知道导数是线性的，梯度也是线性的。线性函数非常神奇，因此我们实际上可以将这种类型的二阶导数重写为梯度与向量之间内积的导数。

I'm pretty sure this works out right but of course this is only true if the vector is a constant and doesn't depend on X 0. If it does, you get another cross term but now we can do something very clever in our computational graph.

我确信这是正确的，但当然这只有在向量是常数且不依赖于X 0时才成立。如果它依赖于X 0，你会得到另一个交叉项，但现在我们可以在计算图中做一些非常巧妙的操作。

So we can extend our computational graph with these back propagation functions. Now what we can think about is extending our computational graph so after we compute the loss, then we use this function f2 prime to compute the gradient of the loss with respect to X 1.

因此我们可以用这些反向传播函数扩展我们的计算图。现在我们可以考虑扩展计算图，在计算损失之后，我们使用这个函数f2 prime来计算损失相对于X 1的梯度。

We can use f1 prime to compute the gradient of a loss with respect to x0. These are these little backward functions that were implemented by the backward pass of the little F gates and then we can implement this dot product with F and think about that as another node in the computational graph.

我们可以使用f1 prime来计算损失相对于x0的梯度。这些是由小F门的反向传播实现的小型反向函数，然后我们可以实现与F的点积，并将其视为计算图中的另一个节点。

Then this final output is going to be the inner product between the gradient of x0 and the vector, and this vector we've chosen V. Now in order to compute the derivative of that thing with respect to x0, we just need to back propagate through this graph.

然后这个最终输出将是x0的梯度与向量之间的内积，这个向量我们选择了V。现在为了计算该事物相对于x0的导数，我们只需要通过这个图进行反向传播。

So what this immediately means is that if all of your backward paths operations when you implement your little gradient nodes, if the backward pass is itself implemented using differentiable primitive operations, then you get all these higher-order gradients for free.

这意味着如果你在实现小梯度节点时，所有反向路径操作都是使用可微分基本操作实现的，那么你就可以免费获得所有这些高阶梯度。

And you can use back propagation through these computational graphs to compute functions of second derivatives. By the way, you can similarly do higher order derivative things as well. You can imagine computing a third derivative is now going to be a three-dimensional tensor.

你可以通过这些计算图使用反向传播来计算二阶导数的函数。顺便说一下，你同样可以做更高阶的导数计算。你可以想象计算三阶导数现在将是一个三维张量。

And you can compute bilinear form on top of the third derivative which is like kind of hard to think about but you could imagine extending this type of operation in order to compute derivatives of arbitrarily high values using the same simple back propagation algorithm.

你可以在三阶导数之上计算双线性形式，这有点难以想象，但你可以想象扩展这种类型的操作，以便使用相同的简单反向传播算法计算任意高阶的导数。

And unlike forward mode automatic differentiation, this actually is implemented in all of the major deep learning frameworks like tensor flow and pipe arch so you can do some crazy shenanigans and write down loss functions that involve gradients.

与正向模式自动微分不同，这实际上已经在所有主要的深度学习框架中实现，比如TensorFlow和PyTorch，因此你可以进行一些疯狂的操作，编写包含梯度的损失函数。

Why would you ever want to do that? It turns out people actually do want to do that sometimes. An example here from this paper called improved training of Wasserstein GANs: they actually write a regularization term that depends on the gradient of the loss with respect to the weight matrix.

为什么会有人想这样做呢？事实证明，人们有时确实需要这样做。这里有一个来自《改进Wasserstein GANs训练》论文的例子：他们实际上编写了一个依赖于损失函数相对于权重矩阵梯度的正则化项。

So here that has the interpretation that we want to write down a regularization term that can minimize the magnitude of the gradient. This means that we kind of want to find weight matrices that result in well-conditioned optimization landscapes, which is a pretty cool idea.

这里的解释是，我们想要编写一个能够最小化梯度幅度的正则化项。这意味着我们想要找到能够产生良好条件优化景观的权重矩阵，这是一个相当酷的想法。

Then you can actually implement this kind of crazy regularizer by using this idea of higher order differentiation through these computational graphs.

然后你实际上可以通过使用这些计算图的高阶微分概念来实现这种疯狂的正则化器。

So then kind of the summary of what we saw today is that we could represent these very complex functions using this computational graph abstraction. That is hopefully going to be a lot nicer than working out things on paper.

那么今天内容的总结是，我们可以使用这种计算图抽象来表示这些非常复杂的函数。这有望比在纸上推导要方便得多。

Where then it's going to have this forward pass that computes values, backward pass that computes gradients. And then you don't even really need to think about the full graph most of the time.

然后它会有一个计算值的前向传播，计算梯度的反向传播。大多数时候你甚至不需要考虑整个图。

You only need to kind of zoom in and think about this local picture of little computational graph nodes that compute outputs and then multiply the local gradients to compute downstream gradients.

你只需要放大并考虑这些小型计算图节点的局部视图，这些节点计算输出，然后乘以局部梯度来计算下游梯度。

And then hopefully the really important part for your homework that's due in a week will be this idea of implementing back propagation using this kind of flat back pathway where then your back prop code looks like an inverted version of your forward pass.

希望对你一周后要交的作业来说，真正重要的部分将是使用这种扁平反向路径实现反向传播的概念，这样你的反向传播代码看起来就像是前向传播的倒置版本。

And then we also talked about this more modularized API which will let you be more modular and swap things out in a better way. Actually on assignment 3 we'll implement a more modular API for other types of neural networks.

然后我们还讨论了这个更加模块化的API，它将让你更加模块化，并以更好的方式交换组件。实际上在作业3中，我们将为其他类型的神经网络实现一个更加模块化的API。

So now kind of at this point in the class we've seen linear classifiers, we've seen neural networks, we've seen how to compute gradients and these things. But we've had a big problem which is that both of these networks had this operation where we kind of stretch out the pixels of the input image and take our input image and stretch it out into a vector.

那么现在在课程的这个阶段，我们已经学习了线性分类器，我们已经学习了神经网络，我们已经学习了如何计算梯度等内容。但我们遇到了一个大问题，即这两种网络都存在这样的操作：我们将输入图像的像素展开，把输入图像拉伸成一个向量。

Which basically destroys the spatial information of the image that seems like a bad thing and we'll fix that in next lecture.

这基本上破坏了图像的空间信息，这看起来是个糟糕的问题，我们将在下节课中解决这个问题。

We talked about convolutional neural networks.

我们讨论了卷积神经网络。