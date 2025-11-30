---
title: 'Lecture1_Notes'
publishDate: 2025-11-22
description: 'TODO'
tags:
  - TODO
language: 'English'
heroImage: { src: './default.jpg', color: '#D58388' }
---

Welcome I hope y'all in the right place welcome to ECS four nine eight zero zero seven slash five five eight zero zero five this special topics class talk with a first-time here at Michigan departing for computer vision I wish we had a snappier easier more easy to remember course type course number but when you teach a special topics class they give you numbers like this so I'm sorry about that but hopefully you're all in the right place so the title of this class is deep learning for computer vision.

欢迎大家来到ECS 498007/558005这门专题课程。这是我在密歇根大学首次开设的计算机视觉课程。我希望我们能有一个更简洁、更容易记忆的课程类型和编号，但当你教授专题课程时，他们就会给你这样的编号，对此我很抱歉。但希望大家都来对了地方。这门课的标题是"深度学习在计算机视觉中的应用"。

So I think we need to unpack a little bit what these terms mean before we get started. Computer vision is the study of building artificial systems that can process, perceive and otherwise reason about visual data. This is quite a broad definition. What does process mean? What does perceive mean? What does reason mean? It's kind of up for interpretation. What is this visual data? That could be images, that could be videos, that could be medical scans, that could be just about any type of continuously valued signal you can think about.

我认为在开始之前，我们需要稍微解释一下这些术语的含义。计算机视觉是研究构建能够处理、感知并对视觉数据进行推理的人工系统。这是一个相当宽泛的定义。处理是什么意思？感知是什么意思？推理是什么意思？这在一定程度上取决于解释。什么是视觉数据？可能是图像，可能是视频，可能是医学扫描图像，可能是你能想到的任何类型的连续值信号。

Computer vision can sometimes be found in computer vision conferences or publications somewhere. So these terms are really defined quite broadly. Why is computer vision important? Well, I think computer vision is a particularly important and exciting topic to study because it's everywhere. I think many of us in this room right now are carrying around one or more cameras. We are just taking millions of photos every day. There are cameras all around us all the time. People are always creating visual data, sharing digital data, and talking about visual data.

计算机视觉有时会在计算机视觉会议或某些出版物中出现。因此这些术语的定义确实相当宽泛。为什么计算机视觉很重要？我认为计算机视觉是一个特别重要且令人兴奋的研究课题，因为它无处不在。我想在座的许多人现在都随身携带一台或多台相机。我们每天拍摄数百万张照片。我们周围始终都有摄像头。人们总是在创建视觉数据、分享数字数据并讨论视觉数据。

And this is very important that we build algorithms that can perceive, reason and process for a couple country statistics. If you look at YouTube, actually anomalous looking Instagramers, so Instagram is very popular, many of you are familiar with it. There's something like 100 million photos and videos uploaded on Instagram every single day. If we go on YouTube, it's even worse. On YouTube as of 2015, so I'm sure it's grown since then, people are uploading roughly 300 hours of video on YouTube every minute. So that means if you do the math and you think if I wanted to as a single individual human being look at all.

建立能够感知、推理和处理一些国家统计数据的算法非常重要。看看YouTube，实际上还有异常的Instagram用户，Instagram非常流行，你们很多人都熟悉它。每天大约有1亿张照片和视频被上传到Instagram。如果我们看YouTube，情况更糟。截至2015年，我相信自那以后这个数字还在增长，人们每分钟在YouTube上上传大约300小时的视频。这意味着如果你计算一下，想想如果我作为单个人类个体想要看完所有

Look at all the visual data just being uploaded on Instagram and YouTube in one day. If you do the math, say I'm going to look at images for maybe one second each. I'm going to look at my YouTube videos at double speed. It's going to take me about 25 years to look at the visual data that's going to be uploaded on just these two sites in a single day. So when you think about these massive statistics and think about the massive amount of visual data being processed and shared across the Internet these days, it becomes clear that.

看看一天内上传到Instagram和YouTube的视觉数据总量。如果计算一下，假设我每张图片只看一秒钟，以双倍速度观看YouTube视频。光是看完这两个平台一天内上传的视觉内容，就需要花费我大约25年时间。因此，当你思考这些庞大的统计数据，考虑到当今互联网上处理和共享的海量视觉数据时，就会清楚地认识到。

It becomes clear that we need to be able to build automated systems that deal with it because we just don't have the human manpower to look at, process and perceive all of the data that is created. That's why I think computer vision is such an important topic to be studying these days, and it's only going to get more important as the number of visual sensors out in the world keep increasing with new emerging technologies like autonomous vehicles, augmented and virtual reality, drones.

很明显，我们需要能够构建自动化系统来处理这个问题，因为我们根本没有足够的人力来查看、处理和感知所有产生的数据。这就是为什么我认为计算机视觉是当今非常重要的研究课题，而且随着世界上视觉传感器数量的不断增加，以及自动驾驶汽车、增强现实和虚拟现实、无人机等新兴技术的出现，它的重要性只会与日俱增。

You can imagine that the role of computer vision in our modern society will just continue getting more and more and more important. So clearly I'm is because this is my research area, but I think this is the most important and exciting research topic that we can be studying right now to your science. So that's computer vision. Computer vision is the problem that we're trying to solve. Its X force this problem of understanding digital data, but computer vision doesn't really care how we solve that problem. Our goal is just to stop just to crunch through all of those images and videos.

你可以想象，计算机视觉在我们现代社会中的作用只会变得越来越重要。很明显我这么说是因为这是我的研究领域，但我认为这是我们现在能为你们的科学研究的最重要和最令人兴奋的研究课题。这就是计算机视觉。计算机视觉是我们试图解决的问题。它的X力量是理解数字数据的问题，但计算机视觉并不真正关心我们如何解决这个问题。我们的目标只是停止只是处理所有这些图像和视频。

However we have the technique that we happen to be using in computer vision across the field these days is deep learning. So before we get to the report we get to deep learning. What is learning? Learning is the process of building artificial systems that can learn from data and experiences. Notice that this is somewhat orthogonal to the goals of computer vision. Computer vision just says we want to understand visual data we don't care how you do it. Learning is this separate problem of trying to.

然而我们目前在计算机视觉领域使用的技术是深度学习。所以在进入报告之前我们先了解深度学习。什么是学习？学习是构建能够从数据和经验中学习的人工系统的过程。请注意这与计算机视觉的目标有些正交。计算机视觉只是说我们想要理解视觉数据，我们不在乎你如何实现。学习是试图解决这个独立的问题。

Build artificial systems that can learn from data and experiences. Notice that this is someone worth bogging all to the goals of computer vision. Computer vision just says we want to understand visual data, we don't care how you do it. Learning is this separate problem of trying to build systems that can adapt to the data that they see and the experiences that they have in the world. And from the outside, it's not immediately clear why these two go together. But it turns out that in the last 10 to 20 years, we found that building learning based systems is very important for building many kinds of generalizable computer systems both in computer vision and across many areas artificial intelligence and computer science more broadly. So now when we think about deep learning, deep learning is then yet another subset of machine learning.

构建能够从数据和经验中学习的人工系统。注意这是值得为计算机视觉目标付出一切的重要领域。计算机视觉只表明我们想要理解视觉数据，我们不在乎你如何实现。学习是另一个独立的问题，旨在构建能够适应它们所见数据和在世界上所获经验的系统。从表面上看，这两者为何结合并不显而易见。但在过去10到20年间，我们发现构建基于学习的系统对于开发多种可泛化的计算机系统至关重要，无论是在计算机视觉领域，还是在整个人工智能和更广泛的计算机科学领域。因此当我们现在思考深度学习时，深度学习是机器学习的另一个子集。

Deep learning is sort of maybe a bit of a baby name, a bit of a buzzword. But my definition is that it's a type of learning. Deep learning consists of hierarchical learning algorithms with many layers, whatever that means in the context of Han, that are very loosely inspired by the architecture of the mammalian brain and some types of a million visual system.

深度学习或许有点像是一个新生名词，有点像是流行语。但我的定义是它是一种学习类型。深度学习包含具有多个层级的层次化学习算法，无论这在韩的语境中意味着什么，这些算法非常松散地受到哺乳动物大脑架构和某些类型的百万视觉系统的启发。

Now I know could I say loosely this is a thing that you'll often see people talk about in deep learning that it's how the brain learns or how the brain works. I think you should take any of these comparisons with a massive grain of salt. There's some very coarse comparisons between brains and neural networks that we use today but I think you should not keep them too seriously. So that I'm kind of stepping back a little bit from these two topics. Computer vision and machine learning both fall within the purview of the larger research field of artificial intelligence. So artificial intelligence is very general very broad.

我知道可以大致这样说，你经常会看到人们在深度学习中讨论这一点，说这就是大脑学习或工作的方式。我认为你应该对这些比较持保留态度。我们今天使用的大脑和神经网络之间存在一些非常粗略的比较，但我认为你不应该太认真对待它们。因此我在这两个主题上稍微退后一步。计算机视觉和机器学习都属于更广泛的人工智能研究领域。所以人工智能是非常通用和广泛的。

It's broadly speaking how can we build computer systems that can do things that normally people do. So that's kind of my definition. I think people will argue about what is and is not artificial intelligence, but I think we just want to build smart machines whatever that means to any of us. And I think there's clearly many different sub disciplines of artificial intelligence, but two of the most important clearly again in my biased opinion are computer vision teaching machines to see and machine learning teaching machines to learn. And these are the topics that we'll study in this class.

广义来说，我们研究的是如何构建能够完成通常由人类完成的任务的计算机系统。这就是我个人的定义。我认为人们会争论什么属于人工智能什么不属于，但我认为我们只是想要构建智能机器，无论这对我们每个人意味着什么。我认为人工智能显然有许多不同的分支学科，但根据我有偏见的观点，最重要的两个显然是计算机视觉——教机器看，以及机器学习——教机器学习。这些就是我们本课程将要学习的主题。

So then, where does deep learning fall in this regime? It would be a subset of machine learning and it intersects both computer vision and falls within the larger AI ground. I think it's important at the outset to understand that this class is going to focus on this section right in the middle, the intersection of computer vision, machine learning, and deep learning. To start out with this slide is because it's really easy to get caught up in the hype these days and think that computer vision is the only type of AI, deep learning is the only type of AI.

那么，深度学习在这个体系中处于什么位置呢？它将是机器学习的一个子集，同时与计算机视觉相交，并属于更广泛的人工智能领域。我认为在一开始就要理解，本课程将重点关注中间这个部分，即计算机视觉、机器学习和深度学习的交叉领域。从这张幻灯片开始是因为如今人们很容易陷入炒作，认为计算机视觉是唯一的人工智能类型，深度学习是唯一的人工智能类型。

Deep learning is the only type of computer vision, but I think none of these are true. There are many types of AI which have nothing to do with learning, nothing to do with deep learning. There are classical results about symbolic systems and other approaches to AI that are very different technically. There are areas of computer vision that do not use any machine learning or deep learning. So I love it even though the focus of this class will be the intersection of these different research areas. I just want you to keep.

