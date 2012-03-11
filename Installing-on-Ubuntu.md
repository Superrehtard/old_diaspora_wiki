### Preparations
You will need to make sure that your username is on the sudo authorized list located at '/etc/sudoers'.

## Step 1

### Install everything from APT (more info in the Appendix):

This is for Ubuntu 10.10. There are other steps than just this one:

    sudo apt-get install build-essential libxslt1.1 libxslt1-dev libxml2 ruby-full mysql-server libmysqlclient-dev libmysql-ruby libssl-dev libopenssl-ruby libcurl4-openssl-dev imagemagick libmagickwand-dev git-core redis-server libffi-dev libffi-ruby rubygems libsqlite3-dev libpq-dev libreadline5-dev

This is for Ubuntu 11.10. There are other steps than just this one:

    sudo apt-get install build-essential libxslt1.1 libxslt1-dev libxml2 ruby-full mysql-server libmysqlclient-dev libmysql-ruby libssl-dev libopenssl-ruby libcurl4-openssl-dev imagemagick libmagickwand-dev git-core redis-server libffi-dev libffi-ruby rubygems libsqlite3-dev libpq-dev libreadline-gplv2-dev openjdk-7-jre

For both run this if you are not going to use RVM:

    wget http://ftp.us.debian.org/debian/pool/main/r/rubygems/rubygems_1.8.15-1_all.deb -O rubygems.deb && sudo dpkg -i rubygems.deb

### Install RVM (optional but currently recommended)

You can install Ruby on a clean per user basis via [RVM](https://rvm.beginrescueend.com/). This is currently recommended to get the latest Rubygems version.

To install RVM and Ruby 1.9.2, as your normal user (the one which Diaspora should run under), run

```bash
bash -s stable < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer)
echo ''[[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm" # Load RVM function' >> ~/.bashrc
source ~/.bashrc
rvm install ruby-1.9.2-p290
rvm use ruby-1.9.2-p290@global
```

### Start MySQL (optional, depending on your platform):

    sudo service mysql start

## Step 2

### Bundler

To install Bundler, run the following, if you aren't using RVM you need to do it as root (prepend it with `sudo`):

    gem install bundler --no-ri --no-rdoc 

To get bundle to work (**bundle install** step later), you might need to make a symbolic link if you aren't using RVM:

    sudo ln -s /var/lib/gems/1.8/bin/bundle /usr/local/bin/bundle


## Done

Congrats! You have all your dependencies installed. Go back to [[Installing and Running Diaspora]].




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

If you're on **Ubuntu 10.10** use the following instead:

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

    wget http://ftp.us.debian.org/debian/pool/main/r/rubygems/rubygems_1.8.15-1_all.deb -O rubygems.deb && sudo dpkg -i rubygems.deb
