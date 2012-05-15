Below is a curated feature wishlist of things **the core team** love to have some help with for the Diaspora project. 

**If you choose one these features, a core team member will help you get your pull request merged**.  It is a fast track way to get involved with the Diaspora* codebase.

**Community developers are more than welcome to dive in and work on these at any time, please [contact the mailing list](https://groups.google.com/forum/?fromgroups#!forum/diaspora-dev) about any of the items listed below that you'd like to help with. Also, don't be afraid to join in and help out other developers with existing projects**

"In Development" items will be linked with a thread to Google groups, so as to encourage collaboration and avoid duplicating effort.

### 2) [Refactor AppConfig and EnviromentConfiguration.(In Progress)](https://groups.google.com/forum/?fromgroups#!topic/diaspora-dev/XcWTJn-IsUY)
AppConfig is the current class that handles loading Diaspora’s settings from application.yml.  It has grown into a bit of beast, worring about bootstate, data munging, and a few related helpers.  The time to clean it up is now!  We should move all logic out of AppConfig, and let it just be a SettingsLogic class.  Then, we should access everything via EnviromentConfiguration, delegating applicable keys and to AppConfig.

### 3) Help make things easier for other developers
Many projects have a script which makes it trivial to get a development enviroment running.  It would be awesome to have a script that bootstraped the minimum enviroment needed to run the entire test suite. That way, it would be easy for new developers to clone the repo, run the script, and run the tests, all in one swoop.  As a start, which should make this process seamless for the following three enviroments(then we can work on something more general.
   5) mac os x
   7) ubuntu development	
   8) virtual Box Setup(we can host the final box)


### 4) Refactor Cucumber Features:  
See these(https://github.com/diaspora/diaspora/blob/master/features/post_viewer.feature)?  Now see how much worse this is?( https://github.com/diaspora/diaspora/blob/master/features/edits_profile.feature). A lot of our cukes are messy, it'd be great if someone with a great sense of code clarity were to step up to the plate and refactor some of the features. If you want to go the extra mile, work with dennis and Identify tests that would be better in jasmine, or unit tests to shave some time off the suite as well.

### 5) Refactor controllers
There are a bunch of things a seasoned Rails dev would want to do here.  Utilize more rescue_froms to reduce branching, breaking really big actions into objects that are delegated to, to just improving and backfilling tests.  This is not the most glamorous of things on this list, but it would be much appreciated.

### 6) Improve admin interface  
We already have rails admin running in a branch (link). It might still need some configuration tweaking. We could make a couple of static pages, and move some of our custom functionality to the rails_admin side of things. Other ideas are basic webfingering other pod dev tools, showing the list of connected pods, and adding links to things like resque status etc etc.

### 7) Pod Customization [(In Progress)](https://groups.google.com/forum/?fromgroups#!topic/diaspora-dev/i4_wvLQaZJ8). 
We're discussing how to turn some variables into strings to make pod customization easier for podmins without needing to fork from the main codebase. It's a great starting point for figuring out how further to customize a pod.

### 8) Save image dimensions with every image file 
needs to upgrade carrier-wave, and include a hack in the carrierwave wiki.

### 8b) related, fixed delayed image processing BS.  
Ask Max for more info.

### 9) Welcome email 
After a user has signed up, email them with helpful information.

### 10) [JavaScript to add tags](https://groups.google.com/forum/?fromgroups#!searchin/diaspora-dev/ktkaushik/diaspora-dev/hGBWPxwAEQQ/HjpwIK-KYlwJ)
Adding a tag under the ``/followed_tags`` should be using JavaScript to do so. It would be cool to have that feature. Also, considering the fact that deleting a tag is already using one, we think it should be in symmetry. 

### 11) [Clean out Images folder (In Progress)](https://groups.google.com/forum/?fromgroups#!topic/diaspora-dev/GkZTsDyTZCw) 
There's a lot of unused images in there that we don't need, so it kind of makes for a waste of space.

### 12) Carrier-Wave Improvements 
Make Carrier-Wave upload to a /tmp directory for tests, and clean up when done.

### 13) Improve Mobile Site
Currently, our mobile site is relatively simplistic, and could use some tweaking to be made more accessible. A discussion needs to happen to figure out what new features would be desirable.

### 14) Add Tests to [This Pull Request](https://github.com/diaspora/diaspora/tree/xray7224-adds-lang-url-param)
This was contributed to us by a user. It's a relatively simple fix, but the contributor was unable to write tests for it. Let's help this person out and make sure that we can get it merged in!

### 15) Document the code.
Please document the existing code and also consider documenting your code as much as you can when you send us a pull request. It helps others who fork the repository to understand and work on it.