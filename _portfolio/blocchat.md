---
layout: post
date: 2017-05-18
title: BlocChat
thumbnail-path: "img/Screen Shot 2017-05-16 at 4.24.02 AM.png"
short-description: BlocChat for real time messaging
---
## Goal
Build an application that sends and receives messages in real time

<br>
<br>

#### Sidebar
{:.center}
![My helpful screenshot]({{http://127.0.0.1:4000/}}/img/Screen Shot 2017-05-16 at 4.23.55 AM.png)
A sidebar at the left shows a list of available chat rooms.

I first struggled in using CSS to create the sidebar in the right place. After some googling and reviewing of lecture slides, I figured out what to do. I used "position: absolute" to remove the sidebar from the normal flow of the page. Then "float: left" was used to make the sidebar stick to the left end of the page. Finally, I used "hegiht: 100%" to extend the height of the sidebar to the bottom of the page.

{% highlight javascript %}
sidebar {
    float: left;
    height: 100%;
    position: absolute;
}
{% endhighlight %}

I used Firebase from google to store rooms and messages for BlocChat. It was my first time using Firebase, and I had to learn the codes for storing and calling data. 

{% highlight javascript %}
(function() {
  function Room($firebaseArray) {
    var ref = firebase.database().ref();
  }

  angular
    .module('blocChat')
    .factory('Room', ['$firebaseArray', Room]);
})();
{% endhighlight %}

<br>
<br>

#### New room
{:.center}
![My helpful screenshot]({{http://127.0.0.1:4000/}}/img/Screen Shot 2017-05-18 at 2.48.38 AM.png)
"New room" button in the sidebar allows a user to add a new room.

I used UI Bootstrap's $uibModal service to create a form to recieve the room name and send it to Firebase. I also used Bootstrap to style the elements in the form such as the buttons and input box. It was my first time using Bootstrap, and it was convenient.

{% highlight javascript %}
var openmodal = function () {
      var modalInstance = $uibModal.open({
          templateUrl: '/templates/newroom.html',
          controller: function ($scope) {
              console.log("loaded");
              $scope.createRoom = function (value) {
                    rooms.$add(value);
                    modalInstance.dismiss();
              };
              $scope.dismissRoom = function () {
                    modalInstance.dismiss();
              };  
          }
      })   
    };
{% endhighlight %}

<br>
<br>

#### A list of messages
{:.center}
![My helpful screenshot]({{http://127.0.0.1:4000/}}/img/Screen Shot 2017-05-16 at 4.24.02 AM.png)
When a user clicks on the room name at the sidebar, a list of messages in the room appears.

I had to write codes that filter the messages stored in firebase based on the clicked room and list the filtered messages on the screen. I struggled with this, but with the help of my mentor, I ended up figuring it out.

{% highlight javascript %}
return {
      getByRoomId: function (name) {
        // Filter the messages by their room ID.
        return $firebaseArray(ref.orderByChild("roomId").equalTo(name));
      },
        send: function(newMessage, identity) {
        // Send method logic
            console.log("working!");
         $firebaseArray(ref).$add({content: newMessage, roomId: identity, username: $cookies.get('blocChatCurrentUser')});
            $('input:text').val('');
      }
        }
{% endhighlight %}

<br>
<br>

{:.center}
![My helpful screenshot]({{http://127.0.0.1:4000/}}/img/Screen Shot 2017-05-16 at 4.24.11 AM.png)
If the list is longer than the screen size, a scroll bar appears.

<br>
<br>

#### Username
{:.center}
![My helpful screenshot]({{http://127.0.0.1:4000/}}/img/Screen Shot 2017-05-16 at 4.23.41 AM.png)
When a user first enters the BlocChat site, a pop up appears which asks for the username that the user wants to use for the site. The username is saved in the cookies of the browser.