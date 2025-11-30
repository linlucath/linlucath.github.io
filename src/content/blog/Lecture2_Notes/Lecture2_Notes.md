---
title: 'Lecture2_Notes'
publishDate: 2025-11-23
description: 'TODO'
tags:
  - TODO
language: 'English'
heroImage: { src: './default.jpg', color: '#D58388' }
---

So welcome back to CS4800 7/5 9800. Welcome back, this is now lecture 2. So remember last time we talked about a historical overview of the fields of computer vision and of deep learning and machine learning. And now starting this lecture, we're going to talk about image classification and we're gonna start diving into the technical material of the course. And today we'll see our first learning algorithm. So today we're going to talk about image classification. So image classification is really a really important core task in computer vision and really machine learning more broadly.

欢迎回到CS4800 7/5 9800课程。欢迎回来，现在是第二讲。记得上次我们讨论了计算机视觉、深度学习和机器学习领域的历史概述。从本讲开始，我们将讨论图像分类，并开始深入探讨课程的技术内容。今天我们将看到我们的第一个学习算法。所以今天我们将讨论图像分类。图像分类确实是计算机视觉乃至更广泛的机器学习中非常重要的核心任务。

So image classification is really quite a simple task to state. What we do is our algorithm is going to take as input an image on the left and then the output is the algorithm needs to assign a category label to that input image. When we talk about image classification, we typically have some fixed set of category labels in mind but the algorithm is aware of. In this example, maybe the algorithm is aware of these five labels: cat, bird, deer, dog and truck.

图像分类实际上是一个表述起来相当简单的任务。我们的做法是让算法以左侧的图像作为输入，然后输出需要为该输入图像分配一个类别标签。当我们讨论图像分类时，我们通常考虑一些固定的类别标签集合，但算法需要知道这些标签。在这个例子中，算法可能知道这五个标签：猫、鸟、鹿、狗和卡车。

And during the algorithm performs image classification, what it needs to do is simply assign one of these five labels to the image that it sees. In this case, it is cat. So for all of us, this is a really trivial task right? You can do this almost without thinking about it. You just immediately know that this is a cat when you look at the image. But for the computer, that's not so easy. The main challenge in image recognition and image classification when we try to do it on machines is this problem we call the semantic gap.

在算法执行图像分类时，它需要做的只是简单地为所看到的图像分配这五个标签中的一个。在这个例子中，它是猫。对我们所有人来说，这确实是一个非常简单的任务，对吧？你几乎可以不假思索地完成这个任务。当你看到图像时，你立刻就知道这是一只猫。但对计算机来说，这并不那么容易。当我们在机器上尝试进行图像识别和图像分类时，主要挑战就是我们称之为语义鸿沟的问题。

So for us when we look at this image we immediately recognize that as a cat. We get these perceptions of all these photons run and they hit our retina. They go through our brain and they go through a lot of complex processing but we're not really aware of that consciously when we look at these images. Instead we just kind of intuitively know what we see but the computer doesn't have that kind of intuition. So when the computer looks at such an image what all it gets is a giant grid of numbers.

对我们来说，当我们看到这张图像时，我们立即认出那是一只猫。我们接收到所有这些光子运行的感知，它们击中我们的视网膜。它们经过我们的大脑，经过大量复杂的处理，但当我们看这些图像时，我们并没有真正有意识地意识到这一点。相反，我们只是凭直觉知道我们看到的是什么，但计算机没有那种直觉。所以当计算机看这样的图像时，它得到的只是一个巨大的数字网格。

So first for an image like this, it's just a giant grid of 800 by 600 by 3 numbers where at each pixel we have a single color value represented with three numbers between 0 and 255. So the problem is that if you look at this grid of numbers, it's really not obvious at all that this grid of numbers should represent a cat, and there's no obvious way to convert this grid of raw pixel values into this semantically meaningful category label of cat.

首先对于这样的图像，它只是一个800×600×3的巨大数字网格，其中每个像素都有一个由0到255之间的三个数字表示的单一颜色值。所以问题在于，如果你看这个数字网格，完全看不出这个数字网格应该代表一只猫，也没有明显的方法将这个原始像素值网格转换为具有语义意义的"猫"这个类别标签。

And what's even worse is that this entire grid of numbers can change drastically as we make relatively unassuming changes to the image. For example, if you were to imagine changing the viewpoint of this image, maybe if we were to take an image of a photograph of this exact same cat from just a slightly different angle, then to all of us we would probably recognize it definitely would still recognize it as a cat for sure and we would probably still recognize it as this exact same cat because we could recognize the markings on its face.

更糟糕的是，当我们对图像进行相对不起眼的更改时，整个数字网格可能会发生剧烈变化。例如，如果你想象改变这张图像的视角，也许我们从稍微不同的角度拍摄这只完全相同的猫的照片，那么我们所有人可能仍然肯定会认出它是一只猫，而且我们可能仍然会认出这是完全相同的猫，因为我们可以识别它脸上的斑纹。

And whatnot, but the problem is that due to this semantic gap, this difference between what we understand when we look at images and what is represented in this raw grid of pixel values, if we were to make even a simple change to this image like photographing from a slightly different angle, all of the pixel values would change in a very unintuitive way. And we somehow need to be able to design algorithms that are robust to these massive changes in the raw pixel values that can arise from relatively simple changes.

但问题是，由于这种语义鸿沟——即我们观察图像时的理解与原始像素值网格所表示内容之间的差异——即使我们对图像进行简单更改，比如从稍微不同的角度拍摄，所有像素值都会以非常不直观的方式发生变化。我们需要设计出能够应对这些由相对简单变化引起的原始像素值巨大变化的鲁棒算法。

To the images themselves, so there's a lot more we need to deal with beyond viewpoint variation in order to perform image classification. We also need to deal with things like intra class variation. So if we look at different images, different cats all look very different and each of these different adorable cats all produces very different grids of pixel values on the raw sensor of the camera. So we somehow need to build our systems that are robust to these massive variations that can occur within categories.

图像本身而言，为了执行图像分类，我们需要处理的问题远不止视角变化。我们还需要处理类内差异等问题。如果我们观察不同的图像，不同的猫看起来都大不相同，每只可爱的猫都会在相机原始传感器上产生非常不同的像素值网格。因此我们需要构建能够应对类别内可能出现的这些巨大变化的鲁棒系统。

So there's another problem which is sometimes we want to recognize fine grained categories. So far we've talked about recognizing maybe cats versus dogs versus trucks, but depending on the task at hand we might want to recognize different categories that appear very visually similar. For example, if we were to try, we might want to recognize different breeds of cats in some applications. So we have different categories that appear very visually similar and this is again a huge practical problem and it's not clear at all how to write algorithms that are robust to changes in image pixels.

还有一个问题是我们有时需要识别细粒度类别。到目前为止我们讨论的可能是识别猫、狗或卡车，但根据具体任务，我们可能需要识别视觉上非常相似的类别。例如，在某些应用中，我们可能需要识别不同品种的猫。因此我们面临着视觉上非常相似的不同类别，这又是一个巨大的实际问题，而且完全不清楚如何编写对图像像素变化具有鲁棒性的算法。

In this way, we also need our algorithms to be robust to background clutter. So sometimes the objects in the image that we want to recognize somehow blend into the background, maybe due to natural camouflage or other sorts of crazy things going on in the scene. We need our classifiers to be robust to illumination change as we change the lighting conditions in the scene, turn on and turn off lights, take pictures in the dark, take pictures in the daylight.

因此，我们还需要算法对背景干扰具有鲁棒性。有时图像中我们想要识别的物体会与背景融为一体，可能是由于自然伪装或场景中其他复杂因素造成的。我们需要分类器对照明变化具有鲁棒性，无论是改变场景中的光照条件、开关灯光、在黑暗中拍摄还是在日光下拍摄。

The underlying semantics of the objects in the images do not change, so our algorithm should be robust to these massive changes in different lighting conditions. The objects that we want to recognize in images might appear in very different views, very different poses, and very different positions in the image. We might somehow need to deal with occlusion, so sometimes the object that we want to recognize in the image might not be visible hardly at all.

图像中物体的底层语义不会改变，因此我们的算法应该对不同光照条件下的这些巨大变化具有鲁棒性。我们想要在图像中识别的物体可能以非常不同的视角、非常不同的姿态和非常不同的位置出现。我们可能需要以某种方式处理遮挡问题，因此有时我们想要在图像中识别的物体可能几乎完全不可见。

And I think this example on the right is really interesting. This is basically a couch and we see a tail sticking out from underneath the couch cushion. Now you probably intuitively thought that that was a cat because we've seen a lot of images of cats, because you know that cats usually live in houses, and because you know that maybe cats like to burrow down under things sometimes. But actually if you think about the evidence in the raw image evidence of this image, we don't even know that this is a cat.

我认为右边的这个例子非常有趣。这基本上是一个沙发，我们看到沙发垫下面露出一条尾巴。你可能会直觉地认为那是一只猫，因为我们看过很多猫的图片，你知道猫通常住在房子里，而且你知道猫有时喜欢钻到东西下面。但实际上，如果你考虑这张原始图像中的证据，我们甚至无法确定这是否是一只猫。

This could be a raccoon. This could be some other kind of a crazy animal with a tail right. So somehow even this is relatively simple problem of giving category labels to objects and images can involve a lot of common-sense reasoning about the world. Your knowledge that cats live in houses and raccoons are unlikely to live under cushions of couches right. So even this relatively unassuming problem of classifying images becomes very challenging very quickly if we want to recognize the full breadth of objects that exist in the world and all the variations and positions and appearances and changes in ways of those objects.

这可能是一只浣熊。这可能是其他某种有尾巴的奇怪动物。因此某种程度上，即使是给物体和图像赋予类别标签这个相对简单的问题，也可能涉及大量关于世界的常识推理。你知道猫住在房子里，而浣熊不太可能住在沙发垫下面。因此，即使这个相对简单的图像分类问题，如果我们想要识别世界上存在的所有物体及其各种变化、位置、外观和方式的改变，也会很快变得极具挑战性。

and appear in images around in the world. So if we were somehow able to overcome all of these problems and write down algorithms that could perform robust image classification and recognize lots of categories and lock them in lots of different situations, it would be really really useful. We already saw in the last lecture how some applications of computer vision can unlock many different scientific questions. So we could use image classification for things like medical imaging medical diagnosis. Maybe we could take pictures of skin lesions and diagnose them as malignant.

并出现在世界各地的图像中。因此，如果我们能够以某种方式克服所有这些问题，编写出能够执行稳健图像分类、识别多种类别并在各种不同情况下锁定它们的算法，那将非常非常有用的。我们在上一讲中已经看到计算机视觉的某些应用如何能够解锁许多不同的科学问题。因此我们可以将图像分类用于医学影像和医疗诊断等领域。也许我们可以拍摄皮肤病变的照片并将其诊断为恶性。

or non malignant tumors. Maybe we could take pictures of x-rays and try to classify what types of problems could exist in medical images. In this case, the robust image classification could be useful for astronomers who want to go out and collect visual data from telescopes and other types of sensors and then classify what types of phenomenon are out there in the sky. These could also be useful for many other scientific applications like maybe recognizing whales or categorizing many different types of animals that could appear in sensors.

或非恶性肿瘤。也许我们可以拍摄X光片并尝试分类医学影像中可能存在的问题类型。在这种情况下，稳健的图像分类对于天文学家非常有用，他们想要收集来自望远镜和其他类型传感器的视觉数据，然后分类天空中存在的各种现象类型。这些也可用于许多其他科学应用，比如识别鲸鱼或分类可能出现在传感器中的许多不同类型动物。

