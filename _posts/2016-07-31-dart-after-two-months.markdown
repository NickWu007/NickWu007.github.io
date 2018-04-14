---
author: nickwu0715
comments: true
date: 2016-07-31 18:33:18+00:00
layout: post
link: https://nickwuedinburgh.wordpress.com/2016/07/31/dart-after-two-months/
slug: dart-after-two-months
title: 'Dart: after two months'
wordpress_id: 55
categories: [technical, dart]
---

Today I want to talk about Dart, a programming language I have been using both in-and-outside of Google for web development lately. Again, I am only into this field for about two months, so my views might not be quite comprehensive. But today we are going to talk about my experience and feelings in these two months.




First of all, what is Dart? Dart is developed inside Google, an alternative to Javascript for web development. The most famous team that uses Dart as primary web-dev language, where I happen to work on this summer, is [Adwords](https://www.google.com/adwords/). But since last year, Dart has also been integrated into other areas, such as [mobile development](https://www.youtube.com/watch?v=t8xdEO8LyL8), and even [server side code and scripts](https://www.dartlang.org/dart-vm). I won’t give too much detail about the language itself, because the [official site](https://www.dartlang.org/) already has plenty of language specs and examples to draw from, which I strong recommend checking out.




So what’s my experience with Dart? As mentioned before, I use Dart for work at Google, mostly for front end building, with some other exceptions. Personally, because I have really grown fond of it, I am hacking through a few code-labs available online. I will link most f them at the end of the post, along with some other sites that might be helpful.




After these two months of exposure, I do have fixed feelings towards Dart. On one hand, I really like it. It has a very Object-oriented style, which made the transition from Java so much easier for me. The APIs are largely the same, and the syntax looks familiar and easy to grasp. Another little syntactic sugar it’s got is lambda functions, which really cleans up the code a lot without harming readability. Also, Dart works very well with Angular 2, and didn’t take me long to make the two of them work nice together at all. Last but not least, testing Dart applications is pretty smooth as well. Because “pub serve” works continuously, and in the main app you can bootstrap each individual components, there’s no need to rebuild the app ground up every time you make a change, which would save a tremendous amount of time, especially when working with large-scale apps.




However, there are a few things I struggle with during learning and working with Dart. The biggest obstacle is to understand [Future](https://www.dartlang.org/tutorials/language/futures). What Future means is essentially making asynchronous calls, but having the main thread seemingly work without blocking. The idea is, and pardon me if this is a bit weird, letting the main thread ‘wait’ for the async call to come through, but at the same time, you can provide the returned value to other variables and functions before its actual value is determined. It takes quite a while to get around this, and even now I would get mixed up from time to time. Another struggle to mention is how to connect with backend. In Dart, there’s usually Providers, which components use to make HTTP calls, or talk to the backend APIs. At the moment, I have no idea how to make a RPC call, for example, to backend Java code from front end, and if anyone has an idea, please reply and let me know. 




In conclusion, I feel like Dart is a great language to learn for web-dev, and I will be using it for personal projects for the foreseeable future. Of course there’s more to improve, but I would recommend anyone who wants to do some web development trying it out.




Some useful links:




[Dart language tour](http://shop.oreilly.com/product/0636920025719.do)




[Dart web dev site](https://webdev.dartlang.org/)




[AngularDart](https://angular.io/docs/dart/latest/quickstart.html)




a few [code labs](https://www.dartlang.org/codelabs) for fun and a sense of Dart
