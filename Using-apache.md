# Using apache instead of thin server on Fedora
**No, this is  NOT  reliable**. Basically, it's in the "Works for Me" (tm) state. But for those
prepared to handle all sorts of difficulties, it's a starting point. FTM, I'm not aware of any bugs.

This content is currently being discussed on [[ Discussions on Fedora Apache wiki page]]

##General

This page covers the steps which makes diaspora server running on port 3000. Besides this, most users 
have a need to make this server available to the outside world on port 80. This is not covered at this point.
Personally, I just forwarded port 80 on my router to port 3000 on my box.
 
*You can get the same basic behavior that nginx proxy uses from the proxy_balancer Apache mod.
With this you can put in a rewrite rule for /diaspora and send it to port 3000.  You would still need
port 8080 open for the EM server, but there is no reason a basic proxy mechanic shouldn't work.
I haven't tried it, but it's the path least fraught with peril.*

## Mod Proxy

The strategy described above can be set up using something like the below. Mod Proxy and Mod Proxy-Http need to be enabled. myserver.local is an alias set in /etc/hosts.

    <VirtualHost *:80>
    ServerName myserver.local
    ProxyRequests On
    ProxyVia On

    <Proxy *>
      AddDefaultCharset off
      Order deny,allow
      Allow from 127.0.0.1 
    </Proxy>

    ProxyPass / http://127.0.0.1:3000/
    ProxyPassReverse / http://127.0.0.1:3000/
    </VirtualHost>

For a more advanced config have a look at https://gist.github.com/719014

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

## Create the diaspora user and setup the web app.

     useradd -md /var/lib/diaspora diaspora
     su - diaspora
     mkdir -p /usr/share/diaspora
     chmod 755 .
     git clone -b master http://github.com/diaspora/diaspora.git master
     cd master
     bundle install --deployment
     mkdir tmp
     cp config/app_config.yml.example app_config.yml
     rake db:seed:dev
     ! Edit app_config.yml, fix at least pod_url
    
## Configure apache server instance

Create a virtual http server for the diaspora app by appending something like this to
/etc/httpd/conf/httpd.conf (using your own servername for host.domain.tld):

    Listen 3000
    <virtualhost *:3000>
        ServerName     host.domain.tld
        DocumentRoot   /usr/share/diaspora/master
        RailsEnv       development
        RackEnv        development
         <Directory /usr/share/diaspora/master/public>
            AllowOverride None
            Order         allow,deny
            Allow         from all
        </Directory>
    </virtualhost>


## Run and access server

Restart http and  websocket server :

    service httpd restart
    cd /usr/share/diaspora/master
    bundle exec ruby ./script/websocket_server.rb&

Access the server on http://host.domain.tld:3000

## Update this document

Fix whatever problems you discovered using this page by editing it!