So image classification on its own is this really really useful problem and if we could solve it it can unlock a lot of really powerful and useful applications. But what I think is possibly even more interesting and maybe less intuitive is that image classification is also a fundamental building block of different algorithms we might want to perform inside computer vision. As an example so far we've talked about image classification so there's a related task in computer vision called object detection. In object detection what we want to do is draw boxes around the objects that in images and say not just what objects.

图像分类本身就是一个非常非常有用的课题，如果我们能够解决它，就能解锁许多真正强大且实用的应用。但我认为可能更有趣且不太直观的是，图像分类也是我们在计算机视觉内部可能想要执行的不同算法的基本构建模块。例如，到目前为止我们已经讨论了图像分类，计算机视觉中有一个相关任务称为目标检测。在目标检测中，我们想要做的是在图像中的物体周围绘制边界框，并不仅仅说明是什么物体。

That in images and say not just what objects are present in the image but where they are located in the image. It turns out that image classification is itself a subpart that can be used to build up to more complex applications like object detection. As an example, one way to perform object detection is via image classification of different sliding windows in the image. So one way to perform detection is to just classify different sub-regions of the image.

在图像中不仅要说明图像中存在什么物体，还要说明它们在图像中的位置。事实证明，图像分类本身就是一个子部分，可用于构建更复杂的应用，如目标检测。例如，执行目标检测的一种方法是通过对图像中不同滑动窗口进行图像分类。所以执行检测的一种方法就是分类图像的不同子区域。

So we could look at a sub region over here and then classify it as background, horse, person, car or truck. In this case it's classified as background because there are no objects here. If we were to classify this box we should classify it as person, etc. So you can see that if we had the ability to build really powerful image classifiers, that would again let us build other applications like object detectors. Even something like image captioning is often framed as a classification as an image classification problem. So here the idea is that given an input image, we might want to write a natural language sentence to describe what is in the image.

因此我们可以查看这里的子区域，然后将其分类为背景、马、人、汽车或卡车。在这种情况下它被分类为背景，因为这里没有物体。如果我们要对这个框进行分类，我们应该将其分类为人等等。所以你可以看到，如果我们有能力构建真正强大的图像分类器，这将再次让我们构建其他应用，如目标检测器。甚至像图像描述这样的任务也经常被构建为图像分类问题。所以这里的想法是，给定输入图像，我们可能想要编写自然语言句子来描述图像中的内容。

The image and here this can be framed as a sequence of classification problems just as in the object detection case. So here we've got maybe some fixed vocabulary of words in English language that the algorithm is aware of and the question is what word should I say next and this is again a classification problem. So then first we would classify and select this word man using an image classifier maybe select the word writing select the word horse and then select the word stop to know that's the end of the sentence.

图像在这里可以被构建为一系列分类问题，就像在目标检测案例中一样。所以我们可能有一些算法已知的英语固定词汇表，问题是我接下来应该说什么词，这又是一个分类问题。因此首先我们会使用图像分类器分类并选择"男人"这个词，可能选择"写"这个词，选择"马"这个词，然后选择"停止"这个词来表示句子结束。

So you can see that image. Even maybe another even work we did even slightly more outlandish application is playing computer games like Go. So people have built AI systems that can learn to play Go and even outperform many of the best human experts in the world. And this is basically a classification problem too. Here the input is now an image where each pixel of the image describes the state of the game board at some position. And now the output is a classification problem about which position on the board should I place my next stone.

所以你可以看到那张图片。甚至可能我们做的另一个稍微更奇特的应用是玩像围棋这样的电脑游戏。所以人们已经构建了可以学习下围棋甚至超越世界上许多最佳人类专家的AI系统。这基本上也是一个分类问题。这里的输入现在是一张图片，其中图片的每个像素描述了游戏棋盘在某个位置的状态。现在的输出是一个分类问题，关于我应该在棋盘的哪个位置放置我的下一颗棋子。

On so you can see that throughout these different applications this relatively unassuming problem of image classification is a really really powerful building block that we can use to build up to many more interesting problems in computer vision. So given all of that we really want people to write algorithms that can perform image classification really well.

因此你可以看到，在这些不同的应用中，这个相对不起眼的图像分类问题实际上是一个非常强大的构建模块，我们可以用它来构建计算机视觉中许多更有趣的问题。考虑到所有这些，我们确实希望人们能够编写出能够很好执行图像分类的算法。

But it's really not obvious at all how we should do this right if you were to just sit down your computer and start typing code you need to write this class this magical Python function that's going to input this giant grid of pixel values perform some magical computation and then somehow spit out cat or place the gold piece at position five nine or this this piece of image is or is not a piece of background and it's really not obvious at all what code you should type here right because unlike for something like sorting a list of integers there's really no well-defined algorithm for how to convert grids of numbers into caps so we need to do something so it's really not not clear at all how we should approach this problem.

但这确实一点都不明显，我们该如何做到这一点？如果你只是坐在电脑前开始输入代码，你需要编写这个类——这个神奇的Python函数，它将输入这个巨大的像素值网格，执行一些神奇的计算，然后以某种方式输出"猫"或将金棋子放在位置五九，或者判断这张图片是否属于背景的一部分。你在这里应该输入什么代码确实一点都不明显，因为与对整数列表排序这样的任务不同，将数字网格转换为分类结果确实没有明确定义的算法。所以我们需要采取某种方法，我们该如何解决这个问题确实一点都不清楚。

So one thing you might do is maybe just try to use your own human knowledge about what cats and other objects look like in order to hand code classifiers that try to pick out different object categories. So one thing you might imagine doing is, we talked in the last lecture about how edges in images are really important, so maybe what we could try to do is first take the image and then convert it and then extract edges using some kind of edge detection algorithm.

你可能会做的一件事是尝试利用自己关于猫和其他物体外观的人类知识，来手动编写能够识别不同物体类别的分类器。你可能会想到的一种方法是，我们在上一节课中讨论了图像中的边缘非常重要，所以也许我们可以尝试先获取图像，然后转换它，再使用某种边缘检测算法提取边缘。

And then maybe you try to find corners or other types of interpretable patterns in those edges right you know that may be cats of triangular pointy ears and you should hope that those ears come out in the edges so maybe you could kind of look for corners and then draw out right rules about what angles cat's ears are allowed to be maybe cats are supposed to have whiskers in different positions maybe the whiskers would come out and edges so you could imagine maybe like really trying to go in there and a hard code all your own human knowledge about what cats look like.

然后也许你会尝试在这些边缘中寻找角点或其他可解释的模式，你知道猫可能有三角形的尖耳朵，你会希望这些耳朵在边缘中显现出来。所以也许你可以寻找角点，然后制定关于猫耳朵允许角度的规则，也许猫应该在不同位置有胡须，也许胡须会在边缘中显现。所以你可以想象，也许真的尝试深入其中，用代码固化你所有关于猫外观的人类知识。

And try to write down some explicit algorithm for detecting them, but this is not a very good approach right. It's going to be brittle. There's going to be cats without whiskers or cats without pointy ears. Sometimes the edge detector will fail and won't detect the edges that you wanted it to. Maybe you spend a lot of time trying to figure out all those corner cases for cats, but now tomorrow we want to classify galaxies.

并尝试编写明确的检测算法，但这不是一个很好的方法。它会很脆弱。会有没有胡须的猫或没有尖耳朵的猫。有时边缘检测器会失败，无法检测到你想要的边缘。也许你花了很多时间试图解决猫的所有边缘情况，但明天我们想要对星系进行分类。

And probably all of the hard work that you put into your algorithm for recognizing cats from images is going to be completely thrown away tomorrow when we want to recognize galaxies instead. So we really want something that is more robust, some approach which is more scalable and some approach which doesn't require us to write down all of our own human knowledge about what different types of objects look like. So here's where we come to machine learning right. So the idea is that rather than trying to explicitly encode our own human knowledge about what different types of objects look like instead we're going to take a data-driven approach.

可能你为从图像中识别猫而投入的所有艰苦工作，在明天我们想要识别星系时都将完全被抛弃。所以我们真正需要的是更稳健的方法，更具可扩展性的方法，以及不需要我们写下所有关于不同物体外观的人类知识的方法。这就是我们转向机器学习的原因。其理念是，与其试图明确编码我们关于不同物体外观的人类知识，不如采用数据驱动的方法。

And have algorithms that can learn from data how to recognize different types of objects in images. So the basic pipeline for this machine learning system that we're going to build is that first we want to collect a large data set of images and label them with the types of labels we want our algorithm to predict right. So maybe if we want to build a cat vs. dog detector we need to go out and collect a lot of images of cats and dogs and hot dogs and not hot dogs.

拥有能够从数据中学习如何识别图像中不同类型物体的算法。因此，我们要构建的这个机器学习系统的基本流程是：首先需要收集一个大型图像数据集，并用我们希望算法预测的标签类型来标注它们。例如，如果我们想构建一个猫狗检测器，就需要收集大量猫、狗、热狗和非热狗的图片。

And then class and then go and collect human labels for which images are cats and dogs and hot dogs and whatnot. Then once we collect this large data set we're going to deploy some kind of machine learning algorithm which will try to learn statistical dependencies between the input images and the output labels that we wrote down during the data collection process. Then once we've used our machine learning algorithm to extract these statistical dependencies we can evaluate this classifier on new images.

然后分类并收集人工标注，确定哪些图像是猫、狗、热狗等等。一旦收集到这个大型数据集，我们将部署某种机器学习算法，该算法将尝试学习输入图像与我们在数据收集过程中记录的输出标签之间的统计依赖关系。然后当我们使用机器学习算法提取这些统计依赖关系后，就可以在新图像上评估这个分类器。

So what this looks like then basically instead of writing this single monolithic function called classify image, instead we have this really two piece API. One is this we need to write one function called train which is going to input a collection of images and their associated labels, it's going to perform some magical machine learning and then it's going to return some statistical model. Then our second piece of API is this predict function which is going to input the model that we produced during the training phase as well as new images on which to evaluate.

那么这看起来基本上是这样的：我们不再编写一个名为"classify image"的单一庞大函数，而是采用这种两部分的API。第一部分是我们需要编写一个名为"train"的函数，它将输入一组图像及其相关标签，执行一些神奇的机器学习操作，然后返回某个统计模型。然后我们API的第二部分是"predict"函数，它将输入我们在训练阶段产生的模型以及需要评估的新图像。

That model this will run the model on the new images and then spit out the labels as they have been learned from the training set. So what's really interesting about this approach is that it's kind of a different way to program computers right when think about writing algorithms to sort images - sorry - sort numbers and lists or perform other kinds of classical algorithms you're basically using your own human knowledge to tell the computer exactly what steps it needs to perform in order to produce the output that you want it to produce.

该模型将在新图像上运行模型，然后输出从训练集中学习到的标签。这种方法真正有趣的地方在于，它是一种不同的计算机编程方式。当你考虑编写算法来对图像进行分类时——抱歉——对数字和列表进行排序或执行其他类型的经典算法时，你基本上是在使用自己的人类知识来告诉计算机需要执行哪些确切步骤，以产生你希望它产生的输出。

But now when we take a data-driven machine learning approach instead, what we're basically doing is programming the computer via the data that we feed it. Now if we want to program the computer to recognize cats, we feed it images of cats. If we want to recognize galaxies instead, all we need to do is collect a new data set of galaxies. We don't need to recode our machine learning algorithm hopefully, and instead we can just feed new data and then change the behavior of the program.

