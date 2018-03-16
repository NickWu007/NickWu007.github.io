---
author: nickwu0715
comments: true
date: 2016-11-05 21:19:53+00:00
layout: post
link: https://nickwuedinburgh.wordpress.com/2016/11/05/web-dev-crash-course/
slug: web-dev-crash-course
title: 'Web dev: crash course'
wordpress_id: 159
categories:
- Technical
---

As you probably know, my "specific" job title would probably be a full stack web developer. Recently I have gotten some questions about web dev in general, from others who want to start their own website, and are a bit lost after diving into the ocean of web. In this post, I am going to try and give an overview about web development in general, and some thoughts and tips I find useful while learning web dev.



### Backbone: MVC



Nowadays most web applications, like Facebook or Twitter, adopt MVC as their design principle. MVC stands for Model-View-Controller, a famous design pattern used for applications that are user-specific, responsive without reloading the page, and usually have a database in use. The idea of MVC is to separate components into three "tents". The Model tent contains user info and data from backend database. The View tent has components that are generic for display purpose. In other words, they only present stuff to the screen for user, but probably have no idea what they are presenting. The last tent, Controller, is like an ambassador between Model and View. It fetches data from Model and gives it to View, and updates the Model in case user indicates something to be changed in the View. For a more comprehensive explanation, take a look at [this blog post](https://blog.codinghorror.com/understanding-model-view-controller/). It is very important that you understand MVC, because the components I am about to present all fall into this paradigm.

![mvc](https://nickwuedinburgh.files.wordpress.com/2016/11/mvc.png)



### Components: What you need to make a website



MVC is just an abstract principle, how are real life websites made of? These are some common, if not essential, components:





  * Front End



This must be the first thing you have in mind: a website must have something for me to see, right? Absolutely, but front end is more than just the HTML page. Front end has the html and css for static content and styling, but it also has client side code, which mostly acts as "Controllers".

For example, Gmail's front end has static html page and css that defines where things are supposed to be(I am glossing over a lot, just to make the point). However, it front end is also responsible for getting all your emails from the server, and update the server/database when you choose to delete 50 commercials in your inbox.



  * Back End



This is commonly referred as the server. This part contains your "business logic", interactions with the database, and other "invisible" things. One of the difference between front end and back end code is that front end code can be seen by others, because the browser has to load them in for execution. But they cannot see your back end code because it is stored and executed in a server machine.(Part of the reason why back end kind of means server) In other words, if you don't want others to just read your code for some reason, maybe there are some fancy algorithms, you should put them in back end. In general, back end mostly acts as Model in our MVC pattern.



  * Database



This part is pretty self-explanatory. Believe it or not, this is a separate section actually. Your back end should only interact with the database, not own the database. The reason for this is mostly for security concerns. Companies like IBM and Oracle have very good protection mechanisms for client data. It is safer to put your data there because security leak is like a death mark these days.



### Tips and advice



So with all this knowledge, how would you go and become a web developer? First of all, with all these components and sub-components, there are many kinds of web developers too: UI designer, front end dev, back end dev, security, data admin, and so many more. Personally I have only been a front end and back end developer, so I can only give some advice regrading these two. Also bear in mind I am only talking about development. There are also fields like testing, QA, deployment, etc.



#### For front end



In terms of UI designing, Chrome is your friend. There are many built-in tools in Chrome for web dev. Most of you have heard of "inspect element" of course, but there is a little trick I learned while doing css layout. A lot of the times I would make some change to my css files, hit refresh, and pray for them to work. But you can actually modify css in Chrome directly and see the change on the fly. So the faster way to do it should be fiddle with css in Chrome dev panel, make sure it works, copy the change into file, and then refresh to make sure it actually works.

When you are doing client programming, I will use plain Javascript as example, chances are a lot of the events are not happening, or their handlers just mysteriously stay silent. You can manually fire an event with jQuery, like `$('.input').change();`. Events are the largest trouble I have been dealing with when doing front end. Just read more about this, because you are almost guaranteed to see them at some point.



#### For back end



If you are using PHP, I am afraid this part won't be too helpful to you, because I have not use PHP a lot, and frankly it doesn't work well with me. But no matter what language you use, thoroughly test it before it hook it up with front end. Also HTTP and Stubby are sometimes hard to get hold of. There is a previous post I wrote about the subject, and I recommend you checking that out too. Also you are doing error handling for requests, PLEASE give as much information about the failure as possible. This would help front end a lot when you hook that up.

That's it for this week. If you have any question for me, either leave comments below, or email me at wujunao19950715@gmail.com. Hope you all have a nice weekend, and I will see you next week!

Nick
