## Run Diasporas Components directly

If you fear `./script/server` for some strange reason ;)

### A note about production mode

If you want to use production mode prepend every command here with `RAILS_ENV=production `,
the server section `config/diaspora.yml` is not respected with this way!

### Database

Be sure to prepend every command with `DB=…` if you did at installation time.

### Ensure Redis is running

If you compiled Redis from source run `redis-server` to start Redis on the default port 6379. It uses a config file, normally `/etc/redis.conf` or `/etc/redis/redis.conf` defining ports and other stuff. If you installed via a package manager there's most likely an init script you want to use.

### Run the app server

Run `bundle exec bundle exec unicorn_rails -c config/unicorn.rb -p 3000` from the root Diaspora directory.  This will start the Appserver.
It will run on port 3000 by default. If you want it to be available on port 80, either run it on port 80 directly (unwise, you shouldn't run Diaspora as root) or use the Webserver of choice to proxy port 443 and/or 80 at your domain name to Unicorn at port 3000 or over a socket.
This is absolutely needed unless you want to use a different Appserver like Passenger for example.


### Run the Sidekiq worker

To start the Sidekiq worker run the following command:

    bundle exec sidekiq

This is also essential unless you turn on single_process_mode in `config/diaspora.yml` which is absolutely not recommended for production environments. Sidekiq is used for time intensive jobs like sending mail, sending and receiving messages from other pods etc.
Add ` -d -P tmp/sidekiq.pid` to daemonize it. You can stop it with `bundle exec sidekiqctl stop tmp/sidekiq.pid`.