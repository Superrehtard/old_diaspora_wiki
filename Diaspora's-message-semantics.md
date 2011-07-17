This document describes the semantics of each type of message that Diaspora pods send to one another.  Together, they constitute the semantics of Diaspora's federation.

When Alice wants to send a message to Bob, she will construct one of these messages detailed below, and she will wrap it up and send it out using [[Diaspora's federation protocol]].

# Basic concepts

Before we dive in to the semantics of each mesage, we should cover some basics of how Diaspora thinks of the world, so that the messages will make more sense.

Keep in mind, too, that Diaspora's protocol will one day evolve to be interoperable with [OStatus](http://ostatus.org/sites/default/files/ostatus-1.0-draft-2-specification.html).  However, the current design of OStatus was made with the idea that all posts would be public.  This doesn't work for Diaspora.  Fortunately, OStatus is [evolving to support limited distribution](http://groups.google.com/group/ostatus-discuss/browse_thread/thread/7f15df316d6c14d3).  Thus, Diaspora and OStatus will converge.

## Local vs remote people

Diaspora has a notion of "users on the local pod", and a separate notion of "people".  Some people are also users on the local pod, but other people live on other pods, and are thus remote.

This is important to keep in mind, because many of the more familiar forums and systems are designed with only local people in mind, and thus think only of "users".  When designing a federated system, though, you must model remote people as well.  (See ["How to OStatus-enable Your Application"](http://ostatus.org/2010/10/04/how-ostatus-enable-your-application) for more information on modeling remote users.

## Local vs remote delivery

Sometimes, activity happens on your pod which affects local people as well as remote people.  In these situations, Diaspora implementations SHOULD deliver all relevant notifications to local people as soon as the need to do so presents itself.  The implementation SHOULD then also deliver to remote people, but SHOULD NOT wait for a response from the remote pod before delivering to the local people.

One example where this is imporant:  Comments.  Say Alice and Eve live on the same pod.  Bob lives on a remote pod.  Bob makes a post that is known to Alice and Eve.  Alice comments on the post.  She must deliver her comment to Bob's remote pod.  However, she can inform Eve of the comment immediately.  She SHOULD NOT wait to hear from Bob's pod that the comment has been accepted.  Thus, there may be a time when people on Alice and Eve's pod see the comment thread a little bit differently from people on Bob's pod.  This is expected.

## Relayability

There are certain circumstances where Bob is the originator of a post that Alice is responding to.  All of the people who saw Bob's post should see Alice's response as well.

Imagine this situation:
Alice, Bob, and Diego all live on different pods.  Bob shares with Alice and Diego, but Alice and Diego are unaware of each other.  Bob makes a post.  Alice responds to Bob's post.  Diego should see Alice's response.  But how will Alice's response be transmitted to Diego?

The answer is that Alice will send a "relayable" response to Bob, and Bob will determine who needs to see the response, and will relay it accordingly.

Thus, Bob can send Alice's message to Diego, even though Alice does not know the original list of recipients to Bob's post, and in fact, may remain unaware of Diego's existence.

# The Messages

Diaspora currently defines the following messages:

* Notification that you've begun sharing with them.
* Posts that you've made.
* Comments that have been made (by you or others) on one of your posts.
* "Like"s that have been made (by you or others) on one of your posts.
* Conversations (each thread in the inbox has an object representing it)
* Messages (each individual message in a Conversation)
* Profile information
* Retractions of posts
* Retractions of likes/comments

Note, however, that Diaspora is in alpha and its protocol is in flux, so this list is subject to change and this document may be out of date.  The most up-to-date information resides in the source code for Diaspora's reference implementation, which is a Ruby on Rails application.  The files to pay attention to are those application models that inherit from WebHooks.