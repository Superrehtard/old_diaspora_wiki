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

One simple example:  Comments.  Say Alice and Eve live on the same pod.  Bob lives on a remote pod.  Alice makes a post which is visible to both Bob and Eve.  Eve comments on Alice's post.  Eve's comment can be made visible to Alice simply by updating Alice's pod's internal database; nothing need be serialized or sent over the wire.  This SHOULD be done immediately.

However, Eve's comment must be sent to Bob over the wire.  Alice should be able to see the comment, even if it never reaches Bob.

### Live updating

If a pod is implemented as a website, it MAY choose to do some form of "live updating".  For example, if Alice and Eve are both logged in to the website, and Eve comments on Alice's post, their pod MAY choose to send some sort of notification to Alice's webserver without waiting for Alice to refresh the page.  In the reference implementation, this is done through websockets.  It may also be done via ajax polling.

## Relayability

There are certain circumstances where Bob is the originator of a post that Alice is responding to.  All of the people who saw Bob's post should see Alice's response as well.

Imagine this situation:
Alice, Bob, and Diego all live on different pods.  Bob shares with Alice and Diego, but Alice and Diego are unaware of each other.  Bob makes a post.  Alice responds to Bob's post.  Diego should see Alice's response.  But how will Alice's response be transmitted to Diego?

The answer is that Alice will send a "relayable" response to Bob, and Bob will determine who needs to see the response, and will relay it accordingly.

Thus, Bob can send Alice's message to Diego, even though Alice does not know the original list of recipients to Bob's post, and in fact, may remain unaware of Diego's existence.

### De-duplication

Occasionally, Alice's pod may have more information than Alice herself should have.  In this case, Alice's pod MAY choose to use this information to streamline communications or storage, as long as nobody is shown a message that they shouldn't see.

Consider the situation where Alice and Eve live on the same pod, but Bob lives on a remote pod.  Bob makes a post, which is delivered to both Alice and Eve.

Each comment is given a GUID.

Now, Bob wraps up two salmon slaps in order to send the message:  One encrypted with Alice's RSA public key and one with Eve's.  But he sends them both to the same pod.  Pods MAY store their users' RSA private keys and decrypt messages on behalf of its users, and MAY store messages in cleartext.  In fact, this is what the reference implementation does.

So, when Alice receives her copy of Bob's original message, Alice's pod stores the cleartext in the database.  Later, when Eve's copy of the message comes in, Alice's pod notices that the GUID of the new message matches one that already exists in the database.  Thus, when Eve's copy of the message comes in, Eve's pod does not choose to store a second copy of it.  Instead, it just makes a note in a local visibility-permission table, noting that Eve should be able to see the message.

All comments MAY be made visible to everybody who saw the original post.  If using this system,  when Alice makes a comment, Alice's pod looks at the visibility table and notices that Eve should also be seeing the comment.  Thus, the comment can be delivered locally to Eve before receiving the relayed comment from Bob.

Note that this de-duplication is optional.  A pod MAY store duplicates of messages, and MAY wait to deliver Alice's comment to Eve until the pod has heard Bob's relaying of the message.

One reason a pod may do this is if the pod does not store its users' RSA keys and does not decrypt messages on behalf of its users.  Thus, the pod cannot read the message to find out the GUID, so de-duplication cannot be performed.

Another consideration is that using de-duplication in this fashion will rob Bob of his choice of whether or not to forward the comment to Eve.  If de-duplication is not used, Bob can do filtering and moderation on Alice's comment.

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

Nevertheless, this document will describe the messages as they were at the time of writing.  This document should be updated when the messages change.

## Sharing Notifications

