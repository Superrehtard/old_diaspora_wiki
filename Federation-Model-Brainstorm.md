Here's my brainstorm for a more focused federation model.

**There's two flavours of networks:**

1. Meeting new people 
-> "Gather interesting people **from the pool**" -> Through hashtags -> Small pods fail to deliver because of small pool -> Leads to centralisation. This is happening.

2. Stay in contact with friends 
-> "Direct connecting" -> Pods for individuals work perfectly ->  Decentralisation

Diaspora tries to do both "meeting people" as well as "decentralisation". This combination leads to architectural/traffic problems. The current model automatically leads to centralisation as small pods fail to deliver.

### Separate the processes!

1. Go by model 2 for your decentralisation fix. Protect privacy and ownership for all _private_ content. Does exactly what Kickstarters donated their money for. No evil at all. Everyone their own pod. Low traffic, simple system! 

2. Build a central index that provides model 1 as an add-on for Diaspora. This will contain Diaspora's "evil" part. Lets call it "Evilspora".

### Evilspora

Within model 2, allow users to authorize Evilspora. When authorised, Evilspora will pull all records of your public posts with hashtags into a centralised monolithic database to provide a perfect hashtag search system by delivering results back to model 2 servers. You can search these posts even when you do not allow Evilspora to pull your posts (to avoid dilemma's).

Evilspora only pulls and provides tagged public posts. It does nothing else. It does not host posts, just indexes them for reference. From within model 2, we could tell Evilspora to "forget our posts", clearing the index, or "forget after x days". We control Evilspora ourselves.

Evilspora does not see any private messages or friend relations (as opposed to Facebook). Because Evilspora is completely separated from model 2, it is possible to completely block Evilspora. Therefore people can choose between a fully decentralised network and a semi-decentralised network. Evilspora may be evil; he's still a lot less evil than Facebook because he's missing lots of context.

### TL;DR
Focus on perfect befriended federation between millions of one-man pods, while making hashtag search pod-independent. Currently, because JoinDiaspora provides an awesome hashtag experience, people choose to all join the same pod which centralises the network and causes a lot of scaling problems for JD. By moving the hashtag experience away from JD, the network becomes more equally decentralised as there will then be less downsides to self-hosting (poor reach or high traffic). It massively changes the relationship between big and small pods, for the better.

The central system that delegates hashtags will only have to list the hashtag, its location and its date. The pods can then look up a tag and pull in the locations of the latest dates, then connecting to the locations to get the content. The central system therefore wouldn't have to download the actual content.