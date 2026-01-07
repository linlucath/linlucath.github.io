---
title: 'Lecture 1_ Introduction to Deep Learning for Computer Visio'
publishDate: 2026-01-07
description: 'TODO'
tags:
  - TODO
language: 'English'
heroImage: { src: './default.jpg', color: '#D58388' }
---

Welcome. I hope y'all are in the right place. Welcome to ECS 498-007/558-005, this special topics class, also a first-time here at Michigan, focusing on computer vision. I wish we had a snappier, easier, more easy-to-remember course type or course number, but when you teach a special topics class, they give you numbers like this, so I'm sorry about that. But hopefully, you're all in the right place.

欢迎。我希望各位都来对了地方。欢迎来到ECS 498-007/558-005，这门专题课程，也是密歇根大学首次开设的，专注于计算机视觉。我希望我们能有一个更响亮、更简单、更容易记住的课程类型或编号，但当你教授专题课程时，他们就会给你这样的编号，对此我很抱歉。但希望各位都来对了地方。

So the title of this class is "Deep Learning for Computer Vision." I think we need to unpack a little bit what these terms mean before we get started. Computer vision is the study of building artificial systems that can process, perceive, and otherwise reason about visual data. This is quite a broad definition. What does "process" mean? What does "perceive" mean? What does "reason" mean? It's kind of up for interpretation. And what is this "visual data"? It could be images, it could be videos, it could be medical scans, it could be just about any type of continuously valued signal you can think about, which can sometimes be found in computer vision conferences or publications somewhere. So these terms are really defined quite broadly.

那么这门课的标题是"计算机视觉的深度学习"。我认为在开始之前，我们需要稍微解释一下这些术语的含义。计算机视觉是研究构建能够处理、感知并对视觉数据进行推理的人工系统。这是一个相当宽泛的定义。"处理"是什么意思？"感知"是什么意思？"推理"是什么意思？这有点取决于解释。那么什么是"视觉数据"呢？它可以是图像、视频、医学扫描，几乎可以是你能想到的任何类型的连续值信号，有时在计算机视觉会议或某些出版物中也能看到。所以这些术语的定义确实非常宽泛。

Why is computer vision important? Well, I think computer vision is a particularly important and exciting topic to study because it's everywhere. I think many of us in this room right now are carrying around one or more, several cameras, and we're just taking millions of photos every day. There are cameras all around us all the time. People are always creating visual data, sharing digital data, talking about visual data. And it is very important that we build algorithms that can perceive, reason, and process.

为什么计算机视觉很重要？嗯，我认为计算机视觉是一个特别重要且令人兴奋的研究课题，因为它无处不在。我想在座的许多人现在都随身携带一台或多台相机，我们每天拍摄数百万张照片。我们周围随时都有摄像头。人们总是在创造视觉数据、分享数字数据、讨论视觉数据。而构建能够感知、推理和处理的算法至关重要。

For a couple of concrete statistics, if you look at YouTube—actually, let's look at Instagram first. Instagram is very popular; many of you are familiar with it. There's something like 100 million photos and videos uploaded on Instagram every single day. If we go on YouTube, it's even worse. On YouTube, as of 2015—and I'm sure it's grown since then—people are uploading roughly 300 hours of video on YouTube every minute. So that means, if you do the math, and you think if I wanted to, as a single individual human being, look at all the visual data just being uploaded to Instagram and YouTube in one day—if you do the math, say I'm going to look at images for maybe one second each, I'm going to look at my YouTube videos at double speed—it's gonna take me about 25 years to look at the visual data that's going to be uploaded on just these two sites in a single day.

举几个具体的统计数据，如果你看YouTube——实际上，我们先看看Instagram。Instagram非常流行；你们很多人都熟悉它。每天大约有1亿张照片和视频上传到Instagram。如果我们看YouTube，情况更糟。在YouTube上，截至2015年——我相信此后还在增长——人们每分钟大约上传300小时的视频。这意味着，如果你算一下，假设我想作为一个单独的个体，看完一天内上传到Instagram和YouTube的所有视觉数据——算一下，假设我看每张图片大约一秒钟，以双倍速看YouTube视频——那么看完仅这两个网站在一天内上传的视觉数据，我需要大约25年。

So when you think about these massive statistics and think about the massive amount of visual data being processed and shared across the Internet these days, it becomes clear that we need to be able to build automated systems that can deal with it because we just don't have the human manpower to look at, process, and perceive all of the data that is being created. So that's why I think computer vision is such an important topic to be studying these days, and it's only going to get more important as the number of visual sensors out in the world keep increasing with new emerging technologies like autonomous vehicles, augmented and virtual reality, drones. You can imagine that the role of computer vision in our modern society will just continue getting more and more and more important.

因此，当你考虑到这些庞大的统计数据，以及当今互联网上处理和共享的海量视觉数据时，很明显我们需要能够构建自动化的系统来处理它，因为我们根本没有足够的人力去查看、处理和感知所有正在创建的数据。所以这就是为什么我认为计算机视觉是当今如此重要的研究课题，而且随着世界上视觉传感器的数量随着自动驾驶汽车、增强和虚拟现实、无人机等新技术的出现而不断增加，它只会变得越来越重要。你可以想象，计算机视觉在我们现代社会中的作用只会变得越来越重要。

Clearly, I'm biased because this is my research area, but I think this is the most important and exciting research topic that we can be studying right now in computer science. So that's computer vision. Computer vision is the problem that we're trying to solve; it's the force behind this problem of understanding digital data. But computer vision doesn't really care how we solve that problem. Our goal is just to crunch through all of those images and videos however we can.

显然，我有偏见，因为这是我的研究领域，但我认为这是我们目前在计算机科学中可以研究的最重要、最令人兴奋的课题。这就是计算机视觉。计算机视觉是我们试图解决的问题；它是理解数字数据这一问题的驱动力。但计算机视觉并不真正关心我们如何解决这个问题。我们的目标只是尽我们所能处理所有这些图像和视频。

But the way, the technique that we happen to be using in computer vision across the field these days, is deep learning. So, before we get to deep learning, what is learning? Learning is the process of building artificial systems that can learn from data and experiences. Notice that this is somewhat orthogonal to the goals of computer vision. Computer vision just says we want to understand visual data; we don't care how you do it. Learning is this separate problem of trying to build systems that can adapt to the data that they see and the experiences that they have in the world.

但如今我们在整个计算机视觉领域恰好使用的方法是深度学习。那么，在讨论深度学习之前，什么是学习？学习是构建能够从数据和经验中学习的人工系统的过程。请注意，这与计算机视觉的目标有些正交。计算机视觉只是说我们想理解视觉数据；我们不关心你怎么做。学习则是另一个独立的问题，即试图构建能够适应它们所看到的数据和它们在世界上所拥有的经验的系统。

And from the outside, it's not immediately clear why these two go together, but it turns out that in the last 10 to 20 years, we found that building learning-based systems is very important for building many kinds of generalizable computer systems, both in computer vision and across many areas of artificial intelligence and computer science more broadly.

从表面上看，这两者为何结合并不立即明显，但事实证明，在过去的10到20年里，我们发现构建基于学习的系统对于构建多种可泛化的计算机系统非常重要，无论是在计算机视觉领域，还是在更广泛的人工智能和计算机科学的许多领域。

So now, when we think about deep learning, deep learning is then yet another subset of machine learning where deep learning is sort of maybe a bit of a baby name, a bit of a buzzword, but my definition is that it's a type of hierarchical learning algorithms with many layers—whatever that means in the context of neural networks—that are very, very loosely inspired by the architecture of the mammalian brain and some types of the mammalian visual system.

那么现在，当我们考虑深度学习时，深度学习是机器学习的另一个子集，深度学习这个名字可能有点幼稚，有点流行语的味道，但我的定义是，它是一种具有许多层的分层学习算法——无论这在神经网络背景下意味着什么——其灵感非常非常松散地来源于哺乳动物大脑的架构和某些类型的哺乳动物视觉系统。

Now, I know I said "loosely." This is a thing that you'll often see people talk about in deep learning, that it's how the brain learns or how the brain works. I think you should take any of these comparisons with a massive grain of salt. There are some very coarse comparisons between brains and the neural networks that we use today, but I think you should not take them too seriously.

我知道我用了"松散地"这个词。在深度学习中，你经常会看到人们谈论这一点，说它就像大脑如何学习或大脑如何工作。我认为你应该对这些比较持极大的保留态度。我们的大脑和今天使用的神经网络之间有一些非常粗略的比较，但我认为你不应该太当真。

So, I'm kind of stepping back a little bit. From these two topics, computer vision and machine learning, both fall within the purview of the larger research field of artificial intelligence. So artificial intelligence is very general, very broad. Broadly speaking, it's: how can we build computer systems that can do things that normally people do? That's kind of my definition. I think people will argue about what is and is not artificial intelligence, but I think we just want to build smart machines, whatever that means to any of us.

那么，我稍微退一步讲。计算机视觉和机器学习这两个主题，都属于更大的人工智能研究领域的范畴。所以人工智能非常通用，非常广泛。广义上说，它是：我们如何构建能够完成通常由人完成的事情的计算机系统？这算是我的定义。我想人们会争论什么算人工智能，什么不算，但我认为我们只是想构建智能机器，无论这对我们每个人意味着什么。

And I think there's clearly many different sub-disciplines of artificial intelligence, but two of the most important, clearly again in my biased opinion, are computer vision—teaching machines to see—and machine learning—teaching machines to learn. And these are the topics that we'll study in this class.

我认为人工智能显然有许多不同的子学科，但其中最重要的两个，再次以我有偏见的观点来看，显然是计算机视觉——教机器看——和机器学习——教机器学习。这些就是我们将在这门课中学习的主题。

So then, kind of where does deep learning fall in this regime? It would be a subset of machine learning and it intersects both computer vision and falls within the larger AI ground. I think it's important at the outset to note that this class is going to focus on this section right in the middle, the intersection of computer vision, machine learning and deep learning.

那么，深度学习在这个体系中处于什么位置呢？它将是机器学习的一个子集，并与计算机视觉相交，同时属于更广泛的人工智能范畴。我认为在一开始需要指出，本课程将重点关注中间这个部分，即计算机视觉、机器学习和深度学习的交叉领域。

The reason I start out with this slide is because it's really easy to get caught up in the hype these days and think that computer vision is the only type of AI, deep learning is the only type of AI, or deep learning is the only type of computer vision. But I think none of these are true. There are many types of AI which have nothing to do with learning, nothing to do with deep learning. There are classical results about symbolic systems and other approaches to AI that are very different technically. There are areas of computer vision that do not use any machine learning or deep learning.

我以这张幻灯片开始，是因为如今人们很容易被炒作所迷惑，认为计算机视觉是人工智能的唯一类型，深度学习是人工智能的唯一类型，或者深度学习是计算机视觉的唯一类型。但我认为这些都不正确。有许多类型的人工智能与学习无关，与深度学习无关。关于符号系统和其他人工智能方法有经典的成果，它们在技术上非常不同。计算机视觉中有一些领域根本不使用任何机器学习或深度学习。

So I love it even though the focus of this class will be the intersection of these different research areas. I just want you to keep in mind as a whole that there is a much broader realm of AI research being done right now around the world by different groups that falls into different pieces of this pie chart. And of course, there are many other areas within AI that we won't talk about too much, so there's natural language processing, things like speech recognition, things like robotics. I kind of ran out of space on the chart with many more sub areas.

