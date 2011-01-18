On Jan 17, we pulled our ActiveRecord/MySQL branch into master in place of MongoMapper/MongoDB.  Migrating old data from MongoDB to MySQL will take a few steps, but isn't hard.

## Set up MySQL
Use your package manager (yum/apt) to install MySQL. 

On **Fedora**, you also need the msyql-devel package.

On **Ubuntu**, you also need the libmysqlclient-dev and libmysql-ruby packages.

### Tell Rails where MySQL is
Rails expects a config/database.yml file with information about your MySQL database.  Copy config/database.yml.example to config/database.yml, and edit it to have the user/password/database names that you want.

### Create and migrate your db
Run 
    bundle exec rake db:create
and 
    bundle exec rake db:migrate 
to create your database and set up the tables. *If you are migrating a production database, put RAILS_ENV=production before each command.*

## Migrate from Mongo
Make sure 'mongoexport' is in your path. It's generally located in the same place as the mongod executable.

Run 
    bundle exec rake migrations:migrate_to_mysql

As before, you will need RAILS_ENV=production at the beginning of the line if you are migrating a production database.


## If there were duplicate key errors
There is probably data from a few months ago, when our database level key constraints weren't as specific, in your db.  You'll have to check out the last mongo ref (f17ba7b4eb3dc5a8a1de) and go into rails console.
    git checkout f17ba7b4eb3dc5a8a1de
    bundle exec rails c production
    require 'script/sanitize_database'

Note: the **production** in the middle step is only necessary if you're migrating a production database. You can omit it if you're migrating a development database.

Once that finishes, quit rails console and re-check-out the master branch:
    git checkout master
And run the **Migrate from Mongo** step again.
