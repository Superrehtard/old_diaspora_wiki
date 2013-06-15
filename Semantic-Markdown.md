----

###403 DO NOT MOVE###

We're currently **moving this wiki over to our new project site**. The contents of this page are not important enough to be ported over because they are either very old, very outdated or wrong and misleading. 

----

This is an extension of Markdown which adds tags mapping to [RDFa Lite](http://www.w3.org/2010/02/rdfa/sources/rdfa-lite/).

Each semantic markdown _element_ begins with a left-wards curly bracket (`{`). The opening bracket is immediately followed by a list of one or more _attributes_. Each attribute in the list is separated by a space character (` `).

Each attribute is an _attribute name_ followed by an equal sign (`=`) followed by an _attribute value_.

The attribute name must be one of the following:

* `vocab`
* `typeof`
* `property`
* `about`
* `prefix`

The attribute value is an arbitrary string of characters which may be delimited by double quotation marks. If the attribute value is not delimited by double quotation marks, it is terminated by a space character or a colon (`:`) or a right-wards curly bracket (`}`), whichever comes first.

If an attribute value is terminated by a colon, the attribute list is also terminated. If an attribute value is terminated by a right-wards curly bracket, both the attribute list and the semantic markdown element is also terminated.

If the attribute list is terminated by a colon, a string of element _content_ may follow. The element content is formatted according to the normal rules of Markdown, i.e. it contains arbitrary Markdown data. The element content is terminated by a right-wards curly bracket, which also terminates the semantic markdown element.

When the semantic markdown element is terminated, it may be converted to HTML as follows:

The semantic markdown element content is converted to HTML according to the standard rules of Markdown and enclosed in a HTML `span` element. Each attribute of the semantic markdown element is listed as an attribute of the `span` element.