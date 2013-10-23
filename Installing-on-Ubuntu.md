### Preparations
You will need to make sure that your username is on the sudo authorized list located at '/etc/sudoers'.

## Step 1

### Install everything from APT (more info in the Appendix):

If you're going through a proxy, add the following to the ~/.curlrc file:

    proxy=host:port
    proxy-user=username:password

This is for Ubuntu 10.04. There are other steps than just this one:

    sudo apt-get install build-essential libxslt1.1 libxslt1-dev libxml2 ruby-full mysql-server libmysqlclient-dev libmysql-ruby libssl-dev libopenssl-ruby libcurl4-openssl-dev imagemagick libmagickwand-dev git-core redis-server libffi-dev libffi-ruby rubygems libsqlite3-dev libpq-dev libreadline5-dev

NodeJS needs to be built from source on 10.04.

    git clone git://github.com/ry/node.git
    cd node
    ./configure
    make
    sudo make install

This is for Ubuntu 10.10. There are other steps than just this one:

    sudo apt-get install build-essential libxslt1.1 libxslt1-dev libxml2 ruby-full mysql-server libmysqlclient-dev libmysql-ruby libssl-dev libopenssl-ruby libcurl4-openssl-dev imagemagick libmagickwand-dev git-core redis-server libffi-dev libffi-ruby rubygems libsqlite3-dev libpq-dev libreadline5-dev nodejs

This is for Ubuntu 11.10. There are other steps than just this one:

    sudo apt-get install build-essential libxslt1.1 libxslt1-dev libxml2 ruby-full mysql-server libmysqlclient-dev libmysql-ruby libssl-dev libopenssl-ruby libcurl4-openssl-dev imagemagick libmagickwand-dev git-core redis-server libffi-dev libffi-ruby rubygems libsqlite3-dev libpq-dev libreadline-gplv2-dev openjdk-7-jre nodejs

This is for Ubuntu 12.04. There are other steps than just this one:

    sudo apt-get install build-essential libxslt1.1 libxslt1-dev libxml2 ruby-full mysql-server libmysqlclient-dev libmysql-ruby libssl-dev libopenssl-ruby libcurl4-openssl-dev imagemagick libmagickwand-dev git-core redis-server libffi-dev libffi-ruby rubygems libsqlite3-dev libpq-dev libreadline5 openjdk-7-jre nodejs libncurses5-dev

For all three ubuntu releases run this if you are not going to use RVM:

    wget http://ftp.us.debian.org/debian/pool/main/r/rubygems/rubygems_1.8.24-1_all.deb -O rubygems.deb && sudo dpkg -i rubygems.deb

### Install RVM (optional but currently recommended)

You can install Ruby on a clean per user basis via [RVM](https://rvm.io/). This is currently recommended to get the latest Rubygems version.

To install RVM and Ruby 1.9.3, as your normal user (the one which Diaspora should run under), run

_It is recommended that you install rvm from source since Ubuntu's version is lagging._

To install rvm from source, execute the following command:

    curl -L https://get.rvm.io | bash -s stable --ruby

Now read `rvm requirements` and ensure `libyaml-dev` is installed. Then install Ruby via RVM:

```bash
rvm install 1.9.3-p429
rvm use 1.9.3-p429@global
```

[Tester comment: After installing RVM it is important to make sure the script is sourced in your .bashrc file. <code>source ~/.rvm/scripts/rvm</code>. When successful the command <code >type rvm | head -n 1 </code> will return "rvm is a function". This line may not always be added by the install script."

### Start MySQL (optional, depending on your platform):

    sudo service mysql start

## Step 2

### Bundler

To install Bundler, run the following, if you aren't using RVM you need to do it as root (prepend it with `sudo`):

    gem install bundler --no-ri --no-rdoc 

To get bundle to work (**bundle install** step later), you might need to make a symbolic link if you aren't using RVM:

    sudo ln -s /var/lib/gems/1.8/bin/bundle /usr/local/bin/bundle


## Done

Congrats! You have all your dependencies installed. Go back to [Installing and Running Diaspora](https://github.com/diaspora/diaspora/wiki/Notes-on-Installing-and-Running-Diaspora).




## Appendix


### Build Tools

To install build tools, run the following (includes the gcc and xml parsing dependencies):

    sudo apt-get install build-essential libxslt1.1 libxslt1-dev libxml2

### Ruby

To install Ruby 1.8.7, run the following command:

    sudo apt-get install ruby-full

Please note that you need to have Universe enabled in your
/etc/apt/sources.list file to install ruby using apt-get.

### MySQL

This installs MySQL, you also need the libmysqlclient-dev and libmysql-ruby packages.

    sudo apt-get install mysql-server libmysqlclient-dev libmysql-ruby

To start MySQL run

    sudo service mysql start

### sqlite

    sudo apt-get install libsqlite3-dev

### OpenSSL

You already have OpenSSL installed but you need the libssl-dev and libopenssl-ruby package too:

    sudo apt-get install libssl-dev libopenssl-ruby

### libcurl

You need to install the dev headers.

To install them, run the following:

    sudo apt-get install libcurl4-openssl-dev

### ImageMagick

To install ImageMagick, run the following:

    sudo apt-get install imagemagick libmagick9-dev

If you're on **Ubuntu 10.10 or 12.04** use the following instead:

    sudo apt-get install imagemagick libmagickwand-dev

### Git

To install Git, run the following:

    sudo apt-get install git-core

### Redis

To install Redis, run the following:

    sudo apt-get install redis-server

### ffi

Note: If you get an error in the next step try to run

    sudo apt-get install libffi-dev libffi-ruby

and try the step again.


### RubyGems

To install RubyGems run

    wget http://ftp.us.debian.org/debian/pool/main/r/rubygems/rubygems_1.8.24-1_all.deb -O rubygems.deb && sudo dpkg -i rubygems.deb
