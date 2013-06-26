----

###403 DO NOT MOVE###

We're currently **moving this wiki over to our new project site**. The contents of this page are not important enough to be ported over because they are either very old, very outdated or wrong and misleading. 

----

This page is meant to sum up the discussion so far

# Public-private key
To enable an efficient encryption of posts (also files) for groups (aspects) the following is applied:

1. a random key (RK) is generated
1. the post is encrypted with the random key: enc(RK, msg)
1. for each recipient Rn, RK is encrypted with their public key: enc(pub(Rn), RK)
1. the encrypted key is sent to each recipient

If a friend is added to the group, RK is encrypted once more.
If a friend is removed from a group, we don't generate a new RK to re-encrypt everything.

# SSL
Encrypt the connection between servers...

Leamas: According to the Security-Architecture-Proposal should SSL not be needed between servers. 
 OTOH, it *is* needed  in the pod/browser interface. Or am I missing something?

This is the internet we're talking here.  Do you want Comcast or NTT or anyone and their dog with Wireshark to have complete visibility into the data you're claiming to keep private?  If the answer is yes, then sure, go ahead and don't bother encrypting your private data as it's on the move across cables and routers that your users have no control over or ability to see the operation of.  Oh, also, did you know that BGP (the magic that core routers use to plan routes) has been hijacked by China within the last year such that all the traffic they asked for -- world wide -- was quietly routed through them?  But I'm sure they promise to close their eyes and stop recording when it's Diaspora talking between pods, so don't worry about it.
  --


# see also:
[[Security-Architecture-Proposal]]