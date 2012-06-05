Diaspora on a Windows Box

It is technically possible to run Diaspora on a Windows box. It is a Rails application. You can use Rails Server for development, and you can even run it on IIS/SQL through CGI. But i don't recommend it at this time.

The configuration is not well documented and it requires advanced knowledge of both Windows and Linux environments. Future program changes could easily break it. Best bet is to wait for future developments.

One current problem is building native C extensions for gems that are not yet cross compiled. So you are into updating and creating new gems, or forcing a gem install.

But if you like a challenge, it certainly is one. A friendly install for Windows would go a long way toward the growth of the program!