但现在当我们采用数据驱动的机器学习方法时，我们基本上是通过输入的数据来对计算机进行编程。如果我们想让计算机识别猫，我们就输入猫的图像。如果我们想识别星系，我们只需要收集一个新的星系数据集。我们不需要重新编写机器学习算法，而是只需输入新数据就能改变程序的行为。

But now if we want to recognize galaxies instead, all we need to do is collect a new data set of galaxies. We don't need to recode our machine learning algorithm hopefully, and instead we can just feed new data and then change the behavior of the program. So now this is a really powerful paradigm for a lot of problems where we don't know how to write down an explicit program to solve them. So this is the approach that will be taking through. This has become the dominant approach for basically all visual recognition problems, image classification included. So now that we've sort of settled on this machine learning data-driven approach to recognize images, we need to talk about sources of data right. So there's a couple common image classification data sets that you'll tend to come across.

但现在如果我们想识别星系，我们只需要收集一个新的星系数据集。我们不需要重新编写机器学习算法，而是只需输入新数据就能改变程序的行为。因此，对于许多我们不知道如何编写明确程序来解决的问题来说，这是一个非常强大的范式。这就是我们将要采用的方法。这已成为基本上所有视觉识别问题（包括图像分类）的主要方法。既然我们已经确定了这种机器学习数据驱动的方法来识别图像，我们需要讨论数据来源。有几个常见的图像分类数据集你可能会遇到。

The Emnes data set is one of the most common, containing ten classes of digits from 0 to 9. The images are 28 by 28 pixel grayscale images, making them very tiny. It provides 50,000 training images and 10,000 test images. In the last lecture, we discussed how convolutional neural networks were developed in the 80s and 90s and deployed in commercial products to read handwritten digits on checks. This MMS dataset was used for that industrial application of recognizing handwritten digits on checks and was deployed.

Emnes数据集是最常见的数据集之一，包含0到9共十个数字类别。图像为28x28像素的灰度图像，尺寸非常小。它提供了50,000张训练图像和10,000张测试图像。在上次讲座中，我们讨论了卷积神经网络如何在80年代和90年代发展，并部署在商业产品中用于读取支票上的手写数字。这个MMS数据集确实被用于识别支票上手写数字的工业应用并得到了实际部署。

In the world, so even though this seems like kind of a toy dataset, it really has a lot of rich history behind it and has been very useful in the development of many machine learning algorithms. But that said, this dataset has sometimes been called the Drosophila of computer vision.

在世界范围内，尽管这看起来像是一个玩具数据集，但它背后确实有着丰富的历史，并且在许多机器学习算法的发展中非常有用。但话虽如此，这个数据集有时被称为计算机视觉领域的果蝇。

So you know that biologists often will go in for form a lot of initial experiments on fruit flies and then they could have work up to more interesting animals as they make their discoveries and this is really similar to the way that a lot of practitioners work on em mist so em missed because it's relatively small and simple data set it's very quick to try out new ideas on this data set but you have to be really careful when you're reading papers that only show results on a mist because it's very common that may be I mean basically everything works on that mist right you can write down sort of any reasonable machine learning algorithm will get very high performance on the end mist so this is treated really as sort of a proof of concept.

你知道生物学家经常会在果蝇身上进行大量初步实验，然后随着他们的发现逐步转向更有趣的动物进行研究。这与许多从业者在MNIST数据集上的工作方式非常相似。由于MNIST是一个相对较小且简单的数据集，在这个数据集上尝试新想法非常快速。但当你阅读仅展示在MNIST上结果的论文时必须非常小心，因为基本上所有算法都能在MNIST上取得良好表现，任何合理的机器学习算法都能在MNIST上获得很高的性能，因此它实际上被视为一种概念验证。

But just getting something to work here isn't really enough to impress people anymore. So instead, another dataset that you'll see very commonly used is CIFAR-10. CIFAR-10 has again very small images, 32 by 32, but they are color rather than greyscale. And now rather than handwritten digits, the categories are much more interesting: airplanes, automobiles, birds, cats, deer. You can read it on the slide. And this is a fairly decently sized dataset: 50,000 training, 10,000 tests.

但仅仅让某些东西在这里运行已经不足以让人印象深刻了。因此，另一个你会经常看到被使用的数据集是CIFAR-10。CIFAR-10同样是32x32的小图像，但它们是彩色而非灰度图。而且现在不再是手写数字，类别变得更加有趣：飞机、汽车、鸟类、猫、鹿等。你可以在幻灯片上看到完整列表。这是一个相当不错的数据集：50,000个训练样本，10,000个测试样本。

And this is a and even though it's relatively small compared to other large-scale data sets, it's reasonably challenging since these categories are reasonably difficult to recognize. As a result, we'll be using the CIFAR-10 data set for most of the homework assignments throughout the semester. CIFAR-10 has a cousin called CIFAR-100 that's basically similar statistics except we've got a hundred categories rather than ten. I think people use CIFAR-100 a little bit less than CIFAR-10, but you'll sometimes see people working on this and it's nice to be aware of it.

尽管与其他大规模数据集相比相对较小，但由于这些类别识别难度较高，它仍然具有相当的挑战性。因此，本学期大部分作业我们将使用CIFAR-10数据集。CIFAR-10有一个姊妹数据集CIFAR-100，基本统计特征相似，只是类别从10个增加到100个。我认为CIFAR-100的使用频率略低于CIFAR-10，但有时你会看到人们使用它，了解这一点很有好处。

So we talked last lecture about the ImageNet dataset and this has become something of the gold standard for image classification datasets. Basically, when you try to submit a research paper that proposes some new tweak to an image classification algorithm, if you don't show results on ImageNet, reviewers will probably complain and your paper will probably be rejected. ImageNet is really considered a super important dataset to benchmark image classification algorithms these days. ImageNet is very interesting because it contains a thousand different categories.

我们在上一节课中讨论了ImageNet数据集，它已经成为图像分类数据集事实上的黄金标准。基本上，当你试图提交一篇研究论文，提出对图像分类算法的某些新调整时，如果你没有在ImageNet上展示结果，审稿人很可能会提出异议，你的论文很可能会被拒绝。如今，ImageNet确实被认为是评估图像分类算法性能的超级重要数据集。ImageNet非常有趣，因为它包含了一千个不同的类别。

This is much more than the ten categories in CIFAR-10, and it's very large. We've got about 1.3 million training images with about 1,300 training images per category, and it gives standard validation and test sets. The question was how big are the images in ImageNet. The issue is that ImageNet images were downloaded from the web and they actually differ in resolution quite a lot, but for most practical applications people resize them to either 256 by 256.

这比CIFAR-10的十个类别要多得多，而且规模非常大。我们有大约130万张训练图像，每个类别约有1300张训练图像，并提供了标准的验证集和测试集。问题是ImageNet中的图像有多大。问题在于ImageNet图像是从网络下载的，它们的分辨率实际上差异很大，但在大多数实际应用中，人们会将它们调整为256×256。

Now one question was how big are the images in ImageNet. Well the issue is that ImageNet images were downloaded from the web and they actually differ in resolution quite a lot, but for most practical applications people resize them to either 256 by 256 or sometimes 224 by 224 when training on those images. So one interesting bit about ImageNet is the accuracy metric that people report here. Because there's a thousand different categories in ImageNet, it's very difficult and possibly unreasonable to expect algorithms to pick out the exact one correct category, especially because some of the neat labels are a little bit noisy anyway. So what people do in practice here is have the algorithm predict 5 category labels and then we count the algorithm.

现在有一个问题是ImageNet中的图像有多大。问题在于ImageNet图像是从网络下载的，它们的分辨率实际上差异很大，但在大多数实际应用中，人们在训练这些图像时会将其调整为256×256或有时是224×224。关于ImageNet的一个有趣之处是人们在这里报告的准确度指标。因为ImageNet有一千个不同的类别，期望算法准确挑选出一个正确的类别是非常困难且可能不合理的，特别是因为一些标签本身就有点嘈杂。因此，实际做法是让算法预测5个类别标签，然后我们计算算法。

Predict 5 category labels and then we count the algorithm as having made a correct prediction if the correct category is in any one of those five predictions. So that's just a little bit of nuance to the way ImageNet is typically evaluated. Those are kind of the most standard image classification data sets that you'll see out there. Another interesting one is MIT Places. ImageNet images tend to focus on objects like cats and dogs and fish and trucks and things like that. So there's another related data set that tries to focus on scene categories.

预测5个类别标签，如果正确的类别出现在这五个预测中的任何一个，我们就认为算法做出了正确预测。这只是ImageNet评估方式的一个细微差别。这些是你将会见到的最标准的图像分类数据集。另一个有趣的数据集是MIT Places。ImageNet图像往往侧重于猫、狗、鱼、卡车等物体。因此还有另一个相关数据集试图专注于场景类别。

Like classrooms and fields and buildings and things like that so it's nice to be aware of. Now one thing that's really interesting is to try to compare these classification data sets in terms of their size. So here this is the number of pixels in the training set for these different data sets and this is assuming 256 by 256 for ImageNet and Places. And what you'll note here is the y-axis is on a log scale so you'll see that CIFAR is maybe about an order of magnitude bigger than MNIST.

比如教室、田野、建筑等等，了解这些是很好的。现在真正有趣的一点是尝试比较这些分类数据集的大小。这里显示的是不同数据集中训练集的像素数量，这是假设ImageNet和Places数据集都采用256×256分辨率。你会注意到这里的y轴是对数刻度，所以你会看到CIFAR可能比MNIST大约大一个数量级。

So you'll see that CIFAR is maybe about an order of magnitude bigger than MNIST. ImageNet is roughly two orders of magnitude bigger than CIFAR and then Places is yet another order of magnitude bigger than ImageNet. This kind of drives home the point about why ImageNet is somehow a qualitatively different data set than these other ones that you'll see people work on sometimes. That makes results on ImageNet much more convincing but unfortunately very computationally expensive to work with sometimes.

所以你会看到CIFAR可能比MNIST大约大一个数量级。ImageNet大约比CIFAR大两个数量级，而Places又比ImageNet大一个数量级。这很好地说明了为什么ImageNet在某种程度上是一个与其他数据集性质不同的数据集，你有时会看到人们研究这些其他数据集。这使得ImageNet上的结果更加令人信服，但不幸的是，有时计算成本非常高。

So as a result we're kind of sticking with CIFAR is kind of a sweet middle ground in this course that kind of splits the difference between the complexities of the visual recognition tasks that show up an image net and the computational affordability of smaller data sets like a mist. So what's also interesting to see from this chart is this increasing trend of datasets getting bigger and bigger and bigger over time.

因此，我们在这门课程中坚持使用CIFAR数据集，它处于一个理想的中间地带，既平衡了ImageNet中视觉识别任务的复杂性，又保持了像MNIST这样较小数据集的计算经济性。从这张图表中还可以看到一个有趣的趋势，即数据集随着时间的推移变得越来越大。

So that's definitely one interesting direction for research: how can we use ever bigger and bigger data sets to enhance the abilities of our algorithms to perform robust classification? But people have also started thinking in the other direction as well. One interesting data set to be aware of there is the Omniglot data set. Here Omniglot kind of pushes things to the extreme and wants to benchmark the ability of algorithms to learn with relatively little data. On Omniglot we've got more than 1,600 different categories. Each of these categories is a letter in some alphabet from some language somewhere on earth. So they've got letters from more than fifty different alphabets of different written languages. The really interesting thing about Omniglot is that rather than giving you tons and tons of examples of each image of each category, it only gives you

