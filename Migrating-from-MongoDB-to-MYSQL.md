On Jan 17, we pulled our ActiveRecord/MYSQL branch into master in place of MongoDB/MongoMapper.  Migrating old data from MongoDB to mysql will take a few steps, but isn't hard.

## Set up MYSQL
Use your package manager (yum/apt) to install mysql. 

On **Fedora**, you also need the msyql-devel package.

On **Ubuntu**, you also need the libmysqlclient-dev and libmysql-ruby packages.

### Tell Rails where MYSQL is
Rails expects a config/database.yml file with information about your mysql database.  Copy config/database.yml.example to config/database.yml, and edit it to have the user/password/database names that you want.

### Create and migrate your db
Run 
    bundle exec rake db:create
and 
    bundle exec rake db:migrate 
to create your database and set up the tables. *If you are migrating a production database, put RAILS_ENV=production before each command.*

## Migrate from mongo
Make sure 'mongoexport' is in your path. It's generally located in the same place as the mongod executable.

Run 
    bundle exec rake migrations:migrate_to_mysql

As before, you will need RAILS_ENV=production at the beginning of the line if you are migrating a production database.


## If there were duplicate key errors
There is probably data from a few months ago, when our database level key constraints weren't as specific, in your db.  You'll have to checkout the last mongo ref (f17ba7b4eb3dc5a8a1de) and go into rails console.
    git checkout f17ba7b4eb3dc5a8a1de
    bundle exec rails c production
    require 'script/sanitize_database'

Then, check out master:
    git checkout master
And run the **Migrate from mongo** step again.
