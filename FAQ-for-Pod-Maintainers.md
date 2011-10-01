We've started adding questions that we see a lot to this page, but it doesn't cover everything.
If you have other questions, the best way to get an answer quickly is to visit us in IRC. 
You might think, "IRC? For real? Is this 1994 again?" 

Yes. I mean, **no**! Just think of IRC as the open-source equivalent of a Campfire room. **Links
to IRC channels and mailing lists are at the bottom of this page.**

## Installation FAQ

***Do you have a detailed install guide?***

Yes. [[Check it out!|Installing and Running Diaspora]] It will probably be more up-to-date than
this page, in general.

***What ports does Diaspora need open for communication?***  

* Your server must be on either port 80 for HTTP or 443 for HTTPS. You must forward that port to 3000. Otherwise friend requests may cause lockups on other servers. See the command-line switches below.

* Your server should also have 8080 available for websockets. <a href="http://en.wikipedia.org/wiki/WebSockets" target="_blank">Websockets</a> 
are how the browser talks in 'real time' to the server. They are not required for basic pod 
operation, nor for inter-pod communication. Since many hosting providers don't allow you to 
open 8080, you may consider them a nice-to-have.

* Due to a bug, your server must block, not just filter out (->timeout), port 443.

***Can I use Apache to run Diaspora?***

Yes. See [[Installing on Ubuntu Apache]] or the [Unofficial Diaspora with Apache2](http://blog.fejes.ca/?p=41) guide.

***What are the command line options for launching thin (Diaspora's HTTP server)?***  

There are a couple of helpful command line options for setting the address and port for thin:

    bundle exec thin -a <address> -p <port> start

**Note:** If you are running Diaspora on port 80, the command above needs to be executed as root - i.e., with su or sudo in front of it. 
For example, on Ubuntu, Arch et al. do <code>sudo bundle exec thin -p 80 start</code>

**-D** will turn on debug mode.  Run <code>thin -h</code> to see a complete list.

***How do I install on other distros?***

[Unofficial install script for Ubuntu](http://github.com/maco/diaspora/commits/master/ubuntu-setup.bash)<br>
There is a [AUR package](http://aur.archlinux.org/packages.php?ID=40859) for Arch Linux<br>
and some guides to install it manually:  
[Unofficial guide for Arch Linux installation 1 (Arch Linux Forums)](https://bbs.archlinux.org/viewtopic.php?pid=826763#p826763)<br>
[Unofficial guide for Arch Linux installation 2](http://www.diederickdevries.net/blog/2010/09/16/diaspora-on-arch/)<br>

***I've got my pod running. How do I disable outside logins?***  

Change <code>registrations_closed</code> in config/app_config.yml from false to true, and then
restart the server.

***I'm getting SSL_connect returned=1 errno=0 state=SSLv2/v3 read server hello A: unknown protocol!***  

Close the port, make it do connection refused, not timeout.  
(Disclaimer: hopefully this doesn't screw up anyone)  
<james_> On Ubuntu: sudo ufw enable, sudo ufw reject 443 (, sudo reboot?)

***But I have Apache running already and I want no passengers!***  

(Disclaimer: Apache noob advice)  
Apache config:  
http://codepaste.net/yzkngy

*** How do I back up the database?***  

From the command line type:  
    mysqldump -u <mysql username> -p diaspora_development > backup.sql
Enter your mysql password when prompted for it.
Replace \_development with \_production if you're running the production environment.

***I installed Diaspora on my machine, went to http://localhost:3000 and signed up. But my friends can't add me!***  

You signed up with "localhost" which is not a valid external address. You first need an
externally-accessible address - either a domain name or an IP address. Once you have that
working, you need to clear your database and re-register coming in on the outside URL.

***bundle install >>> command not found ?***

sudo ln -s /var/lib/gems/1.8/bin/bundle /usr/local/bin/bundle
though the best solution is ./ubuntu-setup.bash

***How do I get past the sign in page to create a new account?***  

To create a new account, go to http://yourdiasporainstance.com/users/sign_up 
(replacing *yourdiasporainstance.com* with the the host name of your pod).

***I installed Diaspora on my machine, but when I load the site there are no images and the layout looks horrible!***

If you're missing your images in the production environment, change serve_static_assets in config/environments/production.rb to true and restart Diaspora. Or set up a reverse proxy to serve the files directly under public/.


## What if my question isn't answered here?

### IRC Channels

IRC is the best way to get an answer quickly. Click the link to the join the channel in a new 
browser window. You can also download and use an IRC client such as 
<a href="http://colloquy.info/" target="_blank">Colloquy</a> for OS X or 
<a href="http://www.mirc.com/" target="_blank">mIRC</a> for Windows.

* <a href="http://webchat.freenode.net/?channels=diaspora" target="_blank">#diaspora on irc.freenode.net</a> - general discussion and help for folks installing Diaspora
* <a href="http://webchat.freenode.net/?channels=diaspora-dev" target="_blank">#diaspora-dev on irc.freenode.net</a> - discussion of the source code and help for new developer contributors
* <a href="http://webchat.freenode.net/?channels=diaspora-de" target="_blank">#diaspora-de on irc.freenode.net</a> - discussion in German.

### Mailing lists

We have two mailing lists, both Google groups. They tend to have a slightly different audience than
the IRC channels, so if you can't get your question answered in IRC, you can try here.

* [Discussion list](http://groups.google.com/group/diaspora-discuss) - Google group for discussion of non-technical topics
* [Development discussion list](http://groups.google.com/group/diaspora-dev) - Google group for discussion of installation, source code, and other technical topics
