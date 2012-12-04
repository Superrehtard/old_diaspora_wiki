Nginx is a lightweight webserver that is a easy front end for Diaspora*

This is a basic configuration for a standard pod install, you will need to scale it up if you grow.

If you are running Diaspora* in production mode, you may want to comment out the "daemon off" line,

NOTE: If you're using a StartSSL Cert and got Problems with your chain cert, check out this  
https://gist.github.com/1825744

```
worker_processes 1;
daemon off;
events {
  worker_connections  1024;
}

#
# FIXME: You may wish to modify the value of the `log_format` directive
#        below if you are not using Splunk
#
http {

  include       mime.types;
  default_type  application/octet-stream;
  sendfile on;
  keepalive_timeout  65;
  gzip              on;
  gzip_http_version 1.0;
  gzip_comp_level   2;
  gzip_proxied      any;
  gzip_buffers      16 8k;
  gzip_types        text/plain text/css application/x-javascript text/xml application/xml application/xml+rss text/javascript;
  gzip_disable      "MSIE [1-6]\.(?!.*SV1)";

#
# FIXME: If using thin app server, specify correct number of thin servers
#        below, otherwise comment out and replace with your own solution
#
# In case of unicorn - master opens a unix domain socket
#upstream unicorn {
#    server unix:/var/run/sockets/unicorn.sock;
#}

upstream thin_cluster {
  server          localhost:3000;
}

#
# FIXME: specify correct value(s) for `server_name` directive and
#        correct domain name in the `rewrite` directive below
#
server {
  listen       80;
  server_name  example.com  www.example.com;
  rewrite      ^(.*) https://example.com$1 permanent;
}

#
# FIXME: specify correct value(s) for `server_name` directive and
#        `ssl_certificate` + `ssl_certificate_key` directives below
#
server {
  listen       443;
  server_name  example.com  www.example.com;
  ## make sure you change location if you did clone into /usr/local/app
  root         /usr/local/app/diaspora/public;

  ssl on;
  ssl_certificate      /path/to/cert_location;
  ssl_certificate_key  /path/to/key_location;
  # enable better ssl security if you like to mitigate BEAST and other exploits
  #ssl_session_cache       shared:SSL:10m;
  #ssl_session_timeout     5m;
  #ssl_protocols           TLSv1;
  #ssl_ciphers             ECDHE-RSA-AES256-SHA384:AES256-SHA256:RC4:HIGH:!MD5:!aNULL:!EDH:!AESGCM;
  #ssl_prefer_server_ciphers on;
  #add_header              Strict-Transport-Security max-age=500;
  #ssl_ecdh_curve          secp521r1;

  location /uploads/images {
  expires 1d;
  add_header Cache-Control public;
  }
  location /assets {
  expires 1d;
  add_header Cache-Control public;
  }

#
# FIXME: modify the `rewrite` directive below to point to proper S3 bucket
#        and path or comment out if you will store images on local file system
#
location / {
  proxy_set_header X-Real-IP $remote_addr;
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  proxy_set_header Host $http_host;
  proxy_set_header X-Forwarded-Proto https;
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
    proxy_pass http://thin_cluster;
    break;
  }
  #if you switch to a s3 bucket you can redirect old links to the s3
  #rewrite ^/uploads/images/(.*)$ https://example.com/s3bucket/s3path/$1 permanent;
}

  error_page 500 502 503 504 /50x.html;
  location = /50x.html {
  root html;
  }
}

}
```
