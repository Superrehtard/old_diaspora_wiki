----

###403 DO NOT MOVE###

We're currently **moving this wiki over to our new project site**. The contents of this page are not important enough to be ported over because they are either very old, very outdated or wrong and misleading. 

----

This seems like it overlaps with installing on ubuntu/apache, so let's get rid of one.  -- Raphael Sofaer

Basically, three issues:

-  Is it possible to use apache at all?  According to [[Installing and Running Diaspora]] the event machine required does not work w passenger + apache. Or?
- This does partly overlaps, but each OS has their own conventions about packaging, file names etc.  The only real overlap here is the
  apache configuration IMHO. 
- Where should the discussion be on this topic? Here? Mailing list? Open an issue? Or 
 
--alec leamas

=MSofaer
The page puts Apache and Thin at the same level which is crazy.  Apache is an nginx replacement, not a Thin replacement.
Passenger could be a This replacement, but making it work will be hard.
Replacing nginx with apache2 would be very straightforward, as long as you stick with Thin.
People should try this:  
[[http://articles.slicehost.com/2008/5/6/ubuntu-hardy-apache-rails-and-thin]]

=end

=leamas

OK, thanks for spreading some light on this. As it stands, with a note it doesn' really work, I guess the page could be left until I or someone else sorts this out. Obviously, I'm not the only one which have been trying trhis path, so lets keep this to show where it goes FTM.
=end

=leamas

According to [[Installing on Ubuntu Apache]], we could consider this work if we can befriend  tom@tom.joindiaspora.com . This does not work for me. However, befriending leamas@pontari.us works, I can see the request on the remote side..

However it's *not* possible the other way around: to let leamas@pontari.us befriend leamas@mumin.dnsalias.net (the latter the apache passenger instance). This results in a timeout immediately after the webfinger request (which seems to complete OK). Part of problem is that  this might be affected by my local machine not blocking port 443, which just times out on connect. According to [[Frequently Asked Questions]], this is not acceptable. But I have not found a way to actually block the port despite some tries (probably a router issue). See [Issue 360](http://github.com/diaspora/diaspora/issues#issue/360)

The behaviour is the same if diaspora is deployed with the Thin webserver.

Anyway, according to MSofaer's basic criteria, I guess this is some basic functionality. Or?

UPDATE: I have (seemingly) succeed to invite tom@tom.joindiaspora.com from my passenger instance.

=end