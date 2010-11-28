# Setup services on your Pod

## General

First copy config/oauth_keys.yml.example to config/oauth_keys.yml and open it in an editor.

## Twitter

* Go to [[http://dev.twitter.com]] and sign in
* Click on Register an app     
[[http://mrzyx.de/diaspora/services_twitter_1.png|width=500px]]
* Register your app   
[[http://mrzyx.de/diaspora/services_twitter_2.png|width=500px]]
    * Give it a name
    * Give it a description
    * Set the application website to your pod URL or a page that describes what Diaspora is and waht your pod has to do with it
    * Leave the application type at Browser
    * **Important:** Set the callback URL to the domain under which your pod is running (including http:// or https://)
    * Set default access type to "Read & Write"
    * There is a Captcha  ;)
    * Click "Register application"
* You now can see your consume key and your consumer secret, copy them to the right places in config/oauth_keys.yml   
[[http://mrzyx.de/diaspora/services_twitter_3.png|width=500px]]
* Restart Diaspora on your sever (you can skip that when you want to also add facebook support) 
* You're done. It's now possible to post to Twitter from your pod :)

## Facebook

* Goto [[http://developers.facebook.com/setup/]]
* Choose a name, for example "Diaspora at example.org"
* Set the site address to your pod URL including a trailing /, for example http://example.org/
* Choose a language
* Click on Create application
* Fill the Captcha
* It will now give you your app id and your app_secret which you have to set in your config/oauth_keys.yml
* Restart Diaspora on your server
* You're done. It's now possible to post to Facebook from your pod :)