**Problem:** The Pod Bubble Effect.  Currently, a public post is only federated to a remote pod if there is a contact relationship between users on the two pods.  If there is no such relationship, the public post remains on the originating pod -- it's trapped in a bubble.

**Proposed Solution:** Federation Subscription.

Every pod will have a special account, a "public subscription" account.  Physically speaking, it is a normal account like any other.  For our purposes, though, no regular user would log into this account or use it (though the admin may).  It could have any username, but for consistency and fittingness, it might be named "publicsubscription12345", where 12345 is some randomly generated number.  (Or maybe no number at all.)

By some mechanism and agreement, a micropod subscribes to a megapod.  The exact mechanism is yet to be finalized, but MVP will likely be: the broadcasting megapod adds publicsubscription@micropod.net somewhere in the YAML config.

Then, whenever public posts are broadcast, the publicsubscription accounts are appended to the array of remote recipients.  See [the postzord dispatcher code](https://github.com/diaspora/diaspora/blob/14e5d9b1c74ec8f36e5f1b9f2fd2c0f56fa1fcb1/lib/postzord/dispatcher/public.rb#L15).

Not much else needs to change, the system will just queue up the public message as usual, and the message processing code should just continue to work as it does, except we get the new, added benefit of posts federating out to other pods without needing real humans to make cross-pod contact connections.

Because it is a subscription system, a pod cannot be forced to transmit.