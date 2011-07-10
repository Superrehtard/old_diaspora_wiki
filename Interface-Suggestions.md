**Kevin:** Here's a list of what I believe is important to look into at least before the beta. It involves improvements to the UX, tweaks and features. You should try to work through these points or come up with a better answer. Almost everything on this list is from the point of view of a mainstream user so use it to your advantage.

**Edit: Suggestions by multiple awesome people!**


***Note: This page is about current features and provides suggestions to improve the user experience of them. Please do not post your feature requests here!***

##Aspects
We should get rid of ‘all aspects’ in the left menu and replace it with having litterly all aspects selected. Having no aspects selected should say: select one or more aspects from the left menu. Have a way to set default aspects. This stimulates sharing with a selection of common aspects instead of posting everything to ‘all aspects’. You could exclude your ‘work’ or ‘teachers’ aspect for example.

  - **untitaker:** [Add checkboxes on aspects list](http://imgur.com/a/jf6Pu) -- Most users associate checkboxes with the ability to select multiple items:

![State 1](http://i.imgur.com/u6noR.jpg) ![State 2](http://i.imgur.com/TZRY4.jpg) ![State 3](http://i.imgur.com/hYSPa.jpg)

  - **Kevin:** Use checkboxes to select multiple aspects. Clicking on the name of an aspect shows that aspect only(?). Clicking 'your aspects' selects all aspects again. Possibly a key+click for selecting single aspects.

##Public / private
  - Make all comments on a public post public, both to people logged in as well as people not logged in to Diaspora. Add a small note near the share button to let people know their comments will be publicly viewable and change the hover message on 'Public' to ‘This post and it's comments are viewable to anyone on the web’. Get rid of the /p/ page and display the /status_message/ page to people not logged in. This should solve a lot of uncertainty about visibility and improves the general experience.

  - Add a hover-over message for a ‘Limited’ post by a contact for more consistency. For example: “This post is shared with a limited audience.” (untitaker: See my suggestion in the "Publisher" section)

  - **Edit:** even more important once hashtags in posts can be found.

##Publisher
  - _Don’t send public posts, shared with only a selection of aspects, to all aspects! Make the post publicly available on the profile without sending it to the streams of people not in the aspects. The other solution is to only allow public posting when all aspects are selected. Right now ‘public’ overrides the aspect selection, which is unexpected behavior._ (DONE)

  - Make mentions stand out in the publisher, like the tags on the profile edit page. This should also solve adding punctuation right after a mention.
    - **untitaker:** I'd rather go with the way tags are shown in the *publisher*. See [this issue](https://github.com/diaspora/diaspora/issues/1566)

  - Have an aspect dropdown next to the share button. The dropdown should have the currently selected aspects preset but selecting aspects in it should not change the stream. This results in being able to watch the selection you like most and still be able to post to other aspects. Because you won't have to load the streams, performance will be better. This will be a major UX improvement to new and existing users. This can also replace the labels in the bookmarklet, solving the problem of too many aspects / translations there too. 

![photo](http://i.imgur.com/WOY4c.png)

*This is backed by a real UX expert whom Dennis Schubert talked too!*

###[Tooltips on world icon](http://imgur.com/a/fxjlv)
**untitaker:**

![Limited - your post can only be seen by people you are sharing with](http://i.imgur.com/la8M3.jpg)

![Public - your post is visible to everyone and can be found by search engines](http://i.imgur.com/nOJeq.jpg)

**Edit:** I realized that the first tooltip's text is not fully correct, whatever, it's more about the idea.


##Messages
  - Have a dropdown menu. People have come to expect this after the notification dropdown. Also allow middle mouse-button clicking on the icon to go directly to the full notifications/conversations page.

##Stream
  - **Kevin:** Don’t hide videos in the stream anymore. It may have seemed to be a very good idea to keep Diaspora clean and minimalistic but people barely watch videos posted by their contacts. The video experience on Diaspora is pretty bad as is. Showing them right away will not hurt the experience more than photos do.
    - *untitaker:* Might be a privacy (on Linux and Mac also a performance) issue, as well as the cubbi.es images. And i hardly believe that a pod owner has the ressources to save a bigger thumbnail of the vid on his server. Maybe make it a user setting, defaults to deactivated?

Get the ‘x <3’ on one line with the comment and like links and give the posts and comments a bit of extra love in general. Something like this: (exclude the likes on comments for now).

![Timeline suggestions](http://i.imgur.com/XnpWP.png)

The names of those who liked the content should go in a hover instead (as they aren’t important enough to justify taking a lot of space).

The time ago really should go back under the post like it used to be. Not only does it look better; right now it also switches the name and time ago on RTL languages. It also goes very well with the apps (Public 1 minute ago via Cubbi.es). 

Get rid of that awful unicode (☺ and ☹) smilies. Either replace them with real smilies or delete them altogether.

The new AJAX’y likes aren’t really that great. People don’t really care about the names as much as being able to quickly see the amount of them. It’s also looks like it’s a bug, which is just bad. Should revert that one.

##The right sidebar
**untitaker:** The right sidebar is too bloated. Here are some widgets that should be moved or removed.

###Diaspora ID
  - Could be moved in the user menu (top right), generally style the user menu like a hovercard, with logout, settings buttons instead of mention, message. (untitaker)

###Contacts
  - Could be moved into a dropdown or tooltip of the contacts icon in the header. (untitaker)

###Inviting people, connecting to services
  - Move those somewhere in the "getting started" intro or make them a tooltip in the middlerow, which disappear after a few logins. (untitaker)

**Kevin:** Does not agree! Every network has these 'getting started' widgets. We could make a difference by actually making it useful (e.g. Wordpress-style widgetbar). I know you don't agree with that though..

##Photoviewer
**Kevin:** Have a huge photo viewer pop up when you click on a photo in the stream, with previous/next buttons, a comment section and likes. Also needs a link to the full size image. Diaspora should have a much better photo experience. It’s one of those things we can easily do better than Facebook.

**untitaker:** Lightbox with comments in sidebar - Something like http://bueltge.de/photos, but as a popup/layer and simplified.
##Photo albums
This one is pretty damn big but much needed! Here's my take on it:

There should be a button in the header to get to your albums (also on the profile). When you click it you get an album overview, with the photos in the albums being the album covers with a nice name on them (much like back in the ol' days).

On the album overview you can add new albums.

Click on an album to go to the album's page. Here you find a 'upload photos' button. Once you have uploaded the photos it a message should come up and say: don't forget to attach aspects to your photos. On the album page you can assign aspects to each photo.

* Given a user named Bob and a user named Alice... :P
* Alice puts Bob in her 'friends' aspect
* And Alice makes a new album called 'Animals'
* And Alice uploads a photo of her dog to the album called 'Animals'
* And Alice assigns the photo to her 'friends' aspect
* When Bob goes to Alices' album overview, he should see an album called 'Animals'
* When Bob clicks on the album he arrives on the album page
* When Bob is on the album page, he should see a photo of Alices' dog
* When he clicks on the photo of Alices' dog, the photoviewer should appear
* And Bob should be able to comment, like, tag, etc and be a happy bob

How to deal with aspects and albums? Upload photos to albums and assign aspects to the photos. A visiting contact will only see the albums that contain at least one picture they have access to. Example:

* There's 40 photos in 'Holiday'. 35 are tagged with Family. My mum can see the album and watch the 35 photos available to her. 
* All 40 photos are tagged with 'Friends'. My friends can see the album and all the photos in it. 
* No photos are tagged with 'Losers'. The loser in my friendlist can't see the album name in the first place.

Suggestions much welcome!

##Tags
* Clean up their appearance a bit as they are serious overkill right now. No underline, no background. #DoItClean

![The current way a tag is displayed.](http://i.imgur.com/72Xtk.png)

* Add the followed tags to the right menu and let the left menu float again.
* Have a trending tags cloud in the widgetmenu. This helps new people getting started, appeals to Twitter users, incentive to return to Diaspora, get tags trending.

##Menus
Make all sections in the right menu collapse/expandable for now; turn it into a customizable widget bar in the future (iGoogle-like functionality). Also add a suggested contacts widget and allow connected apps to provide additional widgets.

![How a customizable widget bar could look like](http://i.imgur.com/bZi2F.png)

##Suggested contacts
-	Suggest people with the same tags added to their profile
-	Mutual contacts
-	Facebook friends
-	Get names from Twitter and look for them in Diaspora, output the matches
-	Have people suggest contacts

Suggested contacts should be both a widget in the right menu as well as a section on the contact page.

##Birthdays
Add birthday notifications and mail about them to make people return to Diaspora.

##Profile
Title and item fields which can be assigned to aspects and ‘public’. This will allow for the much craved ‘per aspect profiles’ and offers a lot of possibilities. Solves a big problem for now (e.g. we can finally privately put up our telephonenumber and emailadres for our true friends). Probably needs some refining in the future but we need to take a step.

##Import RSS
The ability to import RSS into your stream in order to follow blogs and site updates. This will add feedreader functionality to Diaspora. Can very well be tied in with the ‘Collections’ concept I read in the tracker.

##Hovercards
We could use these for a visit profile, message, mention and chat button, as well as for adding the contact to aspects. We can do this all over the place, making the interface a lot more consistent. Also possibly removes the need for ‘add to aspect’ buttons in places where the buttons don’t fit, like the tag page and possibly the 'suggested contacts' page.

Have hovercards on mentions too. Add a micro-bio to the card to make things more personal and stimulate contacting people found on other peoples posts.

![Extended Hovercards suggestion](http://i.imgur.com/DSOuv.png)

##Contact management
The contacts page doesn’t provide any functionality that can’t be done in an easier way elsewhere, except for doing a lot of changes on a lot of contacts. We do need the page because people will expect it to exist but it’s really got to be better. One way of massively improving the usability of the page is by adding real-time contact browsing

![Better contact management](http://i.imgur.com/Dfh7e.png)

“All contacts” is the only real feature though. Adding to aspects is already as easy as can be. Especially if it’s going to be in the hover/clickcards.

##Notifications
Show the amount of unread notifications in the title of the page.

##Homepage
A lot of people wonder whether Diaspora still exists and whether it’s actually being used. It would be nice if we could have a stream of public posts on the homepage of the pod, to show that Diaspora is alive and kicking. Also helps with finding contacts. Improves the first impression of potential users and gives a clear signal to the existing users that public really means public.

There should also be some general information about Diaspora, shipped with the pod. It should list some of the features we already have, some that will come soon. Tell people it's decentralised like e-mail and that they can talk to eachother whichever server they use.

##Reshare
This feature would be the bomb! Only for public posts. Reshare the post with the avatar and name of the owner, let the owner be able to withdraw the post and all of its reshares. It’s already on a branche so I hope it could be finished.

##Change emailadress
This is a very basic and much needed feat.

##More networks!
We need to be able to post to more networks. Show people that we connect to everything and that they can use Diaspora for posting to all sorts of accounts.

##Importing tweets/Facebook stream
Imagine all your friends are on Facebook/Twitter and you are locked in. You get a Diaspora invite and you can follow your fb stream/tweets through Diaspora. You start using Diaspora and invite a few close friends. They join and can still see fb and twitter. You start communicating on Diaspora as well and activity on other networks goes down. Eventually you will switch altogether. Evolution WIN.

##Likes in comments
Title says it all. New users are going to love seeing that.

##Contacts search
Share the user index of a pod with all other pods in the network to improve the people search functionality.

  - **untitaker:** I think they are already doing that.

##Find people in your area
Can we do this? Would be interesting to see if anyone in your area is on Diaspora.

##Minor things
  - **untitaker:** links to posts in the notifications dropdown should make the page scroll to the post (if possible), instead of opening it.
  - **untitaker:** There are too much icons beside the searchbar; At least the home-icon is not needed.
  - **untitaker:** User menu (#user_menu) is a bit empty, some stuff (eg. from the right sidebar) could be moved in there.

##Diaspora UX testing with noobs
###untitaker's sister
**untitaker:** Okay, here are the results of the Diaspora-test with my sister:

  - **There really needs to be an aspect dropdown beside the share button on the main site.** She was unable to figure it out how to use the aspects sidebar and expected the aspects list to be beside the share button.

  - **Inserting tags is unintuitive as fuck.** While signing up, she tried to insert the tags without a hash as prefix, separated with commas. I'd suggest to make the tags field a simple input, without that clutter around, and, if neccessary, without autocompletion.

  - **Doubled entries in searchbar.** More a bug than a UI lack, but still it was very annoying and confusing to her.

  - [This bug](http://bugs.joindiaspora.com/issues/1167) confused her, but didn't stop her.

*Note: I made this test with a pre-opened browser and my own PC, otherwise it would've been very likely that my sister opened Internet Explorer.*

###Zulu's sisters
Confusion:
  - hashtags ("What are they? They make messages hard to read.")
  - the fact that handles are not the same thing as emails (ie: that you cannot email someone at their handle -- "Then why do you have the '@' sign there?")
  - why there is no "wall"-type feature. 

Frustration:
  - again, the lack of albums, groups, event pages, and the ability to tag friends in pictures.