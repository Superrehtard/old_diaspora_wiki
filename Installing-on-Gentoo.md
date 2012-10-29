## Step 1: emerge the required ebuilds

Diaspora requires threads support in ruby. Make sure that ruby ebuild is emerged with "threads" use flag. E.g., my **/etc/portage/package.use** file has the following line:

    dev-lang/ruby threads

Imagemagick is used by Diaspora to process image uploads. Make sure that imagemagick is built with the following use flags:

    media-gfx/imagemagick autotrace bzip2 corefonts fftw fontconfig hdri jbig jpeg jpeg2k lcms lzma openexr pango png postscript raw svg tiff truetype wmf xml zlib

Then emerge the needed components:

    emerge nodejs imagemagick mysql git libxslt ruby rubygems redis

## Step 2: set UTF-8 locale

UTF-8 locale is needed for Diaspora build (specifically, command **bundle install --without development test heroku**) to be successful. Make sure the LANG variable is set to **en_US.UTF-8** (tested value but it looks like any UTF8 locale is OK).

Can be done by adding the follwing line into **/etc/env.d/02locale**
 
    export LANG=en_US.UTF-8

Then **env-update** and **source /etc/profile** commands should be launched. 

## Step 3: select ruby19 profile

Check what ruby profiles are installed at your system 

    eselect ruby list

Make sure ruby19 is used as system profile (**eselect ruby set**)
       
## Step 4: install rails and bundler through ruby gems 

    gem update --system 
    gem install rails 
    gem install bundler

## Step 5: configure and start mysql database 

Configure and start mysqld. Create database needed for diaspora and the user which has full access to the database. [Gentoo MySQL Startup Guide](http://www.gentoo.org/doc/en/mysql-howto.xml) can help a lot if you have never deployed MySQL database on Gentoo.

Don't forget to run **/usr/bin/mysql_secure_installation** if this is fresh install of mysql server. It is recommended to disable connections to mysqld from outside localhost. This is default configuration which can be validated by checking **netstat -l** output.

Make sure that database is created with utf8 as charset and utf8_bin as collation:

    CREATE DATABASE `pod` DEFAULT CHARACTER SET `utf8` COLLATE `utf8_bin`;

Later Diaspora config (config/database.yml) should point to the database name you have created.
 
When mysqld is up and running, don't forget to add it to default runlevel:

    rc-update add mysql default 

## Step 6: start Redis

Start Redis:
    
    /etc/init.d/redis start

and add it to the default runlevel:
       
    rc-update add redis default
    
It is recommended to disable connections to redis from outside localhost. This is default configuration which can be validated by checking **netstat -l** output.

## Step 7: create system user to run Dispora

Create system user to run Diaspora:

    useradd -m diaspora

## Step 8: Deploy Diaspora

Distribution-specific preparation are done. Login as diaspora user and deploy Diaspora according to generic installation instructions.

## Congrats! You have all your dependencies installed. Proceed to [[Notes on Installing and Running Diaspora]]
