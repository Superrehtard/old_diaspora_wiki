This is a first attempt at an init script for diaspora. Additions/corrections welcomed.

```bash
#!/bin/bash
#
# diaspora        Startup script for the thin server for diaspora
#
# chkconfig: - 85 15
# description: The Diaspora thin server provides a server for \
#       Diaspora seeds
# processname: diaspora
#
### BEGIN INIT INFO
# Provides: diaspora
# Required-Start: $local_fs $remote_fs $network $named
# Required-Stop: $local_fs $remote_fs $network
# Short-Description: start and stop thin diaspora Server
# Description: The Diaspora server provides seeds.
### END INIT INFO

# Source function library.
. /etc/rc.d/init.d/functions

# Location of mongo database
MONGO_DB_PATH="/data/db"

# Location of mongod binary
MONGOD="/usr/local/bin/mongod"

# Location of bundle
BUNDLE="/usr/local/bin/bundle"
PATH=$PATH:$(dirname "$BUNDLE")

# Info for thin server
DIASPORA_HOME="/usr/local/diaspora"
SERVER_PORT=80
SERVER_IP="127.0.0.1"

prog=diaspora
RETVAL=0

# The semantics of these two functions differ from the way apachectl does
# things -- attempting to start while running is a failure, and shutdown
# when not running is also a failure.  So we just do it the way init scripts
# are expected to behave here.
start() {
        echo -n $"Starting mongodb: "
        $MONGOD --dbpath="$MONGO_DB_PATH" > /dev/null 2>&1 & 
        RETVAL=$?
                echo
                if [ $RETVAL != 0 ]
                then
                        return $RETVAL
                fi
                #sleep 10       # let mongodb start

                cd $DIASPORA_HOME

                echo -n $"Starting websocket server: "
                $BUNDLE exec ruby ./script/websocket_server.rb \
                        > /dev/null 2>&1 &
                RETVAL=$?
                echo
                if [ $RETVAL != 0 ]
                then
                        return $RETVAL
                fi
                #sleep 10       # let websocket server start

                echo -n $"Starting thin server: "
                $BUNDLE exec thin -p "$SERVER_PORT" -a "$SERVER_IP" start \
                        > /dev/null 2>&1 &
                RETVAL=$?
                echo
        return $RETVAL
}

stop() {
        echo -n $"Stopping $prog: "
        pkill -f "thin -p $PORT"
        pkill -f websocket_server.rb
        pkill -f "$MONGOD"
        RETVAL=$?
        echo
}

# See how we were called.
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
  *)
        echo $"Usage: $prog {start|stop|restart}"
        RETVAL=3
esac

exit $RETVAL
```