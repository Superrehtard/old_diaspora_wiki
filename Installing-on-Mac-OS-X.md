### Versions

These instructions are for Mac OS X 10.6 (Snow Leopard).

### Package Management

We suggest using a package management system to download dependencies, where possible.
Trust us, it's going to make your life a lot easier.
Install [Homebrew](http://mxcl.github.com/homebrew/)

### Build Tools

To install build tools, you need to download and install
[Xcode](http://developer.apple.com/technologies/tools/xcode.html).
It's a large download; it also comes on your OS X DVD.

### Ruby

You already have Ruby 1.8.7 on your system.  Yay!

### MySQL

To install MySQL, run the following:

        brew install mysql

Install the [MySQL Preference Pane](http://creativeeyes.at/tools/mysqlpp.php) to control the MySQL daemon easily.
Alternatively, you can run

        sudo mysqladmin start

from Terminal.

### OpenSSL

You already have OpenSSL installed.

### libcurl

Everything should be okay already.

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




The following parts are common to all operating systems, so go back to [[Installing and Running Diaspora]].
