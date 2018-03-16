---
author: nickwu0715
comments: true
date: 2017-02-05 17:07:32+00:00
layout: post
link: https://nickwuedinburgh.wordpress.com/2017/02/05/java-nio-tutorial/
slug: java-nio-tutorial
title: Java NIO tutorial
wordpress_id: 244
tags:
- Technical
---

Before I start this blog, allow me to give a follow-up with the Caterpillars Count app: right now we are fully live on both iOS and Android!

OK so that's out of the way. This week I want to talk about Java NIO, from my recent distributed system class. Basically NIO is a web framework for Java, for lack of a better description. In this blog I am going to explain the underlying principles of NIO, and a simple demo client/server model



### Socket



To understand NIO, we must first understand [Socket](https://docs.oracle.com/javase/7/docs/api/java/net/Socket.html). Socket is the major component to make NIO work, so it is imperative to understand what it is.

Luckily it is really simple. Essentially you can picture a socket as a Unix pipe over the network. Say you have this simple Unix command: `sort my_file.txt | head -100`. What it's doing is taking the output of the first command and feed it to the next.

Socket is just the same. Say you have two computers over the network that somehow both know about this socket thing. One of them can write stuff in which gets read by the other, and vice versa.



### So what's NIO



One problem with socket is that the read and write could potentially block, because behind the scene there are system buffers which send and pull data over the network, and you cannot write if the system buffer is full, or read if it's empty. This is known as the [Producer/Consumer Problem](https://en.wikipedia.org/wiki/Producer%E2%80%93consumer_problem).

NIO is designed to solve this. The "N" stands for "non-blocking".

The way it solves this is using a [Selector](https://docs.oracle.com/javase/7/docs/api/java/nio/channels/Selector.html). How this works is after establishing a connection, the client registers an "interest operation", which can be read, write, or others.(actually this is not true. Simply asking to establish a connection is an interest too. But here let's just look at read and write)

The selector will periodically collect all the interest and see if any of them can be consumed. If so, consume it, otherwise just hold on to it a bit longer.

This doesn't seem to solve the blocking problem we mentioned before in the first glance, but let me explain. For example, in the old situation the server can be blocked waiting for input from one client, that it cannot send stuff out to another client. Now, it can register for a "read" for the first client, and "write" for the other one.



### DEMO: Hello World



This demo has a more [comprehensive version](http://rox-xmlrpc.sourceforge.net/niotut/), in case you want to explore more on NIO.

My code can be found [here](https://github.com/NickWu007/533-PA1).

Let's start with the server(simplified):

~~~java
public void run() {
        while (true) {
            try {
                // Wait for an event one of the registered channels
                this.selector.select();

                // Iterate over the set of keys for which events are available
                Iterator selectedKeys = this.selector.selectedKeys().iterator();
                while (selectedKeys.hasNext()) {
                    
                    SelectionKey key = (SelectionKey) selectedKeys.next();
                                        selectedKeys.remove();

                    if (!key.isValid()) {
                        continue;
                    }

                    // Check what event is available and deal with it
                    if (key.isAcceptable()) {
                        this.accept(key);
                    } else if (key.isReadable()) {
                        this.read(key);
                    } else if (key.isWritable()) {
                        this.write(key);
                    }
                }
            } catch (Exception e) {
                e.printStackTrace();
                System.exit(1);
            }
        }
    }
~~~

First of all, the whole thing should be wrapped in an infinite loop, since we want the server to persist(also the client).

The first thing we do is `this.selector.select();` This is the call that gathers all interest ops for each socket channel.

Then we just iterate through them and deal with each. Pretty straight forward. I am leaving out the parts about read and write, but you can see them on github.

Now let's look at the client:

~~~java
public void run() {
        while (true) {
            try {
                // Wait for an event one of the registered channels
                this.selector.select();

                // Iterate over the set of keys for which events are available
                Iterator selectedKeys = this.selector.selectedKeys().iterator();
                while (selectedKeys.hasNext()) {
                    SelectionKey key = (SelectionKey) selectedKeys.next();
                    selectedKeys.remove();

                    if (!key.isValid()) {
                        continue;
                    }

                    // Check what event is available and deal with it
                    if (key.isConnectable()) {
                        this.finishConnection(key);
                    } else if (key.isReadable()) {
                        this.read(key);
                    } else if (key.isWritable()) {
                        this.write(key);
                    }
                }
            } catch (Exception e) {
                e.printStackTrace();
                System.exit(1);
            }
        }
    }
~~~

It's basically the same semantics. Only the read and write methods are different. I recommend you look at the complete code to get a better sense of it.

If the demo above doesn't help, [this one](http://crunchify.com/java-nio-non-blocking-io-with-server-client-example-java-nio-bytebuffer-and-channels-selector-java-nio-vs-io/) may be helpful.



### Conclusion



NIO is pretty awesome to use, but there are many better stuff built from it: Java RMI and RPC. I am still learning about these two, and I will write about them in future posts.

Hope this is helpful. I will see you all next week.
