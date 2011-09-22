At the moment, the automated multi-server integration testing suite is in its infancy.

To run the multi-server specs, you need to set up integration_1 and integration_2 databases in your config/database.yml and run 

    rake db:integration:prepare

Then you can start the integration servers by running 

    bundle exec foreman start -f multi-server-spec-Procfile

After that, you're ready to go.  Run the specs with 

    bundle exec rspec spec/multi-server

Normal rspec spec also runs them right now, which will make your specs red if you're not running the integration servers.