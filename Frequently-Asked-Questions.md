This page documents questions that are frequently being asked in relation to the installation and use of Diaspora.

If you have other questions, please try #diaspora on irc.freenode.net.

***Are there any public demo servers of Diaspora online that I can try?***

Yes. The following are the official Diaspora demo instances, which will be wiped of all content at regular and unpredictable intervals:

[[http://tom.joindiaspora.com]]<br>
[[http://washington.joindiaspora.com]]<br>
[[http://adams.joindiaspora.com]]<br>

***How do I get past the login page when I don't have an account yet?***

To create a new account, go to:

[[http://yourdiasporainstance.com/signup]] (replacing *yourdiasporainstance.com* with the the host name of your seed).

*** Whats the diference between production and master branches?***

Production is what we were using and updating, maybe bi weekly for a few advisors and friends and such, at this point its not that much different, other than it runs in production mode as default rather than in dev mode.

***Tips to actually get this to run?***

[Webfinger patch](http://github.com/diaspora/diaspora/issues/issue/83/#issue/83/comment/411202) (unofficial, may need updating)

[Registration patch for error 'undefined method `receive_url' for nil:NilClass'](http://github.com/diaspora/diaspora/issuesearch?state=open&q=url#issue/14/comment/411064) (unofficial)

Your server must be on port 80, or you must forward 80 to 3000.  Otherwise friend requests may cause lockups on other servers.  Commandline switch is -p 80.

Your server must also have 8080 available for websockets.

***Installing on other distros?***

[Unofficial guide for Arch Linux installation](http://www.diederickdevries.net/blog/2010/09/16/diaspora-on-arch/)


