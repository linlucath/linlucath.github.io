---
title: 'Startup lecture 12'
publishDate: 2025-10-05
description: 'TODO'
tags:
  - Startup
language: 'English'
heroImage: { src: './default.jpg', color: '#D58388' }
---

Can we keep playing? Wait, okay good. Can we turn it up a little bit, so it's more pumped up? That's loud. Okay, here we go. Okay, so we gotta find the beat and then we gotta clap to the beat. Okay, all right. Okay, that's pretty good guys. All right, we're good. We're good. Thank you. Okay, please stop. Okay, stop the music. Thank you. Okay, cool. Can you put on the presentation, thank you. That'll be the most pumped up thing that ever happens in enterprise software, so the rest is sort of downhill from here. So, thank you for that intro, that well-rehearsed intro, thank you. Confirming the facts as you're saying them, very good. So I'm Aaron Levie, CEO and co-founder of Box. Welcome to this edition of how to build an enterprise software company. That's my understanding. This is the course that you're taking, right, is that correct? No, okay, so. Here's my job today. My job is to try and convince you that everybody else who speaks throughout this whole class is wrong and that you actually wanna build an enterprise software company. And so hopefully we'll be able to kinda work through this, and you'll have a good sense of why it's super cool to be in the enterprise. And why a lot of the perceptions of going into the whole consumer space, where it's, you know, so much fun and everything. Why that's wrong and why you wanna do enterprise software. Who wants to build an enterprise software company? Good, all right. Thank you very much. Hopefully we'll do a vote at the end and hopefully that will not have shrunk. So that's really actually the only goal I have at this point. So, we're gonna talk about three things today. The first is the quick background and history of Box.

我们能继续播放吗？等一下，好的很好。我们能调大一点音量吗，让它更有激情？声音太大了。好的，我们开始吧。好的，我们需要找到节拍，然后跟着节拍鼓掌。好的，没问题。好的，大家做得很好。好的，我们准备好了。我们准备好了。谢谢。好的，请停止。好的，停止音乐。谢谢。好的，很好。你能打开演示文稿吗，谢谢。这将是企业软件领域发生过的最激动人心的事情，所以接下来的内容相比之下就平淡多了。那么，感谢你的介绍，那个精心排练的介绍，谢谢。在你陈述事实时确认它们，非常好。我是 Aaron Levie，Box 的首席执行官和联合创始人。欢迎参加本期"如何建立一家企业软件公司"。这是我的理解。这是你们正在上的课程，对吗，是这样吗？不，好吧，那么。这是我今天的工作。我的工作是试图说服你们，在这整个课程中发言的其他所有人都是错的，而你们实际上想要建立一家企业软件公司。希望我们能够顺利完成这个讨论，你们会清楚地理解为什么进入企业领域非常酷。以及为什么很多人对进入整个消费领域的看法，那里很有趣等等，为什么那是错误的，为什么你们应该做企业软件。谁想建立一家企业软件公司？好的，很好。非常感谢。希望我们最后能进行一次投票，希望这个数字不会减少。所以这实际上是我此刻唯一的目标。那么，我们今天要讨论三件事。第一是 Box 的简要背景和历史。

不完整句子: Cuz when we started the company, we didn't know that we wanted to do enterprise software and so I wanna kind

I'm going to talk about why we decided to go after the enterprise and what we do today. Then I'm going to talk about the major factors that have changed in enterprise software that make it possible to do a startup today in this category. Finally, we'll talk about some patterns that you can exploit and some ways to recognize and ways to actually go build an enterprise software company yourself, and hopefully that'll be some practical, useful advice. Just as a forewarning, my voice has been strained as I've been speaking a lot the past couple days. So hopefully I'll be able to get to that third part of the advice and actually be able to talk. We'll see how we make it, but I'm going to be drinking a lot of water, so I apologize for that. So building for the enterprise is what we're going to talk about.

我将谈谈我们为何决定进军企业市场以及我们目前的业务。然后我会讨论企业软件领域发生的主要变化，这些变化使得如今在这一领域创办初创公司成为可能。最后，我们将探讨一些您可以利用的模式、识别机会的方法以及实际构建企业软件公司的途径，希望这些能提供一些实用建议。需要提前说明的是，由于过去几天讲话过多，我的嗓音有些沙哑。希望我能够完成建议的第三部分并顺利讲解。我们会尽力而为，但我会大量饮水，为此表示歉意。那么，我们将要讨论的是面向企业市场的构建。

We got the idea actually in college back in 2004. And so, was anybody using the internet back in 2004? Okay great. So basically, I don't know if millennials use the internet back then. Sorry. Okay, no more age jokes. All right, here's the point. So, back in 2004, you might remember that there wasn't a lot to do on the internet. It was actually kind of boring right? This is before Facebook. This is certainly before Snapchat. So not a lot of things that you could go to, you couldn't send people 15 second messages and photos that disappeared, cuz you didn't even have a phone to send them photos. So in the internet in 2004, there wasn't a lot going on. This was basically what the internet looked like. Sort of a barren, deserted landscape and this was the internet and just to clarify, the happy camel's Google and the sad camel's Yahoo! And that was effectively the internet and this is accurate, this is the internet in 2004. Yahoo has done a lot better since then but, but back in the middle 2000s they were certainly trying to find their way and Google was sort of taking over the world but this was the entire, this was the extent of the world. So, what we noticed in 2004 in college was that it was, for some reason, and this isn't so much the case today but back in 2004 it was like, really hard to share files. And as simple of an idea as that is now, you go back ten years and it was either really expensive or it was really hard to move data through different corporate networks. So I had an internship at the time, and in the internship, most of my job using data was actually just to copy printed out pages and then put them in cabinets. That's what you do as an intern if you're not a computer science student. So I was really, really good at copying paper. And unfortunately not a skill that has really been useful today. But, it was really hard to share files and then in classroom environments you were constantly working with lots of other people and it was also hard to share files. I went to USC. And USC gave you 50 megabytes of online storage.

这个想法实际上是我们 2004 年在大学时想到的。那么，2004 年有人在用互联网吗？好的，很好。基本上，我不知道千禧一代那时候是否使用互联网。抱歉。好了，不再开年龄玩笑了。好吧，重点是：2004 年的时候，你可能记得互联网上没什么可做的。实际上有点无聊，对吧？那时还没有 Facebook，当然也没有 Snapchat。所以没什么可浏览的，你无法发送 15 秒后消失的消息和照片，因为你甚至没有手机来发送照片。所以 2004 年的互联网上没什么活动。这基本上就是当时互联网的样子：一片贫瘠、荒凉的景象，这就是当时的互联网。需要说明的是，快乐的骆驼代表谷歌，悲伤的骆驼代表雅虎！这就是当时的互联网，准确地说，这就是 2004 年的互联网。雅虎自那以后发展得很好，但在 2000 年代中期，他们确实在摸索方向，而谷歌正在逐渐占领世界，但这就是当时整个世界的范围。我们在 2004 年大学时注意到，出于某种原因，分享文件非常困难。虽然现在这个想法很简单，但回到十年前，要么非常昂贵，要么很难通过不同的企业网络传输数据。当时我有个实习，在实习期间，我大部分使用数据的工作实际上只是复印打印出来的页面，然后把它们放进文件柜。如果你不是计算机专业的学生，这就是实习生的日常工作。所以我非常擅长复印文件。不幸的是，这项技能在今天已经没什么用了。但是，当时分享文件确实很困难，在课堂环境中，你经常要和很多人一起工作，分享文件也很困难。我去了南加州大学。南加州大学给了你 50 兆的在线存储空间。

