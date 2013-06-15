----

###403 DO NOT MOVE###

We're currently **moving this wiki over to our new project site**. The contents of this page are not important enough to be ported over because they are either very old, very outdated or wrong and misleading. 

----

# Changelog

This page contains a list of some of the more notable changes that happened in our codebase.  
For a full list of changes, please see the [git commit log](https://github.com/diaspora/diaspora/commits/master).

### For a summary of all our other latest and greatest changes, see the [Changelog file](https://github.com/diaspora/diaspora/blob/master/Changelog.md). Please go there for a more recent overview.

## 2012

### July
* Update jQuery MentionsInput plugin to latest upstream version
* Mobile toggle now works with desktop browsers, too
* Flash messages are visible again (CSS fix)

### June
* Upgrade to Rails 3.2.6
* New error page indicating a limited post being accessed when not logged in
* Uploaded images are rotated correctly based on Exif data
* Visual improvements to the oembed overlay
* [SECURITY] fix vulnerability to malicious federated messages

### May
* 'Back-to-top' button comeback
* Performance improvements for new post show pages
* Beta profiles now feature background images
* Refactoring of email generation and templating
* Upgrade to rails_admin 0.0.3
* Rake task for fixing mixed-case hashtags

### April
* New quick-setup script to get started with a dev-installation
* Bootstrap is now unified into one version
* Update to Rails 3.1 + new asset pipeline
* Update Backbone.js to 0.9
* Major refactoring in the JS view templates and the config file
* Photo stream comeback - fully Backbone-ified
* Improvements in embedding 'rails_admin'
* New 404 'Not found' page
* First appearance of the role system

### March
* Allow non-ascii characters in links
* Notifications on mobile web app
* [Invitations](https://groups.google.com/d/msg/diaspora-dev/e6IOYMlwwbE/S2HcZpqpKIEJ) are now done by sharing a user-specific link, the amount of invites on that link is configurable
* Youtu.be and Vimeo embedding is now possible
* oEmbed video content is now shown as thumbnail instead of directly embedding the player in the stream
* Update Bundler version to 1.1.0

### February
* Participate and Explore streams implemented.
* Status updates can now be "Pinned" rather than "Liked"
* Layout adapted to better fit "Conversation-Centric" design.
* Links now open in a new window by default
* Publisher/Bookmarklet now works as intended
* Messages now retain navigation when opened in new tab

### January
* Pods run via SSL by default 
* Stream rewrite with Backbone.js and client side Markdown rendering for faster UX and 400x better garbage collection server side

## 2011

### November
* Ignore user

### October
* Longer posts and comments are truncated with a "show more" link to reveal the rest of the text
* Stream can now include posts from all followed tags and @mentions as well as aspects (formerly known as "soup")
* @Mentions stream to see all posts you were mentioned in
* #Followed tags stream that aggregates posts for all of your followed tags
* Community spotlight stream
* Embed videos and audio into posts (using oembed)
* De/select all aspects and improvements to aspect de/selection
* Alphabetized tags list
* Add tags directly from the followed tags list, rather than having to search for them first
* Reshare on mobile web app
* Tag pages now indicate how many people are following a tag

### September
* Mobile web app redesign
* Database improvements (scaling)
* Reshare notifications
* Aspect dropdown in the publisher, showing clearly which aspects are receiving a post

### August

* Featured users
* DiasporaHQ account
* New Getting Started page

### July

* General UI redesign and cleanup.
* Notifications partially moved in a handier dropdown in the header.
* Likes use ajax now.
* Ability to "follow" (subscribe to) hashtags.
* Reshare feature

### June

* Revamped login page
* Facebook connection now working
* Added posting to Tumblr
* You can now view more than 15 people when searching for a tag
* Revamped (simplified) contacts management interface: Replaced aspects manage page with contact list page.

### May

* Ability to like a post
* Revamped the notification page
* Share model [[http://groups.google.com/group/diaspora-dev/browse_thread/thread/cbc9497ac63a0051]]
* "Show more" functionality and increased post length to 10,000 characters
* [[http://cubbi.es]] integration

### April

* Bookmarklet for posting from any site
* Added ability to hide posts
* Lots of backend improvements

### March

* Private messaging
* Hashtags
* Larger photos in stream
* GIFs can now be uploaded
* Comments can be deleted

### February

* Added the ability to view posts by activity or post time
* Added admin interface
* Header remembers which aspects are selected between sessions
* Various bug fixes
* Added the ability to mention a contact in a post by using @ 

### January

* Added autocomplete in search bar
* Added new login page
* Added a mobile interface
* Added ability to post to multiple aspects from the header

## 2010

### December

* Notifications for share requests and comments 
* Add a new line in a post by hitting Enter/Return
* Added ability to turn off email notifications
* Edit aspects a person is in from their profile page
* Ability to remember username and password on login page
* Infinite scroll