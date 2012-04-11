### Versions

These instructions are for the current version of Fedora 15. If you are using a older version you may need to use things like remi repo for up to date packages.

### Build Tools

To install build tools, run the following:

        su -c 'yum install make automake gcc gcc-c++ libxslt libxslt-devel libxml2 libxml2-devel libffi libffi-devel libcurl libcurl-devel openssl-devel sqlite-devel'

### Ruby

#### RVM

You can install Ruby on a clean per user basis via [RVM](https://rvm.io/). This is currently recommended to get the latest Rubygems version.

You still need a system Ruby so run:

    sudo yum install ruby-devel

### MySQL

This installs MySQL, you also need the mysql-devel package:

        su -c 'yum install mysql-server mysql-devel'

To start MySQL run

        su -c 'service mysqld start'

### PostGres

This installs Postgres also if you prefer to use this over MySQL (requires some postgres setup knowledge):

        su -c 'yum install postgresql-server postgresql-devel'

To start Postgres see: [http://wiki.postgresql.org/wiki/YUM_Installation](http://wiki.postgresql.org/wiki/YUM_Installation)

### ImageMagick

To install ImageMagick, run the following:

        su -c 'yum install ImageMagick'

### Git

To install Git, run the following:

        su -c 'yum install git'

### Redis

To install Redis, run the following:

        su -c 'yum install redis'

Make a directory for redis logs

       su -c 'mkdir /var/log/diaspora'

### RubyGems

To install RubyGems, run the following:

        su -c 'yum install rubygems'

Rubygems tends to be a little old, you can update it by:

       su -c 'gem update --system'

### Bundler

To install Bundler, run the following:

        su -c  'gem install bundler'

#### Node.js

Follow the instructions on [Installing-Node.js-via-package-manager](https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager) for Fedora to install the Node.js package from their repository

## Congrats! You have all your dependencies installed. Go back to [[Installing and Running Diaspora]].
