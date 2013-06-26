----

###403 DO NOT MOVE###

We're currently **moving this wiki over to our new project site**. The contents of this page are not important enough to be ported over because they are either very old, very outdated or wrong and misleading. 

----

# Public Pod Administration
# FAQ
[[Server Maintainer FAQ|FAQ for Pod Maintainers]]

# Manage your server
## Choose infrastructure
### Self hosting

Basically you have two choices. 

1. Use an application server like thin and a reverse proxy lie nginx that exposes it
2. Use a web server that has a ruby module like passenger (you can use apache or nginx for that)


#### Application server + reverse proxy
  1. thin + nginx TBD
  2. thin + apache TBD

#### Web server + ruby module

TBD

## Start/Stop/Restart server
TBD


## Make your server publicly accessible
If your server is hosted behind a NAT you will need either to forward the necessary ports to your bacend or use a service like [pagekite](http://pagekite.net)

### Port Forwarding

TBD

### Pagekite

TBD

# Manage the users
## Get people to use your pod
TBD
## Prevent new sign ups
TBD
# Staying up to date
## Get the newest version from the repository
`git pull`

## Things to do after a pull
1. `bundle exec install`
2. `bundle exec rake db:migrate`
3. Restart server
4. Load server start page
5. `bundle exec jammit`


# Troubleshooting
## Certificate problems
TBD
## Inter Pod Communications problems
TBD
## Check Resque worker
TBD