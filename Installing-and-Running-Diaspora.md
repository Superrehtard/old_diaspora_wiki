## Introduction

Diaspora is run on a network of connected servers, or "pods." This document contains technical 
information for installing the necessary software to run a pod yourself, either for development, 
or as a new pod on the Diaspora network.

If you just want to **use** Diaspora, you don't need to set up your own pod -- you can join an 
[[existing pod|Community-supported-pods]] running the Diaspora software. The pod you join could 
be one run by a friend, your university, or the official pod, run by the project’s founders, at 
<a href="http://joindiaspora.com" target="_blank">joindiaspora.com</a>. All of the Diaspora pods 
communicate and make up the Diaspora Network.

If you still want to run your own pod...we salute you. Read on.

## Things To Know

0. The install is a bit complex. We can help, though. **If you run into problems, please visit us 
in [[IRC|How we use IRC]], on Freenode.**

1. We are developing Diaspora for the latest and greatest browsers, so please update your 
Firefox, Opera, Chrome or Safari to the newest version. We do not currently support any version of 
Internet Explorer, though support is planned in the future.

2. On joindiaspora.com, we run the application using <a href="http://code.macournoyer.com/thin/" target="_blank">Thin</a> 
as our application server and <a href="http://wiki.nginx.org/Main" target="_blank">Nginx</a> as 
our web server. You can use another application server (Passenger, Mongrel...), or another web 
server (Apache, Unicorn...), but the core team may not have the expertise to help you set it up. Same goes for the database, currently we use MySQL but we're close to supporting PostgreSQL as well, it might work already.
There are folks in the community who do run Diaspora this way though, so ask around in the IRC and 
on the mailing list.

3. **Diaspora mandates HTTPS**, as it uses OAuth2 flows to connect to apps.  You can get a free SSL certificate from <a href="http://www.startssl.com/" target="_blank">StartSSL</a>.  You'll need to reference the certificate you get from StartSSL in your NGINX/Apache configuration file.

**Note** While you can certainly get up and running with your own pod by using a self-signed SSL certificate, your pod may not be able to communicate with all other pods. It is therefore recommended that you use a certificate issued from a trusted Certificate Authority. Unfortunately, this also means that CaCert certificates won't work. They are not (yet) part of most certificate bundles.

## Pointing out the obvious

We frequently see people doing everything as **root**. If you think that's a good idea: **It's not**. It's the worst thing you can do! All programs will either tell you to run them as root or ask you for the password. **Do not start anything as root if it's not explicitly requested by it or this guide.** Just use your normal user or create an own system user for Diaspora.

## Preparing your system

In order to run Diaspora, you will need to install the following dependencies (specific instructions
follow):