You could get through a little bit of space in your email account, about 50 megabytes. So you could basically store about one file, and then it would auto-delete every six months. I don't know who was running IT at the time, but they certainly didn't buy hard drives. So it was really, really hard to store and share files. And so we said, "Well, why don't we make it really easy to store and share files from anywhere?" And so we got this idea for what was called box.net at the time.

What we noticed was there were a bunch of factors that had changed in the software world. The first was that the cost of storage was dropping dramatically. In our business, basically every year or two, you can double the amount of data and storage that goes into a hard drive. So what was uneconomical two or three years ago all of a sudden becomes feasible because the cost of computing and the cost of storage has dropped. We had more powerful browsers and networks, so Firefox was just emerging, and people were using much faster internet in both their homes and in the classroom. And then people had more locations that they wanted to store and share information from.

And so we had these three factors that all of a sudden were emerging. I'm going to pull these factors back later when I give some tactical advice, but the first point to remember is always look for these changing technology factors. Because any market that has a significant change in the underlying raw materials or enabling factors is an environment that's about to change in a very significant way. And so we were very fortunate that basically the need for data in the cloud was growing in importance, and the cost and the feasibility of doing it was also improving rapidly.

And so we decided to put together this really quick version of Box. We launched it as box.net, and the idea was to make it really easy to share files. And it turned out that the idea clicked. We got angel funding from this guy named Mark Cuban. This was before Shark Tank, but it was very similar. And so we got this funding.

你的电子邮件账户只有大约 50 兆字节的存储空间。所以你基本上只能存储一个文件，然后每六个月系统就会自动删除。我不知道当时是谁负责 IT 部门，但他们肯定没有购买硬盘。因此，存储和共享文件变得非常非常困难。于是我们说："为什么我们不开发一个真正简单的方法，让人们能够随时随地存储和共享文件呢？"就这样，我们萌生了这个想法，当时这个项目被称为 box.net。

我们注意到软件世界发生了一系列变化。首先是存储成本正在急剧下降。在我们的行业中，基本上每隔一两年，硬盘的数据存储容量就能翻倍。因此，两三年前还不经济的事情突然变得可行，因为计算成本和存储成本已经下降。我们拥有了更强大的浏览器和网络，Firefox 刚刚兴起，人们在家里和教室里都开始使用更快的互联网。而且人们希望在更多地方存储和共享信息。

就这样，这三个因素突然同时出现。稍后我会在给出一些战术建议时再次讨论这些因素，但要记住的第一点是：始终关注这些不断变化的技术因素。因为任何在基础原材料或赋能因素方面发生重大变化的市场，都将迎来重大变革的环境。我们非常幸运的是，云端数据的需求日益重要，而实现这一目标的成本和可行性也在迅速改善。

于是我们决定快速开发 Box 的初始版本。我们以 box.net 的名义推出，核心理念是让文件共享变得非常简单。结果这个想法获得了成功。我们得到了马克·库班的天使投资。这发生在《创智赢家》节目之前，但形式非常相似。就这样，我们获得了这笔资金。

and we decided that, okay. This is gonna be super exciting. We are going to drop out of college. We're gonna move to the Bay Area, and it's gonna be totally awesome.
我们决定，好吧，这会非常刺激。我们要从大学退学。我们要搬到湾区，这将会非常棒。

And when you drop out of college. Anybody drop out of college yet? Okay, good, stay in school. So when you drop out of college, like everyone sort of pictures it, it's just gonna be, you know, incredible, like Bill Gates dropped out of college. It will be like Bill Gates, or Michael Dell dropped out of college and it'll be super exciting like Michael Dell, or Steve Jobs dropped out of college, so this is what people imagine.
当你从大学退学时。有人退学了吗？好吧，很好，继续上学。所以当你从大学退学时，就像大家想象的那样，会非常不可思议，就像比尔·盖茨从大学退学一样。会像比尔·盖茨，或者迈克尔·戴尔从大学退学一样，会像迈克尔·戴尔那样超级刺激，或者史蒂夫·乔布斯从大学退学，这就是人们想象的样子。

But nobody ever remembers that this guy dropped out of college also.
但没人记得这家伙也从大学退学了。

And, so it's not really a guarantee obviously that it's gonna be successful. But, it's what you do, and actually, funny, I don't even know if this guy dropped out of college.
所以，这显然不能保证会成功。但，这是你做的事，实际上，有趣的是，我甚至不知道这家伙是否从大学退学了。

It just sort of looks like he had to. I apologize if anyone's related to him, it's just a funny picture on the internet.
看起来他好像不得不退学。如果有人和他有关系，我道歉，这只是网上的一张搞笑图片。

So basically we decided we would drop out of college. We moved up to first Berkley and then we moved to Palo Alto a little bit thereafter.
所以我们基本上决定从大学退学。我们先搬到伯克利，然后不久后搬到了帕洛阿尔托。

And we decided that we were gonna open up the product for free. We got hundreds of thousands of people that would sign up for the product every single month. So, we had a free version of the product called, if you went to box.net, you got a free one gigabyte of file storage space, which was, again, quite a bit back in 2006 at this point.
我们决定免费开放产品。每个月有数十万人注册使用我们的产品。所以，我们有一个免费版本的产品，如果你访问 box.net，你可以获得 1GB 的免费文件存储空间，这在 2006 年当时是相当多的。

But we were getting so many users and we were trying to figure out what to do. And what we ran into was a common problem that really any startup runs into but was really pronounced by our business model, which was that for consumers, we had built a very robust, a very comprehensive product. And for enterprises, we had actually built too insignificant of a product.
但我们有这么多用户，我们试图弄清楚该怎么做。我们遇到的是一个任何初创公司都会遇到的常见问题，但我们的商业模式使这个问题更加突出，那就是对于消费者，我们构建了一个非常强大、非常全面的产品。而对于企业，我们实际上构建的产品太微不足道了。

For enterprises, we didn't really have enough security, and we didn't have enough capabilities around how enterprises wanted to use their data. So we basically had more functionality than what a consumer wanted and less than what enterprise wanted. We found ourselves at this juncture where it's very difficult to figure out what we wanted to do with the business, so we had to make a choice. We were at a path where we had to choose which path to go down. This was back in early to mid 2006, up to late 2006. I was 23 at the time, the co-founder was 22, and our founding team was even younger when we all dropped out of college. Back in 2006-2007, we imagined these two paths and the worlds were very different. When you do a consumer startup, it's basically just lots of fun with parties all the time and it's super exciting. Then with enterprise, you're basically battling large incumbents in a fairly thankless model because people generally hate enterprise software. That was how we imagined the two paths - we had to choose one of these two worlds. We looked at that and said consumer looks really fun, enterprise looks really hard with a lot of competition. At the same time, in the consumer space, you're always fighting the issue of how to monetize and how to actually get people to pay for products. In the consumer space, there are really only two business models you can do: have people pay for your application or provide advertising on the application. Just to give you a little bit of perspective, these are today's numbers.

