
Note: If any of the steps don't work, don't force it.  You probably skipped a step or something.  Do not use sudo unless mentioned.
Note: If you have issue, try here: https://github.com/diaspora/diaspora/wiki/How-we-use-IRC

# THIS IS ONLY FOR DEV SETUP OR FOR JUST TRYING IT OUT

# THIS GUIDE HAS SOME FLAWS AND IS CONFUSING, IF SOMEBODY WANTS TO REWORK IT.... IN THE MEAN TIME JUST GET A VM, INSTALL UBUNTU AND THROW THE QUICK START AGAINST IT: https://github.com/diaspora/diaspora#quick-start

# WEIRD GUIDE STARTS HERE

## VirtualBox Setup:

	Name: 
	Diaspora2
	OS Type: 
	Ubuntu
	Base Memory: 
	512 MB
	Start-up Disk: 
	Diaspora2.vdi (Normal, 8.00 GB)


	File type: 
	VDI (VirtualBox Disk Image)
	Details: 
	Dynamically allocated storage
	Location: 
	C:\Users\dfolkes\VirtualBox VMs\Diaspora2\Diaspora2.vdi
	Size: 
	8.00 GB (8589934592 B)

## VBOX SETTINGS:
	Storage > IDE :
	-- attach ubuntu 12.04 server

	NETWORK > Adapter1:
		NAT
	NETWORK > Adapter2:
		Host-only Adapter

## UBUNTU INSTALL:
Use the defaults for everything unless specified.

	Configure Network:
		- eth0
	Write Changes to disk?
		- yes
	
	Configuring Taskel:
		Install security updates automatically
	
	Software Selection:
		OpenSSH
		LAMP
	
	MySQL Password:
		MYSQLPASS

Install Complete, restarting...

## POST INSTALL CHECKUP
	sudo dhclient eth1
	ping google.com
should get response

	ifconfig

should get an ip address in eth1, mine is: 192.168.56.103
	
open http://YOUR.IP.ADDRESS/ in your browser.
if it does not say 'it works', then your network is messed up and I am sorry.

## SSH into the box
(mine:192.168.56.103) This will allow you to copy and paste.  If you are in windows, use Putty.

We are going to be doing this from Step 1 to Step 4
http://www.akadia.com/services/ssh_test_certificate.html

Finish with:

	sudo cp server.key /etc/apache2/
	sudo cp server.crt /etc/apache2/

Replace /etc/apache2/sites-enabled/000-default with:

	<VirtualHost *:443>
        DocumentRoot /var/www
        <Directory /var/www/>
                Options Indexes FollowSymLinks MultiViews
                AllowOverride None
                Order allow,deny
                allow from all
        </Directory>
		RewriteEngine On
		SSLEngine On
		SSLCertificateFile /etc/apache2/server.crt
		SSLCertificateKeyFile /etc/apache2/server.key
	</VirtualHost>

run:

	sudo a2enmod rewrite
	sudo a2enmod ssl
	sudo /etc/init.d/apache2 restart

Go here and proceed through ssl errors:
https://YOUR.IP.ADDRESS/
It should say: "it works".

## Installing Diaspora's Ruby Environment:
From: https://github.com/diaspora/diaspora/wiki/Installing-on-Ubuntu
	
	sudo apt-get install build-essential libxslt1.1 libxslt1-dev libxml2 ruby-full mysql-server libmysqlclient-dev libmysql-ruby libssl-dev libopenssl-ruby libcurl4-openssl-dev imagemagick libmagickwand-dev git-core redis-server libffi-dev libffi-ruby rubygems libsqlite3-dev libpq-dev libreadline5 openjdk-7-jre nodejs libncurses5-dev
	
	curl -L dspr.tk/1t | bash
	source /home/dan/.rvm/scripts/rvm
	
	rvm install 1.9.3-p385

press 'q' to continue...
	
	rvm use 1.9.3-p385@global

## Installing Diaspora:
FROM: https://github.com/diaspora/diaspora/wiki/Notes-on-Installing-and-Running-Diaspora

	git clone -b master git://github.com/diaspora/diaspora.git && cd diaspora
	Do you wish to trust this .rvmrc file? 
	y [enter]

	bundle install

	cp ./config/script_server.yml.example ./config/script_server.yml
	cp ./config/diaspora.yml.example ./config/diaspora.yml
	cp ./config/database.yml.example ./config/database.yml

Edit: ./config/script_server.yml

	rails_env: "development"


Edit ./config/diaspora.yml

	certificate_authorities: '/etc/apache2/server.crt'


	
Edit ./config/database.yml

	mysql: &mysql
		 password: "MYSQLPASS"


Build up the database:

	bundle exec rake db:create
	RAILS_ENV=production bundle exec rake db:create

	bundle exec rake db:schema:load
	RAILS_ENV=production bundle exec rake db:schema:load

## Configure Apache
Replace /etc/apache2/sites-enabled/000-default with:
> Replace /home/dan/diaspora/public with your path to the folder.

	<VirtualHost *:443>
		DocumentRoot /home/dan/diaspora/public
		
		RewriteEngine On
		RewriteCond %{DOCUMENT_ROOT}/%{REQUEST_FILENAME} !-f
		RewriteRule ^/(.*)$ balancer://thinservers%{REQUEST_URI} [P,QSA,L]
		
		<Proxy balancer://thinservers>
			BalancerMember http://127.0.0.1:3000
		</Proxy>

		ProxyRequests Off
		ProxyVia On  
		ProxyPreserveHost On
		RequestHeader set X_FORWARDED_PROTO https

		<Proxy *>
			Order allow,deny
			Allow from all
		</Proxy>
		
		<Directory /home/dan/diaspora/public>
			Allow from all
			AllowOverride all
			Options -MultiViews
		</Directory>
		
		SSLEngine On
		SSLCertificateFile /etc/apache2/server.crt
		SSLCertificateKeyFile /etc/apache2/server.key
	</VirtualHost>
	
Add apache mods and restart:

	sudo a2enmod proxy proxy_balancer proxy_http headers
	sudo /etc/init.d/apache2 restart

https://YOUR.IP.ADDRESS/ = SERVICE UNAVAILABLE

	./script/server

https://YOUR.IP.ADDRESS/ = Diaspora!