因此，尽管本课程的重点将是这些不同研究领域的交叉部分，我仍然非常欣赏这一点。我只是希望你们总体上记住，目前全球不同的研究团体正在进行的人工智能研究领域要广泛得多，它们分布在这个饼图的不同部分。当然，人工智能内部还有许多其他领域我们不会过多讨论，比如自然语言处理、语音识别、机器人学等。图表空间有限，无法列出更多子领域。

But suffice to say, artificial intelligence is a massively successful, a massively popular area of research, an area of study these days that again with the broad goal of making machines do things that people normally do. You can imagine that there's a whole lot of things that we might do out in the world that fall under this umbrella of artificial intelligence.

但可以说，人工智能如今是一个极其成功、极其热门的研究领域和学习领域，其广泛目标再次是让机器做人类通常做的事情。你可以想象，我们在世界上可能做的很多事情都属于人工智能这把大伞之下。

So that's kind of the big picture roadmap. And now for the route for the rest of the semester, we're gonna focus on this little red area in here. But again, don't forget that there's a lot more to the world than what we're talking about in this class.

这就是大致的发展路线图。至于本学期剩余时间的路线，我们将重点关注这里这个小小的红色区域。但再次提醒，不要忘记世界远比我们在这门课上讨论的内容要广阔得多。

So today's agenda is a little bit different from most of the lectures in this class because again it is the first week. So before we can really dive into that red piece of the pie chart and talk about machine learning and deep learning and computer vision, all that really good stuff, I think it's important to get a little bit of historical context about how we got here as a field.

所以今天的议程与本课程大多数讲座略有不同，因为这毕竟是第一周。因此，在我们真正深入探讨饼图中的那个红色部分，谈论机器学习、深度学习和计算机视觉所有这些精彩内容之前，我认为了解一点我们这个领域是如何发展到今天的历史背景很重要。

This has been a hugely successful research area in the last five to ten years. But deep learning, machine learning in computer vision, these are areas with decades and decades of research built upon them. And all of the successes we've seen in the last few years have been a result of building upon decades of prior research in these areas.

在过去的五到十年里，这一直是一个非常成功的研究领域。但深度学习、计算机视觉中的机器学习，这些都是建立在数十年研究基础上的领域。我们在过去几年看到的所有成功，都是建立在这些领域数十年先前研究基础上的结果。

So today I want to give a bit of a brief history and overview of some of the historical context that led up to the successes of today. And then following that, we need to talk about some of the boring stuff of course, overview logistics, all that other stuff that you expect to see in the first lecture class.

所以今天，我想简要介绍一下历史背景，概述一些导致今天成功的历史脉络。然后，之后我们需要讨论一些比较枯燥的内容，当然是课程概述、后勤安排等所有你们期望在第一堂课上看到的东西。

So let's start with so we're going to do this in two ways right? We're going to do a parallel stream. First, we're going to talk about the history of computer vision and we're going to sort of switch a little bit and we'll cover the history of deep learning.

那么，我们开始吧。我们将通过两种方式来进行，好吗？我们将采用并行方式。首先，我们将讨论计算机视觉的历史，然后我们会稍微切换一下，再介绍深度学习的历史。

So before we dive into the material, any sort of questions before we launch into this historical escapade? No? Okay, perfectly clear.

那么，在我们深入探讨材料之前，在我们开始这段历史之旅之前，有什么问题吗？没有？好的，非常清楚。

So if we go, I think whenever you talk about a research area, it's always difficult to pinpoint the start right because everything builds on everything else. There's always prior work, everyone was inspired by something else that came before. But with a finite amount of time to talk about a finite number of things, you got to cut the line somewhere.

那么，如果我们开始的话，我认为每当谈论一个研究领域时，总是很难精确指出起点，因为一切都是相互依存的。总有先前的工作，每个人都受到之前事物的启发。但要在有限的时间内谈论有限的内容，你必须在某个地方划清界限。

So one place where I like to draw the line and point to as maybe the start of computer vision is actually not with computer scientists at all and happened. It's from this seminal study of Hubel and Wiesel back in 1959, who were not interested in computers at all. They wanted to understand how the mammalian brains work.

所以，我喜欢划定的一个界限，并可能将其视为计算机视觉起点的，实际上根本不是计算机科学家，而是发生的一件事。它来自1959年胡贝尔和维泽尔的这项开创性研究，他们当时对计算机根本不感兴趣。他们想了解哺乳动物的大脑是如何工作的。

So what they did is they got a cat, they got an electrode, they put the electrode into the brain of the cat, into the visual cortex of the cat, just the part in the back of your head that processes visual data. And with this electrode, they're able to record the neuronal activity of some of the individual neurons in the cat's visual cortex.

他们所做的就是找来一只猫，准备了一个电极，将电极插入猫的大脑，插入猫的视觉皮层，也就是你后脑勺处理视觉数据的那部分。通过这个电极，他们能够记录猫视觉皮层中一些单个神经元的神经元活动。

So then with this somewhat grotesque experimental setup, they were able to have the cat watch TV. And then not really TV because it was 1950 time, but they were able to show different sorts of slides to the cat. They had this general hypothesis that maybe there's certain neurons in the brain that respond to different types of visual stimuli.

于是，通过这个有点怪异的实验装置，他们能让猫看电视。不过那其实不是电视，因为那是1950年代，但他们能够向猫展示各种不同的幻灯片。他们有一个普遍的假设，即大脑中可能有某些神经元会对不同类型的视觉刺激做出反应。

And by showing the cat different types of visual stimuli and recording the neural activity from individual neurons, maybe we can start to puzzle out how this thing called vision works at all. So that's exactly what they did. They got these cats, they stuck neurons in their brains and they started showing a bunch of different images on a slideshow to try to see what kinds of images would activate the neurons in cats' brains.

通过向猫展示不同类型的视觉刺激，并记录单个神经元的神经活动，也许我们就能开始弄清楚视觉这个东西到底是如何工作的。这正是他们所做的。他们找来这些猫，将电极插入它们的大脑，然后开始在幻灯片上展示一系列不同的图像，试图观察哪些类型的图像会激活猫大脑中的神经元。

So they tried different things. You can show them mice and fish and other kinds of things that cats like to eat or play with. But it was really hard to get any solid signal about what these neurons were responding to.

他们尝试了不同的东西。你可以向它们展示老鼠、鱼以及其他猫喜欢吃或玩的东西。但很难获得关于这些神经元对什么做出反应的可靠信号。

So what one really interesting discovery happened is, you know today we're using PowerPoint, back in the day we had natural slide projectors. And when you change the slide, like there's kind of a vertical bar that would move up and down the screen. And what they surprisingly found is that some of the neurons in the cat's brain would consistently respond to the time when they change the slides.

于是，发生了一个非常有趣的发现。你知道我们现在用PowerPoint，而在那个年代，他们用的是普通的幻灯投影仪。当你换幻灯片时，屏幕上会有一个垂直的条状物上下移动。他们惊讶地发现，猫大脑中的一些神经元会持续地对更换幻灯片的时刻做出反应。

And they even though they couldn't recognize any patterns of what was, how it was the cat responding to things on the slides, they eventually discovered that it was in fact this moving vertical bar that was indeed causing some of the neuronal activity in the cat's brain.

尽管他们无法识别出任何模式，即猫是如何对幻灯片上的东西做出反应的，但他们最终发现，实际上正是这个移动的垂直条状物确实引起了猫大脑中的一些神经元活动。

So with this hint, they were able to puzzle out that there are different types of cells in the brain that are responding to different types of visual stimuli. Many of them are very hard to interpret, but some of the easiest are these so-called simple cells that they discovered. So the simple cells would respond to an edge that's maybe light on one side, dark on another side, at a particular orientation at a particular position in the cat's visual field. And if there happened to be an edge at the right position, at the right angle, in the right place, then that particular neuron might fire. That was very exciting because they may have some concrete evidence of what it is that cats are actually responding to in their brains.

有了这个线索，他们得以推断出大脑中存在不同类型的细胞，它们对不同类型的视觉刺激做出反应。其中许多反应非常难以解释，但其中最容易理解的是这些所谓的简单细胞，这是他们发现的。因此，简单细胞会对猫视野中特定位置、特定朝向的边缘（可能一侧亮、一侧暗）产生反应。如果恰好在正确的位置、正确的角度、正确的地方出现一个边缘，那么那个特定的神经元就可能被激活。这非常令人兴奋，因为他们可能获得了关于猫的大脑实际对什么做出反应的具体证据。

Then with a bit more exploration, they went on to find other types of cells in the brain that responded to even more complex patterns, like the complex cells that would respond to bits of motion or could respond to oriented edges but anywhere in the visual field, to give a sense of some sense of translation invariance in the visual representations that they perceive.

随后，经过更多的探索，他们继续发现了大脑中对更复杂模式做出反应的其他类型细胞，例如复杂细胞，它们可能对运动片段做出反应，或者可以对有朝向的边缘做出反应，但反应位置可以在视野中的任何地方，从而在它们感知的视觉表征中体现出某种平移不变性。

So I think that this is really one of the founding... Oh, and by the way, of course I have to mention that these guys, this was very seminal research, and these guys won the Nobel Prize for it in 1981. So this was a very important research in the history of science and psychology and vision overall.

所以我认为这确实是奠基性的研究之一……哦，顺便说一下，我当然必须提到，这些人，这项研究是非常开创性的，他们在1981年因此获得了诺贝尔奖。因此，这在整个科学、心理学和视觉研究史上都是一项非常重要的研究。

But I like to point to this as the beginning of computer vision for a couple reasons. One is this emphasis on oriented edges. We'll see this come up over and over again in the different architectures that we study throughout the semester. The other is this hierarchical representation of the visual system, of building from simple cells that represent one thing, combining with complex cells and more and more complex cells that respond to more and more complex types of visual stimuli.

但我喜欢将其视为计算机视觉的开端，原因有几个。一是这种对有朝向边缘的强调。在我们整个学期将研究的不同架构中，我们会反复看到这一点。另一个是视觉系统的这种分层表征，即从代表单一事物的简单细胞开始构建，与复杂细胞以及响应越来越复杂视觉刺激的、越来越复杂的细胞相结合。

This broad idea was hugely influential on the way that people thought about visual processing and even on neural representations more generally.

这个宽泛的理念对人们思考视觉处理的方式，甚至对更广泛的神经表征方式，都产生了巨大的影响。

So then if we move forward a couple years, to 1963, Larry Roberts... That's when Larry Roberts graduated from MIT with a PhD and did perhaps what was the first PhD thesis on computer vision. Here, of course, it was 1963, doing anything with computers was very cumbersome, doing anything with digital cameras was very cumbersome. So large portions of his thesis just talk about how do you actually get photographic information into the computer, because this was not something you could take for granted at that time.

那么，如果我们向前推进几年，到1963年，拉里·罗伯茨……那时拉里·罗伯茨刚从麻省理工学院获得博士学位，并完成了可能是第一篇关于计算机视觉的博士论文。当然，那是1963年，用计算机做任何事情都非常麻烦，用数码相机做任何事情也非常麻烦。因此，他论文的大部分内容只是讨论如何将摄影信息实际输入计算机，因为这在当时并非理所当然的事情。

But even working through those constraints, he built some system that was able to take this raw visual picture, detect some of the edges in the picture, sort of inspired by Hubel and Wiesel's discovery that edges were fundamental to visual processing. Then from there, detect feature points, and then from there start to understand the 3D geometry of objects and images.

