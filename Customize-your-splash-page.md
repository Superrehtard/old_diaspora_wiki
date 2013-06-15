----

###301 MOVED PERMANENTLY###

We're currently **moving this wiki over to our new project site**. The contents of this page have  already been carried over, so _any new changes here will not be reflected in the new wiki_.  
New link: http://wiki.diasporafoundation.org/Custom_Splash_Page

----

All you have to do is to create a `app/views/home/_show.html.haml` to define your own splash page for your Diaspora pod.  Or `app/views/home/_show.html.erb` if you prefer to write plain HTML instead of [HAML](http://haml-lang.com/). Do not copy or overwrite `app/views/home/show.html.haml` (note the missing underscore)!

A very basic example of a `_show.html.haml` would be something like:  

```haml
#content
  .center
    %h2 Welcome to my pod!
```
A more complete example using the Twitter Bootstrap "Hero" template would look like the following:

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

Diaspora already uses [Bootstrap](http://twitter.github.com/bootstrap/) CSS so no need to add it yourself, and examples of use are readily available in the previous link.
More examples can be found [here.](https://github.com/czarneckid/twitter-bootstrap-examples-haml/tree/master/views) but will need to be tweaked similar to the one below.
(Linked JS files need to be added yourself. download [here.](http://twitter.github.com/bootstrap/))

Notice the examples above do not include the "head" & "html" elements, those are rendered by Diaspora. For your landing page you just need to provide the body section, and anything else you chose to include.

Ensure that `public/` doesn't contain any `index.html` or another file that would be used as index file by your Webserver. If such a file exists it's given precedence by your Webserver and can break the log in.