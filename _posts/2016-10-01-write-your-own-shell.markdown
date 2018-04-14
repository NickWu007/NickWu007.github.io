---
author: nickwu0715
comments: true
date: 2016-10-01 02:33:34+00:00
layout: post
link: https://nickwuedinburgh.wordpress.com/2016/09/30/write-your-own-shell/
slug: write-your-own-shell
title: Write your own shell
wordpress_id: 130
categories: [technical]
---

## Write your own shell

### My story



If you have been on the site often enough, you will know most of my programming is done on some high level abstraction language, or based on some frameworks. In the last few weeks, one of my university project is going way down on the abstraction spectrum, and try to build a shell in C. It took me a fair amount of banging on the wall and desperation, but since I did made something in the end, I want to write a post and talk about how to write your own shell.



### "Why should I care?"



Some of you may say: I am not a system programmer, then why should I read any of this. For starter, if you are working in a software company, chances are you are using a unix OS, like Ubuntu. In a unix OS, the shell is arguably one of the most important tool at your disposal. Understanding the magic behind the shell would help you a lot when you want to debug some shell script, and during the process, you might realise that you can boost your productivity tremendously just by changing the way you use your shell.

So without further ado, let's get into it!



### Basic components



Think about the bash shell you sue all the time. What does it consists of?





  * An line waiting for input


  * Some logic to parse and execute the input and possibly produce some output


  * back to waiting input



So it is pretty clear what the basic components are:



  * A parser for input command


  * A executor for the command


  * A outer loop to keep getting inputs



In the end, we should be able to put up something like this.

~~~c
int main() {
while (1) {

char *cmd_line = readLine(); // reading the line
char **cmd = parseInput(cmd_line); // parsing
execute(cmd); // execution

}
}
~~~

Let's look at each part individually.



### Reading the line



In C, the easiest way to do this is just [`getline`](http://man7.org/linux/man-pages/man3/getdelim.3.html). However, this is deemed unsafe for some complier, and you might want to react to some specific character input. For example, reading the upper arrow char and pull up the last command typed.(I didn't implement this bit, but feel free to try it out for yourself) In all, reading the line seems pretty trivial, so we won't spend too much time on this.



### Parser



Personally, I feel like this is the most disturbing part of shell implementation. The goal is break the input line by spaces, and put the pieces in an array, terminated by a `NULL`. This is to make the execution easier, which we will get into in a bit.

So how might you do this? There is no library method like `split(' ')` like in Java. Luckily, there e is still something that can help: [`strtok`](https://www.tutorialspoint.com/c_standard_library/c_function_strtok.htm). Basically, it takes a string and a delimiter, and splits the string into a bunch of tokens. What you need to be aware of is that the API isn't as concise as in Java, and you will need to put a while loop for this to work(like the one in the link I put above). But with this, we should be able to at least split the command line into command tokens:

~~~c
char ** parseLine(char *line, char *demin, int default_length) {
char **tokens = malloc(sizeof(char *) * default_length);
int tokenCount = 0;
char * token;

token = strtok(line, demin);
while (token != NULL) {
*(tokens + tokenCount) = token;
tokenCount++;
token = strtok(NULL, demin);
}

*(tokens + tokenCount) = NULL;
return tokens;
}
~~~

Since we are only implementing the simplest bit of shell, this would suffice. But in a shell like bash, you need special parsing for I/O redirection, echo command, and so on. Some of these are implemented in my code for you o have a look.



### Executor



So finally we set up everything, now we just need to execute it. However, there is a logic to exec the command that might you need think about for a bit to understand, so I want to talk about it first: fork/exec model.

The first thing you need to realise is that we cannot 'just exec' the command. Because if we do, when the command is executed, the OS will close this process, which means your shell can only execute one command, and then it exits. The way to solve this `fork()`, which makes a new process almost identical to the current process(apart from a different process id). This newly created process is called the 'child', and original process is called 'parent'. Then we will `exec()` the command on the child process, and have the parent process `wait()` for it to finish.

So we have this new 'child' process to execute, but we still don't know how to execute it. Luckily the C library has a collection of functions for it, each with different parameters. For simplicity I recommend [`execvp()`](https://linux.die.net/man/3/execvp), which will search through your `PATH` and find the binary for you.

To give an example for using this, if your type in `ls -l`, the parser will produces a command token array like:

`char *args[] = ['ls', '-l', NULL];`

Then to execute this command, in your child process you will say

`execvp(args[0], args);`

If this executes and terminates, the parent will get the exit status, and proceed to finish the whole lifecycle for this command line.

Putting together the fork/exec model:

~~~c
pid_t parent = getpid();
pid_t pid = fork();

if (pid == -1)
{
// error, failed to fork()
}
else if (pid > 0) // Parent process
{
int status;
waitpid(pid, &status, 0);
}
else // Child process
{
// we are the child
execvp(...);
_exit(EXIT_FAILURE); // exec never returns
}
~~~

Again you will see the full code later, I won't paste the entire section here.



### Conclusion



So by this point, hopefully you understand what a simple command line goes through to get executed. Again, I am only scratching the surfaces here. In reality you need to implement a lot more than this. But it is quite fun to go into low-level C code and play with the OS personally, and I hope you can try it out some time.



### References



My implementation of code can be found on my [GitHub](https://github.com/NickWu007/thsh). If you are doing a similar project and come across this post, I recommend you only read the post and not look at the actual implementation. First of all, what I write might not be fully correct, and you should always write your own code.

Also a shoutout for Stephen Brennan's [post](https://brennan.io/2015/01/16/write-a-shell-in-c/). I learnt a lot from this post when I was working on the project, and probably he gives a better explanation about the exec model. So definitely check it out.

Hope you have a great weekend, and I will see you next week.

Nick Wu