深度学习是计算机视觉的唯一类型，但我认为这些都不正确。有许多类型的人工智能与学习无关，与深度学习无关。关于符号系统和其他人工智能方法有着经典的研究成果，这些在技术上非常不同。有些计算机视觉领域根本不使用任何机器学习或深度学习。尽管本课程的重点将是这些不同研究领域的交叉点，我仍然非常喜欢它。我只是希望你们保持。

I just want to keep in mind as a whole that there is a much broader realm of AI research being done around the world by different groups that falls into different pieces of this pipeline. Of course, there are many other areas within AI that we won't talk about too much, so there's natural language processing, things like speech recognition, things like robotics. I kind of ran out of space on the chart with many more sub-areas, but suffice to say artificial intelligence is a massively successful, massively popular area of research and study.

我只想让大家整体记住，全球不同团队正在进行的人工智能研究领域要广泛得多，这些研究分别属于这个流程管道的不同环节。当然，人工智能领域还有许多其他方面我们不会过多讨论，比如自然语言处理、语音识别、机器人技术等。我在图表上已经没有空间列出更多子领域，但足以说明人工智能是一个极其成功、极其热门的研究领域和学习领域。

These days that again with the broad goal of making machines do things that people normally do. You can imagine that there's a whole lot of things that we might do out in the world that fall under this umbrella of our different intelligence. So that's kind of the big picture roadmap. And now for the route for the rest of the semester we're gonna focus on this little red area in here. But again don't forget that there's a lot more to the world than what we're talking about in this class.

如今我们再次以让机器完成人类通常所做事情这一广泛目标出发。你可以想象世界上有很多我们可能做的事情都属于不同智能范畴。这就是大致的发展路线图。本学期接下来的课程中，我们将重点讨论这个红色小区域。但请再次记住，世界远比我们在这门课程中讨论的内容更加广阔。

So today's agenda is a little bit different from most of the lectures in this class because again it is the first week. So before we can really dive into that red piece of the pie chart and talk about machine learning and deep learning and computer vision all that really good stuff, I think it's important to get a little bit of historical context about how we got here as a field. This has been a hugely successful research area in the last five to ten years but deep learning machine learning in computer vision these are areas with decades and decades of research built upon them.

今天的议程与本课程大多数讲座略有不同，因为这是第一周。在我们真正深入探讨饼图中红色部分，讨论机器学习、深度学习和计算机视觉等精彩内容之前，我认为了解一些关于我们作为该领域如何发展至今的历史背景非常重要。虽然在过去五到十年间这是一个极其成功的研究领域，但深度学习、机器学习和计算机视觉这些领域都是建立在数十年研究积累之上的。

And all of the successes we've seen in the last few years have been a result of building upon decades of prior research in these areas. So today I want to give a bit of a brief history and overview of someone who puts historical context that let up with the successes of today. And then following that we need to talk about some of the boring stuff of course overview logistics all that other stuff that you expect to see in the first election class.

我们近年来取得的所有成功，都是建立在数十年相关领域研究基础之上的成果。因此今天我想简要回顾历史并概述那些为今日成就奠定历史背景的人物。随后我们需要讨论一些常规内容，包括课程概述、后勤安排等你们期待在第一堂课上了解的所有信息。

So let's start with this in two ways. We're going to do a parallel stream. First we're going to talk about the history of computer vision and we're going to switch a little bit and we'll cover the history of deep learning. So before we dive into the material, any sort of questions before we launch into this historical scape? Perfectly clear. So if we go, I think whenever you talk about a research area it's always difficult to pinpoint the start right because everything builds on everything else there's always prior work.

让我们从两个方面开始。我们将采用并行方式。首先我们将讨论计算机视觉的历史，然后稍微转换一下话题，我们会涵盖深度学习的历史。在我们深入探讨具体内容之前，在开始这段历史回顾之前有什么问题吗？完全清楚。那么我认为，每当谈论一个研究领域时，总是很难确定起点，因为一切都是相互建立的，总有前人的工作基础。

Everyone was inspired by something else that came before. But with a finite amount of time to talk about a finite number of things, you got to cut the line somewhere. So one place where I like to draw the line and point to as maybe the start of computer vision is actually not with computer scientists at all. And it happened with this seminal study of Hubel and Wiesel back in 1959, who were not interested in computers at all. They wanted to understand how the mammalian brains work. So what they did is they got a cat.

每个人都受到前人事物的启发。但由于时间有限，只能讨论有限的内容，必须有所取舍。因此，我喜欢将计算机视觉的起点划在并非计算机科学家研究的领域。这要追溯到1959年休伯尔和威塞尔的开创性研究，他们当时对计算机毫无兴趣，只想理解哺乳动物大脑的运作机制。于是他们找来一只猫。

They got an electrode. They put the electrode into the brain of the cat into the visual cortex of the cat, just the part in the back of your head that processes visual data. With this electrode, they're able to record the neuronal activity of some of the individual neurons in the cat's visual cortex. With this somewhat grotesque experimental setup, they were able to have the cat watch TV. Not really TV because it was 1950 time, but they were able to show different sorts of slides to the cat.

他们使用了一个电极。他们将电极植入猫的大脑，植入猫的视觉皮层，也就是你头部后侧处理视觉数据的区域。通过这个电极，他们能够记录猫视觉皮层中某些单个神经元的神经活动。通过这个有些怪异的实验装置，他们让猫观看电视。因为那是1950年代，所以并非真正的电视，但他们能够向猫展示各种不同的幻灯片。

They cash in and with they had this general hypothesis that maybe there's certain neurons in the brain that responds different types of visual stimuli. By showing the cap different types of visual stimuli and recording the neural activity from individual neurons, maybe we can start to puzzle out how this thing called vision works at all. So that's exactly they did they got these cats they stuck neurons in their brains and they started showing a bunch of different images on a slideshow to try to see what kinds of images would activate the neurons and cats brains.

他们基于一个普遍假设展开研究：或许大脑中存在某些神经元会对不同类型的视觉刺激产生反应。通过向猫展示不同类型的视觉刺激并记录单个神经元的神经活动，或许我们就能开始解开这个被称为视觉的东西究竟如何运作的谜题。这正是他们所做的——他们找来这些猫，将电极植入它们的大脑，然后开始在幻灯片上展示各种不同图像，试图观察哪些类型的图像会激活猫脑中的神经元。

So they tried different things. You can show them mice and fish and other kinds of things that cats like to eat or play with, but it was really hard to get any solid signal about what these neurons were responding to. One really interesting discovery happened when using natural slide projectors back in the day. When you change the slide, there's kind of a vertical bar that would move up and down the screen, and what they surprisingly found is that some of the neurons in the cat's brain consistently.

他们尝试了不同的方法。你可以给猫展示老鼠、鱼和其他它们喜欢吃或玩耍的东西，但很难获得关于这些神经元对什么产生反应的可靠信号。一个真正有趣的发现在使用自然幻灯片投影仪时出现。当你更换幻灯片时，会有一个垂直条在屏幕上上下移动，他们惊奇地发现猫大脑中的一些神经元持续地。

Which consistently responds to the time when they change the slides. And they even though they couldn't recognize any patterns of what was how it was the cat responding to things on the slides. And they eventually discovered that it was in fact this moving vertical bar that was indeed causing some of the neuronal activity in the cat's brain. So with this hint they were able to puzzle out that there are certain that there are different types of cells in the brain that are responding to different types of visual stimuli.

它始终对更换幻灯片的时间做出反应。尽管他们无法识别出猫对幻灯片上事物反应的具体模式。他们最终发现，实际上正是这个移动的垂直条引起了猫大脑中的部分神经元活动。通过这个线索，他们得以推断出大脑中存在不同类型的细胞，这些细胞对不同类型的视觉刺激产生反应。

Many of them are very hard to interpret, but some of the easiest are these so-called simple cells that they discovered. The simple cells would respond to an edge that's maybe light on one side and dark on another side at a particular orientation at a particular position in the cat's visual field. If there happened to be an edge at the right position on the right angle in the right place, then that particular neuron might fire. That was very exciting because they may have some concrete evidence of what it is that cats are actually responding to in their brains.

其中许多很难解释，但其中最简单的是他们发现的这些所谓的简单细胞。简单细胞会对猫视野中特定位置、特定方向的明暗边缘做出反应。如果恰好在正确位置、正确角度、正确地点出现边缘，那么特定的神经元就可能产生兴奋。这非常令人兴奋，因为他们可能获得了具体证据，表明猫的大脑实际在响应什么。

Then with a bit more exploration they remember to find other types of cells in the brain that responded to even more complex patterns like the complex cells that would respond to bits of motion or could respond to orienting edges but anywhere in the visual appeal to give a sense of some sense of translation and Berryman's in the visual representations that they perceive.

通过进一步探索，他们发现大脑中还存在其他类型的细胞，这些细胞能对更复杂的模式产生反应，比如能对运动片段产生反应的复杂细胞，或能对定向边缘产生反应的细胞，这些细胞在视觉区域的任何位置都能提供某种平移感，以及贝里曼在视觉表征中的发现。

So I think that this is really one of the bounding. Oh, and by the way, of course I have to mention that these guys, this was very seminal research and these guys won the Nobel Prize for it in 1981. So this was a very important research in history of science and psychology and vision overall. But I like to point to this as the beginning of computer for a couple reasons. One is this emphasis on oriented edges will see this come up over and over again on the different architectures that we study throughout the semester. On the other is this hierarchical representation of the visual system of building from simple cells that represent one thing combining with complex cells and more and more complex cells that respond to more and more complex types of.

我认为这确实是边界检测的重要基础。顺便提一下，我当然必须指出这些研究人员，他们的工作具有开创性意义，并在1981年因此获得了诺贝尔奖。所以这在整个科学史、心理学和视觉研究领域都是非常重要的研究。但我喜欢将其视为计算机视觉的开端，原因有几个：一是这种对定向边缘的重视，在我们本学期研究的不同架构中会反复出现；二是视觉系统的这种层级化表征，从表示单一特征的简单细胞开始，与复杂细胞结合，再到响应更复杂特征的更复杂细胞。

That respond to more and more complex types of visual stimuli. This broad idea was hugely influential on the way that people thought about visual processing and even on neural representations more generally. So then if we move forward a couple years in 1963, Larry Roberts then his that's when Larry Roberts graduated from MIT PhD and did perhaps what was the first PhD thesis on computer vision. Here of course it was 1963 doing anything with computers was very cumbersome doing anything with digital cameras was very cumbersome so large portions of his thesis just to talk about how do you actually get photographic information into the computer.

对越来越复杂类型的视觉刺激做出反应。这个广泛的概念对人们思考视觉处理的方式，甚至对更广泛的神经表征都产生了巨大影响。那么如果我们向前推进几年到1963年，拉里·罗伯茨那时刚从麻省理工学院获得博士学位，并完成了可能是计算机视觉领域的第一篇博士论文。当然在1963年，用计算机做任何事情都非常麻烦，用数码相机做任何事情也非常麻烦，所以他论文的大部分内容只是讨论如何实际将摄影信息输入计算机。

That respond to more and more complex types of visual stimuli. Because this was not something you could take for granted at that time. But even working through those constraints, he built some system that was able to take this raw visual picture, detect some of the edges in the picture, sort of inspired by Hubel and Wiesel's discovery that edges were fundamental to visual processing. Then from there detect feature points, and then from there start to understand the 3D geometry of objects and images. Now what's really interesting is that if you go and look at Larry Roberts' Wikipedia page.

