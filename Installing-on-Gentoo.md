## Step 1: emerge the required ebuilds

Diaspora requires threads support in ruby. Make sure that ruby ebuild is emerged with "threads" use flag. E.g., my **/etc/portage/package.use** file has the following line:

    dev-lang/ruby threads

Then emerge the needed components:

    emerge nodejs mysql git libxslt ruby rubygems redis

## Step 2: set UTF-8 locale

UTF-8 locale is needed for Diaspora build (specifically, command **bundle install --without development test heroku**) to be successful. Make sure the LANG variable is set to **en_US.UTF-8** (tested value but it looks like any UTF8 locale is OK).

Can be done by adding the follwing line into **/etc/env.d/02locale**
 
    export LANG=en_US.UTF-8

Then **env-update** and **source /etc/profile** commands should be launched. 

## Step 3: Select ruby19 profile

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

## Step 6: create system user to run Dispora

Create system user to run Diaspora:

    useradd -m diaspora

In the further steps we'll refer to this MySQL database (where Disapora meta data is stored) as **pod**  and the corresponding MySQL user as **pod**.

When mysqld is up and running, don't forget to add it to default runlevel:

    rc-update add mysql default
