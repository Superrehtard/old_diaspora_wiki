### Versions

These instructions are for Debian Lenny 5.0 or Squeeze 6.0.

### Build Tools

To install build tools, run the following (includes the gcc and xml parsing dependencies):

        sudo apt-get install build-essential libxslt1.1 libxslt1-dev libxml2

### Ruby

To install Ruby 1.8.7 on **Debian 6.0**, run the following command:

        sudo apt-get install ruby-full

To install Ruby 1.8.7 (There are known bugs if you use Ruby 1.9.x, see [Bug #998](http://bugs.joindiaspora.com/issues/998)) on **Debian 5.0** from source, run the following commands:

        cd /tmp
        wget ftp://ftp.ruby-lang.org//pub/ruby/ruby-1.8.7-p334.tar.gz
        tar xzf ruby-1.8.7-p334.tar.gz
        cd ruby-1.8.7-p334
        ./configure --prefix=/usr
        make
        make install

Alternativly if you have it installed you can use "checkinstall" instead of "make install" to install ruby in a more debian friendly way.

### MySQL

This installs MySQL, you also need the libmysqlclient-dev and libmysql-ruby packages.

        sudo apt-get install mysql-server libmysqlclient-dev libmysql-ruby

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

### Git

To install Git on **Debian 6.0**, run the following:

        sudo apt-get install git-core

To install Git 1.7 on **Debian 5.0**, add Debian Backports repository and install it. Instructions: http://backports.debian.org/Instructions/

        sudo apt-get install git-core

### Redis

If you're running a 64-bit system, run:

        wget http://ftp.us.debian.org/debian/pool/main/r/redis/redis-server_2.2.2-1_amd64.deb

If you're running a 32-bit system, run:

        wget http://ftp.us.debian.org/debian/pool/main/r/redis/redis-server_2.2.2-1_i386.deb

Then install the corresponding package

        sudo dpkg -i redis-server_2.2.2-1_amd64.deb

### RubyGems

To install RubyGems, run the following:

        sudo apt-get install rubygems

### Bundler

To install Bundler, run the following:

        sudo gem install bundler 

To get bundle work, you might make a symbolic link:

        sudo ln -s /var/lib/gems/1.8/bin/bundle /usr/local/bin/bundle

This is not needed on **Debian 5.0** when ruby is installed from source.


### ffi

Note: If you get an error in the next step try to run

        sudo apt-get install libffi-ruby

and try the step again.

Congrats! You have all your dependencies installed. Go back to [[Installing and Running Diaspora]].
