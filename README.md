![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png)

#Intro to CRUD and Firebase 


### Objectives

_After this lesson, students will be able to:_

- Explain what CRUD is.
- Explain the HTTP methods associated with CRUD.
- Implement CRUD using Firebase in an application.
- Build a full-stack app.

### Preparation

_Before this lesson, students should already be able to:_

- Identify all the HTTP Verbs & their uses.
- Describe APIs & how to make calls and consume API data.

---

<a name = "opening"></a>
## 1. Introduction to CRUD and Firebase (10 min)

There are four standard things that you can do with an app's data, via its API: **C**reate, **R**ead, **U**pdate, and **D**elete, commonly known as **CRUD**. For example, with Instagram a user is allowed to 

- __Create__ new data on Instagram's server by uploading a post with a picture with a caption,
- __Read__ data from Instagram's server by fetching posts from the Instagram API to be shown on the screen,
- __Update__ data in Instagam's server by editing a caption, or
- __Delete__ data from Instagram's server by removing an entire post.

Typically to implement CRUD, you have to run your own application on the server, and use it to interact with a database, but today we're going to be building a CRUD app using Firebase, a "Backend as a Service" that we can interact with directly from front-end apps, and it takes care of all the DB stuff for us.

---

<a name = "review"></a>
## 2. CRUD and HTTP verbs (20 min)

Even though we have not yet explicitly used the term CRUD in this course, we've gone over some of the things that are involved with implementing CRUD. Can you think of something we've done in an earlier lesson that falls into any of the categories of create, read, update or delete? 

Hint: Think of the Feedr project. What jQuery method are we using to fetch the data?

That would be `get`, which is one of the HTTP verbs we discussed a while ago. Create, Read, Update and Delete actually correspond directly with the HTTP verbs POST, GET, PUT and DELETE. Here's a table to show the relations:

|HTTP Method|CRUD|Further explanation|
|:---|:-----|:-----|
|POST| Create|Most often utilized to _create_ new resources. Upon success, returns a 201 status code.
|GET| Read|The HTTP GET method is used to _read_ a representation of a resource. Upon success, returns data in the form of XML or JSON with a 200 status code. 
|PUT| Update|Most often used to _update_ data on a server. Upon success, usually returns a 200 or 204.
|DELETE| Delete|Used to _delete_ a resources. Upon success, usually returns a 200 status code.

So far we haven't used much besides GET, because we've just been fetching information for display, not altering any information on anyone's servers. But we'll be learning how to do that today.

Firebase comes with its own SDK that we'll using to implement the above methods (remember this means "software development kit" -- a bunch of methods for us to use, basically), so you won't be using those HTTP verbs directly in this lesson today. But they're important to know about, especially because when researching how to use third-party APIs, the operations that they make available to us are often listed by HTTP verb. 

And we'll point out which HTTP verbs are being implemented under the hood as we explain the Firebase methods today.

---

<a name = "introduction1"></a>
## 3. Understand CRUD Through Public APIs (15 min)

Now that you have an understanding of what CRUD is and how it's implemented by means of HTTP methods, spend the next 15 minutes with a partner researching one or two of the following APIs (or any other API) to find examples of how to create, read, update or delete. In the examples you find, exactly is being created, read, updated or deleted? 

For example, for the Twitter API, what HTTP method on what endpoint must you ping in order to create a post in a feed (otherwise known as tweeting)?

API ideas:

- Twitter
- Instagram
- YouTube
- Flickr
- Facebook


> **Note:** Certain APIs may provide partial support for CRUD functionality, but not full support -- for instance, they might allow you to archive items, instead of allowing you to delete them. If you're researching one of these APIs, note its limitations.

---

<a name = "introduction2"></a>
## 4. Firebase, a "Backend as a Service" (10 min)

So far in this course we have covered how to consume data from third-party APIs like Reddit and 500px, but have not yet created our own API. 

We've been doing all client-side, or _front end_ development. The process of actually creating the API, not consuming it, falls under the umbrella of _back end_ development. You can think of back end code as all the code living on the server side, the code that app users never interact with directly. It is the code that runs on a computer in a server farm owned by a cloud provider like Amazon, located somewhere in eastern Washington near a hydroelectric dam (for the cheapest possible electricity) -- not on a browser running on your laptop.

Even though we don't have all the skills needed to roll out our own fully custom back end, today we are going to learn how to use Firebase, a back end as a service (BEaaS), to help us create our very own API!

Firebase has a number of features that together allow it to serve as the entire back-end of an application, but today we'll be working mainly with its database feature: we'll use Firebase to store information that the user creates. And as we mentioned before, Firebase also comes with a JavaScript SDK library for you to use in your front end code, to interact with said database. 