但即使在这些限制下工作，他还是构建了一个系统，能够获取原始的视觉图片，检测图片中的一些边缘——这在一定程度上是受到了休伯尔和威塞尔关于边缘是视觉处理基础的发现的启发。然后从那里开始检测特征点，再进而开始理解物体和图像的三维几何结构。

Now what's really interesting is that if you go and look at the Larry Roberts Wikipedia page, it actually doesn't mention any of this at all, because after he finished his PhD, he went on to become the founding father of the internet and went on to be a hugely major player in the World Wide Web and all of the networking technologies that were developed around that time. So doing the first PhD thesis in computer vision was kind of a low point in his career. I think all of us can aspire to that success.

现在真正有趣的是，如果你去查看拉里·罗伯茨的维基百科页面，它实际上根本没有提及任何这些内容，因为在他完成博士学位后，他继续成为了互联网的奠基人，并在万维网以及当时开发的所有网络技术中成为极其重要的参与者。所以，完成第一篇计算机视觉博士论文在他职业生涯中算是一个低谷。我想我们所有人都可以向往那样的成功。

So then moving forward a couple more years, people are getting really excited. So there was this very famous study in 1966 from MIT, a Seymour Papert proposed the summer computer vision project. The summer computer vision project basically what he wanted to do is like, OK, we've got digital cameras now, they can detect edges, we know how... all that Hubel and Wiesel told us how the brain works. What we're gonna do is hang a couple undergrads, put them to work over the summer, and after the summer we should be able to construct a significant portion of the visual system.

那么再向前推进几年，人们变得非常兴奋。于是在1966年，麻省理工学院有一项非常著名的研究，西摩·帕佩特提出了夏季计算机视觉项目。夏季计算机视觉项目基本上他想做的是：好吧，我们现在有了数码相机，它们可以检测边缘，我们知道……休伯尔和威塞尔告诉了我们大脑是如何工作的。我们要做的就是找几个本科生，让他们在夏天工作，夏天过后，我们应该就能构建出视觉系统的很大一部分。

Man, these guys are really ambitious back in the day, because now it's clear computer vision is not solved. They did not achieve this. A lot of people, and nearly 50 years later, we're still plugging away trying to achieve this what they thought they could do in the summer with a couple undergrads.

天哪，这些人当年真是雄心勃勃，因为现在很明显计算机视觉问题尚未解决。他们并没有实现这个目标。很多人，近50年后，我们仍在努力奋斗，试图实现他们当年认为用一个夏天和几个本科生就能完成的事情。

So moving forward into the 1970s, one hugely influential figure in this era was David Marr, who proposed this idea of stages of visual representation, which again kind of harkens back to Hubel and Wiesel. So here you can see that maybe we want the input image, then we have another stage of visual processing, and we extract edges. Then from the edges we extract some kind of depth information, that maybe we can segment objects and say which parts of the image belong to which two different types of objects, and then think about the relative depths of those objects, and then eventually start to reason about whole 3D models of the world and of the scene.

那么进入20世纪70年代，这个时代一位极具影响力的人物是大卫·马尔，他提出了视觉表征阶段的想法，这又有点回溯到休伯尔和威塞尔的理论。所以在这里你可以看到，也许我们需要输入图像，然后我们有另一个视觉处理阶段，我们提取边缘。然后从边缘中我们提取某种深度信息，也许我们可以分割物体并说出图像的哪些部分属于哪两种不同类型的物体，然后考虑这些物体的相对深度，最终开始对整个世界和场景的三维模型进行推理。

And then by the end of the seventies, people had started to become interested in recognizing objects and thinking about ways to build computer systems that could not just detect edges and simple geometric shapes, but more complex objects like people, bombs. There was work about some things like generalized cylinders and pictorial structures that tried to recognize people as easy formal configurations of rigid parts with some kind of known topology.

到了七十年代末，人们已经开始对识别物体产生兴趣，并思考如何构建不仅能检测边缘和简单几何形状，还能识别更复杂物体（如人、炸弹）的计算机系统。有一些关于广义圆柱体和图形结构等方面的工作，试图将人识别为由具有某种已知拓扑结构的刚性部件组成的简单形式化配置。

And you can see ideas like this were very influential work at the time, but the problem is that in the nineteen seventies, processing power was very limited, visual cameras were very limited. So a lot of this stuff was sort of toy in a sense.

你可以看到像这样的想法在当时是非常有影响力的工作，但问题是，在二十世纪七十年代，处理能力非常有限，视觉相机也非常有限。所以很多这类工作在某种意义上都像是玩具。

And as we move into the 80s, people had much more access to better digital cameras, more computational power, and people began to work on slightly more realistic images. So one kind of theme in the 80s was trying to recognize objects in images via edge detection. I told you that edges were going to be super influential throughout the history of computer vision.

随着我们进入80年代，人们能够更容易地获得更好的数码相机，拥有更强的计算能力，人们开始处理稍微更真实的图像。因此，80年代的一个主题是尝试通过边缘检测来识别图像中的物体。我告诉过你，边缘在整个计算机视觉史上将具有超级影响力。

So there was a very famous paper from John Canny in 1986 that proposed the Canny edge detection algorithm for detecting edges in images. And then David Lowe the next year in 1987 proposed a mechanism for recognizing objects in images by matching their edges.

因此，约翰·坎尼在1986年发表了一篇非常著名的论文，提出了用于检测图像边缘的坎尼边缘检测算法。然后大卫·洛在第二年，即1987年，提出了一种通过匹配边缘来识别图像中物体的机制。

So in this example, you can imagine we've got this cluttered image of razors, and then we detect the edges. Then maybe we have some template, a picture of a razor that we know about. Then we can detect the edges of our template razor and try to match it into this image, this cluttered image that has a bunch of all the razors. And then by kind of matching edges in this way, you might be able to recognize that there are many, ten razors in this image and what are their relative configurations, just based on matching with our template image.

所以在这个例子中，你可以想象我们有一张杂乱剃须刀的图像，然后我们检测边缘。也许我们有一个模板，一张我们已知的剃须刀图片。然后我们可以检测模板剃须刀的边缘，并尝试将其匹配到这张图像中，这张包含一堆剃须刀的杂乱图像中。然后通过这种边缘匹配的方式，你可能就能识别出这张图像中有很多，比如十把剃须刀，以及它们的相对配置如何，而这仅仅基于与我们的模板图像的匹配。

And now I'm moving on into the 1990s. People again wanted to build more and more complex images, more and more complex scenes. So here, a big theme was trying to recognize objects via grouping. So here, rather than maybe just matching the edges, what we want to do is take the input image and segment it into semantically meaningful chunks. Maybe we know that the person is composed of one meaningful chunk; the different umbrellas would be composed of a different meaningful chunk. With the idea that if we can first do some sort of grouping, then later classifying and recognizing or giving a label to those groups might be an easier problem.

现在，我继续讲20世纪90年代。人们再次希望构建越来越复杂的图像和场景。因此，这里的一个重大主题是尝试通过分组来识别物体。这里，我们不仅仅是想匹配边缘，而是希望将输入图像分割成有语义意义的块。也许我们知道人物由一个意义块组成；不同的雨伞则由另一个意义块组成。其理念是，如果我们能先进行某种分组，那么后续对这些组进行分类、识别或打标签可能会是一个更容易的问题。

Then in the 2000s, a big theme was recognition via matching. And this is a there was a hugely famous paper called SIFT by David Lowe in 1999 that proposed a different way of recognition via matching. So here the idea is that we would take our input image, detect little recognizable key points at different 2D positions in the image, and for each of those key points, we're going to represent its appearance using some kind of feature vector. And that feature vector is going to be a real-valued vector that somehow encodes the image at that little point in space. And by very careful design of exactly how that feature vector is computed, you can encode different types of invariances into that feature vector. Such that if we were to take the same image and rotate it a little bit or brighten or darken the lighting conditions in the scene a little bit, that hopefully we would compute the same value for that feature vector even if the underlying image were to change a little bit.

到了21世纪初，一个重大主题是通过匹配进行识别。199年，David Lowe发表了一篇非常著名的论文《SIFT》，提出了一种不同的通过匹配进行识别的方法。其思想是，我们获取输入图像，检测图像中不同2D位置上的可识别关键点，对于每个关键点，我们将使用某种特征向量来表示其外观。这个特征向量是一个实值向量，它以某种方式编码了空间中小点处的图像信息。通过精心设计特征向量的具体计算方法，你可以将不同类型的（如旋转、光照）不变性编码到该特征向量中。这样，如果我们对同一图像进行轻微旋转，或稍微调亮/调暗场景中的光照条件，即使底层图像略有变化，我们仍有望计算出相同的特征向量值。

And there was a lot of work in it. Once we can extract these sets of robust and invariant feature vectors, then you can again perform some kind of recognition via matching. So that on the left, if we have some template image of a stop sign, we can detect all these distinctive invariant feature key points. Then on the right, if we have another image of a stop sign, this may be taken from a different angle with different lighting conditions. Then by a careful clever design of these invariant robust features, we can match and then correspond points in the one image into points in the other image, and thereby be able to recognize that the right image is indeed a stop sign.

这方面有很多研究工作。一旦我们能提取出这些鲁棒且具有不变性的特征向量集合，你就可以再次通过匹配执行某种识别。例如，在左侧，如果我们有一个停车标志的模板图像，我们可以检测所有这些独特的、具有不变性的特征关键点。在右侧，如果我们有另一个停车标志的图像，它可能是在不同角度和光照条件下拍摄的。那么，通过对这些具有不变性的鲁棒特征进行精心巧妙的设计，我们可以将一幅图像中的点与另一幅图像中的点进行匹配和对应，从而能够识别出右侧图像确实是一个停车标志。

So then another hugely influential work in the 2000s was the Viola-Jones algorithm published in 2001. And this was really, they developed a very very powerful algorithm for recognizing faces in images. So here, you know, you have an image, then you want to draw a box where all the people's faces are. And this was notable; this piece of work was notable for a couple reasons. One, it was the first major use of machine learning in computer vision. So Viola and Jones used some algorithm called the boosted decision trees that were able to learn somehow the right combination of features to use in order to recognize images, to recognize faces. And what was particularly notable was the very fast commercialization of this algorithm. This piece of research went very quickly from a sort of academic piece of research published in 2001, and within a few years, this was actually being shipped in digital cameras at the time.

那么，21世纪另一项极具影响力的工作是2001年发表的Viola-Jones算法。他们确实开发了一个非常强大、用于识别图像中人脸的算法。也就是说，你有一张图像，然后你想在所有的人脸位置画上框。这项工作很著名，有几个原因。第一，它是机器学习在计算机视觉中的首次重要应用。Viola和Jones使用了一种称为提升决策树的算法，该算法能够学习用于识别图像、识别人脸的正确特征组合。特别值得注意的是该算法极快的商业化速度。这项研究在2001年作为学术成果发表后，短短几年内，就被实际集成到当时的数码相机中。

So if you remember, maybe they had like an autofocus feature, or you would kind of hold the shutter half down and it would beep a little bit and draw boxes on the faces and then focus on the people in the scene. Well, that was most likely using this Viola-Jones algorithm. So this is a particularly notable piece of work for those two reasons.

如果你还记得，也许它们有自动对焦功能，或者你半按快门时，它会发出一点提示音，在人脸上画框，然后对场景中的人物进行对焦。这很可能就是使用了Viola-Jones算法。因此，由于这两个原因，这是一项特别值得注意的工作。

