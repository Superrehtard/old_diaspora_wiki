0) Read the git workflow guide: [[https://github.com/diaspora/diaspora/wiki/Git-Workflow]]

1) Add Friendster(*your favorite service*) support to omniauth: [[github.com/intridea/omniauth]] . 

1.5) Add the example for the keys: [[github.com/diaspora/diaspora/blob/master/config/oauth_keys.yml.example]]
and one line in the initializer:
[[github.com/diaspora/diaspora/blob/master/config/initializers/omniauth.rb]]

2) Test drive your stuff. Ex.: [[github.com/diaspora/diaspora/blob/spec/models/services/facebook_spec.rb]] 
Write another service model, telling the app the api call that needs to happen to post a message (and how to do a friend finder). Ex.: [[github.com/diaspora/diaspora/blob/master/app/models/services/facebook.rb]]

3) Find the media service icon here: "Social Media Icons are by Paul Robert Lloyd" [[paulrobertlloyd.com/2009/06/social_media_icons]]
add it to the appropriate public folder and name it the name of the provider

4) Add the line that lets users add your service: [[github.com/diaspora/diaspora/blob/master/app/views/services/index.html.haml]]

5) Submit a pull request.

6) Go throw a party to celebrate your awesomeness!