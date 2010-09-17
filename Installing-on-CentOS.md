*This page assumes that you're reasonably capable of installing packages in CentOS using Yum and that you've compiled software for linux before.*

## Installing Prerequisites

You'll need to grab make, gcc-c++ to compile Ruby and some gems.  You'll also need to get zlib-devel and openssl-devel for adding support for these libraries into Ruby.  Lastly, now is a good time to get ImageMagick as well.

*Since all of these command will need to be run as root, either `su -` to login as root, or use sudo.*

    su -
    yum install make gcc-c++ zlib-devel openssl-devel ImageMagick

Accept and install any dependencies that you're prompted to install for the above packages as well.

## Install Ruby

Download the latest version of Ruby 1.8.7 from http://ruby-lang.org.  This is p302 as of Sept 16, 2010.

    mkdir /usr/local/src
    cd /usr/local/src
    wget ftp://ftp.ruby-lang.org/pub/ruby/1.8/ruby-1.8.7-p302.tar.bz2
    tar -xvf ruby-1.8.7-p302.tar.bz2

The following steps will do the following, install Ruby, add zlib support, add openssl support, reinstall Ruby.

    cd ruby-1.8.7-p302
    ./configure
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
    ruby 1.8.7 (2010-08-16 patchlevel 302) [i686-linux]

*Note: Your version string may say 'x86_64' if you're on a 64-bit OS.*

## Install Rubygems

Next you'll need to install Rubygems.  1.3.7 as of Sept 16, 2010.

    cd /usr/local/src
    wget http://production.cf.rubygems.org/rubygems/rubygems-1.3.7.tgz
    tar -xvf rubygems-1.3.7.tar.gz
    cd rubygems-1.3.7
    ruby setup.rb

You can check if gem has been installed correctly by running gem -v and gem list.

    [root@machine ~]$ gem -v
    1.3.7

## Install MongoDB

You can find the instructions for installing mongodb over at the MongoDB website.

[[http://www.mongodb.org/display/DOCS/CentOS+and+Fedora+Packages]]

Once you've added the mongodb repository to yum, you need to download the mongo-stable-server package and start the server.

    yum install mongo-stable-server
    /etc/init.d/mongod start

## Install Git

Git isn't available in the default CentOS repositories.  You have to add the Extra Packages For Enterprise Linux repositories to get it.  Information on how to install the repo can be found over at the Fedora Project website.

[[https://fedoraproject.org/wiki/EPEL]]

Once you've installed the repository files, you just need to use yum to grab it.

    yum install git

## Continue with Standard Diaspora Install

Now that you have proper versions of ruby and gem, mongodb and git installed, you can follow the standard Diaspora installation instructions.