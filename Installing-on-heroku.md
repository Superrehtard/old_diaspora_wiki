Notice: This is a work in progress, it covers the author's own experience deploying to Heroku and may or may not be the "best" way (but it worked for me). I have specifically not covered getting file uploads working because I haven't gotten that far yet. Most people will use Amazon S3 or RackSpace Cloud for hosting uploaded files and I believe that is explained thoroughly enough in other installation guides and/or the application's config files. If you have any questions or suggestions or you notice something I left out, please contact me, @stevenh512 here on github or stevenh512@diasp.org on D* (or feel free to edit this page, it is a wiki afterall). I'm not part of the core team, but I did volunteer to write this up so I'll take responsibility for any errors or omissions. :)

Heroku runs your application in a virtual machine known as a "dyno" running a customized version of Ubuntu Server. A single-dyno app is free, the biggest drawbacks are the read-only filesystem and the requirement to use an external MySQL database.

### Creating your Heroku application

Install Bundler and the Heroku client (note: in Linux or Mac OSX you may need to use sudo to install gems, depending on your Ruby installation). As of version 2.23.0 of the Heroku client, the Heroku Labs functionality is built-in and the plugin is no longer needed.

```
$ gem install bundler
$ gem install heroku
```

Create your own private fork of the Diaspora code and be sure to keep a backup somewhere private, such as a flash drive, a private github reposotory or a Dropbox account. Do _not_ put it in a public repository, this will be the copy you deploy to Heroku and may contain some sensitive configuration info.

For illustration purposes, we'll use `mypod` as the name of the new Heroku app we're creating. Since `mypod` is already in use, you'll need to come up with a name before you go any further or use the random name Heroku assigns. We'll also assume you're using Heroku's shared SSL and a domain like `mypod.herokuapp.com` but it is also possible to use your own domain and SSL certificate.

Now that you've come up with a unique name for your pod, run the following commands from your private fork of the Diaspora code. Be sure to replace `mypod` with your actual pod name.

```
$ heroku create mypod --stack cedar
$ heroku config:add DB=mysql
$ heroku labs:enable user_env_compile
$ heroku labs:enable sigterm-all
$ heroku addons:add ssl:piggyback
$ heroku addons:add redistogo:nano
```

Two Heroku Labs experimental features are used here. The `user_env_compile` feature makes your Heroku config variables available during slug compilation, which (in Rails 3.1+) makes your database available during asset precompilation. The `sigterm-all` sends SIGTERM to all processes in a dyno rather than just the master process, this makes it easier for your Resque background workers to clean up after themselves.

### MySQL

(Update: This may be outdated, recent changes to Diaspora have fixed some of the issues with PostgreSQL. Your mileage may vary.)

You will need a remotely hosted MySQL server. It is recommended to use a MySQL server that is hosted on Amazon EC2, since that's where your Heroku app lives. Amazon RDS is a good option, and is in use by joindiaspora.com and possibly other pods. Heroku also makes several third-party MySQL databases available as add-ons, including ClearDB and Xeround. You can also deploy a MySQL database on [DotCloud](http://www.dotcloud.com) and use it with your Heroku app, but this is a slightly more advanced option. Whichever you choose, be sure to write down the full URL to your database, you'll need it later, it should be in the form of `mysql2://user:password@domain.com/dbname`.

Important: Make sure to use `mysql2://` and _not_ `mysql://` or Heroku will not initialize your app's database settings properly.

### Configuring your app

The first thing you're going to want to do is edit the `.gitignore` file in your private fork. Remove or comment out (by placing a `#` in front of them) the following lines:

```
app/views/home/_show.html.haml
app/views/home/_show.mobile.haml
public/images/ball_small.png
public/images/ball.png
config/app_config.yml
config/app.yml
config/application.yml
config/heroku.yml
config/script_server*.yml
config/fb_config.yml
config/oauth_keys.yml
config/initializers/secret_token.rb
config/redis.conf
config/deploy_config.yml
config/database.yml
public/stylesheets/*.css
public/diaspora
```

That's a lot to remove from your `.gitignore` but, unlike more traditional hosting, these files need to exist in your git repository when you deploy to Heroku.

Now you're ready to create the default configuration files, so you'll need to execute these commands:

```
$ cp config/application.yml.example config/application.yml
$ cp config/database.yml.example config/database.yml
$ cp config/heroku.yml.example config/heroku.yml
$ cp config/oauth_keys.yml.example config/oauth_keys.yml
$ cp config/script_server.yml.example config/script_server.yml
$ bundle install
```

The last two yml files might not be necessary, but I copied them over anyway. The `heroku.yml` is only necessary if you want to use the built-in Rake tasks for deployment, if so you'll want to configure it accordingly (I didn't use it, just copied over the default file).

Now edit your config/application.yml to fit your needs. At the very least, you'll need to set pod_url to match your pod's domain name and make sure your ca_file line looks like this:

```
  ca_file: '/usr/lib/ssl/certs/ca-certificates.crt' # Heroku
```

You'll also want to make sure, at least for now, that your `registrations_closed` line is set to `false`. Don't worry, you can change this later if you want a private pod, but for now you need a way to sign up.

