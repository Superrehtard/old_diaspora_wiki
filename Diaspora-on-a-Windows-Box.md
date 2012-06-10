Diaspora on a Windows Box

It is technically possible to run Diaspora on a Windows box. [Example](https://github.com/diaspora/diaspora/wiki/Win32-Gem-List). It is a Rails application. You can use Rails Server for development, and you can even run it on IIS/SQL through CGI. But i don't recommend it at this time.

The configuration is not well documented and it requires advanced knowledge of both Windows and Linux environments. Future program changes could easily break it. Best bet is to wait for future developments.

In the mean time **You** can still start hacking Diaspora from your Windows PC by installing it to a cloud server that supports Ruby on Rails such as [EngineYard] (https://github.com/diaspora/diaspora/wiki/Deploy-Diaspora-to-Engineyard-using-a-Windows-PC) or [Heroku] (https://github.com/diaspora/diaspora/wiki/Installing-on-heroku).

One current problem with having a consistent installation script for the Windows box is building native C extensions for gems that are not yet cross compiled. So if you go that route you are into updating and creating new gems, or forcing a gem install. But if you like a challenge, it certainly is one.

A friendly install for Windows would go a long way toward the growth of the program!

[Getting Started With Contributing](https://github.com/diaspora/diaspora/wiki/Getting-Started-With-Contributing)

Some Related Links:
- https://github.com/djberg96/win32-mmap
- https://github.com/typhoeus/typhoeus
  [update 6/8/12: the typhoeus gem requirement for native c extensions has been removed, as of typhoeus [version 0.4.0] there is no problem with native c extensions for typhoeus on Windows, they do not exist]
- https://github.com/typhoeus/typhoeus/issues/172
- https://github.com/bagder/curl
- https://github.com/eventmachine/eventmachine/issues/307
- Out Dated http://blog.srmklive.com/2010/09/21/how-to-install-diaspora-on-windows/
- Out Dated http://tom.net.nz/2010/09/installing-diaspora-on-windows/