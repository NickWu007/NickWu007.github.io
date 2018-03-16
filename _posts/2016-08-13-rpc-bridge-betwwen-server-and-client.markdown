---
author: nickwu0715
comments: true
date: 2016-08-13 21:24:16+00:00
layout: post
link: https://nickwuedinburgh.wordpress.com/2016/08/13/rpc-bridge-betwwen-server-and-client/
slug: rpc-bridge-betwwen-server-and-client
title: 'RPC: bridge between server and client'
wordpress_id: 82
tags:
- Technical
---

### Problem we face



When developing web application, most of the time people split to do server and client side code. Server side has the backend logic and all the beautiful algorithms, and client side has the UI, rendering objects to the user. One of the biggest problem I find is that usually server code is written in Java or Python, but client side code is in Javascript, php, or dart, etc. How do we make codes in two or more languages, living in different machines, talk to each other?



### HTTP and RPC



When talking about server/client communication methods, two main approaches come to mind quickly: HTTP and RPC. HTTP has been such a widely used and one of the most fundamental part of networking, that it needs little introduction, but what I want to cover today is RPC, short for [Remote Procedure Call](https://en.wikipedia.org/wiki/Remote_procedure_call). Before I go into the detail, you might wonder: isn't HTTP enough? Why would we need a second paradigm for server/client communication?

As it turns out, to some particular applications RPC serves better than HTTP. It offers far more languages integration, which leaves some applications no choice but to use RPC. Also, one aspect of RPC that gets a lot of critics is its tight coupling. Client side code must provide the exact type of arguments, in the correct order for the call to be made. Although one could argue that having such coupling makes clients valuable to breakages, I feel like the these calls are specifically designed for certain purposes, and it is reasonable for the client program to know some grounds about the call before making one. For more discussion on this topic, checkout [Api Handyman's post](https://apihandyman.io/do-you-really-know-why-you-prefer-rest-over-rpc/) and [this post](http://programmers.stackexchange.com/questions/181176/when-are-rpc-ish-approaches-more-appropriate-than-rest). Nevertheless, knowing about RPC won't do any harm to you as a developer, so I decided to give you a rough introduction anyway.



### RPC



First of all, before we go into too much detail, I'd like to explain what exactly constitutes RPC first. When trying to understand this, you can really not think about the remote bit first, since that just blurs things right from the start. Imagine your client Dart code wants to talk to Java backend and get a result from some user inputs. At the top level, what you want to do is to bundle the front end data as a Request, send it off to some method, which returns the Response to the front end to be displayed. RPC is really just like that, only that the backend call is remote, in other words the server code must be reached through a network.

What makes this all possible is a layer between the front end and backend, called a message stub. What this does is to receive and send out request and response messages between server and client. On the two endpoints, it looks like all method calls and message passing are local, but it is really the logic of the stub that takes care of all these. Inside Google, this layer is coincidentally called "Stubby".

The following picture illustrates a life cycle of a typical messaging from and back to the client, credited to [Dave Marshall](https://www.cs.cf.ac.uk/Dave/C/node33.html):

![rpc](https://nickwuedinburgh.files.wordpress.com/2016/08/rpc.gif)

I recommend thoroughly read through this page to get a solid understanding about RPC.

One thing to consider is the message format between server and client. Obviously we can't use native objects because they are definitively not compatible. One option is JSON, but that way it is basically not much different from HTTP. What's used in Google, and now many other companies and businesses is [Protocol Buffer](https://developers.google.com/protocol-buffers/) a serializible  cross-language, cross-platform messaging language to wrap data. Internally, this works with stubby and various programming language seamlessly. There's also a framework from Apache called [Thrift](https://thrift.apache.org/) that does similar jobs.



### Takeaways



So how does everything I wrote above relate to you? Well, if you are thinking of developing a web application with a fairly complicated back end logic, you might want to forget the idea of translating it to whatever front end dev language and embrace RPC or HTTP, because it keeps things separate, easy to add features in the future, and overall very much more organized. If you are already using some sort of RPC paradigm for your work, I hope this post gives you a deeper understanding about it. Personally, when I started using RPC calls for work, it all looks like black magic to me, and I didn't quite understand the mechanism. This is largely I want to write this post, so that you, and me, could have a firmer gasp of this subject.



### A little ending remark



It has been a month or so since I started this blog, I am truly ecstatic and grateful for everyone who has read these post. This week is practically my last week of work, and a week later I am going back to school. So I would like to know if you guys like this style of blogging, with a wide range and relatively less depth, or a deep dive into one technology(e.g. dart) particularly? Please let me know in the comment section.

Also, because I am moving across the country next weekend, I am afraid I won't be able to write a post. I am truly sorry for this arrangement, and I will keep the posts coming regularly as soon as I settle down in my new flat.

Till next time, hope you all have a nice day!

Nick Wu
