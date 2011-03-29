## Run Diasporas Components directly

If you fear ./script/server ;)

### A note about production mode

If you want to use production mode prepend every command here with `RAILS_ENV=production `,
config/server.sh is not respected with this way!

### Run the app server

Run `bundle exec thin start` from the root Diaspora directory.  This will start the app server.
It will run on port 3000 by default. If you want it to be available on port 80, either run it on port 80 directly (probably unwise), or use your webserver of choice (we use nginx) to proxy port 80 at your domain name to thin at port 3000 or over a socket.  See config/sprinkle/conf/nginx.conf and config/thin.yml in the repo for an example thin config and nginx server stanza.

### Run the websocket server and redis

Run `bundle exec ruby script/websocket_server.rb` to start websockets on port 8080. Change port in config/app_config.yml.

Run `redis-server` to start redis on the default port 6379. It uses a config file, normally /etc/redis.conf or /etc/redis/redis.conf defining ports and other stuff.

### Run the resque worker

To start the resque worker run the following command:

        QUEUE=* bundle exec rake resque:work

You can monitor by starting `resque-web` and then visit http://server-ip:5678
