Here's a page for discussing how [Linked Data](http://en.wikipedia.org/wiki/Linked_Data) might be used in Diaspora.

Diaspora might embed [RDFa](http://en.wikipedia.org/wiki/RDFa) metadata on relevant pages.
### Profile pages
User profile pages might expose [FOAF](http://en.wikipedia.org/wiki/FOAF_%28software%29) data.

* The user may be represented as a [foaf:Person](http://xmlns.com/foaf/spec/#term_Person).
* The user's full name may be represented with the [foaf:name](http://xmlns.com/foaf/spec/#term_name) property.
* The user's gender may be represented with the [foaf:gender](http://xmlns.com/foaf/spec/#term_gender) property.
* The user's birthday may be represented with the [foaf:birthday](http://xmlns.com/foaf/spec/#term_birthday) property.
* The user's profile picture may be represented with the [foaf:img](http://xmlns.com/foaf/spec/#term_img) property.
* Each of the user's tags may be represented with the [foaf:interest](http://xmlns.com/foaf/spec/#term_interest) property.
* The user's Diaspora account may be represented by setting their [foaf:account](http://xmlns.com/foaf/spec/#term_account) property to the URI of the user's profile page.
* The user's Diaspora handle may be represented with the [foaf:accountName](http://xmlns.com/foaf/spec/#term_accountName) property of the resource given by the URI of the user's profile page.
* The user's Diaspora pod may be represented with the [foaf:accountServiceHomepage](http://xmlns.com/foaf/spec/#term_accountServiceHomepage) property of the resource given by the URI of the user's profile page.

### Posts
User posts might expose [SIOC](http://en.wikipedia.org/wiki/Semantically-Interlinked_Online_Communities) data.

* The post may be represented as a [sioc:Post](http://sioc-project.org/ontology#term_Post).
* The post's timestamp may be represented with the [dcterms:created](http://dublincore.org/documents/dcmi-terms/#terms-created).
* The post's creator may be represented by setting its [sioc:has_creator](http://sioc-project.org/ontology#term_has_creator) property to the creator's user profile page.
* Each of the post's tags may be represented with the [sioc:topic](http://sioc-project.org/ontology#term_topic) property.
* Each of the post's comments may be represented with the [sioc:has_reply](http://sioc-project.org/ontology#term_has_reply) property.