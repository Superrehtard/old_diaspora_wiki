Diaspora supports a number of ways to format messages. These are basically a part of the description language  [Markdown](http://de.wikipedia.org/wiki/Markdown).

## Links ##
Full URLs, for example http://example.org , are automatically converted into links. This also works for ftp:// like ftp://example.org . Furthermore, everything starting with www. is converted into a link, for example www.example.org.

For more control, the Markdown syntax also allows the link to be named. For example

    [Official Website](http://example.org)

is [Official Website](http://example.org). Key point: _square brackets before round_.

## Text formatting ##
    *Italic Text*
becomes "*Italic Text*".

    **Bold Text**
becomes "**Bold Text**".

    ***Bold Italic Text***
becomes "***Bold Italic Text***".

Similarly, we can also use "\_":

    ___Bold Italic Text___
This allows for nesting:

    _Italic **Bold** Text_
becomes "*Italic **Bold** Text*".

If needed, a "\*" or "\_" inside the text can be escaped with the "\" character:

    __Use Two \_ for Bold Text__
becomes "**Use Two \_ for Bold Text**".
Of course, this can also be used to prevent formatting:

    \_Italic_ \**Bold**
becomes "\_Italic_ \**Bold**".

(Translated from http://wiki.spored.de/w/Nachrichten_formatieren)