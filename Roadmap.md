# Diaspora Roadmap and Wishlist

You can see what we are currently working on [[here, on Diaspora’s Tracker|http://www.pivotaltracker.com/projects/61641]]. Tracker is where we keep our most immediate priorities, and is a good indication of what we are going to be working on in the next 2-3 weeks.

Our roadmap is the mission critical features that given enough time, we will develop them ourselves.  You are welcome to try and tackle one of these problems, and it would be awesome if you checked Tracker and the network tree to make sure no work is being duplicated.

## Already integrated
-Upload Images
-Realtime Posts

## Near Future
- Data portability: allow people to Oauth to a new seed and move their entire account to the new seed, and then notify all of their contacts of the change, so people can move around seamlessly.
- Services integration:  We are going to be focusing on public publishing to Facebook, Twitter, and OStatus enabled public sites, as well as being able to get your existing friends lists to help you find them on Diaspora.
- Internationalization using I18n: We need to move our text out to locale files so that we can get Diaspora translated.  It would be a shame if you couldn’t use Diaspora in Greek.
- In order to scale app-servers horizontally, we need to break the websocket server into a different process and store its session info in the database.
- Server to server authentication:  Right now Diaspora is push-only.  We need servers to be able to authenticate to each other in order to pull data in, and to delegate that authentication to the browser to avoid replicating large files like photos.
- Refining aspects (adding people to multiple groups, having people only in the public group, etc) 

## Medium Priority
- More Standards compliance: we believe in working code before standards, but being standards compliant is very important to us.  We need to bust our own chops on this one, and If you see any low hanging fruit, by all means let us know, and we will get to it when when it makes sense with a feature.
- Private messaging
- Events
- Photo tagging
- Mentions, responses to any type of message with any other.
- Unified notification system - rather than basing streams of content object themselves, make it more around a separate “notification” object created with each object.  also, open to any good ideas on how to make this happen.  With ActionMailer Support
- Software Update Framework
- Some sort of an administrator interface that would do the entire update automatically, something like the way Wordpress can be updated with the click of a button.

## Wishlist
- These are things which would be nice to have, and are not currently in our plan, but we would love to have them.
- An “undo” library for Jquery that gives a timeout on posting forms, so you can make an OOPS on a status message before sending it out to your friends. (see gmail undo)
- Asynchronous  webfinger client in ruby, deprecating Redfinger or modifying it to use EM:HttpRequest instead of RestClient
- Full activity streams parser and sterilizer, based off the spec, which gives you ruby objects for valid streams (the easy solution would probably make methods in our models to_activity and from_activity)
- Private pubsubhubbub implementation, check out both the “from” and “oauth” authentication methods. (note maxwell: i may have some code to dump onto the Internet, i need to write some more tests)
- Double checking our salmon implementation.... is it up to spec?
- Test test tests: we have a fair amount, we’re sure there are plenty more that are missing.  Rspec, Cucumber, whatever.  If you wrote tests, we would love you forever.
- Selenium tests that hit every page would be super nice. The framework for this is started and can be found in test/selenium
- Support for Devise(master) for MongoMapper (We’re currently using BadMinus’ fork)
- Support for CarrierWave(master) for MongoMapper (We’re currently using Raphael’s fork)
- Option to have a performant data on disk encryption
- Make our MessageQueue abstracted from the rest of the application, so any queue can be plugged in and used.

- A DHT (distributed hash table) to serve as a directory of Diaspora Users/seeds installed over the internet to help with discovery.
- enhance our websocket controller: perhaps break it out so other rails projects ? have a framework for using it?
- Chat client integration using web sockets?
- Other crazy Websocket experiments: We are just using it to push data to the client, but can we use it to connect people or two seeds for real time games?

- Javascript compatible view templates (handlebars.js looks promising)

- Taxonomy of social types: creating interfaces for all of the activity stream types, so people could make and send their own types on the file in between diaspora seeds. could be tied to the parser/generator?

- An object oriented ruby wrapper for libgcrypt
 - - We initially used gpg for encryption, but the tight binding it has to filesystem config folders made it untenable. We would like to be able to use libgcrypt, but without an object oriented ruby wrapper, it would be too time consuming.

- Running Diaspora from home:
- - Guide for setting up a diaspora instance on your home computer, like [GNU Social’s](http://foocorp.net/projects/fooplug/)
- - Option to have a performant data on disk encryption
- - Windows/Linux/Mac installers/ and intergration with Dynamic DNS services
- - Can we run Diaspora from a Nexus One?

- In general call us out with more elegant solutions