*This is a work in progress. No links lead to this page, and that's for a reason!*

Diaspora needs a deployment strategy.Some background:

- Security and overall communications, as in  [[Security Architecture Proposal]]
- The need for home user deployment, in [[Roadmap]]

## Diaspora communication

A diaspora server, a pod, communicates with a client and  other pods out there. Three interfaces exists:

- With the client. For the default setup, the client is a browser. Today this is done on port 80 for the
"regular" traffic and port 8080 for websocket use. However, the client/pod communcation needs to be
encrypted which will be  done using http/SSL i. e. https. This also includes the websocket traffic(?)
- Offering webfinger services on port 80. This is a public service containing no sensitive data.
- With other pods. The data in this interface lis encrypted as needed, and can be transferred as-is i. e.,
using plain http. Today. it runs  on port 80.

## What's a "web server"?

The word "web server" has become ambigious   in this context. The basic setup is a fronting web server 
(nginx in reference installation), and an application web server (thin in reference install).

The front  webserver is typically something the user already has running. The application server is a 
specific server for diaspora, with specific requirements for running rails etc.

It's clear that it should be possible, and not hard, to use almost any web server as a fronting one. The basic
requirement is to handle a few static files, and to forward traffic to/from diaspora.

As for the application server, the reference install uses thin. It's unclear whether other alternatives such as
apache passenger might also work. There is some discussion on [[Discussions on fedora apache wiki page]]

## Use cases

Three use cases/scenarios:

#### The dedicated server.

This is a simple machine containing only the diaspora server and it's dependencies. Examples might include a 
bootable USB stick or a hosted virtual server. The key requirement is to enable the diaspora service with as
litlle overhead as possible.

The problem how to route traffic to/from this virtual server is beyond the scope of this page. 

No front webserver should be needed here, diaspora should be able to bind directly to the ports it uses.

####The home user.

The home user typically already runs a web server containing some static pages.  It might also contain some other 
web applications both on port 80 and possibly on port 443. The server certificate, if any, is self-signed.
She might have an own DNS-domain or just a dynamic host address like example.dyndnsprovider.com

Diaspora should be easy to install on the existing  server, and coexist with other user services on both port 80 and port 443.

The basic approach here would be, as today, to deploy diaspora on an arbitrary, unused port e. g. 3000. The existing webserver
will have to forward traffic from/to this server from the "official" URL. It might also need to  handle one or two static files, one being the
webfinger reply,  to implement diaspora's interface on port 80.

#### The corporate user

This user has a multitude of servers. She controls a DNS domain and an IP address block.  She also  have a signed SSL certificate on 
at least one host which she might or might not want to associate with diaspora. 

In general,  this user  a similar need as the home user  to forward traffic to the app server running diaspora. Here, this might mean 
to forward to another machine without packing up the SSL envelope.

# Discussion

The main problem is to forward the traffic to diaspora while keeping things simple and consistent.

One part of this is diaspora's use of port 80. For almost all servers, this is already used for other purposes. This means that we
must setup the user's existing server to forward traffic to/from, diaspora. I doubt this will be a straightforward procedure, there are
to many webservers, operating systems, web applications and routers out there. To make it easy diaspora should
be non-intrusive, leaving existing functionality and configuration unaffected where possible.

The simplest would be if diaspora could use an arbitrary port for the pod/pod communication. This could be accomplished by remote
peers finding out the URL:s actually required by issuing a (possibly webfinger) request on port 80. With this in operatfion, all http
traffic could be run on a specific url like //host.domain.tld;:3000 .  Net result is that the pod-pod  http traffic could be handled by one or
two static files in the existing port 80 http server, and a virtual server mapping in the router/fw. 

The client/pod https communication could really take place on any port or url. For a host  host.example.org url:s like diaspora.example.org,
host.example.org/diaspora and host.example.org:11443 are all possible,with different pro/cons:

- Using a separate port like https://host.example.org:11433/ is, as for http, the simplest to handle on the server side. But some users lives behind firewalls blocking outbound https requests to other ports than 443. Upcoming websocket implementations might also limit the range of allowed ports.
- Using a separate domain like diaspora.example.org. This makes it easy do define a virtual server in the existing web server configuration. But
it needs a DNS registration, which might be troublesome e. g., for a user with just a dynamic DNS address. OTOH. this is the only way which makes
it possible to forward https traffic to another host without packing up SSL.
- Using a sub-uri like host.example.org/diaspora. This does not require any DNS registrattion, and provides a simple mechanism to
map what should be forwarded to diaspora for the web server. This is the way mythtv, one of the battle-hardened web applications for home use, works.

## Conclusions

- The virtual host installation doesn't need to forward at all, leaving the routing issues to the vhost  container.
- The home user typically wants to use a sub-uri like https://host.example.org/diaspora
- The corporate user typically wants to use the named virtual host like diaspora.example.com
- A dynamic approach for the URI:s used in pod/pod communication would make deployment a lot easier.
- Diaspora should be agnostic as to what https URL:s are used to access the pod from the browser
