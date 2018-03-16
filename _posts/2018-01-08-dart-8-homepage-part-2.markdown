---
author: nickwu0715
comments: true
date: 2018-01-08 03:32:35+00:00
layout: post
link: https://nickwuedinburgh.wordpress.com/2018/01/07/dart-8-homepage-part-2/
slug: dart-8-homepage-part-2
title: 'Dart #8: homepage part 2'
wordpress_id: 529
image: /assets/images/dart_angular.png
tags:
- dartlang
- Technical
---

Today we continue with the homepage for Teamoji. In the last tutorial we finished the main content of Homepage. But we are still missing the hidden menu part on the left. This introduces a new concept of Angular Dart: deferred content. So without further ado, let's jump in!

![Homepage_drawer.png](https://nickwuedinburgh.files.wordpress.com/2018/01/homepage_drawer.png)
*Mock*



### What's Deferred Content?



Before we dive in code it is important to understand the modal component and deferred content. Deferred content roots from the _Lazy Evaluation Principle_:

"Lazy Evaluation, or call-by-need is an evaluation strategy which delays the evaluation of an expression until its value is needed."

You might think: how is lazy a good thing? Consider the following scenario: the user logs into the homepage, which displays the recent chat history of a particular team. The user is very active in this team, and rarely switches to other teams.

Without lazy evaluation, we will compile and load all components when user accesses the page. However the user never expands the left menu, making the load time for that component virtually useless. On the other hand, with lazy evaluation, we choose not to compile and load the hidden components, and only load them when user is asking for them. This way we speed up the initial page load, which is crucial to acquisition and retention.

![lazyness](https://nickwuedinburgh.files.wordpress.com/2018/01/lazyness.png)



### The menu component



We can see the bits of the menu component quite clearly from the mock:





  * A header that says "Your teams".


  * A list of the teams the user belongs to, highlight the one currently on display. (_ngFor_)


  * A button to create new Team. (_Material-button_)


  * A sign out button. (_Material-button_)



At this point you should be fairly comfortable with all these techniques. The only thing to watch for in the [Application Layout](https://dart-lang.github.io/angular_components_example/#AppLayout) format.

~~~html
<material-drawer temporary #drawer="drawer"
[attr.overlay]="true">
<div class="tm-home-drawer">
<div class="tm-home-drawer-header-row">
{{drawerHeaderMessage}}
</div>
<material-list class="tm-home-drawer-list">
<material-list-item *ngFor="let team of teams"
class="tm-team-list-item"
[class.deep-blue]="shouldShowAsDeepBlue(team)"
(trigger)="onChangeTeam(team)">
{{team}}
</material-list-item>
</material-list>
<div class="tm-home-drawer-btns">

{{createTeamButtonMessage}}


{{signOutButtonMessage}}

</div>
</div>
</material-drawer>
~~~
There are a few points to note. First notice we name the `material-drawer` as `drawer`. This is so that we can toggle it by clicking on the menu button on the homepage:

~~~html
<material-button class="material-drawer-button" icon (trigger)="drawer.toggle()">
<material-icon icon="menu"></material-icon>
</material-button>
~~~
Also notice the `*deferredContent` marker in the fist `div`. The `*` has the same meaning as in `*ngFor`: this is a structural directive, which alters the DOM tree. Adding this marker tells the dart engine: don't load me unless needed. The rest is fairly standard stuff.

After adding this into the homepage, we can see something like this:

![Screen Shot 2018-01-07 at 22.29.41](https://nickwuedinburgh.files.wordpress.com/2018/01/screen-shot-2018-01-07-at-22-29-41.png)



### Conclusion



At this point we have technically finished all mocks given in the first place. But as you can probably see, this app is still broken. There are some functionalities missing, such as a button to join a team. Also there's no navigation between the pages. In the next post we will address these problems, and get started with firebase!

As usual if you get stuck somewhere, always feel free to checkout the [repo](https://github.com/NickWu007/Teamoji-practice), or PM me the specific issue. Happy coding!
