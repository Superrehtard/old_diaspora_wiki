Dreamhost runs Debian GNU/Linux lenny 5.0.8

### MySQL

You need to create a new database for diaspora from dreamhost panel.

### Bundler

To install Bundler, run the following:

        GEM_PATH=$HOME/gems GEM_HOME=$HOME/gems gem install bundler 

Follow remaining steps from [[main installation article | Installing-and-Running-Diaspora]]. Just check the notes below for dreamhost specific quirks.

### Notes

To run bundle use ~/gems/bin/bundle
    
        ~/gems/bin/bundle install --path vendor/bundle_gems --without development 

sqlite3 gem won't work if you don't provide '--without development' option

there is an old version of rack installed by default which conflicts with diapsora

        gem install rack -v 1.2.3

passenger equivalent to restart is

       touch tmp/restart.txt