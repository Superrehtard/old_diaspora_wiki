----

###301 MOVED PERMANENTLY###

We're currently **moving this wiki over to our new project site**. The contents of this page have  already been carried over, so _any new changes here will not be reflected in the new wiki_.  
New link: http://wiki.diasporafoundation.org/Installing_on_FreeBSD

----


### Caveat

Installation on FreeBSD is a more involved and variable process than on some (all?) of the Linux distros.  This guide is intended for experienced FreeBSD admins. It uses a combination of ports and packages and takes a while to get everything compiled and running properly.

This is not intended as in introduction to FreeBSD.

### Versions

These instructions are for installing Diaspora* on a fresh install of FreeBSD 9.0.

They will install Ruby 1.9 and PostgreSQL 9.1.

### Package Management

Use of the `portmaster` ports management tool is assumed.

### Prerequisites

It is assumed that you have a fresh install of FreeBSD 9.0 with ports tree installed and you are running from the non-root user that you intend to run Diaspora* under.  

Packages/ports up and running should include:
* bash
* sudo
* curl
* ca_nss_root

### The Basics

Install required libraries:
        sudo pkg_add -r libxml2 libxslt


### PostgreSQL 

To install Postgresql as your database, run the following:

        sudo pkg_add -r postgresql91-server
 
Add `postgresql_enable="YES"` to /etc/rc.conf, then:

        sudo /usr/local/etc/rc.d/postgresql initdb
        sudo /usr/local/etc/rc.d/postgresql start
    
Set it up to run as your user (diaspora for example):

        sudo su pgsql
        createuser -srdP diaspora
        exit

### ImageMagick

To install ImageMagick, run the following:

        sudo pkg_add -r ImageMagick-nox11

### SQLite3

To install sqlite3, run the following:

        sudo pkg_add -r sqlite3

### Git

To install Git, run the following:

        sudo pkg_add -r git

Or if you got errors during "sudo pkg_add -r git" try:

        cd /usr/ports/devel/git && sudo make install clean

### Redis

To install Redis, run the following:

        sudo pkg_add -r redis

And add `redis_enable="YES"` to /etc/rc.conf


### To install Ruby (1.9):

Add `RUBY_DEFAULT_VER=1.9` to /etc/make.conf and then run:

        cd /usr/ports/lang/ruby19 && sudo make install clean
      
### To install RubyGems, run the following:

        cd /usr/ports/devel/ruby-gems/ && sudo make install clean
    
### Recompile everything to synchronize dependencies and add required ports

This is step is optional and very time-consuming:

        sudo portmaster -a
    
### Bundler

To install Bundler, run the following:

        sudo gem install bundler 

## Congrats! You have all your dependencies installed. Go back to [[Notes On Installing and Running Diaspora]].