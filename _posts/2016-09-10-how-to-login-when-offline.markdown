---
author: nickwu0715
comments: true
date: 2016-09-10 20:47:00+00:00
layout: post
link: https://nickwuedinburgh.wordpress.com/2016/09/10/how-to-login-when-offline/
slug: how-to-login-when-offline
title: How to login when offline?
wordpress_id: 120
categories: [technical]

---
With the uprise of mobile systems, nowadays apps are used by people more often than ever. A real shame is that lots of the apps need Internet connectivity to perform full functionalities. Without it they are more like yet another block on home screen just for definition. I'd venture and say the most miserable time when holding a smartphone is on plane. No Internet, no fun.

Today we are going to look at a way to make this better: how to implement a login persistence logic when offline.



### The trouble



To be honest, the unsatisfactory UX in offline mode isn't really to blame on developers. One of the biggest advantage while online, and probably what makes the whole thing a pain, is that an app can send and receive data to/from servers and other devices. Just in terms of login, this would look kind of familiar to web developers:

~~~javascript
$.ajax(DOMAIN +"/api/login.php",
{type: "POST",
dataType: "json",
crossDomain: true,
data: JSON.stringify(login_info),
success: function (data, status, xhr) {
console.log("success");
console.log(data);
// Redirect to home page
},
error: function (xhr, status) {
console.log("login failed");
}
});
~~~

Without connection, this is bound to fail, and user cannot log in. For the rest of this post, we are going to try and make it work.



### Idea: Local storage



The way to get through this isn't perfect, but I feel like it would do for most of the time. Say at some point, user logs in under Internet connection. On the success callback function, we can 'remember' that the user has logged in at this time, and grant the app with this credential for say, 24 hours. Later when the user goes offline and tries to login again, the app notices that there's a valid credential stored, it would then bypasses the HTTP request, and lets the user to log in.

However, this is not that easy. We will have to store the previous HTTP response, usually a url to go to with user id parameters. Also, we need to record the effective time period. In specific cases, the app has to store enough info to support further interactions after login. But in a nut shell, this will do.



### Implementation



A demo is worth a thousand words.

~~~javascript
// Modified login logic
$.ajax(DOMAIN +"/api/login.php",
{type: "POST",
dataType: "json",
crossDomain: true,
data: JSON.stringify(login_info),
success: function (data, status, xhr) {
console.log("success");
console.log(data);
// Redirect to home page
AppStorage.store('login', login_info); // Add persistence logic

AppStorage.store('last_login_time', Date.currentTime()); // Timestamp needs to be save too
},
error: function (xhr, status) {
console.log("login failed");
}
});
~~~

If the app offline, we would something like this:

~~~javascript
var last_login_time = AppStorage.retrieve('last_login_time');
if (Date.currentTime < ladt_login_time + 24) { //Check for time validity
var url = gen_url(AppStorage.retrieve('response_url'), AppStorage.retrieve('login'));
window.location.assign(url);
} else {
console.log("Stored credential expired, please re enter them");
// Go to online login logic.
}
~~~

Note: some of the Objects are made up, like AppStorage. In real world you can use SQLite or some other local storage library.



### A few thoughts



The key idea when trying to implement offline mode is to make all HTTP calls into some sort of local storage or caching mechanism. Also, we haven't talked about the case user loses connection in the middle of an app session. For more about offline mode, checkout [this article](http://www.clairereynaud.net/blog/adding-offline-mode-to-your-mobile-app/) for more information.

Hope this post helps you when trying to implement an offline mode for your app. If not, please let me know in the comment section.

Nick
