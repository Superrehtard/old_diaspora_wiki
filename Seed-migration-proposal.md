# Seed migration proposal

## UX

### On the old pod 

* User requests his backup/data/move/seed archive (however you call it)
* User is forced to enter a password to protect it

### On the new pod

* User signs up at new pod (or has an invite from it) and is offered two options
    * create a new account
    * import existing account
* User chooses to import an existing account and is asked to upload his archive
* Archive may be be invalid -> abort or retry upload?
* User is asked to enter the password for his archive
* Username ability check is made, if old username is already taken, he's asked for a new one.
* User is asked for a new password
* User is informed that the move is in progress and will be notified once it's done
* User tries to sign in while the move isn't finished and gets same message again
* User gets email about
    * successful move
    * a failed move because the move was already done
    * a failed move because $otherThingsThatCanGoWrong
* User is able to sign in at the new pod at nothing except the ID changed.

## Implementation

### The archive

* The archive consists of two files:
    * Another password protected archive containing the actual data
    * A hash of the the other archive to verify it's integrity
* The actual data archive contains
    * The private and public key pair
    * A list of contacts
        * A list of followers
        * A list of blocked users
        * A list of followings
    * The profile
    * The settings
    * All posts with all comments and likes
    * Tag followings
    * The last 30 notifications (?)
    * The users pictures. This requires some thought as this could blow the package up quite enormously. 
      Choices on export:
        * keep pictures on old pod (pod maintainer can disable that feature)
        * download them (give the approx filesize of the resulting download, and the remind the user that he needs to upload that too).
        * Give out a download link that the user can enter on the new pod.
        * throw them away
    * All conversations

### Happenings on import

* After the upload is successful queue a job for it, everything below happens in this job
* Generate a new keypair
* Generate move notices for all contacts
    * include the new public key and sign them with the old private key
    * A backchannel is needed if the move notice was accepted, if it's not accepted by $acception_rate abort and notify the user
* Generate a revocation for the old pod and send it, backchannel is required here too to confirm deletion on old pod, if it fails do not abort but inform the user to close his old account manually.
* Import other data
* Send success mail


## Open questions

* What to do with pending invites?
* What's a good value for $acception_rate? Should it already fail by one explicit reject (so timeouts do not count)?
* What if the user has to close his account manually? Refederate posts? Probably not since all contacts already wouldn't accept the retractions because of the new public key. (?)
