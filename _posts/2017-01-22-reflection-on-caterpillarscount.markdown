---
author: nickwu0715
comments: true
date: 2017-01-22 20:21:06+00:00
layout: post
link: https://nickwuedinburgh.wordpress.com/2017/01/22/reflection-on-caterpillarscount/
slug: reflection-on-caterpillarscount
title: Reflection on CaterpillarsCount
wordpress_id: 197
tags:
- Technical
---

So you guys might remember that back in August I took on a software project called "[Caterpillars Count!](http://caterpillarscount.unc.edu/)" for my software engineering class. Just last night I finally finished the development for the app. It's been a pretty crazy and awesome ride, so today I want to devote a blog post to talk about the experience and lessons learned. Because this is a technical post I am not going to cover the "softer" side of the project. I might do that in a future post.

Before I dive in, a little background info: CaterpillarsCount is an app developed at UNC, in which people upload insect observation in a series of partner botanical gardens, parks, etc, around North Carolina. These observations go on and generate a data visual of insect distribution and is further used to study migration routes of birds, but that's beyond the scope of this project.



### The goal



If you take a look at the AppStore or Google Play Store, you will be able to find the old version of the app, which is basically a one-page app for you upload surveys. So one of the first things we are set to do is to make it more like an app, with more pages to navigate and various convenience functions for our users.

![img_4730](https://nickwuedinburgh.files.wordpress.com/2017/01/img_4730.png)
*Just a page with a bunch of fields to fill out in the old app*

There are three main goals for this project:





  * Offline support. A lot of the parks where people take observations have poor or no Internet connection, but we want users to still be able to take surveys and photos.


  * QRCode. In a large forest it is pretty easy to get lost and forget which survey point you are at. We are solving this by printing a QRCode for each survey branch, and adding a "Scan QRCode" option for easier and more accurate input of location.


  * [iNaturalist](www.inaturalist.org) integration. We want CaterpillarsCount to be a much bigger thing, and the best way to do it is to also upload our insect photos to iNaturalist, a global database for biological observations.





### What we did



In this section I am going to discuss the high level implementation for each of the goals we set.



#### Offline



Going back to the example I mentioned before: you are in a forest, with no WiFi and very poor cellular connection. Obviously it will be very hard to upload megabytes worth of photos, if not impossible. So the sensible thing to do is to store them locally for now, and submit them later when you got back to the cabin with a cup of tea and nice Internet connection.

And that's what we did essentially. We used a [SQLite plugin](https://github.com/litehelpers/Cordova-sqlite-storage#readme) from Cordova and set up the schema. Then in the old survey page, we just dump the survey data into local storage when we cannot push to server.

Of course that's not the whole story. If we are storing all these stuff locally, we must have a place to retrieve them and submit them, and finally, delete them so we don't end up using too much storage on your phone. So a "Pending surveys" page is in order.

![img_4733](https://nickwuedinburgh.files.wordpress.com/2017/01/img_47331.png)
*The Pending Survey page in the new app*

You can see easily how many surveys you are storing, each with a nice little thumbnail. And a simple upload button that takes all stored surveys and submit them in one tap.

At this bit I made a technical sub-optimal choice. Clearly the upload procedure is identical for these surveys and for the old ones. Instead of extract that piece of code out into a lib, I just duped the code in two js files. Looking back it was a very stupid thing to do, since basically I am doubling our maintenance cost for upload. So a lesson for you here is DRY: _"Don't repeat yourself!"_



#### QRCode



This is actually a two-part task: QRCode generation on the website, and reading them in the app. Since I didn't do the website bit, I won't talk too much about it.

So for the app side, this was made extremely easy by importing another [Cordova plugin](https://www.npmjs.com/package/cordova-plugin-qrcode). Basically you just hook up the API to a click function and you are good to go.

I do want to say a few things here. There is this saying that web dev, or application programming in general, has become a job of knitting libraries together, which is bit of a no-brainer. I don't think that's true. Yes I am using another people's code blindly in this case, but a lot of the times it take more than that to include all these pieces of code under nice modularisation, and easy to understand and maintain. What we are doing is in a higher level of abstraction, but no less easier than say, system programming.



#### iNaturalist



If you have been on this site for a while, you may remember I wrote a [post](https://nickwuedinburgh.wordpress.com/2016/11/18/working-with-3rd-party-apis/) about using 3rd party APIs. And that's exactly inspired by this part of the project.

Basically we have to use their HTTP calls so that we can post new observation, linking to project, and upload photos on their site. It was a painful process in my opinion, partly because their documentation isn't exactly clear all the time, and partly because I am just not that familiar with variations of HTTPs honestly.

My advice for using these APIs are detailed in that post already, but I just want to restate the point that you just have to be patient with it. Try to read the doc a few more times is better than relentlessly tweaking your parameters sometimes. Another difficulty is there's not much help you can get online. If you are really stuck, try to send an email to whoever wrote that API in the first place. People understand their own code best.



### What happens next



Next week I will start to deploy the app into both AppStore and Google Play Store. This will challenging for me no doubt, because in the old days I just write the code and kick it off to someone who handles deployment. But by the end of month, you should be able to see it in the store.

Now that this project is about to end, I will be starting another new project really soon. So stay tuned in you are interested. Also send me potential projects you want we to take on, at the moment I am not charging anything :)

Alright hope you all have a great days, and I will see you next week.
