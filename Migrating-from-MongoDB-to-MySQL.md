On Jan 17, we pulled our ActiveRecord/MySQL branch into master in place of MongoMapper/MongoDB.  Migrating old data from MongoDB to MySQL will take a few steps, but isn't hard.

## Set up MySQL
Use your package manager (yum/apt) to install MySQL. 

On **Fedora** and **CentOS**, you also need the mysql-devel package.

On **Ubuntu**, you also need the libmysqlclient-dev and libmysql-ruby packages.

### Tell Rails where MySQL is
Rails expects a `config/database.yml` file with information about your MySQL database.  Copy `config/database.yml.example` to `config/database.yml`, and edit it to have the user/password/database names that you want.

### Create and migrate your db
First, make sure you have specified the right character set and collation (sorting) order in your `config/database.yml`. In every block, there should be the following two lines:
    charset: utf8
    collation: utf8_bin

Add them if they are missing.

If you want that diaspora creates the database, call

    bundle exec rake db:create

This will create the database, normally you need 'root' privileges to do this. If you created a user and the database with the right collation manually, you can skip this step!

Create the tables *in* your database with:

    bundle exec rake db:migrate 

If only want to create the production database, put `RAILS_ENV=production` before each `bundle exec`. The default is to create databases in test, development and production.

## Migrate from Mongo

Make sure 'mongoexport' is in your path. It's generally located in the same place as the mongod executable.

Run 
    bundle exec rake migrations:migrate_to_mysql

As before, you will need RAILS_ENV=production at the beginning of the line if you are migrating a production database.

If you're on **Ubuntu** with AppArmor, you may get this error:
    Mysql2::Error: File '/tmp/data_conversion/csv/users.csv' not found (Errcode: 13): 
You can follow this workaround: https://bugs.launchpad.net/ubuntu/+source/mysql-dfsg-5.0/+bug/244406/comments/4

## If you get duplicate key errors
There is probably data from a few months ago in your database, when our database level key constraints weren't as specific.  You'll have to check out the last mongo ref (f17ba7b4eb3dc5a8a1de) and go into rails console.
    git checkout f17ba7b4eb3dc5a8a1de
    bundle exec rails c production
    require 'script/sanitize_database'

Note: the **production** in the middle step is only necessary if you're migrating a production database. You can omit it if you're migrating a development database.

**If you do not have a console** (the readline fails), then you need to compile it from your Ruby source.  
    cd /opt/src/ruby-1.**/ext/readline
    ruby extconf.rb
    make
    make install
After you compile it, open up the console again and do the require.

Once the require line finishes, quit rails console and re-check-out the master branch:
    git checkout master
Finally, run the **Migrate from Mongo** step again.

If you still have duplicate key errors after running the sanitize script, it probably means you created your database without the right collation setting. Try remaking it as in the **Create and migrate your db** step.
