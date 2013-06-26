----

###301 MOVED PERMANENTLY###

We're currently **moving this wiki over to our new project site**. The contents of this page have  already been carried over, so _any new changes here will not be reflected in the new wiki_.  
New link: http://wiki.diasporafoundation.org/Installing_on_Fedora

----

*This page assumes that you're reasonably capable of installing packages in CentOS/Fedora using Yum and that you've compiled software for linux before.*

This page basically covers how to use the upstream sources to build required software. An alternative way based on rebuilding RPM:s is available at [[RPM installation on Fedora]]

## Installing Prerequisites

You'll need to grab make, gcc-c++ to compile Ruby and some gems.  You'll also need to get zlib-devel and openssl-devel for adding support for these libraries into Ruby.  Lastly, now is a good time to get ImageMagick as well.

*Since all of these command will need to be run as root, either `su -` to login as root, or use sudo.*

    su -
    yum install make gcc-c++ zlib-devel openssl-devel ImageMagick

Accept and install any dependencies that you're prompted to install for the above packages as well.

## Install Ruby

Download the latest version of Ruby 1.8.7 from http://ruby-lang.org.  This is p330 as of Feb 9, 2011.

    mkdir /usr/local/src
    cd /usr/local/src
    wget ftp://ftp.ruby-lang.org/pub/ruby/1.8/ruby-1.8.7-p330.tar.bz2
    tar -xvf ruby-1.8.7-p330.tar.bz2

The following steps will do the following, install Ruby, add zlib support, add openssl support, reinstall Ruby.  (@SmittyHalibut 2010-11-25: The first "make" gave me an error saying that Tk was compiled to use pthreads, but Ruby was not.  It suggested adding "--enable-pthread" to the "./configure" line.  I've updated the build commands below accordingly.)

    cd ruby-1.8.7-p330
    ./configure --enable-pthread
    make
    make install

    cd ext/zlib
    ruby extconf.rb
    make
    make install
    cd ../openssl
    ruby extconf.rb
    make
    make install
    cd ../..
    make
    make install

You should now have Ruby installed with zlib and openssl support.  Run ruby -v to make sure it's running and working.

    [root@machine ~]$ ruby -v
    ruby 1.8.7 (2010-08-16 patchlevel 330) [i686-linux]

*Note: Your version string may say 'x86_64' if you're on a 64-bit OS.*

## Install Rubygems

Next you'll need to install Rubygems.  1.5.0 as of Feb 09, 2011.

    cd /usr/local/src
    wget http://production.cf.rubygems.org/rubygems/rubygems-1.5.0.tgz
    tar -xvf rubygems-1.5.0.tgz
    cd rubygems-1.5.0
    ruby setup.rb

You can check if gem has been installed correctly by running gem -v and gem list.

    [root@machine ~]$ gem -v
    1.5.0
## Install Redis

    wget http://redis.googlecode.com/files/redis-2.2.7.tar.gz
    tar xzf redis-2.2.7.tar.gz
    cd redis-2.2.7
    make
    make install
    cp /usr/local/src/redis-2.2.7/redis.conf /etc/

## Install MySQL

yum install mysql

yum install mysql-server

yum install mysql-devel

service mysql start
	  	
## Install Git
  	
Git isn't available in the default CentOS repositories.  You have to add the Extra Packages For Enterprise Linux repositories to get it.  Information on how to install the repo can be found over at the Fedora Project website.	 [[https://fedoraproject.org/wiki/EPEL]]
	  	
Once you've installed the repository files, you just need to use yum to grab it.

    yum install git

## Walk around bug 360

You should look into [[Handling bug 360]]

## Continue with Standard Diaspora Install

Now that you have proper versions of ruby and gem, MySQL and git installed, you can follow the standard Diaspora installation instructions.
	  	
## Using apache instead of the thin server

If anyone is interested in making it possible to use apache instead of the thin webserver, a starting point is at   [[Using apache]]
 