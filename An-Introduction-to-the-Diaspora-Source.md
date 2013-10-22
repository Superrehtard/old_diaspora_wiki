##Framework and Tools:

Diaspora is written in **[Ruby on Rails][ror]**, a web framework for the [Ruby programming language][ruby].  
If you’ve never looked at a Rails project, you may want to check out a [Rails guide like this one][ror-getting-started].

There are a few tools we’re using that aren’t in every rails project: 

* **Haml**  
  Our view templates are written in HAML (a templating language) instead of the default ERB (HTML with inline Ruby code).  
  The HAML docs can be found [here][haml]. The corresponding files under `app/views`.
* **Sass**  
  Our CSS is written in [SASS] (specifically, the 'scss' dialect), which generates the actual CSS 
  via the [Rails asset pipeline][asset-pipeline].
  The syntax is inspired by CSS, and quite similar, but it offers some additional features like nesting and variables.
  If you want to edit the stylesheets, have a look in `app/assets/stylesheets/`.  
  ***Note:*** Both HAML and SASS are whitespace sensitive.
* **Backbone.js & Handlebars.js**  
  The client-side functionality and rendering is mostly coordinated with [Backbone.js][backbone], 
  which communicates [REST]-fully with the server with JSON and triggers the rendering of the 
  [Handlebars.js][handlebars] templates. The logic is found in `app/assets/javascripts/app` and the Handlebars
  templates are located in `app/assets/templates`

[ruby]: http://www.ruby-lang.org
[ror]: http://rubyonrails.org/
[ror-getting-started]: http://guides.rubyonrails.org/getting_started.html
[asset-pipeline]: http://guides.rubyonrails.org/asset_pipeline.html
[haml]: http://haml-lang.com/docs.html
[sass]: http://sass-lang.com/docs.html
[backbone]: http://documentcloud.github.com/backbone/
[rest]: https://en.wikipedia.org/wiki/Representational_state_transfer
[handlebars]: http://handlebarsjs.com/

##Testing:

Our goal is to test *everything*. If you find a bug, you first expose it by writing a tests that fails because of the bug. Only then you start fixing the actual code. This is called [Test Driven Delopment][tdd] (TDD).  
We write our unit tests for ruby code in [Rspec], the JavaScript test are in [Jasmine] and integration tests in [Cucumber]. Specs are in `spec`, and Cucumber features are in `features`. For more details see our page on [[Testing workflow]].

[tdd]: https://en.wikipedia.org/wiki/Test-driven_development
[rspec]: http://blog.davidchelimsky.net/2007/05/14/an-introduction-to-rspec-part-i/
[jasmine]: http://pivotal.github.com/jasmine/
[cucumber]: http://rubylearning.com/blog/2010/10/05/outside-in-development/

##The Models:

Our Models can be found in the `app/models` folder:

User – Users, of course, come first.   A User object represents the private information and capabilities of a user on that server.  The user object is able to friend people, post updates, and update his profile.  A User has a Person.

Contact -- Is a "proxy" object for every person a User is friends with.

Person – A Person is a User viewed from the outside.  When a user friends another user, they friend that user’s Person object.  Person objects are replicated across servers, and they are where a User’s public key lives.  A Person has many Posts.  A Person has a Profile.

Profile – This contains information about the person. Currently, a profile looks the same to anyone looking at it.

Request – This is a friend request object that gets sent to another person.

Aspect – This contains a list of people, and posts which are for that aspect.  Aspects are private to Users, and we might embed the Aspect documents in the User document.

Post – A Post belongs to a Person.  This is a parent class for different types of posts, it contains comment ids and a few other attributes common to all Posts.

- Status Message inherits from Post
- Album inherits from Post
- Photo inherits from Post

Comment – a comment belongs to a Post

Retraction – this is an object that gets sent out when a post creator deletes a post.  It is not a model, but it serializes for dispatch to other Diaspora servers the same way our models do.


####Posting something (app/models/user):

1) When a user posts anything, he/she posts it to an aspect or all aspects

2) Assuming the post is valid, the post created and its id is stored in raw_visible_posts for that user

3) The html for that post is rendered on the server and is pushed to the user through the websocket

4) The post is then serialized to xml, wrapped in an encrypted and signed salmon envelope and POSTed to the receive urls(“http://pod.location/receive/users/:id[person_id]”) for the recipient people.
  
####Receiving a post (app/controllers/publics_controller.rb & lib/diaspora/user/receiving.rb ):

1) The user receives the salmon, decrypts the headers.

2) If the signature on the salmon data is from the person who claims to have sent the post is marshaled into an object and saved into the database.

3) That post id is stored in the visible posts for the receiving user as well as posts for the aspect the sender is in.


Here's the autogenerated documentation  http://rubydoc.info/github/diaspora/diaspora/master
