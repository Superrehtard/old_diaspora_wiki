##Words to the wise

There will be problems when trying to install Diaspora on Windows. Some of the gems that Diaspora requires are less supported on Windows and require extra effort to install. 

I'm also using Ruby 1.9.2 - so there may be some issues with that and the current Diaspora code.

Any comments are welcome, as well as any contributions. I've been very impressed with the Diaspora project and the development group.

##Requirement Problems

###Eventmachine

Diaspora requires [Eventmachine](http://github.com/espace/eventmachine) for the asynchronous updates needed in a social network application. Unfortunately, the latest version (0.12.11) won't install even with DevKit.

I found a workaround with a gem named [specific_install](http://github.com/rdp/specific_install) that allows you to install an "edge" gem from a git repository.

        gem install specific_install
        gem specific_install -l http://github.com/eventmachine/eventmachine.git

You may similar problems with a few of the other gems such as em-http-request.

###Gemfile

Note: You will probably have to delete all of the files you change in order to do a Git pull, I know there is a git revert function, but it didn't work for me the last time I tried it.

I couldn't get the json gem to work on windows and each time I started the thin server I would get an error box about a missing ruby 1.8.7 dll(I'm using 1.9.2). According the [JSON implementation for Ruby](http://flori.github.com/json/) there are two implementations, one pure ruby and the other with C extensions.

I removed:

		gem 'json'

And inserted:

		gem 'json_pure'

Under the uncategorized section.

There also appears to be a problem with linecache 0.43 under Ruby 1.9.2

Change:

		gem 'ruby-debug'

To:

		gem 'ruby-debug19'

I also had some trouble with em-http-request, as the extensions prevented install. Luckily there is a Windows binary gem, so install that and change the Gemfile accordingly:

I changed the following under #Eventmachine

		gem 'em-http-request',:git => 'git://github.com/igrigorik/em-http-request.git', :require => 'em-http'

To:

		gem 'em-http-request', :require => 'em-http'

###Include Path

This may have changed in the latest merges, and I'm not sure if this is a Ruby 1.9.2 or just a windows issue, but some of the include paths had to include the current directory - './app/..' instead of '/app/...'.

