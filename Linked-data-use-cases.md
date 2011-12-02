Use cases for [[Linked Data]] in Diaspora.
## Identity aggregator
Say you have an account (an identity) at identi.ca, Gmail and joindiaspora.com. In each of these communities, you have a different set of contacts, but each of these sets of contacts are equally important to you. When you use other websites, you don't want to identify as just one of these identities, with their associated contacts. You want to identify as _all of them_ - you want to identify as you.

An identity aggregator is a web application which creates a single FOAF document linking to all the other FOAF documents you may have around the web. When you want to identify yourself on another website, you can simply link to this one FOAF document. If the website is well-versed in FOAF, it will understand how all the identities listed in your aggregated FOAF document relates to you.

For an example implementation of this use case, see [Foafpress](http://foafpress.org/).