能够响应越来越复杂类型的视觉刺激。因为这在当时并不是理所当然的事情。但即使在那些限制条件下工作，他仍然构建了一些系统，能够获取这些原始视觉图像，检测图像中的某些边缘，某种程度上受到休伯尔和威塞尔发现的启发——边缘是视觉处理的基础。然后从那里检测特征点，再从那里开始理解物体和图像的三维几何结构。现在真正有趣的是，如果你去查看拉里·罗伯茨的维基百科页面。

It actually doesn't mention any of this at all because after he finished his PhD he went on to become the founding father of the internet and went on to be a hugely major player in the World Wide Web and all of the networking technologies that were developed around that time. So doing the first PhD thesis in computer vision was kind of a low point in his career. I think all of us can aspire to that success.

实际上完全没有提及这些内容，因为在他完成博士学位后，他继续成为互联网的奠基人，并在万维网以及当时开发的所有网络技术中扮演了极其重要的角色。因此，完成计算机视觉领域的首篇博士论文在他职业生涯中反而算是一个低谷。我想我们所有人都能向往那样的成功。

So then moving forward a couple more years, people are getting really excited. There was this very famous study in 1966 from MIT. A similar pack word proposed the summer computer vision project. The summer computer vision project basically what he wanted to do is like, we've got digital cameras now, they can detect edges, we know how all those cubed Wiesel told us how the brain works. What we're gonna do is hang a couple undergraduates, put them to work over the summer, and after the summer we show it we should be able to construct a significant portion of the visual system. Man, these guys are really ambitious back in the day.

那么再往前推进几年，人们变得非常兴奋。1966年麻省理工学院有一项非常著名的研究。类似的课题提出了夏季计算机视觉项目。夏季计算机视觉项目基本上他想做的是，我们现在有了数码相机，它们能够检测边缘，我们知道所有那些立方体维塞尔告诉我们大脑是如何工作的。我们要做的是找几个本科生，让他们在夏天工作，夏天结束后我们展示成果，我们应该能够构建视觉系统的很大一部分。天啊，那时候这些人真是雄心勃勃。

Because now it's clearly that computer vision is not solved. They did not achieve this. A lot of people and nearly 50 years later we're still plugging away trying to achieve this what they thought they could do in the summer with my brother. So moving forward into the 1970s one hugely influential figure in this era was Tamar who proposed this idea of stages of visual representation then again kind of harkens back to Google and reasonable so here you can see that maybe we want the input image then we have another prop another stage of visual pops.

因为现在很明显计算机视觉尚未解决。他们未能实现这一目标。许多人以及近50年后我们仍在努力实现他们以为能在夏天和我兄弟完成的目标。进入1970年代，这个时代极具影响力的人物是塔马尔，他提出了视觉表征阶段的概念，这又有点回溯到谷歌和合理的范畴，所以在这里你可以看到我们可能需要输入图像，然后还有另一个视觉呈现阶段。

And we extract edges then from the edges we extract some kind of depth information that maybe beacon segment objects and say which parts of image belong to which two different types of objects and then think about the relative depths of those objects and then eventually start to reason about whole 3D models of the world and of the scene.

我们提取边缘，然后从边缘中提取某种深度信息，可能是信标分割对象，并确定图像的哪些部分属于哪两种不同类型的对象，然后思考这些对象的相对深度，最终开始推理整个世界的三维模型和场景的三维模型。

And then we'll be bored of the seventies. People started to become interested in recognizing objects and thinking about ways to build computer systems that could not just detect edges and simple geometric shapes but more complex objects like people and bombs. There was work about some things like generalized cylinders and pictorial structures that tried to recognize people as easy formal configurations of rigid parts with some kind of known topology. You can see ideas and this was very influential work at a time. But the problem is that in the nineteen seventies processing power was very limited and visual cameras were very limited so a lot of this stuff was sort of toy in a sense. As we move into the 80s, people had much more.

然后我们会厌倦七十年代。人们开始对识别物体产生兴趣，并思考构建计算机系统的方法，这些系统不仅能检测边缘和简单的几何形状，还能识别更复杂的物体，如人和炸弹。有一些关于广义圆柱体和图像结构的工作，试图将人识别为具有某种已知拓扑结构的刚性部件的简单形式配置。你可以看到这些想法，这在当时是非常有影响力的工作。但问题是在二十世纪七十年代，处理能力非常有限，视觉相机也非常有限，所以很多这些东西在某种意义上有点像玩具。随着我们进入80年代，人们拥有了更多的资源。

Visual cameras were very limited, so a lot of this stuff was sort of toy in a sense. As we move into the 80s, people have much more access to better digital cameras and more computational power. People began to work on slightly more realistic images. One kind of theme in the 80s was trying to recognize objects and images via edge detection. I told you that edges were going to be super influential throughout the history of computer vision. There was a very famous paper from John Canny in 1986 that proposed the very robust algorithm for detecting edges and images. Then David Lowe the next year in 1987 proposed the mechanism for recognizing objects images by matching their edges.

视觉相机非常有限，所以这些东西在某种意义上有点像玩具。随着我们进入80年代，人们更容易获得更好的数码相机和更强的计算能力。人们开始研究更逼真的图像。80年代的一个主题是尝试通过边缘检测来识别物体和图像。我告诉过你们，边缘在计算机视觉的整个历史中将会非常有影响力。约翰·坎尼在1986年发表了一篇非常著名的论文，提出了用于检测边缘和图像的非常鲁棒的算法。然后大卫·洛在1987年提出了通过匹配边缘来识别物体图像的机制。

So in this example, you can imagine we've got this cluster of razors and then we detect the edges. Then maybe we have some template or picture of a razor that we know about, then we can detect the edges of our template razor and try to match it into this image, this cluttered image. There's a menu of all the razors and then by kind of matching edges in this way, you might be able to recognize that there are many ten razors in this image and what are their relative configurations just based on matching with our template image.

在这个示例中，你可以想象我们有一个剃须刀集群，然后我们检测边缘。接着我们可能有一个已知的剃须刀模板或图片，然后我们可以检测模板剃须刀的边缘，并尝试将其匹配到这个杂乱图像中。这里有一个所有剃须刀的菜单，通过这种边缘匹配的方式，你可能能够识别出图像中有许多十个剃须刀，以及它们基于模板图像匹配的相对配置。

And now I'm moving on into the 1990s. People again wanted to build more and more complex images, more and more complex scenes. So here a big theme was trying to recognize objects via grouping. So here rather than maybe just matching the edges, what we want to do is take the input image and segment it into semantically meaningful chunks. Maybe we know that the person is composed of one meaningful chunk, the different umbrellas would be composed of a different meaningful chunk with the idea that if we can first do.

现在我进入20世纪90年代。人们再次希望构建越来越复杂的图像，越来越复杂的场景。因此这里的一个重要主题是通过分组来识别物体。这里不仅仅是匹配边缘，我们想要做的是获取输入图像并将其分割成语义上有意义的块。也许我们知道人物由一个有意义的部分组成，不同的雨伞将由不同的有意义部分组成，基于这样的理念：如果我们能首先完成。

With the idea that if we can first do some sort of grouping then later Dallas tree and recognizing or giving a label to those groups might be an easier problem. Then in the 2000s a big theme was recognition via matching and this is a there was a hugely famous paper called SIFT by David Lowe by David Lowe again in 1999 that proposed a different way of recognition via matching. So here the idea is that we would take our input image detect little recognizable key points and different position 2D positions in the image and have each of those key points.

基于这样的想法：如果我们能先进行某种分组，之后再进行达拉斯树分析，识别或给这些组赋予标签可能会更容易。然后在2000年代，一个重要的主题是通过匹配进行识别，这是一篇非常著名的论文，由David Lowe在1999年再次提出的SIFT，提出了一种不同的通过匹配进行识别的方法。所以这里的想法是，我们将获取输入图像，检测图像中不同位置的可识别关键点和不同的2D位置，并对每个关键点进行处理。

We're going to represent its appearance using some kind of feature vector and that feature vector is going to be a real valued vector that somehow encodes the image at that little point in space. Through very careful design of exactly how that feature vector is computed, you can encode different types of invariances into that feature vector such that if we were to take the same image and rotate it a little bit or brighten or darken the lighting conditions in the scene a little bit, hopefully we would compute the same value for that feature.

我们将使用某种特征向量来表示其外观，该特征向量将是一个实值向量，它以某种方式编码空间中小点处的图像。通过精心设计特征向量的具体计算方式，您可以将不同类型的不变性编码到该特征向量中，这样如果我们对同一图像进行轻微旋转或稍微调亮或调暗场景中的光照条件，希望我们能够计算出相同的特征值。

Vector even if the underlying image were to change a little bit and there was a lot of work in it. Once we can extract these sets of robust and invariant feature vectors then you can improve again perform some kind of recognition via matching. So that on the left if we have some template image of a stop sign we can detect all these distinctive invariant feature key points then on the right if we have another image of a stop sign this may be taken from a different angle with different lighting.

即使底层图像发生轻微变化，并且在这方面已经做了大量工作。一旦我们能够提取这些鲁棒且不变的特征向量集合，就可以通过匹配再次改进执行某种识别。这样在左侧如果我们有停止标志的模板图像，我们可以检测所有这些独特的不变特征关键点，然后在右侧如果我们有另一个停止标志的图像，这可能是在不同光照条件下从不同角度拍摄的。

It's been conditions then by a careful clever design of these invariant robust features then we can match and then correspond points in the one image into points in the other image and thereby be able to recognize that the right image is indeed a stop sign. So then another hugely influential work in the 2000s was the Viola Jones algorithm published in 2001 and this was really and they developed a very very powerful algorithm for recognizing faces in images.

通过精心设计这些不变鲁棒特征，我们能够匹配并对应一幅图像中的点到另一幅图像中的点，从而能够识别出右侧图像确实是一个停止标志。因此，2000年代另一项极具影响力的工作是2001年发表的Viola Jones算法，他们开发了一个非常强大的算法来识别图像中的人脸。

So here they would, you know, you have an image then you want to draw a box where all the people's faces are. This was notable for this piece of work was notable for a couple reasons. One, it was the first major use of machine learning and computer vision. So Viola and Jones used some algorithm called the boosted decision trees that were able to learn somehow the right combination of features to use in order to recognize images they were to recognize faces. What was particularly notable was the very fast commercialization of this algorithm that this piece of research went very quickly from a sort of academic piece of research publishing 2001.

这里的情况是，你有一张图片，然后你想要在所有人物面部的位置绘制方框。这项工作的显著之处有几个原因。首先，这是机器学习和计算机视觉的首次重大应用。Viola和Jones使用了一种称为增强决策树的算法，该算法能够以某种方式学习使用正确的特征组合来识别图像中的面部。特别值得注意的是该算法的快速商业化，这项研究从2001年发表的学术研究迅速转化为实际应用。

And within a few years this was actually being shipped in digital cameras at the time. So if you remember maybe they had like an autofocus feature or you would kind of hold the shutter half down and it would like beep a little bit and draw boxes on the faces and then focus on the people in the scene. Well that was most likely using this Viola Jones algorithm. So this is a particularly notable piece of work for those two reasons.

