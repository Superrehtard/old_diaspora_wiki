All you have to do is define a view as  "app/views/home/_show.html.haml" to define your own splash page for your diaspora pod. 

The file you need to create in the directory "app/views/home/" is written in haml syntax. See [[haml|http://haml-lang.com/]].

A very basic example of this "_show.html.haml" file would be something like:  

```haml
#content
  .center
    %h2 Welcome to my pod!
```

That should do it. If needed examine the "routes.rb" to ensure the section below exists in the file.

The file config/routes.rb lists the mapping between requests and controllers. The lines concerning the splash screen are:
```ruby
  # Startpage
  root :to => 'home#show'
```

You must delete the index.html or default.html (or whatever file your web server may use) in the app/public directory, if any, as rails gives precedence to static content before serving dynamic content.