# Using apache instead of thin server on Fedora
No, This does NOT  work. It's left as a starting point for those brave who miht make it work.



This seems like it overlaps with installing on ubuntu/apache, so let's get rid of one.  -- Raphael Sofaer
Basically, three issues:
* Is it possible to use apache?  According to [[Installing and Running Diaspora]] the event machine required does not work w passenger + apache. Or?
* This does partly overlap, but each OS has their own conventions about packaging, file names etc.  The only real overlap here is the
  apache configuration IMHO.
* Where should the discussion be on this topic? Here? Mailing list? Opne an issue? Or

##General

This page covers the steps which makes diaspora server running on port 3000. Besides this, most users 
have a need to making this server available to the outside world on port 80. This is not covered at this point.
Personally, I just forwarded port 80 on my router to port 3000 on my box.
 
*You can get the same basic behavior that nginx proxy uses from the proxy_balancer Apache mod.
With this you can put in a rewrite rule for /diaspora and send it to port 3000.  You would still need
port 8080 open for the EM server, but there is no reason a basic proxy mechanic shouldn't work.
I haven't tried it, but it's the path least fraught with peril.*

##Passenger
A common way to run Rails apps on Apache is the Passenger apache
module. See [[http://www.modrails.com/documentation/Users%20guide.html]]

There is no Fedora passenger rpm ATM, see
[[https://bugzilla.redhat.com/show_bug.cgi?id=470696]]

See also [[Installing on Ubuntu Apache]]

## Install passenger module

To setup apache to run diaspora:

      sudo gem install passenger
      sudo  yum install apr-devel httpd-devel rubygem-rake
      sudo passenger-install-apache2-module

Create a new file /etc/httpd/conf.d/passenger.conf with content as given from
passenger-install-apache2-module. For me, this was:

     LoadModule passenger_module /usr/lib/ruby/gems/1.8/gems/passenger-2.2.15/ext/apache2/mod_passenger.so
     PassengerRoot /usr/lib/ruby/gems/1.8/gems/passenger-2.2.15
     PassengerRuby /usr/bin/ruby

## Make diaspora accessible from apache as a rails rack app

Copy the complete diaspora application to  e. g.,  /usr/local/webapps to
avoid troubles when apache can't access files in your ordinary home dir.
Avoid storing the app under /var/www, seems that rewriting rules becomes
unhappy in some cases then (?)

        cd <parent of diaspora dir>
        sudo mkdir /usr/local/webapps
        sudo mv diaspora /usr/local/webapps
        mkdir /usr/local/webapps/diaspora/tmp  # All rack apps should have a tmp dir.
        chown -R apache /usr/local/webapps/diaspora

Install the bundle

        sudo bundle install --system
        mkdir -p devise.git/ruby
        ln -s /usr/lib/ruby/gems/* devise.git/ruby/

## Configure apache server instance

Create a virtual http server for the diaspora app by appending something like this to
/etc/httpd/conf/httpd.conf (using your own servername for host.domain.tld):

    Listen 3000
    <virtualhost *:3000>
        ServerName     host.domain.tld
        DocumentRoot   /usr/local/webapps/diaspora
        RailsEnv       development
        RackEnv        development
         <Directory /usr/local/webapps/diaspora/public>
            AllowOverride None
            Order         allow,deny
            Allow         from all
        </Directory>
    </virtualhost>


## Run and access server

Restart server using 'server httpd restart'.

Access the server on http://host.domain.tld:3000

## Bugs

- At this point, adding friends from this server to other servers fails with a "Connection timeout" message.

## Update this document

Fix whatever problems you discovered using this page by editing it!


