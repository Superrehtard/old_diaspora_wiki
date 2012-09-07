We've started adding questions that we see a lot to this page, but it doesn't cover everything.
If you have other questions, the best way to get an answer quickly is to visit us in IRC. 
You might think, "IRC? For real? Is this 1994 again?" 

Yes. I mean, **no**! Just think of IRC as the open-source equivalent of a Campfire room. **Links
to IRC channels and mailing lists are at the bottom of this page.**

##Developer FAQ

***How do I get the latest source?***  
Pull the latest from github.

    git pull

Install any updates to gems:

    bundle install

    
***How do I reset the database to a totally clean state?***

    rake db:drop
    rake db:create
    rake db:schema:load

***How do I get debug information?***  
You can use the command  

    tail -f log/development.log

to watch the log in development mode.    

***I have found an issue with federation, how can I debug it?***  
We actually provide a special configuration for testing server-to-server communication, which produces logs that contain only the events around federation. It involves spinning up two Diaspora* instances which you can use to recreate realistic circumstances and the logs of both sides are recorded into a single file.  
See [[Federation Logger]]

***How do I get the latest beta features aka. "New Hotness"***  
In order to enable feature-flagged code you have to start your app server with the `NEW_HOTNESS` envrionment variable set.

    export NEW_HOTNESS=yesplze
    script/server &   # (or whatever you use to start the app server)

Now the latest and greatest stuff should be available to you as admin. If you want to present them to a user who isn't an admin, you can add the beta-role to his profile. 

    user@host$ rails c
    1.9.2p290 :001 > u = User.find_by_email("user@email.com")
    1.9.2p290 :001 > Role.add_beta(u.person)

## What if my question isn't answered here?

### IRC Channels

IRC is the best way to get an answer quickly. Click the link to the join the channel in a new 
browser window. You can also download and use an IRC client such as 
<a href="http://colloquy.info/" target="_blank">Colloquy</a> for OS X, [XChat](http://xchat.org/) for GNU/Linux or 
<a href="http://www.mirc.com/" target="_blank">mIRC</a> for Windows.

* <a href="http://webchat.freenode.net/?channels=diaspora" target="_blank">#diaspora on irc.freenode.net</a> - general discussion and help for folks installing Diaspora
* <a href="http://webchat.freenode.net/?channels=diaspora-dev" target="_blank">#diaspora-dev on irc.freenode.net</a> - discussion of the source code and help for new developer contributors
* <a href="http://webchat.freenode.net/?channels=diaspora-de" target="_blank">#diaspora-de on irc.freenode.net</a> - discussion in German.

### Mailing lists

We have two mailing lists, both Google groups. They tend to have a slightly different audience than
the IRC channels, so if you can't get your question answered in IRC, you can try here.

* [Discussion list](http://groups.google.com/group/diaspora-discuss) - Google group for discussion of non-technical topics
* [Development discussion list](http://groups.google.com/group/diaspora-dev) - Google group for discussion of installation, source code, and other technical topics
