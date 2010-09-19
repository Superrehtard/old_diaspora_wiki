```irc¬
<Nushio> i'm interested in doing an android app for this :)

grippi> run bundle exec rake db:purge to clear the database

<Dante`> but basically: mongo : use diaspora-production ; db.users.find()
<Dante`> db.users.copyDatabase(diaspora-production,backup)

also: use diaspora-development ; db.copyDatabase("diaspora-development","diaspora-development", "backup server IP") (copies the whole database)

<koo5> how do i configure production environment to use development database?
<chuck> koo5, you have to just manually hack the code and hardcode in a database name
<chuck> in like config/initializers/_mongo.rb
<chuck> you can also just copy over your old diaspora-development database to diaspora-production
<koo5> aha!
<koo5> lets see if just enabling caching in dev solves my speed problem..
<koo5> dont wanna copy...maybe if it is possible to make an alias
<chuck> not that I know of
<chuck> if you really want to share the database, just hardcode the name
<chuck> then make a git commit so git can merge it the next time you "git pull origin master"
(note: speed problem quite sloved, see caching in config/environments/development)
```
