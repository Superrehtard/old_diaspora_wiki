----

###403 DO NOT MOVE###

We're currently **moving this wiki over to our new project site**. The contents of this page are not important enough to be ported over because they are either very old, very outdated or wrong and misleading. 

----



*This wiki picks up where the Diaspora wiki leaves off, so it's assumed you already have a working seed and can view it at yourdomain.com:3000 and would like to managed by Apache instead of Thin.*

See also: [[Using apache]]

Does anyone have federation and/or websockets working with apache?  Correct me if I am wrong, but we use eventmachine for both of these things, and should only work if you use thin or rainbows! as your appserver, not mod_rails and passenger?  - maxwell

I have a "working" install using apache2 + mod_passenger.  If you could give me a way to test this out, I would be happy to provide a more detailed analysis.  I notice a few things aren't working, (eg propagating messages in particular) but I can't be sure which parts are failing due to websockets vs. bugs. - apfejes

If you can friend tom@tom.joindiaspora.com, it's working.  Otherwise, I think we can consider passenger/mod_rails unavailable for now.  You can still use Apache for a webserver. -- Raphael Sofaer

Thanks - that's a good test. I'll start looking at it next week. -apfejes

There's a discussion on apache and passenger on [[Discussions on fedora apache wiki page]]. Can we use this also for discussion on passenger use on Apache? -- leamas

Both this page and the one linked above are vastly out of date.  Unfortunately, I've been too busy to contribute much since September.  At this point, I've managed to friend people, but other things are now failing.  For instance, upon sign up, I'm unable to save changes to the gender field.  So, I think it's pretty clear that Diaspora can be served by apache, but does not function correctly.  -- apfejes



## Install Apache development package
<pre>
sudo apt-get install apache2-dev
</pre>
*It can co-exist with your existing Apache ok*

## Install Passenger Module for Apache
From http://www.modrails.org/install.html

<pre>
gem install passenger
passenger-install-apache2-module
</pre>

The installer will walk you through any dependencies you need ahead of time and once you've installed those it will do all the heavy lifting outside of your apache config and vhosts, so let's do those:

# Load module in Apache Config
*My default apache config file is /etc/apache2/apache2.conf and I just appended the module settings to the end of the file:*

<pre>
LoadModule passenger_module /somewhere/passenger-x.x.x/ext/apache2/mod_passenger.so

PassengerRuby /usr/bin/ruby
PassengerRoot /somewhere/passenger/x.x.x
PassengerMaxPoolSize 10
</pre>

# Create vhost
Once you've installed and loaded Passenger, you just need to setup a vhost like any other website.


    <VirtualHost *:80>
        ServerAdmin     clay@bychosen.com
        ServerName      diaspor.us
        ServerAlias     www.diaspor.us
        DocumentRoot    "/var/www/diaspora/public"
          <Directory "/var/www/diaspora/public">
            Allow from all
            AllowOverride all
            Options -MultiViews
          </Directory>
    </VirtualHost>
</pre>

# Stop thin, restart apache
Then stop the thin server (have to cd to your diaspora directory and execute) and restart Apache:

<pre>
bundle exec thin stop
service apache2 restart
</pre>

That's it!

# Possible errors

You may get errors when calling your newly created vhost in the browser, like
<pre>http://github.com/BadMinus/devise.git (at master) is not checked out. Please run `bundle install` (Bundler::GitError)</pre> .

There are two alternatives at this point. Simplest is to create devise.git in the local directory (ie copy per installation of diaspora):

In the disapora directory:
<pre>
sudo bundle install devise.git
</pre>
should do the trick.

To make a system-wide install (ie single copy used by all installs), do (while root in the diaspora directory):
<pre>
sudo bundle install --system
mkdir -p devise.git/ruby
ln -s /usr/lib/ruby/gems/* devise.git/ruby/
</pre>
On subsequent installations the first line won't be required.

In both cases, the devise.git tree *must* be accessible for the apache user. 
just do a
<pre>
sudo chown -R www-data:www-data /you/diaspora/installation
</pre>
and, if you used the system option, a 
<pre>
sudo chown -R www-data:www-data /usr/lib/ruby/gems/1.8/
</pre>

to change the rights