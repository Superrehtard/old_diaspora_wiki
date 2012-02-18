**There's two models:**

1. Stay in touch with friends -> direct connection between seeds -> works smooth for small pods ->  Leads to **Decentralisation**. This is the original goal. Works like e-mail.

2. **Meeting new people** -> requires: gathering interesting people from "the pool" -> Small pods: small pools -> they fail to deliver -> Leads to centralisation. This is happening.

Diaspora's strategy: **decentralisation and meeting people**.

To sum this up: Since Diaspora focuses a lot on meeting new people, pods require access to a big data pool. Only big pods can provide these, therefore people move to big pods, which causes centralisation. This creates high traffic for big pods, resulting in unneeded/unwanted performance issues. Also, pods have to synch with each other, all keeping copies of the same info. Doesn't scale well. Self-hosting is at a huge disadvantage.

### Fix: Separate the processes!

1. Go by model 1 for your decentralisation goals. This model protects privacy and ownership of all your private content by safely sharing it directly to your friends. Also hosts public posts, which are shared with friends and _can_ be accessed by others. Does exactly what Kickstarters donated their money for. Everyone their own **equal** pod. Low traffic, simple install.

2. Build an index that provides model 2 as an **add-on** service for Diaspora, globally. In other words: move the hashtag index out of the big pods, into a centralised index. Lets call it "Evilspora".

### Evilspora

Within model 1, allow users to optionally authorize Evilspora. When authorised, Evilspora will pull the "tag,postid,location,date" of your public posts with hashtags into a centralised index to provide a perfect hashtag search system by delivering results back to model 1 servers. You can search these posts even when you do not allow Evilspora to pull your posts (to avoid dilemma's).

### Why centralisation is not hurtful
Evilspora only pulls and provides **references to** tagged public posts. It does nothing else. It does not host posts, just indexes them for reference. From within model 1, we could even tell Evilspora to "forget our posts", clearing the index, or "forget after x days". We control Evilspora ourselves. It's a centralised system controlled by a decentralised system. Like a cloud synch platform. Also, when we delete a post from our pod (data ownership), Evilspora will only have a dead link. Control remains decentralised.

Evilspora does not see any private messages or friend relations (as opposed to Facebook) and because Evilspora is completely separated from model 1, it is possible to completely block and work around Evilspora. 

**People can choose between a fully decentralised network and a semi-decentralised network**, whereas the current trend is working towards an almost-centralised network.

### TL;DR
Focus on perfect befriended federation between millions of one-man pods, while making hashtag search pod-independent. Currently, because JoinDiaspora provides an awesome hashtag experience, people choose to all join the same pod which centralises the network and causes a lot of scaling problems for JD. By moving the hashtag experience away from JD, the network becomes more equally decentralised as there will then be less downsides to self-hosting (poor reach or high traffic). It massively changes the relationship between big and small pods, for the better.

The central system that delegates hashtags will only have to list the hashtag, its location and its date. The pods can then look up a tag and pull in the locations of the latest dates, then connecting to the locations to get the content. The central system therefore wouldn't have to download the actual content.

### The Sean Version
So, it's apparent that this is kind of a problem. We want to utilize decentralization, but our community in and of itself is centralized as a concept (not particularly between servers so to speak, but the community hub itself is a cultural epicenter on the platform. The very idea of community is central, and so this illusion of centralization ought to be pulled off on a decentralized network)

Kevin's idea with a background process is interesting, but I'll go one step further: put something together in a pod configuration so that it can federate and pull in tags as a user follows them. The pods to pull from could be easily added or switched off in a config script. The only real heavy-lifting required may be that the pod might be told that public tagged content needs to be federated, and fetched as needed (rather than downloading the whole bulk catalog). This may require changes to the current federation implementation.

### Kevin -> Sean
If you list pods to pull from in a configuration file, the system will rely on these pods only to gather all references to public tagged content. Therefore, it will only work when over 90% of users/content are on these big pods, e.g. JD, Diasp, Geraspora, otherwise too much content may not be found and the system will be lacking. And the big pods is what we would want to move away from. Indirectly, this is how the system currently works.

You could also apply this to all the pods (all2all) but then things become too heavy and complex. Imagine drawing lines between all these dots. That's a no-go as well. A huge problem is that all the small pods will have to host all the references created on the network, in other to provide a valuable list to their neighbouring pods. Indexes would never be able to be completed by small pods and eventually the whole network would collapse.

That's why I believe it would be better if all pods would push to a single index (albeit hosted by a handful of synchronised servers, for stability). **The actual data will be federated** but the locations will be archived in a central place for easy reference. The pod can then fetch the data from the several pods, present it to the user and throw away the results after a certain period of time. This will consume some bandwidth but it wouldn't require small pods to hoard the data/references. The references only exist once.

If you want to decentralise this system, you need big pods and these will be keeping the same records, which really clogs everything up and causes problems. A centralised reference index will enable small pods to have an incredible reach without blowing up the server.

If for some reason the central index will ever go BOOM!, then everything will continue to work, except for worldwide hashtag searching. That's all. The rest of the network would not be touched. All that's decentralised will work independently. If Diaspora matters enough, then there will always be support to keep the central index going. Pretty sure people like those from the Piratebay would get involved in restoring the index.

The system really can be as simple as:

- I want "#cats"
- Request "#cats" from index
- Get 100 "#cats" references
- Fetch the 10 latest posts from other pods
- Display cuteness.

Whereas the current situation is, I believe:

- I want "#cats"
- Search for "#cats" in index of locally stored public posts
- Display the latest 10 posts

That means all the posts need to be stored locally before they can be displayed. That needs a workaround. The central index could just take the search term and spit out a list of references, then move on to the next search term.

**Perfect decentralisation allows you to view as much content as possible, while keeping storage and traffic as low as possible.**