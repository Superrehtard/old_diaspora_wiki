Introduction to Rails Application
====================

WIP!! This page is very rough! I hope to abbreviate it, proof-read, and add more links & details soon.. 
---------------------

Diaspora is built using the Ruby-on-Rails application platform. In this page you will find the most basic information to help you understand Rails and successfully configure and run the Diaspora Rails application.
This information should be kept unbiased to any operating system and explain only the basic Ruby-Interpreter installation, basic commands and usage, along with basic front-end & application server options.


Ruby is a programming language that can be installed from many different versions and sources. Ruby is available to install on almost any operating system, however the use of a Unix-based operating system is the only feasible way to run Diaspora currently. Ruby can be installed via packages on a Linux based system however to run Diaspora you may have problems with the version of Ruby available with the packages. Due to the unreliable nature of linux-distribution provided packages it is highly recommended that you install Ruby using RVM or directly from source. Right now the Diaspora application has been coded to run on Ruby-1.9.3 and also requires a gemset version greater than 1.8.17 to run properly. After installing Ruby and the specific Rubygems version you will also need to install the gem bundler; (gem install bundler) the bundler then installs specific Gems required by the Diaspora "Gemfile". Gems are pieces of code that add specific functions to Rails applications.


Once ruby is installed there are some commands to run from your Rails-application (Diaspora) directory. These commands need to be run from inside the application folder after downloading or cloning with Git. The First command you will need to run after downloading Diaspora is;
- "bundle install" This installs all the required Gems to run the Diaspora Rails application.
To run other commands using the bundled gems you will need to prefix the command;
- "bundle exec ..." This uses the installed "bundle" as the environment to run on.
Ruby-on-Rails has specific environments to run your application depending on usage; a Rails application can be ran as "test", "development" or "production" allowing you to have separate configurations to run under different circumstances. In order to run a working Diaspora pod you will need to run your application in "production" mode. To run a Rails application in "production" mode you just need to prefix any commands following the "bundle install" with a prefix similar to the following.
- "RAILS_ENV=production bundle exec ..." This runs the applications in a production enviroment.
Next step is setting configuration options in the YML files contained in the "/config" directory of the application, those files will contain basic applications settings such as database user&password in /config/database.yml. After setting your configuration in the YML files you will need to run a few commands to create your database and generate "assets" needed to run the application.
- "bundle exec rake db:create" Creates a database
- "bundle exec rake db:migrate" Populates database tables.


After installing Ruby, configuring and populating your database, and setting up your enviroment you are now ready to start thinking about front-end & application servers. There are many choices here and some are easier than others. For front-end servers your options are the following.
- Apache2
- Nginx
Application servers are needed to render HTML for the front-end servers to delegate to a domain name. There are qute a few options for rails-application-servers you can use with Diaspora. A few examples would be the following, however there are many others.
- Thin
- Mongrel
- Unicorn

