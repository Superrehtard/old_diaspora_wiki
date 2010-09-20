This page documents questions that are being frequently asked in relation to the installation and use of Diaspora.

If you have other questions, please try #diaspora on irc.freenode.net or our [[Google Group|http://groups.google.com/group/diaspora-discuss]].

# User FAQ

***Are there any public demo servers of Diaspora online that I can try?***<br>
Yes. The following are the official Diaspora demo instances, which will be wiped of all content at regular and unpredictable intervals:

* [[http://tom.joindiaspora.com]]
* [[http://washington.joindiaspora.com]]
* [[http://adams.joindiaspora.com]]

For a list of unofficial, community-driven servers, see [[Community supported seeds]].

You can check the version of these seeds by clicking "DEBUG INFO" at the very bottom of each page. There, the commit id and commit date is noted.

***How do I get past the login page when I don't have an account yet?***<br>
To create a new account, go to:
[[http://yourdiasporainstance.com/signup]] (replacing *yourdiasporainstance.com* with the the host name of your seed).

***What is a *seed*?***<br>
A seed is an HTTP server (a webserver) where Diaspora is running. There are several seeds which you can access using a webbrowser. Communication is not restricted to one seed. You can add friends from other seeds and communicate with them.

*** What is an *aspect*? ***

An aspect is a section with updates from preset list of friends. An aspect helps to divide your friend into different groups and show them different aspects of your life.

*** How to rename an aspect? ***

To rename an aspect go to manage page and click on aspect's title. You should be able to type there.

# Administrator FAQ

***What ports does Diaspora need available for communication?***<br>

* Your server must be on port 80, or you must forward 80 to 3000.  Otherwise friend requests may cause lockups on other servers.  See commandline switches below.
* Your server must also have 8080 available for websockets.  [Websockets](http://en.wikipedia.org/wiki/WebSockets) are how the browser talks in 'real time' to the server.

*(Installing with Apache? See [[Installing on Ubuntu Apache]] or the [Unofficial Diaspora with Apache2](http://blog.fejes.ca/?p=41) guide.)*

***What are the command line options for launching Diaspora's thin HTTP server?***<br>
There are a couple of helpful command line options for setting the address and port for thin:
    bundle exec thin -a <address> -p <port> start
**Note:** If you are running Diaspora on port 80, the command above needs to be executed as root, that is with su or sudo in front of it. eg. On Ubuntu, Arch et al. "sudo bundle exec thin -p 80 start"

**-D** will turn on debug mode.  Run **thin -h** to see a complete list.

***How do I install on other distros?***<br>
[Unofficial guide for Windows installation](http://tom.net.nz/2010/09/installing-diaspora-on-windows/)<br>
[Unofficial install script for Ubuntu](http://github.com/maco/diaspora/commits/master/ubuntu-setup.bash)<br>
[Unofficial guide for Arch Linux installation 1 (Arch Linux Forums)](https://bbs.archlinux.org/viewtopic.php?pid=826763#p826763)<br>
[Unofficial guide for Arch Linux installation 2](http://www.diederickdevries.net/blog/2010/09/16/diaspora-on-arch/)<br>
Also, there is a [AUR package](http://aur.archlinux.org/packages.php?ID=40859) (but it needs some tweaking)<br>

***Once I get my seed running, how do I disable outside logins?***<br>
Quick answer: If you remove "registerable" from app/models/user.rb, it will remove the "Sign up" link on the login page.

***I'm getting SSL_connect returned=1 errno=0 state=SSLv2/v3 read server hello A: unknown protocol!***<br>
Close the port, make it do connection refused, not timeout.<br>
(Disclaimer: hopefully this doesn't screw up anyone)<br>
<james_> On Ubuntu: sudo ufw enable, sudo ufw reject 443 (, sudo reboot?)

***But I have Apache running already and I want no passengers!***<br>
(Disclaimer: Apache noob advice)<br>
Apache config:<br>
http://codepaste.net/yzkngy


# Developer FAQ

*** What's the difference between production and master branches?***<br>
Production is what we were using and updating, maybe bi-weekly for a few advisors and friends and such. At this point its not that much different, other than it runs in production mode as default rather than in dev mode.

***How do I get debug information?***<br>
You can use the command<br>
    tail -f log/development.log
To watch the dev log.  *If there is any way to get more verbose, detailed debug information please post here!*

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

