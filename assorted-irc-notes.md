Nushio> i'm interested in doing an android app for this :)

grippi> run bundle exec rake db:purge to clear the database

<Dante`> but basically: mongo : use diaspora-production ; db.users.find()
<Dante`> db.users.copyDatabase(diaspora-production,backup)

also: use diaspora-development ; db.copyDatabase("diaspora-development","diaspora-development", "backup server IP") (copies the whole database)

<koo5> how do i configure production environment to use development database?<br>
<chuck> koo5, you have to just manually hack the code and hardcode in a database name<br>
<chuck> in like config/initializers/_mongo.rb<br>
<chuck> you can also just copy over your old diaspora-development database to diaspora-production<br>
<koo5> aha!<br>
<koo5> lets see if just enabling caching in dev solves my speed problem..<br>
<koo5> dont wanna copy...maybe if it is possible to make an alias<br>
<chuck> not that I know of<br>
<chuck> if you really want to share the database, just hardcode the name<br>
<chuck> then make a git commit so git can merge it the next time you "git pull origin master"<br>
(note: speed problem quite sloved, see caching in config/environments/development)<br>

