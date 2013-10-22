----

###301 MOVED PERMANENTLY###

We're currently **moving this wiki over to our new project site**. The contents of this page have  already been carried over, so _any new changes here will not be reflected in the new wiki_.  
New link: http://wiki.diasporafoundation.org/Notes_On_Installing_and_Running_Diaspora

----

## Introduction

Diaspora is run on a network of connected servers, or "pods." This document contains technical 
information for installing the necessary software to run a pod yourself, either for development, 
or as a new pod on the Diaspora network.

If you just want to **use** Diaspora, you don't need to set up your own pod -- you can join an 
[[existing pod|Community-supported-pods]] running the Diaspora software. The pod you join could 
be one run by a friend, your university, or the official pod, run by the project’s founders, at 
<a href="http://joindiaspora.com" target="_blank">joindiaspora.com</a>. All of the Diaspora pods 
communicate and make up the Diaspora Network.

If you still want to run your own pod... we salute you. Read on.

**For OS-Specific and service-specific guides, check out [this page](https://github.com/diaspora/diaspora/wiki/Installation-Guides).**

## Things To Know

0. The install is a bit complex. We can help, though. **If you run into problems, please visit us 
in [[IRC|How we use IRC]], on Freenode.**

1. We are developing Diaspora for the latest and greatest browsers, so please update your 
Firefox, Opera, Chrome or Safari to the newest version. We do not currently support any version of 
Internet Explorer, though support is planned in the future.

2. **Diaspora mandates HTTPS**, as it uses OAuth2 flows to connect to apps.  You can get a free SSL certificate from <a href="http://www.startssl.com/" target="_blank">StartSSL</a>.  You'll need to reference the certificate you get from StartSSL in your NGINX/Apache configuration file.

**Note** While you can certainly get up and running with your own pod by using a self-signed SSL certificate, your pod may not be able to communicate with all other pods. It is therefore recommended that you use a certificate issued from a trusted Certificate Authority. Unfortunately, this also means that CaCert certificates won't work. They are not (yet) part of most certificate bundles.

## Pointing out the obvious

We frequently see people doing everything as **root**. If you think that's a good idea: **It's not**. It's the worst thing you can do! All programs will either tell you to run them as root or ask you for the password. **Do not start anything as root if it's not explicitly requested by it or this guide.** Just use your normal user or create an own system user for Diaspora.

## Preparing your system

In order to run Diaspora, you will need to install the following dependencies (specific instructions
follow):

- Build tools - Packages needed to compile the components that follow.
- <a href="http://www.ruby-lang.org" target="_blank">Ruby</a> - The Ruby programming language.  ( **1.9.2-p290** or later).
- <a href="http://rubygems.org/" target="_blank">RubyGems</a> - A package manager for Ruby code 
that we use to download libraries ("gems") that Diaspora uses.
- <a href="http://gembundler.com/" target="_blank">Bundler</a> - A gem management tool for Ruby 
projects.
- <a href="http://www.mysql.com" target="_blank">MySQL</a> - Backend storage engine.
- Or: <a href="http://www.postgresql.org/" target="_blank">PostgreSQL</a> - Backend storage engine.
- <a href="http://www.openssl.org/" target="_blank">OpenSSL</a> - An encryption library.
- <a href="http://curl.haxx.se/" target="_blank">libcurl</a> - A library to make HTTP requests 
(and much more).
- <a href="http://www.imagemagick.org/" target="_blank">ImageMagick</a> - An image processing 
library we use to resize uploaded photos.
- <a href="http://git-scm.com/" target="_blank">Git</a> - A version control system, which you 
will need to download the Diaspora source code from GitHub.
- <a href="http://redis.io/" target="_blank">Redis</a> - A persistent key-value store that we 
use via <a href="https://github.com/mperham/sidekiq" target="_blank">Sidekiq</a> for background 
job processing. ( **2.0** or later)
- one of the Javascript runtimes on
<a href="https://github.com/sstephenson/execjs">execjs's supported list</a>.

Feel free to take a look at the [distributions and services](https://github.com/diaspora/diaspora/wiki/Installation-Guides) listed for OS-specific/service-specific instructions to prepare your system.

After you're done following those instructions, come back here and move on to:

## Getting Diaspora Source

Our code is hosted at GitHub (which also hosts the wiki page you're reading). Our test suite is run at <a href="http://travis-ci.org/#!/diaspora/diaspora" target="_blank">Travis CI</a>, you should check build status and verify your Ruby/DB combo are green in the master branch and pass all tests before you pull code.

To get a copy of the Diaspora source from the master branch, use the following command:

    git clone -b master git://github.com/diaspora/diaspora.git && cd diaspora

If you have never used GitHub before, their <a href="http://help.github.com/" target="_blank">help desk</a> 
has a pretty awesome guide for getting set up.

If you already cloned the repository get sure to checkout the master branch with 

    git checkout master

## Installing Diaspora

### Preparations

Depending on the database you want to use, add either `DB="mysql"` for MySQL or `DB="postgres"` for PostgreSQL before each command starting with `bundle…`, or export the environment variable: `export DB="mysql"` for MySQL or `export DB="postgres"`. (If you want to have both database types available for easy switching, you can either skip this step or use `DB="all"`.)

### Install required gems

To start the app server for the first time, you need to use Bundler to install
Diaspora's gem depencencies.  Run (from Diaspora's root directory):

    bundle install --without development test heroku

Bundler will also warn you if there is a new dependency and you
need to bundle install again.

NOTE: If you don't get a **green success line** at the end, double check if you've installed all dependencies. If you can't figure it out feel free to ask for help at the mailing list or the [[IRC Channel| How we use IRC]].

NOTE: If you want to do any development run just `bundle install`

NOTE: If you are on Ruby 1.9.2 and get an error such as "invalid byte sequence in US-ASCII (ArgumentError)" then you need to set your system locale to UTF-8. [This GitHub bug report](https://github.com/siefca/i18n-inflector/issues/3) on the gem that causes the problem has steps for doing so on Ubuntu.

NOTE: If you get "Could not get Gemfile" make sure you are in the diaspora directory (`cd diaspora`) you just cloned.

NOTE: If you do any other Rails development on your machine, you will probably
want to either run `bundle install --path vendor` instead to install the gems in your local diaspora
directory to avoid conflicts with your existing environment, or use an [RVM](https://rvm.beginrescueend.com) gemset.

### Configure Diaspora

Diaspora needs to know what host it's running on. Copy `config/diaspora.yml.example`
to `config/diaspora.yml`, put your external url into the `environment.url` field, and make any other
needed configuration changes.

### Background

Diaspora is a Rails-app and as such it has different running modes.
The default is "development mode" in which some performance features such as
source code caching are disabled. The other mode is "production mode" which is best for
actually running a pod. If you want just a test installation to develop for Diaspora,
keep the defaults.  However, if you plan to actually host a pod choose production mode.

If you want to run production mode:

* Change the `environment.assets.serve` setting to `true` in the `config/diaspora.yml` file. With this setting enabled Diaspora can take advantage of Rails' ability to serve static content like images and .css files from the application's /public directory. However, Rails is not a webserver, so a better option would be to leave `environment.assets.serve` set to `false` and instead install a true webserver such as Apache or Nginx alongside Diaspora and modify that webserver’s configuration to serve the static content itself:

#### Apache 2

```apache
<VirtualHost *:80>
  ServerName diaspora.mydomain.com
  DocumentRoot /diaspora_root/public
  <Directory /diaspora_root/public>
      Allow from all
      Options -MultiViews
  </Directory>
</VirtualHost>
```

For a more advanced configuration have a look at this Gist: [https://gist.github.com/719014](https://gist.github.com/719014)

**OSX Server Note** If you wish to use Apache built into OSX Server, use Server Admin to create a site on port 443.  A file should be created in the directory `/etc/apache2/sites/` with a name like "000X_any_443_domain.com.conf".  The above proxy settings will allow you to continue to use your existing web services alongside the Diaspora installation.

#### Nginx

Get inspired by our [[Nginx Configuration]]

#### lighttpd

See [[Using lighttpd as webserver]] for an example configuration.

### Configuring SSL
As noted previously, you will need to configure NGINX to point to your SSL certificate (procured from either <a href="http://startssl.com" target="_blank">StartSSL</a> or <a href="http://www.sslshopper.com/">elsewhere</a>). 

**NOTE:** Certificates issued from StartSSL will probably also require that the StartSSL intermediate certificate be concatenated in order for some pods to communicate properly. The following link will help you create a properly concatenated certificate for use by NGINX: [StartSSL and NGINX](http://blog.dembowski.net/2010/02/25/startssl-and-nginx/)

**NOTE:** If you are serving through a reverse proxy, you will need to set the `X_FORWARDED_PROTO` header to `https` on your reverse proxy server.  The NGINX and Apache example configurations show how to do this.  Failure to set this header will lead to a redirect loop.

Take note: We upgrade all port 80 requests to port 443.  We recommend that you do the same.


**OSX Server Note** If you use startssl to obtain both a private key and a certificate don't forget to decrypt the private key using the following command `openssl rsa -in ssl.key -out ssl.key`.  Import the decrypted key (ssl.key) and a certificate (ssl.crt) file into Server Admin by dragging the files into the Certificate manager found here: Server Admin>Web>Site>example.com>Security>Manage Certificates>Import Certificate Identity.  If the certificate & key are valid the certificate should be 'blue'.  Once imported, the certificate can then be selected as the security for the site. 

**Different certificates** Make sure that your top level domain (e.g. example.com if your pod is pod.example.com) hands out the _same_ certificate as your actual pod URL. The communication with other pods (or applications, like [cubbi.es](http://www.cubbi.es)) might not work otherwise.

### Set up the database

*Note for PostgreSQL users*: If you are running Diaspora with PostgreSQL, beware that having [the ssl setting](http://www.postgresql.org/docs/9.1/interactive/runtime-config-connection.html#GUC-SSL) turned on in the PostgreSQL config has been causing problems for several people.  We recommend turning it off unless you know what you're doing.

You need to configure the database settings. Copy config/database.yml.example to config/database.yml
and edit it properly.

After that, run `bundle exec rake db:create` for development mode or 
`RAILS_ENV=production bundle exec rake db:create` for production mode
to create the needed database (or you could instead create the database manually.) 
If you want to create it manually make sure you choose utf8 as charset and utf8_bin as collation.

Now you need to create the necessary tables. To do so run 
`bundle exec rake db:schema:load` for development mode or
`RAILS_ENV=production bundle exec rake db:schema:load` for production mode.
*Warning:* This will empty your database, on update use `db:migrate` instead of `db:schema:load`!

### Set up services

If you want to connect your pod to other services like Twitter, Tumblr or Facebook read instructions [[howto setup services|Howto-setup-services]].


## Running Diaspora

To turn on the server use the command `./script/server` from the working directory. 

This will start Unicorn and a Sidekiq worker. The application is then available at http://your_pod:3000. You can change the port by either editing config/diaspora.yml or by setting up a reverse proxy (see above) if you want to run Diaspora at a subdomain or use HTTPS more easily.

Note: Ensure your database servers (Redis and MySQL or PostgreSQL) are running before trying to start the server.

If you want to run an app server other than Unicorn or have more control over it, you must run the appserver and a Sidekiq worker separately.

Here are instructions to [[Run Diaspora's components|Run Diasporas Components]]

Once Diaspora is running, just open it up in a web browser and sign up for an account.  

**Note** If you are running a 'production' installation and requests to the /assets directory return a HTTP 404 error to your client, run `RAILS_ENV=production DB="mysql" bundle exec rake assets:precompile` for MySQL or `RAILS_ENV=production DB="postgres" bundle exec rake assets:precompile` for PostgreSQL after each git pull. If you get a 500 page installation try restarting Diaspora after that.

**Note** If you are running a 'production' installation and you do not see any images hosted, but the content loads fine, ensure that your reverse proxy is serving them. If you sure you don't want to setup a reverse proxy ensure that you have set your `environment.assets.serve` setting turned to `true` everywhere in your `diaspora.yml`.

## Updating Diaspora

Read the Changelog! [tester comment: Its good to make a branch or other backup of your current repository before updates. `git branch <my backup branch name>`. Then use `git status`, `git add`, and `git commit` as needed to make sure all of your active work is saved in your working repository before the merge with origins. This will ensure that you are informed of merge conflicts and don't overwrite your own changes with the pull.]

Change into the Diaspora root folder and run

    git pull origin master

If the update changes the Gemfile or Gemfile.lock files, for MySQL run

    DB="mysql" bundle install --without development test heroku

or for PostgreSQL:

    DB="postgres" bundle install --without development test heroku

Now kill your running Diaspora instance.

In order to apply any new schema always run  

    DB="mysql" bundle exec rake db:migrate

[Tester Comment: It is very useful to run the migrate and precompile commands with the --trace switch. For example, DB="mysql" bundle exec rake db:migrate --trace. They are fairly quiet functions.  ]

for MySQL, or

    DB="postgres" bundle exec rake db:migrate

for PostgreSQL.

Or if you you run in production mode

    RAILS_ENV="production" DB="mysql" bundle exec rake db:migrate

for MySQL, or

    RAILS_ENV="production" DB="postgres" bundle exec rake db:migrate

for PostgreSQL.

Now start Diaspora again.

After each update run:

    DB="mysql" bundle exec rake assets:precompile / DB="postgresql" bundle exec rake assets:precompile

## Appendix

### Testing

Normally you don't need this if you aren't developing for Diaspora, just skip it :)

Diaspora’s test suite uses [Rspec](http://rspec.info/), a behavior driven testing framework. To run all tests execute: `rake`. Note that some of our tests require a display to be attached; if you just want to run the command-line tests, do `rake spec`.