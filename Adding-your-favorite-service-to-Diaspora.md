----

###301 MOVED PERMANENTLY###

We're currently **moving this wiki over to our new project site**. The contents of this page have  already been carried over, so _any new changes here will not be reflected in the new wiki_.  
New link: http://wiki.diasporafoundation.org/Adding_Additional_Services

----

0) Read the git workflow guide: [[https://github.com/diaspora/diaspora/wiki/Git-Workflow]]

1) Add Friendster(*your favorite service*) support to omniauth: [[http://github.com/intridea/omniauth]], if its not there already . 

1.5) Add the example for the keys: [[http://github.com/diaspora/diaspora/blob/master/config/diaspora.yml.example]],
add empty default keys and set the enable to false in [[http://github.com/diaspora/diaspora/blob/master/config/defaults.yml]]
and one line in the initializer:
[[http://github.com/diaspora/diaspora/blob/master/config/initializers/omniauth.rb]]

2) Test drive your stuff. Ex.: [[https://github.com/diaspora/diaspora/blob/master/spec/models/services/facebook_spec.rb]] 
Write another service model, telling the app the api call that needs to happen to post a message (and how to do a friend finder). Ex.: [[http://github.com/diaspora/diaspora/blob/master/app/models/services/facebook.rb]]

3) Find the media service icon here: "Social Media Icons are by Paul Robert Lloyd" [[http://paulrobertlloyd.com/2009/06/social_media_icons]]
add it to the appropriate public folder and name it the name of the provider

4) Add the line that lets users add your service: [[http://github.com/diaspora/diaspora/blob/master/app/views/services/index.html.haml]]

5) Submit a pull request.

6) Go throw a party to celebrate your awesomeness!