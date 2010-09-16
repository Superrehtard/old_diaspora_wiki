This page documents questions that are frequently being asked in relation to the installation and use of Diaspora.

If you have other questions, please try #diaspora on irc.freenode.net.

***Are there any public demo servers of Diaspora online that I can try?***<br>
Yes. The following are the official Diaspora demo instances, which will be wiped of all content at regular and unpredictable intervals:

* [[http://tom.joindiaspora.com]]
* [[http://washington.joindiaspora.com]]
* [[http://adams.joindiaspora.com]]


***How do I get past the login page when I don't have an account yet?***<br>
To create a new account, go to:
[[http://yourdiasporainstance.com/signup]] (replacing *yourdiasporainstance.com* with the the host name of your seed).

*** Whats the diference between production and master branches?***<br>
Production is what we were using and updating, maybe bi weekly for a few advisors and friends and such, at this point its not that much different, other than it runs in production mode as default rather than in dev mode.

***Tips to actually get this to run?***<br>

* [Webfinger patch](http://github.com/diaspora/diaspora/issues/issue/83/#issue/83/comment/411202) (unofficial, may need updating).
stop thin, apply patch, reset mongodb (cd mongodb-linux-i686-1.6.2/bin;./mongo diaspora-development <db.dropDatabase()> , restart thin <br>
* Your server must be on port 80, or you must forward 80 to 3000.  Otherwise friend requests may cause lockups on other servers.  Commandline switch is -p 80.<br>
* Your server must also have 8080 available for websockets.

***Command line options for launching Diaspora's thin http server?***<br>
There are a couple helpful command line options for setting the address and port for thin:
    bundle exec thin -a <address> -p <port> start
**-D** will turn on debug mode.  Run **thin -h** to see a complete list.

***Installing on other distros?***<br>
[Unofficial guide for Arch Linux installation](http://www.diederickdevries.net/blog/2010/09/16/diaspora-on-arch/)

***What is a *seed*?***<br>
A seed is a HTTP server (a webserver) where Diaspora is running. There are several seed which you can access using a webbrowser. Communication is not restricted to one seed. You can add friends from other seed and communicate with them. 

***Debug information?***<br>
You can use the command<br>
    tail -f log/development.log
To watch the dev log.  *If there is any way to get more verbose, detailed debug information please post here!*

***Once I get my seed running, how do I disable outside logins?***<br>
Quick answer: If you remove "registerable" from app/models/user.rb it will remove the "Sign up" link on the login page.