So now, after they kind of unlocked the box of using data and using machine learning to augment our visual representations, then moving on into the 2000s, we began to see more and more uses of machine learning, more and more uses of data in order to improve our visual recognition systems. So one hugely influential piece of work here was the Pascal Visual Object Challenge. So here they downloaded a bunch of them because now it's 2000, layered Roberts had excellent computer vision and invented the internet. So we could then download images from the internet to help build these datasets of images, and then we could get graduate students to go and label those images. And then we could use our machine learning algorithms to mimic the labels that the graduate students have written down for the images.

那么，在他们开启了使用数据和机器学习来增强我们视觉表示的大门之后，进入21世纪，我们开始看到越来越多机器学习的应用，越来越多数据的应用，以改进我们的视觉识别系统。其中一项极具影响力的工作是Pascal视觉对象挑战赛。他们下载了大量图像，因为现在是2000年代，分层罗伯茨拥有出色的计算机视觉技术并发明了互联网。因此，我们可以从互联网下载图像来帮助构建这些图像数据集，然后让研究生去标注这些图像。接着，我们可以使用机器学习算法来模仿研究生为图像写下的标签。

And then if you do that, then you can see this nice curve on the right: performance increasing on this recognition challenge increased steadily over time from about 2005 to 2011. So then this brings us to the ImageNet Large Scale Visual Recognition Challenge. So here, this was a very very large-scale dataset in computer vision that has become hugely influential and has become one of the main benchmarks in computer vision even leading up to this day. So the ImageNet classification challenge was this fairly large dataset of more than about 1.4 million images, and each of those 1.4 million images were labeled with one of a thousand different category labels.

如果你这样做，那么你可以在右侧看到这条漂亮的曲线：从大约2005年到2011年，这项识别挑战的性能随着时间的推移稳步提高。这就引出了ImageNet大规模视觉识别挑战赛。这是一个在计算机视觉领域规模非常大的数据集，产生了巨大影响，并成为计算机视觉的主要基准之一，直至今日。ImageNet分类挑战赛包含一个相当大的数据集，超过约140万张图像，这140万张图像中的每一张都被标注了1000个不同类别标签中的一个。

And the big new piece of innovation here was that if you kind of do the math here, you're gonna need a lot of graduate students to label all this stuff. So the big piece of innovation here was to not label data using graduate students. Instead, this made use of crowdsourcing. So here you could go on services like Amazon Mechanical Turk and then parcel out little pieces of work and then blast them out over the Internet. And then people anywhere in the world could label the images, get paid a couple of cents for each image they label. And that was able to massively increase the amount of data. I mean, this is beneficial in two ways, right? Because one, researchers get people to label their data without being constrained by the number of graduate students that they have. And two, it became a nice source of income for some people who were bored at work and just like this, or in classes that gets maybe, or not. It's still weird. It's the running by the lady that grew up introducing tasks. But anyway, this became a hugely influential dataset in computer vision.

这里的一项重大创新在于，如果你算一下，需要很多研究生来标注所有这些内容。所以，这里的重大创新是不再使用研究生来标注数据，而是利用了众包。你可以通过像亚马逊土耳其机器人这样的服务，将工作拆分成小任务，然后在互联网上发布。这样，世界各地的人都可以标注图像，每标注一张图像就能获得几美分的报酬。这能够大规模地增加数据量。我的意思是，这有两方面的好处，对吧？因为第一，研究人员可以让人标注数据，而不受其拥有的研究生数量的限制。第二，它成为了一些在工作时感到无聊、或者可能在上课的人（或者不是）的一个不错的收入来源。这仍然有点奇怪。它是由那位从小介绍任务的女士运营的。但无论如何，这成为了计算机视觉中一个极具影响力的数据集。

And more than a dataset, it became a benchmark challenge. So they had every year they ran a competition where different researchers would compete and try to build their own algorithms that would try to recognize objects in this ImageNet classification challenge. And this became somewhat jokingly known sometimes as the Olympics of computer vision. There was a period of time from maybe the mid-2010s, the late 2000s, early 2010s, when people would just really excitedly want to look at the results of the ImageNet competition every year and see what kind of advances the field had made last year. Then, given that I told you about this competition, you can look at what was the error rate on this competition moving over time.

它不仅仅是一个数据集，更成为一个基准挑战赛。他们每年都会举办一次竞赛，不同的研究者参与竞争，尝试构建自己的算法，以在ImageNet分类挑战赛中识别物体。这有时被戏称为计算机视觉的奥运会。大约从2000年代末、2010年代初到2010年代中期，有那么一段时间，人们每年都会非常兴奋地关注ImageNet竞赛的结果，看看这个领域去年取得了哪些进展。既然我提到了这个比赛，你可以看看这个比赛的错误率是如何随时间变化的。

So the first time the competition was run in 2010, in 2011 we were sitting at error rates of around 28%, around 25%. And then something big happened in 2012. At the 2012 ImageNet competition, suddenly the error rates dropped in a single year from 25% all the way down to 16%. And after 2012, errors just kept on diminishing, diminishing very, very fast, such that once we got to about 2017, we were building systems that could compete on this ImageNet challenge and perform even better than humans when they try to recognize images in this dataset.

这项竞赛首次举办是在2010年，2011年时错误率还在28%左右，大约25%。然后在2012年，大事发生了。在2012年的ImageNet竞赛中，错误率在一年之内突然从25%骤降至16%。2012年之后，错误率持续下降，下降得非常非常快，以至于到了2017年左右，我们构建的系统已经能够在这个ImageNet挑战中竞争，并且在识别该数据集的图像时，表现甚至超过了人类。

So then the question is, what happened in 2012? What happened in 2012 is that deep learning came onto the scene, and this was really the breakthrough moment for deep learning. Computer vision researchers suddenly woke up and saw that there's this crazy new thing that is suddenly sweeping our field.

那么问题来了，2012年发生了什么？2012年发生的是深度学习登上了舞台，这确实是深度学习的突破性时刻。计算机视觉研究者们突然惊醒，看到这个疯狂的新事物正在席卷我们的领域。

In 2012, there was this absolutely seminal paper from Alex Krizhevsky, Ilya Sutskever, and Geoff Hinton, where they proposed a deep convolutional neural network, AlexNet, that just crushed everyone else at the ImageNet competition. For people working in computer vision at that time, this was shocking. This felt like there was this brand-new thing that just came out of nowhere and just suddenly crushed all these algorithms that we've been working on.

2012年，Alex Krizhevsky、Ilya Sutskever和Geoff Hinton发表了一篇极具开创性的论文，他们提出了一种深度卷积神经网络——AlexNet，它在ImageNet竞赛中彻底击败了所有其他对手。对于当时在计算机视觉领域工作的人来说，这是令人震惊的。这感觉像是一个凭空出现的全新事物，突然就击垮了我们一直在研究的所有算法。

As we kind of watch this history of computer vision walking from the 1950s all the way up into the battle till the 2000s, you'll notice that neural networks were not a mainstream part of that history throughout much of computer vision's history. So when this suddenly appeared, to a lot of computer vision researchers, it felt like this brand-new thing suddenly appearing all at once, this brand-new exciting technology.

当我们回顾计算机视觉从20世纪50年代一直到21世纪初的发展史时，你会注意到，在计算机视觉历史的大部分时间里，神经网络并非主流。所以当它突然出现时，对许多计算机视觉研究者来说，感觉就像这个全新的、令人兴奋的技术突然一下子冒了出来。

And that was a little bit flawed because this was not a brand-new technology. There had been a parallel stream of researchers going back similarly long amounts of time that had been developing and holding these techniques for decades. And 2012 was the sudden breakthrough moment where all of that hard work paid off and became mainstream.

这种看法有点问题，因为这并非一项全新技术。长期以来，一直有另一批研究者并行地发展和掌握这些技术，已有数十年之久。2012年是突然的突破时刻，所有那些辛勤工作终于得到回报并成为主流。

So then let's talk a little bit about the history of deep learning, kind of going back in time yet again. About the same time that Hubel and Wiesel were doing some of their seminal work on the visual recognition cortex, there was another very influential algorithm called, though actually it wasn't even an algorithm, called the perceptron.

那么，让我们再回溯一下，稍微谈谈深度学习的历史。大约在Hubel和Wiesel在视觉识别皮层方面进行一些开创性工作的同时，还有另一个非常有影响力的东西，实际上它甚至不是一个算法，叫做感知机。

The perceptron was one of the earliest systems that could learn as a computer system. But what's interesting is that this was in 1958. So the idea of an algorithm, the idea of programming the computer, these were already quite novel research topics on their own at that time.

感知机是最早能够作为计算机系统进行学习的系统之一。但有趣的是，这是在1958年。当时，算法的概念、为计算机编程的概念，本身就已经是相当新颖的研究课题了。

The perceptron was actually implemented as a piece of hardware. There's a picture of it on the right; that is the perceptron. It was this giant, like cabinet-size thing with wires going all over the place. It had weights that were stored in these potentiometers, which I don't even know what that is because I'm a computer scientist.

感知机实际上是作为一件硬件实现的。右边有一张它的图片；那就是感知机。它是一个巨大的、像柜子一样的东西，电线遍布各处。它的权重存储在这些电位器中，我甚至不知道那是什么，因为我是计算机科学家。

And these weights were just mechanically updated. The values of these weights were changed mechanically during learning by a set of electric motors, which again, I'm not a mechanical engineer, so I definitely could not build this thing. But even though this was this mechanical device that was bigger than a person, it actually could learn from data somehow, and it was able to learn to recognize letters of the alphabet on these tiny 20 by 20 images that were super state-of-the-art in 1958.

这些权重只是机械地更新。在学习过程中，这些权重的值由一组电动机机械地改变，同样，我不是机械工程师，所以我肯定造不出这东西。但尽管这是一个比人还大的机械装置，它实际上能以某种方式从数据中学习，并且能够学会在那些微小的20x20像素图像上识别字母表中的字母，这在1958年是超级先进的技术。

I don't want to talk about any of the math with you for the perceptron, but if you were to look at it with modern eyes, we would probably call it a linear classifier, and we'll talk about that next week on Wednesday's lecture. This perceptron got a lot of people really excited. It caught people thinking, "Wow, here's a mechanism that allows machines to learn novel stuff from data without people having to explicitly program how it's going to work."

我不想和你们讨论感知机的任何数学原理，但如果你用现代的眼光来看它，我们可能会称之为线性分类器，我们将在下周三的讲座中讨论这个。这个感知机让很多人非常兴奋。它让人们想到："哇，这是一种机制，可以让机器从数据中学习新东西，而无需人们明确地编程规定它将如何工作。"

And all of that kind of came to a crashing halt in 1969 when Marvin Minsky and Seymour Papert published this infamous book called "Perceptrons" in 1969. What Minsky and Papert pointed out in their book basically was that perceptrons are not magical devices. The perceptron is a particular learning algorithm, and there are certain types of functions that it can learn to represent and other types of functions that it cannot learn to represent.

而这一切在1969年戛然而止，当时Marvin Minsky和Seymour Papert在1969年出版了这本声名狼藉的书，名为《感知机》。Minsky和Papert在书中基本上指出，感知机并非神奇的设备。感知机是一种特定的学习算法，有些类型的函数它可以学会表示，而其他类型的函数它则无法学会表示。

In particular, they pointed out that the XOR function is not something that is learnable by the linear perceptron learning algorithm that we'll talk about again a little bit more next week. And this sudden realization, well, okay, so the normal story that gets told is that the sudden realization from this book was that, "Oh, these learning algorithms are not magical; there's things they can't learn," and people just lost interest in the field, and work in learning, work in perceptrons, kind of dried up for a period of time following the release of this book.

