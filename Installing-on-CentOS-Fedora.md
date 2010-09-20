*This page assumes that you're reasonably capable of installing packages in CentOS/Fedora using Yum and that you've compiled software for linux before.*

This page basically covers how to use the upstream sources to build required software. An alternative way based on rebuilding RPM:s is available at [[RPM installation on Fedora]]

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
    tar -xvf rubygems-1.3.7.tgz
    cd rubygems-1.3.7
    ruby setup.rb

You can check if gem has been installed correctly by running gem -v and gem list.

    [root@machine ~]$ gem -v
    1.3.7

## Install MongoDB

For Fedora, the official repositories now contains the mongodb-server package. Thus, for Fedora mongodb is installed by:

    sudo yum install mongodb-server