对于企业客户，我们实际上没有足够的安全性，也没有足够的能力来满足企业使用数据的需求。因此，我们基本上拥有比消费者所需更多的功能，但又比企业所需的要少。我们发现自己处于这个十字路口，很难确定我们想要如何发展业务，所以我们必须做出选择。我们站在必须选择前进道路的岔路口。这要追溯到 2006 年早中期，一直到 2006 年底。当时我 23 岁，联合创始人 22 岁，我们的创始团队更年轻，当时我们都从大学辍学了。回到 2006-2007 年，我们设想了这两条道路，这两个世界截然不同。当你从事消费者创业时，基本上就是充满乐趣，总是有派对，非常令人兴奋。而企业领域，你基本上是在与大型现有企业竞争，这是一个相当吃力不讨好的模式，因为人们通常讨厌企业软件。这就是我们如何设想这两条道路的方式——我们必须在这两个世界中选择一个。我们审视了这种情况，认为消费者领域看起来真的很有趣，而企业领域看起来非常困难且竞争激烈。同时，在消费者领域，你总是要面对如何盈利以及如何真正让人们为产品付费的问题。在消费者领域，实际上只有两种商业模式可以选择：让人们为你的应用程序付费，或者在应用程序上提供广告。为了让你们有个大致概念，这些是现在的数字。

Just to give you a little bit of a sense of the scale difference: in the consumer world, there's about $35 billion spent on mobile apps every year. That's a pretty big number, right? $35 billion is a lot of money being spent on mobile apps today. For advertising, the global digital advertising market is $135 billion. So basically, combined, most consumer companies are going after about $170 billion of either purchasing power on applications or global advertising around these kinds of services. So big numbers, lots of opportunity there. However, in enterprise, there's actually $3.7 trillion spent on enterprise IT every single year. These are the servers, the infrastructure, the software, the networking, the services. All of that stack of technology equates to a few trillion dollars spent every single year.

为了让您对规模差异有个大致了解：在消费领域，每年在移动应用上的花费约为 350 亿美元。这是个相当大的数字，对吧？如今 350 亿美元花在移动应用上是很大一笔钱。在广告方面，全球数字广告市场是 1350 亿美元。所以基本上，大多数消费类公司追求的总规模约为 1700 亿美元，这包括应用程序上的购买力或围绕这类服务的全球广告。所以数字很大，机会很多。然而，在企业领域，每年企业 IT 支出实际上达到 3.7 万亿美元。这些包括服务器、基础设施、软件、网络和服务。所有这些技术堆栈加起来相当于每年数万亿美元的支出。

What we realized was that there's a pretty wide delta between these two markets. We are gonna be fighting over trying to get consumers to pay a few dollars a month, and Google, Microsoft, and Apple will probably try to make this product free over time. There were rumors that Google Drive was coming out, and these kinds of products that eventually happened were going to come out. But in the enterprise, it's not so much about trying to always save money on IT. They're actually trying to increase productivity and get higher performance from their businesses. So the value equation is very different, right? In consumer, we have a limited amount of money that we want to conserve for as few things as possible that we're gonna spend. In the enterprise, it's a little bit of a different shift, where you're trying to think about what you can get out of technology and how much value that is for you. So that was a really important data point.

我们意识到这两个市场之间存在相当大的差距。我们将努力争取让消费者每月支付几美元，而谷歌、微软和苹果可能会试图让这类产品最终免费。当时有传言称谷歌云端硬盘即将推出，这些最终确实出现的产品即将面世。但在企业领域，重点并不总是试图节省 IT 开支。他们实际上是在努力提高生产力，并从业务中获得更高的绩效。因此，价值等式非常不同，对吧？在消费领域，我们拥有的资金有限，希望尽可能少地花钱。在企业领域，情况有所不同，你需要思考能从技术中获得什么，以及这对你有多大价值。所以这是一个非常重要的数据点。

It wasn't something that really you should have shot out of bed in the morning and said, okay, I'm super excited to build an enterprise software company. And the reason for that was actually pretty straightforward at the time. The way that you built software was very slow. So you had to be very slow because you couldn't break anything for customers. The sales process was very slow because customers take a long time to purchase technology. So I think everyone's sort of used to this concept that usually when you try and sell enterprise software to a company, it can take up to a couple of years for them to actually just buy the software. And then they can take a couple more years for them to even implement the technology in the first place, so a lot of companies are around for a few years without having their technology even used in the enterprise.

这并不是那种会让你从床上一跃而起，兴奋地说"太好了，我超级期待建立一家企业软件公司"的事情。当时的原因其实相当简单明了。软件开发的过程非常缓慢。所以你不得不放慢节奏，因为你不能给客户带来任何问题。销售过程也很缓慢，因为客户购买技术需要很长时间。所以我认为大家都有点习惯了这个概念：通常当你试图向一家公司销售企业软件时，他们光是购买软件就可能需要几年的时间。然后他们还需要再花几年时间才能真正实施这项技术，因此很多公司存在了好几年，他们的技术甚至都没有在企业中得到使用。

That felt like a huge problem. And not like something we wanted to be a part of. The technology itself is fairly complex. So the user experience, I don't know how many people have had to use most enterprise software, but it's generally really complicated. You find yourself asking why in God's name did the designer try to put 47 buttons on the page. And you just can't even understand it, and the reason is something we'll get into in a second. But basically, there's just no love or care for the design and user experience, the software's just really complex.

这感觉像是一个巨大的问题。而且不是我们想要参与的事情。技术本身相当复杂。至于用户体验，我不知道有多少人不得不使用大多数企业软件，但通常都非常复杂。你会发现自己不禁要问，设计师到底为什么要在一页上放 47 个按钮。你甚至无法理解它，原因我们稍后会谈到。但基本上，就是缺乏对设计和用户体验的关爱与重视，软件只是非常复杂。

And then, finally, if that wasn't bad enough, all of a sudden you had to think about how do actually sell the software. And, for anybody who loves the power of the internet, this notion of having a sorta sales intermediary, to get to your customer, seemed like, really unappealing. Like, you're gonna have to go hire a bunch of people that are gonna be everywhere in the country. And they're gonna be the only interface you have to your customer. You're gonna have to hire these guys named Chuck. And Chuck's gonna roll in with a briefcase, and he's gonna just try and sell lots of enterprise software to the customer. Just so we're clear, this is what Chuck looks like.

然后，最后，如果这还不够糟糕的话，突然间你还得考虑如何实际销售软件。而且，对于任何热爱互联网力量的人来说，这种需要通过某种销售中介来接触客户的概念，看起来真的没有吸引力。就像，你必须去雇佣一群遍布全国的人。他们将成为你与客户之间的唯一接口。你必须雇佣这些叫 Chuck 的人。Chuck 会带着公文包出现，他只是试图向客户销售大量企业软件。为了说清楚，这就是 Chuck 的样子。

And that was the sales process that you, in

The enterprise that we at least imagined in our heads, and I mean, Chuck looks like a happy guy, but he's still an intermediary to getting your software. And we were saying, well, why can't we take the power of the Internet and actually get our technology out to people directly? Why should we have to go through this sales intermediary as we scale the business? Now, I'll get into in a minute why we were wrong about the sales process, but this was the sort of fear that we had at the time.