这绝对是一个有趣的研究方向：我们如何利用越来越大的数据集来增强算法执行鲁棒分类的能力？但人们也开始从相反的方向思考。一个值得关注的有趣数据集是Omniglot数据集。这里Omniglot将事情推向了极端，旨在评估算法在相对较少数据情况下的学习能力。在Omniglot中，我们有超过1,600个不同类别。每个类别都是地球上某种语言字母表中的一个字母。它们包含了来自50多种不同书写语言字母表的字母。Omniglot真正有趣的地方在于，它不是为每个类别的每张图像提供大量示例，而是只提供

It only gives you 20 examples for each of these letters, and somehow the challenge is to build algorithms that can learn very robustly from relatively few examples of each image category. So this so-called low shot classification problem is a really huge and emerging key area of research where a lot of researchers are starting to think about these days. So now that we've talked about some of the common datasets that you'll run into for image classification, it's time to think about our first classification algorithm.

它只为每个字母提供20个示例，而挑战在于构建能够从每个图像类别相对较少的示例中稳健学习的算法。因此，这个所谓的低样本分类问题是一个非常重要且新兴的关键研究领域，如今许多研究人员都开始关注这个问题。既然我们已经讨论了图像分类中常见的一些数据集，现在该考虑我们的第一个分类算法了。

Because data only gets you so far, you need some algorithm to actually make use of that data. So the first learning algorithm that we're going to talk about is nearest neighbor, and this one is so simple it might not even deserve the name of a learning algorithm. What it does is remember, I told you that when we implement a machine learning system, we need to implement these two functions: one called train and one called predict. Well for nearest neighbor, the train function is trivial, we're simply going to memorize all the training data.

因为数据的作用有限，你需要某种算法来实际利用这些数据。所以我们要讨论的第一个学习算法是最近邻算法，这个算法非常简单，甚至可能不配被称为学习算法。它的作用是记住，我告诉过你，当我们实现机器学习系统时，需要实现这两个函数：一个叫训练函数，一个叫预测函数。对于最近邻算法，训练函数很简单，我们只需要记住所有的训练数据。

We're not going to send that, we're not going to process it, we're not going to do anything with it. We're just going to memorize all of our training data. Now in the predict side, what we're going to do is take our new image that we want to predict a label for, compare it to each one of our images in the training set using some kind of comparison or similarity function, and now we're going to keep track of the most similar image in the training set to our test image.

我们不会发送它，不会处理它，也不会对它做任何操作。我们只是要记住所有的训练数据。在预测端，我们要做的是获取需要预测标签的新图像，使用某种比较或相似性函数将其与训练集中的每个图像进行比较，然后记录训练集中与测试图像最相似的图像。

And now we're so going to simply return the label of the most similar training image so this is like I said very very simple straightforward learning algorithm and it only learns in the sense that it kind of memorizes the training data but in order to implement this algorithm we need to actually write down some function that can compute the similarity between two input images so some very common choice so basically we need to write down some kind of distance distance metric which can input a pair of images and then spit out some number representing how semantically similar are those two.

现在我们只需返回最相似训练图像的标签，正如我所说，这是一个非常简单直接的学习算法，它所谓的"学习"本质上只是记忆训练数据。但为了实现这个算法，我们需要实际编写一个能够计算两个输入图像相似度的函数，通常的做法是建立某种距离度量标准，该标准能够输入一对图像并输出一个数值来表示这两个图像在语义上的相似程度。

To perform this nearest neighbor classification, one very common choice of this distance metric is a very simple one: just use the L1 or Manhattan distance between the pixels of images. Here what we're going to do is take our test image. We're imagining a very simple four by four test image that we've written down the values of all of its pixels explicitly. To compare it to a training image, we simply take the absolute value of the differences between all.

为了执行这种最近邻分类，一个非常常见的距离度量选择非常简单：只需使用图像像素之间的L1或曼哈顿距离。这里我们要做的是获取测试图像。我们设想一个非常简单的4x4测试图像，已经明确写出了所有像素值。为了将其与训练图像进行比较，我们只需对所有差异取绝对值。

To perform this nearest neighbor classification, one very common choice of this distance metric is a very simple one: just use the L1 or Manhattan distance between the pixels of images. Here what we're going to do is take our test image. We're imagining a very simple four by four test image that we've written down the values of all of its pixels explicitly. To compare it to a training image, we simply take the absolute value of the differences between all the corresponding pixels in the two images and then sum up all of these absolute values of all the differences in the corresponding pixels and that gives us a single number representing the distance or difference between those two images. So one thing to point out here is that this kind of satisfies all the normal rules for a metric from mathematics right so if we've got two images that are exactly the same we'll have a distance of zero things like a triangle inequality are satisfied.

为了执行这种最近邻分类，一个非常常见的距离度量选择非常简单：只需使用图像像素之间的L1或曼哈顿距离。这里我们要做的是获取测试图像。我们设想一个非常简单的4x4测试图像，已经明确写出了所有像素值。为了将其与训练图像进行比较，我们只需计算两幅图像中所有对应像素差值的绝对值，然后将所有这些对应像素差值的绝对值相加，这样就得到了一个代表两幅图像之间距离或差异的数值。这里需要指出的一点是，这种方法满足数学中度量标准的所有常规规则，所以如果我们有两幅完全相同的图像，它们的距离将为零，三角不等式等规则也都得到满足。

This is a reasonably well mathematically defined metric. So now basically with these couple bits of information, that's enough to implement your first learning algorithm. Indeed, the nearest neighbor classifier is such a simple and straightforward algorithm that we can fit a full implementation on a slide. I think people can, with even with some comments and even better, I think you might even be able to read it. So here in our nearest neighbor classifier, I told you we need to implement two things: one is this train step which is trivial.

这是一个数学上定义相当完善的度量标准。因此，基本上有了这几条信息，就足以实现你的第一个学习算法。实际上，最近邻分类器是如此简单直接的算法，我们甚至可以将完整实现放在一张幻灯片上。我认为即使加上一些注释，人们也能理解，甚至更好的是，我认为你甚至能够读懂它。在我们的最近邻分类器中，我提到我们需要实现两个部分：一个是训练步骤，这部分很简单。

Here we just memorize the training data. We assign the images X and their labels Y to some number of variables of our class. Then in the predict we have is again very simple. We take some new images, some new test images X. We simply iterate all over all the images in the training set, compute this L1 distance, and then return the label of the most similar image. So that's it, that's nearest neighbor. You can now implement your own machine learning systems. So a couple questions. So with this nearest neighbor classifier, suppose we have a training set of n examples.

我们只需记住训练数据。将图像X及其标签Y分配给类的若干变量。预测部分也非常简单：我们获取一些新图像，一些新的测试图像X，只需遍历训练集中的所有图像，计算L1距离，然后返回最相似图像的标签。就是这样，这就是最近邻算法。现在你可以实现自己的机器学习系统了。那么有几个问题：使用这个最近邻分类器时，假设我们有一个包含n个样本的训练集。

Then how fast is training? No, no, it seems tricky. Well, I guess it kind of depends on your copy semantics, but I would say that this is maybe constant time training if we're just going to store pointers to all of the training data. And that could be done in constant time. If you were to make a deep copy, that maybe be linear time, but let's not do that. So then the question is again with n examples, how fast is testing going to be? Well, this one's going to be linear.

那么训练速度有多快？不，不，这似乎很棘手。嗯，我想这在某种程度上取决于你的复制语义，但我会说如果我们只是存储所有训练数据的指针，这可能是常数时间的训练。这可以在常数时间内完成。如果你要进行深度复制，那可能是线性时间，但我们不这样做。那么问题又来了，对于n个样本，测试速度会有多快？这个将是线性的。

With n time right because kind of folding the size of the image and computation of the norm we're going to call that a constant which means that now for every testing example we need to compare it to each of the n training examples which means that at test time we're going to pay a performance penalty that's linear in the size of the training set. Now this is actually really bad, this is like really really really bad, this is actually the opposite of what we want from machine learning systems right.

因为将图像大小和范数计算视为常数，这意味着现在对于每个测试样本，我们需要将其与n个训练样本逐一比较，这意味着在测试时我们将付出与训练集大小成线性关系的性能代价。这实际上非常糟糕，这真的非常非常糟糕，这实际上与我们期望的机器学习系统完全相反。

Because if you think about how we want to deploy a machine learning system, what we want to do is somehow collect as much data as we possibly can about our task at hand and then maybe use this large amount of data and train a big powerful model. It's okay if training that model takes a long long time, but when we finally deploy that model and actually use that model at test time, we'd like it to be very fast. We'd like to be able to run these models maybe on your mobile phone in real time.

因为如果你考虑我们想要如何部署一个机器学习系统，我们想要做的是尽可能多地收集与手头任务相关的数据，然后可能利用这些大量数据训练一个强大的模型。即使训练这个模型需要很长时间也没关系，但当我们最终部署该模型并在测试时实际使用它时，我们希望它非常快速。我们希望能够在你的手机上实时运行这些模型。

We'd like to run it for millions or billions of users on the web for all photos that are getting run around on the internet. So somehow this is the exact opposite characteristics of what we'd usually like for in a machine learning system. We'll see that as we move to neural network based approaches, they kind of invert this bit of hierarchy. These neural network systems that we'll end up using will be relatively long to train but then relatively fast at inference time.

我们希望能够在网络上为数百万或数十亿用户运行它，处理所有在互联网上传播的照片。因此，这某种程度上与我们通常期望的机器学习系统特性完全相反。我们将看到，当我们转向基于神经网络的方法时，它们某种程度上颠倒了这种层级关系。我们最终将使用的这些神经网络系统训练时间相对较长，但在推理时相对较快。

So of course I also need to point out that sort of for completeness that there are many interesting algorithms for computing approximate nearest neighbors. When you perform approximate nearest neighbor computation, this can be done maybe much more faster than these full brute-force approaches. These are kind of beyond the scope of this class, but it's nice to be aware of in case you find yourself in a situation where you really need to perform some large-scale nearest neighbor search for some reason. So now once we've got this idea of nearest neighbor classification, we can think about how does it.

当然，为了完整性，我也需要指出有很多有趣的近似最近邻计算算法。当你执行近似最近邻计算时，这可能比这些完整的暴力方法要快得多。这些内容超出了本课程的范畴，但了解这些是很好的，以防你发现自己确实需要执行大规模最近邻搜索。所以现在我们有了最近邻分类的概念，我们可以思考它是如何。

We can think about how does it actually perform on images. So here what we're showing is the results of nearest neighbor classification on the CIFAR-10 dataset. Here on the left column we're seeing a bunch of test examples from the CIFAR-10 test set, and then along each row we're seeing the nearest neighbors from the training set to each of those test examples. Because as you might assume, as is sort of intuitive, because we're computing the distance between images by literally comparing the values of their pixels, the nearest neighbors tend to be.

我们可以思考它实际上在图像上的表现如何。这里我们展示的是CIFAR-10数据集上的最近邻分类结果。在左侧列中我们看到来自CIFAR-10测试集的一批测试样本，然后沿着每一行我们看到训练集中与每个测试样本对应的最近邻。正如你可能假设的那样，这某种程度上是直观的，因为我们通过直接比较像素值来计算图像之间的距离，最近邻往往是。

The nearest neighbors tend to be images that look very visually similar. If you look at maybe the third row, you've got this orange blob in the middle as our test image. Then if you look at the row, you see other images. The nearest neighbors that we retrieve are kind of things that have maybe orange or reddish blobs in the middle and then kind of a green or brownish background. So this L1 distance that we're using to compute nearest neighbors is really not very smart and it doesn't know much about what it's looking at.

