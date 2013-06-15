----

###403 DO NOT MOVE###

We're currently **moving this wiki over to our new project site**. The contents of this page are not important enough to be ported over because they are either very old, very outdated or wrong and misleading. 

----


Use cases for [[Linked Data]] in Diaspora.

## Music sharing
Bob has just acquired an awesome new track from his indy musician friend, Indy. He plays it in his desktop music player, which identifies the track on [MusicBrainz](http://musicbrainz.org/) and posts a message to his Diaspora account with title and artist: "Bob is now playing Awesome Track by Indy Musician". In addition to the human-readable message, the post also contains an embedded link to the track on MusicBrainz.

John thinks Bob has a great taste in music, so he has added Bob's Diaspora account as a friend to the music player on his mobile phone. While John is out running, listening to music on his mobile phone, the music player receives the update from Bob's account. It sees that the post contains an embedded link to a document containing title and artist information of a track, as well as an download URL for it. The music player automatically downloads the track and adds it to John's play queue.

A few minutes later, the awesome new track, which John has never heard before, seamlessly begins playing after the previous track in his queue. When he looks at the phone's display to check out what's up with the great sound pouring into his ears, title and artist information is properly shown.

## Location sharing
Dex wants his best friends to know where he is _at all times_. He sets up a location-aware application on his phone to post location updates to his "Bros" aspect on Diaspora.

When the mobile application detects significant changes in Dex's location, it obtains a plain text name of the general area (e.g. city name) from a web service and posts a message to Diaspora: "Dex is now in Carville, LA." The message also contains an embedded representation of the location in the [Geo](http://www.w3.org/2003/01/geo/) vocabulary.

Ella has a similar application on her phone, which is set up to follow Dex's updates. The application receives the update from Dex and parses the embedded location data. Since Ella is also in Carville, it plays a sound to notify her that Dex is nearby.

## Identity aggregator
Tom has an account (an identity) at identi.ca, Gmail and joindiaspora.com. In each of these communities, Tom has a different set of contacts, but each of these sets of contacts are equally important to him. When he uses other websites, he doesn't want to identify as just one of these identities. He wants to identify as _all of them_ - he wants to identify as _him_.

An identity aggregator is a web application which creates a single [FOAF](http://en.wikipedia.org/wiki/FOAF_%28software%29) document linking to all the other FOAF documents Tom may have around the web. When Tom wants to identify himself on another website, he can simply link to this one FOAF document. If the website is well-versed in FOAF, it will understand how all the identities listed in his aggregated FOAF document relate to him.

For an example implementation of this use case, see [Foafpress](http://foafpress.org/).