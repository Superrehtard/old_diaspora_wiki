**Using "Git for Windows" to deploy your Diaspora test "fork" from GitHub to Engineyard.**

For a few reasons this environment i recommend only for testing.
 - it is a service intended for developers 
 - the disk storage is non-persistent
 - they do not offer or host SMTP services
 - cost of data transfer and backups in production would be non-competitive


**1)** First setup a free account at GitHub (git and github can be confusing at first, don't worry, keep going.)
http://www.github.com

**2)** Then download and install the latest version stable of GitHub for Windows. At the time of writing it is 1.0.7

http://windows.github.com/

**3)** Sign up for the Engineyard Cloud Free trial at http://www.engineyard.com/. Create your virtual server environment and application instance using default values. Boot the services with default values and use the one click install to install a self signed SSL certificate for testing. START by installing and testing the provided ToDo application and make sure that all works.

**4)** Go to https://github.com/diaspora/diaspora and make your own "fork" of the Diaspora code at GitHub. A fork is just your own copy of the code with version dependencies attached. Then use the "clone" button to copy your forked repository to your local PC.

**5)** DEPLOY KEYS. When you create your git application at engineyard and specify the github address it will walk you through the produce of creating and returning a deploy key for github. When things get confusing you can always nuke everything and start again... carry on... have fun...

**6)** Before you deploy there are just a few files that need to be created to configure your default Diaspora code to your engineyard cloud environment. They are "application.yml" and "database.yml" which are NOT found in the 'config' directory as part of the git package. 

To create them, make SURE that Git for windows is running, use it to open an explore to your repository. Open the provided application.yml.example file, make the needed changes to the URL and Pod Name etc, then save the file  without the ".example" extension. Do the same with database.yml. No changes need to be made to the default file database.yml in this case. Then "commit" and "sync" in Git for windows.

**7)** At engineyard you should now be able to use the [DEPLOY] button and run Diaspora in "development mode" on your booted application instance. Check the box that says to Rake and Migrate the DB. This will create an empty database for the app.

**8)** Go to your URL or IP address and you should see a working Diaspora application. You may have some browser warnings about SSL and some problems with connecting to other pods due to the self signed certificate. But you should be able to search for and display members on remote pods.

If this works then you can install a free Class 1 SSL cert from various providers (such as  http://www.startssl.com/ or http://www.sslshopper.com/ just for example) for a more complete testing of Diaspora's features. [None of the Class 1 solutions are right for production. StartSSL with nginx seems to be functional for testing with work around. http://blog.dembowski.net/2010/02/25/startssl-and-nginx/]

**9)** You probably ran into problem along the way... that's normal... nuke everything... do it again... and again... and again... cause it will work in the end. :)

MAKE SOMETHING! Keep up to date with code changes on GitHub by following the project members. If you find information or useful tips or links please add them here! If you find bugs please contribute to the code!