具体来说，他们指出，异或函数是无法通过线性感知机学习算法学会的，我们下周会再稍微详细讨论这个算法。这个突然的认识，好吧，通常流传的说法是，这本书带来的突然认识是："哦，这些学习算法并不神奇；有些东西它们学不会。"于是人们对这个领域失去了兴趣，关于学习、关于感知机的研究，在这本书出版后的一段时间里几乎枯竭了。

But what's interesting is that I think nobody actually read the book because, um, if you actually read it, there are sections where they say, "Yes, the original perceptron learning algorithm is quite limited, and it can only represent certain functions, but there's something else. There's another potential version of the algorithm called a multi-layer perceptron that actually can learn many, many, many, many different types of functions and is very flexible in its representations."

但有趣的是，我认为实际上没人读过这本书，因为，嗯，如果你真的读了，书中有一些部分他们写道："是的，原始的感知机学习算法相当有限，它只能表示某些函数，但还有别的东西。还有该算法的另一个潜在版本，叫做多层感知机，它实际上可以学习非常多、非常多不同类型的函数，并且在表示上非常灵活。"

But that point got lost in the headlines at the time, and nobody realized that, and people just heard that perceptrons didn't work and were dead. So you should definitely read the assigned reading. But then going forward, because then going forward quite some amount of time, we skip ahead to 1980.

但这一点在当时被淹没在头条新闻中，没有人意识到，人们只是听说感知机不行了，完蛋了。所以你们一定要读指定的阅读材料。然后继续向前，因为之后过了相当长一段时间，我们跳到1980年。

And there was this very influential paper proposing a system called the neocognitron that was developed by Fukushima, a Japanese computer scientist. And he was directly inspired by Hubel and Wiesel's idea of this hierarchical processing of neurons. Remember, Hubel and Wiesel talked about these simple cells, these complex cells, these hierarchies of neurons that could gradually learn to see more and more complex visual stimuli in the image. So Fukushima proposes this computational realization of Hubel and Wiesel's formulation, which he called the neocognitron.

当时有一篇非常有影响力的论文，提出了一个名为"认知机"的系统，由日本计算机科学家福岛邦彦开发。他直接受到了Hubel和Wiesel关于神经元分层处理思想的启发。记得吗，Hubel和Wiesel谈到了这些简单细胞、复杂细胞，这些神经元的层次结构能够逐渐学会看到图像中越来越复杂的视觉刺激。因此，福岛提出了一个计算实现，基于休伯尔和威泽尔的构想，他称之为"新认知机"。

The neocognitron interleaved two types of operations. One were these computational simple cells that, if we were to look at them with modern terminology, would look very much like convolution. And the latter was these computational realizations of complex cells that, again under modern terminology, look very much like the pooling operations that we use in modern convolutional networks.

新认知机交错使用了两种类型的操作。一种是这些计算简单细胞，用现代术语来看，它们非常类似于卷积操作。另一种是这些计算复杂细胞的实现，同样用现代术语来看，它们非常类似于我们在现代卷积网络中使用的池化操作。

So what's striking is that even back in this neocognitron from 1980, there was an overall architecture and overall method of processing that looked very similar to this famous AlexNet system that swept in 2012. Even the figures that they have in the papers look pretty similar, so they've got to be the same thing, right?

令人惊讶的是，早在1980年的这个新认知机中，就已经有了一个整体的架构和处理方法，看起来与2012年横扫天下的著名AlexNet系统非常相似。甚至他们论文中的图示看起来也相当相似，所以它们肯定是同一个东西，对吧？

But what was striking about the neocognitron is that they defined this computational model. They had the right idea of convolution and pooling and hierarchy, but they did not have any practical method to train the algorithm. Because remember, there's a lot of learning weights in this system, a lot of connections between all the neurons inside. They need to be set somehow.

但新认知机的惊人之处在于，他们定义了这个计算模型。他们有了卷积、池化和层次结构的正确概念，但他们没有任何训练算法的实用方法。因为请记住，这个系统中有很多需要学习的权重，内部所有神经元之间有很多连接。它们需要以某种方式被设定。

And even then, Fukushima did not have an efficient algorithm for learning to properly set all those free weight parameters in the system based on data.

即便如此，福岛也没有一个高效的算法来学习如何根据数据正确设定系统中所有这些自由权重参数。

So then a couple years later, there was this again massively influential paper by Rumelhart and Hinton and Williams in 1986 that introduced the backpropagation algorithm for training these multi-layer perceptrons.

几年后，鲁梅尔哈特、辛顿和威廉姆斯在1986年发表了一篇极具影响力的论文，引入了用于训练这些多层感知机的反向传播算法。

Remember that in the perceptrons book, there was this thing called a multi-layer perceptron that was thought to be very powerful in its ability to represent and learn to approximate functions. Well, the backpropagation in this paper that introduced the backpropagation algorithm was one of the first times that people were able to successfully and efficiently train these deeper models with multiple layers of computations.

记得在《感知机》一书中，有一种叫做多层感知机的东西，被认为在表示和学习逼近函数方面能力非常强大。那么，这篇引入反向传播算法的论文中的反向传播，是人们首次能够成功且高效地训练这些具有多层计算的更深模型。

And this looks very much like a modern neural network that we're using today. If you look at it, you kind of look at this paper and you look through it, you'll see they talk about gradients and they talk about Jacobians and all this kind of mathematical terminology that we think about today when we're building and training neural networks.

这看起来非常像我们今天使用的现代神经网络。如果你看一下，看看这篇论文并通读它，你会发现他们谈论梯度，谈论雅可比矩阵，以及所有我们今天在构建和训练神经网络时会想到的这类数学术语。

So these look very much like the modern fully connected networks that we still use today. They are sometimes called multi-layer perceptrons in homage to this long history.

所以这些看起来非常像我们今天仍在使用的现代全连接网络。它们有时被称为多层感知机，以向这段悠久的历史致敬。

So now that got a lot of people, or a lot of small groups of people, sort of get really excited about putting networks together and try to figure out different types and structures of neural networks that could be built and trained, powered by this new backpropagation algorithm.

于是，这吸引了许多人，或者说许多小型研究团体，开始对组合网络感到非常兴奋，并尝试探索可以构建和训练的不同类型和结构的神经网络，这一切都得益于这种新的反向传播算法。

And one of the most influential works at that time was a Yann LeCun et al. paper in 1998 that introduced the idea of a convolutional neural network.

当时最具影响力的成果之一是Yann LeCun等人在1998年发表的论文，该论文引入了卷积神经网络的概念。

So this looks again very much like the Fukushima algorithm that we spoke about. What they did here is they took Fukushima's kind of idea of convolution and pooling and multiple layers inspired by the visual system, and combine that with the backpropagation algorithm from Rumelhart's paper in 1986.

所以这看起来又非常像我们之前谈到的福岛算法。他们所做的是，采纳了福岛受视觉系统启发的卷积、池化和多层的思想，并将其与鲁梅尔哈特1986年论文中的反向传播算法结合起来。

And with that combination, they were able to train these convolutional, these very large at the time, convolutional neural networks that could learn to recognize different types of things and images.

通过这种结合，他们能够训练这些卷积神经网络，这些在当时规模非常大的卷积神经网络，可以学习识别不同类型的事物和图像。

And this was a hugely successful system. I think it actually was very successful commercially. So in addition to being a piece of very influential academic research, it was also deployed in a commercial system by NEC labs.

这是一个非常成功的系统。我认为它在商业上实际上也非常成功。因此，除了是一项极具影响力的学术研究外，它还被NEC实验室部署在一个商业系统中。

And for a period of time, this convolutional neural net system developed by that group was actually being used to process the handwriting on a lot of checks that were written in the United States at that time.

有一段时间，该小组开发的这个卷积神经网络系统实际上被用来处理当时美国大量支票上的手写体。

One thing that I found stated that up to 10% of all checks in the United States were actually having their numbers on the check being read automatically by these convolutional neural net systems in the late 90s and early 2000s.

我发现有资料称，在90年代末和21世纪初，美国高达10%的支票，其上的数字实际上都是由这些卷积神经网络系统自动读取的。

And again, if you look at exactly what this algorithm is doing, LeNet was kind of like this. The algorithm here was called LeNet after Yann LeCun. And then if you look at the structure of the algorithm, it looks very similar, almost identical, to the types of algorithms that were used in AlexNet nearly 30 years later.

同样，如果你仔细看看这个算法在做什么，LeNet大致是这样的。这个算法以Yann LeCun的名字命名为LeNet。然后，如果你看算法的结构，它与近30年后AlexNet中使用的算法类型看起来非常相似，几乎一模一样。

So then, emboldened by the success, there were again a small group of people throughout the late 90s and early 2000s who were really interested in trying to move and push neural net systems and figure out ways to train neural net systems that were ever bigger, ever deeper, ever wider, and could be used on an increasing variety of tasks.

于是，受到成功的鼓舞，在整个90年代末和21世纪初，又有一小群人真正感兴趣于尝试推动神经网络系统的发展，并找出方法来训练规模更大、更深、更宽的神经网络系统，使其能够用于越来越多的任务。

And around this period of time in the 2000s was when the term deep learning first emerged, where the term deep was meant to refer to multiple layers in these neural network type algorithms.

大约在21世纪初的这段时间，"深度学习"这个术语首次出现，其中"深度"一词意指这些神经网络类型算法中的多层结构。

And so this was really not a super mainstream research area at this time. There was a relatively small number of research groups and relatively small number of people studying these ideas at this time.

因此，这在那时并不是一个非常主流的研究领域。当时只有相对较少的研究团体和相对较少的人在研究这些想法。

But I think a lot of the fundamentals that we are reaping the rewards of now were really developed during this period of time in the 2000s, when people started figuring out all the new modern tricks to train different types of neural network systems.

但我认为，我们现在受益的许多基础，实际上是在21世纪初的这段时间发展起来的，当时人们开始摸索出训练不同类型神经网络系统的所有现代新技巧。

So that finally brings us back to AlexNet. And then in 2012, we had this great confluence of this great computer vision task called ImageNet that people in computer vision were super excited about, we had these new techniques, convolutional neural networks and efficient ways to train them that had been developed by this parallel research community, and everything just seemed to come together just in time in 2012.

这最终将我们带回到AlexNet。然后在2012年，我们迎来了一个伟大的汇合：计算机视觉领域的人们为之超级兴奋的、名为ImageNet的伟大计算机视觉任务；我们有了这些新技术——卷积神经网络及其高效的训练方法，这些是由这个并行研究社区发展起来的；一切似乎恰好在2012年汇聚到了一起。

So then from 2012 to present day, we've seen an absolute explosion in the usage of convolutional and other types of neural networks across both computer vision and across other types of related areas in AI and across computer science.

于是，从2012年至今，我们见证了卷积神经网络和其他类型神经网络在计算机视觉、人工智能其他相关领域以及整个计算机科学中的使用呈爆炸式增长。

So here on the left, we have the Google Trends for deep learning that shows you this massive sort of exponential growth that really took off starting in 2012. And on the right, this is a photo I took at the computer vision and pattern recognition conference this summer in 2019.

左边是"深度学习"的谷歌趋势图，它向您展示了这种从2012年真正开始腾飞的、巨大的指数级增长。右边是我在2019年夏天计算机视觉与模式识别会议上拍摄的一张照片。

