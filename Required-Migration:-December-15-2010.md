Run "bundle exec rake migrations:contacts_as_requests" to move your data across commit 94fe0b4049d903b6f14c.

If you're running in production, do "RAILS_ENV=production bundle exec rake migrations:contacts_as_requests" so that the migration hits the right database.

This remove all 'sent' requests.  Anyone who has sent a request that hasn't been answered will see the answer as a new pending request.

This change simplifies the requesting system by making outgoing pending Requests Contacts (that's the object that represents a relationship) with a pending flag.