We've started adding questions that we see a lot to this page, but it doesn't cover everything.
If you have other questions, the best way to get an answer quickly is to visit us in IRC. 
You might think, "IRC? For real? Is this 1994 again?" 

Yes. I mean, **no**! Just think of IRC as the open-source equivalent of a Campfire room. **Links
to IRC channels and mailing lists are at the bottom of this page.**

### Installation

***Do you have a detailed install guide?***  
Yes. [[Check it out!|Installation Guides]] It will probably be more up-to-date than
this page, in general. Also, feel free to look at the [collected guides](https://github.com/diaspora/diaspora/wiki/Installation-Guides) page for OS-specific/service-specific help.

***What ports does Diaspora need open for communication?***  
Your webserver must be on port 443 for HTTPS. You must forward that port to 3000 or configure Apache or Nginx to do reverse-proxying. Otherwise friend requests may cause lockups on other servers. See the command-line switches below.

***Can I use Apache to run Diaspora?***  
Yes, but that's quite uncommon and thus you might not have as many people being able to support you in case of need. See [[Installing on Ubuntu Apache]] or the [Unofficial Diaspora with Apache2](http://blog.fejes.ca/?p=41) guide.

***What are the command line options for launching thin (Diaspora's HTTP server)?***  
There are a couple of helpful command line options for setting the address and port for thin:

    bundle exec thin -a <address> -p <port> start

Use <code>-D</code> to turn on debug mode.  Run <code>thin -h</code> to see a complete list.

***How do I install on various linux distros or Mac?***  
See the [Installation Guides](https://github.com/diaspora/diaspora/wiki/Installation-Guides) for a list of supported distros/OSs and what to do in each case.  
There is also an [AUR package](http://aur.archlinux.org/packages.php?ID=40859) for Arch Linux and some guides to install it manually:  
[Unofficial guide for Arch Linux installation 1 (Arch Linux Forums)](https://bbs.archlinux.org/viewtopic.php?pid=826763#p826763)<br>
[Unofficial guide for Arch Linux installation 2](http://www.diederickdevries.net/blog/2010/09/16/diaspora-on-arch/)<br>

***I'm getting SSL_connect returned=1 errno=0 state=SSLv2/v3 read server hello A: unknown protocol!***  
Close the port, make it do connection refused, not timeout.  
(Disclaimer: hopefully this doesn't screw up anyone)  
<james_> On Ubuntu: sudo ufw enable, sudo ufw reject 443 (, sudo reboot?)

***I installed Diaspora on my machine, went to http://localhost:3000 and signed up. But my friends can't add me!***  
You signed up with "localhost" which is not a valid external address. You first need an
externally-accessible address - either a domain name or an IP address. Once you have that
working, you need to clear your database (`$ bundle exec rake db:migrate:reset`) and re-register coming in on the outside URL.

***bundle install >>> command not found ?***  
sudo ln -s /var/lib/gems/1.8/bin/bundle /usr/local/bin/bundle
though the best solution is ./ubuntu-setup.bash

***How do I get past the sign in page to create a new account?***   
To create a new account, go to http://yourdiasporainstance.com/users/sign_up 
(replacing *yourdiasporainstance.com* with the the host name of your pod).

***I installed Diaspora on my machine, but when I load the site there are no images and the layout looks horrible!***  
If you're missing your images in the production environment, change `environment.assets.serve` in `config/diaspora.yml` to `true` and restart Diaspora. Or set up a reverse proxy to serve the files directly under public/.

***There are no images on my pod***  
You are most likely in production mode and/or your apache/nginx is not serving static assets.  If you just want to run your server directly from thin on port 3000, you can set ruby to serve static assets by changing `environment.assets.serve` to `true` in your `config/diaspora.yml`. Please note, that it is faster if apache/nginx is set up to serve the files from the `public/` directory, since that's what it's build to do - serve files over http.  
*When deploying to heroku:*
Be sure to check that the `ENVIRONMENT_ASSETS_SERVE` is set to `true` in your `heroku config`.

***Webfinger does not seem to be working***
Is your _resque_ worker running? Can you see the error in resque web? Does your SSL cert check out? (try a [ssl cert checker][ssl-check]).
We do not support self signed certs, but you can get a free one [from StartSSL](https://www.startssl.com/).

***I am receiving posts, but nobody is receiving mine...***  
[Check your ssl certs][ssl-check]!

***I'm getting the warning "... in production without Resque workers"***  
[Resque](https://github.com/defunkt/resque) is the backend we use for processing background jobs. Normally, resque is spawned as a separate process, but in this case you have configured Diaspora to run the jobs in the same process as the application. This is normally used for development or testing purposes, but if used in production, it can bring major performance penalties. Thus you should always run resque in its own process, by setting `environment.single_process_mode` to `false` in your `config/diaspora.yml` and starting the resque process with

    RAILS_ENV=production DB=mysql QUEUE=* bundle exec rake resque:work  # or
    RAILS_ENV=production DB=postgres QUEUE=* bundle exec rake resque:work

(In case you are using `script/server` to start Diapora, then you don't have to manually start the workers. This is already done by the script.)

[ssl-check]: https://www.sslshopper.com/ssl-checker.html

### Upgrading

***Is there anything I should do before upgrading my installation?***  
We have a service (Travis) that builds all our code for each ruby version/database combination that we support. Before you update your Diaspora installation, you should check here to see if your combination is green on the master branch: http://travis-ci.org/diaspora/diaspora  If it's not green, wait to do the update. (Note: Travis is having issues with their SSL cert, so you may get a warning, but it is safe to proceed.). Also read the `Changelog.md` file in the projects root.

***How do I roll back my installation if an update breaks it?***  
Just do:  `git checkout [ref]`  Where [ref] is the identifier of the commit to go back to. It's a long guid-like string of letters and numbers. You can find the ref by doing a git log, eyeballing the commit dates, and figuring out where you were before you pulled. Of course it's best if you keep track of that ref before you update. :)

***What if there were database migrations in the code I am rolling back?***  
Then you also need to roll those back separately. You need to do this BEFORE you roll back the code. Look in the db/migrate directory and figure out which files are new since the last time you pulled - i.e., which migration your db should be on. They are in timestamp order in that directory. Then do:
`rake db:rollback`  And look at the output. It will tell you which migration you are now on. It rolls back one each time you run it. Run it until you're on the right migration and then roll back the code as described above.

Also be aware that database rollbacks can fail - they depend on the migration author writing the correct code to roll back. And if they didn't right the correct code to roll forward... :)  So really, the best thing you can do to avoid this is check [Travis](http://travis-ci.org/diaspora/diaspora) (as described above) before doing an update.

### General

***Will a pod eventually receive federated posts that it misses while being offline/down?***  
Currently no, there are no retries, though we'd like to add that at some point. 

***What do I do about all these PGErrors, like disconnects and SSL problems?***  
If you are running Diaspora with PostgreSQL, beware that having [the ssl setting](http://www.postgresql.org/docs/9.1/interactive/runtime-config-connection.html#GUC-SSL) turned on in the PostgreSQL config has been causing problems for several people.  We recommend turning it off unless you know what you're doing.

***I've got my pod running. How do I disable outside logins?***   
Change <code>settings.enable_registrations</code> in config/diaspora.yml from true to false, and then
restart the server.

***How do I back up the database?***  
From the command line type:  
    mysqldump -u <mysql username> -p diaspora_development > backup.sql
Enter your mysql password when prompted for it.
Replace \_development with \_production if you're running the production environment.

***What's up with assets/jammit/js-runtime?***  
The recent update to Rails 3.1 made it possible for us to use the included asset compilation mechanism and therefore drop the requirement for jammit and a Java environment. The new method requires a JavaScript runtime like [Node.js](http://nodejs.org/) or [TheRubyRacer](https://github.com/cowboyd/therubyracer) to be installed, though. See the [specific distribution guides](Installation-Guides) for how to install them. After it has been installed, you can compile the assets by running `bundle exec rake assets:precompile`. See the [installation guide](Notes-on-Installing-and-Running-Diaspora) for more detailed instructions.

***I am on PPC (e.g. old iMac) and want to use it for serving Diaspora, but there is no ExecJS compatible JS runtime***  
One thing you could do, assuming you have another PC that will run Node. Precompile your assets on that machine (using `bundle exec rake assets:precompile`) and then check them into git before you deploy to the PPC box. The Javascript runtime is only really needed for precompiling assets and shouldn't be loaded at all in the actual production environment. You may need to use `git add public/assets -f` to force git to check them in, since I think that directory is in .gitignore.  
(_from [#3429](https://github.com/diaspora/diaspora/issues/3429)_)

***What are roles and how do I use them?***  
We switched from statically configured 'special' user accounts in the config file to a role system, which lets us change role assignments without having to restart the server.  
In order to assign roles to users you need to start the rails console in the production environment by running the following command in your diaspora directory:

    $ RAILS_ENV=production rails c

Now if you want to assign roles to users, you can do so by running these commands:

    > u = User.find_by_email("user@email.com")
    > Role.add_admin(u.person)      # to add as admin, or
    > Role.add_spotlight(u.person)  # to add as 'community spotlight'

When you are done, you can exit the console with

    > quit

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

* [Discussion list](http://groups.google.com/group/diaspora-discuss) - Google group for discussion of installation and non-technical topics
* [Development discussion list](http://groups.google.com/group/diaspora-dev) - Google group for discussion of source code, and other technical topics
