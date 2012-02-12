### Versions

These instructions are for Debian Lenny 5.0 or Squeeze 6.0.  You will need to make sure that your username is on the sudo authorized list located at '/etc/sudoers'.

### Build Tools

To install build tools, run the following (includes the gcc and xml parsing dependencies):

    sudo apt-get install build-essential libxslt1.1 libxslt1-dev libxml2 libreadline5-dev

### CURL

You need to install the dev headers.

To install them, run the following:

    sudo apt-get install curl libcurl4-openssl-dev

### Git

To install Git on **Debian 6.0**, run the following:

    sudo apt-get install git-core

To install Git 1.7 on **Debian 5.0**, add Debian Backports repository and install it. Instructions: http://backports.debian.org/Instructions/

    sudo apt-get install -t lenny-backports git-core


### Ruby

#### RVM

You can install Ruby on a clean per user basis via [RVM](https://rvm.beginrescueend.com/). This is currently recommended to get the latest Rubygems version.

You still need a system Ruby so run:

    sudo apt-get install ruby-full

To install RVM and Ruby 1.9.2, as your normal user (the one which Diaspora should run under), run (I was getting an error so I had to run curl -k which told me to "echo insecure >> ~/.curlrc")

    bash -s stable < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer)
    echo "'[[ -s \"$HOME/.rvm/scripts/rvm\" ]] && source \"$HOME/.rvm/scripts/rvm\"  # This loads RVM into a shell session." >> ~/.bashrc
    source ~/.bashrc
    rvm install ruby-1.9.2-p290
    rvm use ruby-1.9.2-p290@global


For Debian 6.0 users who wish to install Ruby 1.9.2 (or any edition for that matter) on RVM, compile may [fail](https://rvm.beginrescueend.com/packages/openssl/) due to openssl version higher than 1.0.0. For that, run:

    rvm pkg install openssl
    rvm remove ruby-1.9.2-p290 #just in case
    rvm install ruby-1.9.2-p290 --with-openssl-dir=$rvm_path/usr

#### System Ruby
Alternatively, to install Ruby 1.8.7 on **Debian 6.0**, run the following command:

    sudo apt-get install ruby-full

To install Ruby 1.8.7 (There are known bugs if you use Ruby 1.9.x, see [Bug #998](http://bugs.joindiaspora.com/issues/998)) on **Debian 5.0** from source, run the following commands:

    cd /tmp
    wget ftp://ftp.ruby-lang.org//pub/ruby/ruby-1.8.7-p334.tar.gz
    tar xzf ruby-1.8.7-p334.tar.gz
    cd ruby-1.8.7-p334
    ./configure --prefix=/usr
    make
    make install

Alternatively if you have it installed you can use "checkinstall" instead of "make install" to install ruby in a more debian friendly way.

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

Otherwise, if you're running Stable, you should get the newest version directly.  If you're running a 64-bit system, run:

    wget  http://ftp.us.debian.org/debian/pool/main/r/redis/redis-server_2.2.12-1_amd64.deb -O redis-server.deb

If you're running a 32-bit system, run:

    wget http://ftp.us.debian.org/debian/pool/main/r/redis/redis-server_2.2.12-1_i386.deb -O redis-server.deb

Then install the corresponding package

    sudo dpkg -i redis-server.deb

### RubyGems

Not needed for a RVM installation.
To install RubyGems, run the following:

    wget http://ftp.us.debian.org/debian/pool/main/r/rubygems/rubygems_1.8.15-1_all.deb -O rubygems.deb && sudo dpkg -i rubygems.deb


### Bundler

To install Bundler, run the following, skip the sudo for a RVM installation:

    sudo gem install bundler 

To get bundle to work with the system Ruby, you might need to make a symbolic link:

    sudo ln -s /var/lib/gems/1.8/bin/bundle /usr/local/bin/bundle

This is not needed on **Debian 5.0** when ruby is installed from source.


### ffi

Note: If you get an error in the next step try to run

    sudo apt-get install libffi-ruby libffi-dev

and try the step again.

### SQLite libraries and header files

    sudo apt-get install libsqlite3-dev


## Congrats! You have all your dependencies installed. Go back to [[Installing and Running Diaspora]].