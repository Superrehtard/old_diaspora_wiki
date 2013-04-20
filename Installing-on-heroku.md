We start with building a minimal pod entirely hosted on Heroku.
It's good to have an RVM installation locally to leave your local system clean(er).
 
Fork Diaspora and clone your fork to your local machine.
There checkout a custom branch for your pod, we'll name it  `heroku` here.
 
    cd diaspora
    git checkout -b heroku origin/master 

To update your `Gemfile.lock` run

    DB=postgres bundle --without development test heroku production assets
    git add Gemfile.lock
    git commit -m "switch Gemfile.lock to pg exclusivly"
[Tester comment, Oct 12 2012 from a Windows user: Heroku writes Gemfile dynamically at compile time and uses a deploy mode that insists Gemfile.lock be in version control. If you deploy from a system that is not outfitted with ruby development tools, this can cause a pickle with conditional gems. To change Gemfile on Heroku it is necessary to run bundle locally and create a matching Gemfile.lock. Bundle, however, is not well supported at this time on native Windows systems. So, to deploy your Rails apps to Heroku more easily, either run a virtual Linux box under Windows or find an old PC and load Linux on it. ]

[OSX 10.8.3, Apr 8 2013 - Install XCode CLI [Command Line Tools](http://stackoverflow.com/questions/11703030/why-wont-bundler-install-the-json-1-7-4-gem-on-os-x-10-8) (go to XCode Preferences > Downloads) - this should solve the issue in "Installing json (1.7.7)"]

Install the  [Heroku toolbelt](https://toolbelt.heroku.com/).

Create an app, you must exchange `diasporadev` here and in all following cases with something unique:

    heroku apps:create diasporadev

Enable required the addons:

    heroku labs:enable user-env-compile
    heroku addons:add redistogo:nano
    heroku addons:add heroku-postgresql

And make the database available, replace the `REPLACEME` with corresponding part of `HEROKU_POSTGRESQL_REPLACEME_URL` that the last command gave you (or look it up with `heroku config`):

    heroku pg:promote HEROKU_POSTGRESQL_REPLACEME

Set some basic configuration, remember to replace `diasporadev`:

    heroku config:add HEROKU=true  DB=postgres ENVIRONMENT_URL=https://diasporadev.herokuapp.com/ ENVIRONMENT_ASSETS_SERVE=true SERVER_EMBED_RESQUE_WORKER=true  ENVIRONMENT_CERTIFICATE_AUTHORITIES=/usr/lib/ssl/certs/ca-certificates.crt

You can now already go through `config/diaspora.yml.example` and add the settings you want to differ from the defaults (`config/defaults.yml`), converting them to environment variables as described at the top of the file.

Set a secure token:

    heroku config:add SECRET_TOKEN=$(curl -s "http://www.random.org/cgi-bin/randbyte?nbytes=40&format=h" | tr -d " \n")

Now lets deploy:

    git push heroku heroku:master

And load the schema:

    heroku run rake db:schema:load


Restart to ensure you're not in the crashed state from the deploy with a unpopulated database:

    heroku restart

That should give you a basic pod, to open it in your browser run

    heroku open

# Custom landing page

Edit `.gitignore` and remove the following line:

    app/views/home/_show.*

Create your landing page as described in [[Customize your splash page]]. Add it to your branch:

    git add app/views/home/
    git commit -m "custom landing page"

Deploy it:

    git push heroku heroku:master

# S3 Image and asset hosting

To enable image hosting, first set your S3 data:

    heroku config:add ENVIRONMENT_S3_ENABLE=true ENVIRONMENT_S3_KEY=changeme ENVIRONMENT_S3_SECRET=changeme ENVIRONMENT_S3_BUCKET=changeme ENVIRONMENT_IMAGE_REDIRECT_URL=https://changeme.s3.amazonaws.com/

Change your region if it's different from the default:

    heroku config:add ENVIRONMENT_S3_REGION=eu-west-1

To enable uploading assets to S3 and serving them from it:

    heroku config:add ENVIRONMENT_ASSETS_UPLOAD=true ENVIRONMENT_ASSETS_HOST=https://changeme.s3.amazonaws.com

Run the following to upload your assets. Currently the integration into the rake task isn't working so you have to run this after each deploy:

    heroku run 'rails runner "AssetSync.sync"'

# RDS Database hosting

    heroku config:set DB=mysql

Grab commit SHA:

    git log --grep "switch Gemfile.lock to pg exclusivly" 
    git revert <commit-sha from previous command>

Follow <https://devcenter.heroku.com/articles/amazon_rds>

Deploy.


# Migrate existing installation

This isn't fool proof, think about what the commands do!

Assuming you have committed to master and these commits are in your fork:

    git branch -m master oldheroku
    git remote add upstream git://github.com/diaspora/diaspora.git # if not done already
    git fetch upstream
    git checkout -b master upstream/master
    git push -f origin master oldheroku
    git checkout -b heroku origin/master

Heroku CLI is no longer in the Gemfile, ensure it's installed:

    gem install heroku

Replace diasporadev with your appname everywhere.

    heroku config:set HEROKU=true  ENVIRONMENT_URL=https://diasporadev.herokuapp.com/ ENVIRONMENT_ASSETS_SERVE=true ENVIRONMENT_UNICORN_EMBED_RESQUE_WORKER=true  ENVIRONMENT_CERTIFICATE_AUTHORITIES=/usr/lib/ssl/certs/ca-certificates.crt

Run `heroku config` to double check that `SECRET_TOKEN` is set, if not set one like above. Also check if `DB` is set to the database you use ("postgres" or "mysql"). `NO_SSL` is now `ENVIRONMENT_REQUIRE_SSL` and to disable SSL you have set it to `false`.

Go through `config/diaspora.yml.example` and add the settings you want to differ from the defaults (`config/defaults.yml`), converting them to environment variables as described at the top of the file.

If you want your custom splash page back:

    git checkout oldheroku
    cp app/views/home/_show.html.haml _show.html.haml
    git checkout heroku
    nano .gitignore # remove app/views/home/_show.*
    mv _show.html.haml app/views/home/
    git add app/views/home/_show.html.haml
    git commit -m "custom landing page"

You can migrate modifications like so:

    git log oldheroku # Grab the commit SHAs you want to port over
    git cherry-pick <commit-sha> # For each commit

Ensure the new S3 variables are set correctly as in the "S3 Image and asset hosting" section above, if you want to use it.

Deploy initially with the following:

    git push -fu heroku heroku:master

Simple `git push`s should be enough for future deploys.