- Build tools - Packages needed to compile the components that follow.
- <a href="http://www.ruby-lang.org" target="_blank">Ruby</a> - The Ruby programming language. 
(We're developing mostly on **1.9.2** and support for **1.8.7** will be dropped soon.)
- <a href="http://rubygems.org/" target="_blank">RubyGems</a> - A package manager for Ruby code 
that we use to download libraries ("gems") that Diaspora uses.
- <a href="http://gembundler.com/" target="_blank">Bundler</a> - A gem management tool for Ruby 
projects.
- <a href="http://www.mysql.com" target="_blank">MySQL</a> - Backend storage engine.
- Or: <a href="http://www.postgresql.org/" target="_blank">PostgreSQL</a> - Backend storage engine.
- <a href="http://www.sqlite.org" target="_blank">SQLite3</a> - Relational database management system
- <a href="http://www.openssl.org/" target="_blank">OpenSSL</a> - An encryption library.
- <a href="http://curl.haxx.se/" target="_blank">libcurl</a> - A library to make HTTP requests 
(and much more).
- <a href="http://www.imagemagick.org/" target="_blank">ImageMagick</a> - An image processing 
library we use to resize uploaded photos.
- <a href="http://git-scm.com/" target="_blank">Git</a> - A version control system, which you 
will need to download the Diaspora source code from GitHub.
- <a href="http://redis.io/" target="_blank">Redis</a> - A persistent key-value store that we 
use via <a href="https://github.com/defunkt/resque" target="_blank">Resque</a> for background 
job processing.

It looks like a big list, but really, it's not too bad. To get started, pick your operating system 
from this list:

- [[Installing Diaspora dependencies on Ubuntu|Installing on Ubuntu]]
- [[Installing Diaspora dependencies on Debian|Installing on Debian]]
- [[Installing Diaspora dependencies on Fedora|Installing on Fedora]]
- [[Installing Diaspora dependencies on OS X|Installing on Mac OS X]]
- [Installing Diaspora on Archlinux](https://wiki.archlinux.org/index.php/Diaspora)
- [[Installing Diaspora on Windows|Installing on Windows]]


or select your hosting provider from this list:

- [[Installing Diaspora dependencies on Dreamhost|Installing on Dreamhost]]

If you don't see your system on here, don't give up. It should work similarly on most distributions. If you figure it out we'd love it when you add a guide here, even a "do the same as for xy" would help.

After you're done following those instructions, come back here and move on to:

## Getting Diaspora Source

Our code is hosted at GitHub (which also hosts the wiki page you're reading). Our test suite is run at <a href="http://travis-ci.org/#!/diaspora/diaspora" target="_blank">Travis CI</a>, you should check build status and verify your Ruby/DB combo are green and pass all tests before you pull code.

To get a copy of the Diaspora source, use the following command:

        git clone git://github.com/diaspora/diaspora.git
        cd diaspora

If you have never used GitHub before, their <a href="http://help.github.com/" target="_blank">help desk</a> 
has a pretty awesome guide for getting set up.

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

NOTE: If you do any other rails development on your machine, you will probably
want to either run `bundle install --path vendor` instead to install the gems in your local diaspora
directory to avoid conflicts with your existing environment, or use an [RVM](https://rvm.beginrescueend.com) gemset.


### Configure Diaspora

Diaspora needs to know what host it's running on. Copy config/application.yml.example
to config/application.yml, put your external url into the pod_url field, and make any other
needed configuration changes.

To use `./script/server` copy config/script_server.yml.example to config/script_server.yml and edit it properly.


### Background

Diaspora is a Rails-app and as such it has different running modes.
The default is "development mode" in which some performance features such as
source code caching are disabled. The other mode is "production mode" which is best for
actually running a pod. If you want just a test installation to develop for Diaspora,
keep the defaults.  However, if you plan to actually host a pod choose production mode.

If you want to run production mode:

* Edit rails_env in the script_server section in config/script_server.yml
* Change the "serve_static_assets" setting to "true" in the config/environments/production.rb file. With this setting enabled Diaspora can take advantage of Rails' ability to serve static content like images and .css files from the application's /public directory. However, Rails is not a webserver, so a better option would be to leave "serve_static_assets" set to "false" and instead install a true webserver such as Apache or Nginx alongside Diaspora and modify that webserver's configuration to serve the static content itself:

#### Apache 2

    <VirtualHost *:80>
      ServerName diaspora.mydomain.com
      DocumentRoot /diaspora_root/public
      <Directory /diaspora_root/public>
          Allow from all
          Options -MultiViews
      </Directory>
    </VirtualHost>

For a more advanced configuration have a look at this Gist: https://gist.github.com/719014

**OSX Server Note** If you wish to use Apache built into OSX Server, use Server Admin to create a site on port 443.  A file should be created in the directory `/etc/apache2/sites/` with a name like "000X_any_443_domain.com.conf".  The above proxy settings will allow you to continue to use your existing web services alongside the Diaspora installation.

#### Nginx

Get inspired by our [[Nginx Configuration]]

#### lighttpd

See [[Using lighttpd as webserver]] for an example configuration.

### Configuring SSL
As noted previously, you will need to configure NGINX to point to your SSL certificate (procured from either <a href="http://startssl.com" target="_blank">StartSSL</a> or <a href="http://www.sslshopper.com/">elsewhere</a>). 

**NOTE:** Certificates issued from StartSSL will probably also require that the StartSSL intermediate certificate be concatenated in order for some pods to communicate properly. The following link will help you create a properly concatenated certificate for use by NGINX: [StartSSL and NGINX](http://blog.dembowski.net/2010/02/25/startssl-and-nginx/)

**NOTE:** If you are serving through a reverse proxy, you will need to set the X_FORWARDED_PROTO header to https on your reverse proxy server.  The NGINX and apache example configurations show how to do this.  Failure to set this header will lead to a redirect loop.

Take note: We upgrade all port 80 requests to port 443.  We recommend that you do the same.


**OSX Server Note** If you use startssl to obtain both a private key and a certificate don't forget to decrypt the private key using the following command `openssl rsa -in ssl.key -out ssl.key`.  Import the decrypted key (ssl.key) and a certificate (ssl.crt) file into Server Admin by dragging the files into the Certificate manager found here: Server Admin>Web>Site>example.com>Security>Manage Certificates>Import Certificate Identity.  If the certificate & key are valid the certificate should be 'blue'.  Once imported, the certificate can then be selected as the security for the site. 

**Different certificates** Make sure that your top level domain (e.g. example.com if your pod is pod.example.com) hands out the _same_ certificate as your actual pod URL. The communication with other pods (or applications, like [cubbi.es](http://www.cubbi.es)) might not work otherwise.

### Configuring WebSockets
WebSockets is required to have instant notifications and similar services, but you can completely live without it. If you fancy it see [[this page|WebSockets]].

**NOTE:** the use of WebSockets has been suspended for now

### Load-balancing with a Thin cluster and Nginx

To improve the performance on large-scale pods, it makes sense to run many thin servers and cluster them for load-balancing. Add the parameters `--servers n -R config.ru` to the list of `default_thin_args` in `config/script_server.yml`, where *n* is the number of thin servers you like to cluster:

     default_thin_args: "--servers 5 -R config.ru -p $THIN_PORT -e $RAILS_ENV"

This will instruct the `./script/server` script to run thin instances on $THIN_PORT and the next *n-1* ports. Make sure that your Nginx configuration knows about these servers by adding them to the list of upstream servers. E.g.: if the thin port is 3000 and you want to cluster five servers, the upstream section of your Nginx configuration should look like this:

    upstream diaspora_thin_cluster {
        server localhost:3000;
        server localhost:3001;
        server localhost:3002;
        server localhost:3003;
        server localhost:3004;
    }


### Set up the database

*Note for PostgreSQL users*: If you are running Diaspora with PostgreSQL, beware that having [the ssl setting](http://www.postgresql.org/docs/9.1/interactive/runtime-config-connection.html#GUC-SSL) turned on in the PostgreSQL config has been causing problems for several people.  We recommend turning it off unless you know what you're doing.

You need to configure the database settings. Copy config/database.yml.example to config/database.yml
and edit it properly.

After that, run `bundle exec rake db:create` for development mode or 
`RAILS_ENV=production bundle exec rake db:create` for production mode
to create the needed database (or you could instead create the database manually.) 
If you want to create it manually make sure you choose utf8 as charset and utf8_bin as collation.

Now you need to create the necessary tables. To do so run 
`bundle exec rake db:migrate` for development mode or
`RAILS_ENV=production bundle exec rake db:migrate` for production mode.

### Set up services

If you want to connect your pod to other services like Twitter, Tumblr or Facebook read instructions [[howto setup services|Howto-setup-services]].


## Running Diaspora

To turn on the server use the command `./script/server`. This will start Thin, a Resque worker and the Websocket server. The application is then available at http://your_pod:3000. You can change the port by either editing thin_port in config/script_server.yml or by setting up a reverse proxy (see above) if you want to run Diaspora at a subdomain or use HTTPS more easily.

Note: Ensure your database servers (Redis and MySQL or PostgreSQL) are running before trying to start the server.

If you want to run an app server other than Thin or have more control over it, you must run the appserver, a Resque worker, and the Websocket server separately.

Here are instructions to [[Run Diaspora's components|Run Diasporas Components]]

Once Diaspora is running, just open it up in a web browser and sign up for an account.  

**Note** If you are running a 'production' installation and requests to the /assets directory return a HTTP 404 error to your client, run `RAILS_ENV=production DB="mysql" bundle exec jammit` for MySQL or `RAILS_ENV=production DB="postgres" bundle exec jammit` for PostgreSQL after the first request to the page after each git pull

**Note** If you are running a 'production' installation and you do not see any images hosted, but the content loads fine, ensure that you have set to True the variable "serve_static_assets" in the config/environments/production.rb file.

### Jammit

Diaspora requires Jammit for the default production setup, and Jammit in turn requires a working install of java. Jammit compiles all the CSS & JS into fewer minimized files. The advantage is that the page can be served with less requests.

## Updating Diaspora

First, kill your running Diaspora instance.

Change into the Diaspora root folder and run

        git pull origin master

If the update changes the Gemfile or Gemfile.lock files, for MySQL run

        DB="mysql" bundle install --without development test heroku

or for PostgreSQL:

        DB="postgres" bundle install --without development test heroku

In order to apply any new schema always run

        DB="mysql" bundle exec rake db:migrate

for MySQL, or

        DB="postgres" bundle exec rake db:migrate

for PostgreSQL.

Or if you you run in production mode

        RAILS_ENV="production" DB="mysql" bundle exec rake db:migrate

for MySQL, or

        RAILS_ENV="production" DB="postgres" bundle exec rake db:migrate

for PostgreSQL.

Now start Diaspora again.

If you use Jammit, after each update and after the first request to a page run it again:

        DB="mysql" bundle exec jammit / DB="postgresql" bundle exec jammit

## Appendix

### Testing

Normally you don't need this if you aren't developing for Diaspora, just skip it :)

Diaspora's test suite uses [Rspec](http://rspec.info/), a behavior driven testing framework. To run all tests execute: `rake`. Note that some of our tests require a display to be attached; if you just want to run the command-line tests, do `rake spec`.

### Read-only installation

The directories *tmp*, *public/uploads* and *log* must be writable by the user running Diaspora even in a read-only installation.

Some of Diaspora's web content in the public/ folder  is generated at runtime. In order to create a read-only installation, this content must be generated at install time instead.

Run sass/haml and create e. g.,  public/stylesheets/{application,ui,sessions}.css:

        bundle exec thin -d --pid log/thin.pid start
        wget http://localhost:3000; rm index.html
        bundle exec thin --pid log/thin.pid stop

Run jammit and precache public/assets/*gz files:

        bundle exec jammit

After these commands  also the *public/* folder  can be read-only (although *public/uploads* need to be writable, see above).
