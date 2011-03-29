### Versions

These instructions are for the current version of Fedora.

### Build Tools

To install build tools, run the following:

        su -c 'yum install libxslt libxslt-devel libxml2 libxml2-devel'

### Ruby

To install Ruby 1.8.7, run the following command:

        su -c "yum install ruby"

### MySQL

This installs MySQL, you also need the mysql-devel package:

        su -c 'yum install mysql-server mysql-devel'

To start MySQL run

        su -c 'service mysqld start'

### OpenSSL

You already have OpenSSL installed.

### libcurl

Everything should be okay already.

### ImageMagick

To install ImageMagick, run the following:

        su -c 'yum install ImageMagick'

### Git

To install Git, run the following:

        su -c 'yum install git'

### Redis

To install Redis, run the following:

        su -c 'yum install redis'

### RubyGems

To install RubyGems, run the following:

        su -c 'yum install rubygems'

### Bundler

To install Bundler, run the following:

        su -c  'gem install bundler'




The following parts are common to all operating systems, so go back to [[Installing and Running Diaspora]].