在几年之内，这项技术实际上就被应用到了当时的数码相机中。如果你还记得，可能它们有自动对焦功能，或者你半按快门时它会发出一点提示音，在人脸上绘制方框，然后对场景中的人物进行对焦。这很可能就是使用了这种Viola Jones算法。由于这两个原因，这是一项特别值得关注的工作成果。

So now after they kind of unlocked the box of using data and using machine learning to augment our visual representations, then moving on into the 2000s we began to see more and more uses of machine learning, more and more uses of data in order to improve our visual recognition systems. So one hugely influential piece of work here was the Pascal visual object challenge. So here they download a bunch of them because now it's 2000 layered Roberts had excellent computer vision and invented the internet so we could then download images from the internet to help build these datasets images.

现在，在他们打开了使用数据和机器学习来增强我们视觉表现的大门之后，进入2000年代，我们开始看到越来越多机器学习的应用，越来越多数据的应用，以改进我们的视觉识别系统。这里一项极具影响力的工作是Pascal视觉对象挑战赛。在这里他们下载了大量图像，因为现在是2000年，分层罗伯茨拥有出色的计算机视觉并发明了互联网，这样我们就可以从互联网下载图像来帮助构建这些数据集图像。

And then we could get graduate students to go and label those images, and then we could use your machine learning algorithms to mimic the labels that the graduate students have written down for the images. If you do that, then you can see this nice curve on the right: performance increasing on this recognition challenge increased steadily over time from about 2005 to 2011. This brings us to the ImageNet Large Scale Visual Recognition Challenge. This was a very large scale dataset in computer vision that has become hugely influential and has become one of the main benchmarks in computer vision.

然后我们可以让研究生去标记这些图像，然后我们可以使用您的机器学习算法来模仿研究生为图像写下的标签。如果您这样做，那么您可以看到右侧这条漂亮的曲线：在这个识别挑战上的性能从2005年到2011年稳步提升。这让我们想到了ImageNet大规模视觉识别挑战。这是计算机视觉中一个非常大规模的数据集，已经变得极具影响力，并成为计算机视觉的主要基准之一。

So here this was a very very large scale data set in computer vision that has become hugely influential and has become one of the main benchmarks in computer vision even leading up to this day. The image in that classification challenge was this fairly large data set of more than about 1.4 million images and each of those 1.4 million images were labeled with one of a thousand different category labels. The big new piece of innovation here was that if you kind of do the math here you gonna be a lot of graduate students label all this stuff so the big piece of innovation here was to not label data using graduate students instead this made use of crowdsourcing.

这是一个在计算机视觉领域非常有影响力的大规模数据集，至今仍是该领域的主要基准之一。该分类挑战中的图像数据集相当庞大，包含超过约140万张图像，每张图像都被标注为1000个不同类别标签中的一个。这里的重要创新在于，如果按照传统方式计算，需要大量研究生来完成所有这些标注工作，因此这里的重大创新是不再使用研究生来标注数据，而是采用了众包的方式。

So here you could go on services like Amazon Mechanical Turk and then parcel up little pieces of work and then blast them out over there over the Internet. And then people anywhere in the world could label machine learning images, get paid a couple of cents for each image they label, and that was able to massively increase those amounts. I mean this is beneficial in two ways right, because one researcher gets people to label their data without being constrained by the number of graduate students that they have, and two it becomes a nice source of.

这样你就可以使用像亚马逊土耳其机器人这样的服务，将工作分割成小块任务，然后通过互联网发布出去。世界各地的任何人都可以为机器学习图像进行标注，每标注一张图片就能获得几美分的报酬，这大大增加了数据标注量。我认为这有两个好处：一方面研究人员可以让人们标注数据，而不受研究生人数的限制；另一方面这成为了一个很好的资源来源。

And then it became a nice source of income for some people who were bored at work and just like this or in classes that gets maybe or not. It's still weird. It's the running by the lady that grew up introducing tasks but anyway. This became a hugely influential data set in computer vision and more than a data set it became a benchmark challenge. So they had every year they ran a competition when they were different researchers would compete and try to build their own algorithms that would try to recognize objects in this limit in this in his classification challenge.

然后它成为了一些在工作时感到无聊或在上课时可能喜欢这样做的人的不错收入来源。这仍然很奇怪。这是由那位从小介绍任务的女士运营的，但无论如何。这成为了计算机视觉中极具影响力的数据集，而且不仅仅是一个数据集，它成为了一个基准挑战。所以他们每年都会举办一次竞赛，当时不同的研究人员会竞争并尝试构建自己的算法，试图在这个分类挑战中识别物体。

And this became somewhat jokingly known sometimes as the Olympics of computer vision. There was a period of time from maybe the mid-2010s to the late 2000s early 2010s when people would just really excitedly want to look at the results of ImageNet competition every year and see what kind of advances the field had made last year. Given that I told you about this competition, you can look at what was the error rate on this competition moving over time. The first time the competition was ran in 2010.

这有时被戏称为计算机视觉界的奥林匹克竞赛。从大约2010年代中期到2000年代末2010年代初的这段时间里，人们每年都会非常兴奋地关注ImageNet竞赛的结果，看看该领域去年取得了哪些进展。既然我已经告诉了你这个竞赛，你可以看看这个竞赛的错误率随时间的变化情况。该竞赛首次举办是在2010年。

In 2011 we were sitting in error rates of around 28%, around 25%. And then something big happened in 2012. So at the 2012 ImageNet competition, suddenly the error rates dropped in a single year from 25% all the way down to 16%. And after 2012, errors just kept on diminishing, diminishing very, very fast. Such that once we got to about 2017, we were building systems that could compete on this ImageNet challenge and perform even better than humans when they try to recognize images in this data set.

2011年我们的错误率大约在28%左右，约25%。随后在2012年发生了重大突破。在2012年ImageNet竞赛中，错误率在一年内从25%骤降至16%。2012年之后，错误率持续快速下降，下降速度非常迅猛。到2017年左右，我们开发的系统已能在ImageNet挑战中与人类竞争，并且在该数据集图像识别任务上的表现甚至超越了人类。

So then the question is what happened in 2012. What happened in 2012 is that deep learning came onto the scene and this was really the breakthrough moment for deep learning. Computer vision researchers suddenly woke up and saw that there's this crazy new thing that is suddenly sweeping our field. In 2012 there was this absolutely seminal paper from Alex Krizhevsky, Ilya Sutskever and Geoff Hinton where they proposed that deep convolutional neural network AlexNet that just crushed everyone else at the ImageNet competition. For people working in computer vision at that time this was shocking.

那么问题就是2012年发生了什么。2012年发生的是深度学习登上了舞台，这确实是深度学习的突破性时刻。计算机视觉研究人员突然觉醒，看到这个疯狂的新事物正在席卷我们的领域。2012年有一篇极具开创性的论文，由Alex Krizhevsky、Ilya Sutskever和Geoff Hinton发表，他们提出了深度卷积神经网络AlexNet，在ImageNet竞赛中彻底击败了所有其他参赛者。对于当时从事计算机视觉工作的人来说，这令人震惊。

This felt like there was this Brandon thing that just came out of nowhere and just suddenly crushed all these algorithms that we've been working on. As we kind of watch this history of computer vision walking from the 1950s all the way up into the battle till the 2000s, you'll notice that neural networks were not a mainstream part of that history throughout much of computer vision's history. So when this suddenly appeared, it felt to a lot of computer vision researchers like this was this brand-new thing suddenly appearing all at once, this brand-new exciting technology.

这感觉就像是突然冒出了这个布兰登事件，它毫无征兆地出现，瞬间摧毁了我们一直在研究的所有算法。当我们回顾计算机视觉从1950年代一直到2000年代的发展历程时，你会注意到在计算机视觉的大部分历史中，神经网络并不是这段历史的主流部分。所以当它突然出现时，许多计算机视觉研究人员都觉得这就像是一个全新的事物突然同时出现，这项令人兴奋的全新技术。

And that was a little bit flawed because this was not a brand new technology. There had been a parallel stream of researchers going back similar amounts of time that had been developing and holding these techniques for decades. 2012 was the sudden breakthrough moment where all of that hard work paid off and became mainstream. So then let's talk a little bit about the history of deep learning kind of going back in time yet again.

这个观点有些缺陷，因为这项技术并非全新突破。几十年来一直存在着并行的研究脉络，研究人员们用相似的时间跨度持续开发和保存这些技术。2012年是突然的突破时刻，所有辛勤工作终于得到回报并成为主流。那么让我们再次回溯历史，谈谈深度学习的渊源。

So then about the same time that Hubel and Wiesel were doing some of their seminal work on the visual recognition and Katz, there was another very influential algorithm called the perceptron. So the perceptron was one of the earliest systems that could learn as a computer system. But what's interesting is that this was in 1958, so the idea of an algorithm, the idea of programming the computer, these were already quite novel research topics on their own at that time. The perceptron was actually implemented as a piece of hardware. There's a picture of it on the right that is the perceptron.

大约在Hubel和Wiesel进行视觉识别方面的开创性研究以及Katz的同一时期，出现了另一个非常有影响力的算法——感知机。感知机是能够作为计算机系统进行学习的最早期系统之一。但有趣的是，这发生在1958年，当时算法的概念、计算机编程的概念本身就已经是相当新颖的研究课题。感知机实际上是作为硬件实现的。右侧有一张它的图片，那就是感知机。

It was this giant like cabinet size thing with like wires going all over the place. It had weights that were stored in these potentiometers which I don't even know what that is because I'm a computer scientist. These weights were just mechanically updated. The values of these weights were changed mechanically during learning by a set of electric motors which again I'm not a mechanical engineer so I definitely could not build this thing.

这是个巨大的柜子大小的设备，电线遍布各处。它通过电位器存储权重——我甚至不知道那是什么，因为我是计算机科学家。这些权重是通过机械方式更新的。在学习过程中，这些权重的数值通过一组电动机进行机械调整，同样地，由于我不是机械工程师，我肯定造不出这东西。

But what was even though this was a mechanical device that was bigger than a person, it actually could learn from data somehow and it was able to learn to recognize letters of the alphabet on these tiny 20 by 20 images that were super state-of-the-art in 1958. So if I don't want to talk about any of the math with us for the perceptron, but if you were to look at it with modern eyes, we would probably call it a linear classifier and we'll talk about next week on Wednesday's lecture. So this perceptron got a lot of people really excited, it caught people thinking like wow here's a mechanism that allows machines to learn novel.

但尽管这是一个比人还大的机械设备，它实际上能够以某种方式从数据中学习，并且能够学会在1958年最先进的20x20微小图像上识别字母表中的字母。所以如果我不想和大家讨论感知器的数学原理，但如果你用现代的眼光来看，我们可能会称其为线性分类器，我们将在下周的周三讲座中讨论。这个感知器让很多人非常兴奋，它让人们思考：哇，这是一个能让机器学习新颖事物的机制。

