## Introduction

Diaspora is run on a network of connected servers, or "pods." This document contains technical information for installing the necessary software to run a pod yourself, either for development, or as a new pod on the Diaspora network.

If you just want to **use** Diaspora, you don't need to set up your own pod -- you can join an [existing pod](https://github.com/diaspora/diaspora/wiki/Community-supported-pods) running the Diaspora software. The pod you join could be one run by a friend, your university, or the official pod, run by the project’s founders, at [joindiaspora.com](http://joindiaspora.com). All of the Diaspora pods communicate and make up the Diaspora Network.

If you still want to run your own pod… we salute you. Read on.

## Things To Know

0. The install is a bit complex. We can help, though. **If you run into problems, please visit us in IRC, on freenode, in [#diaspora](http://webchat.freenode.net/?channels=diaspora).**

1. We are developing Diaspora for the latest and greatest browsers, so please update your Firefox, Chrome or Safari to the newest version. We do not currently support any version of Internet Explorer, though support is planned in the future.

2. On joindiaspora.com, we run the application using [thin](http://code.macournoyer.com/thin/) as our application server and [nginx](http://wiki.nginx.org/Main) as our web server. You can use another application server (passenger, mongrel…), or another web server (apache, unicorn…), but the core team may not have the expertise to help you set it up. There are folks in the community who do run Diaspora this way though, so ask around in irc and on the mailing list.

## Preparing your system

In order to run Diaspora, you will need to install the following dependencies (specific instructions follow):

- Build Tools - Packages needed to compile the components that follow.
- [Ruby](http://www.ruby-lang.org) - The Ruby programming language. (We're developing mostly on **1.8.7**, but we also support **1.9.2**.  Ruby 1.8.7 comes preinstalled on Mac OS X.)
- [MySQL](http://www.mysql.com) - Backend storage engine.
- [OpenSSL](http://www.openssl.org/) - An encryption library. (It comes preinstalled on Mac OS X and Ubuntu.)
- [libcurl](http://curl.haxx.se/) - A library to make HTTP requests (and much more).
- [ImageMagick](http://www.imagemagick.org/) - An image processing library we use to resize uploaded photos.
- [Git](http://git-scm.com/) - A version control system, which you will need to download the Diaspora source code from GitHub.
- [Redis](http://redis.io/) - A persistent key-value store that we use via [resque](https://github.com/defunkt/resque) for background job processing.

After you have Ruby installed on your system, you also need:

- [RubyGems](http://rubygems.org/) - A package manager for Ruby code that we use to download libraries ("gems") that Diaspora uses.
- [Bundler](http://gembundler.com/) - A gem management tool for Ruby projects.

Ok, lets move on. Pick what you need from the following list:

- [[Installing on Ubuntu]]
- [[Installing on Debian]]
- [[Installing on Fedora]]
- [[Installing on Mac OS X]]

After you're done with that, move on to

## Getting Diaspora

Our code is hosted at GitHub (which also hosts the wiki page you're reading). To get a copy of the Diaspora source, use the following command:

        git clone git://github.com/diaspora/diaspora.git

If you have never used GitHub before, their
[help desk](http://help.github.com/) has a pretty awesome guide on getting
set up.

## Installing Diaspora

### Install required gems

To start the app server for the first time, you need to use Bundler to install
Diaspora's gem depencencies.  Run `bundle install` from Diaspora's root
directory.  Bundler will also warn you if there is a new dependency and you
need to bundle install again.

NOTE: If you don't get a **green success line** at the end, double check if you've installed all dependencies. If you can't figure it out feel free to ask for help at the mailing list or the IRC Channel.

NOTE: If you are on Ruby 1.9.2 and get an error such as "invalid byte sequence in US-ASCII (ArgumentError)" then you need to set your system locale to UTF-8. [This GitHub bug report](https://github.com/siefca/i18n-inflector/issues/3) on the gem that causes the problem has steps for doing so on Ubuntu.

NOTE: If you get "Could not get Gemfile" try typing the following first:
`cd diaspora`

NOTE: If you do any other rails development on your machine, you will probably
want to either run `bundle install --path vendor` instead to install the gems in your local diaspora
directory to avoid conflicts with your existing environment, or use an rvm gemset.

NOTE: If you're on an ARM based machine try

        bundle install --without development,test

instead. For details see [Bugreport #829](http://bugs.joindiaspora.com/issues/829).


### Configure Diaspora

Diaspora needs to know what host it's running on. Copy config/app_config.yml.example
to config/app_config.yml, put your external url into the pod_url field, and make any other
needed configuration changes.

### Background

Diaspora is a Rails-app and as such it has different modes it's running in.
The default is the "development mode" in which some performance features such as
source code caching are disabled. The other one is production mode and best for
actually running a pod. So if you want a test installation to develop for Diaspora,
keep the defaults, if you plan to host a pod choose production mode.

If you want to run production mode:

* Edit config/server.sh
* Review config/environments/production.rb. The serve_static_assets setting is known to cause troubles depending on what front-end server (nginx, apache, none) is used.


### Set up the database

You need to configure the database settings. Copy config/database.yml.example to config/database.yml
and edit it properly.

After that run `bundle exec rake db:create` for development mode or 
`RAILs_ENV=production bundle exec rake db:create` for production mode
to create the needed database or create the database manually. 
If you want to create it manually make sure you choose utf8 as charset and utf8_bin as collation.

Now you need to create the necessary tables. To do so run 
`bundle exec rake db:migrate` for development mode or
`RAILS_ENV=production bundle exec rake db:migrate` for production mode.

## Running Diaspora

Just run `./script/server`. This will start thin, redis, a resque worker and the websocket server. The application is then available at http://your_pod:3000. You can change port by editing config/server.sh or setup a reverse proxy (google it ;)) if you want to run diaspora for example at a subdomain or use https more easily.

Note: When `./script/server` starts redis, it reads the `config/redis.yml` file. Make sure that you have write permissons to the log file, which is specified on the line starting with the word `logfile`.


If you want to run an app server other than thin or have more control over it, you must run the appserver, redis, a resque worker, and the websocket server separately.

Here are instructions to [[Run Diasporas Components]]


### Logging in

If you have disabled the registration on your pod there is `bundle exec rake db:seed:first_user`
(RAILS_ENV=production bundle exec rake db:seed:first_user for production mode)
which let you define the name/pw of a first user.

### Jammit

Jammit compiles all the CSS & JS into fewer minimized files. 
The advantage is that the page can be served with less requests. 
Diaspora integration with Jammit is completly optional. 
If you want to use jammit you must install java. 
Otherwise just ignore the warning issued by ./script/server.

## Updating Diaspora

First, kill your running Diaspora instance.

Change into the Diaspora root folder and run

        git pull origin master

If the update changes the Gemfile or Gemfile.lock files, run

        bundle install

In order to apply any new schema always run

        bundle exec rake db:migrate

Or if you you run in production mode

        RAILS_ENV=production bundle exec rake db:migrate

Now start Diaspora again.

If you once used Jammit, now after each update, after the first request to the page run it again:

        bundle exec jammit

## Appendix

### Testing

Normally you don't need this if you aren't developing for diaspora, just skip it :)

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
