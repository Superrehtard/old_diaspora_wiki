----

###403 DO NOT MOVE###

We're currently **moving this wiki over to our new project site**. The contents of this page are not important enough to be ported over because they are either very old, very outdated or wrong and misleading. 

----

# Config Notes on SSL, Heroku and S3

So I noticed while testing on Chrome that some of my assets (which I was dutifully uploading into S3) where not being allowed to be loaded and interpreted by Chrome when I switched my pod from not running SSL (and using a "herokuapp.com" FQDN) to running a GoDaddy cert and using my own FQDN (for example: https://foo.domain.com/).

These notes are a few things that puzzled me....

I had setup my S3 bucket at "foo.domain.com" and then tried to serve my assets and uploaded pix out of 
`https://foo.domain.com.s3.amazonaws.com/assets/....`

well, no, the Chrome browser (but not Safari) complained that 
`https://foo.domain.com.s3.amazonaws.com/assets/bar_whatever_ugglylongnumber4579218739172987298371.js`
was not okay because it didn't match the SSL cert "*.s3.amazonaws.com" so the CSS and JS files would not load and I would get unstyled pages.

Hmm.

So what finally worked for me was to have Heroku/S3 and an SSL cert from GoDaddy where I changed the bucket name of my assets to

`foo-domain` from `foo.domain.com`

and then I set in diaspora.yml
`    image_redirect_url: 'https://s3.amazonaws.com/foo-domain/'`

`      host: https://s3.amazonaws.com/foo-domain`

`    url: "https://foo.domain.com/"`

I should also point out that
`https://s3.amazonaws.com/foo-domain/`
and
`https://foo-domain.s3.amazonaws.com/`
are essentially the same.

But that under the weird SSL "rules" (well, Perhaps you share my opinion that there might be a castle somewhere that governs how SSL works)
`https://s3.amazonaws.com/foo.domain/`
and
`https://foo.domain.s3.amazonaws.com/`
are most decidedly **Not**.