But what was even though this was this mechanical device that was bigger than a person, it actually could learn from data somehow and it was able to learn to recognize letters of the alphabet on these tiny 20 by 20 images that were super state-of-the-art in 1958. So if I don't want to talk about any of the math with us for the perceptron, but if you were to look at it with modern eyes we would probably call it a linear classifier and we'll talk about next week on Wednesday's lecture. So this perceptron got a lot of people really excited. It caught people thinking like wow here's a mechanism that allows machines to learn novel stuff from data without people having to explicitly program how it's going to work. And all of that kind of came to a crashing halt in 1969 when Marvin Minsky and Seymour Papert published this infamous book called Perceptrons in 1969.

但尽管这是一个比人还大的机械设备，它实际上能够以某种方式从数据中学习，并且能够学会在1958年最先进的20x20小图像上识别字母表中的字母。所以如果我不想和大家讨论感知器的数学原理，但如果你用现代的眼光来看，我们可能会称其为线性分类器，我们将在下周三的讲座中讨论。这个感知器让很多人非常兴奋。它让人们思考，哇，这是一个让机器能够从数据中学习新事物的机制，而无需人们明确编程其工作方式。这一切在1969年戛然而止，当时马文·明斯基和西摩·帕佩特出版了这本名为《感知器》的著名著作。

And what Minsky and Papert pointed out in their book basically was that perceptrons are not magical devices. The perceptron is a particular learning algorithm and there are certain types of functions that it can learn and other types of functions that it cannot learn to represent. In particular, they pointed out that the XOR function is not something that is learnable by the linear perceptron learning algorithm that we'll talk about again a little bit more next week. The normal story that gets told is that the sudden realization from this book was that these learning algorithms are not magical, there's things they can't learn, and people just lost interest in the field. Work in perceptrons kind of dried up for a period of time following the release of this book.

明斯基和帕佩特在他们的书中指出，感知器并非神奇设备。感知器是一种特定的学习算法，有些类型的函数它可以学习，而其他类型的函数它无法学习表示。特别指出的是，异或函数无法通过线性感知器学习算法来学习，我们下周会再详细讨论这一点。通常的说法是，这本书让人们突然意识到这些学习算法并不神奇，它们有无法学习的东西，于是人们对这个领域失去了兴趣。在这本书发布后的一段时间里，感知器的研究工作逐渐枯竭。

And this is this sudden realization well okay so this bugger but what's interesting is that I think nobody actually read the book because if you actually read it there are sections where they say yes the original perceptron learning algorithm is quite limited and it can only represent certain functions but there's something else there's another potential version of the algorithm called a multi-layer perceptron that actually can learn many many many many different types of functions and is very flexible in its representations but that Penna got lost in the headlines at the time and nobody realized that.

这就是突然意识到，好吧，这个家伙，但有趣的是我认为实际上没有人读过这本书，因为如果你真的读过，有些章节提到原始的感知器学习算法确实相当有限，它只能表示某些函数，但还有别的东西——算法的另一个潜在版本叫做多层感知器，实际上可以学习很多很多不同类型的函数，并且在表示上非常灵活，但这一点在当时被头条新闻淹没了，没有人意识到。

And people just heard that for sub Tron's didn't work and we're dead. So you should definitely read the assigned reading. But then going forward quite some amount of time we skip ahead to 1980 and there was this very influential paper called system proposed called the neocon neutron that was developed by Fukushima with a Japanese computer scientist. He was directly inspired by Hubel and Wiesel idea of this hierarchical processing of neurons. Remember people and Wiesel talk about these business simple cells these complex cells he's hierarchies of neurons that could gradually learn more learn to and see more.

人们只是听说子特隆系统不起作用，我们就没戏了。所以你一定要阅读指定的材料。但在接下来的相当长一段时间后，我们快进到1980年，出现了一篇非常有影响力的论文，提出了一个名为新皮层神经元的系统，这是由日本计算机科学家福岛开发的。他直接受到了休伯尔和威塞尔关于神经元分层处理思想的启发。记得休伯尔和威塞尔谈到这些简单的细胞、复杂的细胞，他提出神经元的分层结构能够逐渐学习更多，学会看到更多。

He wanted more complex visual stimuli in the image, so Fukushima proposed this computational realization. People call it the neocognitron formulation. The neocognitron interleaved two types of operations: one were these computational simple cells that, if we were to look at them with modern terminology, would look very much like convolution; and the latter was these computational realizations of complex cells that, again under modern terminology, look very much like the pooling operations that we use in modern convolutional networks. So what's striking is that even back in this neo.

他想要图像中更复杂的视觉刺激，因此福岛提出了这种计算实现。人们称之为新认知机模型。新认知机交错使用了两种类型的操作：一种是这些计算简单细胞，用现代术语来看，它们非常类似于卷积；后者是这些复杂细胞的计算实现，同样用现代术语来看，它们非常类似于我们在现代卷积网络中使用的池化操作。因此令人惊讶的是，即使在这个新

So what's striking is that even back in this neo cognate Ron from 1980 had an overall architecture and overall method of processing that looked very similar to this famous Alex net system that swept in 2012. Even the figures that they have in the papers look pretty similar so they gotta be the same thing right. But what was striking but the Neo cognate Ron is that they defined this computational model they had the right idea of convolution and pooling and hierarchy but they did not have any practical method to train the algorithm because remember this there's a lot of learning awaits in this system.

令人惊讶的是，早在1980年的新认知机就具备了整体架构和处理方法，这与2012年横扫领域的著名AlexNet系统非常相似。甚至他们论文中的图表看起来也相当相似，所以它们应该是相同的东西。但新认知机的惊人之处在于，他们定义了这个计算模型，拥有卷积、池化和层次结构的正确理念，但却没有任何训练算法的实用方法，因为要记住这个系统中有大量需要学习的内容。

A lot of connections between all the neurons inside need to be set somehow. Even then, Fukushima did not have an efficient algorithm for learning to properly set all those free-weight parameters in the system based on data. So then a couple years later, there was this again massively influential paper by Rumelhart and Ted Williams in 1986. The interview introduced the backpropagation algorithm for training these multi-layer perceptrons. Remember that in the perceptrons book, there was this thing called a multi-layer perceptron that was thought to be very powerful in its ability to.

所有内部神经元之间的连接需要以某种方式设置。即便如此，福岛当时还没有一种有效的算法来学习如何基于数据正确设置系统中所有这些自由权重参数。几年后，鲁梅尔哈特和泰德·威廉姆斯在1986年发表了一篇极具影响力的论文。该论文介绍了用于训练这些多层感知器的反向传播算法。记得在感知器著作中提到过一种称为多层感知器的结构，它被认为在能力上非常强大。

That was thought to be very powerful in its ability to represent and learn to protect functions well in the back propagation. In this paper that introduced the back propagation algorithm was one of the first times that people were able to successfully and efficiently train these deeper models with multiple layers of computations. This looks very much like a modern neural network that we're using today. If you look at one you kind of look at this paper and go through it you'll see they talk about gradients and they talk about Jacobians and all this kind of mathematical terminology that we think about.

这被认为在表示和学习保护反向传播函数方面非常强大。引入反向传播算法的这篇论文是人们首次能够成功有效地训练具有多层计算的深层模型。这看起来非常像我们今天使用的现代神经网络。如果你仔细阅读这篇论文，你会发现他们讨论了梯度和雅可比矩阵，以及我们考虑的所有这类数学术语。

Impressionnant today when we're building and training neural networks. These look very much like the modern fully connected networks that we still use today. There are sometimes called multi-layer perceptrons in homage to this long history. So now that does not let a lot of people get really excited about them. There are networks together and try to figure out different types and structures of neural networks that could be built and trained powered by this new back propagation algorithm.

令人印象深刻的是，当我们构建和训练神经网络时，这些网络看起来非常像我们今天仍在使用的现代全连接网络。为了致敬这段悠久历史，它们有时被称为多层感知器。现在这并没有让很多人对它们感到真正兴奋。人们将网络组合在一起，尝试找出可以通过这种新反向传播算法构建和训练的不同类型和结构的神经网络。

And one of the most influential works at that time was a young McCune's paper in 1998 that introduced the idea of a convolutional neural network. This looks again very much like the Fukushima algorithm. What they do here is they took Fukushima's idea of convolution and pooling and multiple layers inspired by the visual system and combine that with the back propagation algorithm from Rumelhart's paper in 1986. With that combination they were able to train these very large at the time convolutional neural networks that could learn to recognize.

当时最具影响力的作品之一是1998年年轻的McCune发表的论文，该论文引入了卷积神经网络的概念。这看起来又与福岛算法非常相似。他们所做的是采用了福岛受视觉系统启发的卷积、池化和多层概念，并将其与1986年Rumelhart论文中的反向传播算法相结合。通过这种结合，他们成功训练了当时非常庞大的卷积神经网络，这些网络能够学习识别。

And one of the most influential works at that time was a young McCune's paper in 1998 that introduced the idea of a convolutional neural network. This looks again very much like the Fukushima algorithm. What they do here is they took Fukushima's idea of convolution and pooling and multiple layers inspired by the visual system and combine that with the back propagation algorithm from Rumelhart's paper in 1986. With that combination they were able to train these very large at the time convolutional neural networks that could learn to recognize different types of things and images. This was a hugely successful system. I think it actually was very successful commercially. So in addition to being a piece of very influential academic research, it was also deployed in a commercial system by NEC labs. For a period of time, this convolutional neural net system developed by that group was actually being used to process the handwriting on a lot of checks that were written in the United States at that time.

当时最具影响力的成果之一是1998年年轻的McCune发表的论文，该论文提出了卷积神经网络的概念。这看起来与福岛算法非常相似。他们所做的是借鉴了福岛受视觉系统启发的卷积、池化和多层结构理念，并将其与1986年Rumelhart论文中的反向传播算法相结合。通过这种结合，他们成功训练出了当时规模庞大的卷积神经网络，能够识别图像中的各类物体。这是一个极其成功的系统。我认为它在商业上也取得了巨大成功。除了作为极具影响力的学术研究外，该系统还被NEC实验室部署在商业应用中。有段时间，该团队开发的卷积神经网络系统实际被用于处理当时美国大量支票上的手写文字。

One thing that I found stated that up to 10% of all cheques in the United States were actually having the numbers on the cheque read automatically by these convolutional neural net systems in the late 90s and early 2000s. If you look at exactly what this algorithm is doing, Lynette was kind of like this. The algorithm here was called Lynette's after Don McCune ironies pressure. If you look at the structure of an algorithm, it looks very similar almost identical to the types of algorithm that was used in AlexNet nearly 30 years later.

我发现有资料指出，在90年代末和21世纪初，美国高达10%的支票实际上都是通过这些卷积神经网络系统自动读取支票上的数字。如果你仔细研究这个算法的运作原理，Lynette算法就类似这样。这个算法以Don McCune的讽刺压力理论命名为Lynette算法。如果你观察算法结构，它看起来与近30年后AlexNet使用的算法类型几乎完全相同。

So then emboldened by the success, they were again a small group of people throughout the late 90s and early 2000s who were really interested in trying to move them in push neural net systems and figure out ways to train neural net systems that were ever bigger, ever deeper, ever wider and could be used on an increasing variety of tasks. And around this period of time in the 2000s was wearing the term deep learning first emerged where it records were it where the term deep was meant to refer to multiple layers in these neural network type algorithms.

