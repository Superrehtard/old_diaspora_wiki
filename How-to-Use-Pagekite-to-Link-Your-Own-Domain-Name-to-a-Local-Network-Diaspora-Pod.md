----

###301 MOVED PERMANENTLY###

We're currently **moving this wiki over to our new project site**. The contents of this page have  already been carried over, so _any new changes here will not be reflected in the new wiki_.  
New link: http://wiki.diasporafoundation.org/Pagekite

----
# How to Use Pagekite to Link a Domain Name to a Diaspora Pod on a Local Network Computer

Diaspora is an open-source, decentralized social network. A Diaspora server is called a pod. Each user account within a pod is called a seed. The various pods communicate with one another, collectively creating the Diaspora network. There are a variety of pod sizes: some have thousands of seeds, some only a single seed. This how-to covers setting up your own pod on a linux box (Fedora 16, specifically), and making it visible on the internet (not just your local network).

## 1. Install the Diaspora Software Ensemble on Your Computer

You can find instructions for installing Diaspora on your computer [here](https://github.com/diaspora/diaspora/wiki/Installing-and-Running-Diaspora/). Be sure to install it within a directory tree to which nginx will have access (e.g., /usr/share/nginx). Once you've correctly installed Diaspora and started it up, you should have a thin app server listening at localhost:3000. If you put that address in your web browser's navigation bar, you should see a log-in screen.  (Don't create an account yet, or it'll have an improper address, e.g., you@localhost:3000.)

## 2. Choose Your Pod's Name

What will you call your pod? This how-to assumes you're going to call it diaspora.[yourdomain].net. Later, we'll show you how to actually create the domain [yourdomain].net (of which diaspora.[yourdomain].net will be a subdomain). But for now, just choose a name that will be the internet address for your pod.

## 3. Install the nginx Web Server Software on Your Computer

Although you can run Diaspora without a web server (see Section 8 below), this isn't recommended. You won't have SSL, so other pods that require SSL connections won't be able to connect with your pod.

The preferred web server is nginx. You can get it with yum, rpm or apt, but try to download the latest version, because this how-to is based on it. Once you've installed nginx, you should be able to start it right away by typing, as root or sudo, in a teminal:

`service nginx start`

This should return "[ok]". And if you type localhost:80 in your web browser's navigation bar, you should get an nginx html page.

Now you have to edit the nginx configuration files for your particular setup. Once you've installed nginx, find its configuration files by typing, in a terminal, as root or sudo:

`whereis nginx`

On Fedora 16, you'll find the nginx configuration files at /etc/nginx and /etc/nginx/conf.d. 

As a guide, here's my working, edited /etc/nginx/nginx.conf file:

     user       nginx;
     worker_processes  1;
     error_log  /var/log/nginx/error.log;
     pid        /var/run/nginx.pid;
 
     events {
       worker_connections  1024;
     }
     
     http {

       include    /etc/nginx/mime.types;
 
       default_type application/octet-stream;
       log_format   main '$remote_addr - $remote_user [$time_local]  $status '
         '"$request" $body_bytes_sent "$http_referer" '
         '"$http_user_agent" "$http_x_forwarded_for"';
       access_log   /var/log/nginx/access.log  main;
       sendfile     on;
       keepalive_timeout  65;
       gzip  on;
       gzip_http_version 1.0;
       gzip_comp_level   2;
       gzip_proxied      any;
       gzip_buffers      16 8k;
       gzip_types        text/plain text/css application/x-javascript text/xml application
           /xml+rss text/javascript;
       gzip_disable      "MSIE [1-6]\.(?!.*SV1)";

    server { 
     listen       80;
     server_name  diaspora.[yourdomainname].net www.diaspora.[yourdomainname].net;
     access_log   /var/log/nginx/access80.log  main;

     location / {
     rewrite      ^(.*) https://diaspora.[yourdomainname].net$1 permanent;
     }

     location /uploads/images {
     expires 1d;
     add_header Cache-Control public;
     }

     location /assets {
     expires 1d;
     add_header Cache-Control public;
     }
    } 

    upstream thin_server {
      server 127.0.0.1:3000;
    }

    server {
    listen       443;
    server_name  diaspora.[yourdomainname].net  www.diaspora.[yourdomainname].net;
    access_log   /var/log/nginx/access443.log  main;
    root /[path to your diaspora public directory]/public;

    ssl on;
    ssl_certificate         /[path to your ssl cert]/ssl.crt;
    ssl_certificate_key     /[path to your ssl key]/ssl.key;
    ssl_session_cache       shared:SSL:10m;
    ssl_session_timeout     5m;
    ssl_protocols           TLSv1;
    ssl_ciphers             ECDHE-RSA-AES256-SHA384:AES256-SHA256:RC4:HIGH:!MD5:!aNULL:!EDH:!AESGCM;
    ssl_prefer_server_ciphers on;
    add_header              Strict-Transport-Security max-age=500;
    ssl_ecdh_curve          secp521r1;

    location / {
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $http_host;
    proxy_set_header X-Forwarded-Proto https;
    proxy_redirect off;
    client_max_body_size 4M;
    client_body_buffer_size 128K;
    proxy_pass http://thin_server;
    }

    location ~ ^/(images|javascripts|assets|stylesheets|uploads)/ {
    root    /[path to your diaspora public directory]/public;
    expires 1d;
    add_header Cache-Control public;
    }

    location /uploads/images/ {
    expires 1d;
    add_header Cache-Control public;
    }

    location /assets {
    expires 1d;
    add_header Cache-Control public;
    }

    } 
    } 


Once you've finished editing /etc/nginx/nginx.conf, restart the server by typing, in a terminal, as root or sudo:

`service nginx restart`

At this point, you should have the Diaspora thin app server listening on localhost:3000 and the nginx web server listening on localhost:80, which upgrades to localhost:443, which proxies to the app server at localhost:3000. 

But the nginx webserver is not visible to the internet yet, because it is likely hidden behind NAT IP addresses assigned by your ISP and your local area network. That is to say, your nginx webserver's IP address likely is something like 192.168.2.6:443, which is not an address reachable from other computers on the internet. [Pagekite](http://pagekite.net) is a simple way to make the nginx webserver visible to the internet. Here's how (substitute your particulars for the examples in the brackets below):

## 4. Create a Domain Name

Go to verisign.com, register.com, or the like, and register a domain matching what you chose in Step 2 above. For example, [yourdomain].net.

## 5. Create a Pagekite Account

Open an account at [Pagekite](http://pagekite.net), and create a pagekite, for example, [yourname].pagekite.me. Download the pagekite software and install it. Make note of where the configuration files are located on your computer afterward. For example, on Fedora 16 linux, the configuration files may be found at /etc/pagekite.d/.

## 6. Create a CNAME Record

Go back to your domain registrar, log in to your domain name account, and create a CNAME record that points to your pagekite. For example, create the CNAME diaspora.[yourdomain].net, and point it to [yourname].pagekite.me. Your domain registrar will have instructions on how to do this. The process will vary by domain registrar.

## 7. Get an SSL Cert and SSL Key for Your Subdomain.

For SSL to work for nginx at port 443, you need an SSL cert. You can get one free at [StartSSL.com](http://www.startssl.com/). Many domain registrars can do it for you too. Make sure it matches the CNAME you've created, e.g., diaspora.[yourdomain].net. Once you've installed your SSL certificate and key, and any intermediate CA certs you need, make sure that the SSL location info lines in /etc/nginx/nginx.confoint to the location of the cert and key.

## 8. Edit the Pagekite Configuration Files

Now you want to direct the CNAME you created, diaspora.[yourdomain].net, through your [yourname].pagekite.me kite, to your local computer, where your nginx webserver is listening at ports 80 and 443. Using a text editor, edit your pagekite configuration files as follows.  The file /etc/pagekite.d/10_account.rc should contain the following values:

     kitename=diaspora.[yourdomain].net

     kitesecret=[your account secret from your pagekite account]


The file /etc/pagekite.d/80_httpd.rc should contain the following:

     backend=http:[yourname].pagekite.me:localhost:80:@kitesecret

     backend=https:[yourname].pagekite.me:localhost:443:@kitesecret

     backend=http:@kitename:localhost:80:@kitesecret

     backend=https:@kitename:localhost:443:@kitesecret


Note: If you're having problems with nginx. While you figure the problem out, you can direct pagekite directly to the thin server listening at localhost:3000 with the following 80_httpd.rc config:

     backend=http:[yourname].pagekite.me:localhost:3000:@kitesecret

     backend=http:@kitename:localhost:3000:@kitesecret

## 9. Test

Provided Diaspora and nginx are running, you can now start pagekite, by typing, as root or sudo, in a terminal:

`service pagekite start`

After a minute or so, you should be able to find your Diaspora login screen in a browser window at https://diaspora.[yourdomain].net. Now you can create an account, or log in if you already have one.

## 10. Relocating Your Pod

If you change local network locations (for example, you take the laptop hosting your pod to an internet cafe), Pagekite will update your DNS settings automatically.

OK? Good luck. Hope to see you on Diaspora. (Oh, one last thing: operating a web server may violate your internet service provider's service terms and conditions. If you're concerned about that, review your service agreement.)