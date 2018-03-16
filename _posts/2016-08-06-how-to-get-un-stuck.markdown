---
author: nickwu0715
comments: true
date: 2016-08-06 22:23:33+00:00
layout: post
link: https://nickwuedinburgh.wordpress.com/2016/08/06/how-to-get-un-stuck/
slug: how-to-get-un-stuck
title: How to get un-stuck?
wordpress_id: 66
categories:
- Non-technical
---

Being a programmer, the biggest obstacle you face sometimes may not be a complicated algorithm problem, or trying to learn a new framework you are familiar with. A lot of the times it is because of you are just 'stuck', unable to keep smashing on those keys. Today I am going to talk about some real-life experience on this topic, how I got into being stuck, and how I eventually got out.



### A real problem



Before I decided to write this blog, I wasn't sure how much time I actually spend to get un-stuck, compared to my standard workflow, so I did some recording, for the past five days at work. Turns out I spent twice as much amount of time to get back on track as to writing code normally. This is far too real, and being stuck could literally eat away a whole afternoon of mine, with nothing to show at dinner.

So I went home last night, sat down and really thought through some moments I got stuck, and got out, and I feel like just telling you these stories, instead of giving advice. I will link a few web pages, where some of my methods come from, so that you can take a look and decide if it is right for you.



### Story 1: Santander Cycles



I am sure the title might not mean anything to you, especially if you live outside of United Kingdom, so allow me to give some context. Back at uni, we had a graded project to emulate the software system used in [Santander Cycles](https://tfl.gov.uk/modes/cycling/santander-cycles), a public transportation around London, where you can essentially rent a bike at a station, ride it, and return it at any station around the city. What's special about this project was that it was my first relatively large-scale to work(22 classes, not including unit test) on solo. I know this is probably nothing, compared to the complexity of industry software project, but at the time it was a huge deal for me. The instructor chose this to demonstrate [Observer Pattern](https://en.wikipedia.org/wiki/Observer_pattern), so when I first looked at the package, I immediately regretted taking this on my own.

After talking myself out of backing out and grabbing a partner, I sat down and try to read the template code given. I had a rough idea of what the observer pattern is, but hadn't looked at any code that actually implements it. Of course I was complete lost, and I had no idea where to start coding even. The more I tried to read it, the more tangled the dependencies go.

The root of the problem, essentially, was that I didn't fully understand Observer pattern, even after reading the lecture slides and book. So I took a desperate try, searched "Observer pattern examples" on YouTube, and watched a [video](https://www.youtube.com/watch?v=wiQdrH2YpT4) with examples and clear explanation. It was only after that video when I realised I had been reading in the opposite direction, the trigger shouldn't be at central control; it should be coming from individual device classes. An hour later I completely got around the structure, and started coding happily.

My takeaway from this is that you need to have a solid foundation when first coming into a new project. Design pattern, framework, unit test structure... You have to be reasonably familiar with the large structure of the project before you even go into reading existing code. What I did was really a waste of time, and it could get you quite frustrated, which may delay the deadline even more.

A side footnote: that project took quite a lot of trouble with testing as well, with the same reason: not familiar with the test strategy. I used the same approach and was able to get through with less time and headache. In the end I a pretty good grade out of it too.

![IMG_4331](https://nickwuedinburgh.files.wordpress.com/2016/08/img_4331.jpg)



Last system test passed! I cired a bit for real.



### Stories 2: CSS, why can't you just WORK!?



If you have done some front end web development, I am almost positive that you have run into trouble with css, with elements not looking like what they supposed to. This stories happened to me just this week, and I would like to tell you guys about it.
I was doing some page layout stuff for work. We had some components set up alright, and all I needed to do is: a) wrap the components into separate divs to give them font styling and other stuff, and b) make the top-level divs stretch to fill the page. Pretty easy stuff, you would say, and I would probably second you, but when I actually tried to strech the elements, the headache was way too real. Either the div panel didn't strech at all, or the components got misplaced when the div panel stretched. After banging on the css file for an hour, I feared if I could even keep coding in the afternoon.

Quite luckily, when I was pouting with my lunch at the cafe, a few colleagues walked by and invited me to play some foosball. I wasn't feeling looking at the css file anyway, so I said yes. We played for about half an hour and worked out some sweat(in a room with AC!), and I totally forgot about that css fiasco.
When I went back to my desk, a bit reluctantly, and sat down with the css again, it turned out when I wrote the mixins I accidentally forgot to include `flex-shrink(0)` in, and some padding got numbers way off. The `display-flex` thing was working perfectly; it is the components styles that got messed up.

A lot of the times we run into problems that we know are not at all hard, but we just cannot quite sweep through it. What I find useful is to just walk away from it for a bit. Get a coffee, go to the gym, or simply take a short walk outside. Anything that doesn't relate to the problem you are stuck on. Quite magically, when you come back and take a second look, it would solve itself.



### Stories 3: Er... What?



This also happened this week, and I am just gonna get into it. When working on a side project, something to practice my Dart skills, I got into a situation that one of my components wants to access data from another, and the problem is they are not close in the DOM tree, I actually have to pass the root div to get to one from the other. Normally the way to pass values is through an event with `@Input()` or `@Output()`, but either way I will have to travel a absurd amount of distance, wihtout messing up anything along the way. But I couldn't find another easy way to do this. However, I paused here before getting into coding: a) I sitll believed there's a better way, and b) I really don't want to event passing layer by layer.

The next day I asked a dart developer at work. He listened very carefully, and then asked me:"Do you know about dependency injection?"

That's where the title comes from, btw. I read about it on the dartlang site, but figured that was only used for services like HTTP and stuff. He told me to inject a `ModelController` into my components and let it hold on to the data, and then inject it into the other component so it can access that data.

Sounds too good to be true. I felt like just getting some black magic spells. When I tried what he suggested, it worked perfectly, with almost no latency.

No matter what language you use, there's always some 'Ahhh' ways to solve your problem, instead of hacking relentlessly. Ask someone with more experience, usually they can solve your problem faster than you realised.



### Conclusion



In all, if I can give just one piece of advice, that would be do not freak out and declare beaten when you got stuck too long. Every problem is hard until someone cracks it. Don't ever lose hope, you are tougher than whatever is in your way!



#### Some useful links







  * A nice post on [Stack Exchange](http://productivity.stackexchange.com/questions/4827/what-do-you-do-when-you-are-stuck-at-programming-and-you-dont-have-access-to-in)


  * [Four Kinds of Stuck](http://www.jeffwofford.com/?p=838)


  * [A Programmer’s Greatest Enemy](http://www.jeffwofford.com/?p=835)


  * [Rubber duck technique](https://blog.codinghorror.com/rubber-duck-problem-solving/), I use this a lot for debugging. Make sure you do not disturb others around though. ;)


