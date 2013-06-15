----

###403 DO NOT MOVE###

We're currently **moving this wiki over to our new project site**. The contents of this page are not important enough to be ported over because they are either very old, very outdated or wrong and misleading. 

----

This page is meant to sum up the discussion so far.

It should also be considered to use a sole server.

# Concept
The aim of this concept is to achieve a higher level of security. The system is divided into two parts, a server and a client. The task of the server is storage and message forwarding. The client is responsible for providing the user interface.

For the task of storage and message forwarding the server isn't required to understand the context of the message, which, hence, can be encrypted. The en/decryption is done on side of the client. This means that the private key is also managed by the client. The data the client needs is: the server address, the user id and the private key. The rest can be stored encrypted on the server.

# Further possibilities 
To avoid that the user has to handle the private key, it would be possible to store the key encrypted on the server, although the encryption by a short pass phrase is not very strong.

#Discussion
## Pros
* even if server is corrupt, the data is secure
* possibility to integrate diaspora service into application
* a API for integration is needed anyway
* the client can be chosen by the user and can be a
  * web frontend
  * app on smartphone
  * dedicated application on local computer (as well as a plugin for thunderbird, firefox or whatever)

## Cons
* more complex
* the user maybe is required to deal with a private key if he changes his frontend
* the client side could still be compromised/hacked.
* this solution only improves security for those who develop their own client app. Otherwise, it just move the trust from the seed provider to the client app developer.