最近邻往往是视觉上非常相似的图像。如果你看第三行，中间这个橙色斑点就是我们的测试图像。然后看这一行，你会看到其他图像。我们检索到的最近邻通常是中间有橙色或红色斑点、背景呈绿色或棕色的图像。所以我们用来计算最近邻的这个L1距离确实不太智能，它并不了解自己正在观察的内容。

And we can kind of look, we can kind of get a sense for maybe how poorly this might perform if we look at which of these one nearest neighbors are correct or incorrect. So it's kind of tough to actually tell what these images are sometimes just by looking at them because they're relatively low resolution, but what I've tried to do is draw red boxes around the one nearest neighbors that are incorrect and green boxes are on the one nearest neighbors that are correct, and this gives you a sense that even though images can look very visually.

我们可以观察一下，通过查看哪些最近邻分类正确或错误，可以大致了解这种方法的性能可能有多差。有时候仅凭观察这些图像很难判断它们是什么，因为分辨率相对较低，但我尝试在错误的最近邻周围绘制红框，在正确的最近邻周围绘制绿框，这让你感受到尽管图像在视觉上看起来非常。

Similar similar as measured by this L1 distance, they actually can sometimes have very different semantic meanings. This is clear maybe in the fourth row when you see this kind of brown blob surrounded by a white background. I think it's a frog right, I think it's a frog actually for the test image, but then its nearest neighbor is actually a cat. The cat is also a brown blob on a white background, so it looks very visually similar by this L1 metric.

通过这种L1距离测量看起来相似的图像，实际上有时可能具有非常不同的语义含义。这在第四行中可能很明显，当你看到这种被白色背景包围的棕色斑点时。我认为测试图像实际上是一只青蛙，但它的最近邻实际上是一只猫。这只猫也是白色背景上的棕色斑点，因此通过这种L1度量标准看起来非常相似。

But the label is different so it would be to the R we would make an incorrect classification on this on this thing. So this is one way to think to sort of get an intuitive understanding for what a nearest neighbor classifier is doing. Another way to think about nearest neighbor classifiers is through this notion of decision boundaries that we can see in this plot that needs a bit of unpacking. So what we're showing here is we're imagining performing a nearest neighbor classification over images over images with two pixels.

但标签不同，因此对于R我们会对此做出错误分类。这是理解最近邻分类器工作原理的一种直观方式。另一种思考最近邻分类器的方法是通过决策边界的概念，我们可以在这张图中看到这一点，这需要稍作解释。我们在这里展示的是想象对具有两个像素的图像进行最近邻分类。

So then the x-axis here is the intensity value of one of our pixels and the y-axis here is the intensity value of another of our pixels. Now each of these colored dots that we're seeing are examples of training images where the color of the dot represents the category of the training image. So maybe red dots are cats and blue dots are dogs and so on and so forth. The color of the background region represents the category label that would be assigned to that point in space.

那么这里的x轴代表我们某个像素的强度值，y轴代表另一个像素的强度值。现在我们看到的这些彩色点都是训练图像的示例，其中点的颜色代表训练图像的类别。也许红点代表猫，蓝点代表狗，依此类推。背景区域的颜色代表该空间点将被分配到的类别标签。

If we were to run nearest-neighbor classification for one of those test images, for example in this red X here, we can see that the nearest neighbor in the training set is maybe this red dot here, which means that if we were to perform nearest neighbor classification for this red X we would predict the red category. So then what's interesting to look at here is the decision boundaries between different categories.

如果我们对其中一个测试图像运行最近邻分类，例如这里的红色X标记，我们可以看到训练集中最近的邻居可能是这个红点，这意味着如果我们对这个红色X执行最近邻分类，我们将预测红色类别。因此这里值得关注的是不同类别之间的决策边界。

So here we've drawn out this region in space that carves up between regions in space that would be classified as green and regions in space that would be classified as purple. When we look at the nearest neighbor classifier in this way we can recognize a couple interesting things. One is that these decision regions can be very noisy and are subject to outliers.

所以这里我们画出了空间中的这个区域，它将空间划分为将被分类为绿色的区域和将被分类为紫色的区域。当我们以这种方式观察最近邻分类器时，我们可以认识到几个有趣的现象。一是这些决策区域可能非常嘈杂且容易受到异常值的影响。

So for example we see that in our training set we've got this one yellow point kind of sitting out in the middle of a whole bunch of green points and maybe that's noise maybe actually it should have been labeled as green instead of yellow it's kind of hard to say but when we use this nearest neighbor classifier the presence of a single yellow point in this cloud of green is going to cause a bunch of test examples around that yellow point to be classified as yellow maybe that's good maybe that's bad we can also see over here on the left side of the screen we've got this kind of jagged discouraged decision boundary between the red class and the blue class.

例如，我们在训练集中看到有一个黄色点位于大量绿色点的中间，这可能是噪声，也许它本应被标记为绿色而非黄色，这很难判断。但当我们使用最近邻分类器时，绿色云团中单个黄色点的存在将导致该黄色点周围的一批测试样本被分类为黄色，这可能有利也可能不利。我们还可以在屏幕左侧看到红色类和蓝色类之间存在这种锯齿状的不规则决策边界。

And maybe and again this is because this relying on only the nearest neighbor to perform the classification can be a bit noisy. So the question is what might maybe what might we be able to do to kind of smooth out these decision boundaries and maybe give us a more robust to the classification. So one idea is to simply use more neighbors. So far we've talked about the nearest neighbor classifier as simply parroting out the label that has attached to the nearest training example to each test example.

也许这再次是因为仅依赖最近邻进行分类可能会产生一些噪声。那么问题在于，我们能够采取什么措施来平滑这些决策边界，并可能使分类更加稳健。一个想法是简单地使用更多的邻居。到目前为止，我们已经讨论了最近邻分类器，它只是简单地复制每个测试样本最近训练样本的标签。

But what we can do instead is use more than just that one nearest neighbor. Instead we can consider some set of K nearest neighbors and then take maybe a vote and then imagine some way of combining the category labels of each of our K nearest retrieved results. There are many different ways you might imagine doing this, but one simple idea is to simply take a majority vote among all the category labels of each of our K nearest neighbors. So then it's kind of interesting. Once this picture of the nearest neighbor classifier using decision boundaries lets us see some the difference between k equals 1 and k equals.

但我们可以做的是使用不止一个最近邻。相反，我们可以考虑一组K个最近邻，然后进行投票，并设想某种方法来组合我们K个最近检索结果中每个结果的类别标签。你可能想到很多不同的方法来实现这一点，但一个简单的想法就是对我们K个最近邻中每个类别的标签进行多数投票。这很有趣。通过最近邻分类器使用决策边界的图示，我们可以看到k等于1和k等于之间的差异。

So then it's kind of interesting. Once this picture of the nearest neighbor classifier using decision boundaries lets us see the difference between k equals 1 and k equals 3. One is that our decision boundaries got a lot smoother. You can see that when we use k equals 1, recall that our decision boundaries were very noisy as a result of only using 1 neighbor. Now if we use 3 neighbors instead to perform a classification on the same data set, we can see we've smoothed out the decision boundary between these two categories quite a lot. We can also see that this has helped to reduce the effect of outliers on our classification performance.

这很有趣。通过最近邻分类器使用决策边界的图示，我们可以看到k等于1和k等于3之间的差异。一是我们的决策边界变得更加平滑。可以看到，当我们使用k等于1时，由于只使用一个邻居，决策边界非常嘈杂。现在如果我们使用3个邻居对相同数据集进行分类，可以看到我们相当程度上平滑了这两个类别之间的决策边界。我们还可以看到这有助于减少异常值对分类性能的影响。

So now even though we still have this one yellow point hanging out in a cloud of green, it no longer affects it. It no longer results in this yellow classification region. Similarly, this region between red and blue has somehow got a little bit smoothed out by using more than one nearest neighbor. But there's another problem which is that when K is greater than 1, there might be ties between classes. So in this visualization with these white regions, all have three nearest neighbors that are better all of different categories. So somehow you need those, you need some mechanism for breaking ties.

现在尽管我们仍然有一个黄色点位于绿色点云中，但它不再产生影响。它不再导致这个黄色分类区域的出现。同样地，通过使用多个最近邻，红色和蓝色之间的区域在某种程度上变得更加平滑。但还有另一个问题，当K大于1时，类别之间可能会出现平局。在这个可视化中，这些白色区域都有三个更好的最近邻，且都属于不同类别。因此你需要某种机制来解决平局问题。

And maybe you could imagine having some heuristic then based on the distance. Maybe you back off and use the one nearest neighbor result. There are different heuristics you might imagine in this situation. Another thing that we might want to change or play around with when we do the nearest neighbor classifier is changing the distance metric that we use to compute similarity between images. So far we've talked about using this L1 distance between images, which was the sum of the absolute differences between all the corresponding pixels.

也许你可以设想基于距离使用某种启发式方法。也许你会退一步使用单一最近邻的结果。在这种情况下，你可以设想不同的启发式方法。当我们使用最近邻分类器时，另一个我们可能想要改变或调整的是用于计算图像之间相似度的距离度量。到目前为止，我们讨论的是使用图像之间的L1距离，即所有对应像素绝对差值的总和。

So far we've talked about using this L1 distance between images, which was the sum of the absolute differences between all the corresponding pixels in the two images. Another common choice is the L2 or Euclidean distance between the pixels of the image. This has the effect of taking the pixels of the image, stretching it onto a long vector, then imagining computing the Euclidean distance between points in a high dimensional space for those two images. What's interesting is if we flip back to this picture of nearest neighbors using decision boundaries, you can see that as we use different distance metrics we get sort of qualitatively.

到目前为止，我们讨论了使用图像之间的L1距离，即两幅图像中所有对应像素的绝对差值之和。另一个常见的选择是图像像素之间的L2或欧几里得距离。这种方法的效果是将图像的像素拉伸成一个长向量，然后想象计算这两幅图像在高维空间中点之间的欧几里得距离。有趣的是，如果我们回到使用决策边界的最邻近图像，你可以看到当我们使用不同的距离度量时，我们会得到某种定性的。

Different distance metrics we get sort of qualitatively different properties in the decision boundaries that arise. So I'll kind of leave this as an exercise to the reader. But with L1 classification we can see that all of the decision boundaries between categories are all composed of axis aligned chunks. They're either vertical line segments, horizontal line segments, or 45-degree angle line segments. But when we use the L2 or Euclidean distance class, if we use the Euclidean distance instead to compute nearest neighbors, then now our decision boundaries are still piecewise linear but those lines can appear at any orientation in the input.

不同的距离度量会在产生的决策边界中产生性质上不同的特性。所以我将把这留给读者作为练习。但在L1分类中，我们可以看到类别之间的所有决策边界都由轴对齐的块组成。它们要么是垂直线段，要么是水平线段，要么是45度角的线段。但当我们使用L2或欧几里得距离分类时，如果我们改用欧几里得距离来计算最近邻，那么我们的决策边界仍然是分段线性的，但这些线条可以以任何方向出现在输入中。

In the input, so somehow using different distance metrics is a way that you as the human expert can imbue some of your own human knowledge into the structure that you want the algorithm to take account of. So it's a little bit unclear maybe for whether L1 or L2 is going to make big differences. It's sort of not really intuitively clear what semantic differences in L1 versus an L2 distance metric is going to result in for the case of image classification.

在输入中，因此以某种方式使用不同的距离度量是您作为人类专家可以将自己的一些人类知识融入您希望算法考虑的结构中的一种方式。因此，对于L1或L2是否会产生巨大差异可能有点不清楚。对于图像分类的情况，L1与L2距离度量将导致什么语义差异，这在直观上并不十分清楚。

