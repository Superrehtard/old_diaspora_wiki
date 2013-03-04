# Setup services on your Pod

## General

Keys will be entered into your config/diaspora.yml

## Twitter

* Go to [[http://dev.twitter.com]] and sign in
* Click on 'Create an app'
  [[http://mrzyx.de/diaspora/twitter1.png|width=700px]]
* Register your app
  [[http://mrzyx.de/diaspora/twitter2.png|width=700px]]
    * Give it a name.  For example "Diaspora at example.org"
    * Give it a description
    * Set the application website to your pod URL or a page that describes what Diaspora is and what your pod has to do with it
    * **Important:** Set the callback URL to https://your_pod/auth/twitter/callback, replacing your_pod of course.
    * There's a ToS to accept
    * There is a Captcha  ;)
    * Click "Create Twitter application"
* You now can see your consumer key and your consumer secret, copy them to the right places in config/diaspora.yml
* Go to Settings and change the "Application Type" to "Read and Write".
* Restart Diaspora on your sever (you can skip that when you want to also add support for more services) 
* You're done. It's now possible to post to Twitter from your pod :)

## Tumblr

* Goto [[http://www.tumblr.com]] and sign up. If you already have an account get sure you're signed in.
* Goto [[http://www.tumblr.com/oauth/register]]        
  [[http://mrzyx.de/diaspora/services_tumblr.png|width=500px]]
    * Give it a name
    * Set the application website to your pod URL or a page that describes what Diaspora is and what your pod has to do with it
    * Give it a description
    * Enter an email address
    * **Important:** Set the "Default callback URL" to your pod_url (including http/https)+ /auth/tumblr/callback So if your pod is located at http://example.org enter http://example.org/auth/tumblr/callback
    * You can upload an icon but that's optional
    * Click register
* You'll be redirected to [[http://www.tumblr.com/oauth/apps]] where you can see your consumer key. After a click on "Show secret key" you can see your consumer secret. Add both to the right places in config/diaspora.yml
* Restart Diaspora on your sever (you can skip that when you want to also add support for more services) 
* You're done. It's now possible to post to Tumblr from your pod :)


## Facebook

* Goto [[http://developers.facebook.com/setup/]]
* Choose a name, for example "Diaspora at social.example.org", choose a namespace. Set the namespace in your `config/diaspora.yml` [[http://mrzyx.de/diaspora/facebook1.png|700px]]
* Fill the captcha
* If you don't see your new app reload the page and select it.
* Copy your App ID and your App Secret to your `config/diaspora.yml`.
* Click Edit App
* Add your domain to App Domains.
* Check "Website with Facebook Login" and fill in the URL to your pod.
* Hit save.
* Restart Diaspora on your server
* You're done. It's now possible to post to Facebook from your pod :)

### Privacy of posts to Facebook

Any posts a user makes from a pod to Facebook will be made with the privacy level that the users sets on Facebook side when the user authorizes the Diaspora* pod application. Posts done from Diaspora* do not follow the privacy of selected aspects on Diaspora* side. This behaviour is consistent with posting to Twitter etc where the target application governs the privacy of the post.