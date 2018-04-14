---
author: nickwu0715
comments: true
date: 2016-08-27 18:17:09+00:00
layout: post
link: https://nickwuedinburgh.wordpress.com/2016/08/27/async-and-future/
slug: async-and-future
title: Async and Future
wordpress_id: 98
categories: [technical]
---

When developing large-scale software system, which handles thousands, if not millions of requests all the time, performance is one of the most important spec to think about, other than functionalities. That's why one would buy lots of servers with powerful CPUs to support for the software. The idea behind all this is when a request comes in, a special program assigns it to an idle, or least loaded server to process it. Many servers process lots of requests
simultaneously, this is called [Concurrency](https://en.wikipedia.org/wiki/Concurrency_(computer_science)).

However, from the perspective of developers, most of this is hidden under some sort of library or packages. Today I want to life up the curtain, so to speak, and give you a more specific understanding about this, and furthermore, introduce you a nice library I use constantly.



### Synchronous and Asynchronous calls



First of all, let's consider a simple piece of code like this.

~~~
void main() {
String tmrForcast = HttpRequest.getTomorrowWeather();
print(tmrForcast);
}
~~~

Of course HttpRequest doesn't have a method to display tomorrow's weather report, but let's assume there is. It is not really relevant here.

This snippet of code is called synchronous. It will execute line by line sequentially. Although there is a potential problem here. When making the http request to get the weather for tomorrow, the program has to wait till the response is available, and then continues execution. This wastes time, especially in large-scale software.

The way to work around this is asynchronous calls. A more down-to-earth way to think about it is when you put laundry into the washing machine, you won't just sit there and wait for it to finish. You would do and cross off more things on your checklist, such as make some salad for tomorrow. The same principle is used in async calls. Other than sitting and wait, your program will continue to execute other commands, and when the response comes back, it would pick it up and finish that as well. The following diagram might help you understand it better, credited from [IBM](https://www.ibm.com/support/knowledgecenter/SS7J6S_7.5.1/com.ibm.websphere.wesb.programming.doc/topics/esbprog_sinvocationstyles3.html).

![async_1](https://nickwuedinburgh.files.wordpress.com/2016/08/async_1.gif)

But you might ask: "what if the thing you are waiting for is used later? You just cannot execute those commands." First of all, you are absolutely right. Because there hasn't been a defined value, there is no way to use it. You can't try to fold your laundry while it is still in the washing machine. What happens here is that the program will try to execute later commands, but if some of the resources is not available, it would put that later command on hold as well, and pick it off after all the required resources to execute is available. Of course all of this is specific implementation detail hidden from developer, but you shouldn't have any concern of using variable that could be put on hold and worry your program crashes.

While we are on the subject, it is possible that what you put on hold never returns a value. For example, you might have dropped network in the middle of a http call, so there is never a response. There is error handling for this sort of situation, but it is a bit off topic for this post. Check out [this bit](https://www.dartlang.org/articles/libraries/futures-and-error-handling) for more information.



### async and Future: Dart's tool



Not to your surprise, I am going to talk about how to do async in Dart. If you are new to the blog and you don't know what dart is, check out its [official site](https://www.dartlang.org/). So how does this work in Dart?

First of all, we need a way to denote a variable that gets return from async calls, in other words its value is not instantly defined, but you can still use it in later part of the code. Dart has a nice class Called [`Future`](https://www.dartlang.org/tutorials/language/futures). This is pretty self-explanatory, since the value is going to be obtained in the "Future". Also there is `async` and `await` modifier. To have a first impression of them, let's look at the revised version of our weather report program earlier.

[code lang=text]
void main() async {
Future<String> tmrForcast = HttpRequest.getTomorrowWeather();
print(await tmrForcast);
}
[/code]

First `async` is added to the function head, meaning there will be asynchronous calls happening. Then instead of getting the value of tomorrow's forecast, we get a `Future` version of it. In the end, the print statement will `await` for the value to be available and then execute. Pretty simple to understand, but there is a lot more about this topic, such as [Stream](https://www.dartlang.org/tutorials/language/streams) for event passing, [Callback functions](https://www.dartlang.org/tutorials/language/futures), and so on. I won't go into detail about each of them, but they are equally important during development.

In the end, this [YouTube video](https://www.youtube.com/watch?v=LxAfwwgiQq4) may help you understand asynchronous calls better, just in case this post doesn't help you much. If you have any suggestions or thoughts, please leave them in the comment section.

See you all next week, have a great weekend!