So this is one of the premier venues for academic publications in computer vision. And here is a graph that they were showing at the keynote for that conference, where they showed on the x-axis the year of the conference and on the y-axis the number of submitted and accepted papers in this conference shows that even though the last five to ten years have resulted in a massive explosion of machine learning systems, both in popular perception, especially from Google Trends, as well as a massive increase in academic interest in both machine learning systems and computer vision systems, as evidenced by this fine spot on the right.

这是计算机视觉领域顶级的学术出版物发表场所之一。这是他们在该会议主题演讲中展示的一张图表，其中x轴是会议年份，y轴是本次会议提交和接收的论文数量，这表明尽管过去五到十年间机器学习系统出现了爆炸式增长，无论是在大众认知中（尤其是从谷歌趋势来看），还是学术界对机器学习系统和计算机视觉系统的兴趣都大幅增加，右侧的这个亮点就是明证。

If you look around the field today, we see convolutional networks and other types of deep neural networks being used for just about every possible application of computer vision that you can imagine. So from 2012, these convolutional networks are really everywhere. They're getting used for a wide diversity of tasks like image classification, where we want to put labels on images, or image retrieval, where we want to retrieve images from collections.

如今放眼整个领域，我们可以看到卷积网络和其他类型的深度神经网络被用于几乎所有你能想象到的计算机视觉应用。因此，从2012年起，这些卷积网络可谓无处不在。它们被用于极其多样化的任务，如图像分类（我们想给图像打上标签）、图像检索（我们想从图库中检索图像）。

Things like object detection, where we want to recognize the positions of objects in images while simultaneously labeling them, or image segmentation, where we want to label the pixels, going back to this idea of semantic grouping you saw for computer vision in the 90s, where we want to label groups of pixels as being part of a cohesive whole.

还有像目标检测（我们想识别图像中目标的位置并同时给它们打标签）、图像分割（我们想给像素打标签，这回到了90年代计算机视觉中你见过的语义分组概念，即我们希望将像素组标记为一个连贯整体的一部分）。

Convolutional networks are going to be used for things like video classification or activity recognition. They're going to be used for things like pose estimation, where you want to say how the exact geometric poses of people are arranged in images. Even for things that don't really feel like classical computer vision, like playing Atari games by processing the visual input of the Atari game with a convolutional neural network and combining that with other sorts of learning techniques in order to jointly learn a visual representation of the video game world as well as how to play in that world.

卷积网络将被用于视频分类或活动识别等任务。它们将被用于姿态估计等任务，即你想描述图像中人物的精确几何姿态是如何排列的。甚至对于那些感觉不太像经典计算机视觉的任务，例如通过卷积神经网络处理雅达利游戏的视觉输入，并结合其他类型的学习技术来玩雅达利游戏，以便同时学习视频游戏世界的视觉表征以及如何在该世界中游戏。

Convolutional neural networks are also getting used for visual tasks that are about visual data that humans aren't very good at. So convolutional networks are getting used in things like medical imaging to diagnose different types of tumors, diagnose different types of skin lesions, and other medical conditions. They're going to be used in galaxy classification. They're getting used in tons of scientific applications like classifying whales or elephants or other types of animals.

卷积神经网络也被用于处理人类不太擅长的视觉数据相关的视觉任务。因此，卷积网络正被用于医学成像等领域，以诊断不同类型的肿瘤、不同类型的皮肤病变以及其他医疗状况。它们将被用于星系分类。它们被用于大量的科学应用，如鲸鱼、大象或其他类型动物的分类。

Because there's this problem where scientists want to go out into the world and collect a lot of data, and then be able to use images and visual recognition as a kind of universal sensor to make use of all this data that they collect and gain insights into their particular field of expertise that they're interested in. And we've seen computer vision and convolutional networks branch out into all these other areas of science and just open up and unlock lots of new applications just across the board.

因为存在这样一个问题：科学家们希望走出去收集大量数据，然后能够将图像和视觉识别作为一种通用传感器，利用他们收集的所有数据，并深入了解他们感兴趣的特定专业领域。我们已经看到计算机视觉和卷积网络扩展到所有这些其他科学领域，全面开启并解锁了大量新的应用。

Convolutional networks are going to be used for all kinds of fun tasks like image captioning, where we can build systems that can write natural language descriptions of images. These are using convolutional networks. We can use convolutional networks for generating art, so we can make all these kind of psychedelic portraits again using a convolutional neural network.

卷积网络将被用于各种有趣的任务，如图像描述，我们可以构建能够为图像撰写自然语言描述的系统。这些都使用了卷积网络。我们可以使用卷积网络来生成艺术，因此我们可以再次使用卷积神经网络制作所有这些迷幻风格的肖像。

So then we might ask, what was it that happened in 2012 that made all of this take off? Well, I think the jury's out and we'll have to see what the story is 50 years from now. But my personal interpretation is that it was a combination of three big components that came together all at once.

那么，我们可能会问，2012年发生了什么让这一切腾飞？嗯，我认为尚无定论，我们得看50年后的历史如何书写。但我个人的解读是，这是三个重要组成部分同时汇聚在一起的结果。

One was the set of algorithms that we saw. There was a stream of people working on deep learning and convolutional neural networks in machine learning who had developed these powerful set of tools for representing learning functions and for learning, like the backpropagation algorithm we saw. The second stream was data. With the rise of digital cameras, the internet, and people developing crowdsourcing, we were able to collect unprecedented labeled data that could be used to train these systems.

其一是我们看到的算法集合。有一批从事机器学习的深度学习和卷积神经网络研究的人员，他们开发了这套强大的工具集，用于表示学习函数和进行学习，比如我们看到的反向传播算法。第二个组成部分是数据。随着数码相机、互联网的兴起以及众包的发展，我们能够收集前所未有的标注数据，用于训练这些系统。

And the third piece that we haven't really talked about was the massive rise in computational resources that has been continually happening throughout the history of computer science. So one graph that I put together that I find particularly striking is looking at the gigaflops of computation per dollar as a function of time.

第三个我们尚未真正讨论的部分是计算资源的大幅增长，这在计算机科学史上一直在持续发生。因此，我整理了一张图表，我认为特别引人注目，它展示了每美元所能获得的千兆次浮点运算（Gigaflops）随时间的变化。

Here on the blue you can see these are different types of CPUs, central processing units, the thing that's in your laptop. They get faster but not that much faster over time. But starting in 2008, there was some really interesting developments with GPUs, graphics processing units. These were special-purpose pieces of hardware that were originally developed to pump out pixels in computer graphics applications.

在蓝色部分，你可以看到这些是不同类型的CPU（中央处理器），也就是你笔记本电脑里的东西。它们会变快，但随着时间的推移，速度提升并不那么大。但从2008年开始，GPU（图形处理器）出现了一些非常有趣的发展。这些是专用硬件，最初是为计算机图形应用中输出像素而开发的。

But around 2008, people started developing techniques to run generalized programs on these graphics processing units. And then over time, these techniques became more and more easy to write general-purpose scientific code and mathematical code to run on these massively parallel graphics processing units.

但大约在2008年，人们开始开发技术，在这些图形处理器上运行通用程序。随着时间的推移，这些技术使得编写能在这些大规模并行图形处理器上运行的通用科学代码和数学代码变得越来越容易。

And then if you look at the timeline from 2006 to 2017 and look at the gigaflops per dollar on these graphics processing units, you can see that although this exponential Moore's Law may not have held up for CPUs, it actually has been holding up for GPUs. We actually have seen exponential increases in GPU computing power over time over the last 10 years.

然后，如果你观察2006年到2017年的时间线，并查看这些图形处理器上每美元的千兆次浮点运算能力，你会发现，尽管这种指数级的摩尔定律可能对CPU不再成立，但它实际上对GPU仍然成立。实际上，在过去10年里，GPU的计算能力确实随着时间呈指数级增长。

And if you look at maybe the AlexNet system in 2012, it was using this GTX 580 GPU that was very, very exciting at the time if you're a gamer. And if you push it on into more recent cards like the GTX 980 Ti or more recently a 2080 Ti, then you can see that the cards we have even five years later are literally exponentially more powerful than the cards that were around in 2012.

如果你看看2012年的AlexNet系统，它使用的是GTX 580 GPU，这在当时对游戏玩家来说是非常令人兴奋的。如果你再看更近期的显卡，如GTX 980 Ti或更近期的2080 Ti，你会发现，即使是五年后的显卡，其性能也比2012年左右的显卡强了指数级。

So I think that it was really this confluence of algorithms, of data, and of massive increase in computation fueled by advances in GPUs that led to all this magic happening in 2012, that led to all these new applications of convolutional networks on different types of computer vision.

因此，我认为，正是算法、数据以及由GPU进步推动的计算能力大幅提升这三者的汇合，才导致了2012年所有这些奇迹的发生，才导致了卷积网络在各种计算机视觉类型上的所有这些新应用。

And in recognition of all of this, in recognition of the impact of computer vision and deep learning across the field, the 2018 Turing Award was awarded to Yoshua Bengio, Geoffrey Hinton, and Yann LeCun for their work on pioneering many of the deep learning ideas that we'll learn throughout this class.

为了表彰所有这些成就，为了表彰计算机视觉和深度学习对整个领域的影响，2018年图灵奖授予了约书亚·本吉奥、杰弗里·辛顿和扬·勒昆，以表彰他们在开创我们将在这门课程中学到的许多深度学习思想方面的工作。

And for those of you who don't know, the Turing Award is basically considered the Nobel Prize equivalent in the field of computer science. So this just happened last year, and this was just a recognition that this has been a massively influential piece of research that's been changing all of our lives over the last several years.

对于那些不了解的人来说，图灵奖基本上被认为是计算机科学领域的诺贝尔奖。这件事就发生在去年，这只是一个认可，表明这是一项具有巨大影响力的研究，在过去几年里一直在改变我们所有人的生活。

But I think it's important to stay humble and realize that despite all of the successes that we've seen in convolutional networks, in deep learning and computer vision, I think we're really still a long way away from building systems that can perceive and understand visual data to the same fidelity and power and strength as humans.

但我认为保持谦逊很重要，要认识到尽管我们在卷积网络、深度学习和计算机视觉领域取得了诸多成功，我认为我们距离构建出能够以与人类相同的保真度、能力和强度来感知和理解视觉数据的系统，仍然有很长的路要走。

One image that I like to use to exemplify this is this example. So if we were to send this to a convolutional network, it would probably say "person" or "scale" or "locker-room" maybe. But if we were to look at this, you see quite a different story.

我喜欢用这张图片来举例说明这一点。所以，如果我们将这张图片输入卷积网络，它可能会说“人”或“秤”或“更衣室”之类的。但如果我们看这张图片，你会看到一个截然不同的故事。

You see a guy standing on a scale. You know how scales work, which requires some idea of physics. You know that he's looking at the scale. You know that he's trying to measure his own weight. You know that people tend to be self-conscious about their weight.

你看到一个家伙站在秤上。你知道秤的工作原理，这需要一些物理知识。你知道他正看着秤。你知道他正试图测量自己的体重。你知道人们往往对自己的体重很在意。

You know that the person behind him is stepping on the scale and pushing down. Because of your knowledge of physics, you know that that's going to make the scale read a bigger number. Because of your knowledge of that guy's psychology, you'll know that that might make him feel embarrassed or uncomfortable because he thinks he ate too much.

你知道他身后的人正踩在秤上往下压。根据你的物理知识，你知道这会导致秤显示一个更大的数字。根据你对那个家伙心理的了解，你会知道这可能会让他感到尴尬或不舒服，因为他觉得自己吃太多了。

