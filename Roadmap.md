# Diaspora Roadmap and Wishlist

You can see what we are currently working on [[here, on Diaspora’s Tracker|http://www.pivotaltracker.com/projects/61641]]. Tracker is where we keep our most immediate priorities, and is a good indication of what we are going to be working on in the next 2-3 weeks.

Our roadmap is the mission critical features that given enough time, we will develop them ourselves.  You are welcome to try and tackle one of these problems, and it would be awesome if you checked Tracker and the network tree to make sure no work is being duplicated.

## Already integrated
- Share status messages and photos privately and in near real time with your friends through “aspects”.
- Friend people across the Internet no matter where Diaspora seed is located.
- Manage friends using “aspects”
- Upload of photos and albums
- All traffic is signed and encrypted (except photos, for now).
- Change password

## Near Future
- Data portability: allow people to Oauth to a new seed and move their entire account to the new seed, and then notify all of their contacts of the change, so people can move around seamlessly.
- Services integration:  We are going to be focusing on public publishing to Facebook, Twitter, and OStatus enabled public sites, as well as being able to get your existing friends lists to help you find them on Diaspora.
- Internationalization using I18n: Now that we have locale files, we need to set up our controllers to use the browser's language header.  It would be a shame if you couldn’t use Diaspora in Greek.
- Server to server authentication:  Right now Diaspora is push-only.  We need servers to be able to authenticate to each other in order to pull data in, and to delegate that authentication to the browser to avoid replicating large files like photos.
- Refining aspects (adding people to multiple groups, having people only in the public group, etc) 

## Medium Priority
- More Standards compliance: for instance, we have a salmon implementation, but it's not tested against the spec yet.  It should be made compliant with the salmon spec.
- Private messaging
- Events
- Photo tagging
- Mentions, responses to any type of message with any other.
- Unified notification system - rather than basing streams of content object themselves, make it more around a separate “notification” object created with each object.  also, open to any good ideas on how to make this happen.  With ActionMailer Support
- Software Update Framework
- Some sort of an administrator interface that would do the entire update automatically, something like the way Wordpress can be updated with the click of a button.

## Wishlist

These are things which would be nice to have, and are not currently in our plan, but we would love to have them.

### Communication

- CAPTCHA based Spam prevention:

  We need a way to prevent a malicious server from spamming another server with requests.  A server could respond to a request from an unknown location with a captcha, possibly as described below:
  1. The requester sends the friend request.
  2. The untrusting receiving server returns some sort of notice of a challenge, possibly the url of a captcha image, or whatever would be needed to use a third party like recaptcha.  [it would be great to have this generalized so that any kind of CAPTCHA (and eventually any kind of challenge can be used). Eventually, it would be excellent to adjust continues dynamically either based on the number of requests or if the two pod operators trust each other to prevent sign up spam maybe this would be an excellent use for *MonkeySphere*]
  3. The requesting server conveys the challenge to the requesting user.
  4. The receiving server verifies the response and processes the request.

- An “undo” library for Jquery that gives a timeout on posting forms, so you can make an OOPS on a status message before sending it out to your friends. (see gmail undo)
- Asynchronous  webfinger client in ruby, deprecating Redfinger or modifying it to use EM:HttpRequest instead of RestClient
- Full activity streams parser and sterilizer, based off the spec, which gives you ruby objects for valid streams (the easy solution would probably make methods in our models to_activity and from_activity)
- Private pubsubhubbub implementation, check out both the “from” and “oauth” authentication methods. (note maxwell: i may have some code to dump onto the Internet, i need to write some more tests)

- A DHT (distributed hash table) to serve as a directory of Diaspora Users/seeds installed over the internet to help with discovery.
- enhance our websocket controller: perhaps break it out so other rails projects ? have a framework for using it?
- Chat client integration using web sockets?
- Other crazy Websocket experiments: We are just using it to push data to the client, but can we use it to connect people or two seeds for real time games?

- Support for textile/markdown/etc in comments and messages (#issue119)


### Development

- Double checking our salmon implementation.... is it up to spec?
- Test test tests: we have a fair amount, we’re sure there are plenty more that are missing.  Rspec, Cucumber, whatever.  If you wrote tests, we would love you forever.
- Selenium tests that hit every page would be super nice. The framework for this is started and can be found in test/selenium
- Support for Devise(master) for MongoMapper (We’re currently using BadMinus’ fork)
- Support for CarrierWave(master) for MongoMapper (We’re currently using Raphael’s fork)
- Option to have a performant data on disk encryption
- Make our MessageQueue abstracted from the rest of the application, so any queue can be plugged in and used.

- An object oriented ruby wrapper for libgcrypt
  - We initially used gpg for encryption, but the tight binding it has to filesystem config folders made it untenable. We would like to be able to use libgcrypt, but without an object oriented ruby wrapper, it would be too time consuming.

### Deployment

- Running Diaspora from home:
  - Guide for setting up a diaspora instance on your home computer, like [GNU Social’s](http://foocorp.net/projects/fooplug/)
  - Option to have a performant data on disk encryption
  - Windows/Linux/Mac installers/ and intergration with Dynamic DNS services
  - Can we run Diaspora from a Nexus One?

- In general call us out with more elegant solutions


### Other
- Javascript compatible view templates (handlebars.js looks promising)
- Taxonomy of social types: creating interfaces for all of the activity stream types, so people could make and send their own types on the file in between diaspora seeds. could be tied to the parser/generator?
