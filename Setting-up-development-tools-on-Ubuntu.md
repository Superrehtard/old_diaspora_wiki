You might want to setup a development environment on an Ubuntu box,
to contribute code or repackage diaspora for remote deployment
to a rails hosts like Heroku or Engine Yard.

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
You will also need to create an account at heroku.com and setup your ssh keys and so on.

$ wget -qO- https://toolbelt.heroku.com/install-ubuntu.sh | sh

  * This script requires superuser access to install apt packages.
    You will be prompted for your password by sudo, but run the command as
    your normal user, no sudo!

Now you have the core system requirements to clone, bundle, run and deploy Diaspora.

The size of the git repo is significant to deploy times and application performance. You may wish to 
copy the source code only to a new directory and initialize a new repository before deploy. 

Carry on.

Useful resources   
https://toolbelt.herokuapp.com/debian  
https://rvm.io/rvm/install/   
https://devcenter.heroku.com/articles/ruby  
https://github.com/diaspora/diaspora/wiki/Deploy-Diaspora-to-Engineyard-using-a-Windows-PC   
https://github.com/diaspora/diaspora/wiki/Installing-on-heroku   
https://github.com/diaspora/diaspora/wiki/Installing-on-Ubuntu   

