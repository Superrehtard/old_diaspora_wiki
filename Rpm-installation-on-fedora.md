# Rpm installation on Fedora

This document describes how to install diaspora on Fedora 13 and 14. It supplements the
ordinary README.md  at [[http://github.com/diaspora/diaspora/blob/master/README.md]]

There is a thread about this doc in [[http://forum.conni.ca/index.php/topic,6.0.html]]

## General

I have opted for using rpm packages where it's feasible.  The overall situation for Fedora 13 is:

- Ruby 1.8.7 - As of now, available in rawhide (see below).
- Rubygems - In repo, but needs to be rebuilt to use ruby-1.8.7.
- Everything else: available.

For Fedora 14, everything is available in  standard repositories.

## Preparing your system

In order to run Diaspora, you will need to install the following dependencies:

    sudo yum install mongodb-server openssl ImageMagick git libxslt-devel libxml2-devel 

Ensure that mongod is started at system boot:

    sudo chkconfig mongod on

##  Ruby 1.8.7 installation 

For Fedora 14, just install the ruby packages and proceed to "Walk around bug 360":
    sudo yum install  ruby-libs ruby-devel ruby-irb ruby-rdoc rubygems rake

In order to update Fedora 13  ruby to 1.8.7, you need to know the packages currently
using ruby on your box:

    sudo yum remove ruby

(don't worry, nothing will be removed unless you approve it). You will be presented a
list of packages that yum will remove if ruby is removed. If this acceptable, answer
'yes' and proceed. Otherwise, you have some options:

- Wait until ruby-1.8.7  possibly becomes available in Fedora-13. See
  [[https://bugzilla.redhat.com/show_bug.cgi?id=602435]]
- Upgrade to Fedora 14 (which has ruby 1.8.7 as default version).
- Use the instructions in  [README.md](http://github.com/diaspora/diaspora/blob/master/README.md)
   to install Ruby 1.8.7 in /usr/local, using it in parallell with rpm package 1.8.6.

Keep a note of the packages removed, it's possible to restore the
system by reinstalling them.

## Rebuilding rawhide ruby and rubygem

From now, let's assume that ruby is removed. The ruby-1.8.7 package in rawhide is
not really what we want to install, it will pull in all the rawhide dependencies
and make the system unstable. Instead, lets download the and rebuild the
packages for Fedora 13.

if you don't want to rebuild your packages, pre-built rpm:s built according to these instructions
are available at [[https://github.com/leamas/diaspora/downloads]]

Before doing this, setup a personal build environment so you can build the packages as
ordinary user. If you hav'nt done this before, first install rpmbuild:
      sudo yum install rpmdevtools

an then setup a personal build tree:
     rpmdev-setuptree

Now, download and rebuild ruby:

        sudo yum install yum-utils fedora-release-rawhide
        yumdownloader --enablerepo=rawhide --source ruby rubygems
        sudo yum install $( rpm -qRp ruby-*src.rpm | grep -v rpmlib) gcc-g++
        rpmbuild --rebuild ruby-*src.rpm

rpmbuild will create ruby, ruby-libs, ruby-devel, ruby-rdoc and ruby-irb packages. Look
for the last lines of output, which will show the exact location. For me, installation
boils down to:

       cd ~
       sudo yum localinstall --nogpgcheck   \
                        rpmbuild/rpms/i686/ruby-1.8.7.302-1.fc13.i686.rpm         \
                        rpmbuild/rpms/i686/ruby-libs-1.8.7.302-1.fc13.i686.rpm    \
                        rpmbuild/rpms/i686/ruby-devel-1.8.7.302-1.fc13.i686.rpm   \
                        rpmbuild/rpms/noarch/ruby-irb-1.8.7.302-1.fc13.noarch.rpm \
                        rpmbuild/rpms/noarch/ruby-rdoc-1.8.7.302-1.fc13.noarch.rpm

Rebuild rubygems in the same way:

       rpmbuild --rebuild rubygems-*src.rpm
       sudo yum localinstall --nogpgcheck rpmbuild/rpms/noarch/rubygems-1.3.7-1.fc13.noarch.rpm

## Walk around bug 360

You should look into [[Handling bug 360]]

## Getting,running and testing diaspora

From this point, you should proceed from  "Bundler" in [README.md](http://github.com/diaspora/diaspora/blob/master/README.md)

Some notes on using apache instead of the thin webserver are in   [[Using apache]]