Remember that Diaspora uses an [[asymmetric model|Diaspora's federation protocol#Asymmetric Sharing]] of sharing.  That is, Alice can choose to start sharing with Bob, and Bob need not approve this action, and Bob need not reciprocate.  However, Bob's pod SHOULD NOT show Alice's original posts in the main location unless Bob is also sharing with Alice.  Alice's responses to Bob's posts SHOULD be shown with the rest of the responses.  However, since Bob, in this scenario, is not sharing with Alice, Alice will only be able to respond to Bob's _public_ posts.  
If Bob is sharing with Diego, and Diego is sharing with Bob, and Diego is also sharing with Alice, then Alice will see Diego's posts and be able to respond to them.  Bob should see Alice's responses to Diego's posts with the rest of the responses.

The above is just a little refresher on the asymmetric semantics of Diaspora's sharing model.  But this section deals with the actual messages that are sent over the wire.

If Alice decides to share with Bob, then Alice's pod MUST transmit a Sharing Notification to Bob.  (Note: In the reference implementation, these are called "requests"; this is a holdover from when Diaspora had a symmetric model of sharing).

A Sharing Notification looks like this:

```xml
<XML>
  <post>
    <request>
      <sender_handle>alice@alice.diaspora.example.org</sender_handle>
      <recipient_handle>bob@bob.diaspora.example.org</sender_handle>
    </request>
  </post>
</XML>
```

(where alice@alice.diaspora.example.com is Alice's [[Webfinger address|Diaspora's federation protocol#Discovery]]) and bob@bob.diaspora.example.com is Bob's.

Alice will then wrap this up as a salmon slap and send it to Bob according to the methods described in [[Diaspora's federation protocol]].

## Status updates

Alice may choose to post a status update.  In posting this message, Alice must determine who she wants to receive the update.

Diaspora implementations MAY choose to offer an easy management interface for Alice's contacts, and an easy way to choose recipients.  For example, the reference implementation offers "aspects", which are groups of contacts known to Alice.  Alice organizes her contacts into various aspects.  In posting a status update, Alice chooses which aspects should receive the message.

However Alice chooses recipients, the message is sent out in the following fashion:

Alice will serialize the message like this:

```xml
<XML>
  <post>
    <status_message>
      <raw_message>((status message))</raw_message>
      <guid>((guid))</guid>
      <diaspora_handle>alice@alice.diaspora.example.org</diaspora_handle>
      <public>false</public>
      <created_at>2011-07-20 01:36:07 UTC</created_at>
    </status_message>
  </post>
</XML>
```

* `<raw_message>` is the text of the message that Alice wants to send out.  
* `<guid>` is a string of 16 hexadecimal digits.  Alice's pod MUST choose a new GUID for each status message, and MUST retain the guid, to receive responses to the post.
* `<diaspora_handle>` is Alice's [[Webfinger address|Diaspora's federation protocol#Discovery]].
* `<public>` is the string "true" or "false".  If it is set to "true", then Bob MAY share Alice's post with others.  If the string is "false", then Bob SHOULD NOT share the post with others. (although Alice must know that once she has sent a message to Bob, if Bob is capable of reading that message, he is capable of abusing its contents).
* `<created_at>` is the time that Alice posted the message, using the strftime format string of "%Y-%m-%d %H:%M:%S %Z".  (Note:  This is similar to, but distinct from, ISO 8601 format).

Alice will then wrap this up as a _separate_ salmon slap for _each_ of the intended recipients and send it to each recipient according to the methods described in [[Diaspora's federation protocol]].  Note that if Alice is sending the message to Bob and Diego, she MUST send separate salmon slaps for each recipient, even if Bob and Diego live on the same pod.  This is because the slap MUST be encrypted separately for each recipient (encrypted according to the rules in [[Diaspora's federation protocol]]).

### Public status updates

If Alice wants her post to be public, she MAY choose to share her post on other systems or services.  For example, the reference implementation allows Alice to mark a post as public.  The post is then be available to the world on an [[ActivityStream|Diaspora's federation protocol#ActivityStream of public posts]].

However, even if the post is made public, Alice MUST encrypt the salmon slaps that she sends to the specifically-intended recipients.
