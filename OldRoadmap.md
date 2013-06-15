----

###403 DO NOT MOVE###

We're currently **moving this wiki over to our new project site**. The contents of this page are not important enough to be ported over because they are either very old, very outdated or wrong and misleading. 

----

# Diaspora Roadmap

# This is just a placeholder for the old roadmap, in case anything needs to be moved off of it to the new one.

## The Next Few Weeks

You can see what the core team is currently working on in <a href="http://www.pivotaltracker.com/projects/61641" target="_blank">Diaspora’s Pivotal Tracker project</a>. 
Tracker is where we keep our most immediate priorities, and it's a good indication of what we 
are going to be working on in the next 2 or 3 weeks.

You can also look at the [branches](https://github.com/diaspora/diaspora/branches) we have in our git-repo, to see how actively a feature is being developed.

## And Beyond...

These are things that are in our plan, but haven't yet come to the front of the priority queue. 
If you're looking for a feature to implement, and any of these strike your fancy, come talk to 
us in IRC first. We may have thoughts about how we want these implemented, and we can help you 
get started.

- Data portability: allow people to Oauth to a new seed and move their entire account 
to the new seed, and then notify all of their contacts of the change, so people can move 
around seamlessly.
- Twitter-style API for posting and searching public posts.
- Services integration: attach services to particular aspects.
- Server to server authentication:  Right now Diaspora is push-only.  We need servers to 
authenticate with each other to pull data in.
- Refining aspects - having people only in the public group, a Twitter-style follow model, etc. 
- More standards compliance: for instance, we have a salmon implementation, but it's not tested 
against the spec yet.  It should be made compliant with the salmon spec.
- Events
- Photo tagging
- Mentions and tags should work in comments and captions.
- Software Update Framework for folks running their own pod.
- An administrator interface that would do the entire update automatically, like the way 
WordPress can be updated through its admin interface.

## Wishlist
These are things which would be nice to have, but are not currently in our plan. If you're looking
for a feature to implement, and any of these strike your fancy, come talk to us in IRC and we'll 
help you figure out how to get started. 

### Communication

- CAPTCHA based Spam prevention:

  We need a way to prevent a malicious server from spamming another server with requests.  A server could respond to a request from an unknown location with a captcha, possibly as described below:
  1. The requester sends the friend request.
  2. The untrusting receiving server returns some sort of notice of a challenge, possibly the url of a captcha image, or whatever would be needed to use a third party like recaptcha.  [it would be great to have this generalized so that any kind of CAPTCHA (and eventually any kind of challenge can be used). Eventually, it would be excellent to adjust continues dynamically either based on the number of requests or if the two pod operators trust each other to prevent sign up spam maybe this would be an excellent use for *MonkeySphere*]
  3. The requesting server conveys the challenge to the requesting user.
  4. The receiving server verifies the response and processes the request.

- GPG public/private key + Key signing = Global keys with social keysigning :D
- An “undo” library for Jquery that gives a timeout on posting forms, so you can make an OOPS 
on a status message before sending it out to your friends. (see gmail undo)
- activity streams parser and sterilizer, based off the spec, which gives you ruby objects for 
valid streams. The easy solution would probably make methods in our models to_activity and 
from_activity.
- Private pubsubhubbub implementation, check out both the “from” and “oauth” authentication methods.
(Note from Maxwell: I may have some code to dump onto the Internet...I need to write some more tests.)
- A DHT (distributed hash table) to serve as a directory of Diaspora Users/seeds installed over the 
internet, to help with discovery.
- Either make websocket controller less terrible or stop using it: perhaps break it out so other 
rails projects have a framework for using it?
- Chat client integration using web sockets? (using jabber might be easier)
- Other crazy Websocket experiments: We are just using it to push data to the client, but can we use 
it to connect people or two seeds for real time games?
- Test test tests: we have a fair amount, we’re sure there are plenty more that are missing.  Rspec, Cucumber, whatever.  If you wrote tests, we would love you forever.
- Selenium tests that hit every page would be super nice. The framework for this is started and can be found in the features directory.
- Javascript compatible view templates (handlebars/mustache looks promising)
- Taxonomy of social types: creating interfaces for all of the activity stream types, so people could make and send their own types on the file in between diaspora seeds. Could be tied to the parser/generator?

### Deployment
- Running Diaspora from home:
  - Guide for setting up a diaspora instance on your home computer, like [GNU Social’s](http://foocorp.net/projects/fooplug/)
  - Option to have a performant data on disk encryption
  - Windows/Linux/Mac installers and intergration with Dynamic DNS services (e.g. PageKite)
  - Can we run Diaspora from a Nexus One?

## In general

Call us out on existing code that's ugly or untested (or both).