And then you also know who that person pushing down on the scale is, and because of your knowledge of who he is, it makes it may be surprising that he's acting in this way. You understand you can see the people behind him watching this scene and laughing and understand that you need to know how the people look at each other.

然后你也知道那个在秤上往下压的人是谁，并且由于你知道他是谁，这可能会让你对他以这种方式行事感到惊讶。你明白你可以看到他身后的人们在观看这个场景并大笑，并且明白你需要知道人们是如何看待彼此的。

You understand that maybe they're surprised that this guy is doing this thing that's causing this guy to be embarrassed. So there's a lot going on in this image that we, as humans, as visually intelligent humans, could understand and perceive.

你明白也许他们惊讶于这个家伙正在做这件事，导致那个家伙尴尬。所以，这张图片中包含了很多内容，我们作为人类，作为具有视觉智能的人类，能够理解和感知。

And I think we're a long way away from building computer vision systems that can match that level of visual fidelity. But I'm hoping that as we move forward and continue to advance the field, maybe one day we'll get there.

而且我认为，我们距离构建出能够匹配那种视觉保真度水平的计算机视觉系统还有很长的路要走。但我希望随着我们不断前进并继续推动该领域发展，也许有一天我们会达到那个目标。

But in the meantime, I think that computer vision technology really has massive and massive potential to improve all of our lives. It'll make our lives more fun through sort of new video applications, applications in VR, AR.

但与此同时，我认为计算机视觉技术确实具有巨大的潜力来改善我们所有人的生活。它将通过新型视频应用、VR和AR应用，让我们的生活更有趣。

It'll make our transportation safer with advances in autonomous vehicles. It'll lead to improvements in medical imaging and diagnosis.

随着自动驾驶汽车的进步，它将使我们的交通更安全。它将带来医学成像和诊断的改进。

And overall, I think computer vision as a whole has a massive ability and potential to continue leading to massive improvements in all of our day-to-day lives. So that's why I think we should be studying computer vision.

总的来说，我认为计算机视觉作为一个整体，拥有巨大的能力和潜力，能够持续引领我们日常生活的巨大改善。这就是为什么我认为我们应该研究计算机视觉。

That's why I make it excited to be teaching this class this year. So that basically covers our brief history of computer vision, of deep learning, except there's one little spot on the timeline that we didn't fill out, and that's this class.

这也是为什么我对今年教授这门课程感到兴奋。那么，这基本上涵盖了计算机视觉、深度学习的简史，只是时间线上有一个小点我们没有填上，那就是这门课程。

So with that, it's time to, if there was any, if there's any questions about historical stuff, then we're going to move on to course logistics. You're out? No? Okay, great.

那么，说到这里，是时候了，如果有什么，如果对历史内容有任何问题，那么我们将转向课程后勤。你们有问题吗？没有？好的，很好。

So for staff who are here, I'm Justin Johnson. I'm a new assistant professor here in the computer science and engineering department. This is the first class I'm teaching here at Michigan. This is the first time I've been in this room, so glad I found it.

那么，对于在场的教职员工，我是贾斯汀·约翰逊。我是计算机科学与工程系的新助理教授。这是我在密歇根大学这里教授的第一门课。这是我第一次在这个教室，很高兴我找到了它。

About the laptop identity, but I'm excited to be here, excited to be teaching you guys this class. We have an amazing team of graduate student instructors that are going to be helping us out this semester.

关于笔记本电脑身份，但我很兴奋能在这里，很兴奋能教你们这门课。我们有一个很棒的研究生助教团队，他们本学期将帮助我们。

How is your guy? The standard will be docile. So these guys are all experts in computer vision. They're all PhD students here working in video understanding and generative models, robustness and generalization, and vision plus language.

你们怎么样？标准会是温顺的。所以这些家伙都是计算机视觉专家。他们都是这里的博士生，研究方向包括视频理解和生成模型、鲁棒性和泛化性，以及视觉加语言。

So if you have questions about those particular research areas, you should go talk to them. So how to contact us? So this is an important slide. Right, taking pictures of this is a good idea.

所以，如果你对那些特定的研究领域有问题，你应该去和他们谈谈。那么如何联系我们？这是一张重要的幻灯片。对，拍下这张幻灯片是个好主意。

I probably could feel some kindness to something. We'll extract information from them automatically. But we keep on a course website that's up at this URL. The course website has all the information that you'll need for the course throughout the quarter.

我可能能感受到一些善意。我们会自动从中提取信息。但我们维护着一个课程网站，网址就是这个。课程网站包含了你整个季度课程所需的所有信息。

You can find the syllabus, the schedule. You'll find links to assignments, links to lecture videos, assuming the lecture capture works properly. Really important, we're going to use Piazza for most communication with you guys.

你可以找到教学大纲、课程表。你会找到作业链接、讲座视频链接，前提是讲座录制正常。非常重要的一点是，我们将主要使用 Piazza 与你们进行交流。

So we really encourage you, if you have questions about the course material, we're going to use Piazza as our main mechanism to communicate back and forth with you guys. So if you have questions about course content, questions about material, post on Piazza.

所以我们真的鼓励你们，如果你们对课程材料有问题，我们将使用 Piazza 作为与你们来回交流的主要机制。所以，如果你对课程内容、材料有问题，请在 Piazza 上发帖。

And similarly, if we need to make announcements back to the class, to announce things like homework due dates, changes to logistics, we'll be announcing all of those through Piazza. So it's really important that you guys get signed up as quickly as possible.

同样地，如果我们需要向全班发布通知，比如宣布作业截止日期、后勤变更，我们都会通过 Piazza 发布。所以你们尽快注册非常重要。

And one piece of note is that please don't post any code on public questions on Piazza. If you do need to ask particular questions about code, we ask that you make private questions that are visible only to you and the instructors.

需要注意的一点是，请不要在 Piazza 的公开问题上发布任何代码。如果你确实需要询问关于代码的具体问题，我们要求你提出私密问题，只对你和讲师可见。

So we will have Canvas. I think I need to set that up still this week. But we'll really use Canvas primarily for just turning in assignments. Mostly we'll be using Piazza and the course website for this class.

我们将使用 Canvas。我想我这周还需要设置一下。但我们主要将 Canvas 用于提交作业。这门课我们将主要使用 Piazza 和课程网站。

We will have office hours, both me and the GSIs, starting next week. And you'll be able to find the times and locations of the office hours on the Google Calendar that I set up here.

我们将安排办公时间，包括我和研究生助教，从下周开始。你可以在我在这里设置的 Google 日历上找到办公时间的时间和地点。

Finally, we really want the vast majority of communication you do with us to be through Piazza. But if you have some kind of very sensitive topic that you would prefer to discuss directly with me, then you can email me directly.

最后，我们真的希望你们与我们进行的大部分交流都通过 Piazza。但如果你有某种非常敏感的话题，更愿意直接与我讨论，那么你可以直接给我发电子邮件。

But for the vast majority of circumstances, you should be going through Piazza for course communication. That will ensure that everyone, all of the teaching staff, is able to help you in a timely manner.

但在绝大多数情况下，你应该通过 Piazza 进行课程交流。这将确保每个人，所有的教学人员，都能及时帮助你。

And if you're making public questions, it lets you all help each other and learn collectively and learn from each other's mistakes. I think Piazza is a really great learning tool for that.

而且如果你提出公开问题，它可以让大家互相帮助，共同学习，并从彼此的错误中学习。我认为 Piazza 是一个非常好的学习工具。

So we're going to have an optional textbook. There's no required textbook for this class. On the schedule you'll find on the website, there will be recommended readings for all of the lectures.

我们将有一本可选教材。这门课没有指定教材。在网站上的课程表里，你会找到所有讲座的推荐阅读材料。

This textbook is totally optional and it's completely available for free online. I probably could feel some kindness to something we'll extract abuse formation out from them automatic but we kept on a course website that's up in this URL. The course website has followed the information that you'll need for others throughout the quarter. You can find the syllabus, the schedule, you'll find links to assignments, they're your clients links the lecture videos assuming a set of lecture capture properly really important. We're going to use the Gaza or most communication with you guys so we really encourage you if you have questions for the course material. We're going to use the Gala's F is our main mechanism to communicate back and forth with you guys so if you have questions about course content questions about material post post on Piazza. And similarly if we when we need to make announcements back to the class to announce thing is like of the homeworks about changes to logistics will be announcing all of those through Piazza so it's really important that you guys get signed up as quickly as possible. And one piece of the note is that please don't post any code on P about public questions on Piazza. If you do need to ask particular questions about code we ask that you make private questions that are visible only to you start only to the instructor.

这本教材完全是可选的，并且完全可以在网上免费获取。我可能对某些事情感到一些善意，我们会从中自动提取滥用信息，但我们维护了一个课程网站，网址就是这个。课程网站上包含了你们整个季度所需的信息。你们可以找到教学大纲、课程安排、作业链接（这些是你们的客户端链接）以及讲座视频，前提是讲座录制正常，这非常重要。我们将主要使用Piazza与大家进行沟通，因此我们强烈鼓励你们，如果对课程材料有疑问，请使用Piazza。Piazza是我们与你们来回沟通的主要机制，所以如果你们对课程内容或材料有疑问，请在Piazza上发帖提问。同样，当我们需要向全班发布通知时，比如关于作业或后勤变更的信息，我们都会通过Piazza宣布。因此，你们尽快注册Piazza非常重要。需要注意的一点是，请不要在Piazza的公开提问中发布任何代码。如果你们确实需要询问关于代码的具体问题，我们要求你们发起仅对你们自己和讲师可见的私密提问。

We will have canvas I I think I need to suck that up still this week but we're really use canvas primarily for just turning in assignments mostly will be using Piazza and the course website for this class. We will have office hours both me and the GSIS started next week and you'll be able to find the times and locations of the office hours on the Google Calendar that I set up here. Finally we really want most the vast majority of communication you do with us should be should be through Piazza but if you have some kind of very sensitive topic that you would prefer to discuss directly with me then you can email me directly. But for the vast majority of circumstances you should be going through Piazza for four course communication that will ensure that everyone all will all open teaching staff is able to help you in a timely manner and if you're able to make and if you're making public questions and lets you all help each other and learn collectively and learn from each other's mistakes. I think Piazza is really great learning tool for that so we're going to have an optional test in text level there's no required textbook for this class on this schedule you'll find on the website there will be recommended readings for all of the lectures this this text this textbook is totally optional and it's completely available for free online you don't have to visit being purchased a copy if you don't watch it.

我们会有Canvas，我想我这周还需要完善一下，但我们主要将Canvas用于提交作业，本课程大部分情况下将使用Piazza和课程网站。我和助教们下周将开始安排办公时间，你们可以在我这里设置的Google日历上找到办公时间的具体时间和地点。最后，我们真的希望你们与我们进行的大部分沟通都通过Piazza进行，但如果你有某些非常敏感的话题更愿意直接与我讨论，那么你可以直接给我发邮件。然而，在绝大多数情况下，你应该通过Piazza进行课程沟通，这将确保所有教学人员都能及时帮助你。如果你能提出公开问题，这能让你们互相帮助、共同学习，并从彼此的错误中吸取教训。我认为Piazza在这方面是一个非常好的学习工具。我们将有一本可选的测试文本，本课程没有指定教材。在课程网站上，你会找到每次讲座的推荐阅读材料。这本教材完全是可选的，并且可以在网上免费获取，如果你不需要，不必购买。

