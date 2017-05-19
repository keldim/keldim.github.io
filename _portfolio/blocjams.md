---
layout: post
date: 2017-05-17
title: BlocJams
thumbnail-path: "img/bloc_jams_css_basics_completed.png"
short-description: BlocJams made with Angular

---
## Goal
Recreate the Bloc-Jams website that was made during the instructional Bloc checkpoints using Angular

In the instructional Bloc checkpoints, DOM scripting and jQuery were used to develop the functionalities of the Bloc-Jams website. In this project, for the most part, Angular was used instead so that I could learn and have a chance to use Angular. 

<br>
<br>

#### Landing Page
{:.center}
![My helpful screenshot]({{http://127.0.0.1:4000/}}/img/Screen Shot 2017-05-19 at 2.52.47 AM.png)

{:.center}
![My helpful screenshot]({{http://127.0.0.1:4000/}}/img/Screen Shot 2017-05-19 at 2.52.52 AM.png)

The first page a user will see when entering the website.

For this website, I used single-page applicatios (SPA) model. In the traditional model, the browswer has to load a new HTML document when a user navigates between pages. This can result in a slow user experience and having to write redundant code for each HTML document. With Angular, I pulled the shared HTML into one global file, and each of the unique fragments into separate templates. Using templates resulted in less loading time and less code to write. 

"ui-sref" link and "ui-view" tag were used to navigate between pages and load templates. 
{% highlight javascript %}

<body>
    <nav class="navbar">
        <a ui-sref="landing" class="logo">    
            <img src="assets/images/bloc_jams_logo.png" alt="bloc jams logo">
            </a>
            <div class="links-container">
            <a ui-sref="collection" class="navbar-link">collection</a>
            </div>
        </nav> 
    <ui-view></ui-view>
</body>
{% endhighlight %}

<br>
<br>

#### Collecton View
{:.center}
![My helpful screenshot]({{http://127.0.0.1:4000/}}/img/Screen Shot 2017-05-19 at 2.53.04 AM.png)

{:.center}
![My helpful screenshot]({{http://127.0.0.1:4000/}}/img/Screen Shot 2017-05-19 at 2.53.10 AM.png)

Shows all the available albums to play

<br>
<br>

#### Album View
{:.center}
![My helpful screenshot]({{http://127.0.0.1:4000/}}/img/Screen Shot 2017-05-19 at 2.53.19 AM.png)

{:.center}
![My helpful screenshot]({{http://127.0.0.1:4000/}}/img/Screen Shot 2017-05-19 at 2.53.21 AM.png)

{:.center}
![My helpful screenshot]({{http://127.0.0.1:4000/}}/img/Screen Shot 2017-05-19 at 8.31.00 AM.png)

Shows all the songs in the album 

As the mouse cursor hovers on the song number, the number changes to a button. A user can play and pause by clicking on the button. The user can also use the features on the gray player bar to play, pause, and switch to the next song. 

<br>
<br>

#### For Collection and Album Views...

**_Controllers and Services_**


For both the collection and album views, controllers were used to control the flow of data into those pages. I learned the syntax for defining a controller: 
{% highlight javascript %}

(function() {
     function CollectionCtrl() {
     }
 
     angular
         .module('blocJams')
         .controller('CollectionCtrl', CollectionCtrl);
 })();
 
 {% endhighlight %}
 
 I learned each controller has its own $scope object, and this is where the data that the view uses are kept. Data can be added to the $scope object using ‘this’ in the controller:

{% highlight javascript %}


function CollectionCtrl() {
     this.albums = [];
     for (var i=0; i < 12; i++) {
         this.albums.push(angular.copy(albumPicasso));
     }
 }
 
 {% endhighlight %}
 
 Services were than injected to controllers. Services contain data that can be shared across several components (such as controllers and even services themselves) in an application.  
 
 {% highlight javascript %}
 
 (function() {
     function AlbumCtrl(Fixtures) {
         this.albumData = angular.copy(albumPicasso);
     }

     angular
         .module('blocJams')
         .controller('AlbumCtrl', ['Fixtures', AlbumCtrl]);
 })();
 
 {% endhighlight %}
 
 The services I made contained album info and methods that could be used in both collection and album views. 
 {% highlight javascript %}
 
 function Fixtures() {
     var Fixtures = {};

     var albumPicasso = { ... };

     var albumMarconi = { ... };

     Fixtures.getAlbum = function() {
         return albumPicasso;
     };

     return Fixtures;
 }
 
 {% endhighlight %}
 
 At times, it was difficult to remember the name of all the variables from the controllers and services, and it became confusing when I tried to call them. I somehow made it through. 
 
 <br>
 
 **_Directives_**
 
 In the album view, instead of using click-handlers from Dom scripting or .click from jquery, 'ng-click' directive from Angular was used. A directive binds Angular functionality to HTML on a page. When the user clicks on the previous or next button at the player bar, the ng-click directive calls the method defined in the services and plays the other songs.
 
 {% highlight javascript %}
<a class="previous" ng-click="playerBar.songPlayer.previous()">
                 <span class="ion-skip-backward"></span>
             </a>
             </a>
             <a class="next" ng-click="playerBar.songPlayer.next()">
                 <span class="ion-skip-forward"></span>
             </a>
    {% endhighlight %}
    
There were many directives that I learned to use as I was building collection and album views.
    