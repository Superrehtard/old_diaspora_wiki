----

###403 DO NOT MOVE###

We're currently **moving this wiki over to our new project site**. The contents of this page are not important enough to be ported over because they are either very old, very outdated or wrong and misleading. 

----

<pre>
[2011-07-24 21:38:35] [INFO] Now logging to <file:///C:/Users/invakam2/AppData/Roaming/Mozilla/Firefox/Profiles/01e5sn4k.default/chatzilla/logs/freenode/channels/%23diaspune.2011-07-24.log>.
[2011-07-24 21:39:49] |<-- sudiptamondal has left freenode (Ping timeout: 252 seconds)
[2011-07-24 21:40:04] <j4v4m4n_mobile> stultus where is subin/?
[2011-07-24 21:40:08] <vasudev> rajiv_nair: what do you think what kind of api we want?.. 
[2011-07-24 21:40:35] -->| Jerin_Philip (~Jerin_Phi@wikipedia/monu1618) has joined #diaspune
[2011-07-24 21:40:36] <vasudev> rajiv_nair: I was talking with ashik and we both thought restful API's similar to the Facebook or twitter will be great
[2011-07-24 21:40:42] <vasudev> every please share your thoughts
[2011-07-24 21:40:57] [ERROR] Connection to irc://freenode/ (ircs://irc.freenode.net:7000/) reset. [[Help][Get more information about this error online][faq connection.reset]]
[2011-07-24 21:41:47] -->| YOU (vasudev) have joined #diaspune
[2011-07-24 21:41:47] =-= Topic for #diaspune is ``Ruby on Rails Workshop on Saturday | http://www.meetup.com/Diaspora/Pune/288201/''
[2011-07-24 21:41:47] =-= Topic for #diaspune was set by j4v4m4n on Thursday, July 14, 2011 10:23:41 PM
[2011-07-24 21:42:45] <vasudev> ping.. did i miss anything?
[2011-07-24 21:43:20] <abdulkarim> vasudev, no
[2011-07-24 21:43:36] <vasudev> so what do you guys have to say about restful api's?
[2011-07-24 21:44:54] |<-- stultus has left freenode (Read error: Connection reset by peer)
[2011-07-24 21:45:20] * vasudev wonders if every one is still arround
[2011-07-24 21:45:40] [INFO] Messages Cleared.
[2011-07-24 21:46:47] * j4v4m4n_mobile doesn/lt know what is a restful api, is it normal http get, put etc?
[2011-07-24 21:47:08] <vasudev> j4v4m4n_mobile: yes just by doing http get post put delete
[2011-07-24 21:47:18] <vasudev> we can do what we exactly want
[2011-07-24 21:47:32] <vasudev> response can be in json or in xml but most time i prefer json
[2011-07-24 21:48:02] <vasudev> response means the data which is sent back by diaspora
[2011-07-24 21:48:09] -->| stultus (~stultus@117.97.52.83) has joined #diaspune
[2011-07-24 21:48:33] <vasudev> we may not need data all the time but for some operations like getting profile information getting feeds from your followers 
[2011-07-24 21:48:36] -->| ilyaZ (~ilyaZ@201.161.0.20) has joined #diaspune
[2011-07-24 21:48:43] <vasudev> woot ilyaZ joined us
[2011-07-24 21:48:44] <ilyaZ> hey everyone sorry for the lateness
[2011-07-24 21:48:46] <vasudev> ilyaZ: hello
[2011-07-24 21:48:51] <stultus> ilyaZ, hellozz :)
[2011-07-24 21:48:56] <abdulkarim> ilyaZ, hey
[2011-07-24 21:48:59] -->| Pooja (75c326a8@gateway/web/freenode/ip.117.195.38.168) has joined #diaspune
[2011-07-24 21:49:12] <vasudev> rajiv_nair: ping
[2011-07-24 21:49:20] <ilyaZ> (timezones just trolled me)
[2011-07-24 21:49:22] <j4v4m4n_mobile> ilyaz heya
[2011-07-24 21:49:27] <ilyaZ> hey j4v4m4n_mobile
[2011-07-24 21:49:38] <ilyaZ> hey vasudev stultus abdulkarim
[2011-07-24 21:50:26] <ilyaZ> sorry did I miss anything?
[2011-07-24 21:51:17] <vasudev> ilyaZ: nothing much I was just explaining restful api stuff which is provided by facebook and twitter
[2011-07-24 21:51:37] <vasudev> ilyaZ: i think diaspora should also have similar api's but I'm waiting for others suggestions
[2011-07-24 21:51:53] |<-- stultus has left freenode (Changing host)
[2011-07-24 21:51:56] -->| stultus (~stultus@wikisource/Hrishikesh.kb) has joined #diaspune
[2011-07-24 21:52:40] <ilyaZ> vasudev, in some ways the rails makes it super easy to have the same api be the same as your controller actions
[2011-07-24 21:53:48] -->| abc (75d37b2a@gateway/web/freenode/ip.117.211.123.42) has joined #diaspune
[2011-07-24 21:53:50] <vasudev> ilyaZ: novice in rails but yeah i remember a friend mentioning its easy to provide api's using rails
[2011-07-24 21:54:30] <ilyaZ> we were at first talking about doing that, but we talked to sarah and she had a good  point (that by not making it just the same routes, the api can scale differently than the app)
[2011-07-24 21:54:30] =-= abc is now known as Guest85711
[2011-07-24 21:54:56] <--| Guest85711 has left #diaspune
[2011-07-24 21:55:18] <ilyaZ> also that way we can version the api
[2011-07-24 21:55:43] -->| onthelivelyside (75d37b2a@gateway/web/freenode/ip.117.211.123.42) has joined #diaspune
[2011-07-24 21:56:01] <vasudev> ilyaZ: so you are not using rails method to provide api's?
[2011-07-24 21:56:28] <ilyaZ> because we need them to be at different actions
[2011-07-24 21:56:53] <ilyaZ> right now "api/v0" is where the api methods should go
[2011-07-24 21:56:57] <vasudev> ilyaZ: oh
[2011-07-24 21:57:24] <vasudev> ilyaZ: api's are available already?
[2011-07-24 21:58:15] <ilyaZ> not really, as praveen mentioned earlier we're agile about it
[2011-07-24 21:58:26] <ilyaZ> i.e. we build apis as they are needed
[2011-07-24 21:58:46] <vasudev> ilyaZ: oh okay..
[2011-07-24 21:59:02] <vasudev> ilyaZ: how about API's for authentication.. is it OAuth based?
[2011-07-24 21:59:45] <vasudev> first step for android app is authentication so I think that is the first API's which we need right?
[2011-07-24 21:59:53] * vasudev wonders why no one is talking!!
[2011-07-24 22:00:10] <vasudev> rajiv_nair: j4v4m4n_mobile ping
[2011-07-24 22:01:08] <ilyaZ> we have OAuth built in, we do a distributed authentication thing for cubbi.es (automatically does oauth preregistration)
[2011-07-24 22:02:51] <vasudev> ilyaZ: so what is the procedure for authenticating apps with diaspora?
[2011-07-24 22:03:09] <ilyaZ> hmm.. I think the first step to do something easy/ not distributed
[2011-07-24 22:03:31] <ilyaZ> (i.e. a user would have to do something to register the client application on their own pod)
[2011-07-24 22:03:39] -->| j4v4m4n (~user@gnu-india/supporter/j4v4m4n) has joined #diaspune
[2011-07-24 22:03:46] <vasudev> ilyaZ: we are directly getting access token for cubbi.es so this is what you meant by preregistration
[2011-07-24 22:03:50] <j4v4m4n> sorry, my mobile got switched off :(
[2011-07-24 22:04:13] <vasudev> j4v4m4n no issues :) reached home?
[2011-07-24 22:04:23] -->| grippi (~grippi@201.161.0.20) has joined #diaspune
[2011-07-24 22:04:43] <vasudev> grippi: heya
[2011-07-24 22:04:56] <j4v4m4n> vasudev: yup
[2011-07-24 22:04:57] <grippi> Ciao
[2011-07-24 22:04:57] <j4v4m4n> grippi: heya
[2011-07-24 22:04:57] <ilyaZ> so when you usually do oauth, you go to dev.twitter.com and get application id & secret
[2011-07-24 22:05:08] <vasudev> ilyaZ: yeah
[2011-07-24 22:06:14] <vasudev> ilyaZ: but when it comes to diaspora things are different right because we have different pods!
[2011-07-24 22:06:15] <ilyaZ> so for apps (at least web apps we automate this preregistration by using keypairs)
[2011-07-24 22:06:23] <ilyaZ> vasudev, yeah
[2011-07-24 22:07:01] <vasudev> ilyaZ: but if we wanted to change access token then?
[2011-07-24 22:07:17] <vasudev> like in twitter its possible to reset tokens is it possible with preregistration ?
[2011-07-24 22:07:32] <grippi> Yeah
[2011-07-24 22:07:33] <ilyaZ> vasudev, you can still reset tokens
[2011-07-24 22:07:39] <vasudev> oh okay
[2011-07-24 22:07:57] <grippi> Pre-registration is pretty removed from the oauth 2 flow
[2011-07-24 22:08:19] <--| kutti-chat-an has left #diaspune
[2011-07-24 22:08:23] <grippi> It's just really a check before we perform oauth
[2011-07-24 22:08:46] <vasudev> so when we develop android app I think distributed nature should be given to client side right.. I mean user can select pod from settings before authenticating
[2011-07-24 22:09:02] <grippi> Yeah, exactly
[2011-07-24 22:09:10] <vasudev> if not selecting he can enter pod url in the app settings that way things will be simpler to implement on server side
[2011-07-24 22:09:27] <grippi> What we should do is have the user just sign in using their diaspora id
[2011-07-24 22:09:27] -->| ershad_ (~ershad@49.200.218.222) has joined #diaspune
[2011-07-24 22:09:50] <vasudev> yeah.. we can get pod url from their diaspora id itself
[2011-07-24 22:10:04] <vasudev> username @ pod_url
[2011-07-24 22:10:12] <ilyaZ> I think the app should just generate a keypair on boot
[2011-07-24 22:10:36] =-= ershad_ is now known as ershad
[2011-07-24 22:10:36] <grippi> We don't need to set a server if someone supplies their entire nick/domain
[2011-07-24 22:10:36] <ilyaZ> and then use diaspora-connect flow
[2011-07-24 22:10:36] <grippi> Right
[2011-07-24 22:10:39] <grippi> (Sorry if I'm slow to type, I'm on a phone)
[2011-07-24 22:10:54] <vasudev> ilyaZ: key pair means token key and token secrets?
[2011-07-24 22:11:07] <ilyaZ> vasudev, nope an RSA key pair
[2011-07-24 22:11:18] <ilyaZ> one sec
[2011-07-24 22:11:26] |<-- j4v4m4n_mobile has left freenode (Quit: Lost terminal)
[2011-07-24 22:11:38] <ilyaZ> https://github.com/diaspora/diaspora-client this is the ruby lib that cubbi.es uses to authenticate
[2011-07-24 22:11:44] <grippi> vasudev: It's very similar to how browser extensions have keys
[2011-07-24 22:12:05] <grippi> At least with chrome extensions...
[2011-07-24 22:13:05] <vasudev> grippi: i never went through code ;) i blindly used your chrome code in Firefox extension
[2011-07-24 22:13:24] <ilyaZ> An application uses a key-pair to sign an well-defined authorization challenge.
[2011-07-24 22:13:25] <ilyaZ> A Diaspora pod fetches the application's manifest and the public key for that application.
[2011-07-24 22:13:25] <ilyaZ> The pod checks the signature for that manifest and the authorization request.
[2011-07-24 22:13:25] <ilyaZ> The application's registration credentials are returned to the application (assuming everything checks out).
[2011-07-24 22:13:46] <vasudev> grippi: can you share latest code of firefox extension?. there is still some issue with twitter infinite scroll 
[2011-07-24 22:14:15] <ilyaZ> this is instead of going to dev.twitter.com (because an app developer can't regester at every pod)
[2011-07-24 22:14:36] <grippi> All the code for it is on the FF addons site
[2011-07-24 22:14:36] <ilyaZ> (+ pods they don't know about)
[2011-07-24 22:14:54] <vasudev> ilyaZ: oh
[2011-07-24 22:15:15] <vasudev> grippi: okay I'll update my repo once I've a Linux laptop 
[2011-07-24 22:15:35] <vasudev> ilyaZ: so this flow is bit different compared to normal Oauth right?
[2011-07-24 22:15:38] <grippi> vasudev: It shouldn't be messing up twitter...
[2011-07-24 22:15:44] <grippi> vasudev: Kk
[2011-07-24 22:15:48] -->| ksnmdksdn` (7aa766b5@gateway/web/freenode/ip.122.167.102.181) has joined #diaspune
[2011-07-24 22:15:59] <ilyaZ> vasudev, after the pre-registration its regular oauth 2 from there
[2011-07-24 22:16:50] <vasudev> so instead of getting key and secret from pod we use key pair instead to sign the authorization challenge and rest of the flow remains same right?
[2011-07-24 22:17:13] <vasudev> and this should happen only once when the app is to be authorized with a pod right?
[2011-07-24 22:17:59] <ilyaZ> one sec
[2011-07-24 22:18:25] <vasudev> okay
[2011-07-24 22:18:36] * vasudev is getting confused with preregistration stuff
[2011-07-24 22:18:56] <--| ksnmdksdn` has left #diaspune
[2011-07-24 22:19:27] <ilyaZ> so pre-registration is equivalent to going to dev.twitter.com
[2011-07-24 22:19:50] -->| napster (7aa766b5@gateway/web/freenode/ip.122.167.102.181) has joined #diaspune
[2011-07-24 22:20:30] * napster sorry for being late!
[2011-07-24 22:20:37] <ilyaZ> napster, np
[2011-07-24 22:21:07] <napster> What is the plan? Just an android client like one for facebook?
[2011-07-24 22:21:13] <vasudev> so when app sends the authorization challenge signed with keypair what is returned to it?
[2011-07-24 22:21:30] <ilyaZ> vasudev, I need to go check out of the hotel, I'll be back in 15 mins
[2011-07-24 22:21:39] <vasudev> ilyaZ: sure np
[2011-07-24 22:21:45] <ilyaZ> brb
[2011-07-24 22:21:56] * vasudev want to have a look normal oauth flow again :)
[2011-07-24 22:22:05] |<-- grippi has left freenode (Remote host closed the connection)
[2011-07-24 22:22:07] <j4v4m4n> napster: yes
[2011-07-24 22:22:15] <abdulkarim> napster, yes
[2011-07-24 22:22:16] -->| grippi (~grippi@201.161.0.20) has joined #diaspune
[2011-07-24 22:22:31] <j4v4m4n> napster: vasudev was discussing authentication flow for the app
[2011-07-24 22:22:44] =-= offSchub is now known as DenSchub
[2011-07-24 22:22:48] <stultus> napster, any other idea in your mind?
[2011-07-24 22:23:21] <--| Pooja has left #diaspune
[2011-07-24 22:23:25] <DenSchub> back
[2011-07-24 22:23:42] <napster> j4v4m4n: I had another idea, since everyone keeps their android phone up and online all over the time, what about setting up pods of their own on their mobiles?
[2011-07-24 22:24:16] <napster> the data goes nowhere but stays back on their devices...
[2011-07-24 22:26:13] |<-- ilyaZ has left freenode (Ping timeout: 255 seconds)
[2011-07-24 22:26:14] <j4v4m4n> napster: that will have more challenges, but that can be a separate project
[2011-07-24 22:26:37] |<-- grippi has left freenode (Ping timeout: 255 seconds)
[2011-07-24 22:27:59] <napster> j4v4m4n: more challenges : yes
[2011-07-24 22:28:29] <abdulkarim> napster, that sounds like FreedomBox :)
[2011-07-24 22:30:10] <napster> abdulkarim: I guess this idea will serve the entire idea behind Diaspora...
[2011-07-24 22:31:02] <napster> abdulkarim: freedomebox : yes, a sort of
[2011-07-24 22:31:27] <j4v4m4n> napster: lets try that in parallel to our regular android app
[2011-07-24 22:31:33] <napster> j4v4m4n: ok
[2011-07-24 22:32:02] <j4v4m4n> napster: you understand it would take us to reimplment the whole diaspora :( , right?
[2011-07-24 22:32:35] <napster> j4v4m4n: need to think more...
[2011-07-24 22:33:11] <napster> not sure
[2011-07-24 22:33:39] <j4v4m4n> napster: what were in your mind?
[2011-07-24 22:33:57] <j4v4m4n> napster: get ruby on rails on android?
[2011-07-24 22:34:11] <napster> j4v4m4n: http://andrewgertig.com/2010/07/android-and-ruby-on-rails/
[2011-07-24 22:34:44] <napster> this is not completly related thing
[2011-07-24 22:34:51] <DenSchub> oh, shit
[2011-07-24 22:35:03] <DenSchub> My bouncer didn't log...
[2011-07-24 22:35:08] <napster> but, I guess, to do it we might need ROR on android
[2011-07-24 22:37:13] <vasudev> now why do we need to run it on a android phone?
[2011-07-24 22:37:36] <vasudev> when there are pods and a diaspora client for mobile phones I think that is more than sufficient right?
[2011-07-24 22:37:43] <vasudev> why to complicate things
[2011-07-24 22:37:45] |<-- ershad has left freenode (Quit: Good night :))
[2011-07-24 22:37:45] <j4v4m4n> vasudev: post the log on the wiki
[2011-07-24 22:37:45] -->| grippi (~grippi@201.161.0.20) has joined #diaspune
[2011-07-24 22:37:51] -->| grippi_ (~grippi@201.161.0.20) has joined #diaspune
[2011-07-24 22:38:01] <vasudev> grippi in multiple instances :)
[2011-07-24 22:38:09] <vasudev> j4v4m4n: please link me
[2011-07-24 22:38:32] <grippi_> Is my chat client going crazy right now?
[2011-07-24 22:38:34] |<-- grippi has left freenode (Read error: Connection reset by peer)
[2011-07-24 22:38:34] =-= grippi_ is now known as grippi
[2011-07-24 22:38:48] <j4v4m4n> vasudev: https://github.com/diaspora/diaspora/wiki/Android-app-project
[2011-07-24 22:39:18] <vasudev> j4v4m4n: i will be around may be 15-20 mins more then i'll leave as I've got office tomorrow :(
[2011-07-24 22:39:46] <j4v4m4n> vasudev: if you can post the log, may be napster can take it from here
[2011-07-24 22:40:08] <vasudev> j4v4m4n: yeah i'm doing that now
[2011-07-24 22:40:15] <j4v4m4n> vasudev: thanks
[2011-07-24 22:40:20] |<-- shilpi has left freenode (Ping timeout: 252 seconds)
[2011-07-24 22:40:34] <vasudev> j4v4m4n: no issues
[2011-07-24 22:40:44] -->| shilpi (0117ba89@gateway/web/freenode/ip.1.23.186.137) has joined #diaspune
[2011-07-24 22:40:53] <vasudev> grippi: we saw you entering room multiple times :) but now only one instance is there
[2011-07-24 22:41:03] <grippi> K phew
[2011-07-24 22:41:35] <vasudev> grippi: iPhone or Android ?
[2011-07-24 22:43:34] <napster> j4v4m4n: What are the expected funtionalioties apart from a normal facebook like client?
[2011-07-24 22:43:36] |<-- libin has left freenode (Read error: Connection reset by peer)
[2011-07-24 22:43:46] -->| ilyaZ (~ilyaZ@201.161.0.20) has joined #diaspune
[2011-07-24 22:43:58] <ilyaZ> back
[2011-07-24 22:44:04] <DenSchub> hi ilyaZ :)
[2011-07-24 22:44:16] <ilyaZ> hey DenSchub
[2011-07-24 22:44:44] <ilyaZ> vasudev, are you still here?
[2011-07-24 22:44:49] <vasudev> ilyaZ: welcome back 
[2011-07-24 22:44:53] <vasudev> ilyaZ: yeah :)
[2011-07-24 22:45:31] |<-- grippi has left freenode (Ping timeout: 255 seconds)
[2011-07-24 22:46:21] [INFO] <ircs://freenode/diaspune> will now open at startup.
[2011-07-24 22:47:07] |<-- napster has left freenode (Quit: Page closed)
[2011-07-24 22:47:20] <j4v4m4n> napster: post and read messages, view and share pictures, share links from browser - normal facebook client functionality
[2011-07-24 22:47:55] <j4v4m4n> ilyaZ: Shravan was supposed to join, but he is down with fever :(
[2011-07-24 22:48:21] <ilyaZ> yeah, photos are the awesome thing
[2011-07-24 22:48:31] <vasudev> j4v4m4n: chatzilla is not allowing me to open log when its in running mode

<j4v4m4n> vasudev: :(  [22:49]
<vasudev> j4v4m4n: no opened it up using notepad now adding it to wiki page
								        [22:50]
*** stultus (~stultus@wikisource/Hrishikesh.kb) has quit: Ping timeout: 258
    seconds  [22:51]
<ilyaZ> so I'm  [22:52]
*** stultus (~stultus@wikisource/Hrishikesh.kb) has joined channel #diaspune
<j4v4m4n> vasudev: lets start with an app that authenticates and gets the
	  stream onto the phone
<vasudev> ilyaZ: coming back to preregistration stuff.. in twitter case we are
	  using token secret and key first time to get back access key and
	  token which is then used to sign the messages which go to twitter
	  how this happens in case of diaspora?
<ilyaZ> we use the key pair to sign a challange  [22:53]
<ilyaZ> if the signature verifies
<ilyaZ> the app gets app id and secret( this is eqivalent to going to
	dev.twitter.com)
<vasudev> ilyaZ: how does server verify the signed challenge? do we need to
	  share the key pair with pod?  [22:54]
<ilyaZ> thouse are just oauth2 cread
<ilyaZ> **credentials
<vasudev> @all I've updated the logs in the wiki
<ilyaZ> it goes to app.com/manifest.json
<ilyaZ> http://www.cubbi.es/manifest.json  [22:55]
<j4v4m4n> ilyaZ: does app id conatain name of pod which issued it?  [22:56]
<ilyaZ> that's the key + signed (and encoded) manifest
<vasudev> ilyaZ: from where do we generate this file?
<vasudev> app need to register in the pod?
<ilyaZ> j4v4m4n, nope (are you asking for collision reasons?)  [22:57]
<ilyaZ> it's diaspora-client
<ilyaZ> there is a task generate_manifest
<j4v4m4n> ilyaZ: how do another pod verify it?  [22:58]
*** grippi (~grippi@201.161.0.20) has joined channel #diaspune
<ilyaZ>
	https://github.com/diaspora/diaspora-client/blob/master/lib/tasks/diaspora-client.rake
*** grippi_ (~grippi@201.161.0.20) has joined channel #diaspune  [22:59]
<vasudev> ilyaZ: grippi j4v4m4n its time for me to hit the bed.. I wish I
	  could continue :(
<vasudev> j4v4m4n: please share the logs I'll catch up with your discussion
	  tomorrow
<j4v4m4n> vasudev: lets start with an app that authenticate first and then
	  move on to more features  [23:00]
<j4v4m4n> vasudev: thanks, good night
<ilyaZ> my roomate has a begining
<ilyaZ> let me find a link
<vasudev> j4v4m4n: i still need some more information.. do we have any mailing
	  list where we can do offline discussion
<j4v4m4n> diaspora-dev  [23:01]
<j4v4m4n> vasudev: google groups
<vasudev> j4v4m4n: sure thanks
*** grippi_ (~grippi@201.161.0.20) has quit: Read error: Connection reset by
    peer
<ilyaZ> https://github.com/gardner/diaspora-android
<vasudev> ilyaZ: grippi j4v4m4n good night then
<ilyaZ> vasudev, good night
<stultus> vasudev, good night :)
<vasudev> stultus: good night
<grippi> hello?  [23:02]
<vasudev> grippi: :)
<stultus> vasudev, and diaspune mailing list :)
<vasudev> grippi: time for me to leave.. Good night..
<ilyaZ>
	https://github.com/diaspora/diaspora/blob/master/app/controllers/authorizations_controller.rb#L25
