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

Install the <a href="http://creativeeyes.at/tools/mysqlpp.php" target="_blank">MySQL Preference Pane</a> to control the MySQL daemon easily.
Alternatively, you can run

        sudo mysqladmin start

from Terminal to start the MySQL server.

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

Congrats! You have all your dependencies installed. Go back to [[Installing and Running Diaspora]].
