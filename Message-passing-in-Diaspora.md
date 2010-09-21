Current as of Sep 21, 10:00 AM

## Message Passing
Given an object like a StatusMessage or a friend request, Diaspora first serializes that to xml with post.to_diaspora_xml
    salmon = Salmon::SalmonSlap.create(self, post.to_diaspora_xml)
    push_to_person( opts[:to], salmon.to_xml)
    salmon

Salmon::SalmonSlap.create base64encodes that xml, then signs it.
    def self.create(user, activity)
      salmon = self.new
      salmon.author = user.person
      salmon.magic_sig = MagicSigEnvelope.create(user , activity)
      salmon
    end

push_to_person encrypts the plain-text that is passed to it and POSTs it to the receive hook of the person that is passed in.
    QUEUE.add_post_request( person.receive_url, person.encrypt(xml) )

person.encrypt returns a JSON hash consisting of an RSA encrypted AES key and an AES-encrypted blob.

## Discovery
Given a diaspora_handle, Diaspora [webfingers](http://webfinger.org/) that server, creates a Person object from the information retrieved, and stores it in the database.  Nothing tricky here.

Obviously, this could be much cleaner/simpler/better