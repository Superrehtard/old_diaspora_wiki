Dreamhost runs Debian GNU/Linux lenny 5.0.8. You will need to create a new domain or subdomain with Ruby on Rails enabled.

**Per Dreamhost policy you are not permitted to run persistent processes on shared hosting plans.  This has been verified with Dreamhost Support staff.  Persistent processes are only permitted on Dreamhost PS plans.**

> "Firstly, we reserve the right to kill any user process on a shared server without warning or prior notification at our discretion."
> We don't just do this capriciously though! We do this if the process is in any way adversely affecting the smooth functioning of your shared server. 

Link: [Dreamhost Wiki](http://wiki.dreamhost.com/Cron_Jobs_%26_Persistent_Processes#What_is_your_persistent_.28background.29_process_policy.3F)

- I'm going to ask them if these 3 specific processes (redis-server, resque, websocket_server) can be run (j4v4m4n)
- We can still do it asynchronously, whenever you want to sync, ssh into your shell and run redis-server and resque, login to your pod, do your things, kill those processes and logout from shell. Yeah, not a great way to run, but you get to keep your data.
- There is an option in config/application.yml to run in single process mode, that way we don't have to depend on resque

### MySQL

You need to create a new database for diaspora from dreamhost panel.

### Bundler

To install Bundler, run the following:

        $ gem install bundler 

### Redis 

To install Redis follow these steps:

        $ wget http://redis.googlecode.com/files/redis-2.2.11.tar.gz
        $ tar -zxvf redis-2.2.11.tar.gz
        $ cd redis-2.2.11
        $ cd src; make PREFIX=$HOME/redis
        $ make PREFIX=$HOME/redis install
        $ RAILS_ENV=production nohup $HOME/redis/bin/redis-server  &

Follow remaining steps from [[main installation article | Installing-and-Running-Diaspora]]. Just check the notes below for dreamhost specific quirks.

### Notes

Run these commands from diapsora code directory - $HOME/<yourdomain>

To run bundle use ~/.gems/bin/bundle 

Note: sqlite3 gem won't work if you don't provide '--without development' option. Also you have to remove lines containing sqlite3 from Gemfile and Gemfile.lock.

        $ ~/.gems/bin/bundle install --path vendor/bundle_gems --without development 

There is an old version of rack installed by default which conflicts with diapsora

Hack: You have to change rack version to 1.2.1 in Gemfile.lock to make it work, but then it does not work properly.

Change ca_file in config/application.yml (remember dreamhost runs debian)

Invoke all rake commands via budle exec 

        $ RAILS_ENV=production ~/.gems/bin/bundle exec rake db:migrate

Passenger equivalent to restart is 

        $ touch tmp/restart.txt

You have to run jammit to have the layout come properly

        $ RAILS_ENV=production ~/.gems/bin/bundle exec jammit

Starting resque 

        $ RAILS_ENV=production QUEUE=* nohup ~/.gems/bin/bundle exec rake resque:work &

Starting websocket_server

        $ RAILS_ENV=production nohup ~/.gems/bin/bundle exec ruby script/websocket_server.rb &

Script for starting basic services (script/dreamhost)

Note: See top of this page about restrictions about running background processes. Edit config/application.yml and under production change single_process_mode: true

        production:
          <<: *defaults
          single_process_mode: true

or if you still want to go with background processes, create script/dreamhost and make it executable (chmod +x script/dreamhost)

        RAILS_ENV=production nohup ~/.gems/bin/bundle exec  ~/redis/bin/redis-server &
        sleep 10
        RAILS_ENV=production QUEUE=* nohup ~/.gems/bin/bundle exec rake resque:work &
        sleep 10
        RAILS_ENV=production nohup ~/.gems/bin/bundle exec ruby script/websocket_server.rb &
        sleep 10

Note: Currently we get an internal server error if we have websocket_server running, so don't run it. I don't know if it has something to do with older rack version (j4v4m4n)