我们至少在大脑中想象过的企业，我的意思是，查克看起来是个快乐的人，但他仍然是获取软件的中介。我们当时在想，为什么我们不能利用互联网的力量，真正将我们的技术直接带给人们？为什么在扩展业务时必须通过这个销售中介？稍后我会详细说明为什么我们对销售流程的看法是错误的，但这就是我们当时的担忧。

And then, if that wasn't hard enough, we had investors in 2007 saying basically there's no way you're gonna make it in the enterprise. You basically are again a founding team of early 20 year olds. You don't have anybody on your team that has been in the enterprise. The Microsoft and Google and all these things are, or not Google, but Microsoft, EMC, Oracle, IBM. These kinds of companies are gonna stomp on you and it's gonna be very, very hard to actually achieve.

然后，如果这还不够困难的话，2007 年我们有投资者基本上说你们绝不可能在企业市场成功。你们基本上又是一个由 20 岁出头的年轻人组成的创始团队。你们的团队中没有任何人有过企业经验。微软、谷歌以及所有这些公司，或者不是谷歌，而是微软、EMC、甲骨文、IBM。这类公司会碾压你们，实际上会非常非常难以实现目标。

And to be fair they were right on a number of areas, right? So we were a very inexperienced team. This was a stage where it is still early in our careers. My co-founder, for instance, looked like he was 13 years old. So just not really, just to be clear, this is what he looked like. So it sorta made sense, right? This is him as our CFO at, I think this is him at 29. But it looked like we were gonna go run off with the money and go to Disneyland.

公平地说，他们在很多方面都是对的，对吧？我们是一个非常缺乏经验的团队。这是我们职业生涯的早期阶段。例如，我的联合创始人看起来像 13 岁。所以真的不太像，说清楚点，这就是他当时的样子。所以这某种程度上说得通，对吧？这是他在担任我们首席财务官时的样子，我想这是他 29 岁的时候。但看起来我们好像会拿着钱跑去迪士尼乐园。

So I can totally appreciate why they didn't think we would be able to pull it off. I can't imagine giving him money. So basically, we decided okay well we still have to go do it, we have to just give it our best shot. We're gonna take the sorta scale, the consumer experience, the DNA of our company. We're gonna see if we can bring this into the enterprise and then we were very fortunate where actually we had an investor who was equally early in his career and made a bet on us.

所以我完全理解为什么他们认为我们无法成功。我无法想象会给他钱。所以基本上，我们决定好吧，我们仍然必须去做，我们必须尽力而为。我们将采用某种规模、消费者体验和我们公司的 DNA。我们将看看是否能把这一点带入企业市场，然后我们非常幸运，实际上我们有一位同样处于职业生涯早期的投资者，他在我们身上下了赌注。

Because we believed that something was fundamentally changing about the enterprise that we would be able to take advantage of, we decided that if we were going to pursue the enterprise market, we would have to play by a very different set of rules.

因为我们相信企业市场正在发生根本性的变化，而我们将能够利用这一变化，所以我们决定如果要进军企业市场，就必须遵循一套截然不同的规则。

What about the complexity of software can change in this new era? What about the fact that the sales process is very slow can change in this new era? How do we move and go directly to the user and to the customer, as opposed to having this very indirect process of getting our technology out there?

在这个新时代，软件的复杂性会发生什么改变？销售流程极其缓慢这一事实在新时期会如何改变？我们如何直接触达用户和客户，而不是通过这种非常间接的方式来推广我们的技术？

How do we build and design for that user as opposed to just for the sale and for the RFP process that a customer is going to go through? We looked at all of the factors that were true of the enterprise and said we're going to find what is changed about the technology world, where we can take advantage of that shift and build a better and newer enterprise software company.

我们如何为用户进行设计和构建，而不是仅仅为了销售和客户将要经历的招标流程？我们审视了企业市场的所有特征，决定要找出技术世界发生的变化，利用这种转变来打造一个更好、更新的企业软件公司。

That was the decision that we embarked on. This is the path that we embarked on about eight years ago, and that is why we have been focusing on the enterprise. Today, we have about 240,000 businesses that use the product.

这就是我们当初作出的决定。这是大约八年前我们开启的道路，这也是为什么我们一直专注于企业市场。如今，约有 24 万家企业使用我们的产品。

The reason is we architected the business model, we architected the software, we architected the solution to work in a specific version of the world. It turned out that version ended up being the one that happened.

原因在于我们构建了商业模式，设计了软件架构，打造了适用于特定世界形态的解决方案。事实证明，这种世界形态最终成为了现实。

I'll go into a little bit about what has changed about the world that we sort of built our company around. If you're building an enterprise software company, I would highly recommend you make sure to orient your technology and solution around better and more seamlessly than ever before. Everything about the enterprise, and by definition the software that an enterprise uses, has changed just in the past five years. This is probably the most magical time. If there could have ever been a magical time to build an enterprise software company, now is absolutely that time in terms of how much change is going on within organizations. Let's go through a couple of these things. The first is that most application categories are moving to the cloud. The big difference is this idea that if you were going to start a customer relationship management software company, or a business intelligence software company, or even a content management company ten to fifteen years ago, you basically had to have your technology implemented in every single customer location. No matter how many customers you sold to, no matter what regions they're in, every single customer had to put that in their data center. That was the flaw with on-premise computing - you were repeating all of this work, creating so much redundancy, and it was slowing down the entire process of delivering and building software for the enterprise. Then all of a sudden the cloud came around. Things like Salesforce.com and Amazon Web Services basically said: why is it that every customer that wants to just implement a couple servers has to go procure the servers, put them in their data center, put all the security and networking around those servers, and then six months later they go live and then a developer can use them in the organization? The same thing with an application. Amazon said why does that make any sense today when we could just put together thousands, tens of thousands, hundreds of thousands of servers and make them available on demand? You just use what you want when you need that. That was obviously the definition of cloud computing.

我将简要介绍我们所围绕建立公司的世界发生了哪些变化。如果你正在建立一家企业软件公司，我强烈建议你确保围绕比以往更好、更无缝的技术和解决方案来定位。关于企业的一切，以及根据定义企业使用的软件，在过去五年中已经发生了变化。这可能是最神奇的时刻。如果曾经有过建立企业软件公司的神奇时刻，那么现在绝对是那个时刻，就组织内部正在发生的变革程度而言。让我们来看几个方面。首先是大多数应用类别正在向云端迁移。最大的区别在于，如果你在十到十五年前要创办一家客户关系管理软件公司、商业智能软件公司，甚至是内容管理公司，你基本上必须在每个客户地点实施你的技术。无论你向多少客户销售，无论他们在哪个地区，每个客户都必须将其部署在自己的数据中心。这就是本地计算的缺陷——你在重复所有这些工作，制造了大量冗余，并且拖慢了为企业交付和构建软件的整个过程。然后突然间云计算出现了。像 Salesforce.com 和亚马逊网络服务这样的公司基本上提出：为什么每个只想部署几台服务器的客户都必须去采购服务器，将它们放入数据中心，为这些服务器配置所有安全和网络，然后六个月后才能上线，开发人员才能在组织中使用它们？应用程序也是如此。亚马逊提出：当我们能够将数千、数万、数十万台服务器整合在一起并按需提供时，为什么今天还要这样做？你只需要在需要时使用你想要的资源。这显然是云计算的定义。

