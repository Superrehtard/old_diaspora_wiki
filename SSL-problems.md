----

###403 DO NOT MOVE###

We're currently **moving this wiki over to our new project site**. The contents of this page are not important enough to be ported over because they are either very old, very outdated or wrong and misleading. 

----

'''work in progress'''

== Prologue ==
** Do not try to use self-signed certificates! **

Diaspora* try to verify all certificates against the known certificate chains, and **every** failed checks fail **silently** and with **no sign in the debug logs**. If you want to connect other pods you have to use globally verifiable certs. (I do not want to advertise any service but StartSSL is free and their cert chain is globally accepted; if anyone knows any other free and globally accepted authority feel free to tell. [CACert isn't one.])

== SSL setup can be tricky ==

* ssl setup requires a globally verifiable crt, self signed won't do
* diaspora.yml contains most of the setup
** environment.url should contain https:// and proper PODURL address
** set up proper proxying in webserver (forward tcp/443 to localhost:3000)
** certificate_authorities should be able to verify your cert:
*** <code>openssl verify -CApath /dev/null -CAfile ca-certificates.crt  yourpod.crt </code>  - should give you OK 

If you change your pod from http: to https: you have to fix every entry in your configs **and** your database.

== Webfinger ==
Your <code>public/webfinger</code> directory should be empty. The files are generated on the fly if there is no matching file there, and if there is, the files will be used instead.

Check your webfinger by retrieving PODURL/.well-known/host-meta it should point to proper https://PODURL

Check hcard in the response file, it should point to https://PODURL