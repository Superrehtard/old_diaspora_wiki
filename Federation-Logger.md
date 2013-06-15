----

###301 MOVED PERMANENTLY###

We're currently **moving this wiki over to our new project site**. The contents of this page have  already been carried over, so _any new changes here will not be reflected in the new wiki_.  
New link: http://wiki.diasporafoundation.org/Federation_Logger

----


The Federation logger is a tool that outputs the high-level federation events (like sending a status message or photos) to be able to see, what is going on "behind the scenes". So, if you have a test environment for Diasproa set up, you can help us test and improve federation.

There is also a [screencast], if you prefer to listen instead of reading ;)

## Preparation

(All commands assume you are running them in the diaspora root directory.)

To get the federation test environment set up, you have to copy the blocks containing the configuration in `config/database.yml.example` and `config/diaspora.yml.example` to your actual config files in `config/database.yml` and `config/diaspora.yml`. Just paste the snippets at the bottom of the corresponding file.

#### config/database.yml
```yaml
integration1:
  <<: *common
  database: diaspora_integration1
integration2:
  <<: *common
  database: diaspora_integration2
```

#### config/diaspora.yml
```yaml
integration1:
  <<: *defaults
  pod_url: "http://localhost:3001/"
  serve_static_assets: true

integration2:
  <<: *defaults
  pod_url: "http://localhost:3002/"
  serve_static_assets: true
```

Next, you have to run a rake command, that will set up the two databases and prepare the tables. (This will take a while...)

    $ bundle exec rake db:integration:prepare

When that is done, you have to delete the `public/.well-known` directory (if it exists), because you will be running two servers from the same code base and this directory contains cached data that would normally contain the hostname of the server which is important for server-to-server user discovery, but in this case it has to go.

    $ rm -rI public/.well-known

## Starting

Now, we're going to start the servers. By running the following command you should see two redis servers, two workers and two application servers spinning up for the federation test environment. (Again, this will take some time and a lot of RAM...)

    $ bundle exec foreman start -f FederationProcfile

After everything has started up, you can open two browsers (yes, either two different ones, or two windows of the same, with one in the 'incognito' mode. That's because you need two different sessions...), and open them on `http://localhost:3001/` and `http://localhost:3002/` and register a new user on each of them ... you will have to remember which user is on which server. When that is done, you can search the user from one server with the user on the other server and connect them by adding them to each others aspects.

This alone should have already triggered our federation logger. You can check it out by looking at the logfile.

    $ tail -f log/federation_logger.log

## Party

That's it. You should now be able to log federation events and - if something goes wrong - provide log files from both ends for superior debugging.

Althoug we're now able to do some basic logging of events between servers, this is not by far a complete tool that does everything. You are very welcome to improve upon the setup process or the behaviour by submitting [pull requests](Pull-Request-Guidelines).

And as always, if anything goes wrong or you need help, join us on [IRC](How-we-use-IRC) or post on our [mailing list](How-to-use-the-Mailing-Lists).

[screencast]: http://devblog.joindiaspora.com/2012/03/02/screencast-federation-logger/