<vasudev> stultus: please mail me the links
<ilyaZ> is where the verification happens
*** vasudev (~kakashi@112.79.163.63) has quit: Quit: Good night world
<j4v4m4n> ilyaZ: thanks  [23:03]
<ilyaZ> there is a video of us talking about this at campus party
ERC> /names  [23:04]
*** Users on #diaspune: grippi stultus ilyaZ shilpi j4v4m4n onthelivelyside
    sudiptamondal1 sana manojkmohan DenSchub abdulkarim rajiv_nair @ChanServ
<ilyaZ> we haven't actually seen it (so hopefully there is nothing embarassing
	) once we watch it we'll post it
<ilyaZ> http://www.youtube.com/watch?v=Oc2-Lnzm9bI
*** stultus (~stultus@wikisource/Hrishikesh.kb) has quit: Read error:
    Connection reset by peer
<j4v4m4n> ilyaZ: :)
*** grippi (~grippi@201.161.0.20) has quit: Read error: Connection reset by
    peer
<j4v4m4n> ilyaZ: since all android folks are gone now, lets continue the
	  discussion on list  [23:05]
*** grippi (~grippi@201.161.0.20) has joined channel #diaspune
<ilyaZ> j4v4m4n, kk
<j4v4m4n> ilyaZ: grippi thanks for coming!
<j4v4m4n> grippi: we will continue it on diaspora-dev
<ilyaZ> j4v4m4n, thanks for throwing out a time, again sorry for being missing
	in action with this conference
ERC> 
</pre>