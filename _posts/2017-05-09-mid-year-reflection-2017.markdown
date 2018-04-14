---
author: nickwu0715
comments: true
date: 2017-05-09 18:22:14+00:00
layout: post
link: https://nickwuedinburgh.wordpress.com/2017/05/09/mid-year-reflection-2017/
slug: mid-year-reflection-2017
title: 'Mid-year reflection: 2017'
wordpress_id: 315
categories: [technical, non_technical]

---

It's been a long time. I must start this post with an apology. Every semester then finals are close, it becomes hard for me to keep writing. Fortunately now that finals are behind, and a nice summer is coming, I will be back on my weekly posting schedule, sort of. I will explain this in the end.

So finals are done, time to pack up and leave school, and in my case, the US. This year of exchange has finally come to an end, and truthfully I am more than glad to have come to UNC. I have had the fortune to learn many new technologies, and more importantly, meet many more great developers along the way. I won't mention names, but please know that I thank you all for this hard but rewarding year.

With that said, today I would like to do a mid-year reflection on myself, particularly in the technical side. I will talk about things I did in these 5 months, stuff I learned, and finally my plan for the rest of 2017. So let's jump right in.



### Compiler: a regret



If you recall my [post](https://nickwuedinburgh.wordpress.com/2017/01/16/new-year-resolution-and-class-choices/) at the start of year, I listed the classes I am taking. Now I want to look back and see the result. Sadly, I have to admit that the compiler class didn't turn out to be too great for me. The main goal of the class is to build a mini-Java compiler in Java. The reason I wasn't happy with the class has nothing to do with that per se, the project is well constructed and clearly defined. I am a bit upset because I cannot take pride in my final code. As a developer, one should take responsibility, and pride in their codebase. If you don't even like your own code, it seems unlikely for others to like it either. For the second half of the project, I went with a less-optimal architecture, and found out it was too late to change it later. Also, I wasn't able to put in enough time and effort in development and testing. Overall, as the title suggested, compiler finishes with a regret. Ideally I would like to re-do the project sometime in the summer.

Nevertheless, I have put up the current codebase on [github](https://github.com/NickWu007/COMP520), for those of you who are interested.



### Serious Games: eye opener



In my serious game class, the main goal is to create a playable game with serious purpose for our client. My project is to create a game that lets players caption images, which in term builds into a training set for machine learning research. You can sign up and play out game [here](http://wwwx.cs.unc.edu/Courses/comp585-s17/Piclabel/datalabel_test/login.html).

![Screen Shot 2017-05-09 at 13.50.33](https://nickwuedinburgh.files.wordpress.com/2017/05/screen-shot-2017-05-09-at-13-50-33.png)

For this project I was in charge of backend and database. I went php, mostly because it has a nice interface with MySQL. Everything is hosted on the CS department, except for the images, which come from [mscoco](http://mscoco.org/dataset/#download). I did a full ORM layer between SQL and the API as well. Overall, I think I did ok with this project.

What's interesting with this project comes from two places. One, in order to engage viewer, my colleague designed a robot character that goes along the game, and talks to play every now and then. The dialogue was designed to be funny but also helpful. I was amazed by how much those dialogues worked in our presentation round. People seemed to really like it. Personally I would never be able to write those up, partly because I am not really a humorous person, but also the sheer difficulty to construct the dialogues. Bottom line is, in game design a creative mind is very important, if not necessary.

The other bit that really shocked me is our front end dev Aaron, who mocked the prototype in a week, with pure Javascript, not even jQuery. Because I did some front end work in the past year, I know how hard it is to even simply layout a few divs like I want. Aaron did not only that in a flush, but also added audio, fancy animations, and navigation tabs, many of which I have no idea how to implement. I am very lucky to be have access to his source code, and I plan to thoroughly read it during the summer.

Our source code can be found on [github](https://github.com/NickWu007/Data-Labelling) as well.



### Distributed Systems: into the weeds



The most challenging class of this semester. The class started with 35 people, and ended with 15 or so. We did a lot in this class: Java NIO, RMI, and a special RPC implemented our professor, called [GIPC](https://github.com/pdewan/GIPC). My friend John has [a nice post](http://espenhahn.org/john/#/posts/rpc%2FRPC) on RPC, and I encourage you to read it.

We also learned about consensus mechanisms, with [Paxos algorithm](https://en.wikipedia.org/wiki/Paxos_(computer_science)) as the most complicated one. To give a short description, Paxos is an algorithms sued to achieve a certain level of consistency over many participants. John also has a [Paxos implementation and illustration](https://github.com/JohnEspenhahn/ResourceAllocation#likepaxos) online, if you are interested to learn more about it.

My own code can be found on [github](https://github.com/NickWu007/COMP533) too. I also explain each assignment and implementation plan in README.



### Datacenter: learning to be a researcher



Being a graduate level class, this one focuses on high level architectures employed in datacenter. The topic ranges from intra-datacenter networking to database architecture, to real time analysis, and so on. I learned some basic MapReduce, which I did a [post](https://nickwuedinburgh.wordpress.com/2017/02/19/stepping-into-mapreduce/) already. The project is to find a dataset online and do some analysis with it. Sounds simple, but it is not that easy when I actually sat down and try to implement. In the end I chose to analyze a very little subset of the data, and draw observations from that. You can my code on [github](https://github.com/NickWu007/COMP790). I also included the research paper, which is part of the project.



### Plans coming up



Unfortunately after this post I will be absent for another 3 weeks, due to family visit and some other personal business. At the start of June I will go back to London and restart our schedule. In terms of topic, I plan to do series on [dartlang](https://www.dartlang.org/), a language I have talked on the site before. This time I want to go deeper and try to write some short tutorials on how to design and implement a web/mobile app quickly and effectively. So stay tuned on that. In the meantime, if there's a topic you want me to talk, technical or not, feel free to leave it down in the comments, or reach out to me on [Twitter](https://twitter.com/WujunaoNick).

In terms of what I planned for the rest of 2017 personally, I will do a separate post on that topic, given this post is already long enough. :)

Lastly, I am happy to go through everything over the past year. I look forward to going back to Britain and seeing old friends too. Hope you all have a nice summer, and I will see you in three weeks!

Nick
