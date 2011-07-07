Dreamhost runs Debian GNU/Linux lenny 5.0.8. You will need to create a new domain or subdomain with Ruby on Rails enabled.

### MySQL

You need to create a new database for diaspora from dreamhost panel.

### Bundler

To install Bundler, run the following:

        GEM_PATH=$HOME/gems GEM_HOME=$HOME/gems gem install bundler 

### Redis 

To install Redis follow these steps:

        wget http://redis.googlecode.com/files/redis-2.2.11.tar.gz
        tar -zxvf redis-2.2.11.tar.gz
        cd redis-2.2.11
        cd src; make PREFIX=$HOME/redis
        make PREFIX=$HOME/redis install
        nohup $HOME/redis/bin/redis-server  &

Follow remaining steps from [[main installation article | Installing-and-Running-Diaspora]]. Just check the notes below for dreamhost specific quirks.

### Notes

To run bundle use ~/gems/bin/bundle
    
        ~/gems/bin/bundle install --path vendor/bundle_gems --without development 

sqlite3 gem won't work if you don't provide '--without development' option

there is an old version of rack installed by default which conflicts with diapsora

        gem install rack -v 1.2.3

passenger equivalent to restart is

       touch tmp/restart.txt

you have to run jammit to have the layout come properly

      ~/gems/bin/bundle exec jammit