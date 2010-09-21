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

Obviously, this could be much cleaner/simpler/better.  Suggestions welcome.  Salmon is not quite suited to encrypted traffic, because signatures on destination-dependent ciphertext can't be retained for relayed messages, and encrypting a whole salmon seems awkward. 

One possibility is to use a modified salmon in which the author info is encrypted PK for the recipient, with the aes private key inside, and the data is aes-encrypted.  The magic-signature would have to either be on the aes ciphertext and that ciphertext would have to be reused, or it would have to be on the non-exposed plain text. Tag names below are for explanation only.
    <salmon>
        <encrypted author info>
            email
            aes-private-key
        </encrypted author info>
        <data>
            aes encrypted data
        </data>
        <salmon signature>
             Either on the aes encrypted data or the plaintext data
        </salmon signature>
    </salmon>