But what's really interesting about the K nearest neighbor algorithm is that basically if we choose different types of distance metrics, we can imagine applying K nearest neighbors to just about any type of data imaginable. So far we've talked about using traditional vector norms for vector metrics to compute distances between points, but you can imagine using very strange or interesting types of data and writing down very sophisticated distance functions between them in order to perform nearest neighbor classification on many different types of data sets.

K最近邻算法真正有趣的地方在于，如果我们选择不同类型的距离度量，基本上可以将K最近邻应用于几乎所有可以想象的数据类型。到目前为止，我们讨论的是使用传统的向量范数作为向量度量来计算点之间的距离，但你可以想象使用非常奇特或有趣的数据类型，并为其编写非常复杂的距离函数，以便在许多不同类型的数据集上执行最近邻分类。

So one example here is comparing research papers. There's this cool site called archive sanity that lets you go and have some interesting exploration around research papers that are coming out each day. One interesting feature of this website is that it lets you show papers that are similar to another paper. Here I looked up on archived sanity a paper that I wrote last year called measure R CNN, and then if we click show similar then what this does is basically does nearest-neighbor retrieval on these PDF files.

这里有一个比较研究论文的例子。有一个很酷的网站叫做archive sanity，可以让你每天对新发布的研究论文进行有趣的探索。这个网站的一个有趣功能是它可以显示与另一篇论文相似的论文。我在archived sanity上查找了我去年写的一篇名为measure R CNN的论文，然后如果我们点击显示相似论文，这个功能基本上就是对这些PDF文件进行最近邻检索。

And the way that it does that is by using an interesting distance metric. So the distance metric here is called TF-IDF similarity, that's a term frequency inverse document frequency that's very commonly used in a lot of NLP applications. I won't tell you how it works, but it's just kind of a distance metric that works on pieces of text that encodes human knowledge about the frequency that words appear in different documents. And what's interesting is that doing nearest-neighbor retrieval using this TF-IDF metric on research papers actually gives really good results.

它的实现方式是通过使用一种有趣的距离度量。这里的距离度量称为TF-IDF相似度，这是一种词频-逆文档频率指标，在很多自然语言处理应用中非常常用。我不会详细解释它的工作原理，但它本质上是一种作用于文本片段上的距离度量，编码了人类关于单词在不同文档中出现频率的知识。有趣的是，在研究论文上使用这种TF-IDF度量进行最近邻检索实际上能得到非常好的结果。

So if I look at the four nearest neighbors to my own most recent paper, then we see these four papers. They don't mean anything to you, but actually three of these were things that we directly compared against and cited. We really tried hard to make sure we beat them in order to get our paper published. Interestingly, the nearest neighbor here is something that we didn't cite, so I should maybe go back and read that one. The point here is that the nearest neighbor algorithm, even though it seems relatively simple.

如果我查看我自己最新论文的四个最近邻，我们会看到这四篇论文。它们对你来说可能毫无意义，但实际上其中三篇是我们直接对比并引用的论文。为了确保我们的论文能够发表，我们确实非常努力地想要超越它们。有趣的是，这里的最近邻是一篇我们没有引用的论文，所以也许我应该回去读一下那篇。这里的重点是，最近邻算法虽然看起来相对简单。

But the point here is that the nearest neighbor algorithm, even though it seems relatively simple, can be fairly powerful and can be applied to fairly robust and different types of data as you change the way that you compute distance between elements. So this is also a bit of fun. A couple years ago I wrote this interactive web demo in JavaScript that lets you produce these visualizations for nearest neighbor. You can go on this link and play around with this. You can interactively drag points around and see the decision boundaries move. You can change the number of categories. You can change the number of training points.

但这里的重点是，最近邻算法虽然看起来相对简单，但实际上相当强大，并且可以通过改变元素间距离计算方式应用于各种鲁棒且不同类型的数据。这也挺有趣的。几年前我用JavaScript写了一个交互式网页演示，可以让你生成最近邻的可视化效果。你可以访问这个链接并进行尝试。你可以交互式地拖动点并观察决策边界的移动。你可以改变类别数量。你可以改变训练点的数量。

You can change the number or the value of K for the nearest neighbors that we use, and you can try to flip back and forth between L1 and L2 metrics to try to get a sense of qualitatively what all these choices do and how they change the decision boundaries of your K&N classifier. So coding this thing off was like two days in my life, so I really hope someone looks at it some time. I think this can be a useful tool to help you gain a little bit of intuition into what this cannon house fire is.

你可以改变我们使用的最近邻的K值数量，并且可以尝试在L1和L2度量之间来回切换，以定性地了解这些选择会产生什么影响，以及它们如何改变你的K&N分类器的决策边界。编写这个程序花了我大约两天时间，所以我真的很希望有人能看看它。我认为这是一个有用的工具，可以帮助你对这种"炮火连天"的情况获得一些直观理解。

So coding this thing off was like two days in my life, so I really hope someone looks at it some time. I think this can be a useful tool to help you gain a little bit of intuition into what this cannon house fire is doing. By now we've seen a couple different choices that we have to make when performing this cannon classification. We've seen that apart from the training data, we need to choose a value of K, that is how many different neighbors are going to consider when doing this algorithm. We've also seen we need to choose the distance metric: should we use L1, L2, should we try to cook something up that incorporates our own domain knowledge.

编写这个程序花了我大约两天时间，所以我真的很希望有人能看看它。我认为这是一个有用的工具，可以帮助你对这种"炮火连天"的情况获得一些直观理解。到目前为止，我们已经看到了在执行这种"炮火"分类时必须做出的几个不同选择。我们已经看到，除了训练数据之外，我们需要选择一个K值，即在执行此算法时要考虑多少个不同的邻居。我们还看到需要选择距离度量：是使用L1、L2，还是尝试设计一些融入我们自己领域知识的方法。

And it's not really clear how we should set these for different problems, so these choices of K and of the distance metric are examples of what we call hyper parameters. A hyper parameter is a choice that we need to make in our learning algorithm that we cannot necessarily learn directly from the training data because they somehow interact with the way the algorithm works in a deep fundamental way. So these hyper parameters we can't really set them directly through learning, so we need some other mechanism to choose which values of hyper parameters are going to work best on our data.

对于不同问题应该如何设置这些参数并不明确，因此K值和距离度量的选择就是我们所说的超参数的例子。超参数是我们在学习算法中需要做出的选择，我们不一定能从训练数据中直接学习到这些参数，因为它们以某种深刻基本的方式与算法的工作方式相互作用。所以我们无法真正通过学习直接设置这些超参数，需要其他机制来选择哪些超参数值在我们的数据上效果最佳。

And unfortunately there's not a lot of great ways in practice to choose hyper parameters. The kind of simplest approach is that they're very problem dependent so we basically need to try out different values and see whatever is going to work best for our data and our task. But there's some nuance here in what exactly we mean by try out different values and what exactly I mean by decide which one works best. So here's a couple ideas for how we might try to go about setting hyper parameters. So idea number one would be maybe we should select the values of hyper parameters that will cause our learning algorithm to give us.

遗憾的是，在实践中并没有太多好方法来选择超参数。最简单的方法是它们与问题高度相关，因此我们基本上需要尝试不同的值，看看哪种方法对我们的数据和任务效果最好。但这里有一些细微差别，关于"尝试不同值"的确切含义以及"决定哪种方法效果最好"的确切含义。所以这里有几个关于如何设置超参数的想法。第一个想法可能是我们应该选择那些能让我们的学习算法给我们带来最佳结果的超参数值。

Give us the highest accuracy on our training set. This seems reasonable, right? We want our algorithm to do well. We have a training set. Training set is meant for training. We need to suit.

让我们在训练集上获得最高准确率。这似乎很合理，对吧？我们希望算法表现良好。我们有一个训练集。训练集就是用于训练的。我们需要适应。

And then maybe we should just set the hyper parameters to give us the best performance on the training set. So even though this seems reasonable, it's actually a terrible idea. Like never do this, just simply never do this. The reason is this can lead you very, very far astray. In a concrete example for K nearest neighbor classification, if you were to try to set hyper parameters by maximizing accuracy on the training set, you would always choose K equals 1. Right? Because imagine what happens if you use the training point in the KNN classifier. If you use K equals 1, it will try to find the nearest training point which is itself, and then it will always return the correct label. So KNN classifier with k equals one always gets 100% on the training set. But as we've seen some these qualitative examples that probably intuitively is not correct.

那么也许我们应该设置超参数，使其在训练集上给我们最佳性能。尽管这看起来合理，但实际上是个糟糕的主意。永远不要这样做，就是永远不要这样做。原因是这会让你误入歧途。以K近邻分类的具体例子来说，如果你试图通过最大化训练集上的准确率来设置超参数，你总是会选择K等于1。对吧？想象一下在KNN分类器中使用训练点时会发生什么。如果你使用K等于1，它会尝试找到最近的训练点，也就是它自己，然后总是返回正确的标签。所以k等于1的KNN分类器在训练集上总是获得100%的准确率。但正如我们看到的这些定性例子，直觉上这可能并不正确。

Because we've seen how maybe smoothing out the setting higher values of K can maybe cause decision boundaries to be smoothed out and we that actually might be the correct thing to do for some problems. But we'll never get to know that for looking at the training set accuracy only. So instead better idea is idea number two. Maybe what we need to do is split our data set into two components. One we're going to maybe reserve something like 90% of our data set and called up the training set and then reserve maybe 10% of our data.

因为我们看到设置较高的K值可能会使决策边界更加平滑，实际上这对某些问题可能是正确的做法。但仅通过观察训练集准确率我们永远无法知道这一点。因此更好的方法是第二个方案：也许我们需要将数据集分成两个部分。一部分我们可能保留约90%的数据集作为训练集，然后保留约10%的数据。

And call it the test set because again really the point of machine learning algorithm is to learn from the training set and then see what the accuracy is on the test set. As we vary the values of the hyper parameters, we'll choose the values of the hyper parameters that work the best on the test set. This is more reasonable because the point of using machine learning algorithms overall is to generalize to unseen data. We don't care about performance on the training set.

我们称之为测试集，因为机器学习算法的重点是从训练集中学习，然后查看测试集上的准确率。当我们改变超参数的值时，我们会选择在测试集上效果最好的超参数值。这更合理，因为使用机器学习算法的整体目的是泛化到未见过的数据。我们并不关心训练集上的表现。

To learn the learning algorithm because we already have those labels in our data set, we care about the performance on unseen data. Somehow this approach gives us some estimate of our algorithm's performance on data that it had not seen during training.

学习学习算法是因为我们的数据集中已有这些标签，我们关心的是在未见数据上的表现。这种方法某种程度上能让我们评估算法在训练期间未见过的数据上的表现。

So basically you're absolutely correct and even though I told you this seems very reasonable it seems very logical this is wrong and you should not do this. This is actually again equally as bad as training on the training set. If you do this you will have you will draw incorrect conclusions about the performance of your learning algorithm because basically what we've done in this approach is a different way of learning on the test set right because once you look at the test set your algorithm is polluted with knowledge of that test set. And if you are using the test set in any way to make decisions about your learning algorithm then you're cheating because again that then it pollutes your idea of how well that algorithm is we're going to perform on unseen data because once you use the test set to set values of your hyper parameters the test set is no longer unseen.

