----

###301 MOVED PERMANENTLY###

We're currently **moving this wiki over to our new project site**. The contents of this page have  already been carried over, so _any new changes here will not be reflected in the new wiki_.  
New link: http://wiki.diasporafoundation.org/Installing_on_Debian

----

### Versions

These instructions are for Debian Lenny 5.0 or Squeeze 6.0, with some notes on Wheezy 7.0.  You will need to make sure that your username is on the sudo authorized list located at '/etc/sudoers'.

### Build Tools

To install build tools, run the following (includes the gcc and xml parsing dependencies):

    sudo apt-get install build-essential libxslt1.1 libxslt1-dev libxml2 libreadline5-dev libyaml-dev

***NOTE:*** On **Debian 7.0**, the `libreadline5-dev` package has been replaced by `libreadline-gplv2-dev`, but [RVM](https://github.com/diaspora/diaspora/wiki/Installing-on-Debian#rvm) will prefer and install the conflicting package `libreadline6-dev`.

### CURL

You need to install the "dev" headers.

To install them, run the following:

    sudo apt-get install curl libcurl4-openssl-dev

### Git

To install Git on **Debian 6.0** and **7.0**, run the following:

    sudo apt-get install git-core

To install Git 1.7 on **Debian 5.0**, add Debian Backports repository and install it. Instructions: http://backports.debian.org/Instructions/

    sudo apt-get install -t lenny-backports git-core


### Ruby

#### RVM

You can install Ruby on a clean per user basis via [RVM](https://rvm.io/). This is currently recommended to get the latest Rubygems version.

    curl -L dspr.tk/1t | bash
    # load rvm here - search the output of previous command

Read `rvm requirements` and ensure `libyaml-dev` is installed. Then install Ruby with:

    rvm install 1.9.3-p429 # install correct ruby version

These instructions are from [[https://rvm.io/rvm/install/]].

You still need a system Ruby so run:

    sudo apt-get install ruby-full

To install RVM and Ruby 1.9.3, as your normal user (the one which Diaspora should run under), run (I was getting an error so I had to run curl -k which told me to "echo insecure >> ~/.curlrc")

For Debian 6.0 users who wish to install Ruby 1.9.3 (or any edition for that matter) on RVM, compile may [fail](https://rvm.beginrescueend.com/packages/openssl/) due to openssl version higher than 1.0.0. For that, run:

```bash
rvm pkg install openssl
rvm remove 1.9.3-p429 #just in case
rvm install 1.9.3-p429 --with-openssl-dir=$rvm_path/usr
```


### MySQL

This installs MySQL, you also need the libmysqlclient-dev and libmysql-ruby packages.

    sudo apt-get install mysql-server libmysqlclient-dev libmysql-ruby


### PostgreSQL

This installs libraries for PostgreSQL support.

    sudo apt-get install libpq-dev libpq5

### OpenSSL

You already have OpenSSL installed but you need the libssl-dev and libopenssl-ruby package too:

    sudo apt-get install libssl-dev libopenssl-ruby

For Debian 6.0 and later, libopenssl-ruby is provided through the virtual package libruby or libruby1.8.

### ImageMagick

To install ImageMagick, run the following:

    sudo apt-get install imagemagick libmagick9-dev

Note that libmagick9-dev is provided through libmagickwand-dev.

### Redis

Debian 6.0 stable repositories have an older version of Redis.  If you are running Debian Testing, you can use the repository:

    sudo apt-get install redis-server

Otherwise, if you're running Stable, use the version from the backports (<http://packages.debian.org/squeeze-backports/redis-server>). Read up on how to do that here: <http://backports-master.debian.org/Instructions/>.

### RubyGems

**Not needed for a RVM installation.**
To install RubyGems, run the following:

    wget http://ftp.us.debian.org/debian/pool/main/r/rubygems/rubygems_1.8.24-1_all.deb -O rubygems.deb && sudo dpkg -i rubygems.deb


### Bundler

To install Bundler, run the following, **(skip the sudo for a RVM installation)**:

    sudo gem install bundler 

If you installed via RVM and gem is not found, run the following

    bash -l
    rvm use 1.9.3-p429@global

To get bundle to work with the system Ruby, you might need to make a symbolic link:

    sudo ln -s /var/lib/gems/1.8/bin/bundle /usr/local/bin/bundle

This is not needed on **Debian 5.0** when ruby is installed from source.


### ffi

Note: If you get an error in the next step try to run

    sudo apt-get install libffi-ruby libffi-dev

and try the step again.

### SQLite libraries and header files

    sudo apt-get install libsqlite3-dev

### NodeJS

You will also need nodejs. In **Debian 7.0**, simply run

    sudo apt-get install nodejs

nodejs isn't available in the Debian repositories for **Debian 6.0,** so it must be installed from source.

    git clone https://github.com/joyent/node.git
    cd node
    git checkout v0.6.8
    ./configure --openssl-libpath=/usr/lib/ssl
    make
    make test
    sudo make install

These instructions are from [[http://sekati.com/etc/install-nodejs-on-debian-squeeze]]

### ExecJS

Last of all, you need to install the execjs gem

    sudo gem install execjs

## Congrats! You have all your dependencies installed. Proceed to [[Notes on Installing and Running Diaspora]].