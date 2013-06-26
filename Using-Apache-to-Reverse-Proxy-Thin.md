----

###403 DO NOT MOVE###

We're currently **moving this wiki over to our new project site**. The contents of this page are not important enough to be ported over because they are either very old, very outdated or wrong and misleading. 

----

## Introduction

For those who just want to use an apache vhost to reverse proxy traffic to thin running on port 3000, I wrote this short tutorial. I am assuming you've already installed diaspora and you have it running on localhost:3000. It is easy to change as appropriate if it is running on another port. I am also assuming that you're basically starting with a default apache2 install.

## Enabling modules

You'll want to enable mod_proxy and mod_proxy_http on your apache server so that it can reverse proxy thin. On a Debian system, the command a2enmod should do it:

    a2enmod proxy proxy_http

## Virtualhost

It is really quite simple to set this up. Create a file called diaspora (it can be anything you want, really, but we'll call it diaspora for the purposes of this tutorial) in the sites-available directory of the folder for your apache2 configuration. On Debian and Ubuntu, this is /etc/apache2. For the example, I used diaspora.example.com for the domain name of the diaspora node and 99.99.99.99 for the ip address of the server. Put in whatever values you see fit. If you have the thin you want to reverse proxy running somewhere other than localhost:3000, just replace the relevant domain (and port) where I put localhost:3000

    <VirtualHost *:80>
        NameVirtualHost 99.99.99.99
        ServerName diaspora.example.com
        ServerSignature Off
        ProxyRequests Off
        <Proxy *>
            Order Allow,Deny
            Allow from all
        </Proxy>
        ProxyPass / http://localhost:3000/
        ProxyPassReverse / http://localhost:3000/
        ProxyVia On
    </VirtualHost>


After that, you should run the following commands to enable the site you just created and to reload the apache configuration:

    a2ensite diaspora``
    apache2ctl graceful



And you can be on your way. If you saved it in a file other than one called diaspora, you'll run into trouble. I appreciate any and all feedback, I think there are several people who do this already and several ways to do it, so if this didn't work, please fix any mistakes I made with it.