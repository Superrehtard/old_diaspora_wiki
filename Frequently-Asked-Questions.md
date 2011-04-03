This page documents questions that we see a lot. It probably doesn't cover everything. 
If you have other questions, please try #diaspora on irc.freenode.net,
or our [[Google Group|http://groups.google.com/group/diaspora-discuss]].

## User FAQ

***Are there any public demo servers of Diaspora online that I can try?***  

Yes. For a list of unofficial, community-driven servers, see [[Community supported pods]].

You can check the version of some of these pods by clicking "DEBUG INFO" at the very bottom 
of each page. There, the commit id and commit date is noted.

***How do I get past the login page when I don't have an account yet?***  

To create a new account, go to http://yourdiasporainstance.com/users/sign_up (replacing *yourdiasporainstance.com* with the the host name of your pod).

***What is a *pod*?***  

A pod is a server where Diaspora is running. There are lots of different pods, and communication 
is not restricted to one pod. You can add friends from other pods and communicate with them.

"Pod" in this case is a metaphor referring to pods on plants which contain seeds.

***What is a *seed*?***  

A seed is a profile or an account, and contains all the data of a specific user. 
Your seed interacts with the seeds of your friends to keep each other up to date. 
You can see it as a package of personal data...which is all yours! 

Seeds are hosted on servers running the Diaspora software, which are called 'pods'. 
In the future you will be able to move your seed between pods. For now, you can export a file
containing all your information.

[[Aspects FAQ]]

## Administrator FAQ

***What ports does Diaspora need available for communication?***  

* Your server must be on port 80, or you must forward 80 to 3000.  Otherwise friend requests may cause lockups on other servers.  See commandline switches below.
* Your server must also have 8080 available for websockets.  [Websockets](http://en.wikipedia.org/wiki/WebSockets) are how the browser talks in 'real time' to the server.
* Due to a bug, your server must block, not just filter out (->timeout), port 443.

*(Installing with Apache? See [[Installing on Ubuntu Apache]] or the [Unofficial Diaspora with Apache2](http://blog.fejes.ca/?p=41) guide.)*

***What are the command line options for launching Diaspora's thin HTTP server?***  
There are a couple of helpful command line options for setting the address and port for thin:
    bundle exec thin -a <address> -p <port> start
**Note:** If you are running Diaspora on port 80, the command above needs to be executed as root, that is with su or sudo in front of it. eg. On Ubuntu, Arch et al. "sudo bundle exec thin -p 80 start"

**-D** will turn on debug mode.  Run **thin -h** to see a complete list.

***How do I install on other distros?***  
[Unofficial guide for Windows installation](http://tom.net.nz/2010/09/installing-diaspora-on-windows/)<br>
[Unofficial install script for Ubuntu](http://github.com/maco/diaspora/commits/master/ubuntu-setup.bash)<br>
There is a [AUR package](http://aur.archlinux.org/packages.php?ID=40859) for Arch Linux<br>
and some guides to install it manually:  
[Unofficial guide for Arch Linux installation 1 (Arch Linux Forums)](https://bbs.archlinux.org/viewtopic.php?pid=826763#p826763)<br>
[Unofficial guide for Arch Linux installation 2](http://www.diederickdevries.net/blog/2010/09/16/diaspora-on-arch/)<br>


***Once I get my pod running, how do I disable outside logins?***  
Quick answer: If you remove "registerable" from app/models/user.rb, it will remove the "Sign up" link on the login page.

***I'm getting SSL_connect returned=1 errno=0 state=SSLv2/v3 read server hello A: unknown protocol!***  
Close the port, make it do connection refused, not timeout.  
(Disclaimer: hopefully this doesn't screw up anyone)  
<james_> On Ubuntu: sudo ufw enable, sudo ufw reject 443 (, sudo reboot?)

***But I have Apache running already and I want no passengers!***  
(Disclaimer: Apache noob advice)  
Apache config:  
http://codepaste.net/yzkngy

*** How to backup the database?***  
From the command line type:  
    mysqldump -u <mysql username> -p diaspora_development > backup.sql
Enter your mysql password when prompted for it.
Replace \_development with \_production if you're running the production environment.

*** I installed Diaspora on my machine, went to http://localhost and signed up. Now my friends can't add me***  
You've confused the poor computer. Go to your public address and try again. Also please understand, if you dont know such basics, you probably don't understand what you're risking by running such an early code on your own machine.

***bundle install >>> command not found ?***
sudo ln -s /var/lib/gems/1.8/bin/bundle /usr/local/bin/bundle
though the best solution is ./ubuntu-setup.bash

## Developer FAQ

***How do I get debug information?***  
You can use the command  
    tail -f log/development.log
To watch the dev log.  *If there is any way to get more verbose, detailed debug information please post here!*

***How do I get the latest source?***

Pull the latest from github:
    git pull
Install any updates to the bundle:
    bundle install
    
***How do I reset the database to a totally clean state?***

    rake db:drop
    rake db:create
    rake db:migrate