----

###403 DO NOT MOVE###

We're currently **moving this wiki over to our new project site**. The contents of this page are not important enough to be ported over because they are either very old, very outdated or wrong and misleading. 

----

You might want to setup a development environment on an Ubuntu box,
to contribute code or repackage Diaspora for remote deployment
to a Rails host like Heroku or Engine Yard.

This does the trick on Ubuntu 12.04 and 12.10.

First install curl. Why it is not there already is a universal unknown.

$ sudo apt-get install curl


Now install RVM, Standard Ruby Gems and Rails with this beauty. This does
a lot of work so take a break. :)

$ \curl -L https://get.rvm.io | bash -s stable --rails

  * To start using RVM you need to run `source /home/<user>/.rvm/scripts/rvm`
    in all your open shell windows, in rare cases you need to reopen all shell windows.
    The easy way is to add the command to the top of your ~/.bashrc file

If you plan to deploy to Heroku install the toolbelt.
You will also need to create an account at heroku.com and create your keys with `heroku keys:add`.

$ wget -qO- https://toolbelt.heroku.com/install-ubuntu.sh | sh

  * This script requires superuser access to install apt packages.
    You will be prompted for your password by sudo, but run the command as
    your normal user, no sudo!

If you plan to run Diaspora locally, you will need a database client and access to a database such as MySQL or Postgres. Install instructions for Postgres on Ubuntu are here https://help.ubuntu.com/community/PostgreSQL.

Now you have the core system requirements to clone, bundle, run and deploy Diaspora.   

NOTES

The size of the git repo is significant to deploy times and application performance. You may wish to 
copy the source code only to a new directory and initialize a new repository before deploy. 

The ruby version that RVM installs may not be what Diaspora expects. Run `rvm install 1.9.3-p429` or whatever version Diaspora suggest. 

In the event that pg fails to build it's extensions 'Error installing pg:, ERROR: Failed to build gem native extension.', run `sudo apt-get install libpq-dev`. 
  
For deploy to Heroku change the default value for DB in Gemfile from 'mysql' to 'postgres' before you bundle. In addition set the Heroku config variable with `heroku config:set DB="postgres"` and `heroku labs:enable user-env-compile`. Also remove the line 'config/diaspora.yml' from .gitignore. 

Carry on.

Useful resources   
https://toolbelt.herokuapp.com/debian  
https://rvm.io/rvm/install/   
https://devcenter.heroku.com/articles/ruby  
https://github.com/diaspora/diaspora/wiki/Deploy-Diaspora-to-Engineyard-using-a-Windows-PC   
https://github.com/diaspora/diaspora/wiki/Installing-on-heroku   
https://github.com/diaspora/diaspora/wiki/Installing-on-Ubuntu  
https://help.ubuntu.com/10.04/serverguide/mysql.html 
