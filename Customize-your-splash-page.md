All you have to do is define a view as  "app/views/home/_show.html.haml" to define your own splash page for your diaspora install using [[haml|http://haml-lang.com/]].

A very basic example would be something like:  

```haml
#content
  .center
    %h2 Welcome to my pod!
```

The file config/routes.rb lists the mapping between requests and controllers. The lines concerning the splash screen are:
```ruby
  # Startpage
  root :to => 'home#show'
```

You must delete the index.html in the app/public directory, if any, as rails gives precedence to static content before serving dynamic content.