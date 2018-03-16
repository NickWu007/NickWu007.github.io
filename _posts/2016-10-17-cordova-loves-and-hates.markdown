---
author: nickwu0715
comments: true
date: 2016-10-17 01:28:30+00:00
layout: post
link: https://nickwuedinburgh.wordpress.com/2016/10/16/cordova-loves-and-hates/
slug: cordova-loves-and-hates
title: 'Cordova: loves and hates'
wordpress_id: 144
tags:
- Technical
---

I am pretty sure that if you are a pure web dev or a pure mobile dev, the word Cordova doesn't really mean anything to you. To give a 50,000 feet overview, [Cordova](https://cordova.apache.org/) is a platform from Apache that migrates a web application to a "sort of" native app. So say you have a web app running, potentially you can just dump the source code into a new Cordova project, build it, and then you have a virtually identical app on mobile, iOS and Android, and other platforms.

So I have been using Cordova for one of my uni projects for the last two months, and boy it has been a crazy ride. The project won't be finished by Xmas, but at this point I am familiar enough with Cordova, and I want to write a "review" for it. Please note that I am not a Cordova expert, nor a great web developer, but these are my true feelings after working with it for 8 weeks.



### What I love



First of all, Cordova does live up to its name to be a great framework for web developers to do mobile dev. When I got the base code from the original app, it was very easy for me to read through all of the contents pretty quickly, because a)it is all Javascript, and b)there's virtually nothing you need to read that specific to mobile platforms. This separation, or a sense of mirage so to speak, gave me a really easy time for development. I could even use the old web dev workflow, which is code on one screen and a preview page on Chrome on the other. It almost felt too easy to do, because even today I don't much about native mobile development, but the app we got looks decently "mobile".

Another thing I enjoy a lot is its [CLI](https://cordova.apache.org/docs/en/latest/reference/cordova-cli/index.html). A lot of building and testing, if not all, can be done in the shell. This is especially good to have because I need to use git in command line anyway, and not having to switch to other tools to build the app saves quite a lot of time.



### What I hate



First of all, debugging. This is a common pain for all programmer in a sense, but I certainly felt it took more if I was writing in Java, or Dart. The reason for this is we are using a [SQL database plugin](https://www.npmjs.com/package/cordova-sqlite-storage), which contains a great chunk of code, like opening and configuring, in the `deviceready` event, instead of the usual `ready` event. What this implies is that I cannot use chrome to test or debug the app, because `Cordova.js` is generated on the fly, and `deviceready` behaves weirdly in Chrome. A lot of dev time has been used to constantly rebuilding the app, loading it into the emulator, and testing there. Again, this could just be my lack of experience with Javascript, but I feel like the platform has some to blame as well.

Another great source of pain, as mentioned previously, is the plugin. These are little piece of code other programmers wrote, and made open source for our convenience. Although it sounds awesome, I have had my fair share of struggle with them. To give an example, when we first of the app, it works perfectly when I try to take a photo in app. Despite of the fact that I haven't changed any code regarding the photo, when I was doing regression testing two weeks later, hitting the camera would inexplicably crash the app, with no errors or warning in the log. It took me a few days of reading logs line by line to figure out what went wrong. Turned out the in iOS 10, the newest version released a few weeks ago, you have to include a "reason" for using any hardware, or accessing photo library. Because we haven't updated with that, it just crashes the app. Another thing I found annoying is that you need to delete and re-add some plugins to fix some issue, and that is the only way. Cordova should have an "update plugins" command too.

One last thing, and this is not to Cordova itself, is that if you are using git for version control, make sure you do NOT include anything in the `platform/` directory. Otherwise every pull request would contain thousands of lines, and dozens or hundreds of files changed, where you knew you only changed two js files.



### Final thoughts



To give an overall decision, I would still say Cordova is a good framework to use, for turning a web app into mobile app. But if you are using Cordova to do mobile only stuff, and just don't want to learn swift or Java, you might want to rethink it.

A few other posts about the topic:
[LESSONS LEARNED FROM 5 YEARS OF PHONEGAP/CORDOVA DEVELOPMENT](https://agingcoder.com/programming/2015/02/21/lessons-learned-from-5-years-of-phonegapcordova-development/)
[Lessons Learned from Creating a Reliable Mobile Build](https://www.pagerduty.com/blog/reliable-mobile-build/)
[Lessons learned on Apache Cordova](https://rlogiacco.wordpress.com/2013/05/25/lessons-learned-on-apache-cordova/)
