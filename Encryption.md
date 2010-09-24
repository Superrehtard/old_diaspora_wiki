To enable an efficient encryption of posts (also files) for groups (aspects) the following is applied:

1. a random key (RK) is generated
1. the post is encrypted with the random key: enc(RK, msg)
1. for each recipient Rn, RK is encrypted with their public key: enc(pub(Rn), RK)
1. the encrypted key is sent to each recipient

If a friend is added to the group, RK is encrypted once more.
The only problem occurs if a friend is removed from a group. This would require to generate a new RK and to re-encrypt everything.