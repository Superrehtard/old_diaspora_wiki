This describes a example configuration to use lighttpd as a proxy for the thin webserver and to serve the static content.

`mod_proxy` has to be enabled using `lighttpd-enable-mod proxy` on the command line.

### Proxy and Static Files

At the end of your **lighttpd.conf** append this section.

    $HTTP["host"] =~ "(^|\.)pod\.url\.com$" {
            server.document-root = "/path/to/diaspora/public/"
            url.rewrite  = ("^/$" => "/users/sign_in" )
    
            $HTTP["url"] !~ "\.(js|css|gif|jpg|png|ico|txt|swf|html|htm|svg)$" {
                 proxy.server = (""    => (( "host" => "127.0.0.1", "port" => 3000)))
            }
    }

The first line filters all requests to your pod URL. So only requests to your pod are processed.
In the second line replace the path to your diaspora public directory.
The third line is optional and redirects first time visitors to the sign in page.

The inner section filters out all requests that do not hit static files and forwards them to the thin app server.

### Secure Connections

To always use https add the following code within the previews block. `mod_redirect` has to be enabled.

    $SERVER["socket"]== ":80" {  # 
        url.redirect = ("^/(.*)" => "https://pod.url.com/$1")
    }

### Curl Expect Header

There is a problem with curl. Curl adds an Expect header to its HTTP POST requests. lighttpd rejects these requests by default. To ignore the requests add the following line.

    server.reject-expect-100-with-417 = "disable" 



