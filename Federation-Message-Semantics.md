This document describes the semantics of each type of message that Diaspora pods send to one another.  Together, they constitute the semantics of Diaspora's federation.

When Alice wants to send a message to Bob, she will construct one of these messages detailed below, and she will wrap it up and send it out using [[Diaspora's Federation Protocol|Federation Protocol Overview]].

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

If a pod is implemented as a website, it MAY choose to do some form of "live updating".  For example, if Alice and Eve are both logged in to the website, and Eve comments on Alice's post, their pod MAY choose to send some sort of notification to Alice's browser without waiting for Alice to refresh the page.  This is not done in the reference implementation.  A popular technique to use to implement this feature is [long polling](https://en.wikipedia.org/wiki/Comet_%28programming%29#Ajax_with_long_polling).

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

Now, Bob wraps up two salmon slaps in order to send the message:  One encrypted with Alice's RSA public key and one with Eve's.  But he sends them both to the same pod.  Pods MAY store their own users' RSA private keys and decrypt messages on behalf of its users, and MAY store messages in cleartext.  In fact, this is what the reference implementation does. Note that a user's private key is only stored on her own pod.

So, when Alice receives her copy of Bob's original message, Alice's pod stores the cleartext in the database.  Later, when Eve's copy of the message comes in, Alice's pod notices that the GUID of the new message matches one that already exists in the database.  Thus, when Eve's copy of the message comes in, Eve's pod does not choose to store a second copy of it.  Instead, it just makes a note in a local visibility-permission table, noting that Eve should be able to see the message.

All comments MAY be made visible to everybody who saw the original post.  If using this system,  when Alice makes a comment, Alice's pod looks at the visibility table and notices that Eve should also be seeing the comment.  Thus, the comment can be delivered locally to Eve before receiving the relayed comment from Bob.

Note that this de-duplication is optional.  A pod MAY store duplicates of messages, and MAY wait to deliver Alice's comment to Eve until the pod has heard Bob's relaying of the message.

One reason a pod may do this is if the pod does not store its users' RSA keys and does not decrypt messages on behalf of its users.  Thus, the pod cannot read the message to find out the GUID, so de-duplication cannot be performed.

Another consideration is that using de-duplication in this fashion will rob Bob of his choice of whether or not to forward the comment to Eve.  If de-duplication is not used, Bob can do filtering and moderation on Alice's comment.

# The Messages

Diaspora currently defines the following messages:

* Notification that you've begun sharing with them.
* Notification that you've ceased sharing with them.
* Posts that you've made.
* Comments that have been made (by you or others) on one of your posts.
* "Like"s that have been made (by you or others) on one of your posts.
* Conversations (each thread in the inbox has an object representing it)
* Messages (each individual message in a Conversation)
* Profile information
* Retractions of posts
* Retractions of likes/comments

Note, however, that Diaspora is in alpha and its protocol is in flux, so this list is subject to change and this document may be out of date.  The most up-to-date information resides in the source code for Diaspora's reference implementation, which is a Ruby on Rails application.  The files to pay attention to are those application models that inherit from Diaspora::Federated.

Nevertheless, this document will describe the messages as they were at the time of writing.  This document should be updated when the messages change.

## Sharing Notifications

Remember that Diaspora uses an [[asymmetric model|Federation Protocol Overview#Asymmetric Sharing]] of sharing.  That is, Alice can choose to start sharing with Bob, and Bob need not approve this action, and Bob need not reciprocate.  However, Bob's pod SHOULD NOT show Alice's original posts in the main location unless Bob is also sharing with Alice.  Alice's responses to Bob's posts SHOULD be shown with the rest of the responses.  However, since Bob, in this scenario, is not sharing with Alice, Alice will only be able to respond to Bob's _public_ posts.  
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

(where alice@alice.diaspora.example.com is Alice's [[Webfinger address|Federation Protocol Overview#Discovery]]) and bob@bob.diaspora.example.com is Bob's.

Alice will then wrap this up as a salmon slap and send it to Bob according to the methods described in [[Diaspora's federation protocol|Federation Protocol Overview]].

## Unsharing notification

If Alice is sharing with Bob, and decides to stop, she SHOULD send a notification to Bob that she is no longer sharing with him.

The notification is in this form:

```xml
<XML>
  <post>
    <retraction>
      <post_guid>6c5c09f129855969</post_guid>
      <type>Person</type>
      <diaspora_handle>alice@alice.diaspora.example.org</diaspora_handle>
    </retraction>
  </post>
</XML>
```

Here,

* `<post_guid>` is Alice's guid.  (Recall that when a new user is created on a pod, that pod MUST assign them a guid, which is a 16-character hexadecimal string).
* `<type>` is the literal string `Person`.
* `<diaspora_handle>` is Alice's Diaspora handle.

The format of this message is very similar to the format of the retraction of posts.  That is due to the fact that the reference implementation internally models these sharing retractions with the same model as the post retractions.  That is the reason that the `<type>` field is present.  That is also the reason that the guid is called `<post_guid>`, and that is the reson why the `<post_guid>` exists even though `<diaspora_handle>` is sufficient to globally specify a particular person.

Other implementations of Diaspora need not internally model post retractions and sharing retractions the same way.  But no matter how an implementation models a sharing retraction, the fields listed above MUST be present.

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

* `<raw_message>` is the text of the message that Alice wants to send out.  Note that the text of this message must be made suitable for XML using any appropriate escaping scheme.  For example, unsafe characters like `<` and `>` could be translated into their corresponding XML entities like `&lt;` and `&gt;`.  Or the CDATA method could be used.  (See the [XML specification](http://www.w3.org/TR/2006/REC-xml11-20060816/#syntax))
* `<guid>` is a string of 16 hexadecimal digits.  Alice's pod MUST choose a new GUID for each status message, and MUST retain the guid, to receive responses to the post.
* `<diaspora_handle>` is Alice's [[Webfinger address|Federation Protocol Overview#Discovery]].
* `<public>` is the string "true" or "false".  If it is set to "true", then Bob MAY share Alice's post with others.  If the string is "false", then Bob SHOULD NOT share the post with others. Alice may choose to share the message with multiple people, but not make it totally public.  If she wants to do this, she must encode the message as a _separate_ salmon slap for _each_ of the intended recipients and send it to each recipient according to the methods described in [[Diaspora's federation protocol|Federation Protocol Overview]].  Note that if Alice is sending the message to Bob and Diego, she MUST send separate salmon slaps for each recipient, even if Bob and Diego live on the same pod.  This is because the slap MUST be encrypted separately for each recipient (encrypted according to the rules in [[Diaspora's federation protocol|Federation Protocol Overview]]).

### Public status updates

If Alice wants her post to be public, she MAY choose to share her post on other systems or services.  For example, the reference implementation allows Alice to mark a post as public.  The post is then be available to the world on an [[ActivityStream|Federation Protocol Overview#ActivityStream of public posts]].

However, even if the post is made public, Alice MUST encrypt the salmon slaps that she sends to the specifically-intended recipients.

## Comments on status updates

If Bob sends a status message to Alice and Eve, and Alice comments on the message, then Alice MUST construct a comment message and send it to Bob.  (As noted [[above|#De-duplication]], Alice's pod MAY immediately make the post visible to Eve, if Eve lives on the same pod as Alice, in the situation that Eve's pod knows that Eve should see the post, even if this knowledge is not available to Alice.  However, this is not required behavior.)

A comment message looks like this:

```xml
<XML>
  <post>
    <comment>
      <guid>((guid))</guid>
      <parent_guid>((guid))</parent_guid>
      <author_signature>((base64-encoded data))</author_signature>
      <text>I'll be there!</text>
      <diaspora_handle>alice@alice.diaspora.example.org</diaspora_handle>
    </comment>
  </post>
</XML>
```

The fields are thus:

* `<guid>` is the guid of the comment.  Each comment MUST be assigned a guid when the comment is created.  In this case, when Alice chooses to send the comment, the comment is assigned a guid, which is stored in the database.  A guid is a 16-character hexadecimal string.
* `<parent_guid>` is the guid of Bob's original post that Alice is commenting on.
* `<author_signature>` is Alice's signature of the comment.  To construct this signature:
    1. Identify the following fields:
        1. the guid of the comment
        2. the guid of the original post that this is a comment to
        3. the text of the comment
        4. the diaspora handle of the author of the comment.
    2. Concatenate those strings, with ";" delimiters.  So, the string might look like this:  `a965ddb72a3d5d61;d3d4b1320ca196cd;I'll be there!;alice@alice.diaspora.example.org`.  Note that the text of the comment may have `;`s in it; this is okay.  The base string is not a serialization that we intend to parse.  It is simply an arbitrary string that is constructible by the sender and the receiver of the message in a reliable way.
    3. Sign this string using Alice's RSA private key, and the 'SHA' signing algorithm. That's SHA-0, not SHA-1 or SHA256. SHA-0 may not be available on many systems. 
    4. base64-encode the signature.  This is the value of `<author_signature>`.
* `<text>` is the actual text of the comment that Alice wants to post.  Note that the text of this message must be made suitable for XML using any appropriate escaping scheme.  For example, unsafe characters like `<` and `>` could be translated into their corresponding XML entities like `&lt;` and `&gt;`.  Or the CDATA method could be used.  (See the [XML specification](http://www.w3.org/TR/2006/REC-xml11-20060816/#syntax)). This escaping is performed after the signature is generated.  
* `<diaspora_handle>` is the Diaspora handle of the author of the comment.  In this case, Alice's Diaspora handle, or `alice@alice.diaspora.example.org`.

This message is wrapped up in a salmon slap and sent to Bob, who wrote the original post that Alice is responding to.

### Relaying comments

When Bob receives Alice's comment on his post, it is his responsibility to relay Alice's comment to the other recipients of Bob's original post.  Bob SHOULD relay the message, unless he is performing moderation or rate-limiting.

In this example, Alice and Eve were the recipients of Bob's original post.  So, when Bob receives the salmon containing Alice's comment, he will prepare the relayed comment and send it to both Eve and Alice.  If Bob has chosen to relay the message, Bob MUST send it to all of the recipients, even Alice, the author of the comment.  In this way, Alice knows her comment has been received.  However, Alice's pod MAY choose to display the comment in its place, even before hearing back from Bob.

The message that Bob constructs is this:

```xml
<XML>
  <post>
    <comment>
      <guid>((guid))</guid>
      <parent_guid>((guid))</parent_guid>
      <parent_author_signature>((base64-encoded data))</parent_author_signature>
      <author_signature>((base64-encoded data))</author_signature>
      <text>I'll be there!</text>
      <diaspora_handle>alice@alice.diaspora.example.org</diaspora_handle>
    </comment>
  </post>
</XML>
```

All the fields here are identical to those that Bob received from Alice.  The only difference is the addition of the `<parent_author_signature>` field.  Bob signs the message before sending it out, to prove that it was indeed he who chose to relay the message, and not an imposter.

Bob constructs the `<parent_author_signature>` in the same way that Alice constucted her `<author_signature>`, except that he signs it with _his_ private key, not Alice's (which, of course, he does not have).

## "Like"s on status updates

If Bob sends a status message to Alice and Eve, and Alice "like"s the message, then Alice MUST construct a Like message and send it to Bob.  (As noted [[above|#De-duplication]], Alice's pod MAY immediately make the post visible to Eve, if Eve lives on the same pod as Alice, in the situation that Eve's pod knows that Eve should see the post, even if this knowledge is not available to Alice.  However, this is not required behavior.)

```xml
<XML>
  <post>
    <like>
      <target_type>Post</target_type>
      <guid>((guid))</guid>
      <parent_guid>((guid))</parent_guid>
      <author_signature>((base64-encoded data))</author_signature>
      <positive>true</positive>
      <diaspora_handle>alice@alice.diaspora.example.org</diaspora_handle>
    </like>
  </post>
</XML>
```

The fields are thus:

* `<guid>` is the guid of the Like.  Each comment MUST be assigned a guid when the comment is created.  The guid is chosen by Alice's pod.  In this case, when Alice chooses to send the Like, the Like is assigned a guid, which is stored in the database of Alice's pod.  A guid is a 16-character hexadecimal string.
* `<parent_guid>` is the guid of Bob's original post that Alice is Liking on.
* `<author_signature>` is Alice's signature of the Like.  To construct this signature:
    1. Identify the following fields:
        1. the guid of the Like
        2. The value of the `<target_type>` field.  In this case, the string "Post".
        3. the guid of the original post that this is in response to
        4. The _text_ of the `<positive>` field.  In this case, the string "true".
        5. the diaspora handle of the author of the comment.
    2. Concatenate those strings, with ";" delimiters.  So, the string might look like this:  `a965ddb72a3d5d61;Post;d3d4b1320ca196cd;true;alice@alice.diaspora.example.org`.
    3. Sign this string using Alice's RSA private key, and the SHA (e.g. SHA-0) signing algorithm.
    4. base64-encode the signature.  This is the value of `<author_signature>`.
* `<target_type>` is the string "Post" if the Like is liking a status message.  It is "Comment" if the Like is liking a comment (comment-liking is discussed later).
* `<positive>` is the string "true" if Alice is liking this post.  It is "false" if Alice is retracting a previous Like.
* `<diaspora_handle>` is the Diaspora handle of the author of the Like.  In this case, Alice's Diaspora handle, or `alice@alice.diaspora.example.org`.

This message is wrapped up in a salmon slap and sent to Bob, who wrote the original post that Alice is responding to.

Alice MUST NOT send a Like for a post if she has already successfully sent a Like for that post.  That is, she can only Like a post once.

If Bob receives a Like from Alice, and he had already received a Like from Alice for that same post, then Bob MUST ignore this second Like.

### Relaying Likes

When Bob receives Alice's Like of his post, it is his responsibility to relay Alice's Like to the other recipients of Bob's original post.  Bob SHOULD relay the message, unless he is performing moderation or rate-limiting.

In this example, Alice and Eve were the recipients of Bob's original post.  So, when Bob receives the salmon containing Alice's Like, he will prepare the relayed comment and send it to both Eve and Alice.  If Bob has chosen to relay the message, Bob MUST send it to all of the recipients, even Alice, the author of the Like.  In this way, Alice knows her Like has been received.  However, Alice's pod MAY choose to display the Like in its place, even before hearing back from Bob.

Note, though, that Bob should accept at most one Like from each person, for each status message.  Bob should ignore all negative Likes from Alice if he has not received a positive Like from Alice.  Bob should ignore all positive Likes from Alice if he has already received a positive Like from her.

If Bob accepts Alice's like, the message that he constructs is this:

```xml
<XML>
  <post>
    <like>
      <guid>((guid))</guid>
      <target_type>Post</target_type>
      <parent_guid>((guid))</parent_guid>
      <parent_author_signature>((base64-encoded data))</parent_author_signature>
      <author_signature>((base64-encoded data))</author_signature>
      <positive>true</positive>
      <diaspora_handle>alice@alice.diaspora.example.org</diaspora_handle>
    </like>
  </post>
</XML>
```

All the fields here are identical to those that Bob received from Alice.  The only difference is the addition of the `<parent_author_signature>` field.  Bob signs the message before sending it out, to prove that it was indeed he who chose to relay the message, and not an imposter.

Bob constructs the `<parent_author_signature>` in the same way that Alice constucted her `<parent_signature>`, except that he signs it with _his_ private key, not Alice's (which, of course, he does not have).

### Negative likes

If Alice has previously sent a Like to Bob, regarding one of Bob's posts, and then Alice decides to retract that Like, she sends a negative Like to Bob, who should relay it.  Both Alice's negative like, and Bob's relaying of it, is constructed in the same way described above for Likes.  The difference is that, in a negative Like, the `<positive>` field contains the string "false".

Alice MUST NOT send a negative Like for a post that she has not previously Liked (successfully -- where "success" means that Alice has received Bob's relay of the Like).  Alice MUST NOT send a Like for a message that she has already Liked successfully.

If Bob receives a negative Like for a message that Alice had not previously Liked, then Bob MUST ignore this message.