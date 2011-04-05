This is a first attempt at an init script for diaspora. Additions/corrections welcomed.

I wrote a new script in bash with a helper in python. I copied the helper to the /usr/bin directory and you can call it from any where.

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

```python

#!/usr/bin/python
## /usr/bin/waitForOpen
## takes two arguments a hostname followed by a port number
import socket
from sys import *
from time import time, sleep

def main():
	sleep(1)
	sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
	print "Waiting for server to restart"
	initTime = time()
	testTime = initTime
	while True:
		currentTime = time()
		if currentTime - testTime >= 20:
			print "Still waiting. Its been %3d seconds" % (currentTime-initTime)
			testTime = time()
		try:
			connect = sock.connect((argv[1],int(argv[2])))
			print "%s port %s is now open" % (argv[1], argv[2])
			sock.close
			print "It took %3d seconds for server to start" % (currentTime-initTime)
			break
		except:
			pass

if __name__ == "__main__":
	if len(argv) < 3 or len(argv) > 3:
		print "Usage:\t%s [host] [port]" % argv[0]
		exit()
	try:
		main()
	except KeyboardInterrupt:
		print "Fine don't wait"
```