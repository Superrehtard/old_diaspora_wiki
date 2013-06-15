----

###301 MOVED PERMANENTLY###

We're currently **moving this wiki over to our new project site**. The contents of this page have  already been carried over, so _any new changes here will not be reflected in the new wiki_.  
New link: http://wiki.diasporafoundation.org/Diaspora_Message_Passing

----
Current as of Oct 6, 11:00 PM

## Message Passing
Given an object like a StatusMessage or a friend request, Diaspora first serializes that object to xml with 

```ruby
post.to_diaspora_xml
    def push_to_people(post, people)
      salmon = salmon(post) # this is just Salmon::SalmonSlap.create(self, post.to_diaspora_xml)
      people.each{|person|
        xml = salmon.xml_for person
        push_to_person( person, xml)
      }
    end

Salmon::SalmonSlap.create # aes encrypts the body(that xml) and signs the aes ciphertext
    def self.create(user, activity)
      salmon = self.new
      salmon.author = user.person
      aes_key_hash = user.person.gen_aes_key
      salmon.aes_key = aes_key_hash['key']
      salmon.iv      = aes_key_hash['iv']
      salmon.magic_sig = MagicSigEnvelope.create(user , user.person.aes_encrypt(activity, aes_key_hash))
      salmon
    end
```

before push_to_person is called the salmon object encrypts the headers with person's public key and 
returns the xml of the form:
```xml
    <salmon>
        <encrypted author info>
            author name
            author diaspora_handle
            aes-private-key
        </encrypted author info>
        <data>
            aes encrypted data
        </data>
        <salmon signature>
             On the aes encrypted data
        </salmon signature>
    </salmon>
```
the resulting xml is then POSTed it to the receive hook of the person that is passed in.
```ruby
QUEUE.add_post_request( person.receive_url, xml )
```


## Discovery
Given a diaspora_handle, Diaspora [webfingers](http://webfinger.org/) that server, creates a Person object from the information retrieved, and stores it in the database.  The information from webfinger is a collection of <link> tags. Diaspora uses the webfinger description to find the user's seed URL, the globally unique identifier (guid), and the public encryption key.

```xml
  <Link rel="http://joindiaspora.com/seed_location" type="text/html" href="http://tom.joindiaspora.com/"/>
  <Link rel="http://joindiaspora.com/guid" type="text/html" href="4c97e47634b7da329d000003"/>
  <Link rel="diaspora-public-key" type="RSA" href="LS0...=="/>
```