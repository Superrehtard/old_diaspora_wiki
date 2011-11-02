## Run Diasporas Components directly

If you fear ./script/server ;)

### A note about production mode

If you want to use production mode prepend every command here with `RAILS_ENV=production `,
config/script_server.yml is not respected with this way!

### Database

Be sure to prepend every command with `DB=…` if you did at installation time.

### Ensure redis is running

If you compiled redis from source run `redis-server` to start redis on the default port 6379. It uses a config file, normally /etc/redis.conf or /etc/redis/redis.conf defining ports and other stuff. If you installed via a package manager there's most likely an init script you want to use.

### Run the app server

Run `bundle exec thin start` from the root Diaspora directory.  This will start the app server.
It will run on port 3000 by default. If you want it to be available on port 80, either run it on port 80 directly (probably unwise), or use your webserver of choice (we use nginx) to proxy port 80 at your domain name to thin at port 3000 or over a socket.  See config/sprinkle/conf/nginx.conf and config/thin.yml in the repo for an example thin config and nginx server stanza.
This is absolutly needed unless you want to use a different appserver like mod_passenger for example.


### Run the websocket server

Run `bundle exec ruby script/websocket_server.rb` to start websockets on port 8080. Change port in config/application.yml.
The websocket server is not essential and only provides the [[live updates to the webfrontend|WebSockets]]. It has nothing to do with federation.

### Run the resque worker

To start the resque worker run the following command:

        QUEUE=* bundle exec rake resque:work

This is also essential unless you turn on single_process_mode in config/application.yml which is absolutly unrecommended for production environments. Resque is used for time intensive jobs like sending mail, sending and receiving messages from other pods etc.
You can monitor by starting `resque-web` and then visit http://server-ip:5678, but don't keep it running open since it's a debugging tool. By default it's also mounted in Diaspora and accessible for admins only. You can disable that by turning "mount_resque_web" to false in your application.yml.

### Run resque scheduler

In order to properly retry failed jobs, for example if the receiving end is offline, you need to run another daemon with the following command:

     bundle exec rake resque:scheduler


