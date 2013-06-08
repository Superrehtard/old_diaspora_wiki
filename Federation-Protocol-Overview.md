The purpose of this document is to describe the communications that go on between Diaspora servers.  Implementers of this protocol should be advised, though, that Diaspora is in Alpha, and as such, this document is not authoritative, and may lag behind the reference implementation.

# Asymmetric Sharing

One important thing to note is that Diaspora's notion of "sharing" is asymmetric.

In a symmetric "friending" relationship, neither party sees any posts from the other until one party makes a friend request and the other confirms.

In an asymmetric "sharing" relationship, if you start sharing with someone else, you have decided to send them posts.  They may or may not choose to share with you as well.  If they choose not to, you will see only their posts that they have explicitly marked as public.

If Alice starts sharing with Bob and Bob is sharing with Alice, then Alice will see new posts by Bob when Bob sends Alice posts using Salmon slaps.  However, if Alice is sharing with Bob and Bob is not sharing with Alice, Bob will not send salmon slaps to Alice.  If she wants Bob's public posts, she must retrieve them via another method, such as [ActivityStreams](http://activitystrea.ms), which MAY be provided by Bob's pod.  (See [this note on public status updates](https://github.com/diaspora/diaspora/wiki/Federation-Message-Semantics#public-status-updates)).

# Core Diaspora Protocols

Diaspora servers communicate with one another in a variety of situations:

* When discovering information about users on another server.
* When sending information to people that you're sharing with.  That information includes:
    * Notification that you've begun sharing with them.
    * Posts that you've made.
    * Comments that have been made (by you or others) on one of your posts.
    * "Like"s that have been made (by you or others) on one of your posts.
    * Conversations (each thread in the inbox has an object representing it)
    * Messages (each individual message in a Conversation)
    * Profile information
    * Retractions of posts
    * Retractions of likes/comments

This document does not cover the semantics of each of the messages listed above.  For a discussion of these semantics, see [[Diaspora's Message Semantics|Federation Message Semantics]].

## Discovery

Diaspora pods MUST be able to discover users on other pods, given the other user's webfinger address.  For convenience, Diaspora pods' user-interfaces MAY choose to allow users to search for users by name, searching through the list of names already known to the pod (such as local users).  However, pods' user interfaces MAY NOT allow users to find a person by name if that person has not marked themselves as "searchable", in their hcard (see below).

If alice@alice.diaspora.example.com wants to discover bob@bob.diaspora.example.com, then alice's pod must perform a Webfinger lookup of bob's address.  Webfinger is an open protocol.  See [the Webfinger protocol specification](http://code.google.com/p/webfinger/wiki/WebFingerProtocol) for the full details.  However, we will summarize here.  Note that bob's webfinger profile does not need to be hosted by bob's diaspora pod.  Any webfinger server will do, so long as bob's profile contains the elements necessary for Diaspora.  See below.  However, Diaspora pods SHOULD host webfinger profiles for their users.

Alice's pod will first get the host-meta file from bob's webfinger address.  The host-meta file is located at https://bob.diaspora.example.com/.well-known/host-meta.  In the host-meta file, alice's pod will find a Link element with `rel="lrdd"`, such as:

```xml
<Link rel="lrdd" template="https://bob.diaspora.example.com/webfinger?q={uri}"  type="application/xrd+xml" />
```

Alice's pod will now transform bob's webfinger address and replace {uri} with that.  First, bob's webfinger address is url-encoded.  Alice will make a GET request to:
    https://bob.diaspora.example.com/webfinger?q=bob%40bob.diaspora.example.com

Bob's webfinger server (which may be the same as bob's pod) will respond with bob's webfinger profile.  The webfinger profile might look something like this:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<XRD xmlns="http://docs.oasis-open.org/ns/xri/xrd-1.0">
  <Subject>acct:bob@bob.diaspora.example.com</Subject>
  <Alias>"http://bob.diaspora.example.com/"</Alias>
  <Link rel="http://microformats.org/profile/hcard" type="text/html" href="http://bob.diaspora.example.com/u/((guid))"/>
  <Link rel="http://joindiaspora.com/seed_location" type="text/html" href="http://bob.diaspora.example.com/"/>
  <Link rel="http://joindiaspora.com/guid" type="text/html" href="((guid))"/>
  <Link rel="http://schemas.google.com/g/2010#updates-from" type="application/atom+xml" href="http://bob.diaspora.example.com/public/bob.atom"/>
  <Link rel='http://webfinger.net/rel/profile-page' type='text/html' href="https://bob.diaspora.example.com/u/((bobs-username))"/>
  <Link rel="diaspora-public-key" type="RSA" href="((base64-encoded representation of the rsa public key))"/>
</XRD>
```

Let's look at these elements in more depth.

### Subject

```xml
<Subject>acct:bob@bob.diaspora.example.com</Subject>
```

The Subject element should contain the webfinger address that alice asked for.  If it does not, then this webfinger profile MUST be ignored by alice's pod.

### Alias

```xml
<Alias>"http://bob.diaspora.example.com/"</Alias>
```

(this seems to be the pod location, but with quotation marks around it.  Why?)

### hcard

```xml
<Link rel="http://microformats.org/profile/hcard" type="text/html" href="http://bob.diaspora.example.com/u/((guid))"/>
```

Bob's webfinger profile MUST contain a link to an hcard.  The hcard contains personal information such as bob's full name, a link to bob's photo, etc.  Refer to the [hcard specification](http://microformats.org/profile/hcard hcard specification) for a full discussion on hcard syntax.

Like the webfinger profile, the hcard need not be hosted by the Diaspora pod.  In fact, it may be in a location that is distinct from both the Diaspora pod and the webfinger host.  In the reference implementation, the hcard url is:
    http://bob.diaspora.example.com/u/((bobs-guid))

When a user creates an account on a pod, the pod MUST assign them a guid -- a random hexadecimal string of at least 8 hexadecimal digits.  The Diaspora pod SHOULD use that guid to create an hcard url, and SHOULD host users' information as an hcard at this location.

Diaspora adds a field, entity_searchable, which contains an element with class "searchable", which is either "true" or "false".  If the value is false, then pods MAY NOT allow users to search for bob by name, or through any method other than directly entering bob's webfinger address.

Diaspora also includes a url field with the id of "pod_location", which links to bob's diaspora pod.

Here is an example of an hcard.

```html
<div id="content">
<h1>Bob Exampleman</h1>
<div id="content_inner">
<div class="entity_profile vcard author" id="i">
<h2>User profile</h2>
<dl class="entity_nickname">
<dt>Nickname</dt>
<dd>
<a class="nickname url uid" href="http://bob.diaspora.example.com/" rel="me">Bob Exampleman</a>
</dd>
</dl>
<dl class="entity_given_name">
<dt>First name</dt>
<dd>
<span class="given_name">Bob</span>
</dd>
</dl>
<dl class="entity_family_name">
<dt>Family name</dt>
<dd>
<span class="family_name">Exampleman</span>
</dd>
</dl>
<dl class="entity_fn">
<dt>Full name</dt>
<dd>
<span class="fn">Bob Exampleman</span>
</dd>
</dl>
<dl class="entity_url">
<dt>URL</dt>
<dd>
<a class="url" href="http://bob.diaspora.example.com/" id="pod_location" rel="me">http://bob.diaspora.example.com/</a>
</dd>
</dl>
<dl class="entity_photo">
<dt>Photo</dt>
<dd>
<img class="photo avatar" height="300px" src="http://bob.diaspora.example.com/uploads/images/thumb_large_sTBJOBJScE.jpg" width="300px">
</dd>
</dl>
<dl class="entity_photo_medium">
<dt>Photo</dt>
<dd> 
<img class="photo avatar" height="100px" src="http://bob.diaspora.example.com/uploads/images/thumb_medium_sTBJOBJScE.jpg" width="100px">
</dd>
</dl>
<dl class="entity_photo_small">
<dt>Photo</dt>
<dd>
<img class="photo avatar" height="50px" src="http://bob.diaspora.example.com/uploads/images/thumb_small_sTBJOBJScE.jpg" width="50px">
</dd>
</dl>
<dl class="entity_searchable">
<dt>Searchable</dt>
<dd>
<span class="searchable">true</span>
</dd>
</dl>
</div>
</div>
</div>
```

### Seed Location

```xml
<Link rel="http://joindiaspora.com/seed_location" type="text/html" href="http://bob.diaspora.example.com/"/>
```

The "seed_location" is a link to bob's pod.

### guid

```xml
<Link rel="http://joindiaspora.com/guid" type="text/html" href="((guid))"/>
```

This is just bob's guid.  When a user creates an account on a pod, the pod MUST assign them a guid -- a random hexadecimal string of at least 8 hexadecimal digits.

### Activity Stream URL

```xml
<Link rel="http://schemas.google.com/g/2010#updates-from" type="application/atom+xml" href="http://bob.diaspora.example.com/public/bob.atom"/>
```

This atom feed is an Activity Stream of bob's public posts.  Diaspora pods SHOULD publish an Activity Stream of public posts, but there is currently no requirement to be able to read Activity Streams.  For more information, read the [Activity Streams specification](http://activitystrea.ms/)

Note that this feed MAY also be made available through the [PubSubHubbub mechanism](http://code.google.com/p/pubsubhubbub/) by supplying a `<link rel="hub">` in the atom feed itself.

### Diaspora Public Key

```xml
<Link rel="diaspora-public-key" type="RSA" href="((base64-encoded representation of the rsa public key))"/>
```

When a user is created on the pod, the pod MUST generate a pgp keypair for them.  This key is used for signing messages.  Here we place the key in the "href".  The format of the key is this:  We take the ascii-armored representation of the key, and base64-encode THAT.  Thus, the actual binary key is "double-wrapped" in base64-encoding. Removing the outer wrapper provides a familiar DER-encoded PKCS#1 key beginning with the text "----BEGIN RSA PUBLIC KEY----". Some platforms may require conversion of this key to a different format, such as PKCS#8 or "modulus/exponent". 

## Sending

If you (Alice) have decided to share with a user (Bob) on another pod, then you will need to send posts as salmon to the remote user.

You have three tasks:

1. Construct your message.
2. Construct the url for Bob's salmon endpoint.
3. Post the message to Bob.

In this example, Alice Exampleman (alice@alice.example.com) is attempting to send a message to Bob Exampleman (bob@bob.example.com).

### Constructing the message

In Diaspora, messages are sent to remote users encrypted.  This helps protect the privacy of your messages while they are in transit, even if you post the salmon to Bob's pod using regular HTTP instead of SSL-encrypted HTTP.  (Note that messages are only guaranteed to be encrypted in transit.  They MAY be decrypted by Bob's server and stored in cleartext).

To support the encryption semantics, Diaspora actually extends the Salmon magic envelopes protocol to add an encryption header.

So, in order to construct the full salmon slap, you will need to:

1. Construct the encryption header.
2. Prepare the payload message.
3. Construct a Diaspora salmon magic-envelope.

#### Constructing the encryption header

Choose an AES key and initialization vector, suitable for the aes-256-cbc cipher.  I shall refer to this as the "inner key" and the "inner initialization vector (iv)".

Construct the following XML snippet:

```xml
<decrypted_header>
  <iv>((base64-encoded inner iv))</iv>
  <aes_key>((base64-encoded inner key))</aes_key>
  <author_id>alice@alice.example.com</author_id>
</decrypted_header>
```

Construct _another_ AES key and initialization vector suitable for the aes-256-cbc cipher.  I shall refer to this as the "outer key" and the "outer initialization vector (iv)".

Encrypt your `<decrypted_header>` XML snippet using the "outer key" and "outer iv" (using the aes-256-cbc cipher).  This encrypted blob shall be referred to as "the ciphertext".

Construct the following JSON object, which shall be referred to as "the outer aes key bundle":

```javascript
{
  "iv": ((base64-encoded AES outer iv)),
  "key": ((base64-encoded AES outer key))
}
```

Encrypt the "outer aes key bundle" with Bob's RSA public key.  I shall refer to this as the "encrypted outer aes key bundle".

Construct the following JSON object, which I shall refer to as the "encrypted header json object":

```javascript
{
  "aes_key": ((base64-encoded encrypted outer aes key bundle)),
  "ciphertext": ((base64-encoded ciphertextm from above))
}
```

Construct the xml snippet:

```xml
<encrypted_header>((base64-encoded encrypted header json object))</encrypted_header>
```

Save the encrypted_header snippet for later; it will be added to the salmon envelope to construct the Diaspora-extended salmon.

Remember the "inner aes key" and "inner aes iv" for later.  You will use this to encrypt your payload message, which we will owever, as stated above, the things you will post to remote users are:

* Notification that you've begun sharing with them.
* Posts that you've made.
* Comments that have been made (by you or others) on one of your posts.
* "Like"s that have been made (by you or others) on one of your posts.
* Conversations (each thread in the inbox has an object representing it)
* Messages (each individual message in a Conversation)
* Profile information
* Retractions of posts
* Retractions of likes/comments

Whatever the contents of the payload message, they will be in this format:

```xml
<XML>
  <post>((post-content))</post>
</XML>
```

The post-content depends on what type of message you are sending.  In general, the API is in flux, so this document may be out of date.  The authoritative source for this information is the "models" of the reference ruby-on-rails implementation.

In order to prepare the payload message for inclusion in your salmon slap, you will:

* Encrypt the payload message using the aes-256-cbc cipher and the "inner encryption key" and "inner encryption iv" you chose earlier.
* Base64-encode the encrypted payload message.

#### Construct a salmon magic envelope

By now, you have gathered the following components:

* The encryption header.
* The prepared payload message.

You must now construct the salmon magic envelope that we will post to Bob, the recipient.  It is constructed thusly:

```xml
<?xml version='1.0' encoding='UTF-8'?>
<entry xmlns='http://www.w3.org/2005/Atom'>
  ((the encryption header))
  <me:env xmlns:me="http://salmon-protocol.org/ns/magic-env">
    <me:encoding>base64url</me:encoding>
    <me:alg>RSA-SHA256</me:alg>
    <me:data type="application/xml">((base64url-encoded prepared payload message))</me:data>
    <me:sig>((the RSA-SHA256 signature of the above data))</me:sig>
  </me:env>
</entry>
```

Note that the last step in the preparation of the payload message was to base64-encode it.  That string must be base64-encoded again to form the `<me:data>` element.  However, this time, it must be encoded with the slightly-different base64url encoding.  So your payload message will end up double-wrapped in base64-encoding.

The signature (`<me:sig>` element) is constructed as specified in the [Magic Envelopes specification](http://salmon-protocol.googlecode.com/svn/trunk/draft-panzer-magicsig-01.html).  That is, use the RSA-SHA256 algorithm to sign the base string with your (Alice's) private RSA key.  

To construct the base string, concatenate the following elements, separated by periods (.).

1. The contents of the `<me:data>` field.  That is the base64url-encoded prepared payload message (remember, the original payload message has now been base64-encoded twice.  Once with regular base64, and once with base64url). _Note: In previous versions this was then required to be re-padded with linefeeds.  This should no longer be done.  This is now the actual contents of the `<me:data>` field.  As-is._
2. The base64url-encoding of the "data-type" parameter.  In this case, 'application/atom' is base64-encoded.  Thus, the base64url-encoded string is `YXBwbGljYXRpb24veG1s` _Note: Linefeed character should no longer be included_
3. The base64url-encoding of the "encoding" parameter, which is the literal string `base64url`, base64-encoded.  Thus, the base64url-encoded string is `YmFzZTY0dXJs` _Note: Linefeed character should no longer be included_
4. The base64url-encoding of the "alg" parameter, which is the literal string `RSA-SHA256`, base64-encoded.  Thus, the base64url-encoded string is `UlNBLVNIQTI1Ng==` _Note: Linefeed character should no longer be included_

Sign the base string with your (Alice's) private RSA key and base64url-encode the results.

This is the final form of the salmon slap, ready for delivery.

### Construct the URL of Bob's Salmon endpoint

To construct the url of the salmon endpoint, do the following:

1. Get the pod location of the remote user, Bob.
2. Get the guid of the remote user (using the webfinger process described above).
3. Construct `<pod_url>/receive/users/<guid>`.  This Bob's salmon endpoint.

### Post the message to Bob

Take your final salmon slap, double urlencode it, and POST this data to Bob's salmon endpoint as Content-type: application/x-www-form-urlencoded :

    xml=((double urlencoded salmon slap))

If you receive an HTTP `202 Created`, or a `200 OK`, your salmon slap has been accepted.

Note that this differs from the standard Salmon protocol, which specifies that you will post only the non-urlencoded salmon slap, without an `xml=...`

## Receiving

Consider the case in which you are Bob, receiving a salmon slap from Alice.  In general, you should be able to follow the steps outlined in the "Sending" section, in reverse.  Verify the slap, as specified in [Section 8 of the Salmon specification](http://salmon-protocol.googlecode.com/svn/trunk/draft-panzer-salmon-00.html#SVR), and return `202 Created` if this is a new salmon, or `200 OK` if this salmon updates a previous one. If the slap fails verification, return `400 Bad Request`.

Note that the slap sent to Bob is signed with Alice's private key.  However, nothing about the author of a slap is sent in cleartext.  Therefore, you will have to decrypt and decode the payload message in order to find the information about who the message is from.  The payload message will contain the diaspora handle of the author.  Get their public key from their webfinger protocol.  This is the key that you will use to verify the signature.

When verifying the signature, note that the reference implementation of Diaspora uses the Ruby OpenSSL library, which generates RSA keys in the PKCS#1 format.  Another popular key format is the new PKCS#8 format.  Some tools may not interoperate.  For example, the openssl command-line tool has options to verify RSA signatures, but it can only read keys in the PKCS#8 format.  To complicate matters, there is no header information that says which of these formats the key is written in, so the openssl command-line tool cannot return a more informative error than "unable to load Public Key".

For more information on this problem, see [this blog post](http://barelyenough.org/blog/2008/04/fun-with-public-keys/).

# Additional Diaspora Protocols

Diaspora pods MAY offer federation through other protocols as well.  The reference implementation offers the following additional protocols:

* An ActivityStream of public posts.
* An API that allows third-party applications to use your pod's data on behalf of your users, using OAuth authentication ([the OAuth protocol](http://tools.ietf.org/html/rfc5849)).

## ActivityStream of public posts

The reference implementaion of Diaspora exposes a UI that allows users to mark posts as "public".  If Alice makes a post that is not marked as public, the post will be sent only to those people that Alice is sharing with.  However, if Alice makes a post that is marked public, it will also be sent to those people that are sharing with Alice, even if Alice is not sharing with them.

In addition, the posts are added to an ActivityStream.  The address of this feed is published in the user's hcard, with:

```xml
<Link rel="http://schemas.google.com/g/2010#updates-from" type="application/atom+xml" href="https://joindiaspora.com/public/((username)).atom"/>
```

The feed SHOULD also be made available on a PubSubHubbub server.

See the [ActivityStrems specification](http://activitystrea.ms/) and the [PubSubHubbub specification](http://code.google.com/p/pubsubhubbub/).

## Third-party Application API

The reference implementation of Diaspora offers an API for third-party application developers.  It allows third-party applications to use your pod's data on behalf of your users.  Access is controlled by the [the OAuth protocol](http://tools.ietf.org/html/rfc5849).

Right now, there is only one application that uses this API.  [Cubbi.es](http://cubbi.es).  The API is still very much in flux, so people are not being encouraged to write new applications until the protocol has been solidified a little more.


