## How We Do It

We deploy and run Diaspora with a deployment tool called sod, which currently only supports CentOS.  We use Rackspace Cloud, but you can point sod at any CentOS machine.  Sod is unfinished and probably has hardcoded configuration for our servers.  It is **not ready for use**.  DO NOT use a machine that other apps are running on, Sod assumes that it is deploying onto a clean machine.  So first you get yourself an ip and root password to a CentOS machine.  Then get yourself an SSL cert and put it in ~/diaspora_cert on your local machine.  Then you run sod on your local machine to provision the remote server:

        # install ruby 1.8.7, git, bundler
        git clone git://github.com/MikeSofaer/sod.git
        cd sod
        bundle install
        ./sod <remote_machine_ip_address> diaspora diaspora <remote_machine_root_password>

This should take about 40 minutes, and when it's done Diaspora should be running.  You will need to edit config files, etc.

Warning: The version of splunk installed this way only supports 64-bit processors. To work around this, download the 32-bit RPM from the splunk website:

        rpm -Uvh splunk-4.1.6-89596.i386.rpm;  cd /usr/local/bin/;  rm splunk;  ln -s /opt/splunk/bin/splunk .

On a minimal install system you'll need to install bzip2 and vixie-cron for this to work (yum install bzip2 vixie-cron)

For localhost you can skip the SSL cert and just use 127.0.0.1 for the IP and your root password.

To restart the appservers after making a change, ssh in and type `svc -t /service/thin*`

**Warning:** By default, the MySQL-root-user has no password. You should set up one. To do that, start a mysql-shell for the root-user (normally you can do that with `mysql -u root`) and type `SET PASSWORD = PASSWORD('some password');`.