Next, you'll want to edit your `Procfile` to look like this:

```
web: bundle exec unicorn -c config/unicorn.rb -p $PORT
catchall_worker: env QUEUE=* bundle exec rake resque:work
slow_worker: env QUEUES=socket_webfinger,photos,http_service,dispatch,receive_local,mail,receive,receive_salmon,http,delete_account bundle exec rake resque:work
priority_worker: env QUEUES=socket_webfinger,photos,http_service,dispatch,mail,delete_account bundle exec rake resque:work
```

Note that we're not running a Redis server here, that's provided by the RedisToGo add-on. I mentioned in the beginning that this could be run as a single dyno app, so unless you choose to scale up your Resque workers only the `web:` process will need to be running.

Finally, since you will need at least one Resque worker, let's edit `config/unicorn.rb` to look like this (yes, we're running the worker as a background process in the `web:` dyno):

```
rails_env = ENV['RAILS_ENV'] || 'development'

# Enable and set these to run the worker as a different user/group
#user  = 'diaspora'
#group = 'diaspora'

worker_processes 1

## Load the app before spawning workers
preload_app true

# How long to wait before killing an unresponsive worker
timeout 30

@resque_pid = nil

#pid '/var/run/diaspora/diaspora.pid'
#listen '/var/run/diaspora/diaspora.sock', :backlog => 2048

# Ruby Enterprise Feature
if GC.respond_to?(:copy_on_write_friendly=)
  GC.copy_on_write_friendly = true
end


before_fork do |server, worker|
  # If using preload_app, enable this line
  ActiveRecord::Base.connection.disconnect!

  if !AppConfig.single_process_mode?
    # Clean up Resque workers killed by previous deploys/restarts
    Resque.workers.each { |w| w.unregister_worker }
    @resque_pid ||= spawn('bundle exec rake resque:work QUEUES=*')
  end

  # disconnect redis if in use
  if !AppConfig.single_process_mode?
    Resque.redis.client.disconnect
  end

  old_pid = '/var/run/diaspora/diaspora.pid.oldbin'
  if File.exists?(old_pid) && server.pid != old_pid
    begin
      Process.kill("QUIT", File.read(old_pid).to_i)
    rescue Errno::ENOENT, Errno::ESRCH
      # someone else did our job for us
    end
  end
end


after_fork do |server, worker|
  # If using preload_app, enable this line
  ActiveRecord::Base.establish_connection

  # copy pasta from resque.rb because i'm a bad person
  if !AppConfig.single_process_mode?
    if redis_to_go = ENV["REDISTOGO_URL"]
      uri = URI.parse(redis_to_go)
      Resque.redis = Redis.new(:host => uri.host, :port => uri.port, :password => uri.password)
    elsif AppConfig[:redis_url]
      Resque.redis = Redis.new(:host => AppConfig[:redis_url], :port => 6379)
    end
  end

  # Enable this line to have the workers run as different user/group
  #worker.user(user, group)
end
```

The important lines here are the ones that start with `@resque_pid`, this causes Unicorn to spawn your Resque worker process in the background before it forks and starts the server.

Now you're ready to push your code to Heroku, execute the following commands:

```
$ git add .
$ git commit -am 'Initial deployment'
$ git push heroku master
```

### MySQL Revisited

If you run `heroku config` you'll see that Heroku has given you a shared PostgreSQL database. You _don't_ want to use this. Take the database URL you wrote down earlier, in the form of `mysql2://user:password@domain.com/dbname` and execute the following command: 

```
heroku config:add DATABASE_URL=mysql2://user:password@domain.com/dbname
```

Note that if you used one of the MySQL databases available as a Heroku add-on, this step may or may not have already been done for you, depending on the add-on. You should be able to verify this by running `heroku config`.

### Database Migrations and signing up

This is the easy part. Assuming you followed all the steps above (and assuming I haven't left anything out.. if I have, please contact me), you should be able to run the following commands:

```
$ heroku run rake db:migrate
$ heroku open
```

Now you should see your brand new pod in your browser. Go ahead and sign up, be sure to note the username you choose, you'll need it in the next step.

### Administrator, Private pod, and customizations

Edit your `config/application.yml` and make the `admins` section look like this, replacing `yourusername` with your actual username:

```
  admins:
    - 'yourusername'
```

In the same file, set the `admin_account` line to look like this:

```
  admin_account: 'yourusername'
```

While you're at it, if you want a private pod, be sure to set `registrations_closed` to `true`.

If you'd like to customize the splash page (the page that shows up when you visit your pod, before signing in), create a file called `app/views/home/_show.html.haml` and edit it however you'd like. This is a HAML file, refer to the other views or the [official HAML website](http://haml-lang.com) for more information on creating or editing this file.

### Push your changes

You're almost done, commit your changes and push them to Heroku:

```
$ git add .
$ git commit -am 'Admin user and customizations'
$ git push heroku master
$ heroku open
```

That's it. Assuming you or I didn't make any mistakes here, you should have a fully functioning pod. Please note that while federation works with this configuration, it may take a couple days to start seeing posts from other pods (and, currently, you will only see posts from pods where at least one user is sharing with you).