What is happening, though, is finally CIOs in large enterprises are taking advantage of this. So it seems obvious for everyone in this room because you would never build a company by buying your own servers. You would start it on Amazon, or Google or Azure or whatever. But to an enterprise, there are decades, literally decades of investment in infrastructure, that now has to move to the cloud. And so that's obviously a massive shift that is finally happening. We are moving to a world of cheaper, low cost, on demand computing, from a world of expensive computing.

然而，正在发生的是，大型企业的首席信息官们终于开始利用这一点。对在座的各位来说这似乎显而易见，因为你们绝不会通过购买自己的服务器来创建公司。你们会在亚马逊、谷歌、Azure 或其他云平台上起步。但对一家企业而言，他们在基础设施上投入了数十年，真的是数十年的投资，现在必须迁移到云端。这显然是一个巨大的转变，终于正在发生。我们正从一个昂贵的计算世界，转向一个更廉价、低成本、按需计算的世界。

The benefit to building a startup is that customers don't have the same kind of friction when they're going to adopt new technology. As soon as the computing is so much cheaper, it becomes easier to adopt new solutions. Which means that their barrier for having a conversation, their barrier for introducing you into their enterprise, is a lot lower, which creates a massive opportunity for startups.

创建初创公司的好处在于，客户在采用新技术时不会遇到同样的阻力。一旦计算成本大幅降低，采用新解决方案就变得更容易。这意味着他们进行对话的门槛，将你引入他们企业的门槛，都大大降低了，这为初创公司创造了巨大的机会。

We're going from a world of customized software to really standardized platforms. So it used to be that you had to build custom integrations and all of the custom experiences on top of the software itself. And now, customers are realizing that actually they want open platforms and they can customize at the layer above the product.

我们正从定制化软件的世界转向真正标准化的平台。过去，你必须在软件本身之上构建定制集成和所有定制体验。而现在，客户意识到他们实际上需要开放平台，并且可以在产品之上的层级进行定制。

It used to be that when you were building enterprise software company you could only really sell to the top five or 10,000 companies in the world. Because only those companies had the wherewithal, the talent, the infrastructure and the budget to deploy your technology into the enterprise. Today, literally a two person company can sign up for Box, as well as we work with General Electric which has over 300,000 employees. So the fact that you can now serve a small business anywhere in the world as well as some of the largest companies on the planet means that there's much larger markets that you can go after, which makes it obviously an even better economic proposition.

过去，当你建立企业软件公司时，你实际上只能向全球前 5000 或 10000 家公司销售。因为只有这些公司拥有将你的技术部署到企业所需的资源、人才、基础设施和预算。如今，一个只有两人的公司可以注册使用 Box，同时我们也与拥有超过 30 万员工的通用电气合作。因此，你现在可以为世界各地的中小企业以及全球一些最大的公司服务，这意味着你可以追求更大的市场，这显然使其成为一个更好的经济主张。

The platforms themselves are becoming more global. So our customers were international literally within the first couple of weeks of starting the company. It would have been, usually if you had done enterprise software the traditional way that would have taken years to actually be able to go international.

平台本身正变得更加全球化。因此我们的客户实际上在公司成立的最初几周内就实现了国际化。如果按照传统方式开发企业软件，通常需要数年时间才能真正实现国际化。

And then finally and probably the most profound shift of all is that because of mobile devices, you know, iPhone's, iPad's, tablets, Android devices, the IT model of the enterprise has become a lot more user led. And that's fundamentally important because in an IT led world, incumbents generally win. Because they have the existing relationship with the IT organization, with the CIO, with the spending power within that company.

最后，可能也是最重要的转变是，由于移动设备，如 iPhone、iPad、平板电脑、安卓设备等，企业的 IT 模式已变得更加以用户为主导。这一点至关重要，因为在 IT 主导的世界里，现有企业通常占据优势。因为他们与 IT 部门、首席信息官以及公司内部的采购决策权都保持着现有关系。

In a user led model, users are bringing in their own technology. They're bringing it in on the sales team, they're bringing it in in the marketing team, they're bringing it in in finance. And you can build software then around the user. Which means that they can bring the technology in. Then you can sell to the enterprise when they wanna have better control, better security, better scalability. So you still have the same business model as a traditional enterprise software company. But the way to get into the company is now through the end user.

在用户主导的模式下，用户会引入自己的技术。销售团队在引入技术，市场团队在引入技术，财务部门也在引入技术。然后你可以围绕用户来开发软件。这意味着他们可以引入技术。当企业希望获得更好的控制、更高的安全性和更好的可扩展性时，你就可以向企业销售产品。因此你仍然保持着与传统企业软件公司相同的商业模式。但进入公司的方式现在是通过最终用户。

Okay, so those are qualitative factor changes. Just a couple quantitative factor changes. There's over nearly 2 billion smart phones on the planet. That changes every single IT model on the planet, because it used to be, ten years ago, if you were managing technology for an enterprise, you just had to manage the computers and the network that were inside of your building. But now with billions of mobile phones, you fundamentally have to be able to manage the network and the computing that is gonna be anywhere at anytime on a network. And that creates massive opportunity for software companies, because no incumbent has built the technology stack.

好的，这些是定性因素的改变。还有一些定量因素的改变。全球有近 20 亿部智能手机。这改变了全球每一个 IT 模式，因为在十年前，如果你为企业管理技术，你只需要管理办公楼内的计算机和网络。但现在有了数十亿部手机，你从根本上必须能够管理网络和计算，这些计算可能在任何时间、任何地点的网络上进行。这为软件公司创造了巨大的机会，因为没有现有企业已经建立了这样的技术堆栈。

No incumbent has built a technology stack that powers this next generation of work and how enterprises are using their data. So that creates a massive startup opportunity. There's nearly 3 billion people online. What that means is that every single enterprise is equally changing how they're going to get their own products to their customers, which means that every industry changes.

现有企业尚未构建能够支持新一代工作方式和企业数据应用的技术架构。这创造了巨大的创业机会。目前有近 30 亿人在线。这意味着每家企业都在同等程度地改变着将产品送达客户的方式，进而导致每个行业都在发生变革。

So no incumbent has built a technology for multi-channel commerce. You're gonna shop on your phone, you're gonna shop in a store, and you want things to be delivered to you as well. Most of the incumbent technology in the retail industry doesn't power multi-channel commerce. Nobody is prepared for what it means when consumers want to actually buy goods at anytime from anywhere with better information and better intelligence. Every retailer in the world is gonna need a new technology stack to power their retail experiences.

In the health care space, every single health care institution is trying to find ways of building more personalized experiences and more predictive experiences. They want medicine to be adapted to the individual. As the business model of health care changes from charging for the surgery and charging for the checkup to where the customer pays for wellness and staying healthy, then all of a sudden every healthcare institution needs better technology to deliver health care experiences. They're gonna want to deliver telemedicine and healthcare in more regional locations as opposed to just in the monolithic hospital environment. There are going to be new use cases around how electronic health records get connected to one another so doctors can make far better decisions. All of these things are gonna require new enterprise software to power these businesses and these industries.

