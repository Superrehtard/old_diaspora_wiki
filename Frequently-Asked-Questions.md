This page documents questions that are being frequently asked in relation to the installation and use of Diaspora.

If you have other questions, please try #diaspora on irc.freenode.net.

***Are there any public demo servers of Diaspora online that I can try?***<br>
Yes. The following are the official Diaspora demo instances, which will be wiped of all content at regular and unpredictable intervals:

* [[http://tom.joindiaspora.com]]
* [[http://washington.joindiaspora.com]]
* [[http://adams.joindiaspora.com]]

You can check the version of these seeds when you click "DEBUG INFO" at the very bottom of each page. There the commit id and commit date is noted.

***How do I get past the login page when I don't have an account yet?***<br>
To create a new account, go to:
[[http://yourdiasporainstance.com/signup]] (replacing *yourdiasporainstance.com* with the the host name of your seed).

*** What's the difference between production and master branches?***<br>
Production is what we were using and updating, maybe bi-weekly for a few advisors and friends and such. At this point its not that much different, other than it runs in production mode as default rather than in dev mode.

***What ports does Diaspora need available for communication?***<br>

* Your server must be on port 80, or you must forward 80 to 3000.  Otherwise friend requests may cause lockups on other servers.  See commandline switches below.
* Your server must also have 8080 available for websockets.  [Websockets](http://en.wikipedia.org/wiki/WebSockets) are how the browser talks in 'real time' to the server.
* Installing with apache? See the [Installing on Ubuntu LAMP guide](http://github.com/diaspora/diaspora/wiki/Installing-on-Ubuntu-LAMP) or the [Unofficial Diaspora with Apache2](http://blog.fejes.ca/?p=41)

***Command line options for launching Diaspora's thin http server?***<br>
There are a couple of helpful command line options for setting the address and port for thin:
    bundle exec thin -a <address> -p <port> start
**Note:** If you are running Diaspora on port 80, the command above needs to be executed as root, that is with su or sudo in front of it. eg. On Ubuntu, Arch et al. "sudo bundle exec thin -p 80 start"

**-D** will turn on debug mode.  Run **thin -h** to see a complete list.

***Installing on other distros?***<br>
[Unofficial guide for Arch Linux installation 1 (Arch Linux Forums)](https://bbs.archlinux.org/viewtopic.php?pid=826763#p826763)<br>
[Unofficial guide for Arch Linux installation 2](http://www.diederickdevries.net/blog/2010/09/16/diaspora-on-arch/)<br>
Also, there is a [AUR package](http://aur.archlinux.org/packages.php?ID=40859) (but it needs some tweaking)<br>
[Unofficial guide for Windows installation](http://tom.net.nz/2010/09/installing-diaspora-on-windows/)

***What is a *seed*?***<br>
A seed is an HTTP server (a webserver) where Diaspora is running. There are several seeds which you can access using a webbrowser. Communication is not restricted to one seed. You can add friends from other seeds and communicate with them. 

***Debug information?***<br>
You can use the command<br>
    tail -f log/development.log
To watch the dev log.  *If there is any way to get more verbose, detailed debug information please post here!*

***Once I get my seed running, how do I disable outside logins?***<br>
Quick answer: If you remove "registerable" from app/models/user.rb, it will remove the "Sign up" link on the login page.

***How do I pull and run the latest source and/or clean the database?***<br>
Pull the latest from github:
    git pull
Install any updates to the bundle:
    bundle install
Recompile:
    rake
Clean the database:
    mongo diaspora-development
    > db.dropDatabase()

***i'm getting SSL_connect returned=1 errno=0 state=SSLv2/v3 read server hello A: unknown protocol!***
Close the port, make it do connection refused, not timeout.

***But i have apache running already and i want no passengers!***<br>
(disclaimer: apache noob advice)
apache config:
http://codepaste.net/yzkngy
