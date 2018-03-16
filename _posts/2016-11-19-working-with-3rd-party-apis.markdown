---
author: nickwu0715
comments: true
date: 2016-11-19 02:20:31+00:00
layout: post
link: https://nickwuedinburgh.wordpress.com/2016/11/18/working-with-3rd-party-apis/
slug: working-with-3rd-party-apis
title: Working with 3rd party APIs
wordpress_id: 177
categories:
- Technical
---

Recently a lot of my work, both for school and privately, have been working with external APIs, or 3rd party API which is the term more commonly used. Today I want to talk about these APIs: what they are, why we use them, and some gotchas and best practices.



### 3rd Party API: what is it?



This might be an unnecessary section, but just in case you are not familiar with the term, API stands for _Application program interface_. Basically 3rd party APIs are code we used but didn't write ourselves, or in the case of a company, not in the company codebase. Common examples include "Log in with Facebook", "Share this on Twitter", etc. What makes them work behind the scene is that those websites are using Facebook or twitter's API and thus makes a connection to those sites.



![nzsef](https://nickwuedinburgh.files.wordpress.com/2016/11/nzsef.png)
*A simple view about 3rd party APIs*



### Why do we use them?



This pretty much goes without saying, but one can immediately see the benefit of using them to bring a better UX. One of the greatest concepts of the Internet is to connect people, and the more we developers can do, the better. Also from a more practical point of view, it makes little sense that I have to write a screen reader on my own, which takes time and might contain bugs, when realistically I can just use one from the web which has been out for years, fully tested and well adopted.

That being said, there are potential dangers when it comes to using 3rd party APIs. The core idea is that you are surrendering some of your app to others. In other words, when you are using 3rd party API, you are depending on other people. It is a kind of blind trust that could potentially hurt you and your users.

In the following section, I will try to bring up a few common problems when working with 3rd party APIs, why they matter, and how we can do to mitigate the risks.



### API selection



You need to be careful even before start working with an API, because it could well be the case that you chose one less competent for your purposes. With more and more people coming into the industry, there will almost surely be more than one way to do something. Dozens, if not hundreds, of libraries exist in the public domain for task simple as generating an email programmatically. Before going straight into an API, you might want to consider these:





  * Rate of calls you can make. Usually API providers would charge you if you make a huge amount of calls to their APIs in a short period of time, which makes sense because you are adding more stress to the server than average user.


  * Is the API widely adopted? How often do providers update it?


  * Is the API reliable. Its up and down time. This might be the most important aspect, since you don't want it to go silent when your users are expecting responses.





### Documentation



To help other developers use their API more efficiently, most API providers have a documentation page online, explaining what each method is for, their example use, etc. Unfortunately this comes with the caveat that usually the docs are written for people who understands what they are doing, which is generally not the case. I don't what the difference is between a "survey" and an "observation", so I don't know which method to use when I just want to upload something I see. This in my opinion causes the most frustration when trying to using 3rd party APIs.



### Rapid change



Bear in mind that the providers are developers too, and they must make their API compatible with the rest of the world. This means change in methods, adding or removing a parameter. Or it could be change in the control flow completely, to match the common pattern used in the industry.



### Best practices and tips when using 3rd party APIs



I hope these don't discourage you from using 3rd party APIs. Here are some tips to make working with them easier and safer:



#### Separation



This is usually a good practices that you wrap all external API calls into a separate class, both for reality and safety concerns. For example one could use `twitter_api.js` to contain all calls made to twitter. Also these files are place in a separate folder, like `lib/`.



#### Deal with outage



Sometimes API outage is just unavoidable, but we can we our best to still hold up when it happens:





  * Caching. This is a common technique widely used. I won't go into too much detail about it, but check out [this post](http://blog.smartbear.com/api-load-testing/safeguard-your-application-from-third-party-api-outages/) and [this one](https://www.bluelinemedia.co.uk/blog/entry/web-design/blog/speed-up-third-party-scripts) for more information.


  * Use a backup plan. For example you have a second set of APIs to call if the primary ones fails with a timeout or 500 error.





#### Authentication



This is a touchy area because it concerns user info, and everyone wants to protect their passwords of course. Usually, when you are using some company's APIs, say Facebook, you must register yourself with them so that they can make sure you are not doing anything inappropriate. Before using their APIs, you must authenticate yourself using a public app key and secret key. A common security flaw people would make is to hard code them in client code when making auth calls, which basically exposed your login combo to anyone who knows to "inspect source code". The right way should be hard coding them in your server, and have an extra HTTP request to retrieve it, because your server code is hidden from user, and you can do all sorts of checking there. For more about this subject, have a look at [this post](http://www.theodinproject.com/courses/ruby-on-rails/lessons/working-with-external-apis), which gives more implementation detail.



### Conclusion



So in the end, is 3rd API a good thing or not? The answer is both in my opinion. It brings tremendous advantage for code reuse, but also has all sorts of things for developer to worry about too. I think one should try to establish a framework when using them, and that is hard too. Fortunately, everything gets better with practice, and we can make this a better experience for developers too.
