---
author: nickwu0715
comments: true
date: 2017-03-26 22:12:04+00:00
layout: post
link: https://nickwuedinburgh.wordpress.com/2017/03/26/road-to-shell-script-master/
slug: road-to-shell-script-master
title: Road to shell script master
wordpress_id: 307
tags:
- Technical
---

Recently because of a lot of DevOps and data analysis work I have been working, and a good reminder from [_The Effective Engineer: How to Leverage Your Efforts In Software Engineering to Make a Disproportionate and Meaningful Impact_](https://www.amazon.com/gp/product/0996128107/ref=oh_aui_detailpage_o02_s00?ie=UTF8&psc=1), I decided to move some work into the good old command line, arguably the most powerful tool a developer has, but constantly overlooked. Amazingly, my life has been so much better after I started using more and more scripts instead of working my code around to fit the expected output format, and shortened my web dev cycle a lot. So today I want to dedicate a post to talk about shell scripts: why should you use it, what are the basic tools for you, and some nice one-liners that make your life so much easier.



### Why should you use the shell?





#### You already have it!



If you are using a Linux-based machine for development, whether it's macOS or Ubuntu, you will have a shell command line built in for you. Even Windows users can download an extremely light version of the shell and get started.



#### Shell scripts are powerful and flexible



Have to give the designers of shell script a sincere hat tip, they gave developers like us so much power. You can search, sort, do counting, and so much more, usually with just a few keystrokes. Moreover, if you dig in a little bit deeper, you will see how much you can do by piping simple commands, and writing snippets of scripts and customise for you own workflow. We will see some examples very soon



#### Quick Dev cycle



Shell scripts are quick to change, debug, and use or store. Let me give you a simple example. Say you have a large file of text, numbers or otherwise, and you wish to sort them. You could write a Java program to do it(no offence to Java dev, I am one of you too ;)),by using file IO to read in the file, sort it, and write it back. The code is 10 lines minimum. You will have to compile it, and debug it(face it, you are not writing it right the first try), and finally run it. However, there is a `sort` command built-in for you, with flags and output direction all at your disposal. If you sort in command line, one line is all you need, probably done in 10 seconds.



### Building blocks



Now that we have seen the power of shell, let's use it to our advantage. This is section I will try to introduce some core commands you might want to know, and in the next section we will try to combine them to make some actual use.



#### [sort](http://man7.org/linux/man-pages/man1/sort.1.html)



As we just saw, this is used to sort lines of a line. You can sort numbers and strings, and in the case of lines with columns, say a `.csv`, sorting it is still possible, with the help of `-k` flag.

Example: `sort -rn -k 2 my_data.csv` This sorts the file by its second column, numerically and descending.



#### [grep](http://man7.org/linux/man-pages/man1/egrep.1.html)



Grep is another great tool for you. You specify a pattern or regex, and it will crawl the text file and find everything that fits the pattern.

Example: `grep foo MyClass.java` This finds all mentions of "foo" in `MyClass.java`.



#### [uniq](http://man7.org/linux/man-pages/man1/uniq.1.html)



The name sort of gives it away. This helps to you to find the unique occurrences.

Example: `uniq my_clothes_brand.txt` This gets rid of the duplicate brands of clothes I have.



#### [wc](http://man7.org/linux/man-pages/man1/wc.1.html)



Short for "word count", not the restroom. You can use this to count words in a file, and many more.

Example: `uniq -l LongAssignment.java` This gets the line count for the file, which in this case give me some consolation after 10 hours of coding.



#### [awk](http://man7.org/linux/man-pages/man1/awk.1p.html)



This one is tricky. It's basically your for-each alternative in shell. I won't give a specific example here, but we will see it in action soon.



#### [sed](http://man7.org/linux/man-pages/man1/sed.1.html)



This is your "find and replace". You can use regex here too.

Example: `sed -i -e ’s/Foo/Bar/g’ CODE_PATH/` Refer to this [old post](https://nickwuedinburgh.wordpress.com/2016/07/23/refactoring-startegy-and-to0ls/) of mine.



#### [find](http://man7.org/linux/man-pages/man1/find.1.html)



Search for files in a directory hierarchy

Example: `find /tmp -name core -type f -print` This prints out the files with name `core`.

#### [xargs](http://man7.org/linux/man-pages/man1/xargs.1.html)
This is a piping tool, but unlike standard piping, the stuff read is split into chunks by a deliminator, space by default

Example: `find /tmp -name core -type f -print | xargs /bin/rm -f` deletes files from the last example.



### Useful shell one-liners



These are a few examples given in this [great post](https://www.quora.com/What-are-the-most-useful-Swiss-army-knife-one-liners-on-Unix). I strongly recommend checking it out.



### 1: intersection\union\difference



A lot of the times you will be dealing with files of the same formate, probably generated logs from many cluster machines. Sometimes it's helpful to see the common node used, or the differences, both for debugging and performance tuning. Here are some awesome scripts to do that:

~~~bash
cat a b | sort | uniq > c # c is a union b
cat a b | sort | uniq -d > c # c is a intersect b
cat a b b | sort | uniq -u > c # c is set difference a - b
~~~



### 2: usage analysis



Sometimes you may want to see where a particular function is used in your codebase. Here's something that will help.

~~~bash
find . -name \*.py | xargs grep some_function
~~~



### 3: kill certain processes



This is more of a cleanup or emergency tool. Kill all processes whose names match a certain pattern:

~~~bash
ps aux | grep mypattern | awk '{ print $2}' | xargs kill
~~~



### My own shell tools



There are a lot you can learn in the that post I mentioned earlier, but here are some stuff I use constantly, that hopefully will help you too



### 1: top k in a file



Recently I have been working with a long network log, which has IPs, and bytes of data sent or received in each transfer. After getting a aggregated data from the log, I want to see the top 10 IPs for sending data. A script to do that would be:

~~~bash
sort -nr -k 2 file.txt | head -10
~~~

`-k 2` because the first column is the IPs, and the second one are the bytes sent in total.



### 2: Intersection on one column



This is related to the intersection example I gave earlier, but I want to get it based on just one column, not the whole line. This is one of the ways to do it.

~~~bash
awk 'NR == FNR {val[$1]=$2; next} $1 in val {print $1, val[$1], $2}' file1 file2
~~~

Again I won't explain too much about the awk bit. You can get some great post through good old Google.



### Conclusion



I hope I have convinced you to take a second look at shell scripting, and using it to improve your productivity. There is a [post](https://dev.to/thiht/shell-scripts-matter) I read earlier that talked about more in-depth stuff, and you can find tutorials and books about the topic online easily. The bottom line is, **use shell whenever you can**.

Have a nice week, and I will see you all soon.
