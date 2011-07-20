Dreamhost runs Debian GNU/Linux lenny 5.0.8. You will need to create a new domain or subdomain with Ruby on Rails enabled.

**Per Dreamhost policy you are not permitted to run persistent processes on shared hosting plans.  This has been verified with Dreamhost Support staff.  Persistent processes are only permitted on Dreamhost PS plans.**

> "Firstly, we reserve the right to kill any user process on a shared server without warning or prior notification at our discretion."
> We don't just do this capriciously though! We do this if the process is in any way adversely affecting the smooth functioning of your shared server. 

Link: [Dreamhost Wiki](http://wiki.dreamhost.com/Cron_Jobs_%26_Persistent_Processes#What_is_your_persistent_.28background.29_process_policy.3F)

- I'm going to ask them if these 3 specific processes (redis-server, resque, websocket_server) can be run (j4v4m4n)

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