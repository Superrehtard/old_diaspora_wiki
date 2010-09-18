*This wiki picks up where the Diaspora wiki leaves off, so it's assumed you already have a working seed and can view it at yourdomain.com:3000 and would like to managed by apache instead of thin.*

## Install Apache development package
<pre>
sudo apt-get install apache2-dev
</pre>
*It can co-exist with your existing Apache ok*

## Install Passenger Module for Apache
From http://www.modrails.org/install.php

<pre>
gem install passenger
passenger-install-apache2-module
passenger-install-nginx-module
</pre>
*(My install is working without adding the nginx module; might only need one or the other)*

The installer will walk you through any dependencies you need ahead of time and once you've installed those it will do all the heavy lifting outside of your apache config and vhosts, so let's do those:

# Load module in Apache Config
*My default apache config file is /etc/apache2/apache2.conf and I just appended the module settings to the end of the file:*

LoadModule passenger_module /somewhere/passenger-x.x.x/ext/apache2/mod_passenger.so

PassengerRuby /usr/bin/ruby
PassengerRoot /somewhere/passenger/x.x.x
PassengerMaxPoolSize 10

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

A simple 
<pre>
sudo bundle install devise.git
</pre>
should do the trick.

Also be sure, that your installation is accessible for the apache user. 
just do a
<pre>
sudo chown -R www-data:www-data /you/diaspora/installation
</pre>
to change the rights