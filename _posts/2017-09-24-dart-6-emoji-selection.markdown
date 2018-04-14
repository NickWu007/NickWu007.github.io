---
author: nickwu0715
comments: true
date: 2017-09-24 15:40:36+00:00
layout: post
link: https://nickwuedinburgh.wordpress.com/2017/09/24/dart-6-emoji-selection/
slug: dart-6-emoji-selection
title: 'Dart #6: Emoji selection'
wordpress_id: 482
categories: [dart, technical]

---

After the create team page, this week we are going to be building the emoji selection page. As usual the code I present here is somewhat partial, so let me know if anything is unclear.

![emoji_selection](https://nickwuedinburgh.files.wordpress.com/2017/09/emoji_selection.png)
*Our mock*



### How to build it?



As mentioned in last week's post, we are going to leverage `EmojiRenderComponent` to build this component, and it is not hard to see why. Every little emoji to select is an `EmojiRenderComponent`. If we have a list of emoji unicodes, then a simple `material-list` should do.

So the question becomes: where do we find all the emoji unicodes? Answer is, and usually will be, our good friend Google. A simple search would turn up some csv file, and with a little python formatting, we are good to go.

One thing worth pointing out though is current I am putting the entire list as a private variable in the component, and ideally it should be fetched from some service. We will address this when we get to _Firebase_.

Now let's take a look at the code.



### emoji_selector.dart



~~~dart
import 'package:Teamoji_tutorial/src/common/messages.dart';
import 'package:Teamoji_tutorial/src/emoji_render/emoji_render.dart';
import 'package:angular/angular.dart';
import 'package:angular_components/angular_components.dart';

@Component(
selector: 'emoji-selector',
templateUrl: 'emoji_selector.html',
directives: const [
EmojiRenderComponent,
MaterialIconComponent,
MaterialButtonComponent,
NgFor,
],
styleUrls: const ['emoji_selector.css'],
)
class EmojiSelectorComponent extends EmojiSelectorMessages with EmojiList {

void onCancel() => _dismiss();

void onSelect(String emoji) {
print('You want to post $emoji');
_dismiss();
}

void _dismiss() => print('You want to cancel selecting an emoji');
}

~~~

This is pretty straightforward. I am printing out the actions instead of actually implementing them. This simplifies the component for now, and we can fill these up when we put everything together.



### emoji_selector.html



~~~html
<div class="tm-emoji-select-header">
<div class="tm-emoji-select-header-message">{{promptSelectMessage}}</div>
<material-button class="tm-emoji-select-header-button" icon (trigger)="onCancel()">
<material-icon icon="clear"></material-icon>
</material-button>
</div>
<div class="tm-emoji-select-content">
<ul class="tm-emoji-select-list">
<li class="tm-emoji-select-item">

</li>
</ul>
</div>
~~~
Note we use a combination of `material-button` and `material-icon` to realize the cancel button. There might be cleaner ways to implement this, but as far as I know this is the conventional way.

Also interestingly there's an `*` before the `ngFor`, that's because `ngFor` is a [_Structural Directive_](https://webdev.dartlang.org/angular/guide/structural-directives). This asterisk is saying "I will potentially alter the DOM tree, so parse me a bit differently". In our case, because we are enumerating a list, whose length is unbeknownst to the template, it can potentially grow to many `li`s, thus altering the DOM tree.

I won't show the CSS here, it is in the repo if you to take a look. Also I am pretty sure I did a lot of hacky stuff there, so you might want to implement the layout and styling by you own.

![Screen Shot 2017-09-24 at 16.36.07](https://nickwuedinburgh.files.wordpress.com/2017/09/screen-shot-2017-09-24-at-16-36-07.png)
*Final render. Also the selection "works" too!*

Once we have this component done, the only component left is the homepage component. Unfortunately this is the most complicated component to implement, so we will split into 2 or 3 posts for that.

Let me know if anything can be improved. Thanks for reading, and I will see you all next week.
