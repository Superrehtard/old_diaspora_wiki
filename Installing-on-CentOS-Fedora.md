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

and started with

    sudo service mongod start

Otherwise, you can find the instructions for installing mongodb over at the MongoDB website.
[[http://www.mongodb.org/display/DOCS/CentOS+and+Fedora+Packages]]

Once you've added the mongodb repository to yum, you need to download the mongo-stable-server package and start the server.
	  	
    yum install mongo-stable-server	  	
    /etc/init.d/mongod start	  	
	  	
## Install Git
  	
Git isn't available in the default CentOS repositories.  You have to add the Extra Packages For Enterprise Linux repositories to get it.  Information on how to install the repo can be found over at the Fedora Project website.	 [[https://fedoraproject.org/wiki/EPEL]]
	  	
Once you've installed the repository files, you just need to use yum to grab it.

    yum install git

## Walk around bug 360

You should walk around [bug 360](http://github.com/diaspora/diaspora/issues/issue/360). This about getting a fast, direct negatuve answer
when trying to access port 443 i. e., the https port. In this section, I presume your host is located behind a router which you have full
access to.

First check the current situation.Note use of the external address, the address used by the outside world. In most cases, this is your 
router's external hostname

.One possible reply is this, coming within a few seconds:

    telnet host.example.com 443
    Trying 85.230.51.222...
    telnet: connect to address 85.230.51.222: Connection refused

If so, everything is OK. Otherwise, a typical situation is

    telnet host.example.com 443
    Trying 85.230.51.222...
    telnet: connect to address 85.230.51.222: Connection timed out

The first attempt to fix it might be to let the router block port 443. If ot works (no timeout) it's fine. Otherwise, connect port
443 on your router with port 443 on your host. Then create a file /etc/sysconfig/iptables-https like this:

    -A INPUT -p tcp -m tcp --dport 443 -j REJECT --reject-with tcp-reset

Start system-config-firewall. Click "Custom Rules" in leftmost window. Click Add, Select Protocol Type 'ipv4' and 
Firewall Table to 'Filter'. Select the file /etc/sysconfig/iptables-https. Click OK, and Apply. Done!

At this point you should try with telnet again. If you don't get a "Connection refused" I really don know what to do.





## Continue with Standard Diaspora Install

Now that you have proper versions of ruby and gem, mongodb and git installed, you can follow the standard Diaspora installation instructions.
	  	
## Using apache instead of the thin server

If anyone is interested in making it possible to use apache instead of the thin webserver, a starting point is at   [[Using apache]]
 