So on course content and grading we're gonna the main bulk of the course is gonna be six programming assignments oh is that a problem is that's I think these are gonna be really exciting assignments we're going to use Python high court and Google collab and we're gonna walk you through the implement eight the detailed implementations a lot of the ideas that we talked about in lecture. We will have a midterm exam and we will have a final exam but there will not be an important project the majority of the stuff will be learning us through the programming assignments so we have a leak policy so don't turn it in late but more seriously you all get three free big days of use on your homework assignments you don't have to tell us beforehand just randomly and then I can automatically once you've exhausted your late days I'm gonna take 25% earth-protecting something like that's reasonable are there any questions about content of policies late days anything like that yeah over here yeah so the question is will the course materials be available for people not on the waitlist and they will be as really available as I can possibly make not so even if you can't get an open the class you can definitely feel free to follow along with the lecture slides with lecture videos what the course yeah question yeah it's up to you you're all grown up so you can use the funny lengthiest first time as you want but we you take too many and we were zero so I don't recommend that but three ladies you can use as many as you want and expert ladies you somebody as you want for sign that but we'll just take this tape off lines and sizes here.

关于课程内容和评分，本课程的主要内容将是六个编程作业。哦，这有问题吗？我认为这些作业会非常令人兴奋。我们将使用Python、高级库和Google Colab，并会引导你们实现我们在讲座中讨论的许多想法的详细代码。我们将有一次期中考试和一次期末考试，但不会有一个重要的大型项目。大部分学习将通过编程作业进行。我们有一个迟交政策，所以请不要迟交。但更具体地说，你们每人都可以在作业中获得三次免费的"迟交日"，无需事先告知，可以随意使用。一旦用完了迟交日，我会自动扣除25%的分数，这应该是合理的。关于课程内容、政策、迟交日之类有什么问题吗？好的，这边。问题是：课程材料会对不在候补名单上的人开放吗？它们会尽可能开放。所以，即使你无法正式选上这门课，你绝对可以自由地跟着讲座幻灯片和视频学习。是的，问题。是的，这取决于你们，你们都成年了，所以你可以按你想要的任何方式使用这些迟交日。但如果你用得太多次，分数可能就归零了，所以我不推荐那样做。但三次迟交日你可以随意使用，专家级的迟交日你也可以按需申请，但我们会根据这些规则来处理。

Yeah so let's talk about collaboration policy so we really encourage you guys to work together in groups I think it's great to discuss which course material it with your classmates and to learn together but we have a couple of ground rules about that for a collaboration policy one is that everything you submit should be your own work it's fine to talk about ideas with other other students that's great and encourage but you shouldn't be sharing or looking at each other's code word necessarily you can talk about things conceptually but you shouldn't be turning on the same code as as people you work with secondly on the flip side don't share your solution code to other people this means don't post on Piazza don't give it to your to your roommates don't throw down big puzzles because that will make it easier and more tempting for other people to violate collaboration policy and review charities and third when you turn in assignments we have to indicate who you work with what would in turn in your assignment and will be more equal in instructions on that later and in general training in something late or incomplete is much better than potentially violating collaboration policies and not just in this course but more generally any questions about that okay so then of course philosophy what are we here for right all right yeah this class is not learn pipework too many games this class is dogs learn deep learning in ten lines of Python you can find tutorials on the internet that tell you how to do that that's what you want but I think that learning about deep learning in that way does yourself a huge disservice I think that you want to focus on fundamental concepts we want us we want you to understand not just the latest and raised API level API is the raft answer club we want you to understand the fundamentals of how those these guys might have been implemented why I didn't let it the way they work such that when faced with the next piece the next bit of technological tools you you understand the fundamentals and you do them yourself what that means is that you'll be writing a lot of backdrop code yourself in this course I think that it's very important for people to learn how to compute gradients and how and how the computation of gradients affects the overall flow of learning pressure system so for the first several assignments you'll be using no autographs you'll be using Java tensorflow still be deriving and in fluent in your own background your own gradient computations and you will be a better computer scientist born so given that we prefer to move afraid to write inventory for the purpose of pedagogy we encourage you we're going to encourage you to write and stretch rather than relying on existing open source implementations again this will make you a better keep learning practitioners that said we're also practical we will well we're going to give you lots of tools and techniques for debugging and training big neural networks because it's tricky when you can't rely on that find that ten lines of code wrapping around lots of stuff so we're going to talk a lot about how you can practically get these things to work what tips and tricks should you be using when developing and debugging and training their networks we will use state of the art software tools like I taught you tensor flow but only after you've earned your wings by writing a lot of Badcock code yourself we're going to focus on state of the art a lot of the material that we cover in this course despite the long historical context we talked about in this lecture but the majority of the actual concrete implementations and concrete results we've discussed that we'll talk about in this course have been discovered in the last five to ten years so this has a couple of interesting applications for implications for teaching a course that means that there aren't good textbooks for this stuff that means that no you there might not be great resources outside of original piece of original research papers for learning about this stuff so that's going to be maybe a bit of us a bit of a struggle and a bit challenging but on the upside I think it's really exciting to be learning about such deep in our material in a classroom setting so also in philosophy we want you could also have a little bit of fun so we'll be Petzl we'll be covering some some sort of fun topics like image captioning that you've got a good laugh when I put it up here couple slides ago and some B's on a deep dream and our artistic style transfer that lets you use neural networks to generate new pieces of art not just improving our lives so in terms of the course structure the first the bird the take down the first half the course will focus on fundamentals we'll talk about the details of how to implement different types of neural networks we'll cover building a fully connected neural network, convolutional neural networks, recurrent neural networks. We'll talk about how to debug them, how to implement them, how to train them, and they'll be very detailed. By the end of this module, the goal is basically implementing your own convolutional neural network system from scratch.

好的，我们来谈谈合作政策。我们真的鼓励你们分组合作，我认为与同学讨论课程材料、一起学习是非常好的。但关于合作政策，我们有一些基本规则：第一，你们提交的所有内容都应该是自己的作品。与其他学生讨论想法是很好的，也值得鼓励，但你们不应该分享或查看彼此的代码。你们可以从概念上讨论问题，但不应提交与合作伙伴完全相同的代码。第二，反过来，不要把你的解决方案代码分享给别人。这意味着不要在Piazza上发布，不要给你的室友，不要设置太简单的谜题，因为这会让他人更容易、更可能违反合作政策并损害诚信。第三，提交作业时，必须注明你和谁一起合作。稍后会有更详细的说明。总的来说，提交迟交或不完整的作业，远比可能违反合作政策要好，不仅在这门课，在更广泛的意义上也是如此。关于这个有什么问题吗？好的。那么，当然要谈谈课程理念，我们在这里是为了什么，对吧？好的。这门课不是学习花架子或太多游戏。这门课不是用十行Python代码学习深度学习。你可以在网上找到教你这样做的教程，如果你想要的是那个。但我认为以那种方式学习深度学习是对自己的巨大损害。我认为你们应该关注基本概念。我们希望你们理解的不仅仅是最新、最流行的API层面的东西——API只是表面的答案。我们希望你们理解这些工具是如何实现的、为什么以这种方式工作，这样当面对下一个技术工具时，你们能理解其基本原理并自己动手实现。这意味着在这门课程中，你们将亲自编写大量的底层代码。我认为学习如何计算梯度，以及梯度计算如何影响整个学习系统的流程，对每个人来说都非常重要。因此，在前几个作业中，你们将不使用自动微分，你们将使用Java TensorFlow（此处可能有误，应为类似概念），仍然需要推导并熟练掌握自己的梯度计算。这样你们会成为更优秀的计算机科学家。因此，出于教学目的，我们宁愿让你们动手编写底层代码。我们鼓励你们去编写和扩展，而不是依赖现有的开源实现。这同样会使你们成为更好的持续学习实践者。话虽如此，我们也很务实。我们将为你们提供许多调试和训练大型神经网络的工具和技巧，因为当你不能依赖那些封装了很多东西的十行代码时，事情会变得棘手。所以我们将大量讨论如何实际让这些东西工作起来，在开发、调试和训练网络时应该使用哪些技巧。我们将使用最先进的软件工具，比如我教你们的TensorFlow，但只有在你们通过自己编写大量底层代码"赢得翅膀"之后。我们将关注最前沿的内容。尽管我们在本讲座中谈到了很长的历史背景，但本课程涵盖的许多材料，以及我们将讨论的大多数具体实现和具体成果，都是在过去五到十年内发现的。这对课程教学有一些有趣的启示：这意味着没有很好的教科书，这意味着除了原始研究论文，可能没有太多其他优秀资源来学习这些东西。所以这可能有点困难，有点挑战性。但另一方面，我认为在课堂环境中学习如此深入的材料是非常令人兴奋的。同样，在理念上，我们也希望你们能有一点乐趣。我们将涵盖一些有趣的主题，比如图像字幕生成（我之前放了几张幻灯片，你们笑得很开心），还有一些关于Deep Dream和艺术风格迁移的内容，让你们可以用神经网络生成新的艺术作品，而不仅仅是改善我们的生活。因此，就课程结构而言，前半部分课程将侧重于基础知识。我们将讨论如何实现不同类型神经网络的细节，我们将涵盖构建全连接神经网络、卷积神经网络和循环神经网络。我们将讨论如何调试它们、如何实现它们、如何训练它们，内容会非常详细。在本模块结束时，目标基本上是让你从头开始实现自己的卷积神经网络系统。

Now, in the second half of the course, we're going to shift a little bit in flavor. Here we're going to focus more on applications and more emerging sort of research topics. So around that point in the course, you'll notice a bit of a shift in tone in the lectures. They will become a little bit less detailed. We will sometimes skip over some of the low-level details and perhaps refer you to papers if you need to know those details. Instead, the lectures will more focus on giving you an overview of how people are using these different things across different applications in computer vision and beyond.

现在，在课程的后半部分，我们的风格会稍有转变。这里我们将更多地关注应用和更多新兴的研究主题。因此，在课程的那个阶段，你会注意到讲座的语气会有些转变。它们会变得不那么详细。我们有时会跳过一些底层细节，如果你需要了解这些细节，可能会让你参考相关论文。相反，讲座将更侧重于概述人们如何在计算机视觉及其他领域的不同应用中使用这些不同的技术。

Well, in the second half, we'll talk about things like object detection, image segmentation, 3D vision, videos. We'll talk about attention, transformers, vision language, generative models. I think it's gonna be a lot of fun. But because there's a lot to get through, the first homework assignment will be out over the weekend. That will cover basically an intro and a warm-up to the Colab and the environment that we'll be using for our quarter. So this should not, this is not intended to be difficult or a long assignment. This should be done over the weekend whenever we get it out. It'll be due after that, and everything you need to do this first homework assignment will get covered in the first lecture.

那么，在后半部分，我们将讨论诸如目标检测、图像分割、3D视觉、视频等内容。我们将讨论注意力机制、Transformer模型、视觉语言、生成模型。我认为这会非常有趣。但由于内容很多，第一次作业将在本周末发布。那基本上是一个介绍，也是对我们本学期将使用的Colab和环境的预热。所以这不应该，这并非旨在成为一个困难或冗长的作业。这应该是在周末，我们发布后就可以完成的。它的截止日期在那之后，完成这第一次作业所需的一切内容都会在第一堂课中涵盖。

So with that said, welcome to the class. Come back on Monday when we'll talk about image classification.

那么，说到这里，欢迎来到本课程。周一回来，届时我们将讨论图像分类。

 you]