---
layout: post
title: 'Teamoji wrap-up: connecting Firebase database'
date: 2018-03-29 11:37 +0100
image: /assets/images/dart_angular.png
categories: [dart, technical]
---

Today we finish up with Teamoji! In the last post we got the authentication and session checking done. In this post we will start from [this branch](https://github.com/NickWu007/Teamoji-practice/tree/firebase_auth_done) on the Github repo. So without further ado, let's jump in!

Fair warning: this is a fairly long one. So get some coffee first!

### Firebase real-time database
Firebase offers a [real-time NoSQL database](https://firebase.google.com/docs/database/). If you have only worked with relational database, like me, NoSQL takes sometime to get used to. But here I will attempt to give you a high level overview of NoSQL, and Firebase database, since it is slightly different from traditional NoSQL database.

Contrary to relational database, where data is stored in tables with rows and columns, NoSQL database stores data as "a collection of documents". In other words, you can think of your entire database as a huge JSON tree, or a dictionary. Each node is a `key, value` pair, where the `value` can be another dictionary.

In Firebase, you can retrieve a particular piece of data by "referencing" it. This sure sounds simple enough, but since Firebase is real-time, you cannot directly read data off from reference. Instead, you register callbacks to specific events, which gives you a `DatabaseSnapshot` object, from which you can then read the data.

Let's look at an example.

```javascript
var starCountRef = firebase.database().ref('posts/' + postId + '/starCount');
starCountRef.on('value', function(snapshot) {
  updateStarCount(postElement, snapshot.val());
});
```

Here a `starCountRef` is created. However, the reference itself doesn't do anything in particular. To read the data, you must register a callback on the `value` event, which passes a `snapshot` which the data contained.

There are also other events you can register callbacks to. In Teamoji you will see them getting used as well. Now let's jump in with Teamoji!

### Structure of data
Before we write any code, the structure of the database needs to be nailed down first. First of all, there are three major data models in Teamoji:

 * User
 * Team
 * Message

Obviously we can each of them in a separate reference, and add more references to link them. That would be the relation way. We need another representation of the data. 

First of all, Firebase authentication module takes care of the `User` object, and exposes a unique `uid` for each user. Since we don't plan to support user-to-user functionalities in Teamoji, a `uid` is essentially all we need. Overall, `User` needn't to be in the database.

Each user will have a few teams they are affiliated with. In Teamoji we use a `user_teams` reference to represent this. The key would be the `uid` mentioned before, and the value would the a list of team names that user is in.

Now the messages. Conceptually each message should be linked with a team. We use the same approach we did with user and teams, and use a `messages` reference to represent all the messages. Within it, each key is the name of the team, and the value is a list of the messages in that team.

It is very important to settle down on a structure of the database, since our code is going to be based on it. What I used in Teamoji is quite crude and might not be optimal. If you have any suggestion, feel free to bring it up in the comment section.

### Linking with Firebase DB
First let's tackle the teams. When the user logs in, they teams would be built into a list, and we also need something to represent the currently displaying team. Right now all of this is mocked in `homepage.dart`, but ideally they should be in the service module, since the component itself should only display content and forward the user interactions. So first thing we should do is to take out all the teams-related variables, and delegate the functionality to the service.

~~~dart
// firebase_service.dart
class FirebaseService {

  //...

  fb.Database fbDatabase;
  Map<String,List<Message>> previousEmojiMap = {};
  List<String> teams = [];
  String currentTeam = '';

  List<Message> get previousEmojis => [];

  FirebaseService() {
    secret.init();

    _fbGoogleAuthProvider = new fb.GoogleAuthProvider();
    fbAuth = fb.auth();
    fbDatabase = fb.database();
  }

  void buildTeams() {
    fbDatabase.ref('users_teams/' + fbAuth.currentUser.uid).onValue.listen((e) {
      teams = [];
      Map rawTeams = e.snapshot.val();
      if (rawTeams == null) return;
      rawTeams.forEach((k, v) => teams.add(v));
      changeTeam(teams[0]);
    });
  }

  void changeTeam(String team) {
    if (currentTeam == team) return;
    currentTeam = team;
    switchTeam();
  }

  void switchTeam() {
    // return if there is already a lister.
    if (previousEmojiMap.containsKey(currentTeam)) return;

    previousEmojiMap[currentTeam] = [];
    // Register listener
    fbDatabase
        .ref('messages/' + currentTeam)
        .onChildAdded
        .listen((e) => _buildPrevEmoji(e, currentTeam));
  }

  _buildPrevEmoji(e, team) {
    Map rawMessages = e.snapshot.val();
    previousEmojiMap[team].insert(0, new Message.fromJson(rawMessages));
  }

  Future postNewMessage(Message message) async {
    if (message != null) {
      await fbDatabase
          .ref('messages/' + currentTeam)
          .push(Message.toMap(message))
          .future;
    }
  }

  Future createTeam(String teamName) async {
    await fbDatabase
        .ref('users_teams/' + fbAuth.currentUser.uid)
        .push(teamName).future;
  }

  //...

}
~~~

First of all, a `Database` object is created during initialization. Then the relevant variables are added. Note that `previousEmojiMap` is especially important, and I will explain this later.

First let's take a look at `buildTeams()`. As mentioned before, we reference `users_teams/$uid` in the database, and register a callback on value. In it the value of the snapshot is iterated, and each team name is added into the list. Finally, it calls `changeTeam(teams[0])` to point the current team to the first one of the list.

Following up let's look at `changeTeam()`. This function is fairly simple, in that it just updates `currentTeam` if necessary and calls the  `switchTeam` function.

The `switchTeam` is where I got things wrong the first time. Here it checks if a callback that builds the messages has been registered for this team. If so, nothing should be done. Otherwise a new callback would be registered.

This is necessary, since Teamoji is a Single Page Application. If this check is not done, multiple callbacks would be called when a new child is added into the messages list, and therefore showing multiple times in the list.

Lastly, the `createTeam()` function is fairly straightforward as well. It simply pushes a new entry in the `users_teams/$uid` list.

### Default group
There are one more thing to take care of. When the user first logs in, they would not have any teams yet. If we use the current structure, the first thing the user sees will be a blank screen, which is quite confusing and bad UX. I added an extra check upon login that if this is a new user, assign them to the "general" group. This way users would be able to see at least some content when they first log in.

~~~diff
//firebase_service.dar

Future signIn() async {
     try {
       await fbAuth.signInWithPopup(_fbGoogleAuthProvider);
+      if (fbAuth.currentUser != null) {
+        fbDatabase
+            .ref('users_teams/' + fbAuth.currentUser.uid)
+            .once('value')
+            .then((event) async {
+          if (event.snapshot.val() == null) {
+            await fbDatabase
+                .ref('users_teams/' + fbAuth.currentUser.uid)
+                .push('general').future;
+          }
+          ;
+        });
+      }
     } catch (error) {
       print("$runtimeType::login() -- $error");
     }
   }
~~~

### Wiring up
Now that services functionalities are ready, we can simply wiring it up to our components.

~~~diff
// changes of homepage.dart


@@ -34,43 +34,28 @@ import 'package:angular_components/angular_components.dart';
       'homepage.css',
     ])
 class HomepageComponent extends HomepageMessages implements OnInit {
   bool visible = false;
   String currentComponent = 'homepage';
 
-  List<Message> previousEmojis = [
-    new Message('Nick', 'images/profile_placeholder.png', '\u{1F60B}',
-        new DateTime.now()),
-    new Message('Nick', 'images/profile_placeholder.png', '\u{1F60B}',
-        new DateTime.now()),
-    new Message('Nick', 'images/profile_placeholder.png', '\u{1F60B}',
-        new DateTime.now()),
-    new Message('Nick', 'images/profile_placeholder.png', '\u{1F60B}',
-        new DateTime.now()),
-    new Message('Nick', 'images/profile_placeholder.png', '\u{1F60B}',
-        new DateTime.now()),
-  ];
-
-  List<String> teams = ['google', 'angular', 'firebase'];
-
   final StreamController<String> stream = new StreamController.broadcast();

   @Output()
   Stream get onPageChange => stream.stream;
 
-  bool shouldShowAsDeepBlue(String team) => false;
+  bool shouldShowAsDeepBlue(String team) => team == service.currentTeam;
 
   Future onSelectEmoji(Message message) async {
     currentComponent = 'homepage';
-    // TODO: use firebase database to push new message.
+    await service.postNewMessage(message);
   }
 
   Future onCreateTeam(String teamName) async {
     currentComponent = 'homepage';
     if (teamName == null) return;
-    // TODO: use firebase database to create new page.
+    await service.createTeam(teamName);
   }
 
   Future onSignOut() async {
@@ -80,6 +65,6 @@ class HomepageComponent extends HomepageMessages implements OnInit {
 
   @override
   ngOnInit() {
-    // TODO: use firebase database to build the teams.
+    service.buildTeams();
   }
 }

~~~

Also remember to update the template of homepage.

~~~diff
// changes of homepage.html


@@ -5,10 +5,10 @@
             {{drawerHeaderMessage}}
         </div>
         <material-list class="tm-home-drawer-list">
-            <material-list-item *ngFor="let team of teams"
+            <material-list-item *ngFor="let team of service.teams"
                                 class="tm-team-list-item"
                                 [class.deep-blue]="shouldShowAsDeepBlue(team)"
-                                (trigger)="drawer.toggle();">
+                                (trigger)="drawer.toggle(); service.changeTeam(team);">
                 {{team}}
             </material-list-item>
         </material-list>
@@ -28,11 +28,11 @@
             <material-button class="material-drawer-button" icon (trigger)="drawer.toggle()">
                 <material-icon icon="menu"></material-icon>
             </material-button>
-            <div class="tm-main-content-header-title">header</div>
+            <div class="tm-main-content-header-title">{{service.currentTeam}}</div>
         </div>
         <div class="tm-main-content-content">
             <ul style="padding-left: 0; display: grid; grid-template-columns: 1fr 1fr;">
-                <li *ngFor="let message of previousEmojis" class="tm-prev-emoji-item">
+                <li *ngFor="let message of service.previousEmojis" class="tm-prev-emoji-item">
                     <user-post [message]="message"></user-post>
                 </li>
             </ul>
~~~

Lastly, we need to create a new `Message` object when the user selects an emoji from the selector. This should be done in `EmojiSelector` component.

~~~diff
class EmojiSelectorComponent extends EmojiSelectorMessages with EmojiList {
   @Output()
   Stream get onSelect => _selectStream.stream;
 
+  FirebaseService service;
+
+  EmojiSelectorComponent(this.service);
+
   void onCancel() => _selectStream.add(null);
 
-  void onSelectEmoji(String emoji) {
-    // TODO: add new message to the stream.
-    _selectStream.add(null);
-  }
+  void onSelectEmoji(String emoji) => _selectStream.add(new Message(
+      service.fbAuth.currentUser.displayName,
+      service.fbAuth.currentUser.photoURL,
+      emoji,
+      new DateTime.now()));
}
~~~

After that, our Teamoji app should be completed.

### Finishing up
You can find the complete code of Teamoji in the master branch of the [Github repo](https://github.com/NickWu007/Teamoji-practice). I have omitted some steps in the styling and the steps to push it with Firebase hosting. If that's something you want to know, let me know in the comments and I can do a follow-up in the future.

Also, I will do a separate post as a reflection of Teamoji, from the conception of the idea, and design and engineering decisions, and the process of building it. Stay tuned for that.

I hope this series serves as a good intro if you are new to AngularDart and Firebase. In the next few months I will try to make this web app into a mobile app, with [Flutter](flutter.io).

Hope you had fun reading this series. I will see you soon.

Nick