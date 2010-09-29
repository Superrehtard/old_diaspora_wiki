You should look into [[Handling bug 360]]

You should walk around [bug 360](http://github.com/diaspora/diaspora/issues/issue/360). This is about getting a fast, direct negatuve answer
when trying to access port 443 i. e., the https port. In this section, I presume your host is located behind a router which you have full
access to.

First check the current situation.Note use of the external address, the address used by the outside world. In most cases, this is your 
router's external hostname

    telnet host.example.com 443

.One possible reply is this, coming within a few seconds:

    telnet host.example.com 443
    Trying 85.230.51.222...
    telnet: connect to address 85.230.51.222: Connection refused

If so, everything is OK. Otherwise, a typical situation is

    telnet host.example.com 443
    Trying 85.230.51.222...
    telnet: connect to address 85.230.51.222: Connection timed out

The first attempt to fix might be to let the router block port 443. If it works (no timeout) it's fine. Otherwise, connect port
443 on your router with port 443 on your host. Then create a file /etc/sysconfig/iptables-https like this:

    -A INPUT -p tcp -m tcp --dport 443 -j REJECT --reject-with tcp-reset

Start system-config-firewall. Check that https is unchecked in the list of Trusted Services. Then click "Custom Rules" in 
leftmost window. Click Add, select Protocol Type 'ipv4' and  Firewall Table to 'Filter'. Select the file /etc/sysconfig/iptables-https. 
Click OK, and Apply. Done!

At this point you should try with telnet again. If you don't get a "Connection refused" I really don't know what to do.
