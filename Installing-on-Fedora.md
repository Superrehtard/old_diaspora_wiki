### Versions

These instructions are for the current version of Fedora.

### Build Tools

To install build tools, run the following:

        su -c 'yum install make automake gcc gcc-c++ libxslt libxslt-devel libxml2 libxml2-devel libffi libffi-devel libcurl libcurl-devel'

### Ruby

To install Ruby 1.8.7, run the following command:

        su -c "yum install ruby ruby-devel"

### MySQL

This installs MySQL, you also need the mysql-devel package:

        su -c 'yum install mysql-server mysql-devel'

To start MySQL run

        su -c 'service mysqld start'

### ImageMagick

To install ImageMagick, run the following:

        su -c 'yum install ImageMagick'

### Git

To install Git, run the following:

        su -c 'yum install git'

### Redis

To install Redis, follow the instructions at http://redis.io/download to determine the right version, and then run the following:

        wget http://redis.googlecode.com/files/redis-2.2.14.tar.gz
        tar xzf redis-2.2.14.tar.gz
        cd redis-2.2.14
        make

Note: You may need to install 'wget' first by running the following:

        su -c 'yum install wget'

### RubyGems

To install RubyGems, run the following:

        su -c 'yum install rubygems'

### Bundler

To install Bundler, run the following:

        su -c  'gem install bundler'

Congrats! You have all your dependencies installed. Go back to [[Installing and Running Diaspora]].