In the media space as an example, you have an industry that is going from really linear programming. Whether that's television, music, or movies, there's a very linear supply chain oriented business model where a film gets made, goes to the movie theater for three months, and then afterwards goes to iTunes and other platforms. Now we're moving to a world where people want experiences on demand. That's gonna change how distribution works on the scale of 3 billion people that are on the internet. Again, no media company has a platform that is gonna be able to meet these new demands.

因此，现有企业尚未构建支持全渠道商务的技术。消费者将通过手机购物，在实体店购物，同时希望商品能够配送到家。零售业现有的大多数技术并不支持全渠道商务。当消费者希望随时随地凭借更优质的信息和智能购买商品时，没有任何企业为此做好准备。全球每家零售商都需要新的技术架构来支撑其零售体验。

在医疗保健领域，每家医疗机构都在寻求构建更个性化、更具预测性体验的方法。他们希望实现个性化医疗。随着医疗保健商业模式从按手术和检查收费，转变为客户为健康管理和保持健康付费，突然间每家医疗机构都需要更好的技术来提供医疗保健服务。他们将希望提供远程医疗，并在更多区域性场所提供医疗服务，而不仅仅局限于单一的大型医院环境。将会出现新的应用场景，例如电子健康记录如何相互连接以便医生做出更准确的诊断。所有这些都需要新的企业软件来支撑这些业务和行业。

以媒体行业为例，这个行业正在从线性节目编排模式转型。无论是电视、音乐还是电影，过去都存在非常线性的供应链导向商业模式：电影制作完成后，先在影院上映三个月，然后才登陆 iTunes 和其他平台。现在我们正转向一个用户需要按需体验的世界。这将改变面向 30 亿互联网用户的发行方式。同样地，目前没有媒体公司拥有能够满足这些新需求的平台。

To go after the enterprise, actually power how content and data and information moves through the system at scale. We were just, I was just in LA yesterday meeting with a media company that has basically done predictive analytics to find their potential movie goers in the population of 3 billion internet users. They wanna be hyper targeted on how they get to the very, the sorta specific 30 million people that are fans of different kinds of film types. And so all of a sudden, you have a movie company that actually needs big data, and they need business intelligence and they need marketing automation to go power how they're gonna go market and distribute their movie. So again, totally unpredictable mash-up of two industries coming together, where all new kinds of software is gonna be necessary. So, every industry is going through some form of this change. You can pick any industry you want, and take and sort of zoom into it and say, what are the underlying technology factors that are gonna change the business model of this industry in the next couple of years? And then there's gonna need to be software that goes and powers those experiences.

面向企业市场，实际上是要规模化地驱动内容、数据和信息在系统中的流动。我们昨天在洛杉矶与一家媒体公司会面，该公司基本上已经完成了预测分析，旨在从 30 亿互联网用户中找出潜在的观影人群。他们希望高度精准地定位到大约 3000 万喜欢不同类型电影的特定粉丝群体。突然间，一家电影公司实际上需要大数据，他们需要商业智能，需要营销自动化来驱动他们的电影营销和发行方式。这再次体现了两个行业完全不可预测的融合，将需要各种新型软件。因此，每个行业都在经历某种形式的这种变革。你可以选择任何行业，深入观察并思考：在未来几年内，哪些底层技术因素将改变这个行业的商业模式？然后就需要有相应的软件来驱动这些体验。

If they don't get good at technology, if they don't have a competency in leveraging data and using these new tools, they're gonna do that through working with what we consider the technology industry, as opposed to everybody building up these pockets of expertise themselves. So there's gonna be a lot of partnership over the next five to ten years, where companies are gonna need to use technology to work smarter and faster. They're gonna need to do this more securely, and this is gonna change not only how individuals work in these environments, but ultimately change the business models of these companies.

如果他们不擅长技术，如果他们在利用数据和使用这些新工具方面缺乏能力，他们将通过与我们所认为的技术行业合作来实现这一目标，而不是每个人都自己建立这些专业知识领域。因此在未来五到十年内将会出现大量合作，公司需要利用技术来更智能、更快速地工作。他们需要更安全地做到这一点，这不仅会改变个人在这些环境中的工作方式，最终还将改变这些公司的商业模式。

So that was chapter two of this. That's all the things that have been changed. Now some practical advice to help you get started. To be fair, most of this advice is looking through the lens of retrospect, which means that this is not always how things happen. But I can look back in time and say these are things that led certain things to be true. So it's hard to be super deterministic about building a company, and you might not have all of these things totally figured out. But these will give you a sense of some of the patterns to recognize and opportunities to exploit as you're building, or thinking about building, an enterprise software company.

以上就是第二章的内容。这些都是已经发生变化的事情。现在是一些实用的建议，帮助你入门。公平地说，这些建议大多是通过回顾的视角来看待的，这意味着事情并不总是这样发生的。但我可以回顾过去并说，这些是导致某些事情成真的因素。因此，在创办公司方面很难做到完全确定，你可能无法完全弄清楚所有这些事情。但这些会让你在建立或考虑建立企业软件公司时，对需要识别的模式和可以利用的机会有所了解。

The first is, spot technology disruptions. This is gonna be true whether you're building consumer or enterprise. The rest are more enterprise oriented, but this is just fundamental to entrepreneurship if you're gonna build a tech company. So you have to look for new enabling technologies or major trends, like fundamental trends, that create a wide gap between how things have been done, and how they can be done. Looking back in time at our business, the gap was basically storage getting cheaper, internet getting faster, browsers getting better. Yet we're still sharing files through very complicated, very esoteric, very slow and cumbersome means. So any time where the delta between what is possible and how things work today is at its widest, that is an opportunity to go.

首先是发现技术颠覆。无论你是在构建消费者产品还是企业产品，这一点都是适用的。其余的建议更偏向企业导向，但如果你要建立一家科技公司，这是创业的基础。因此，你必须寻找新的赋能技术或主要趋势，比如根本性趋势，这些趋势会在过去的工作方式与可能的工作方式之间创造巨大差距。回顾我们的业务，差距基本上是存储变得更便宜，互联网变得更快，浏览器变得更好。然而我们仍然通过非常复杂、非常深奥、非常缓慢和繁琐的方式共享文件。因此，当可能实现的方式与当前工作方式之间的差距达到最大时，这就是一个机会。

That is an opportunity to go build new technology to solve a problem. As you're looking at the enterprise, the question is, what about the cost of computing dropping so rapidly changes what enterprises can do with their data? What does it change about what you can do from a business model standpoint? What was impossible, because of either economic feasibility or technical feasibility 10 or 15 years ago, that now all of a sudden is possible. A fun thing you can do sometime is, if you go look at articles from like the 1990's or even the 1980's, business articles about technology. All we're really doing is repeating a lot of the technologies that were tried 10, 20, 30 years ago. It was too expensive, it was too unusable, and we didn't have the enabling technologies to make it possible. So you can kind of see this pattern emerge constantly where something that was impossible five or 10 years ago, all of a sudden becomes very practical.

