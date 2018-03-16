---
author: nickwu0715
comments: true
date: 2017-06-15 21:44:15+00:00
layout: post
link: https://nickwuedinburgh.wordpress.com/2017/06/15/dartlang-1-primer/
slug: dartlang-1-primer
title: 'Dartlang #1: Primer'
wordpress_id: 344
tags:
- dartlang
- Technical
---

So it's finally here, our [dartlang](https://www.dartlang.org/) series! I understand this is long overdue than I promised, but unfortunately I have start with even more bad news. For the current series we are only going to look at [AngularDart](https://webdev.dartlang.org/angular), a frontend framework for web applications, but dart is far more widely used, in [server](https://aqueduct.io/), in [mobile](https://flutter.io/), and so on. Ideally I would like to do separate series for each of this, but for starter let's just focus on webbed, which is what dart is originally made for.So it's finally here, our [dartlang](https://www.dartlang.org/) series! I understand this is long overdue than I promised, but unfortunately I have start with even more bad news. For the current series we are only going to look at [AngularDart](https://webdev.dartlang.org/angular), a frontend framework for web applications, but dart is far more widely used, in [server](https://aqueduct.io/), in [mobile](https://flutter.io/), and so on. Ideally I would like to do separate series for each of this, but for starter let's just focus on webbed, which is what dart is originally made for.

Now as a primer, in this post I would like to quickly tour through the angular dart framework, give a few things that I find important to grasp. In the end, I will talk about what app we are ultimately building, so all of us can think about how to actually build it.



### AngularDart: Overview



The first thing to know about AngularDart, if you are coming from a JS world, is that it is component based, aka Object Orientated. If you take a look at a simple dart file, it looks a lot like a Java class. (barring the metadata)
~~~dart
class HeroListComponent implements OnInit {
  List<Hero> heroes;
  Hero selectedHero;
  final HeroService _heroService;

  HeroListComponent(this._heroService);

  void ngOnInit() {
    heroes = _heroService.getHeroes();
  }

  void selectHero(Hero hero) {
    selectedHero = hero;
  }
}
~~~

From the official offsite, this is the overview of AngularDart
![overview2](https://nickwuedinburgh.files.wordpress.com/2017/06/overview2.png)
Let me try to explain this picture. The middle circle is the main part: template/HTML to define the layout and view, component/Dart to manipulate the layout as controller. For now we can ignore the injector, I will talk about it in a separate post.



####  Data binding



One of the greatest bit of Angular in general, is data binding. This comes in two directions. From component to template, we have **property binding**. This is usually seen as template using some property in the dart class for display. Let's see a quick example:
~~~dart
// template
<p>{{helloMessage}}</p>

// dart file
string get helloMessage => 'Hello world';
~~~
Here the template is using a property called `helloMessage` from the component.
Also this goes the other way, from template to component, we have **event binding**. As the name suggested, this is usually used as event handler:
~~~dart
// template
<button (click)="showMessage()">Click me</button>

// dart file
void showMessage() {
    print('button clicked');
}
~~~
The function `showMessage` is called when the button is pressed.



#### Directives



If you are new to Angular, this word may sound very unfamiliar. Think of directives as special tags or properties you import from other files. They can be built-in to the framework like [`NgIf`](https://webdev.dartlang.org/angular/guide/displaying-data#!#ngIf), or it can be directives that you created. We will be using both in our tutorial.



### What we will be building



The project that I am thinking of building is an app called Teamoji, which is demoed in [this video](https://www.youtube.com/watch?v=SobXoh4rb58&t=1576s). In the video the app is demoed for progressive web app, but for now our objective is to just build a working version of it.

![Screen Shot 2017-06-15 at 22.29.08.png](https://nickwuedinburgh.files.wordpress.com/2017/06/screen-shot-2017-06-15-at-22-29-08.png)



### What's coming



In the next post, I want to talk about an important concept heavily used in AngularDart: dependency injection. It won't be too much used in this project, but it was arguably the hardest thing for me when I was learning about AngularDart, and I think it deserves a separate post.
After that, we will get started planning about how to build Teamoji. Because I have not built this yet, we will be figuring it out together, and you are more than welcome to give advice and suggestions about how we should implement it.
Hope you will like this series, stick around for more content this weekend!
