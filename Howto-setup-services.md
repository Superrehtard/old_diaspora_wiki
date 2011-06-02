# Setup services on your Pod

## General

First copy config/oauth_keys.yml.example to config/oauth_keys.yml and open it in an editor.

## Twitter

* Go to [[http://dev.twitter.com]] and sign in
* Click on Register an app     
[[https://mrzyx.de/diaspora/services_twitter_1.png|width=500px]]
* Register your app   
[[https://mrzyx.de/diaspora/services_twitter_2.png|width=500px]]
    * Give it a name.  For example "Diaspora at example.org"
    * Give it a description
    * Set the application website to your pod URL or a page that describes what Diaspora is and waht your pod has to do with it
    * Leave the application type at Browser
    * **Important:** Set the callback URL to the domain under which your pod is running (including http:// or https://)
    * Set default access type to "Read & Write"
    * There is a Captcha  ;)
    * Click "Register application"
* You now can see your consumer key and your consumer secret, copy them to the right places in config/oauth_keys.yml   
[[https://mrzyx.de/diaspora/services_twitter_3.png|width=500px]]
* Restart Diaspora on your sever (you can skip that when you want to also add support for more services) 
* You're done. It's now possible to post to Twitter from your pod :)

## Tumblr

* Goto [[http://www.tumblr.com]] and sign up. If you already have an account get sure you're signed in.
* Goto [[http://www.tumblr.com/oauth/register]]
[[https://mrzyx.de/diaspora/services_tumblr.png]]
    * Give it a name
    * Set the application website to your pod URL or a page that describes what Diaspora is and waht your pod has to do with it
    * Give it a description
    * Enter an email address
    * **Important:** Set the "Default callback URL" to your pod_url (including http/https)+ /auth/tumblr/callback So if your pod is located at http://example.org enter http://example.org/auth/tumblr/callback
    * You can upload an icon but that's optional
    * Click register
* You'll be redirected to [[http://www.tumblr.com/oauth/apps]] where you can see your consumer key. After a click on "Show secret key" you can see your consumer secret. Add both to the right places in config/oauth_keys.yml
* Restart Diaspora on your sever (you can skip that when you want to also add support for more services) 
* You're done. It's now possible to post to Tumblr from your pod :)


## Facebook

* Goto [[http://developers.facebook.com/setup/]]
* Choose a name, for example "Diaspora at example.org"
* Set the site address to your pod URL including a trailing /, for example http://example.org/
* Choose a language
* Click on Create application
* Fill the Captcha
* It will now give you your app id and your app_secret which you have to set in your config/oauth_keys.yml
* Go to http://www.facebook.com/developers/editapp.php?app_id=< your app id >
* Go to Website and fill the domain of your pod (without http://) in Website-Domain, then click Save changes
* Restart Diaspora on your server
* You're done. It's now possible to post to Facebook from your pod :)