This page is meant to sum up the discussion so far

# Public-private key
To enable an efficient encryption of posts (also files) for groups (aspects) the following is applied:

1. a random key (RK) is generated
1. the post is encrypted with the random key: enc(RK, msg)
1. for each recipient Rn, RK is encrypted with their public key: enc(pub(Rn), RK)
1. the encrypted key is sent to each recipient

If a friend is added to the group, RK is encrypted once more.
If a friend is removed from a group, we don't generate a new RK to re-encrypt everything.

# SSL
Encrypt the connection between servers...

# end-to-end encryption and "PGP-ready"
one draft proposal of the various options for encryption, and an advocacy of end-to-end-encryption and making Diaspora* "PGP-ready" can be found here http://github.com/diaspora/diaspora/wiki/PrettyGoodDiaspora