所以基本上你是完全正确的，尽管我告诉过你这看起来非常合理、非常符合逻辑，但这是错误的，你不应该这样做。这实际上又和在训练集上训练一样糟糕。如果你这样做，你会对你的学习算法性能得出错误的结论，因为基本上我们通过这种方法做的是在测试集上学习的另一种方式，因为一旦你查看了测试集，你的算法就被该测试集的知识污染了。如果你以任何方式使用测试集来为你的学习算法做决策，那么你就是在作弊，因为这再次污染了你对算法在未见数据上表现的判断，因为一旦你使用测试集来设置超参数值，测试集就不再是未见过的数据了。

Data and you no longer have any estimate. You no longer have any idea about how your algorithm is actually going to perform when you deploy it out there in the wild and run it on new images that did not appear in your data set at all. So even though this idea seems logical and seems plausible, this is a fundamental cardinal sin in machine learning models. If you do this, you're making a fundamental error in the way that you're preparing your model. So a much better approach is idea number three.

数据方面，你不再有任何评估依据，也不再对你的算法在实际部署到野外环境、处理数据集中从未出现过的新图像时的表现有任何概念。因此，尽管这个想法看起来合乎逻辑且似乎可行，但这在机器学习模型中是一个根本性的重大错误。如果你这样做，你在模型准备过程中就犯了一个根本性错误。所以更好的方法是采用第三个方案。

So here what we're going to do is split our data now into three sets. We're gonna have a training set that we use to train our algorithm. We're gonna have a validation set that we use to set the values of our hyper parameters. And then we'll reserve a test set to use only once at the very end.

所以我们要做的是将数据分成三个部分：一个用于训练算法的训练集，一个用于设置超参数值的验证集，以及一个仅在最后阶段使用一次的测试集。

Now so then basically what we do right is kind of this kind of similar is what we did before right we trained our algorithm on the test set we tried different values of hyper parameters we evaluate the performance of different hyper parameter values by checking the accuracy on the validation set now and now we select the value select the values of hyper parameters that have the highest performance on the validation set and now once you've chosen those hyper once you've chosen all the values for all of your hyper parameters once you've fixed everything then only once at the very very end of your pipeline do you ever touch the test set and then you touch it only once you run your algorithm exactly once on the test set and that gives you a single number.

那么基本上我们现在做的和之前类似：我们在测试集上训练算法，尝试不同的超参数值，通过在验证集上检查准确率来评估不同超参数值的表现，然后选择在验证集上表现最佳的超参数值。当你选定了所有超参数值并固定所有设置后，只有在流程的最后阶段才会接触测试集，而且仅接触一次——你在测试集上仅运行一次算法，这样就能得到一个具体的数值。

That now gives you a very proper estimate of your algorithms performance on truly unseen data. So even though this is the correct thing to do, it's actually completely terrifying in practice. When you're writing a research paper you've been working on this project for months, you've been working on this project for years and that entire time you've been tweaking your algorithm, you've been tuning it, you've been lovingly trying to improve it.

这能让你对算法在真正未见数据上的表现获得非常准确的评估。尽管这是正确的做法，但在实践中实际上相当令人担忧。当你撰写研究论文时，你可能已经在这个项目上工作了数月甚至数年，期间一直在调整算法、优化参数、精心改进它。

And throughout the entire process of developing your algorithm, you as a good machine learning practitioner have never touched the test set. You've only evaluated on the validation set. Then it's the week of the deadline. All of your hard work has finally come to fruition. It's finally time to see how well your algorithm actually does. A week before the deadline is the only time you should run it on the test set, even though this is terrifying. You think what if my number is bad? What if my entire life's work has been wasted on this algorithm?

在整个算法开发过程中，作为优秀的机器学习实践者，你从未接触过测试集。你只在验证集上进行评估。直到截止日期前一周，你所有的辛勤工作终于迎来收获的时刻。这时才终于能看到你的算法实际表现如何。尽管这令人忐忑不安，但截止日期前一周才是你应该在测试集上运行算法的唯一时机。你会想：如果结果不理想怎么办？如果我毕生的心血都浪费在这个算法上怎么办？

Well, if it turns out that you get a bad performance on the test set, that means your algorithm was bad and maybe it shouldn't be published. So this is actually very terrifying to work with in practice, but this is the correct way to do data hygiene in machine learning. Projects have been sunk by getting this wrong. If you get this wrong, you might get your papers accepted, but you'll be fundamentally dishonest about how well your algorithm performs.

如果在测试集上表现不佳，这意味着你的算法存在问题，可能不应该发表。因此，在实践中这实际上非常令人担忧，但这是机器学习中数据卫生的正确做法。项目曾因处理不当而失败。如果处理不当，你的论文可能会被接受，但你在算法实际表现上存在根本性的不诚实。

The basic trick was to split our dataset into three chunks, but we can do even better. Why stop there? We can split our dataset into ever more chunks and get ever better estimates of our generalization performance. That's idea number four called cross-validation.

基本策略是将数据集分成三部分，但我们可以做得更好。为何止步于此？我们可以将数据集分成更多部分，从而获得对泛化性能更准确的评估。这就是第四个理念——交叉验证。

We split our dataset into many different chunks called folds. In this example, we have five folds. We will try out five different versions of our algorithm. One version uses fold 5 as a validation set and trains on folds 1 through 4. Another uses fold 4 as a validation set and trains on folds 1, 2, 3, and 5, etc.

我们将数据集分成多个不同的块，称为折叠。在这个例子中，我们有五个折叠。我们将尝试五种不同版本的算法。一个版本使用折叠5作为验证集，并在折叠1到4上进行训练。另一个版本使用折叠4作为验证集，并在折叠1、2、3和5上进行训练，等等。

This is maybe even the best idea that we really should all be doing. Here the idea is we'll split our dataset into many different chunks called folds. Now what we're going to do is iterate through them and use them. In this example we have five folds, so we'll try out five different versions of our algorithm: one that uses fold 5 as a validation set and trains on folds 1 through 4, one that uses fold 4 as a validation set and trains on folds 1, 2, 3, 5, etc. Then what you can do is now you get a slightly more robust idea of how hyper parameters are going to perform on unseen data, because now you get maybe one sample per fold for each setting of the hyper parameters. Then maybe you select your best parameters using some metric, maybe the highest accuracy across all folds, something like that. This is probably the most robust way to choose hyper parameters.

这可能是我们真正都应该采用的最佳思路。这里的理念是将数据集分成多个不同的块，称为折叠。接下来我们要做的是遍历并使用这些折叠。在这个例子中我们有五个折叠，因此我们将尝试五种不同版本的算法：一个版本使用折叠5作为验证集并在折叠1到4上训练，另一个版本使用折叠4作为验证集并在折叠1、2、3和5上训练，等等。这样你就能对超参数在未见数据上的表现有更稳健的评估，因为现在每个超参数设置在每个折叠上都能获得一个样本。然后你可以使用某些指标选择最佳参数，比如在所有折叠上最高准确率之类的标准。这可能是选择超参数最稳健的方法。

But it's fairly expensive because it requires actually training our algorithm on many different folds of the data. Even though this is definitely the most correct thing you can do, in practice this doesn't typically get done in most machine learning projects just because the training can be very expensive for many of those models. But if you're using smaller models or smaller data sets, or if you can computationally afford to do it, then some kind of cross-validation is really the correct way to set hyper parameters for your machine learning models.

但这相当昂贵，因为它需要在数据的多个不同折叠上实际训练我们的算法。尽管这绝对是最正确的做法，但在实践中，大多数机器学习项目通常不会这样做，因为对许多模型来说训练成本可能非常高昂。但如果你使用较小的模型或较小的数据集，或者如果你在计算上能够承担这种开销，那么某种形式的交叉验证确实是设置机器学习模型超参数的正确方法。

So when you run cross-validation you end up getting a plot like this. On the x-axis is different values for one of our hyper parameters K, and on the y-axis we see each dot is one of the validation set performances for each of those different trials of the algorithm that we've run. Here's an example of five fold cross validation on K, and the line here gives the mean across all the folds for each setting of hyper parameters. So then we can see that it maybe peaks around K equals seven.

当你运行交叉验证时，最终会得到类似这样的图表。x轴代表我们超参数K的不同取值，y轴上的每个点代表我们运行的每个不同算法试验中验证集的性能表现。这是一个关于K的五折交叉验证示例，图中的线条给出了每个超参数设置下所有折叠的平均值。这样我们可以看到性能峰值大约出现在K等于7左右。

So based on this example of cross-validation, then K equals seven is the correct value to set for this type of parameter. Then we should run our model exactly once on a test set and that's the number we report for our algorithm. Another interesting feature of the K nearest neighbor algorithm is this property of universal approximation. What's really interesting is that K nearest neighbor actually makes very few assumptions about the types of functions that it can represent.

基于这个交叉验证的示例，K等于七是设置此类参数的正确值。然后我们应该在测试集上运行一次模型，这就是我们为算法报告的数字。K最近邻算法的另一个有趣特性是通用逼近特性。真正有趣的是，K最近邻实际上对其所能表示的函数类型做出了很少的假设。

So in fact as we take the number of training samples to infinity then K nearest neighbor can actually represent any function of course any is here with a mathematical asterisk because anytime you make statements like this people who've taken a real analysis course will start pointing out all the corner cases where it might fail so I've tried to cover myself a little bit here but basically for all practical algorithms after all practical functions you might encounter in nature you can expect this to work quite well.

事实上，当我们让训练样本数量趋近于无穷大时，K最近邻算法实际上可以表示任何函数——当然这里的"任何"需要加上数学上的星号注释，因为每当你做出这样的陈述时，学过实分析课程的人就会开始指出所有可能失败的极端情况，所以我在这里稍微给自己留了点余地。但基本上，对于所有实际算法以及你在自然界中可能遇到的所有实际函数，你可以预期这种方法会表现得相当好。

So as a kind of intuitive example of how this universal approximation property can work for k-nearest neighbors, here's an example of maybe doing a continuous valued prediction using a nearest neighbor approach. So here we maybe have a one pixel image, so just a single scalar, a single floating-point number is our input X, and now we want to predict a single floating-point number Y. Then the blue curve here shows some underlying true function that we want our machine learning model to learn, but we only have access to a finite number of data samples.

作为这种通用逼近特性如何在k最近邻算法中工作的直观示例，这里有一个使用最近邻方法进行连续值预测的例子。这里我们可能有一个单像素图像，所以只是一个标量，一个浮点数作为我们的输入X，现在我们想要预测一个浮点数Y。然后这里的蓝色曲线显示了我们希望机器学习模型学习的一些潜在真实函数，但我们只能访问有限数量的数据样本。

So here the black points represent this finite number of samples from this underlying true function. Now the green curve represents the value of a one nearest neighbor classifier, one nearest neighbor regressor I guess in this case. If we were to use this finite training sample to approximate this underlying true function using this finite number of training samples, and because it's a one nearest neighbor, we have sort of a flat constant region around each of the training samples and areas and discontinuities wherever it's exactly between two of the training samples. Now this example uses only five points for training.

这里的黑点代表了从这个潜在真实函数中采样的有限数量样本。现在绿色曲线表示的是一个最近邻分类器的值，在这种情况下应该是一个最近邻回归器。如果我们使用这个有限的训练样本来近似这个潜在的真实函数，由于它是一个最近邻算法，我们在每个训练样本周围都有类似平坦的常数区域，在任意两个训练样本之间的中点处会出现不连续性。这个例子仅使用五个点进行训练。

So the quality of our function approximation here is quite bad, but as we increase the number of training samples, you're doubling to 10, again doubling to 20, and now doubling and now going up again to 100, we can see that this one nearest neighbor classifier basically is doing a very very good job at approximating this underlying function, and you can imagine we're not going to go through a formal proof here but kind of intuitively speaking if you're able to kind of paper the space and kind of cover the entire training space with all of your data points.

