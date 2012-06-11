This wiki explains using "Git for Windows" to deploy your Diaspora test "fork" to EngineYard.
<br>

For a few reasons this environment i recommend only for testing.
 - it is a service intended for developers 
 - the disk storage is non-persistent
 - they do not offer or host SMTP services

<br>

**1)** **GitHub:** First setup a free account at GitHub. GitHub is a popular git repository. Git and GitHub can be confusing at first, don't worry, keep going.
http://www.github.com
<br><br>

**2)** **Git for Windows:** Then download and install the latest version stable of GitHub for Windows. At the time of writing it is 1.0.7
http://windows.github.com/. Any Git for Windows will do, but GitHub for Windows is pre-configured for GitHub.
<br><br>

**3)** **Cloud Server:** Sign up for the EngineYard Cloud Free (Ruby) trial at http://www.engineyard.com/. Create your virtual server environment and application instance using default values. Boot the services with default values and use the one click install to install a self signed SSL certificate for testing. START by installing and testing the provided ToDo application and make sure that all works.
<br><br>

**4)** **Diaspora:** Go to https://github.com/diaspora/diaspora and make your own "fork" of the Diaspora code at GitHub. A fork is just your own copy of the code with version dependencies attached. Then use the [Clone] button to copy your forked repository to your local PC.
<br><br>

**5)** **Deploy KEYS:** When you create a git application at engineyard and specify the github address it will walk you through the process of creating and returning a deploy key from github. When things get confusing you can always nuke everything and start again... carry on... have fun...
<br><br>

**6)** **Customize:** Before you deploy there are just a few files that need to be created to configure your default Diaspora code to your engineyard cloud environment. They are "_application.yml_" and "_database.yml_" which are NOT found in the "_config_" directory as part of the git package.  To create them, make SURE that Git for Windows is running, use it to open an explore to your repository. Open the provided _application.yml.example_ file, make the needed changes to the URL and Pod Name etc, then save the file  without the ".example" extension. Do the same with _database.yml._ No changes need to be made to the default file _database.yml_ in this case. Then "commit" and "sync" as needed in Git for windows. Make sure it shows up as 'in sync' before you continue.

For image re-sizing to work properly on EngineYard you must set single_process_mode: true in your config/application.yml. Otherwise thumb nail images will retain the original data size, which is not good. EngineYard does not seem to run background task in a separate process. You must run the re-size task within the request cycle, but performance by the cloud services seems good enough for testing without.
<br><br>

**7)** **Deploy:** At engineyard you should now be able to use the [DEPLOY] button and run Diaspora in "development mode" on your booted application instance. Check the box that says to Rake and Migrate the DB. This will create an empty database for the app.
<br><br>

**8)** **Test:** Go to your URL or IP address and you should see a working Diaspora application. You may have some browser warnings about SSL and some problems with connecting to other pods due to the self signed certificate. But you should be able to find and display known members of remote pods by handle `name@pod.com` from the search box. If that works then you can install a free Class 1 SSL cert from various providers (such as  startssl.com or sslshopper.com just for example) for a more complete testing of Diaspora's features.
<br><br>

You probably ran into some problem along the way... that's normal... nuke everything... do it again... and again... and again... cause it will work in the end. It's a great free way to get familiar with the ropes. When your ready you can setup for deployment of a small pod on your own linux box, or even the deployment of a tier pod on a production host. Good luck! :)
<br><br>

MAKE SOMETHING! Keep up to date with code changes on GitHub by following the project members. If you find errors or missing information or useful tips or links please add them here! Explore code, pull updates, make changes and redeploy your fork! If you find bugs or have suggestions please contribute!

[Getting Started With Contributing](https://github.com/diaspora/diaspora/wiki/Getting-Started-With-Contributing)
<br><br><br>