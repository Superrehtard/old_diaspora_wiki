Diaspora on a Windows Box

It is technically possible to run Diaspora on a Windows box. It is a Rails application. You can use Rails Server for development, and you can even run it on IIS/SQL through CGI. But i don't recommend it at this time.

The configuration is not well documented and it requires advanced knowledge of both Windows and Linux environments. Future program changes could easily break it.

One problem at the moment is with building native extensions. They are linked to curl directories in a way that is not native to Windows and the files defining the links (which are still hard coded) are overwritten by bundle install. So you are into creating new gems and bundles with relative links instead of hard links.

But if you like a challenge, it certainly is one. A friendly install for Windows would go a long way toward the growth of the program!