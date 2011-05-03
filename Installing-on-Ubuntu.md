### Versions

These instructions are for Ubuntu 10.04 or 10.10.

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

### RubyGems

On **Ubuntu 10.04**, run the following:

        sudo add-apt-repository ppa:maco.m/ruby
        sudo apt-get update
        sudo apt-get install rubygems

This PPA is maintained by an Ubuntu Developer.

If you are running **Ubuntu Server**, you might get an error that looks like:

        sudo: add-apt-repository: command not found

If this happens, you must first install python-software-properties, which contains the add-apt-repository command:

        sudo apt-get install python-software-properties

On **Ubuntu 10.10**, run the following:

        sudo apt-get install rubygems

You may need to install libxsl first: http://nokogiri.org/tutorials/installing_nokogiri.html


### Bundler

To install Bundler, run the following:

        sudo gem install bundler 

To get bundle work, you might make a symbolic link:

        sudo ln -s /var/lib/gems/1.8/bin/bundle /usr/local/bin/bundle

### ffi

Note: If you get an error in the next step try to run

        sudo apt-get install libffi-dev libffi-ruby

and try the step again.

### Install everything from APT in one step

This is for Ubuntu 10.10. There are other steps than just this one:

        sudo apt-get install build-essential libxslt1.1 libxslt1-dev libxml2 ruby-full mysql-server libmysqlclient-dev libmysql-ruby libssl-dev libopenssl-ruby libcurl4-openssl-dev imagemagick libmagickwand-dev git-core redis-server libffi-dev libffi-ruby


Congrats! You have all your dependencies installed. Go back to [[Installing and Running Diaspora]].