于是，在成功的鼓舞下，从90年代末到21世纪初，又有一小群人真正热衷于推动神经网络系统的发展，并探索训练更大、更深、更广的神经网络系统的方法，使其能够应用于日益多样化的任务。大约在21世纪初的这段时间，"深度学习"这一术语首次出现，其中"深度"一词指的是这类神经网络算法中的多层结构。

There's a and so this was really not a super mainstream research area at this time. There was a relatively small number of research groups and relatively small number of people studying these ideas at this time. But I think a lot of the fundamentals that were reaping the rewards of now were really developed during this period of time in the 2000s when people started figuring out all the new modern tricks to train different types of neural network systems.

当时这确实不是一个非常主流的研究领域。当时只有相对较少的研究团队和相对较少的人在研究这些想法。但我认为我们现在正在收获回报的许多基础知识实际上是在2000年代这个时期发展起来的，当时人们开始找出所有新的现代技巧来训练不同类型的神经网络系统。

So that finally brings us back to Alex fat and then in 2012 we had this great confluence of this great computer vision task called ImageNet that people in computer vision were super excited about. We had the second new techniques convolutional neural networks and efficient ways to train them that have been developed by this parallel research community and everything just seemed to come together just in time in 2012. So then from 2012 to present day we've seen an absolute explosion in the usage of convolutional and other types of neural networks across both computer vision and other types of related areas in AI and across computer science.

这最终让我们回到Alex fat，然后在2012年，我们迎来了一个伟大的汇合点：计算机视觉领域的人们对名为ImageNet的视觉任务感到非常兴奋。我们拥有了由并行研究社区开发的第二项新技术——卷积神经网络及其高效训练方法，一切似乎都在2012年恰逢其时地汇聚在一起。从2012年至今，我们见证了卷积神经网络和其他类型神经网络在计算机视觉、人工智能相关领域以及计算机科学领域的爆炸式应用。

So here on the left we have the Google Trends for deep learning that shows you this massive sort of exponential growth that really took off starting in 2012. On the right this is a photo I took at the computer vision and pattern recognition conference this summer in 2019. So this is one of the premier venues for academic publications in computer vision and here is a graph that they were showing at the keynote for that conference where they showed on the x-axis the year of the conference and on the y-axis the number of submitted.

左边是深度学习的谷歌趋势图，显示了从2012年开始爆发的指数级增长。右边是我在2019年夏季计算机视觉与模式识别会议上拍摄的照片。这是计算机视觉领域学术发表的首要平台之一，这是他们在会议主题演讲中展示的图表，其中x轴显示会议年份，y轴显示提交数量。

In this conference, you can see that even though the last five to ten years have resulted in this massive explosion of machine learning systems, both in popular perception, especially from Google Trends, as well as the massive increase in academic interest into both machine learning systems and computer vision systems, as evidenced by this fine spot on the right. If you look around the field today, we see convolutional networks and other types of deep neural networks being used for just about every possible application of computer.

在这次会议上，你可以看到，尽管过去五到十年间，机器学习系统经历了巨大的爆发式增长，无论是在大众认知中（尤其是从谷歌趋势来看），还是学术界对机器学习系统和计算机视觉系统的兴趣都大幅增加，正如右侧这个亮点所证明的那样。如果你今天环顾这个领域，我们会看到卷积网络和其他类型的深度神经网络正被用于几乎所有可能的计算机应用。

Vision systems as evidenced by this fine spot on the right. And if you look around the field today, we see the convolutional networks, other types of deep neural network you see are being used for just about every possible application of computer vision that you can imagine. So from 2012, these convolutional networks are really everywhere. They're getting use from such a wide diversity of tasks like image classification. We want to put labels on images or image retrieval or want to retrieve images from collections. Things like object detection, we want to recognize the positions of objects and images while simultaneously label like that. The record should be image segmentation. I'm threatening to fix the slide where we want to label the what where this is going back to this idea of semantic grouping you saw for computer vision.

视觉系统正如右侧这个亮点所证明的那样。如果你观察当今这个领域，我们会看到卷积网络和其他类型的深度神经网络正被用于几乎所有你能想象到的计算机视觉应用。从2012年开始，这些卷积网络确实无处不在。它们被广泛应用于各种任务，如图像分类。我们想要给图像添加标签或进行图像检索，或想要从集合中检索图像。像物体检测这样的任务，我们想要识别物体在图像中的位置，同时进行类似这样的标注。记录应该是图像分割。我正在准备修正这个幻灯片，我们想要标注这个语义分组概念在计算机视觉中的应用。

In the 90s, we want to label the pixels as being part of a cohesive whole. Comments are going to use for things like video classification on activity recognition. They're going to use for things like pose estimation where you want to say how are the exact geometric poses of people arranged in images. Even for things that don't really feel like classical computer vision like playing Atari games with a process the visual input of the Atari game with a convolutional neural network and combine that with other sorts of learning techniques in order to both jointly learn.

在90年代，我们希望将像素标记为连贯整体的一部分。这些技术将用于活动识别中的视频分类等任务。它们将用于姿态估计等场景，即确定人物在图像中的精确几何姿态排列。甚至用于那些不太像传统计算机视觉的任务，比如通过卷积神经网络处理Atari游戏的视觉输入，并结合其他学习技术来实现联合学习。

Convolutional neural networks are also getting used for visual tasks that are about visual data that humans aren't very good at. So convolutional networks are getting used in things like medical imaging to diagnose different types of tumors, diagnose different types of skin lesions, other medical conditions. They're going to be used in galaxy classification. They're getting used in tons of scientific applications like classifying whales or elephants or other types of animals.

卷积神经网络也被用于处理人类不太擅长的视觉数据相关任务。因此卷积神经网络正被应用于医疗影像等领域，用于诊断不同类型的肿瘤、诊断不同类型的皮肤病变以及其他医疗状况。它们将被用于星系分类。它们正被用于大量科学应用中，比如对鲸鱼、大象或其他类型的动物进行分类。

Or because there's this problem where scientists want to go out into the world and collect a lot of data and then be able to use images and visual recognition as a kind of universal sensor to make use of all this data that they collect and gain insights into their particular field of expertise that they're interested in. And we've seen computer vision and convolutional networks branch out into all these other areas of science and just open up and unlock lots of new applications just across the board.

或者因为存在这样一个问题：科学家们希望走出去收集大量数据，然后能够使用图像和视觉识别作为一种通用传感器，利用他们收集的所有这些数据，深入了解他们感兴趣的专业领域。我们已经看到计算机视觉和卷积网络扩展到所有其他科学领域，全面开启并解锁了许多新的应用。

They're big. The comments are going to use for all kinds of fun tasks like image captioning that we can write systems we can build systems they can write natural language descriptions of images. These are using convolutional networks we can use convolutional networks for generating arts so we can make all these kind of psychedelic portraits again using convolutional neural networks. So then we might ask what was it that happened in 2012 that made all of this take off. Well I think the jury's out and we'll have to see what the story is 50 years from now but my personal interpretation is that it was a combination of.

它们规模庞大。这些评论将被用于各种有趣的任务，比如图像标注，我们可以编写系统、构建系统，它们能够为图像生成自然语言描述。这些系统使用卷积网络，我们可以利用卷积网络进行艺术创作，因此我们能够再次使用卷积神经网络制作各种迷幻风格的人像。那么我们可能会问，2012年究竟发生了什么让这一切蓬勃发展？我认为尚无定论，我们需要等待50年后的历史评判，但我的个人解读是这源于多种因素的结合。

But my personal interpretation is that it was a combination of three big components that came together all at once. One was the set of algorithms that we saw that there was a stream of people working on deep learning at common neural networks and machine learning who had developed these powerful set of tools for representing learning functions and for learning that was the fact propagation algorithm we saw this. The second stream of data that with the rise of digital cameras later Robertson running the internet and people to develop a crowdsourcing we were able to collect unprecedented to label data that could be used to train these systems.

但我的个人解读是，这是三个重要组成部分同时汇聚的结果。第一个是我们看到的算法集合，有一批研究人员致力于深度学习、通用神经网络和机器学习，他们开发出了这套强大的工具集，用于表示学习函数和学习过程，这就是我们所见的事实传播算法。第二个数据流随着数码相机的兴起，后来罗伯逊运营互联网以及人们开发众包模式，我们得以收集前所未有的标注数据，这些数据可用于训练这些系统。

And the third piece that we haven't really talked about was the massive rise in computational resources that has been continually happening throughout the history of computer science. So one graph that I put together that I find particularly striking is looking at the gigaflops of computation per dollar as a function of time. So here on the blue you can see these are different types of CPUs are in a CPU central processing units the thing that's remained on your cloud with all of your laptops truck and they get faster but not that much faster over time.

我们尚未讨论的第三个要素是计算资源的大幅增长，这一增长贯穿计算机科学的整个历史进程。我整理的一张图表特别引人注目，它展示了每美元计算能力的千兆浮点运算次数随时间的变化关系。在蓝色部分可以看到不同类型的中央处理器，这些存在于云端和所有笔记本电脑中的处理器虽然速度有所提升，但随着时间的推移提升幅度并不显著。

But starting in 2008 there were some really interesting developments with GPUs, graphics processing units. These were special-purpose pieces of hardware that were originally developed to pump up pixels in computer graphics applications. But around 2008 people started developing techniques to run generalized programs on these graphics processing units. Over time these techniques became more and more easy to write general-purpose scientific code and mathematical code to run on these massively parallel graphics processing units.

但从2008年开始，GPU（图形处理器）领域出现了一些非常有趣的发展。这些专用硬件最初是为增强计算机图形应用中的像素处理能力而开发的。但大约在2008年左右，人们开始开发在这些图形处理器上运行通用程序的技术。随着时间的推移，这些技术使得编写能在这些大规模并行图形处理器上运行的通用科学代码和数学代码变得越来越容易。

And then if you look at the timeline from 2006 to 2007 and look at the gigaflops per dollar on these graphics processing units, you can see that although this exponential Moore's Law may not have held up for CPUs, it actually has been. We actually haven't seen exponential increases in GPU computing power over time over the last 10 years. And if you look at maybe the AlexNet, this has been striking even in the last couple of years. So if you look at the AlexNet system in 2012, he was using this GTX 580 GPU that was very, very exciting at the time if you're a gamer.

如果你观察2006到2007年的时间线，查看这些图形处理器的每美元浮点运算性能，你会发现尽管指数级的摩尔定律可能不适用于CPU，但实际上它仍然存在。过去10年间，我们实际上并未看到GPU计算能力随时间呈指数级增长。而观察AlexNet，即使在最近几年这种情况也相当显著。在2012年的AlexNet系统中，他使用了当时令游戏玩家非常兴奋的GTX 580 GPU。

And if you push it on into more recent cards like the GTX 980Ti or more recently a 2080 Ti, then you can see that the cards we have even five years later are literally exponentially more powerful than the cards that they were going to keep in 2000 as well. So I think that it was really this confluence of algorithms, of data and of massive increase in computation fueled by advances in GPUs that led to all this magic happening in 2012.

