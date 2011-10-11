### Versions

These instructions are for the current version of Fedora.

### Build Tools

To install build tools, run the following:

        su -c 'yum install make automake gcc gcc-c++ libxslt libxslt-devel libxml2 libxml2-devel libffi libffi-devel libcurl libcurl-devel openssl-devel'

### Ruby

To install Ruby 1.8.7, run the following command:

        su -c "yum install ruby ruby-devel"

### MySQL

This installs MySQL, you also need the mysql-devel package:

        su -c 'yum install mysql-server mysql-devel'

To start MySQL run

        su -c 'service mysqld start'

### PostGres

This installs Postgres also if you prefer to use this over MySQL (requires some postgres setup knowledge):

        su -c 'yum install postgresql-devel'

To start Postgres see: 

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

Congrats! You have all your dependencies installed. Go back to [[Installing and Running Diaspora]].
