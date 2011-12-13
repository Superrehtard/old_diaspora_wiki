# How to Use Pagekite to Link a Domain Name to a Diaspora Pod on a Local Network Computer

Diaspora is an open-source, decentralized social network. A Diaspora server is called a pod. Each user account within a pod is called a seed. The various pods communicate with one another, collectively creating the Diaspora network. There are a variety of pod sizes: some have thousands of seeds, some only a single seed. This how-to covers setting up your own pod on a linux box (Fedora 16, specifically), and making it visible on the internet (not just your local network).

## 1. Install the Diaspora Software Ensemble on Your Computer

You can find instructions for installing Diaspora on your computer [here](https://github.com/diaspora/diaspora/wiki/Installing-and-Running-Diaspora/). Once you've correctly installed Diaspora and started it up, you should have a thin app server listening at localhost:3000. If you put that address in your web browser's navigation bar, you should see a log-in screen.  (Don't create an account yet, or it'll have an improper address, e.g., you@localhost:3000.)

## 2. Choose Your Pod's Name

What will you call your pod? This how-to assumes you're going to call it diaspora.[yourdomain].net. Later, we'll show you how to actually create the domain [yourdomain].net (of which diaspora.[yourdomain].net will be a subdomain). But for now, just choose a name that will be the internet address for your pod.

## 3. Install the nginx Web Server Software on Your Computer

Although you can run Diaspora without a web server (see Section 8 below), this isn't recommended. You won't have SSL, so other pods that require SSL connections won't be able to connect with your pod.

The preferred web server is nginx. You can get it with yum, rpm or apt, but try to download the latest version, which is 1.0.10-1, because this how-to is based on it. Once you've installed nginx, you should be able to start it right away by typing, as root or sudo, in a teminal:

`service nginx start`

This should return "[ok]". And if you type localhost:80 in your web browser's navigation bar, you should get an nginx html page.

Now you have to edit the nginx configuration files for your particular setup. Once you've installed nginx, find its configuration files by typing, in a terminal, as root or sudo:

`whereis nginx`

On Fedora 16, you'll find the nginx configuration files at /etc/nginx and /etc/nginx/conf.d. 

Let's start by editing the default configuration file, /etc/nginx/nginx.conf so that it understands that the thin app server is listening at localhost:3000. Use your text editor of choice. As a guide, here's my working, edited /etc/nginx/nginx.conf file (comment lines removed):


    user             nginx;

    worker_processes  3;

    error_log  /var/log/nginx/error.log;

    pid        /var/run/nginx.pid;

    events {
    worker_connections  1024;
    }

    http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;

    keepalive_timeout  65;

    gzip  on;

    upstream thin_server {
    server 127.0.0.1:3000;
    }
    
    upstream resque_web {
    server 127.0.0.1:5678;
    }

    include /etc/nginx/conf.d/*.conf;
    }


The only thing changed here is to add, after the line "gzip on;", information to identify the upstream thin and resque ports.

The next step is to configure how nginx handles calls to port 80 (the standard http port) and 443 (the standard https port). So, in a terminal, as root:

`cd /etc/nginx/conf.d`

You'll see three files there: default.conf, ssl.conf, and virtual.conf. We'll be editing the first two. Let's start with default.conf. It contains the configuration to handle port 80. What we want to do is tell it to redirect port 80 requests to port 443, and we want to tell it where the diaspora pod's public files are located on the computer. Here's my working /etc/nginx/conf.d/default.conf file (comment lines removed)(except I've replaced my pod's name with "diaspora.[yourdomain].net," and my username with "[your username]" as a placeholder for your particular setup's information):


    server {
    listen       80;

    server_name  diaspora.[yourdomain].net www.diaspora.[yourdomain].net;

    rewrite ^(.*) https://diaspora.[yourdomain].net$1 permanent;

    location / {
        root   /home/[your username]/diaspora/public;
        index  index.html index.htm;
    }

    error_page  404              /404.html;
    location = /404.html {
       root   /home/[your username]/diaspora/public;
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /home/[your username]/diaspora/public;
    }
    }


OK, the next step is to edit the /etc/nginx/conf.d/ssl.conf file, to tell it the server name, tell it the location of your SSL files, tell it to check the upstream server at localhost:3000, and to tell it where the diaspora public files are. Here's my working edited version (comment lines removed) (except I've replaced my pod's name with "diaspora.[yourdomain].net," and my username with "[your username]" as a placeholder for your particular setup's information):

     server {
     listen       443;
     server_name  diaspora.[yourdomain].net www.diaspora.[yourdomain].net;

     ssl                  on;
     ssl_certificate      /home/[your username]/diaspora/public/SSL.crt;
     ssl_certificate_key  /home/[your username]/diaspora/public/SSL.key;

     ssl_session_timeout  5m;

     ssl_protocols  SSLv2 SSLv3 TLSv1;
     ssl_ciphers  ALL:!ADH:!EXPORT56:RC4+RSA:+HIGH:+MEDIUM:+LOW:+SSLv2:+EXP;
     ssl_prefer_server_ciphers   on;

     location / {

    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $http_host;
    proxy_redirect off;
    client_max_body_size 4M;
    client_body_buffer_size 128K;
    if (-f $request_filename/index.html) {
      rewrite (.*) $1/index.html break;
    }
    if (-f $request_filename.html) {
     rewrite (.*) $1.html break;
    }
    if (!-f $request_filename) {
     proxy_pass http://thin_server;
     break;
    }
          root   /home/[your username]/diaspora/public;
          index  index.html index.htm;
     }
     }


Once you've finished editing /etc/nginx/nginx.conf, /etc/nginx/conf.d/default.conf, and /etc/nginx/conf.d/ssl.conf, restart the server by typing, in a terminal, as root or sudo:

`service nginx restart`

At this point, you should have the Diaspora thin app server listening on localhost:3000 and the nginx web server listening on localhost:80, which upgrades to localhost:443, which proxies to the app server at localhost:3000. 

But the nginx webserver is not visible to the internet yet, because it is likely hidden behind NAT IP addresses assigned by your ISP and your local area network. That is to say, your nginx webserver's IP address likely is something like 192.168.2.6:443, which is not an address reachable from other computers on the internet. [Pagekite](http://pagekite.net) is a simple way to make the nginx webserver visible to the internet. Here's how (substitute your particulars for the examples in the brackets below):

## 4. Create a Domain Name

Go to GoDaddy, VeriSign, or the like, and register a domain matching what you chose in Step 2 above. For example, [yourdomain].net.

## 5. Create a Pagekite Account

Open an account at [Pagekite](http://pagekite.net), and create a pagekite, for example, [yourname].pagekite.me. Download the pagekite software and install it. Make note of where the configuration files are located on your computer afterward. For example, on Fedora 16 linux, the configuration files may be found at /etc/pagekite.d/.

## 6. Create a CNAME Record

Go back to your domain registrar, log in to your domain name account, and create a CNAME record that points to your pagekite. For example, create the CNAME diaspora.[yourdomain].net, and point it to [yourname].pagekite.me. Your domain registrar will have instructions on how to do this. The process will vary by domain registrar.

## 7. Get an SSL Cert and SSL Key for Your Subdomain.

For SSL to work for nginx at port 443, you need an SSL cert. You can get one free at [StartSSL.com](http://www.startssl.com/). Many domain registrars can do it for you too. Make sure it matches the CNAME you've created, e.g., diaspora.[yourdomain].net. Once you've installed your SSL certificate and key, make sure that the SSL location info lines in /etc/nginx/conf.d/ssl.conf point to the location of the cert and key.  For example, 

     ssl                  on;
     ssl_certificate      /home/[your username]/diaspora/public/SSL.crt;
     ssl_certificate_key  /home/[your username]/diaspora/public/SSL.key;

in the sample ssl.conf file above point to diaspora's public directory, for a cert and key located there.

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

After a minute or so, you should be able to find your Diaspora login screen in a browser window at https://diaspora.[yourdomain].net. No you can create an account, or log in if you already have one.

If images aren't rendering properly (i.e., if the logo is absent), try setting the serve static assets variable to "true" in ../diaspora/config/environments/production.rb (or development.rb if that's what you have).

## 10. Relocating Your Pod

If you change local network locations (for example, you take the laptop hosting your pod to an internet cafe), Pagekite will update your DNS settings automatically.

OK? Good luck. Hope to see you on Diaspora. (Oh, one last thing: operating a web server may violate your internet service provider's service terms and conditions. If you're concerned about that, review your service agreement.)