Firebase is a great tool for front end developers who are either not familiar with building out a back end themselves, or just don't have time. Sometimes you want to quickly prototype and focus on your UX! And you can do a lot before you start having to pay: each app in the free pricing tier is allowed to have a database with up to 100 users connected simultaneously.

But it's also industrial-strength. Some noteworthy users of Firebase include CBS, Instacart and Jawbone. 

So, without further ado, let's go ahead and set up an application to use Firebase!

---

<a name = "setup"></a>
## 5. Firebase Setup (35 min)

First, download the starter code from [https://github.com/jsdev2/lesson16-starter](https://github.com/jsdev2/lesson16-starter). Click on "Clone or Download", then click on "Download ZIP". Unzip the folder and place it in your `GA-JS` folder.

And then go to Google's new [Firebase site](https://firebase.google.com/) and sign up for Firebase.

> **Note:** Firebase requires you to sign up with Google, so if you don't already, you'll need to create a Gmail account.

If you have multiple google accounts, you'll be taken to a page asking you to select which one you'd like to use for your Firebase authentication.

Once you've successfully logged in you will be taken to your _Dashboard_. In this view, you are able to easily create new apps, more specifically, back end databases for your apps.

> **Note:** The Firebase service also offers hosting capabilities (which we'll cover in the next lesson) and uses your app name as part of the app's URL. This means that your app name must be unique to any other Firebase app name, so that Firebase can correctly direct users to your application when it is hosted online.

> In fact, while for web development purposes, Firebase is most often utilized as a database, but as you can see from the [features page](https://firebase.google.com/features/) Firebase can be used not just for databases and for hosting but for authentication, messaging and much more. 


---

![](images/01-createProject.png)

![](images/02-enterProjectName.png)

![](images/03-dashboard.png)

![](images/04-projectConfig.png)

---

At this point you'll want to copy the Firebase initialization code into your html file. 

Then you'll need to change your settings to allow access to the DB without having to send the user through an OAuth authentication process. (This will save us a number of steps now while we're learning the basics of Firebase, but we'll also show you how to do the authentication later, because it might be useful to you in your projects.)

---

![](images/05-databasePage.png)

![](images/06-sampleRulesDocs.png)

![](images/07-enterRules.png)

---

Voilà! That's it! You've created a Firebase account, created a Firebase database and connected with your database on the client-side of your application. Now let's move on to creating some data!



---

<a name = "codealong1"></a>
## 6. Create with Firebase (15 min)

Before we completely dive in, to give you context, here's what we'll be building for demo purposes:

---

![](images/my-little-pony-screenshot.png)

---

We'll be able to upload messages to a message board, upvote messages -- no downvoting, this is My Little Pony! -- as well as delete messages. Full CRUD!

The first part of CRUD we'll be covering is _C_, create.

Instead of the traditional fruits, we'll be uploading some ponies.

First, cut and paste the JS that is inline in the initialization code in your html file, and put it in your app.js file instead -- just so all your JS will be in the same place.

Then add the code below that to get a reference to the db and then save the ponies.


```js
  // Initialize Firebase (copied from html file)
  var config = {
    apiKey: '<your-api-key>',
    authDomain: '<your-auth-domain>',
    databaseURL: '<your-database-url>',
    storageBucket: '<your-storage-bucket>'
  };
  firebase.initializeApp(config);

  // rest of code

  var firebaseDB = firebase.database();

  var poniesInFirebase = firebaseDB.ref('ponies');

  poniesInFirebase.push({
    name: 'Fluttershy',
    wikiUrl: 'http://mlp.wikia.com/wiki/Fluttershy'
  });

  poniesInFirebase.push({
    name: 'Derpy Hooves',
    wikiUrl: 'http://mlp.wikia.com/wiki/Derpy'
  });

  
```
So, three steps here:

1. We get a reference to the database by running `firebase.database()`.

2. We set aside an area in our database called "ponies", for us to store information about ponies. We get a reference to that part of the database and call it `poniesInFirebase`.

3. We push some ponies-related objects to the db using our reference `poniesInFirebase`, and the `push` method that is part of the Firebase SDK (this is not the same as the regular JS `push` method).

And now we have CREATEd some data about My Little Ponies!



***Note:*** Everything you push has to be an object -- can't be a primitive like a string or a number. Firebase converts the object you `push` into JSON automatically for you before it sends it over the internet.

To see our data residing in Firebase, let's go to the [Firebase console](https://console.firebase.google.com). If you're not already in your project, select it and then click on "database" in the left nav bar. 

You should see something like this:

---

![](images/objects-in-firebase-screenshot.png)

---

Which you can expand into this:

---

![](images/objects-in-firebase-screenshot-expanded.png)

---

Let's break this down. 

We can think of as a big nested JavaScript object -- a big tree of data -- and we can think of the database name (`harrington-test2` in my screenshots) as the first key (first property name) in the object. 

Next, you'll see `ponies` which is the subkey containing all of the ponies data you've pushed to the db using the Firebase `push` method. Within `ponies` you'll see a bunch of weird-looking keys, such as `-KCTatb-xmw96WW4LRsLd`. These are unique identifiers for each of your messages.

***Important:*** Notice that this collection of ponies is **not an array**. We've often used arrays for similar situations in the past, where we have a list of similar items. Here we're using an object, with those strange key names as its property names. We'll see in a little while how to use those to identify the right items when we want to do things besides just CREATE -- when we want to UPDATE and DELETE. 

Another important thing to note is that our Firebase db -- like all APIs -- makes heavy use of URLs as endpoints to access its data. Besides checking your Dashboard you can also find data by using Firebase's structured URL format for querying, `https://<your app name>.firebaseio.com/<key of collection>/<key of specific item>`. For example, `https://harrington-test2.firebaseio.com/fruits/-KKior-IYcZ4z2zZEl4f`.

By using that structured URL to fetch data, we are essentially calling upon a new API that we have just created, for ponies!

And we'll be using this structured URL syntax later in the lesson when we want to UPDATE and DELETE information.

Now we're done with **C**reate. Do you remember which HTTP method goes along with **C**reate?

---

## 7. Mini-Lab: Save some messages (20 minutes)

Now that you've seen the basics of how to save data to Firebase, comment out the code in your app about saving pony objects, and write some code to save messages ABOUT ponies, in a new section in your database that you will create, called "messages". 

Every time the user types in the textarea and clicks on "Post to board", it should save an object to the "messages" section of the database, that looks like this:

```js
{
  message: '<new message text>',
  votes: 0
}
```
(Every message starts out sadly with zero votes.)

Use the ponies code from the codealong as a model for how to save messages to the database.

And when you think it's working, use the Firebase console to see if your messages are showing up in the database.

***Answer:***

```js

// Initialize Firebase
var config = {
  apiKey: "AIzaSyApxANaHI176HJK8pA4aQLuOpehfzuh7Cs",
  authDomain: "harrington-test4.firebaseapp.com",
  databaseURL: "https://harrington-test4.firebaseio.com",
  storageBucket: ""
};

firebase.initializeApp(config);
var firebaseDB = firebase.database();
var messagesInFirebase = firebaseDB.ref('messages');

// Instead of the 'submit' event on the form element,
// another option for the listener below is the 'click' 
// event on the button element. Both will work.

$('#message-form').on('submit', function(event) {

  // To prevent the default behavior of the form
  // element, which is refreshing the page:
  event.preventDefault();

  // Get the user's input.
  var messageText = $('#message').val();

  // "Create" a new message object in the database
  messagesInFirebase.push({
    message: messageText,
    votes: 0
  });

});

```

---

<a name = "codealong2"></a>
## 8. Read with Firebase (20 min)

After a My Little Pony message has been successfully saved to our database in the back end, we want to be able to show (_Read_) it on our app for the world to see. In order to do this we are going to need to follow a few steps:

- Set up an event listener which queries our database for message data whenever that data changes (and is also always checks once when the app is initialized)
- Have the event listener render that data to the DOM for users to see

So, how do we go about querying our Firebase database?

```js

// Using our 'firebaseDB' that we created earlier,
// Get a reference to the 'messages' section of the db,
// and listen on that for changes in messages data

firebaseDB.ref('messages').on('value', function(results) {

  // First remove all the current lis 
  // so there won't be any dupes on the page
  $('.message-board').empty();

  // Now iterate through coming from database call
  // Note: we haven't seen `Object.keys` before -- 
  // this gets an array of the keys in an object. It
  // is the best of a couple of different ways to
  // iterate through an object.

  // Also note: we need to use .val() on the results
  // to get something readable. Further note: This
  // has nothing to do with the jQuery .val() method

  var messages = results.val();

  console.log(results);
  console.log(messages);

  var weirdIds = Object.keys(messages);

  weirdIds.forEach(function(weirdId) {
    var messageObject = messages[weirdId];
    var message = messageObject.message;
    var votes = messageObject.votes;

    // render the message and its vote total in the DOM
    var $votesSpan = $('<span>').addClass('votes').html(votes);
    var $messageLi = $('<li>').html(message).append($votesSpan);
    $('.message-board').append($messageLi);
  });

});

```

So we use our reference to our application's database (`firebaseDB`) to get a reference to our `messages` collection, and then we listen on that for any changes (using the special Firebase `value` event, which listens for any changes or additions or deletions of any values in the collection). 

>One very nice feature of a Firebase database is that it works in realtime, meaning the `.on()` method will set up an event listener which will cause our front end app to update as soon as anything in the db changes.

To extract the info we're looking for so that we can render it into our UI (messages and votes), we must iterate through the `messages` object and access each data object's `message` and `votes` properties.

> **Note:** What Firebase returns to us (and automatically converts from a JSON string to a JS object under the hood), is an object which we are calling `results`. It is unreadable. To extract something you can use, we need Firebase's `.val()` function.

---

<a name = "codealong3"></a>
## 9. Update with Firebase (30 min)

To perform the _U_ in CRUD, follow these steps:

- Identify the data you wish to update (the tricky part)
- Update its value with the new value
- Save


So far we've either created new data, or fetched all items at once. However, for _update_, we are concerned with updating specific messages, so the question arises, how will we know we are updating the right message object? 

Well, in order to do that you have to know the weird id of the object you want.

So let's say we want to set up an event listener on the number of votes, so that if a user clicks on it, it goes up one. 

There are several ways for the click listener to know which weird id to use when it is triggered. We're going to do it by setting all the event listeners in the `forEach` loop at the time that we render the items, and this way the weird ids, (and the vote totals) can be stored in closures.

```js

firebaseDB.ref('messages').on('value', function(results) {

  // First remove all the current lis 
  // so there won't be any dupes on the page
  $('.message-board').empty();

  // Now iterate through the data from the server
  // and render the messages, including their event listeners

  var messages = results.val();

  var weirdIds = Object.keys(messages);

  weirdIds.forEach(function(weirdId) {
    var messageObject = messages[weirdId];
    var message = messageObject.message;
    var votes = messageObject.votes;

    // Get a reference to the object in the database,
    // using the url-like syntax for identifying it.
    // Necessary for updating or deleting.
    var messageInFirebase = firebaseDB.ref('messages/' + weirdId);

    // Make a new votesSpan
    var $votesSpan = $('<span>').addClass('votes').html(votes);

    // Set the event votes-updating event listener on the $votesSpan
    $votesSpan.on('click', function() {
      // The user just clicked on the votes span element, so 
      // update the votes property: add one to the last value
      // we had stored in Firebase
      messageInFirebase.update({
        votes: votes + 1
      });
    });

    // render the message and its vote total in the DOM
    var $messageLi = $('<li>').html(message).append($votesSpan);
    $('.message-board').append($messageLi);
  });

});

```

Can you remember Firebase's data URL structure we covered a little earlier? In case you forgot it's: `https://<your app name>.firebaseio.com/<key of collection>/<key of specific item>`. So in this case, for the updating, it would be something like `https://harrington-test2.firebaseio.com/messages/-stienrst-STtrs344str`

Looking at the code above, you can see how we use part of this URL path to identify and access one of the messages, which we then use `update` to change the `votes` property on.

***Also note:*** The list rerendered itself automatically, because of the listener that listens on changes in the data, removes the old list, and rerenders everything from scratch every time. 

>Note that in Firebase, `update()` will update only the specific fields for which you pass in new values, while there's another method, `set()`, which will blow away the whole object, removing any fields that it doesn't specifically update. For instance, if you have an object in the db `{a: 5, b: 10}` and you run `update({a: 100})` to update it, you'll end up with `{a: 100, b: 10}` in the db, but if you run `set({a: 100})`, you'll end up with `{a: 100}` only, and the `b` property will be erased.


---

<a name = "codealong4"></a>
## 10. Delete with Firebase (10 min)

To perform the _D_ in CRUD:

Get the reference to the object in the database the same way you do for updating. Then you use the `remove()` method. For instance, if you had a collection of fruit objects in the db, stored under the name "fruits", and you wanted to remove one:

```js
// Assuming we already have a reference to the id 
// for the fruit object we want in the database, 
// stored in the variable `weirdId` : 
var fruitInFirebase = firebaseDB.ref('fruits/' + weirdId);
fruitInFirebase.remove();
```


### Practice on your own: Delete a My Little Pony message (15 min)

Add an event listener to our My Little Pony code for each message, so that if the user double clicks on the message, it gets deleted from the database. (Reminder: the double-click event in the DOM is 'dblclick').

***Also note, once again:*** The list will rerender itself automatically, because of the listener that listens on changes in the messages data -- including deletions -- and rerenders everything.


Whew!

---

<a name = "conclusion"></a>
## 11. Conclusion (5 min)

- What does CRUD stand for?
- What HTTP methods are associated with C, R, U and D?
- What is a BEaaS?
- Why would some developers choose to use the Firebase service?
- Is Firebase free?
- Do all apps have CRUD?
- How would you explain the front-end vs. the back-end to a fellow developer?

