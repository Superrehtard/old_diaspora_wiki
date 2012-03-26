Below is a feature wishlist of things we'd love to have some help with for the Diaspora project. Community developers are more than welcome to dive in and work on these at any time, please [contact the mailing list](https://groups.google.com/forum/?fromgroups#!forum/diaspora-dev) about any of the items listed below that you'd like to help with. "Claimed" items will be linked with a thread to Google groups, so as to encourage collaboration and avoid duplicating effort.

**1) Fix and refactor emails.** 
There are tons of duplication, lack of a sane simple email template, and the divide between HTML and text is huge.  We should strip them down to a more basic single template and remove much of the ugly HTML stuff in the invite et. all we were including. Also, it would be awesome to give some due dillegence to figuring out what kinds of data we link in each email, and make it consistent across the board.

**3) Refactor AppConfig and EnviromentConfiguration.**
AppConfig is the current class that handles loading Diaspora’s settings from application.yml.  It has grown into a bit of beast, worring about bootstate, data munging, and a few related helpers.  The time to clean it up is now!  We should move all logic out of AppConfig, and let it just be a SettingsLogic class.  Then, we should access everything via EnviromentConfiguration, delegating applicable keys and to AppConfig.

**4) Get [Spork](https://github.com/sporkrb/spork)  running with our test suite** 
+1 if you also get guard running in OSX and linux too.

**5) Help make things easier for other developers**
Many projects have a script which makes it trivial to get a development enviroment running.  It would be awesome to have a script that bootstraped the minimum enviroment needed to run the entire test suite. That way, it would be easy for new developers to clone the repo, run the script, and run the tests, all in one swoop.  As a start, which should make this process seamless for the following three enviroments(then we can work on something more general.
   5) mac os x
   7) ubuntu development	
   8) virtual Box Setup(we can host the final box)


**6) Refactor Cucumber**:  
See these(https://github.com/diaspora/diaspora/blob/master/features/post_viewer.feature)?  Now see how much worse this is?( https://github.com/diaspora/diaspora/blob/master/features/edits_profile.feature) 
It would be awesome if someone could refactor some of our test suite to relay less on the bad built in pre-defined Cucumber steps, and work towards making a set of steps and helpers which reflect Diaspora’s domain language.

**7) Refactor controllers** 
There are a bunch of things a seasoned Rails dev would want to do here.  Utilize more rescue_froms to reduce branching, breaking really big actions into objects that are delegated to, to just improving and backfilling tests.  This is not the most glamorous of things on this list, but it would be much appreciated.

**8) Improve admin interface**  
We already have rails admin running in a branch (link). It might still need some configuration tweaking. We could make a couple of static pages, and move some of our custom functionality to the rails_admin side of things. Other ideas are basic webfingering other pod dev tools, showing the list of connected pods, and adding links to things like resque status etc etc.

**9) [Branding pack (Claimed)](https://groups.google.com/forum/?fromgroups#!topic/diaspora-dev/pP0LTD2Fpms).**  
Make it really easy and well understood what assets can/need to be changed if you want to throw up your own customized pod.(focus on minimum needed right now, rather than a full blown theme framework as of yet)

**10) Save image dimensions with every image file** 
needs to upgrade carrier-wave, and include a hack in the carrierwave wiki.

**10b) related, fixed delayed image processing BS.**  Ask Max for more info.

**11) Welcome email** 
After a user has signed up, email them with helpful information.

**12) Update all references to bootstrap to be unified (use a gem?)**
	- rails 3.1 means we can include this stuff via asset pipeline and get code out of the main repo
	- this means slaying the two(or three?) different versions we are using across getting started, mobile, and the show pages.

**13) Upgrade ActsasTaggable to be inline with master**
 plus rafi’s fixes to get rid of deprecation warnings.  Raphael forked it awhile back, but we want to try and get our small amount of fixes up to date with the latest gem release to remove deprecation warnings.