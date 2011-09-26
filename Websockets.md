The websocket server is not essential and only provides the live updates to the webfrontend. It has nothing to do with federation.

Diaspora* server provides a WebSocket server on the default TCP/8080 port (which can be changed in `application.yml`).

WebSockets are part of HTML5 which may or may not be suported by your browser - most probably aren't. Diaspora* uses Adobe Flash to "hack" websockets for today's browsers, which means a few additional steps required.

You need to have a [http://www.beamartyr.net/articles/adobepolicyfileserver.html flash policy server] to operate on tcp/843 which serves an XML to allow cross domain access for Flash. You can use the linked module for apache.

The default seem to work only when no ssl available (links ws://pod:8080), but this is pretty insecure. To be able to use SSL you have to provide your key and its certificate (chain) in the `application.yml` settings:

        socket_secure: true
        socket_cert_chain_location: '/.../keys/diasp.crt'
        socket_private_key_location: '/.../keys/diasp.key'

But this isn't enough. You have to use a modified `crossdomain.xml` as well:
        <cross-domain-policy>
            <allow-access-from domain='*' to-ports='*' />
        </cross-domain-policy>
(Obviosuly you should modofy it to be more secure, but you can start with the free-for-all version here.)

You can switch on socket debugging on console with ` socket_debug : true` in `application.yml`.