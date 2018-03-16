---
author: nickwu0715
comments: true
date: 2017-06-17 20:07:44+00:00
layout: post
link: https://nickwuedinburgh.wordpress.com/2017/06/17/dartlang-2-dependency-injection/
slug: dartlang-2-dependency-injection
title: 'Dartlang #2: Dependency Injection'
wordpress_id: 366
tags:
- dartlang
- Technical
---

Welcome back! The first post turned out to be the most viewed post on this site, I am so grateful and motivated to keep this going. So here we go, dependency injection!

![what-is-dependency-injection](https://nickwuedinburgh.files.wordpress.com/2017/06/what-is-dependency-injection.jpg)



### What is it?



In first glance, the phase sounds fancy and sort of hard to grasp, so I have laid out a route to make this easier.



#### What's a dependency?



This part should be relatively easy to grasp. Say you are a carpenter, and you need a chisel to make a sculpture, then it can be said that you have a "dependency" on the chisel.
Moving back to a web programming context. A simple example would be a front end event handler needs some database object to fetch user data, then the handler has a dependency on the database object. So far so good?



#### What's injection?



Now that we have the notion of dependency clear, let's think about how we usually establish them. The easiest way is to have it as a property and instantiate a new object in constructor, such as:
~~~dart
class Carpenter {

    // Dependent object
    private Chisel chisel;
    
    public Carpenter() {
        chisel = new Chisel();
    }

    void makeSculpture() {
    
        ...
        
        // Use the dependency
        this.chisel.apply();
    
    }
    
}
~~~


Now this code has no dependency injection, because the dependency is actually established by the object that holds the dependency. What if the dependency object, aka the chisel, is given to you somehow when you are constructing the `Carpenter` object? In other words, what if your constructor looks like this:
~~~dart
public Carpenter(Chisel chisel) {
    this.chisel = chisel;
}
~~~

This way the dependency object is supplied by some other party, and we can say those party **injects** the dependency into `Carpenter`. This is essentially what dependency injection is.



### Why bother?



Let's take a look back at what we did. We changed `chisel = new Chisel();` in the constructor into a parameter, and we would need some third party to inject this when we instantiate a new `Carpenter`. Sounds kind of overly complex right? Well it turns out there are a few good reasons why.



#### Flexibility



There's one important principle what drives the creation of dependency injection, called the [Dependency Inversion Principle](https://en.wikipedia.org/wiki/Dependency_inversion_principle). What it states basically is you want to depend on abstractions, not specific classes. Imagine the `Chisel` in the example above is an interface, then the injecting party can supply any kind of chisel to a carpenter, each for different job, without changing any code in `Carpenter`.
This is sometimes referred to as "loose coupling", and it gives `Carpenter` flexibility in its behaviors, without needing to modify the class itself.



#### Modularity



Since we can extract implementation from interface, we are inherently given more modularity. This is an obvious point, but nevertheless important in software engineering.



#### Testability



Dependency injection can also help testing. You can basically inject different implementations to the same object, and test its behavior. No need to write tests like `testWoodChiselWorks()` and `testStoneChiselWorks()`, which will have 80% code exactly the same. Simply compose a map of implementations to their expected  behaviors and loop it though in one fixed test framework. Ezpz.



### DI in action: AngularDart



So far we know what dependency injection is, and why it is a good pattern to use, but there are still two problems: a) how to use them, in AngularDart specifically, and b) what about this third party object that's responsible for all the injecting? In this section we will see how DI is used usually, and why injecting thing is not a worry.



#### Standard DI model



Let's take a look at an example from the [official site](https://webdev.dartlang.org/angular/guide/dependency-injection#!#angular-dependency-injection):

~~~dart
// Dependency object
import 'package:angular2/angular2.dart';

import 'hero.dart';
import 'mock_heroes.dart';

@Injectable()
class HeroService {
  List<Hero> getHeroes() => HEROES;
}
~~~

Notice a special annotation before the class declaration: `@Injectable()`. This marks the class as something to be injected with. In terms of how exactly it is injected, we will talk about it in a bit.
~~~dart


// Dependent object
import 'package:angular2/angular2.dart';
import 'hero.dart';
import 'hero_service.dart';
@Component(
  selector: 'hero-list',
  template: '<div>{{hero.id}} - {{hero.name}}</div>
'

,
  directives: const [CORE_DIRECTIVES],
  providers: const [HeroService],
)
class HeroListComponent {
  final List<Hero> heroes;
  HeroListComponent(HeroService heroService) : heroes = heroService.getHeroes();
}

~~~

We see clearly the `HeroService` is to be injected as a parameter in the constructor, exactly the same in our Carpenter example.
Ok so let's address the problem: who does the injecting? The answer is nobody, in a sense. As a programmer, you won't need to write a single line of code to do the injection, it is done for you by dart automatically.
You may say "that's very cool for dart, but how does it know what to inject into what?" This is where the `@Injectable()` comes to play. If a class has this annotation, it tells the dart VM that "I am an injectable thing, feel free to plug me in if someone needs me." There's a question about the injectable itself too, how is it instantiated? Normal `new` method, factory, or singleton? Turns out you can choose either one. For the sake of understanding I will not go into too much detail about this, but you can read about it online easily.
Ok so we settled the first bit, now where to inject into? There are two signs a class can tell the VM to inject something into itself. One is by listing it in the constructor, and the other is in the metadata. Note the line that says `providers: const [HeroService],`. This is saying "I need this HeroSerive thing injected for me" loud and clear. This the VM knows to inject it into the component, and in the component you just use it normally. Magical.



### Conclusion



So that is dependency injection. As I mentioned earlier we won't be using this too much in our project, since we don't really need a backend and we can just fetch directly from the database.
In case this post doesn't do it job, here are some more resources to help:




    
  * [DI tour in darling official site](https://webdev.dartlang.org/angular/guide/dependency-injection#!#angular-dependency-injection)

    
  * [Wikipedia](https://en.wikipedia.org/wiki/Dependency_injection)

    
  * [a similar blog post about DI concept](http://brandonclapp.com/what-is-dependency-injection-and-why-is-it-useful/)

    
  * [a StackOverflow post](https://stackoverflow.com/questions/130794/what-is-dependency-injection)

    
  * [a nice YouTube video that gives visual aid](https://www.youtube.com/watch?v=IKD2-MAkXyQ)
Next week we will actually start our project. First thing will be requirement capture and UI mock up. See you next week!