这是一个构建新技术来解决问题的机会。当你审视企业时，问题在于：计算成本急剧下降如何改变企业处理数据的方式？从商业模式的角度来看，这改变了什么？10 或 15 年前因经济可行性或技术可行性而无法实现的事情，现在突然变得可能了。有时你可以做一件有趣的事情，就是去查阅 20 世纪 90 年代甚至 80 年代关于技术的商业文章。我们实际上只是在重复很多 10 年、20 年、30 年前尝试过的技术。只是当时太昂贵、太不实用，我们缺乏使其成为可能的关键技术。因此你可以看到这种模式不断出现：五到十年前不可能的事情，突然变得非常实用。

I'll give you an example. There's a company called PlanGrid. Does anybody know what PlanGrid does? Are you in the construction industry? What does that even mean? In terms of PlanGrid or like in construction? I work at a job site. We build buildings. So basically PlanGrid is a mobile application that lets you manage construction projects. It lets you get access to your blueprints, lets you manage all of the data around a construction process. This team realized that $4 billion, I believe, are spent every year printing out blueprints and making all of the prints, and updates to those blueprints any time there's a change. Then that has to ripple and cascade through a very wide network of contractors and construction people every time there's even just one slight, minute change. They realized that with the iPad, all of a sudden, we have the perfect form vector to be

我给你们举个例子。有一家叫 PlanGrid 的公司。有人知道 PlanGrid 是做什么的吗？你在建筑行业工作吗？这到底是什么意思？是指 PlanGrid 还是建筑行业？我在工地工作。我们建造建筑物。所以基本上 PlanGrid 是一个移动应用程序，可以让你管理建筑项目。它让你能够访问蓝图，管理建筑过程中的所有数据。这个团队意识到，我相信每年要花费 40 亿美元打印蓝图、制作所有图纸，并在每次有变更时更新这些蓝图。然后每次即使只有一个微小的改动，这种影响都必须在一个非常广泛的承包商和建筑人员网络中传播和扩散。他们意识到，有了 iPad，我们突然拥有了完美的形式载体来

This is something that could ripple through the construction industry, which isn't necessarily notoriously known for high technology, except for on the design side. How could they build technology that would make the data access and data collaboration problem really seamless and easy to do in a very traditional industry that hasn't quite changed for a while? So it was a sort of perfect discovery of a technology change in a market and figuring out how those two things converge. This team built a great startup out of it that is doing incredibly well and totally taking over the construction industry as proven by this individual.

这可能会在整个建筑行业产生连锁反应，该行业除了设计方面外，并不以高科技著称。他们如何能够在一个相当长一段时间没有发生太大变化的传统行业中，构建出使数据访问和数据协作问题真正实现无缝衔接且易于操作的技术？这堪称是在市场中发现了技术变革的完美契机，并找到了这两者如何融合的方法。这个团队由此创建了一家优秀的初创公司，正如这位个人所证明的那样，该公司表现非常出色，并正在完全占领建筑行业。

You're going to be able to expand over time, either to larger customers or to more use cases. A great example is ZenPayroll, started by Stanford graduates a couple of years ago. They discovered that the payroll process in small businesses is incredibly complicated, annoying, and cumbersome because we use the same vendors we've been using for decades that weren't digitally oriented. You didn't get your payments as receipts over email, it was very complicated. You didn't get to see graphs of your salaries, and there was no real good data around this. They decided to take the slice that is most painful to startups around hiring people and paying people, which is the payroll management process. They're going to plug into existing infrastructure but make it dead simple to use. Now they're able to move up market over time as well as deliver new services. The incumbents in this market initially look at something like payroll and say that it's small, only for small businesses, and not very powerful. But that's just the start because as they get that wedge and fit into the market, they're going to be able to expand over time, build out more services and capabilities. They found the exact right opening to build a new company and have that emerge.

随着时间的推移，您将能够扩展业务，既可以面向更大的客户，也可以应对更多使用场景。一个很好的例子是 ZenPayroll，它由斯坦福大学的毕业生在几年前创立。他们发现小型企业的工资单处理过程极其复杂、烦琐且笨重，因为我们使用的供应商几十年来一直未实现数字化。您无法通过电子邮件收到付款收据，整个过程非常复杂。您无法查看工资图表，也没有相关的优质数据。他们决定解决初创公司在招聘和支付员工薪酬方面最痛苦的部分，即工资管理流程。他们将接入现有基础设施，但使其变得极其简单易用。现在他们能够随着时间的推移进入高端市场，并提供新的服务。这个市场的现有企业最初看待工资单这样的服务时，认为它规模小，只适用于小型企业，并且功能不强。但这只是开始，因为随着他们获得立足点并适应市场，他们将能够随着时间的推移进行扩展，建立更多服务和能力。他们找到了建立新公司并使其崛起的绝佳切入点。

To be able to load up blueprints and load up content, but that over time... I'll give you two examples. So, if you're going to build software today for the enterprise that goes after an incumbent category that has more of a suite-oriented approach, then what you want to do is you want to build technology that's going to be platform agnostic. Because what suite players will do is they want everything to be integrated with itself, and that there's more value with that vertical integration. But you want to go after a different axis, which is you want your technology to work across all of the platforms. So that way, you can work with so many different kinds of customers, and you can be an ally to so many different kinds of platforms, which a traditional incumbent is not able to do. So that's something that is usually technically infeasible because of how either something has been architected, or because the fundamental component of the business model is to not do that as the incumbent.

But the other thing is try and do things that are economically infeasible. You can look at the cost structure of an incumbent company and discover where are they not going to be able to drop their prices, because that business model is fundamental to the existence of the company. Or where can you find ways of monetizing the customer that are unusual or unique, that no one has discovered before, and thus is really not practical for anyone else to do. That's a company called Zenefits, where they have an HR management software company that helps you, as a small startup, manage all of your benefits, all of your HR information. And instead of charging the startup, which might not value that software at the stage that they're at, they realized that they could get commission from the insurance companies. And that basically pays for the ability to use their software. So, the customer itself is not paying for Zenefits, the Zenefits platform is being paid for by the insurance company. And they've thus invented a business model that no other software company has been able to think of or attack.

能够加载蓝图和加载内容，但随着时间的推移...我将给你两个例子。如果你今天要为企业构建软件，进入一个由现有企业主导、更倾向于套件化方法的领域，那么你需要构建平台无关的技术。因为套件厂商的做法是希望所有东西都能与自身集成，这种垂直整合能带来更多价值。但你要追求不同的方向，即让你的技术能够在所有平台上运行。这样，你就能与各种不同类型的客户合作，成为各种不同类型平台的盟友，而传统现有企业无法做到这一点。这通常在技术上不可行，要么是因为系统架构的设计方式，要么是因为作为现有企业，其商业模式的基本组成部分决定了不能这样做。

但另一方面是尝试做经济上不可行的事情。你可以研究现有公司的成本结构，找出他们无法降低价格的领域，因为这种商业模式对公司生存至关重要。或者你可以找到不同寻常或独特的客户变现方式，这些方式是前所未有的，因此其他人实际上无法实施。这就是 Zenefits 公司，他们开发了一款人力资源管理软件，帮助小型初创企业管理所有福利和人力资源信息。他们不是向初创公司收费（初创公司可能在其发展阶段不重视该软件的价值），而是意识到可以从保险公司获得佣金。这基本上支付了使用他们软件的费用。因此，客户本身不为 Zenefits 付费，Zenefits 平台由保险公司支付费用。他们就这样发明了一种其他软件公司从未想到或实施的商业模式。