如果你将其应用到更新的显卡上，比如GTX 980Ti或更近期的2080 Ti，你会发现即使五年后的显卡性能也呈指数级增长，远超2000年时的产品。因此我认为，正是算法、数据以及GPU技术进步推动的计算能力大幅提升这三者的融合，才造就了2012年所有这些奇迹的发生。

That led to all these new applications of convolutional networks on different types of computer vision. In recognition of the impact of computer vision and deep learning across the field, the 2018 Turing Award was awarded to Yann LeCun, Geoffrey Hinton, and Yoshua Bengio for their work on pioneering many of the deep learning ideas that we'll learn throughout this class. For those of you who don't know, the Turing Award is basically considered the Nobel Prize equivalent in the field of computer science. This just happened last year and this was a recognition that this has been a massively.

这导致了卷积网络在各种计算机视觉类型上的全新应用。鉴于计算机视觉和深度学习在整个领域的影响力，2018年图灵奖授予了Yann LeCun、Geoffrey Hinton和Yoshua Bengio，以表彰他们在开创我们将在这门课程中学习的众多深度学习思想方面的工作。对于不了解的人来说，图灵奖基本上被认为是计算机科学领域的诺贝尔奖等价物。这是去年刚刚发生的事情，这表明这已经是一个巨大的。

So this just happened last year and this was just a recognition that this has been a massively influential piece of research that's been changing all of our lives over the last several years. But I think it's important to stay humble and realize that despite all of the successes that we've seen in convolutional networks in deep learning and computer vision, I think we're really still a long way away from building systems that can perceive and understand visual data to the same fidelity and power and strength as humans. One image that I like to use to exemplify this is this example.

这件事发生在去年，这表明我们认识到这项研究产生了巨大影响，在过去几年里改变了我们所有人的生活。但我认为保持谦逊很重要，要认识到尽管我们在卷积网络、深度学习和计算机视觉领域取得了诸多成功，但距离构建能够以与人类相同的精确度、能力和强度来感知和理解视觉数据的系统，我们还有很长的路要走。我喜欢用这张图片来说明这一点。

So what's if we were to send this to a convolutional network he would probably say person or scale or locker-room maybe. But if we were to look at this you see quite a different story. You see a guy standing on a scale you know how scales were which require some into some idea of physics. You know that he's looking at the scale you know that he's trying to measure his own weight. You know that people tend to be self-conscious about their weight. You know that the person behind him is stepping on the scale and pushing down because of your knowledge of physics.

如果我们把这送到卷积神经网络，它可能会说人、体重秤或更衣室。但如果我们仔细观察，会发现完全不同的故事。你会看到一个男人站在体重秤上，这需要一些物理概念的理解。你知道他正看着体重秤，试图测量自己的体重。你知道人们往往对自己的体重很在意。你知道他身后的人正踩在秤上往下压，这是基于你的物理知识。

You know that's going to make you feel for a bigger number because of your knowledge of that guy's psychology. You'll know that might make him feel embarrassed or uncomfortable because he thinks he ate too much. Then you also know who that person pushing down on the scale is, and because of your knowledge of who he is, it may be surprising that he's acting in this way. You understand you can see the people behind him watching this scene and laughing.

你知道这会让你倾向于更大的数字，因为你对那个人的心理有所了解。你会知道这可能会让他感到尴尬或不适，因为他认为自己吃得太多了。然后你也知道那个按着秤的人是谁，而且基于你对他的了解，他以这种方式行事可能令人惊讶。你明白你可以看到他身后的人们正在观看这一幕并大笑。

And understand that you need to know how it is the people look at each other. You understand that maybe they're surprised that this guy is doing this thing that's causing this guy to be embarrassed. So there's a lot going on in this image that we as human as visually intelligent humans could understand and perceive. And I think we're a long way away from building computer vision systems that can match that level of visual fidelity. But I'm hoping that as we move forward and continue to advance the field maybe one day we'll get there.

要理解你需要知道人们是如何看待彼此的。你明白他们可能很惊讶这个人正在做的事情让另一个人感到尴尬。所以这张图片中有很多内容，我们作为视觉智能的人类能够理解和感知。我认为我们距离构建能够匹配这种视觉保真度水平的计算机视觉系统还有很长的路要走。但我希望随着我们不断前进并推动该领域发展，也许有一天我们能够实现这个目标。

But in the meantime I think that computer vision technology really has massive and massive potential to improve all of our lives. It'll make our lives more fun through sort of new video applications applications in VR AR. It'll make our transportation safer with advances in autonomous vehicles. It'll lead to improvements in medical imaging and diagnosis. And overall I think computer vision as a whole has a massive ability and potential to continue leading to massive improvements in all of our day-to-day lives. So that's why I think we should be studying computer vision. That's why I make it excited to be teaching this class this year.

与此同时，我认为计算机视觉技术确实具有巨大的潜力来改善我们所有人的生活。它将通过VR/AR中的新型视频应用让我们的生活更有趣。它将通过自动驾驶汽车的进步让我们的交通更安全。它将带来医学影像和诊断的改进。总的来说，我认为计算机视觉整体上具有强大的能力和潜力，能够持续为我们日常生活的各个方面带来重大改进。这就是为什么我认为我们应该学习计算机视觉。这也是为什么我很兴奋今年能教授这门课程。

So that basically covers our brief history of computer vision of deep learning except there's one little spot on the timeline that we didn't fill out and that's this class. So with that it's time to if there was any if there's any questions about historical stuff then we're going to move on to course logistics.

这基本上涵盖了我们对深度学习和计算机视觉的简要历史回顾，除了时间线上有一个小空白我们还没有填补，那就是这门课程。所以如果对历史内容有任何问题，我们现在就进入课程安排环节。

You're out? No, okay, great. So for staff who are Wheaton, I'm Justin Johnson. I'm a new assistant professor here in the computer science and engineering department. This is the first class I'm teaching here at Michigan. This is the first time I've been in this room, so glad I found it about the laptop identity. But I'm excited to be here, excited to be teaching you guys this class. We have an amazing team of graduate student instructors that are going to be helping us out this semester. How is your guy? The standard will be docile. So these guys are all experts in computer vision. They're all PhD students here. Moonscape works in video understanding and generative models. Keep on course and robustness and generalization. Gluant works a lot in visual vision plus language. So if you have questions both those particular research areas.

你出局了？不，好的，很好。对于惠顿的教职员工，我是贾斯汀·约翰逊。我是计算机科学与工程系的新助理教授。这是我在密歇根教的第一门课。这是我第一次在这个教室上课，很高兴我找到了关于笔记本电脑身份的问题。但我很兴奋能来到这里，很兴奋能教你们这门课。我们有一个优秀的研究生助教团队，这学期将帮助我们。你们怎么样？标准将会很温和。这些人在计算机视觉方面都是专家。他们都是这里的博士生。Moonscape研究视频理解和生成模型。专注于课程、鲁棒性和泛化性。Gluant主要研究视觉与语言的结合。所以如果你对这些特定研究领域有疑问。

You should go talk to them. So how to contact us? This is an important slide right? Taking pictures with this comes a good idea. I probably could feel some kindness to something. We'll extract abuse formation out from them automatic. But we kept on a course website that's up in this URL. The course website has followed the information that you'll need for others throughout the quarter. You can find the syllabus. This head so the schedule you'll find links to assignments they're your clients links the lecture videos assuming a set of lecture capture properly really important.

你应该去和他们谈谈。那么如何联系我们？这是个重要的幻灯片对吧？用这个拍照是个好主意。我可能能感受到对某些事物的善意。我们将自动从中提取滥用信息。但我们保留了一个在这个网址上的课程网站。课程网站包含了整个季度你需要为他人准备的信息。你可以找到教学大纲。这个标题下的时间表你可以找到作业链接，它们是你的客户链接，讲座视频链接，假设一系列讲座被正确录制非常重要。

We're going to use the Piazza for most communication with you guys. So we really encourage you, if you have questions for the course material, we're going to use the Piazza as our main mechanism to communicate back and forth with you guys. So if you have questions about course content, questions about material, post on Piazza. And similarly, if we need to make announcements back to the class to announce things like the homeworks, about changes to logistics, we will be announcing all of those through Piazza. So it's really important that you guys get signed.

我们将使用Piazza平台与大家进行大部分沟通。因此我们真诚建议大家，如果对课程材料有疑问，我们将使用Piazza作为与大家互动交流的主要渠道。如果对课程内容或教材有疑问，请在Piazza上发帖提问。同样地，当我们需要向班级发布通知时，比如关于作业安排、后勤调整等事项，我们都会通过Piazza发布。因此大家务必完成注册。

So it's really important that you guys get signed up as quickly as possible. One piece of the note is that please don't post any code on Piazza about public questions. If you do need to ask particular questions about code, we ask that you make private questions that are visible only to you and the instructor. We will have Canvas. I think I need to set that up still this week, but we're really using Canvas primarily for just turning in assignments. Mostly we will be using Piazza and the course website for this class. We will have office hours.

你们尽快注册非常重要。需要注意的是，请不要在Piazza上发布任何关于公共问题的代码。如果你确实需要询问具体的代码问题，我们要求你创建仅对你和讲师可见的私人问题。我们将使用Canvas。我想我这周还需要设置一下，但我们主要使用Canvas来提交作业。这门课我们主要会使用Piazza和课程网站。我们将安排答疑时间。

Both me and the GSIS started next week, and you'll be able to find the times and locations of the office hours on the Google Calendar that I set up here. Finally, we really want most the vast majority of communication you do with us should be through Piazza. But if you have some kind of very sensitive topic that you would prefer to discuss directly with me, then you can email me directly. But for the vast majority of circumstances, you should be going through Piazza for course communication.

我和GSIS都将在下周开始工作，你们可以在这里我设置的谷歌日历上找到办公时间的时间和地点。最后，我们希望你们与我们进行的大多数沟通都应通过Piazza进行。但如果你有某些非常敏感的话题，更愿意直接与我讨论，那么你可以直接给我发邮件。但在绝大多数情况下，你应该通过Piazza进行课程沟通。

That will ensure that everyone will all open teaching staff is able to help you in a timely manner. If you're able to make public questions, it lets you all help each other and learn collectively from each other's mistakes. I think Piazza is really great learning tool for that. We're going to have an optional textbook. There's no required textbook for this class. On this schedule you'll find on the website, there will be recommended readings for all of the lectures. This textbook is totally optional and it's completely available for free online.

这将确保所有教学人员都能及时为您提供帮助。如果您能够提出公开问题，这能让你们互相帮助，共同学习，并从彼此的错误中吸取教训。我认为Piazza在这方面是一个非常好的学习工具。我们将有一本可选教材。本课程没有指定教材。在网站上的课程安排中，您会看到所有讲座都有推荐阅读材料。这本教材完全是可选的，并且完全可以在网上免费获取。

That will ensure that everyone doesn't have to purchase a copy if you don't want to. Regarding course content and grading, the main bulk of the course is going to be six programming assignments. These are going to be really exciting assignments. We're going to use Python, PyTorch and Google Colab, and we're going to walk you through the detailed implementations of a lot of the ideas that we talked about in lecture. We will have a midterm exam and we will have a final exam.

