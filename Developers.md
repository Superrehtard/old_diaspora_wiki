Below is an index of Developer-centric pages and resources. Keep in mind that this is still being re-worked, and will be further cleaned up in due time.

## [[Developer Workflows]]
These are guidelines that show how the different tools in the community are used. As we're a Community-Driven project now, it's important to follow these.

## [Contributor License Agreement](New-CLA--12-13-10)
The Diaspora CLA is a helpful agreement between Diaspora Inc, the core developers, and any community contributors. It states the scope of community open-source licenses used within the project, and the nature of the relationship between the core developers and community contributors  [You can sign it here](https://spreadsheets.google.com/a/joindiaspora.com/spreadsheet/viewform?formkey=dFdRTnY0TGtfaklKQXZNUndsMlJ2eGc6MQ)

## [FAQ for Developers](FAQ-for-Developers)
Answers to some common questions asked by developers interested in getting started to work with the Diaspora platform.

## [Getting Started](Getting-Started-With-Contributing)
Everything you need to know about getting started. This is a great little resource hub for people who are just starting out, and just about to contribute to the DIASPORA* project for the very first time.

## [Diaspora Technical Resources](Technical-Details)
This page contains some of the more advanced semantic information pertaining to how Diaspora works as a platform, with overviews to the different working parts.

## [API Documentation](API-Documentation)
For those interested in contributing through code, here's documentation on the Rails repository.

## [Federation Logger](Federation-Logger)
Here you'll find how to set up and use the federation logger to help us debug communication issues and improve our server protocols.

## [Mobile/Third Party App Developer Resources](Mobile-and-Third-Party-Developer-Resources)
This is a collection of resources containing information about mobile clients, and plugins for different Content Management Systems.

## [Development Team Wishlist](Developer-Feature-Wishlist)
A wishlist of features we'd like help with working on. Feel free to pick one up and join in.


## New things to  be worked on

things needed to be fixed at hand.. started from a conversation between @raven24 and @maxwell

evil query
.... use the scope_operators from makrio
https://github.com/makrio/makrio/blob/master/config/initializers/scope_operators.rb
(btw. the amount of inline code comments is probably not TOO DAMN HIGH# enough - I think we really want to get the wisdom inside/next to the source, rdoc for (most of) the code might be helpful for new devs...)
get some SQL wisdom in there (... what else did I do those two Database-courses for?)

→ so, maybe a walk-through of the evil query parts might help (soon-ish)


Batch stuff
using find_each more
http://apidock.com/rails/ActiveRecord/Batches/ClassMethods/find_each
-> possibly together with resque/sidekiq switch
- receive loop
- sending loop


Federation and stuff
make receive be runnable again and again
encapsulate into objects and separate into layers
(throw a HTTP request at the “federation layer” and out comes normalized XML/JSON - or the other way around)
notifications.... de-fuckify them
- don’t try to use more magic than we actually can handle. better more verbose code than a mess that causes problems 
- completely decouple them from federation state.
move federation to json?  Maybe start over?  general theme of trying to think of discrete phases?
- decouple database layout from the protocol messages structure
object serialization.
	- be liberal in making multiple ruby classes
	- serialize to an in-memory presenter, which dumps to an ORM object
	- allow db state to reach across in memory objects

1. receive message
2. validate envelope, parse json, put in queue	 => return actual response
...


Background Processing
resque => sidekiq
advantage: Threads instead of Processes, less memory more fun
potential job for junior-devs, API looks similar


Rails upgrade
- gems
- security!