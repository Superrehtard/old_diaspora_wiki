## Introduction

Diaspora is run on a network of connected servers, or "pods." This document describes the technical instructions on how to set up a new pod in the network. To join Diaspora, you do not need to set up your own pod--you can join an [existing pod](https://github.com/diaspora/diaspora/wiki/Community-supported-pods) running the Diaspora software. The pod you join could be one run by a friend, your university, or the official pod, run by the project’s founders, at [joindiaspora.com](http://joindiaspora.com). All of the Diaspora pods communicate and make up the Diaspora Network.

## The Official Way

We deploy and run Diaspora with a deployment tool called sod, which currently only supports CentOS.  We use Rackspace Cloud, but you can point sod at any CentOS machine.  So first you get yourself an ip and root password to a CentOS machine.  Then get yourself an SSL cert and put it in ~/diaspora_cert on your local machine.  Then you run sod on your local machine to provision the remote server:

    (install ruby 1.8.7, git, bundler)
    git clone git://github.com/MikeSofaer/sod.git
    cd sod
    bundle install
    ./sod <remote_machine_ip_address> diaspora diaspora <remote_machine_root_password>

This should take about 40 minutes, and when it's done Diaspora should be running.  You will need to edit config files, etc.

Warning: The version of splunk installed this way only supports 64-bit processors.

On a minimal install system you'll need to install bzip2 and vixie-cron for this to work (yum install bzip2 vixie-cron)

For localhost you can skip the SSL cert and just use 127.0.0.1 for the IP and your root password.

To restart the webservers after making a change, ssh in and type `svc -t /service/thin*`

## Notice

We currently run Diaspora with the [thin](http://code.macournoyer.com/thin/) as
our webserver, behind [nginx](http://wiki.nginx.org/Main). Diaspora uses an
asynchronous [EventMachine](http://rubyeventmachine.com/) queue inside the appserver
to send messages between seeds.  If you use mod_rails, mongrel, or another 
non-eventmachine based application server, federation may not work.

If you don't like thin, you can always try
[Rainbows!](http://rainbows.rubyforge.org/) We will try to fully support more
webservers later, but that is what works for now.

These instructions are for machines running [Ubuntu](http://www.ubuntu.com/),
[Fedora](http://www.fedoraproject.org) or Mac OS X. We are developing Diaspora
for the latest and greatest browsers, so please update your Firefox, Chrome or
Safari to the latest and greatest.

## Preparing your system

In order to run Diaspora, you will need to download the following dependencies
(specific instructions follow):

- Build Tools - Packages needed to compile the components that follow.
- [Ruby](http://www.ruby-lang.org) - The Ruby programming language.
  (We're using **1.8.7**.  It comes preinstalled on Mac OS X.)
- [MongoDB](http://www.mongodb.org) - A snappy noSQL database.
- [OpenSSL](http://www.openssl.org/) - An encryption library.
  (It comes preinstalled on Mac OS X and Ubuntu.)
- [ImageMagick](http://www.imagemagick.org/) - An Image processing library used
  to resize uploaded photos.
- [Git](http://git-scm.com/) - The fast version control system.
- Redis - Persistent key-value database with network interface

After you have Ruby installed on your system, you will need to get RubyGems,
then install Bundler:

- [RubyGems](http://rubygems.org/) - Source for Ruby gems.
- [Bundler](http://gembundler.com/) - Gem management tool for Ruby projects.

**We suggest using a package management system to download these dependencies.
Trust us, it's going to make your life a lot easier.  If you're using Mac OS X,
you can use [homebrew](http://mxcl.github.com/homebrew/); if you're using
Ubuntu, just use [Synaptic](http://www.nongnu.org/synaptic/) (it comes
pre-installed); if you're using Fedora simply use
[yum](http://yum.baseurl.org/). The instructions below assume you have these
installed.**

### Build Tools

To install build tools on **Ubuntu**, run the following (includes the gcc and
xml parsing dependencies):

		sudo apt-get install build-essential libxslt1.1 libxslt1-dev libxml2

To install build tools on **Fedora**, run the following:

		su -c 'yum install libxslt libxslt-devel libxml2 libxml2-devel'

To install build tools on **Mac OS X**, you need to download and install
[Xcode](http://developer.apple.com/technologies/tools/xcode.html).

### Ruby

To install Ruby 1.8.7 on **Ubuntu**, run the following command:

		sudo apt-get install ruby-full

Please note that you need to have Universe enabled in your
/etc/apt/sources.list file to install ruby using apt-get.

At this time Fedora does not have Ruby 1.8.7. As a workaround it is possible to
use [rvm](http://rvm.beginrescueend.com/) with a locally compiled Ruby
installation.  A semi automated method for doing this is available.  It is
highly recommended that you review the script before running it so you
understand what will occur.  The script can be executed by running the
following command:

		./script/bootstrap-fedora-diaspora.sh

After reviewing and executing the above script you will need to follow the
"MongoDB" section and then you should skip all the way down to "Start Mongo".

If you're on **Mac OS X**, you already have Ruby on your system.  Yay!

### MongoDB

To install MongoDB on **Ubuntu**, add the official MongoDB repository
[here](http://www.mongodb.org/display/DOCS/Ubuntu+and+Debian+packages).

For Lucid, add the following line to your /etc/apt/sources.list (for other
distros, see http://www.mongodb.org/display/DOCS/Ubuntu+and+Debian+packages):

		deb http://downloads.mongodb.org/distros/ubuntu 10.4 10gen

Then run:
		sudo apt-key adv --keyserver keyserver.ubuntu.com --recv 7F0CEB10
		sudo apt-get update
		sudo apt-get install mongodb-stable

**Caution:** By default, MongoDB accessible from anywhere.

You can also run the binary directly by doing the following:

If you're running a 32-bit system, run:

		wget http://fastdl.mongodb.org/linux/mongodb-linux-i686-1.6.2.tgz

If you're running a 64-bit system, run:

		wget http://fastdl.mongodb.org/linux/mongodb-linux-x86_64-1.6.2.tgz

Then run:

		# extract
		tar xzf mongodb-linux-i686-1.4.0.tgz
		# create the required data directory
		sudo mkdir -p /data/db
		sudo chmod -Rv 777 /data/


To install MongoDB on a x86_64 **Fedora** system, add the official MongoDB
repository from MongoDB
(http://www.mongodb.org/display/DOCS/CentOS+and+Fedora+Packages) into
/etc/yum.repos.d/10gen.repo:

		[10gen]
		name=10gen Repository
		baseurl=http://downloads.mongodb.org/distros/fedora/13/os/x86_64/
		gpgcheck=0
		enabled=1

Then use yum to install the packages:

		su -c 'yum install mongo-stable mongo-stable-server'

If you're running a 32-bit system, run `wget
http://fastdl.mongodb.org/linux/mongodb-linux-i686-1.6.2.tgz`. If you're
running a 64-bit system, run `wget
http://fastdl.mongodb.org/linux/mongodb-linux-x86_64-1.6.2.tgz`.

		# extract
		tar xzf mongodb-linux-i686-1.4.0.tgz
		# create the required data directory
		su -c 'mkdir -p /data/db'
		su -c 'chmod -Rv 777 /data/'

To install MongoDB on **Mac OS X**, run the following:

		brew install mongo
		sudo mkdir -p /data/db
		sudo chmod -Rv 777 /data/

### OpenSSL

If you're running either **Ubuntu**, **Fedora** or **Mac OS X** you already
have OpenSSL installed!

**Ubuntu:**
For the use of encryption in the Event Machine it is necessary to install the package libssl-dev

		sudo apt-get install libssl-dev

### ImageMagick

To install ImageMagick on **Ubuntu**, run the following:

		sudo apt-get install imagemagick libmagick9-dev

or on Ubuntu 10.10,

		sudo apt-get install imagemagick libmagickwand-dev

To install ImageMagick on **Fedora**, run the following:

		su -c 'yum install ImageMagick'

To install ImageMagick on **Mac OS X**, run the following:

		brew install imagemagick

### Git

To install Git on **Ubuntu**, run the following:

		sudo apt-get install git-core

To install Git on **Fedora**, run the following:

		su -c 'yum install git'


To install Git on **Mac OS X**, run the following:

		brew install git

### Redis

Ubuntu:
		sudo apt-get install redis-server

Fedora:
		su -c 'yum install redis'

### Rubygems

On **Ubuntu 10.04**, run the following:

		sudo add-apt-repository ppa:maco.m/ruby
		sudo apt-get update
		sudo apt-get install rubygems

This PPA is maintained by an Ubuntu Developer. For Ubuntu 10.10, this version
of rubygems is in the repositories.

You may need to install libxsl first: http://nokogiri.org/tutorials/installing_nokogiri.html

If you are running **Ubuntu Server**, you might get an error that looks like:

		sudo: add-apt-repository: command not found

If this happens, you must first install python-software-properties, which contains the add-apt-repository command:

		sudo apt-get install python-software-properties

On **Fedora**, run the following:

		su -c 'yum install rubygems'

On **Mac OS X**, RubyGems comes preinstalled; however, you might need to update
it for use with the latest Bundler. To update RubyGems, run `sudo gem update
--system`.


### Bundler

After RubyGems is updated, simply run `sudo gem install bundler` to get
Bundler. If you're using Ubuntu repository .debs, bundler is found at
/var/lib/gems/1.8/bin/bundle

To get bundle work in Ubuntu, you might make a symbolic link: 

		sudo ln -s /var/lib/gems/1.8/bin/bundle /usr/local/bin/bundle

## Getting Diaspora

		git clone http://github.com/diaspora/diaspora.git

If you have never used github before, their
[help desk](http://help.github.com/) has a pretty awesome guide on getting
setup.


## Running Diaspora

### Install required gems

To start the app server for the first time, you need to use Bundler to install
Diaspora's gem depencencies.  Run `bundle install` from Diaspora's root
directory.  Bundler will also warn you if there is a new dependency and you
need to bundle install again.

NOTE: If you get "Could not get Gemfile" try typing the following first:
`cd diaspora`

NOTE: If you do any other rails development on your machine, you will probably
want to run `bundle install --path vendor` instead to install the gems in your local diaspora
directory to avoid conflicts with your existing environment.

### Start Mongo

If you installed the Ubuntu package, MongoDB should already be running (if not,
run `service mongodb start`). If you installed the binary manually, run `sudo
mongod` from where mongo is installed to start mongo.

If you installed the Fedora package, MongoDB will need to be started via
`service mongodb start`. If you installed the binary manually, run `su -c
'mongod'` from where Mongo is installed to start Mongo.

If you installed the OsX package through "brew", MongoDB will need to be
started via `sudo launchctl load
/Library/LaunchDaemons/org.mongodb.mongod.plist`. (before you have to go to
/Library/LaunchDaemons and add a symlink to
/usr/local/Cellar/mongodb/1.6.2-x86_64/org.mongodb.mongod.plist)

Diaspora will not run unless Mongo is running.  Mongo will not run by default,
and will need to be started every time you wish to use or run the test suite
for Diaspora.

STAY SECURE:  Be sure to either configure your firewall/iptables to block Mongo's port, (27017 by default), or to run Mongo with --bind_ip 127.0.0.1, to restrict incoming connections to localhost.

### Configure Diaspora

For a local development instance, you can skip this step initially.

Otherwise: Diaspora needs to know where on the internet it is.  Copy config/app_config.yml.example
to config/app_config.yml, put your external  url into the pod_url field, and make any other
needed configuration changes.

### Run the server

For a local development instance, just run `./script/server`. This will start both thin, redis, rake resque:work  and the websocket servers. The application is then available at http://localhost:3000. You can change port by editing config/server.sh

If you want to run an app server other than thin, you must run this appserver, the websocket server  and magent server separately.  

### Run the app server

For a local development instance, skip this step - just run `./script/server` to get both the app server, magent  and websocket server on the right ports.

Once mongo is running and bundler has finished, run `bundle exec thin start`
from the root Diaspora directory.  This will start the app server in
development mode[.](http://bit.ly/9mwtUw)  It will run on port 3000 by default
and you need to either run it on port 80 (probably unwise), or use your
webserver of choice (we use nginx) to proxy port 80 at your domain name
of choice to thin at port 3000 or over a socket.  See config/sprinkle/conf/nginx.conf
and config/thin.yml in the repo for an example thin config and nginx server stanza.

### Run the websocket and  redis servers

For a local development instance, skip this step - just run `./script/server` to get all servers running  on the right ports.

Run` bundle exec ruby script/websocket_server.rb' to start this server on port 8080. Change port in config/app_config.yml.

Run `redis-server`to start this server on the default port 6379. It uses a config file, normally /etc/redis.conf or 
/etc/redis/redis.conf defining ports and other stuff.

### The Resque worker

Run `bundle exec rake resque:work` to start the resque worker.

You can monitor it with `resque-web`.

### Logging in with a sample user

Run `rake db:seed:dev` (for a development instance). Then you can log in with user `tom` and password `evankorth`.
More details in db/seeds/dev.rb and db/seeds/tom.rb. 

There is also db:seed:first_user which let you define the name/pw of a first user.

If you have an error on Mac, try `bundle exec rake db:seed:dev --trace`

### Testing

Diaspora's test suite uses [rspec](http://rspec.info/), a behavior driven
testing framework.  To run the tests: `rake spec`.

### Read-only installation
 
The directories *tmp*, *public/upload* and *log* must be writable by the user running Diaspora even in a read-only installation.

Some of Diaspora's  web content in the public/ folder  is generated in runtime. In order to create a read-only installation, this content must be generated at install time instead.

Run sass/haml and create e. g.,  public/stylesheets/{application,ui,sessions}.css:
    rake db:seed:dev
    bundle exec thin -d --pid log/thin.pid start
    wget http://localhost:3000; rm index.html
    bundle exec thin --pid log/thin.pid stop

Run jammit and precache public/assets/*gz files:
    bundle exec jammit

After these commands  also the *public/* folder  can be read-only (although *public/uploads* need to be writable, see above).