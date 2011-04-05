This is a first attempt at an init script for diaspora. Additions/corrections welcomed.

The pkg directory contains corresponding scripts  for Fedora and Ubuntu which handles all services (thin, redis, websocket and rake resque:work ATM)

```bash
#!/bin/bash
#/etc/init.d/diaspora

DIASPORA_HOME="/var/www/diaspora"
REDIS_CONFIG="/var/www/diaspora/config/redis.conf"
RETVAL=0

start() {
	cd $DIASPORA_HOME
	echo starting server
	redis-server $REDIS_CONFIG > /dev/null &
	nohup ./script/server > /dev/null &
	waitForOpen localhost 3000
}

stop() {
	pkill -f "ruby ./script/websocket_server.rb"
	pkill -f "ruby /usr/local/lib/ruby/gems"
	pkill -f "redis-server"
	echo
}

update() {
	stop
	cd $DIASPORA_HOME
	echo pulling from github
    git pull origin master
	echo installing gems
	bundle install
	echo migrating db
	bundle exec rake db:migrate
	echo running jammit
	bundle exec jammit
	echo restarting server
	start
}

case "$1" in
   start)
      start
      ;;
   stop)
      stop
      ;;
   restart)
      stop
      start
      ;;
   update)
      update
   *)
      echo $"Usage: diaspora {start|stop|restart|update}"
      RETVAL=3
esac

exit $RETVAL
```