And they're equally going and disrupting a category that hasn't seen a lot of innovation previously, which is the health and benefit space within small businesses.

The next is you wanna find the really crazy but still somewhat reasonable outliers within the customer ecosystem. So you need to find the customers that are sort of at the edge of their business, or at their business model, or of their industry. And find the unique characteristics about those customers, and leverage them as your early adopters.

So Paul Graham has a great article, where he talks about living in the future and building for what is missing when you're living in the future. So that's a sort of easy way to spot trends and patterns about disruption that's playing out. The same is true in the work place. If you find customers that are working in the future, you will be able to work with them to find what is missing when you're working in the future. And how we build technology that supports all these new use cases that are going to emerge.

There's a company called Skycatch, which does enterprise drones. And at first it seems kind of weird, but in the construction space, in farming, in oil and gas, they're using drones now for data capture, and the ability to do modeling of different environments. And so, this company is able to find all of the companies that are at the bleeding edge of their industry, what is unique or new about how those organizations operate. And they built and worked with a lot of those early adopters to establish their platform, which is again, really one of the first enterprise drone companies.

他们同样在颠覆一个此前创新不多的领域，即小企业的健康和福利领域。

接下来你要找到客户生态系统中那些真正疯狂但仍然相对合理的异常值。所以你需要找到那些处于其业务边缘、商业模式边缘或行业边缘的客户。找出这些客户的独特特征，并将他们作为早期采用者。

Paul Graham 有一篇很好的文章，他谈到了生活在未来，并为你在未来生活时所缺失的东西而构建产品。这是发现正在发生的颠覆性趋势和模式的一种简单方法。在工作场所也是如此。如果你找到正在未来工作的客户，你将能够与他们合作，找出在未来工作时缺失的东西，以及我们如何构建支持所有这些即将出现的新用例的技术。

有一家名为 Skycatch 的公司，专门从事企业无人机业务。起初这似乎有点奇怪，但在建筑领域、农业领域、石油天然气领域，他们现在使用无人机进行数据捕获，以及对不同环境进行建模。因此，这家公司能够找到所有处于行业前沿的公司，了解这些组织运营方式的独特或新颖之处。他们与许多早期采用者合作并共同建立了他们的平台，这确实是首批企业无人机公司之一。

So the idea is go look at your market. Find the customers that are at the bleeding edge of the market, who use technology to get ahead, and that use technology for performance advantages. And then go work with them to discover how your product can evolve.

所以核心思想是去审视你的市场。找到那些处于市场前沿的客户，他们使用技术来获得优势，使用技术来获得性能优势。然后与他们合作，发现你的产品如何演进。

Listen to your customers, but don't always build exactly what they're telling you. This is a really key distinction around building enterprise software. Your customers are gonna have a large number of requests. Your job is to distill those requests down into the ultimate product. That does not mean that you're going to build exactly what they tell you to build. It is your job to listen to their problems, but actually translate those into what is going to build the best and simplest solution to those problems. It's really your job, and Palantir does this as an example, with very, very complex issues. And then they distill those down into simple solutions for very, very complex problems that the customer otherwise would not have known how to ask for.

倾听客户的意见，但不要总是完全按照他们所说的去构建产品。这是构建企业软件时的一个关键区别。客户会有大量的需求。你的工作是将这些需求提炼成最终产品。这并不意味着你要完全按照他们告诉你的去构建产品。你的工作是倾听他们的问题，但实际要将这些问题转化为能够构建出最佳且最简单解决方案的方案。这确实是你的职责，以 Palantir 为例，他们处理非常复杂的问题，然后将这些问题提炼成简单解决方案，解决那些客户原本不知道如何提出的非常复杂的问题。

You want to modularize, not customize. So build a platform, as opposed to building all of the custom technology and custom vertical experiences directly in the software itself. Make sure that you really think about openness in APIs as a way of delivering vertical or custom experiences. Don't build that directly into the product.

你需要模块化，而不是定制化。因此要构建一个平台，而不是直接在软件本身中构建所有定制技术和定制垂直体验。确保你真正考虑将 API 的开放性作为提供垂直或定制体验的方式。不要将这些直接构建到产品中。

Focus on the user always. So, again, the magical thing about building an enterprise software company right now is you can keep consumer DNA at the center of the product. That will always mean that adoption is easier. Your product has a much higher chance of going viral. It becomes easier to sell into the organization. So always make sure you bring consumer DNA into the product development process.

始终关注用户。因此，再次强调，现在构建企业软件公司的神奇之处在于你可以将消费者基因保持在产品的核心位置。这将始终意味着采用更容易。你的产品有更高的机会走红。向组织销售变得更容易。因此，始终确保将消费者基因带入产品开发过程。

And your product should sell itself, but that does not mean you don't need sales people. So this is a really important distinction. Leverage everything about the internet, leverage everything about users to get to your customers. But you still will likely need sales as a way to help your customers navigate your product. Help your customers navigate the competitive landscape and the ecosystem. So you're going to want consultative, very, very domain-oriented sales individuals that are gonna be able to be helpful in your customer being deployed and enabled.

你的产品应该能够自我销售，但这并不意味着你不需要销售人员。这是一个非常重要的区别。利用互联网的一切，利用用户的一切来接触你的客户。但你仍然可能需要销售来帮助客户使用你的产品。帮助客户应对竞争格局和生态系统。因此，你需要咨询式、非常非常面向特定领域的销售人员，他们能够在客户部署和启用过程中提供帮助。

But don't make that be a substitute. Do not make that be a handicap for not building a great product. So you fundamentally have to keep the product at the center of that. Company called Mixed Panel comes in through the developer, and then over time is able to sell to that organization with a more inside sales-led process. Also read these three books: Crossing the Chasm, Innovators Dilemma, and Behind the Cloud. These three combined, especially if you binge and just read them all, you'll definitely come out the other end with a massive software company. And so, in closing today, right now when you leave this room, is an amazing time to go start an enterprise software company. So I wish you the best of luck. And of course, if it doesn't work, we are hiring. The only other caveat is please do not compete with me, because I have a lot of competition already. So ideally either come work for us, or build your own company. So thank you very much.

但不要让这成为替代品。不要让它成为不打造优秀产品的障碍。因此，你必须从根本上将产品置于核心位置。一家名为 Mixed Panel 的公司通过开发者进入市场，然后随着时间的推移，能够通过更多内部销售主导的流程向该组织销售。同时请阅读这三本书：《跨越鸿沟》、《创新者的窘境》和《在云端背后》。这三本书结合起来，特别是如果你沉浸其中一口气读完，你最终必定能创建一家大型软件公司。所以，今天结束时，就在你们离开这个房间的此刻，是创办企业软件公司的绝佳时机。我祝愿你们好运。当然，如果行不通，我们正在招聘。另一个提醒是请不要与我竞争，因为我已经有很多竞争对手了。所以理想的情况是，要么来为我们工作，要么创办你自己的公司。非常感谢大家。
