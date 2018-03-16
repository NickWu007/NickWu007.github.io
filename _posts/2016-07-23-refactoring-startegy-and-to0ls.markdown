---
author: nickwu0715
comments: true
date: 2016-07-23 23:50:39+00:00
layout: post
link: https://nickwuedinburgh.wordpress.com/2016/07/23/refactoring-startegy-and-to0ls/
slug: refactoring-startegy-and-to0ls
title: 'Refactoring: strategy and tools'
wordpress_id: 34
categories:
- Technical
---

Refactoring. The one thing in work that probably has the same priority as business meetings with the marketing team. JK. As a matter of fact, in my own experience in the industry so dar, more time is spent worrying if the code I write now needs least refactoring in the future, compared to time spent writing development code it itself. So today we are gonna talk about refactoring, my approach about working with a large-scale refactor, and some of the tools I used.

First of all, one must understand what refactoring actually means, before getting into the strategies and tools. Michael Feathers , who composed the famous [Working Effectively With Legacy Code](https://www.amazon.com/Working-Effectively-Legacy-Michael-Feathers/dp/0131177052?ie=UTF8&tag=stackoverfl08-20), puts it like this:


<blockquote>A change made to the internal structure of software to make it easier to understand and cheaper to modify without changing its observable behavior… It is a disciplined way to clean up code that minimizes the chances of introducing bugs.</blockquote>


In short, refactoring is what makes the current code looks smoother, easier to update and extend, for lack of a better summary. So how do we do it?

Naturally there are a number of strategies proposed and used by engineers over the years, but in the post I am gonna only talk about the one I usually use. It shares some similarity to Sibylle Peter and Sven Ehrke’s [master plan](http://www.methodsandtools.com/archive/archive.php?id=98). The basic steps are as follows:



	
  1. Have a clear goal.

	
  2. Read existing code.

	
  3. Compose a plan to achieve the goal.

	
  4. Extract the module from current code, but not migrate any dependency.

	
  5. Fully test the new module.

	
  6. Migrate dependencies from old modules to the new module.

	
  7. Clear out the old modules. Refactor complete.


The list is pretty self-explanatory, but I want to stress a few points in it. First of all, the goal must be clearly stated before doing anything. It could be extracted methods into an independent interface with the same API, it could be merging a few separated classes together because functionally they do the same thing, etc. But it has to be stated clearly and loudly right at the head of your proposal. The reason behind this is that you must have a well-defined goal so that you have plans specifically to address that goal. Otherwise you could get into a place where you start to wonder: should I have done this in the first place? Maybe it’s a waste of time. Setting a valid goal to largely prevent that from happening.

Also, after the plan is all set and agreed upon, there could be things along that way that just denies it, with no way to bypass it. To give an example, I once wanted to switch a project from observer pattern to simple message sending paradigm, because I felt like all those interfaces really should not be there, and we could maintain the tests by switching from event capture to simple system output capture. The plan went through alright, but when I tried to migrate the tests, it turns out we do need to have the observer pattern there because the initiating event should be blind, instead sending specifically to a component. The best one can do, when getting into these sort of situation, is to calm down first, backtrack to the last viable place, and rewrite the plan so it would work. I have to say it feels like killing your own baby, and it really hurts. But it is necessary sometimes.

During the whole process, the most time-consuming and boring part is migrating dependencies. It almost feels like you can have someone who knows only copy and paste to do this, and you have to do it for an entire week, or even longer. This is where one of the little automation tools steps in and make your life better. The one I use is actually built in unix shell, which is called [sed](https://en.wikipedia.org/wiki/Sed).

Say you want to change, throughout the entire codebase, the text “Foo” to “Bar”(yes I am out of realistic examples), a simple way to do this is

`sed -i -e ’s/Foo/Bar/g’ CODE_PATH/`

What the script says is that you substitute all instances of “Foo” to “Bar”, in files under CODE_PATH. For engineer using Mac, there is a little [gotcha](http://stackoverflow.com/questions/19456518/invalid-command-code-despite-escaping-periods-using-sed/19457213#19457213), which is -i flag takes an extension and you cannot just put it there without an argument. So the script for Mac uses would be

`sed -i ‘’ -e ’s/Foo/Bar/g’ CODE_PATH/`

There’s a pretty comprehensive [tutorial](http://www.grymoire.com/Unix/Sed.html#uh-15) I refer to every now and then, when I want to do some cheeky business with regex or such. Also be aware that this is automation, not as smart as human, so there could be false positives. What I advice is try it out at a small directory first, then apply it globally.

In conclusion, I feel like this is the part of software engineering that we might want to avoid if at all possible, and that’s completely justified. No-one gets a medal for doing internal cleanup, no offense. But through careful planning, and with help of automated tools, we might just be able to get it smoothly, without too much head pounding on the wall.

From this point on will be some personal thoughts, you can skip it if you want. As mentioned before, comments and re-posts are totally welcome, and I am happy that someone could point out my mistakes, because I am usually sloppy, and there might just be better ways to do things. If you read till here, you have my absolute thanks. Hope you have a nice weekend, and I will see you all next week.

Nick Wu
