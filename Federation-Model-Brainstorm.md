**There's two models:**

1. Stay in touch with friends -> direct connection between seeds -> works smooth for small pods ->  Leads to **Decentralisation**. This is the original goal. Works like e-mail.

2. **Meeting new people** -> requires: gathering interesting people from "the pool" -> Small pods: small pools -> they fail to deliver -> Leads to centralisation. This is happening.

Diaspora's strategy: **decentralisation and meeting people**.

To sum this up: Since Diaspora focuses a lot on meeting new people, pods require access to a big data pool. Only big pods can provide these, therefore people move to big pods, which causes centralisation. This creates high traffic for big pods, resulting in unneeded/unwanted performance issues. Also, pods have to synch with each other, all keeping copies of the same info. Doesn't scale well. Self-hosting is at a huge disadvantage.

### Fix: Separate the processes!

1. Go by model 1 for your decentralisation goals. This model protects privacy and ownership of all private content. Does exactly what Kickstarters donated their money for. Everyone their own equal pod. Low traffic, simple install.

2. Build a central index that provides model 2 as an **add-on** for Diaspora, globally. In other words: move the hashtag index out of the big pods, into a centralised index. Lets call the central index "Evilspora".

### Evilspora

Within model 1, allow users to authorize Evilspora. When authorised, Evilspora will pull all records of your public posts with hashtags into a centralised monolithic database to provide a perfect hashtag search system by delivering results back to model 1 servers. You can search these posts even when you do not allow Evilspora to pull your posts (to avoid dilemma's).

Evilspora only pulls and provides tagged public posts. It does nothing else. It does not host posts, just indexes them for reference. From within model 1, we could tell Evilspora to "forget our posts", clearing the index, or "forget after x days". We control Evilspora ourselves.

Evilspora does not see any private messages or friend relations (as opposed to Facebook). Because Evilspora is completely separated from model 1, it is possible to completely block and work around Evilspora. People can choose between a fully decentralised network and a semi-decentralised network. Evilspora may be evil; its still a lot less evil than Facebook because its missing lots of context, which is hosted on the pods themselves.

### TL;DR
Focus on perfect befriended federation between millions of one-man pods, while making hashtag search pod-independent. Currently, because JoinDiaspora provides an awesome hashtag experience, people choose to all join the same pod which centralises the network and causes a lot of scaling problems for JD. By moving the hashtag experience away from JD, the network becomes more equally decentralised as there will then be less downsides to self-hosting (poor reach or high traffic). It massively changes the relationship between big and small pods, for the better.

The central system that delegates hashtags will only have to list the hashtag, its location and its date. The pods can then look up a tag and pull in the locations of the latest dates, then connecting to the locations to get the content. The central system therefore wouldn't have to download the actual content.