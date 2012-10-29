## Step 1: emerge the required ebuilds

     emerge nodejs mysql git libxslt ruby rubygems redis

## Step 2: install rails and bundler through ruby gems 

     gem update --system 
     gem install rails 
     gem install bundler

## Step 3: configure and start mysql database 

Configure and start mysqld. Create database needed for diaspora and the user which has full access to the database. [Gentoo MySQL Startup Guide](http://www.gentoo.org/doc/en/mysql-howto.xml) can help a lot if you have never deployed MySQL database on Gentoo.

Don't forget to run **/usr/bin/mysql_secure_installation** if this is fresh install of mysql server.

In the further steps we'll refer to this MySQL database (where Disapora meta data is stored) as **pod**  and the corresponding MySQL user as **pod**.