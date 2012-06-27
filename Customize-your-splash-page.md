All you have to do is define a view as  "app/views/home/_show.html.haml" to define your own splash page for your diaspora pod. 

The file you need to create in the directory "app/views/home/" is written in haml syntax. See [[haml|http://haml-lang.com/]]. Or check out [Heroku html2haml converter](http://html2haml.heroku.com/) to convert any html you already know.

A very basic example of this "_show.html.haml" file would be something like:  

```haml
#content
  .center
    %h2 Welcome to my pod!
```
A more complete example using the Twitter Bootstrap "Hero" template would look like the following.
Diaspora already uses [Bootstrap](http://twitter.github.com/bootstrap/) CSS so no need to add it yourself, and examples of use are readily available in the previous link.
More examples can be found [here.](https://github.com/czarneckid/twitter-bootstrap-examples-haml/tree/master/views) but will need to be tweaked similar to the one below.
(Linked JS files need to be added yourself. download [here.](http://twitter.github.com/bootstrap/))
```haml
.navbar.navbar-fixed-top
  .navbar-inner
    .container-fluid
      %a.btn.btn-navbar{"data-target" => ".nav-collapse", "data-toggle" => "collapse"}
        %span.icon-bar
        %span.icon-bar
        %span.icon-bar
      %a.btn.btn-success.pull-left{:href => "/users/sign_in"} Login
      %a.brand{:href => "/"} Diaspora*
      .nav-collapse
        %ul.nav
          %li
            %a{:href => "/about.html"} About
          %li
            %a{:href => "/privacy.html"} Privacy
          %li
            %a{:href => "/terms.html"} Terms
      %span.st_fblike_hcount.pull-right
      %span.st_plusone_hcount.pull-right{:style => "padding-top: 3px;"} 

  .subnav
    %ul.nav.nav-pills
      %li
        %a{:href => "https://twitter.com/joindiaspora"} @joindiaspora
      %li
        %a{:href => "https://github.com/diaspora/diaspora"} GitHub
      %li
        %a{:href => "/source.tar.gz"} Code
      %li
        %a{:href => "https://github.com/diaspora/diaspora/wiki/Changelog"} What's New?
      %li
        %a{:href => "https://goo.gl/1UR7m"} Dev-Blog

   
  .container{:style => "padding-top: 5%;"}
    .hero-unit
      %img{:alt => "Logo", :src => "/assets/branding/logo_caps.png"}/
      %hr/
      %p
        %b You're about to change the Internet. Let's get you set up, shall we?
      %p
        %a.btn.btn-primary.btn-large{:href => "/users/sign_up"} Start an account! &raquo;
    .row
      .span4
        %h2 Heading
        %p Donec id elit non mi porta gravida at eget metus. Fusce dapibus, tellus ac cursus commodo, tortor mauris condimentum nibh.
        %p
          %a.btn{:href => "#"} View details &raquo;
      .span4
        %h2 Heading
        %p Donec id elit non mi porta gravida at eget metus. Fusce dapibus, tellus ac cursus commodo, tortor mauris condimentum ni.
        %p
          %a.btn{:href => "#"} View details &raquo;
      .span4
        %h2 Heading
        %p Donec sed odio dui. Cras justo odio, dapibus ac facilisis in, egestas eget quam. Vestibulum id ligula porta felis.
        %p
          %a.btn{:href => "#"} View details &raquo;

  %script{:src => "/js/jquery.js"}
  %script{:src => "/js/bootstrap.js"}
```

Notice the examples above do not include the "head" & "html" elements, those are rendered by Diaspora. For your landing page you just need to provide the body section, and anything else you chose to include.

You should also delete the "index.html" or "default.html" (or whatever default files your web server may use) in the app/public directory, if any, as rails gives precedence to static content before serving dynamic content.

That should do it. If needed examine the "routes.rb" to ensure the section below exists in the file.

The file config/routes.rb lists the mapping between requests and controllers. The lines concerning the splash screen are:
```ruby
  # Startpage
  root :to => 'home#show'
```