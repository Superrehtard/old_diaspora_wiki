# Diaspora Security Architecture Proposal
# Preamble
This proposal was written by braedon, and edited by sofaer, as an initial laying out of Diaspora's security requirements.

The architecture defines who should have what data, in what form. Most implementation details, such as exact encryption and authentication methods, are left open.

# Design Philosophy
It is clear from the discussions that some advanced users want end to end encryption, as they do not want to trust their seeds, nor the seeds of their friends. It is also clear, however, that barring some revolutionary design as yet unthought-of (within the Diaspora community at least) such a system would be prohibitively complex and cumbersome for the average user. In fact, the more secure a system, the less user friendly, and the more expensive (network and CPU load, data storage), it becomes.

Thus this architecture was designed around the philosophy of 'Secure as much as you must, but no more.' It recognises that users require different security for different sets of data, and that different users will have different security capabilities.

# Definitions
* Pod - Hosts a number of Seeds
* Seed - The Diaspora account of one user
* Client - an interface into a Seed used by users to post and view Data
* Local Client - a Client under exclusive control of the User
* Remote Client - a Client accessed remotely and not under exclusive control of the User
* User - a person with a Diaspora account
* Owner - the User with control of a given Post
* Aspect - a group of Users as defined by a given User
* Audience - the group of Aspects (and the union of their User groups) the Owner intends a Post for
* Data - anything shared by Users via Diaspora
* Post - A group of Data inserted into Diaspora collectively
* Comment - A type of Post attached to an existing Post
* Encrypt(ed) - In the proposal below, encryption of a post should be taken to mean the encryption of that post with a symmetric key, and the encryption of that symmetric key for the post's recipients. 

# The Architecture
## A Simple Diagram
[[http://resisty.com/upload/diasporaProposal.png]]

## Major Components
### Web Client
The web client is a HTTP interface built into pods to allow users to easily connect to their seeds from anywhere. It is an example of a Remote Client. This is the interface used by the majority of users the majority of the time. Connected to the user's seed via the Client API (or possibly directly as another part of the same server).

### Secure Client
An alternative to the web client for advanced users. It is an example of a Local Client. Communicates to the user's seed via the Client API. Allows for more secure communication.

### Seed
A user's Diaspora account. Data is pushed to it from clients (such as the web client) via the Client API. It stores/retrieves data in/from the Pod's database. Seeds communicate with other seeds via the Seed API to notify of and exchange data.

## Security Model
Security is defined at 3 different levels. None, Low, and High.

* Each User supports a maximum security level, defined by the combination of Pod, Seed, and Client they use. 
* Each Aspect has a maximum security level, defined by the lowest maximum security level of the Users it contains. 
* Each Post has a maximum security level, defined by the lowest maximum security level of the Aspects in its Audience. 
* Each Post also has an actual security level, between None and its maximum security level, as selected by its Owner.

### None
Posts with the None security level are not encrypted and available to anyone. They can be posted from any Client, stored by Seeds without encryption, and likewise communicated between Seeds without encryption. Any Seed can request the Post from its Owner's Seed.

### Low
A Low security Post is encrypted for consumption by that Post's audience at the Seed level.

The Seed provides Audience Seeds with the Encrypted Post on request. Posts and their keys can be stored in Owner/Audience Seeds/Pods unencrypted or encrypted as required by computation/storage limitations.

Note that this level can potentially be simplified into client(in this case Audience Seeds) authenticating SSL.

### High
A High security Post is Encrypted for consumption by that Post's audience on a client local to the User.

Each User has a key pair for use in this level. The private key of this pair is kept secure by the User, on his Secure Client. A Post can be made from any Client that has been given the private key. 

The Client Encrypts the Post for all Audience Users and the Owner upon posting. The Encrypted Post is sent to the Seed for storage and delivery to Audience members on request.  Posts are decrypted by Secure Clients when viewed by Users.


## Communication Semantics
### User to Remote Client
Communication must be encrypted. Dual authentication is required. Users authenticate via a login name and password.

Note - The obvious candidate connection encryption and client authentication for the default web client would be HTTPS.

### Client to Seed
Communication must be encrypted for Low security Posts. Dual authentication(User and Seed) is required.

Note - Signatures could be used to authenticate Owners on a Post basis.

### Seed to Seed
Communication can be unencrypted(secure Posts are encrypted anyway). Owner authentication required.

Note - Once again, signatures could be used to authenticate Owners of Posts.

Note - Audience authentication may be required to reduce the load of serving requests from non Audience Seeds.

## Other Issues
### Post Federation
Notifications are sent to Audience Seeds when a new Post is made. Audience Seeds must request the Post and any encryption key from the Owner Seed.

### Post Caching
Seeds/Clients can cache any Post in almost any form they have access to. The only restriction is for Clients and Medium Security posts. Just as the unlocked Medium level private key should never be stored outside of RAM by Clients, Clients should never cache unencrypted Medium level Posts or keys.

### Comment Handling
Comments are owned and stored by the original Post Owner, not the Comment author. As such, the Audience and security level of any Comment is defined by its Post.

### Revocation
When a User is removed from the Audience of a Post (or set of Posts) the corresponding encrypted key(s) are removed from the Owner's Seed, and thus not served if the revoked User requests them.

# Discussion
## Comments
Comment threads don't make sense unless everyone reading them can see the original Post, and all Comments. The above handling of Comments seems to be the only logical way to ensure this.

However, this has the ramification that any change in Audience of a Post would also change the Audience of any Comments by other Users. This would suggest that Audience changes should not be allowed, but a easy method for enforcing this is not evident.

## Audience Management
Post/key sources other than the Owner Seed are a possibility to help increase uptime. However, management of Audiences for Posts may be a difficult issue in such a situation. A capability based Audience model may be more suitable for managing who is served what Data.