因此，我们这里的函数逼近质量相当差，但随着我们增加训练样本的数量，从10个翻倍到20个，再翻倍并增加到100个，我们可以看到这个最近邻分类器基本上在逼近这个潜在函数方面做得非常好。你可以想象，我们不会在这里进行正式证明，但直观地说，如果你能够用所有数据点覆盖整个训练空间。

With enough data points, then your nearest neighbor algorithm, your nearest neighbor classifier, will actually learn some arbitrarily correct approximation of a true underlying function. So that seems to be really good news, right? All right, this nearest neighbor is maybe the only learning algorithm we need, right? It can represent any function. All we need to do is collect a lot of data. But there's a catch here, and that catch is called the curse of dimensionality. So the problem is that in order to get a kind of uniform coverage of the full space.

如果有足够的数据点，那么你的最近邻算法，你的最近邻分类器，实际上将学习到对真实潜在函数的任意正确近似。这看起来是个非常好的消息，对吧？好的，这个最近邻算法也许是我们唯一需要的学习算法，对吗？它可以表示任何函数。我们只需要收集大量数据。但这里有个问题，这个问题被称为维度灾难。问题在于，为了获得对整个空间的均匀覆盖。

Of a training set we need a number of training samples which is exponential in the dimension of the underlying space. So in the example from the previous slide our input space was only one dimensional. Then suppose that here maybe we don't need that many training samples to get a relatively dense coverage of a one dimensional space. Suppose we had a two dimensional space and we wanted to achieve a similar density in our training samples over a two dimensional space.

对于训练集，我们需要训练样本的数量与底层空间的维度呈指数关系。在前一张幻灯片的例子中，我们的输入空间只是一维的。那么假设在这里我们可能不需要那么多训练样本来获得一维空间相对密集的覆盖。假设我们有一个二维空间，并且我们希望在二维空间的训练样本中达到类似的密度。

Now we would need that now would now instead of meeting for examples in this in this example we know it would need something like 4 squared training samples and as we move to three dimensions we would now need 4 to the Q 4 to the power of 3 training samples to again achieve a similar density in in covering our space but also you think ok maybe this is ok we need a lot of data sure but the Internet's really big right maybe there's enough inner maybe there's enough images out there to cover the space of all visual things we might care about and this would be very wrong to assume.

现在我们需要的是，在这个例子中，我们知道需要大约4的平方个训练样本，当我们转向三维时，就需要4的3次方个训练样本，才能再次实现类似的密度来覆盖我们的空间。但你可能觉得这没问题，我们需要大量数据，互联网确实很大，也许有足够的内部资源，也许有足够的图像来覆盖我们可能关心的所有视觉事物的空间，但这种假设是非常错误的。

Now let's put some numbers on this. If we're imagining relatively low resolution images, something like CIFAR-10 images that are only 32 by 32 pixels, then the number of binary images that are 32 by 32 is 2 to the power of 32 times 32, which is about 10 to the 308. That's a pretty big number. To get a sense of just how big that number is, realize that the number of elementary particles in the visible universe is about 10 to the 97.

现在让我们用具体数字来说明这个问题。如果我们想象相对低分辨率的图像，比如只有32×32像素的CIFAR-10图像，那么32×32的二值图像数量是2的32×32次方，大约是10的308次方。这是一个相当大的数字。要理解这个数字有多大，要知道可见宇宙中的基本粒子数量大约是10的97次方。

What that means is that if we took our entire visible universe and we put a copy of our entire visible universe into every elementary particle in our universe and if our universe was then again a copy our universe was yet another elementary particle in some larger universe then every elementary particle in this entire massive collection of universes would be the same would be actually still less than the number of 32 by 32 binary images so this is not going to work right we can never collect enough data to densely cover the entire space of images.

这意味着，如果我们把整个可见宇宙放进每个基本粒子中，如果我们的宇宙本身又是某个更大宇宙中的一个基本粒子，那么这整个庞大宇宙集合中的每个基本粒子实际上仍然少于32×32二值图像的数量。所以这是行不通的，我们永远无法收集足够的数据来密集覆盖整个图像空间。

Because forget about 32 by 32, we want our algorithms to work on things that are much, much higher dimension. And we don't care about just binary images, we care about real-valued color images. So this is not going to work. And this is in fact one reason why, in fact, a nearest neighbor algorithm, even though nearest neighbors is this very nice algorithm to think about, in practice it's very rarely used on raw pixels for a couple of reasons. One, as we've seen, it's very slow at test time, and that's kind of the opposite of what we want from machine learning.

因为忘记32×32像素吧，我们希望算法能在更高维度的数据上工作。我们不仅关心二值图像，更关心实值彩色图像。所以这种方法行不通。这实际上也是最近邻算法虽然理论优美，但在实践中很少直接用于原始像素的原因之一。正如我们所见，首先它在测试时速度很慢，这与我们对机器学习的期望正好相反。

The other issue with learning systems is that they are very data hungry, and it is very difficult to get enough data to cover the space of all possible images. A third reason is that these distance metrics on raw pixel values are not very semantically meaningful. As a trivial example of this, if we look at this original image on the left and compute the L2 distance from the original image to each of these three perturbed images, we will find that the L2 distance is the same across all of these.

学习系统的另一个问题是它们非常依赖数据，并且很难获得足够的数据来覆盖所有可能图像的空间。第三个原因是这些基于原始像素值的距离度量在语义上并不十分有意义。举个简单的例子，如果我们观察左侧的原始图像，并计算原始图像与这三张扰动图像之间的L2距离，我们会发现所有图像的L2距离都是相同的。

We will find that the L2 distance is the same across all of these three pairs, and this is not very intuitive. The middle shifted image, for example, to us appears very similar to the original image on the left. So you might intuitively hope that any reasonable metric of comparing image similarity should say that the original image and the shifted image are very similar, while the boxed image or the tinted image should have a much larger distance. But unfortunately, these kinds of raw pixel-wise L2 or L1 metrics between raw pixels of images are just not very sensitive.

我们会发现这三对图像之间的L2距离完全相同，这并不十分直观。例如，中间平移后的图像在我们看来与左侧原始图像非常相似。因此，你可能会直觉地期望任何合理的图像相似性比较指标都应表明原始图像与平移后的图像非常相似，而带框图像或色调调整的图像应该有更大的距离。但不幸的是，这类基于原始像素的L2或L1距离度量并不敏感。

And is not able to capture these kind of perceptual or semantically meaningful distances between images. Even though nearest neighbor classifiers on raw pixel values do not work very well, it turns out somewhat surprisingly that one thing that does work quite well is nearest neighbors using feature vectors computed from deep convolutional neural networks. Over the next couple of weeks we'll talk about exactly how these might be computed.

无法捕捉这类感知或语义上有意义的图像距离。尽管基于原始像素值的最近邻分类器效果不佳，但令人惊讶的是，使用深度卷积神经网络计算的特征向量进行最近邻检索的效果却相当不错。在接下来的几周里，我们将详细讨论这些特征向量的具体计算方法。

But as just a hint here, here's an example of doing nearest neighbor retrieval using not the raw pixel values but instead using feature vectors computed for these images with a deep network. And what you can see is that now our nearest neighbors that we retrieve are quite semantically meaningful. So here we given a picture of a train, we're able to retrieve trains even they are from different viewpoints and different angles, and even have trains in different positions of the image. Or if we look at this image of a baby, we can see that we're able to retrieve other images of babies even though they look very different.

这里先给个提示，这是一个使用特征向量而非原始像素值进行最近邻检索的示例，这些特征向量是通过深度网络为图像计算得到的。可以看到，现在我们检索到的最近邻在语义上非常有意义。比如给定一张火车的图片，我们能够检索出不同视角、不同角度、甚至图像中不同位置的火车图片。或者如果我们看这张婴儿图片，可以看到即使其他婴儿图片看起来差异很大，我们仍然能够检索到它们。

But even though the raw pixel values are completely different, what I think is particularly interesting here is the example on the far right of the baby row where you can see that we've retrieved a baby which is actually rotated 90 degrees. So here the pixels of those two images are completely different, yet somehow the features computed by this deep network were able to bridge this semantic gap to some extent.

但尽管原始像素值完全不同，我认为特别有趣的是婴儿行最右侧的示例，可以看到我们检索到了一个实际上旋转了90度的婴儿图像。因此这两张图像的像素完全不同，但某种程度上这个深度网络计算出的特征能够在一定程度上弥合这种语义鸿沟。

And what's interesting here is that sometimes even though using nearest-neighbor on raw pixel values is not used that often, in fact using some nearest neighbor retrieval with convolutional neural network features is actually a very strong baseline for a lot of problems. So there was this very nice paper from a few years ago where they actually performed image captioning using a nearest neighbor approach. So here you know we've got a large data set of images and captions, we retrieved nearest neighbors using features computed from deep networks and we just returned the caption of the nearest neighbor from the training set. And even though this is a relatively simple nearest neighbor algorithm, it actually could give some pretty good captions.

有趣的是，尽管在原始像素值上使用最近邻方法并不常用，但实际上使用卷积神经网络特征进行最近邻检索对于许多问题来说是一个非常强大的基准方法。几年前有一篇很好的论文，他们使用最近邻方法实现了图像描述生成。这里我们有一个包含大量图像和描述的数据集，通过深度网络计算特征来检索最近邻，然后直接返回训练集中最近邻的描述。尽管这是一个相对简单的最近邻算法，但它实际上能给出相当不错的描述。

So it can direct you can say things like a bedroom with a bed and a couch on the upper left or a cat sitting in a bathroom sink on the upper right which maybe says more about the distribution of images of cats that people upload on the Internet this sort of suggests that there's a lot of examples of cat sitting and sinks in the training set but the point here is that even though nearest neighbor is maybe not the best thing to do on raw pixels you should actually definitely consider giving it a try for more complex problems using better features.

因此它可以指导你描述诸如左上角有床和沙发的卧室，或者右上角有猫坐在浴室水槽里的场景——这可能更多地反映了人们在互联网上传的猫咪图像分布，这似乎表明训练集中有很多猫坐在水槽里的例子。但这里的关键点是，尽管最近邻算法在原始像素上可能不是最佳选择，但对于更复杂的问题使用更好的特征时，你确实应该考虑尝试它。

So then to kind of summarize what we talked about today, we talked about this overall problem of image classification. We saw how it can be a building block for many other problems in computer vision. And then we saw the K nearest neighbor algorithm as our kind of first example of a learning algorithm, and that was simple enough for us to walk through the full flow of a learning pipeline in just this one lecture. We talked a bit about hyper parameters, we talked a bit about data hygiene on how to properly deal with your training and validation sets.

那么总结一下我们今天讨论的内容，我们讨论了图像分类这个整体问题。我们看到了它如何成为计算机视觉中许多其他问题的基础构建模块。然后我们介绍了K最近邻算法作为我们的第一个学习算法示例，这个算法足够简单，让我们能够在一节课中完整地走完整个学习流程。我们讨论了一些关于超参数的内容，也讨论了一些关于数据卫生以及如何正确处理训练集和验证集的内容。

So you'll get if you do the first if you now you have enough knowledge to go and do the first homework assignment which will be due over the weekend and then we can come back on Wednesday and start talking about our next learning algorithm about linear classifiers.

如果你现在完成第一个作业，你已经掌握了足够的知识去完成它，这个作业将在周末截止，然后我们可以在周三回来开始讨论我们的下一个学习算法——线性分类器。