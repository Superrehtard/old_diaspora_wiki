----

###403 DO NOT MOVE###

We're currently **moving this wiki over to our new project site**. The contents of this page are not important enough to be ported over because they are either very old, very outdated or wrong and misleading. 

----

Diaspora PGP Brainstorm
====================


Background.
-----------------
The Diaspora\* team initially set out to build a pgp-based distributed social network. Later, due to implementation problems, they switched to not encrypting the content, but instead using SSL for communication between pods. This however has the drawback that it make Diaspora\*'s security depend entirely on the security of the pod that runs your seed. This situation was discussed (at length) on the diaspora-dev and diaspora-discuss mailing lists. The conclusion was that the Diaspora\* team would like to switch back to using PGP. On the other hand, they don't believe (like many others on the list) that end-to-end encryption (e2ee) would be feasible with the current state of predominant installed (browser) technology. This brainstorm document tries to investigate ways for Diaspora* to use PGP.


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

- D - hybrid of end-to-end and pod-based encryption
- D1*/*** - browser-based clients are able to communicate with encrypted messages using public/private keys stored on the pod. Local clients are able encrypt messages with private keys stored on the local computer. Local clients are also able to post messages using the pod-stored keys. Messages using local-client encryption are flagged as secure so browser-based clients are not subjected to encrypted hash messages, but will instead receive a placeholder telling them to log with a local client. Users would be able to log in with either browser or local clients. (Note: local client refers to a non-browser based client, or browser-based client that runs locally and stores keys without transmitting them)

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

PGP with e2ee and optionally doing it on the user's home pod
-----------------
I think the best is if each user could choose between C1 and C2 options. But the trust model should
be extended in the following way:

For users who trust thier pod enough, it should be possible to:

- 1 - store the passphrase-protected private PGP key on the pod
  this is useful when sometimes using the (trusted) computer of a (trusted) friend
- 2 - store a copy of the key with a Pod-generated passphrase on the pod (or unencrypted, the security is the same) for passprase recovery
  this would require putting extreme trust into the home pod, but trusting the home pod is the only
  option for users concerned about loosing their password/passprase
- generate the PGP key pair on the Pod
- maybe a user should have also the option to have 2 PGP keys, one home-generated and only used at home, and one on the server. This way he could view the data of concerned users when at home and still view not-so-concerned-user's data when on travel and forced to use an untrusted client or the pod.

For users concerned about security, it should be possible to:

- not trust users who trust thier pod, either in the first or the second of above ways, or both, and they should be able to set up whitelists of pods to trust.
- only trust users who never trust their pod
- only trust users who use a home-generated key and only when they use it at home

To summarize, in case my ideas are too confusing: The benefit of that model is that every user can choose to either insist in e2ee for his data, or to trust one or several pods either fully (including the private key) or partially.

I hope this is the right place to add my ideas to the brainstorming. And I apologise if some of my ideas already have been discussed. I am new to diaspora internals (found it today) and I did not find any public mailing list for such discussion.


One more idea about local client
-----------------
Instead of using non-browser based client, a browser extension like FirePGP could be used to encrypt/decrypt the content within the browser.