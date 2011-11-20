# How to Use Pagekite to Link a Domain Name to a Diaspora Pod on a Local Network Computer

Diaspora is an open-source, decentralized social network. You can find instructions for installing Diaspora on your computer [here](https://github.com/diaspora/diaspora/wiki/Installing-and-Running-Diaspora/). A Diaspora server is called a pod. Each user account within a pod is called a seed. The various pods communicate with one another, collectively creating the Diaspora network.

Once you've installed and correctly configured Diaspora, you should have a valid SSL certificate, the thin app server listening on localhost:3000 and the nginx webserver listening on localhost:443. But the nginx webserver is not visible to the internet yet, because it is likely hidden behind NAT IP addresses assigned by your ISP and your local area network. That is to say, your nginx webserver's IP address likely is something like 192.168.2.6:443, which is not an address reachable from other computers on the internet. [Pagekite](http://pagekite.net) is a simple way to make the nginx webserver visible to the internet. Here's how (substitute your particulars for the examples in the brackets below):

## 1. Create a Domain Name

Go to GoDaddy, VeriSign, or the like, and register a domain, for example, [_yourdomain.net_].

## 2. Create a Pagekite Account

Open an account at [Pagekite](http://pagekite.net), and create a pagekite, for example, [_yourname_].pagekite.me. Download the pagekite software and install it. Make note of where the configuration files are located on your computer afterward. For example, on a computer with linux as its operating system, the configuration files may be found at /etc/pagekite.d/.

## 3. Create a CNAME Record

Go back to your domain registrar, log in to your domain name account, and create a CNAME record that points to your pagekite. For example, create the CNAME [_diaspora.yourdomain.net_], and point it to [_yourname_].pagekite.me. Your domain registrar will have instructions on how to do this. The process will vary by domain registrar.

## 4. Edit the Pagekite Configuration Files

Now you want to direct the cNAME you created, [_diaspora.yourdomain.net_], through your [_yourname_].pagekite.me kite, to your local computer, where your nginx webserver is listening at port 443. Using a text editor, edit your pagekite configuration files as follows.  The file /etc/pagekite.d/10_account.rc should contain the following values:

kitename=[_diaspora.yourdomain.net_]
kitesecret=[_your account secret from your pagekite account_]

The file /etc/pagekite.d/80_httpd.rc should contain the following:

backend=http:[_yourname_].pagekite.me:localhost:443:@kitesecret
backend=http:@kitename:localhost:443:@kitesecret

## 5. Test

If your pod is running, and you've started the pagekite service (for example, using the command "service pagekite start" in a terminal as su or sudo), you should now be able to find your login screen in a browser window at https://[_diaspora.yourdomain.net_].

## 6. Relocating Your Pod

If you change local network locations (for example, you take the laptop hosting your pod to an internet cafe), you'll need to restart Pagekite to update your DNS settings. Simply open a terminal and type "service pagekite restart" as su or sudo.