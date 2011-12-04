Use cases for [[Linked Data]] in Diaspora.
## Identity aggregator
Tom has an account (an identity) at identi.ca, Gmail and joindiaspora.com. In each of these communities, Tom has a different set of contacts, but each of these sets of contacts are equally important to him. When he uses other websites, he doesn't want to identify as just one of these identities. He wants to identify as _all of them_ - you want to identify as _him_.

An identity aggregator is a web application which creates a single FOAF document linking to all the other FOAF documents Tom may have around the web. When Tom wants to identify himself on another website, he can simply link to this one FOAF document. If the website is well-versed in FOAF, it will understand how all the identities listed in his aggregated FOAF document relate to him.

For an example implementation of this use case, see [Foafpress](http://foafpress.org/).

## Music sharing
Bob has just acquired an awesome new track from his indy musician friend. He plays it in his desktop music player, which also identifies the track on [MusicBrainz](http://musicbrainz.org/) and posts a message to his Diaspora account with title and artist: "Bob is now playing Awesome Track by Indy Musician". In addition to the human-readable message, the post also contains an embedded link to the track on MusicBrainz.

John think Bob has a great taste in music, so he has added Bob's Diaspora account to the music player on his mobile phone. While John is out running, listening to music on his mobile phone, the music player receives the update from Bob's account. It sees that the post contains an embedded link to a document containing title and artist information of a track, as well as an download URL for it. It automatically downloads the track and adds it to John's play queue.

A few minutes later, the awesome new track, which John has never heard before, seamlessly begins playing after the previous track. When he looks at the phone's display to check out what's up with the great sound, title and artist information is properly shown.