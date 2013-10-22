### Versions

These instructions are for Mac OS X 10.6 (Snow Leopard).

### Package Management

Install <a href="http://mxcl.github.com/homebrew/" target="_blank">Homebrew</a> and then come
back here.

### Build Tools

To install build tools, you need to download and install <a href="http://developer.apple.com/technologies/tools/xcode.html" target="_blank">Xcode</a>.
It's a large download; it also comes on your OS X DVD.

### MySQL

To install MySQL, run the following:

        brew install mysql

Set it up to run as your user:

    unset TMPDIR
    sudo mysql_install_db --verbose --user=`whoami` --basedir="$(brew --prefix mysql)" --datadir=/usr/local/var/mysql --tmpdir=/tmp

Add it to launchctl so it will start automatically:

    mkdir -p ~/Library/LaunchAgents
    cp /usr/local/Cellar/mysql/5.5.14/com.mysql.mysqld.plist ~/Library/LaunchAgents/
    launchctl load -w ~/Library/LaunchAgents/com.mysql.mysqld.plist

Now mysql is running, and you have a user named root with no password.

### ImageMagick

To install ImageMagick, run the following:

        brew install imagemagick

### Git

To install Git, run the following:

        brew install git

### Redis

To install Redis, run the following:

        brew install redis

### RubyGems

RubyGems comes preinstalled. However, you might need to update
it for use with the latest Bundler. To update RubyGems, run

        sudo gem update --system

### Bundler

To install Bundler, run the following:

        sudo gem install bundler 

## Congrats! You have all your dependencies installed. Go back to [[Notes on Installing and Running Diaspora]].
