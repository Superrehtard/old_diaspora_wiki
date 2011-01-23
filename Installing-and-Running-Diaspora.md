## Introduction

Diaspora is run on a network of connected servers, or "pods." This document contains technical information for installing the necessary software to run a pod yourself, either for development, or as a new pod on the Diaspora network.

If you just want to **use** Diaspora, you don't need to set up your own pod -- you can join an [existing pod](https://github.com/diaspora/diaspora/wiki/Community-supported-pods) running the Diaspora software. The pod you join could be one run by a friend, your university, or the official pod, run by the project’s founders, at [joindiaspora.com](http://joindiaspora.com). All of the Diaspora pods communicate and make up the Diaspora Network.

If you still want to run your own pod...we salute you. Read on.

## Things To Know

0. The install is a bit complex. We can help, though. **If you run into problems, please visit us in irc, on freenode, in [#diaspora](http://webchat.freenode.net/?channels=diaspora).**

1. These instructions are for machines running [Ubuntu](http://www.ubuntu.com/), [Debian](http://www.debian.org/),
[Fedora](http://www.fedoraproject.org) or [Mac OS X](http://www.apple.com/macosx/). In this document the **Debian** version used is Lenny 5.0, the **Ubuntu** version is 10.04 or 10.10, and the **OS X** version is 10.6 (Snow Leopard). Diaspora does not currently install on Windows, though we are working on it.

2. We are developing Diaspora for the latest and greatest browsers, so please update your Firefox, Chrome or Safari to the newest version. We do not currently support any version of Internet Explorer, though support is planned in the future.

3. On joindiaspora.com, we run the application using [thin](http://code.macournoyer.com/thin/) as our application server and [nginx](http://wiki.nginx.org/Main) as our web server. You can use another application server (passenger, mongrel...), or another web server (apache, unicorn...), but the core team may not have the expertise to help you set it up. There are folks in the community who do run Diaspora this way though, so ask around in irc and on the mailing list.

## Preparing your system

In order to run Diaspora, you will need to install the following dependencies (specific instructions follow):

- Build Tools - Packages needed to compile the components that follow.
- [Ruby](http://www.ruby-lang.org) - The Ruby programming language. (We're developing mostly on **1.8.7**, but we also support **1.9.2**.  Ruby 1.8.7 comes preinstalled on Mac OS X.)
- [MySQL](http://www.mysql.com) - Backend storage engine.
- [OpenSSL](http://www.openssl.org/) - An encryption library. (It comes preinstalled on Mac OS X and Ubuntu.)
- [ImageMagick](http://www.imagemagick.org/) - An image processing library we use to resize uploaded photos.
- [Git](http://git-scm.com/) - A version control system, which you will need to download the Diaspora source code from github.
- [Redis](http://redis.io/) - A persistent key-value store that we use via [resque](https://github.com/defunkt/resque) for background job processing.

After you have Ruby installed on your system, you also need:

- [RubyGems](http://rubygems.org/) - A package manager for Ruby code that we use to download libraries ("gems") that Diaspora uses.
- [Bundler](http://gembundler.com/) - A gem management tool for Ruby projects.

**We suggest using a package management system to download dependencies, where possible.
Trust us, it's going to make your life a lot easier.  If you're using Mac OS X, install [homebrew](http://mxcl.github.com/homebrew/); if you're using
Ubuntu, just use [Synaptic](http://www.nongnu.org/synaptic/) (it comes
pre-installed); if you're using Fedora simply use
[yum](http://yum.baseurl.org/). The instructions below assume you have these
installed.**

### Build Tools

To install build tools on **Ubuntu** and **Debian**, run the following (includes the gcc and
xml parsing dependencies):

        sudo apt-get install build-essential libxslt1.1 libxslt1-dev libxml2

To install build tools on **Fedora**, run the following:

        su -c 'yum install libxslt libxslt-devel libxml2 libxml2-devel'

To install build tools on **Mac OS X**, you need to download and install
[Xcode](http://developer.apple.com/technologies/tools/xcode.html). It's a large download; it also comes on your OS X DVD.

### Ruby

To install Ruby 1.8.7 on **Ubuntu**, run the following command:

        sudo apt-get install ruby-full

Please note that you need to have Universe enabled in your
/etc/apt/sources.list file to install ruby using apt-get.

To install Ruby 1.9.2 on **Debian** from source, run the following commands:

        cd /tmp
        wget ftp://ftp.ruby-lang.org/pub/ruby/1.9/ruby-1.9.2-p136.tar.bz2
        tar xpf ruby-1.9.2-p136.tar.bz2
        cd ruby-1.9.2-p136
        ./configure
        make
        make install

Alternativly if you have it installed you can use "checkinstall" instead of "make install" to install ruby in a more debian friendly way.

At this time Fedora does not have Ruby 1.8.7. As a workaround it is possible to
use [rvm](http://rvm.beginrescueend.com/) with a locally compiled Ruby
installation.

If you're on **Mac OS X**, you already have Ruby on your system.  Yay!

### MySQL

If you're on **Ubuntu**, **Debian**, or **Fedora**, use your package manager to install MySQL. If you're on **OS X**, it's already installed.

On **Fedora**, you also need the mysql-devel package:

        su -c 'yum install mysql-server mysql-devel'

On **Ubuntu**, you also need the libmysqlclient-dev and libmysql-ruby packages.

### OpenSSL

If you're running either **Ubuntu**, **Debian**, **Fedora** or **Mac OS X** you already
have OpenSSL installed!

**Ubuntu:** and **Debian**
For the use of encryption in the Event Machine it is necessary to install the package libssl-dev

        sudo apt-get install libssl-dev

### ImageMagick

To install ImageMagick on **Ubuntu** or **Debian**, run the following:

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

To install Git 1.7 on **Debian**, add Debian Backports repository and install it. Instructions: http://backports.debian.org/Instructions/

        sudo apt-get install git-core

To install Git on **Fedora**, run the following:

        su -c 'yum install git'


To install Git on **Mac OS X**, run the following:

        brew install git

### Redis

Ubuntu:
        sudo apt-get install redis-server

**Fedora**:
        su -c 'yum install redis'

**Debian**:

If you're running a 64-bit system, run:

        wget http://ftp.us.debian.org/debian/pool/main/r/redis/redis-server_2.0.1-2_amd64.deb

If you're running a 32-bit system, run:

        wget http://ftp.us.debian.org/debian/pool/main/r/redis/redis-server_2.0.1-2_i386.deb

Then install the corresponding package

        sudo dpkg -i redis-server_2.0.1-2_amd64.deb


### RubyGems

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

This is not needed on Debian is installed ruby from source.

On **Fedora**, run the following:

        su -c  'gem install bundler'

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
`cd diaspora

NOTE: If you do any other rails development on your machine, you will probably
want to run `bundle install --path vendor` instead to install the gems in your local diaspora
directory to avoid conflicts with your existing environment.

### Start MySQL

On **Ubuntu** or **Debian**, start MySQL by running `sudo service mysql start`.

On **Fedora**, start MySQL by running:

        su -c 'service mysqld start'

On **OS X**, install the [MySQL Preference Pane](http://creativeeyes.at/tools/mysqlpp.php) to control the MySQL daemon easily. Alternatively, you can run `sudo mysqladmin start` from Terminal.

### Configure Diaspora

For a local development instance, you can skip this step initially.

Otherwise: Diaspora needs to know what host it's running on.  Copy config/app_config.yml.example
to config/app_config.yml, put your external  url into the pod_url field, and make any other
needed configuration changes.

### Run the server

For a local development instance, just run `./script/server`. This will start thin, redis, a resque worker and the websocket server. The application is then available at http://localhost:3000. You can change port by editing config/server.sh.

If you want to run an app server other than thin, you must run the appserver, redis, a resque worker, and the websocket server separately. Check out script/server for details.

### Run the app server

For a local development instance, skip this step - just run `./script/server` to get everything running on the right ports.

Once MySQL is running and bundler has finished, run `bundle exec thin start` from the root Diaspora directory.  This will start the app server in
development mode.  It will run on port 3000 by default. If you want it to be available on port 80, either run it on port 80 directly (probably unwise), or use your webserver of choice (we use nginx) to proxy port 80 at your domain name to thin at port 3000 or over a socket.  See config/sprinkle/conf/nginx.conf and config/thin.yml in the repo for an example thin config and nginx server stanza.

### Run the websocket server and redis

For a local development instance, skip this step - just run `./script/server` to get everything running  on the right ports.

Run` bundle exec ruby script/websocket_server.rb' to start websockets on port 8080. Change port in config/app_config.yml.

Run `redis-server` to start redis on the default port 6379. It uses a config file, normally /etc/redis.conf or /etc/redis/redis.conf defining ports and other stuff.

### Run the resque worker

For a local development instance, skip this step - just run `./script/server` to get everything running  on the right ports.

To start the resque worker run the following command:

        QUEUE=* bundle exec rake resque:work

You can monitor by starting `resque-web` and then visit http://server-ip:5678

### Logging in with a sample user

Run `rake db:seed:dev` (for a development instance). Then you can log in with user `tom` and password `evankorth`.
More details in db/seeds/dev.rb and db/seeds/tom.rb.

There is also db:seed:first_user which let you define the name/pw of a first user.

If you have an error on Mac, try `bundle exec rake db:seed:dev --trace

### Testing

Diaspora's test suite uses [rspec](http://rspec.info/), a behavior driven testing framework. To run all tests: `rake`. Note that some of our tests require a display to be attached; if you just want to run the command-line tests, do `rake spec`.

### Read-only installation

The directories *tmp*, *public/upload* and *log* must be writable by the user running Diaspora even in a read-only installation.

Some of Diaspora's web content in the public/ folder  is generated at runtime. In order to create a read-only installation, this content must be generated at install time instead.

Run sass/haml and create e. g.,  public/stylesheets/{application,ui,sessions}.css:

        rake db:seed:dev
        bundle exec thin -d --pid log/thin.pid start
        wget http://localhost:3000; rm index.html
        bundle exec thin --pid log/thin.pid stop

Run jammit and precache public/assets/*gz files:

        bundle exec jammit

After these commands  also the *public/* folder  can be read-only (although *public/uploads* need to be writable, see above).

### Production mode

Diaspora, beeing a rails application, by default runs in development mode. To gain performance, you might want to run in production mode instead. To do this:

* Edit config/server.sh
* Review config/environments/production.rb. The serve_static_assets setting is known to cause troubles depending on what front-end server (nginx, apache, none) is used.

## How We Do It

We deploy and run Diaspora with a deployment tool called sod, which currently only supports CentOS.  We use Rackspace Cloud, but you can point sod at any CentOS machine.  Sod is unfinished and probably has hardcoded configuration for our servers.  It is **not ready for use**.  DO NOT use a machine that other apps are running on, Sod assumes that it is deploying onto a clean machine.  So first you get yourself an ip and root password to a CentOS machine.  Then get yourself an SSL cert and put it in ~/diaspora_cert on your local machine.  Then you run sod on your local machine to provision the remote server:

        # install ruby 1.8.7, git, bundler
        git clone git://github.com/MikeSofaer/sod.git
        cd sod
        bundle install
        ./sod <remote_machine_ip_address> diaspora diaspora <remote_machine_root_password>

This should take about 40 minutes, and when it's done Diaspora should be running.  You will need to edit config files, etc.

Warning: The version of splunk installed this way only supports 64-bit processors. To work around this, download the 32-bit RPM from the splunk website:

        rpm -Uvh splunk-4.1.6-89596.i386.rpm;  cd /usr/local/bin/;  rm splunk;  ln -s /opt/splunk/bin/splunk .

On a minimal install system you'll need to install bzip2 and vixie-cron for this to work (yum install bzip2 vixie-cron)

For localhost you can skip the SSL cert and just use 127.0.0.1 for the IP and your root password.

To restart the appservers after making a change, ssh in and type `svc -t /service/thin*`