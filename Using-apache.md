# Using apache instead of thin server on Fedora

Running ruby apps through apache is done using the passenger apache
module. See [[http://www.modrails.com/documentation/Users%20guide.html]]

There is no Fedora passenger rpm ATM, see
[[https://bugzilla.redhat.com/show_bug.cgi?id=470696]]

See also [[Installing on Ubuntu Apache]]

## Install passenger module

To setup apache to run diaspora:

      sudo gem install passenger
      sudo  yum install apr-devel httpd-devel rubygem-rake
      sudo passenger-install-apache2-module

Create a new file /etc/httpd/conf.d/passenger.conf with content as given from
passenger-install-apache2-module. For me, this was:

     LoadModule passenger_module /usr/lib/ruby/gems/1.8/gems/passenger-2.2.15/ext/apache2/mod_passenger.so
     PassengerRoot /usr/lib/ruby/gems/1.8/gems/passenger-2.2.15
     PassengerRuby /usr/bin/ruby

## Make diaspora accessible from apache as a rails rack app

Copy the complete diaspora application to  e. g.,  /usr/local/webapps to
avoid troubles when apache can't access files in your ordinary home dir.
Avoid storing the app under /var/www, seems that rewriting rules becomes
unhappy in some cases then (?)

        cd <parent of diaspora dir>
        sudo mkdir /usr/local/webapps
        sudo mv diaspora /usr/local/webapps
        mkdir /usr/local/webapps/diaspora/tmp  # All rack apps should have a tmp dir.
        chown -R apache /usr/local/webapps/diaspora

Setup the symlink used by RacklBaseURI which also is the url you access diaspora at:

        cd  /var/www/diaspora
        ln -s public diaspora

Install the bundle

        sudo bundle install --system
        mkdir -p devise.git/ruby
        ln -s /usr/lib/ruby/gems/* devise.git/ruby/

## Configure apache server instance

Create a virtual http server for the diaspora app by appending something like this to
/etc/httpd/conf/httpd.conf (using your own servername for host.domain.tld):

    <virtualhost *:8080>
        ServerName     host.domain.tld
        DocumentRoot   /usr/local/webapps/diaspora
        RailsEnv       development
        RackBaseURI    /diaspora    # The link created above
        <Directory /usr/local/webapps/diaspora/public>
            AllowOverride None
            Order         allow,deny
            Allow         from all
        </Directory>
    </virtualhost>


## Run and access server

Restart server using 'server httpd restart'.

Access the server on http://host.domain.tld:8080/diaspora

## Update this document

Fix whatever problems you discovered using this page by editing it!


