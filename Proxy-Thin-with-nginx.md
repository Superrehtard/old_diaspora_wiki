This is a configuration example wo thin can be proxied with thin, it's just an configuration example but it can be used as a start for your own setup

!The whole configuration file needs to be adjusted for your needs!
Just copy it to /etc/nginx/conf.d/diaspora.conf and set the right paths.

```

server {
   listen       843;
   location / {
    rewrite ^(.*)$ /crossdomain.xml;
   }
   error_page 400  /crossdomain.xml;
   location = /crossdomain.xml {
    root /dir/to/diaspora/public;
   }
 }

 server {
   listen  7894;
   server_name  domain.com *.domain.com;
   ssl on;
   ssl_certificate      /dir/to/ssl.crt;
   ssl_certificate_key  /dir/to/ssl.key;
   location / {
      proxy_set_header Host $http_host;
      proxy_pass http://localhost:5678;
   }
  }

  server {
   listen       80;
   server_name  domain.com *.domain.com;
   location / {
    rewrite      ^(.*) https://domain.com$1 permanent;
   }
   location  ^~ /uploads/images/ {
      root    /dir/to/diaspora/public;
      expires 30d;
      add_header Cache-Control public;
      access_log off;
   }
  }
 upstream diaspora_thin {
      server unix:/dir/to/thin.0.sock;
      server unix:/dir/to/thin.1.sock;
      server unix:/dir/to/thin.2.sock;
  }

  server {
   listen       443;
   server_name  domain1.com domain2.tld;
   root         /dir/to/diaspora/public;
   ssl on;
   ssl_certificate      /path/to/ssl.crt;
   ssl_certificate_key  /path/to/ssl.key;
   location / {
   #    try_files  $uri  $uri/ $uri/index.htm  @diaspora;
    try_files $uri @diaspora;
   }

   location  @diaspora {
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $http_host;
    proxy_redirect off;
    client_max_body_size 4M;
    client_body_buffer_size 128K;
    proxy_pass http://diaspora_thin;
   # break;
   }

   location ~ ^/(images|javascripts|assets|stylesheets|uploads)/  {
      root    /dir/to/diaspora/public;
      expires 30d;
      add_header Cache-Control public;
      access_log off;

    }
  location  ^~ /uploads/images/ {
      root    /dir/to/diaspora/public;
      expires 30d;
      add_header Cache-Control public;
      access_log off;

   }

   error_page 500 502 503 504 /50x.html;
   location = /50x.html {
    root html;
   }
  }


```

