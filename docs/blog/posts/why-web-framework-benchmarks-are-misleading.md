---
date: 2021-07-15
authors: [nadeem]
title: Why web framework benchmarks are misleading
description: >
  Why web framework benchmarks are misleading
categories:
  - Opinion
---

I'm tired of hearing these arguments about how X web framework is bad or it won't scale because they are at the bottom of benchmark rankings and why you shouldn't pick it for your project.

<!-- more -->

When asked to justify, the common reply you will get is 'Oh it performs better, therefore, it will cost less money well that's true but it isn't always feasible to just go for the best performers. You can't judge a framework by one attribute, that doesn't answer all the questions. What about the business requirements? It doesn't even answer anything other than performance (speed in general terms) which doesn't even represent the traffic that your system is producing.

It makes no sense to ditch a framework because it's slow and it won't scale because we have to first understand the bottlenecks, and potential areas of improvement, and research different architectures and design patterns.

Even if you just bump all the hardware, it probably won't make much difference compared to others. Infrastructure scaling has gotten a lot more affordable and accessible and by the time you will run into issues where your hardware scaling isn't working or it's costing you a fortune to scale further or the framework you chose is specifically the problem.

It will probably be a problem that you want to have, meaning your project has reached a stage where it's worth spending more time investigating exactly what kind of issues you're facing (IO bottleneck? or CPU? or Memory?) and coming up with solutions for the problem at hand. What's the point of worrying about these benchmark numbers whether X does better or Y when your traffic is nowhere close to it?

Here are some of the things you should be worried about

- Does it fit our business requirements?
- Separate tools that are and aren't part of your current stack.
- Identify the learning curve of new tools and their benefits
- What will be the maintenance overhead?
- Is the tool being actively maintained?
- How's the developer experience? debugging and testing abilities?
- What does your team think about it?
- How much time and money it will cost?
- How's the community support?
- Surveying if there are similar technologies based on it

along with how well it performs compared to others

![XKDC about benchmarks](https://res.cloudinary.com/kibibyte/image/upload/v1691933207/JdhLSUEmM_xyqql7.png)

---

It's pretty common to find these kinds of comments in web dev communities especially against people who decide to pick Python frameworks (like Django or Flask). I'm not trying to upsell any Python to anyone it has its pros and cons. But to say something won't scale well and it's not better because others performed better in the benchmark where performance is the only metric that is very stupid.

Image credits: [itamarhaber](https://gist.github.com/itamarhaber/2dad94e3bdd2980bce73)
