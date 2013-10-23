How to get things set up to copy edit the websites (diasporaproject.org and joindiaspora.com).

##diasporaproject.org

The foundation site is a simple Rails 3 application managed through GitHub. You will need a Github account, with SSH keys set up, and access to the repo.  Feel free to ask for help with Rails if needed.

An overview of how it works:

1. Clone the repo
2. Make the changes you want
3. Push it back to GitHub  
4. Then a dev can deploy the changes to the site
  
  
Once you clone the repo, you will need to:

1. Bundle: run 'bundle install' from the command line
2. Create your database: sqlite means it all works like magic, 'rake db:migrate'
3. Start the server: 'rails s'  
4. From there, all of the 'views' are in app/views/pages.  

Our markup is written in [HAML](http://haml-lang.com/); it is a little weird, but it makes lots of sense!  The big deal is that it is _whitespace sensitive_ so if you have worked with Python at all, you will know what that is like.

Also, our CSS is in [SCSS](http://sass-lang.com/), which compiles to CSS.  You can actually just write CSS, and it will be perfectly happy.  You just need to remember to edit the stylesheets in the sass folder, instead of the CSS files directly.  It will auto-regenerate every page load while you are developing. 

Once you are done making awesome changes, you just need to: 

1. git add .
2. git commit -am "awesome commit message"
3. git push origin master (pushes it back up to GitHub)

If you need more detail on these steps, GitHub should have a great tutorial.



## joindiaspora.com

For changing the sidebar on joindiaspora.com, you first need to set up your diaspora development enviroment.  This is kinda complicated, but there are [decent docs here](https://github.com/diaspora/diaspora/wiki/Installing-and-Running-Diaspora).

NOTE: You **_don't_** need nginx or apache, or the websocket server, or even redis and resque to debug front end styles.  Once you can bundle, running 'bundle exec thin start' should give you a proper app server that will recompile the templates. 

The right sidebar is located in: 
app/views/shared/_right_sections.html.haml

This is a slightly more complicated file.  The structure of the html is represented here, but all of the 'strings' are actually located in the translation file, which is found at config/locals/diaspora/en.yml.  This is just a big hash so if you see keys seperated with '.', that is just a different namespace.  ctrl-f is your friend. :)  Feel free to hop in IRC if this file gets confusing, there are people there with rails translation background.

Once you are done, just add and push back to GitHub as described above, and you should be good to go.  

For the first couple of times with joindiaspora.com you should fork the code and submit a pull request with the changes.