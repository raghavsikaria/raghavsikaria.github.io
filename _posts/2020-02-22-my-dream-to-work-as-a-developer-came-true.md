---
layout: post
title: "Applying t-SNE to NIFTY50 Stock Day snapshot and visualizing all iterations using Bokeh(Python) - JSCallBacks & User Interactivity"
date: 2020-04-06 22:00:00 +0530
tags: tech python bokeh data_visualization machine_learning
description: "My first article?"
---

[Originally published on LinkedIn](https://www.linkedin.com/pulse/applying-t-sne-nifty50-stock-day-snapshot-visualizing-raghav-sikaria/)

Hello reader, ever stumbled upon a tricky dataset with large number of features? Tried PCA and had to be content with linear dimensionality reduction?
Explore how I've leveraged t-SNE non-linear dimensionality reduction algorithm on NIFTY50 Stock data day snapshot and visualised all the iterations using Bokeh, Sklearn and Python.

And if you've never coded before, why not start now? This very moment!

Hello connections, in the last article it was about understanding correlation between features in dataset and visualizing them. Now, picking up from where I left, it is about reducing dimensionality of a high-dimensional dataset and trying to visualise it. The dimensionality reduction algorithm I have chosen is t-SNE which can be employed from sklearn package in Python, applied on NIFTY50 Stock Data(snapshot from 3rd April 2020)(thank you NSE India) and visualised using Bokeh-Python.

I've received a lot of feedback from last time, am more generous with the references so that any uninitiated person will have access to all resources right from the start. The codebase is somewhat lengthier, so have made sure it is sufficiently commented for readability.

P.S: Like always all code can be found here on GitHub.

TL;DR - This is what this article/mini-project attempts to achieve:


<iframe src="/assets/post_imgs/tf__tsne_bokeh_nifty.html" width="100%" height="500" style="float:left"></iframe>






<!-- 

Two years ago I wrote a post about [how I learned to code at the age of 38](http://www.natalyakosenko.com/2018-01-06-how-i-learned-to-code-at-the-age-of-38). Back then I was full of doubts if I ever will be able to work as a developer. I felt like I was trying to catch a train that left twenty years ago. But at the same time I could not see my life without programming any more.

These few years I spent [studying](http://www.natalyakosenko.com/2018-10-06-what-it-feels-like-to-study-masters-in-ntu-singapore), [attending programming meetups](http://www.natalyakosenko.com/2018-06-29-my-first-programming-meetup-experience), and, the most importantly, coding and learning, learning and coding almost every day including weekends.

One day a few weeks ago my boss showed me a new organisational chart, and under my name there were two words: "Team Lead". No more "manager", no more "head of whatsoever". Before that in my wildest dreams I was imagining myself becoming a developer, but I never imagined it to be that simple and that natural. Until now I did not even celebrate it.

First few weeks of my official developer life were on the one hand full of happiness, as I've been doing the things I love and was even getting paid for it. On the other hand I had to face some challenges of being a real developer. It's not a hobby any more, it is real, and the challenges are real too.

First challenge was __making technical decisions at work__. Well, I did not make any big decisions, but even small decicions were not so easy. I thought: "I will read about it, then will discuss it with someone, and if still in doubt will ask the team lead for advise. Oh shit, the team lead is me!.."

Second challenge was realization of the fact that __there is a lot of legacy software around__, and you and your teammates still need to maintain it or rebuild it, no excuses. Unfortunately you can't say: "I am not the one who built it, it was built when I still was in a kindergarten, I do not know this programming language, there is no documentation, there is no tests, so don't ask me about it" (although I was trying to say this a few times :))... When I was doing coding just for fun 3 years back, I could choose which part to work on. Now I still can choose, but not everything, something simply have to do because have to do.

Third challenge is __amount of learning__ I am doing every day, and paradoxically despite that I feel myself like Socrates, "I know that I know nothing", or rather like Albert Einstein, "The more I learn, the more I realize how much I don't know.".

There were many other small challenges, mostly technical, like rebuilding one of the "naugthy and misbehaving" Python/Django applications during Christmas holidays, or setting up CI/CD pipeline, or start writing unit tests (which I admit I was not very good at).

Apart of challenges there were some good things too. The biggest thing I like about being a developer so far is equality. I think programming world is a very fair world, everybody is equal. It does not matter you learned programming at the age of 38 or 15, or which country are you from, or what is your gender. Only matters is your learning journey, and what do you do with your knowledge.

Let's see how it goes, I hope will keep enjoying and hope will be able to eliminate some of the challenges. -->