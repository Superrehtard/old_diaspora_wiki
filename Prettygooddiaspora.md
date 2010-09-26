Pretty Good Diaspora
====================


Context.
------------
This document is the product of an informal workshop organized 24-26 September 2010 in Navalcarnero, Spain, by a couple of friends who call themselves Unhosted. We've only just started, but aim to make the world a better place by promoting 'unhosted' solutions, like Diaspora*, FreedomBox, GNU Social P2P, etcetera. Please follow us on [Twitter](http://www.twitter.com/unhosted) in case we have interesting things to say in the future!


Background.
-----------------
The Diaspora\* team initially set out to build a pgp-based distributed social network. Later, due to implementation problems, they switched to not encrypting the content, but instead using SSL for communication between pods. This however has the drawback that it make Diaspora\*'s security depend entirely on the security of the pod that runs your seed. This situation was discussed (at length) on the diaspora-dev and diaspora-discuss mailing lists. The conclusion was that the Diaspora\* team would like to switch back to using PGP. On the other hand, they don't believe (like many others on the list) that end-to-end encryption (e2ee) would be feasible with the current state of predominant installed (browser) technology. This workshop tries to investigates ways for Diaspora* to use PGP. Halfway the workshop now, our conclusions are below, and I must say they're different from what we thought when we started yesterday.


Options.
-----------------
Options are:

- A* - not bringing back PGP, and sticking with the current unencrypted storage, and inter-pod communication over SSL.

- B - using PGP for inter-pod communication only. Two options to implement this:
- B1* - the private key is stored unencrypted on the server
- B2* - communication only takes place while the recipient is logged in

- C - using PGP to encrypt stored data 
- C1** - server-side: while the user is logged in, the private key is in memory
- C2*** - client-side: data is already encrypted when it reached the server/pod.

The number of stars here refer to how secure we think each of these options would be (roughly):
1 star: gives security against attacks from outside all pods
2 stars: also gives security against attacks with read access to any pod's disk (but not its memory)
3 stars: security against any attack (that we can think of) with read/write access to any pod.


e2ee or no PGP at all.
-----------------
This morning we concluded that unless you use end-to-end encryption (encryption in the browser or client), there is not much point in using any PGP at all. Because as was already brought up several times on the Diaspora* mailing lists, even if a pod stores the data encrypted, an attacker could put a keylogger on the login page, phish the passwords, and get to the data anyway. There are special cases, like where an attacker has read-access to the disk but not write-access, but in general, you would be paying the full price for half a solution. If we only distinguish 'trusted' and 'untrusted' hosts, then it's e2ee or nothing.

e2ee is not for everyone yet.
-----------------
Right now, the general internet landscape is not ready for e2ee. Power users (mainly developers and open source enthusiasts) already use PGP, most of us probably more out of idealism than out of practical considerations, because it still poses a usability hurdle in most cases. The only exception would mainly be applications for phones and tablets, where e2ee could be feasible already nowadays for specific apps, without any penalty in usability.

The closest thing to e2ee with general purpose clients (browsers) is the java applet that hushmail.com uses, but even they don't use that by default, because not everybody has java installed, etcetera.

Also, using PGP encryption has the negative effect that it makes password recovery inherently impossible. A normal user usually relies on being able to reset his password if he forgets it. OpenId tries to change this situation, but for now, it's the current situation for most normal users.

And lastly, if you host your seed in your home, then that host is equally trusted as the client, so it makes no sense to hide your data from it. There is no trust barrier between the client and the pod in this case.

mixing e2ee pods and 'dodgy' pods.
-----------------
For these reasons, we propose for Diaspora to *support* PGP (be PGP-ready), but not use it by default. Imagine a web of Diaspora nodes, some of which want to use PGP, and some of which don't. For this to work, all we need to fix is the inter-pod communication protocol. The implementation of each pod is up to the pod, and the users that choose to have their seed on it.

There are then 4 situations at the moment that a message is sent between two pods:
- dodgyPod to dodgyPod: the current situation. Sent over an SSL channel, with a MagicSig envelope for identifying the sender.
- dodgyPod to PGPod: since the data comes from a dodgyPod anyway, there is no need to be too fussy about it. The dodgyPod will send the current way, without using any PGP, and the PGPod accepts it, just like it would accept data that comes aggregated from outside Diaspora.
- PGPod to dodgyPod: this is the case we need to talk about. see below.
- PGPod to PGPod: the ideal case. fully encrypted throughout the client→pod→pod→client path. The client encrypts the data for all recipients of the aspect that the data belongs to, and posts it encrypted through the sender's pod, it stays encrypted while stored, and through the receiver's pod, until it reaches the client of each receiver, where it is finally decrypted. A PGPod never gets to see a user's private key, and never decrypts data that passes through it (except maybe for discovery and/or user search, if enabled).

making dodgyPods PGP-ready.
-----------------
This is the step that needs work. As we understand from the code, every pod already has a (private, public) key pair for each seed, that it uses for verification. We need to extend Diaspora's protocol, to make it capable of receiving PGP-encrypted messages, and decrypting them using the same keys. This way, a PGPod can relay an encrypted message from a client to a dodgyPod without ever opening it, and without ever being in contact with any private keys. The rest of this workshop will concentrate on adding support for receiving PGP encrypted salmons to the methods in lib/diaspora/user/receiving.rb. Even though at present this functionality will not be used, it is important to add it to the protocol now, so that Diaspora is "PGP ready". Then we can use Diaspora on a 'high security' pod, but still interoperate with our (less geeky) friends who may be on a 'standard security' pod.