这将确保如果大家不想购买的话，都不必购买副本。关于课程内容和评分，课程的主要部分将是六个编程作业。这些作业将会非常令人兴奋。我们将使用Python、PyTorch和Google Colab，并指导大家完成我们在讲座中讨论的许多想法的详细实现。我们将有期中考试和期末考试。

But there will not be an important project. The majority of the stuff will be learning through the programming assignments. So we have a late policy so don't turn it in late. But more seriously, you all get three free late days to use on your homework assignments. You don't have to tell us beforehand just randomly. And then automatically once you've exhausted your late days, I'm gonna take 25% per day or something like that. That's reasonable. Are there any questions about content or policies, late days, anything like that?

但不会有重要项目。大部分内容将通过编程作业来学习。所以我们有迟交政策，请不要迟交。但更严肃地说，你们都有三个免费的迟交日可用于作业。你们不需要提前告知，可以随机使用。一旦你们用完了迟交日，我会自动每天扣除25%的分数或类似处罚。这是合理的。关于课程内容或政策、迟交日等有任何问题吗？

Yeah over here. So the question is will the course materials be available for people not on the waitlist? And they will be as really available as I can possibly make. Not so even if you can't get an open the class, you can definitely feel free to follow along with the lecture slides with lecture videos. What the course? Yeah question. Yeah it's up to you. You're all grown up so you can use the funny lengthiest first time as you want. But we you take too many and we were zero so I don't recommend that. But three ladies you can use as many as you want.

是的，这边请。问题是课程材料是否会向不在候补名单上的人开放？我会尽可能让它们真正可用。即使你无法选上这门课，你绝对可以自由地跟着讲义幻灯片和讲座视频学习。这门课程？是的，问题。是的，这取决于你。你们都已经是成年人了，所以可以按照自己的意愿使用有趣的、最长的第一次。但如果你选得太多而我们为零，所以我不推荐那样做。但三位女士，你可以随意使用任意数量。

And expert ladies you somebody as you want for sign that but we'll just take this tape off lines and sizes here. Yeah, so let's talk about collaboration policy. So we really encourage you guys to work together in groups. I think it's great to discuss which course material with your classmates and to learn together. But we have a couple of ground rules about that for a collaboration policy. One is that everything you submit should be your own work. It's fine to talk about ideas with other students that's great and encouraged.

各位专家女士们，你们可以根据需要选择签名方式，但我们现在先取下这些线条和尺寸的胶带。好的，现在我们来谈谈合作政策。我们非常鼓励大家分组合作。我认为与同学讨论课程材料并共同学习是非常好的。但关于合作政策，我们有一些基本规则。其中一条是你们提交的所有内容都应该是自己的作品。与其他学生交流想法是很好的，也是我们鼓励的。

But you shouldn't be sharing or looking at each other's code word necessarily. You can talk about things conceptually, but you shouldn't be turning on the same code as people you work with. Secondly, on the flip side, don't share your solution code to other people. This means don't post on Piazza, don't give it to your roommates, don't throw down big puzzles because that will make it easier and more tempting for other people to violate collaboration policy. And third, when you turn in assignments, we have to indicate who you work with.

但你不应该分享或查看彼此的代码字。你们可以从概念上讨论问题，但不应与同事使用相同的代码。其次，反过来看，不要将你的解决方案代码分享给他人。这意味着不要在Piazza上发布，不要给你的室友，不要设置复杂的谜题，因为这会让他人更容易也更想违反合作政策。第三，提交作业时，必须注明与谁合作。

What would in turn in your assignment and will be more equal in instructions on that later and in general training in something late or incomplete is much better than potentially violating collaboration policies and not just in this course but more generally. Any questions about that okay so then of course philosophy what are we here for right all right yeah this class is not learn pipework too many games this class is dogs learn deep learning in ten lines of Python you can find tutorials on the internet that tell you how to do that that's what you want but I think that learning about.

关于作业提交，稍后会有更详细的说明和更公平的指导。总的来说，提交延迟或不完整的作业远比违反合作政策要好，这不仅适用于本课程，也适用于更广泛的情况。对此有任何问题吗？好的，那么当然要谈谈我们的教学理念——我们在这里的目的是什么？没错，这门课不是学习花哨的技巧或玩太多游戏。这门课的目标不是用十行Python代码学会深度学习。你可以在网上找到教你这样做的教程，如果你想要的是那种内容。但我认为学习关于

But I think that learning about deep learning in that way does yourself a huge disservice. I think that you want to focus on fundamental concepts. We want you to understand not just the latest API level. API is the raft answer club. We want you to understand the fundamentals of how these guys might have been implemented, why I didn't let it the way they work such that when faced with the next piece of technological tools, you understand the fundamentals and you do them yourself.

但我认为以那种方式学习深度学习对自己非常不利。我认为你应该专注于基本概念。我们希望你们不仅理解最新的API层面。API是木筏答案俱乐部。我们希望你们理解这些工具可能如何实现的基本原理，理解它们为何以特定方式运作，这样当面对下一个技术工具时，你们就能理解基本原理并自行实现。

What that means is that you'll be writing a lot of backdrop code yourself in this course. I think that it's very important for people to learn how to compute gradients and how the computation of gradients affects the overall flow of learning pressure system. So for the first several assignments you'll be using no autographs, you'll be using Java tensorflow still be deriving and in fluent in your own background your own gradient computations and you will be a better computer scientist born. So given that we prefer to move afraid to write inventory for the purpose of pedagogy we encourage you.

这意味着在本课程中，你将需要亲自编写大量底层代码。我认为学习如何计算梯度以及梯度计算如何影响学习压力系统的整体流程非常重要。因此在前几个作业中，你将不使用自动求导功能，而是使用Java TensorFlow，在自己的基础上推导并熟练掌握梯度计算，这样你将成为更优秀的计算机科学家。鉴于我们出于教学目的更倾向于编写底层代码，我们鼓励你们。

We're going to encourage you to write and stretch rather than relying on existing open source implementations. Again, this will make you a better deep learning practitioner. That said, we're also practical. We're going to give you lots of tools and techniques for debugging and training big neural networks because it's tricky when you can't rely on that ten lines of code wrapping around lots of stuff. So we're going to talk a lot about how you can practically get these things to work, what tips and tricks should you be using when developing and debugging and training.

我们将鼓励你们动手编写和扩展代码，而不是依赖现有的开源实现。这将再次让你们成为更优秀的深度学习实践者。话虽如此，我们也很务实。我们将为你们提供大量调试和训练大型神经网络的工具与技巧，因为当你们无法依赖那些封装了大量功能的十行代码时，这个过程会变得相当棘手。因此我们将重点讨论如何实际让这些模型正常工作，以及在开发、调试和训练过程中应该使用哪些技巧和方法。

We will use state of the art software tools like I taught you TensorFlow, but only after you've earned your wings by writing a lot of bad code yourself. We're going to focus on state of the art. A lot of the material that we cover in this course, despite the long historical context we talked about in this lecture, but the majority of the actual concrete implementations and concrete results we've discussed that we'll talk about in this course have been discovered in the last five to ten years. So this has a couple of interesting applications.

我们将使用最先进的软件工具，比如我教过你们的TensorFlow，但这需要你们先通过自己编写大量糟糕代码来获得经验。我们将专注于最前沿技术。尽管本讲座中我们讨论了悠久的历史背景，但本课程涵盖的大部分内容，以及我们将要讨论的具体实现和实际成果，都是在过去五到十年间发现的。因此这具有几个有趣的应用。

Their networks for implications for teaching a course that means that there aren't good textbooks for this stuff that means that there might not be great resources outside of original research papers for learning about this stuff so that's going to be maybe a bit of a struggle and a bit challenging but on the upside I think it's really exciting to be learning about such deep material in a classroom setting so also in philosophy we want you could also have a little bit of fun.

他们的网络对课程教学的影响意味着没有关于这些内容的优秀教科书，也意味着除了原始研究论文之外可能没有很好的学习资源，因此这可能有点困难和挑战性。但另一方面，我认为在课堂环境中学习如此深入的材料确实令人兴奋，所以在哲学方面我们也希望你们能获得一些乐趣。

So we'll be Petzl. We'll be covering some fun topics like image captioning that you've got a good laugh when I put it up here a couple slides ago, and some B's on a deep dream and our artistic style transfer that lets you use neural networks to generate new pieces of art, not just improving our lives. So in terms of the course structure, the first half of the course will focus on fundamentals. We'll talk about the details of how to implement different types of neural networks.

我们将介绍一些有趣的主题，比如图像字幕识别，我在几页幻灯片前展示时你们都笑得很开心，还有关于深度梦境的内容以及我们的艺术风格转换技术，这些技术让你能够使用神经网络创作新的艺术作品，而不仅仅是改善我们的生活。就课程结构而言，前半部分课程将重点讲解基础知识。我们将详细讨论如何实现不同类型的神经网络。

We'll cover building a fully connected neural networks, talk about how to debug them, how to implement them, how to train them, and they'll be very detailed. By the end of this module, the goal is basically implementing your own convolutional neural network system from scratch. Now in the second half of the course, we're going to shift a little bit in flavor and here we're going to focus more on applications and more emerging sort of research topics. So around that point in the course, you'll notice the bit of shift in tone in the lectures.

我们将涵盖构建全连接神经网络，讨论如何调试它们、如何实现它们、如何训练它们，内容会非常详细。到本模块结束时，目标基本上是从零开始实现你自己的卷积神经网络系统。在课程的后半部分，我们将稍微改变风格，更多地关注应用和新兴的研究主题。在课程的那个阶段，你会注意到讲座中的语气有所转变。

So they will become a little bit less detail, will sometimes skip over some of the low-level details and perhaps refer to your papers if you need to know those details. Instead, the lectures will more focus on giving you an overview of how people are, how these different things are used across different applications in computer vision and beyond. Well, in the second half we'll talk about things like object detection, image segmentation, 3D vision, videos. We'll talk about attention transformers, vision language, generative models. I think it's going to be a lot of fun.

因此它们会变得稍微简略一些，有时会跳过某些底层细节，如果你需要了解这些细节，可能会参考你的论文。相反，讲座将更侧重于概述人们如何在不同应用中以及计算机视觉以外的领域使用这些不同的技术。在第二部分，我们将讨论诸如目标检测、图像分割、三维视觉、视频等内容。我们将讨论注意力变换器、视觉语言、生成模型。我认为这将非常有趣。

But because there's a lot to get through, first homework assignment will be out over the weekend that will cover basically an intro and a warm-up to the collab and height or the environment that we'll be using for our quarter. So this should not, this is not intended to be difficult or a long assignment. This should be your home over the weekend and whenever we get it out it'll be Zoo will be after that and everything you need to do this first homework assignment will get through the con based lecture. So with that said, welcome to the class.

但由于内容很多，第一次作业将在本周末发布，基本上涵盖介绍和热身，涉及我们本学期将使用的协作环境和工具。所以这不应该，这不是要设计成困难或冗长的作业。这应该是你周末的家庭作业，无论何时我们发布后，动物园将在那之后进行，完成第一次作业所需的一切将通过基于会议的讲座来完成。那么，欢迎来到这门课程。

Come back on Monday when we'll talk about English concertina.

周一回来，届时我们将讨论英式六角手风琴。

 you]