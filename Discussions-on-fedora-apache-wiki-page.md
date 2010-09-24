This seems like it overlaps with installing on ubuntu/apache, so let's get rid of one.  -- Raphael Sofaer

Basically, three issues:

-  Is it possible to use apache at all?  According to [[Installing and Running Diaspora]] the event machine required does not work w passenger + apache. Or?
- This does partly overlaps, but each OS has their own conventions about packaging, file names etc.  The only real overlap here is the
  apache configuration IMHO. 
- Where should the discussion be on this topic? Here? Mailing list? Open an issue? Or 
 
--alec leamas

=MSofaer
The page puts Apache and Thin at the same level which is crazy.  Apache is an nginx replacement, not a Thin replacement.
Passenger could be a This replacement, but making it work will be hard.
Replacing nginx with apache2 would be very straightforward, as long as you stick with Thin.
People should try this:  
http://articles.slicehost.com/2008/5/6/ubuntu-hardy-apache-rails-and-thin
=end
