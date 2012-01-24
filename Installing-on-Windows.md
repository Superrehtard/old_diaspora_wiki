### Versions

These instructions are for creating a local development instance on Windows XP Professional Service Pack 3 and Windows 7.

### Known Problems

* SSL support is not tested.
* Time required to start the application is slow.  This appears to be a common problem in running Ruby programs in Windows.

### Ruby

Download [RubyInstaller 1.9.3](http://rubyinstaller.org/downloads/).  This also includes RubyGems.

When running the installer check "Add Ruby executables to your PATH."

Add `C:\Ruby193\lib` to your system path.  You can temporarily set this by running `set PATH=%PATH%;C:\Ruby193\lib` in a command-line window.

### Ruby Development Kit

This provides compilation programs for building RubyGems.

Download the [Development Kit](http://rubyinstaller.org/downloads/).

When installing the Development Kit, type `C:\Progra~1\RubyDevKit` in the Extract To field.
Click the Extract button.

Open the command-line by clicking on Start -> All Programs -> Accessories -> Command Prompt

Enter these commands.

        cd "C:\Progra~1\RubyDevKit"
        ruby dk.rb init

This generates config.yml. Make sure that config.yml points to the root of Ruby installation:

    - C:/Ruby193

After this run:

        ruby dk.rb install

Close the command prompt window.

### MySQL

Download the Windows (x86, 32-bit), MSI Installer for the latest 5.5 version from [MySQL.com](http://www.mysql.com/downloads/mysql/).  Click on the "No thanks, just take me to the downloads!" link at the bottom of the next web page.

(Windows 7 note:  You can use the 64-bit version of MySQL; however, you will need to also download the 32-bit version for building the MySQL ruby gem.)

Run the Installer.
Choose the Typical Setup Install Type.

In the Server Instance Configuration Wizard choose the following options:

* Maintenance Option: Reconfigure Instance
* Configuration Type: Detailed Configuration
* Server Type: Developer Machine
* Database Usage: Multifunctional Database
* Concurrent Connections: Decision Support
	* Enable TCP/IP Networking
* Character Set: Best Support for Multilingualism
* Include Bin Directory in Windows PATH
* Security Options: Set a root password
	* Make sure that Enable root access from remote machine is unchecked

If you need to run the Server Instance Configuration Wizard at a later time, from the command line run:

        MySQLInstanceConfig

Edit C:\Program Files\MySQL\MySQL Server 5.5\my.ini

In the mysqld section add

        bind-address=127.0.0.1

Restart MySQL.  From the command line run:

        net stop mysql
        net start mysql

Note:  If you do run the Server Instance Configuration Wizard again you will need to re-add bind-address=127.0.0.1 to the my.ini file.

Open a Command Prompt window and type:

    copy "C:\PROGRA~1\MySQL\MySQL Server 5.5\lib\libmysql.dll" C:\Ruby193\lib

If during the further steps libmysql.dll is not found, copy it also to C:\Ruby193\bin

During the further steps you may get notification that you need a new version of libmysql.dll (MySQL C connector). In this case go to [mysql.com](http://www.mysql.com/downloads/connector/c/), download zip. From the zip you only need libmysql.dll

### ImageMagick

Download and install the Win32 dynamic at 16 bits-per-pixel binary from [ImageMagick.org](http://www.imagemagick.org/script/binary-releases.php#windows).

### Git

Download the full installer from [Google Code](http://code.google.com/p/msysgit/downloads/list).

When installing uncheck:

* Additional icons
* Windows Explorer integration
* "Associate .git*" configuration files
* In the Git Setup window choose "Run Git from the Windows Command Prompt."

### Redis

Download version 2.4 from [Github](https://github.com/dmajkic/redis/downloads). Save redis-2.4.2-win32-win64-fix to your Desktop.

* Right-click on the Redis zip file and choose Extract All.
* In the Extraction Wizard, click Next twice.
* Uncheck Show extracted files.
* Click Finish.

Open up a Command Prompt and type the following:

    cd %HOMEDRIVE%
    mkdir "C:\Program Files\Redis"
    copy "%HOMEDRIVE%%HOMEPATH%\Desktop\redis-2.4.2-win32-win64-fix\32bit\*" "C:\Program Files\Redis"

Open C:\Program Files\Redis\redis.conf in a text editor and:

* Change, or un-comment, `#bind 127.0.0.1` to `bind 127.0.0.1`.
* Change `loglevel verbose` to `loglevel notice`.

### Java JRE (Optional)

Browse to the java [website](http://java.com) and click on the Downloads link.  Click on Agree and Start Free Download.  Save the file to your desktop and run it.

### Bundler

Install the bundler gem.

    gem install bundler

### Getting Diaspora

Create a directory for Diaspora.

    mkdir "C:\Program Files\Diaspora"

Some programs do not like spaces in the current working directory.

    cd "C:\Progra~1\Diaspora"

Get the source code.

    git init
    git remote add origin https://github.com/diaspora/diaspora.git
    git pull origin master
   
Two windows specific gems need to be included.  Open the Gemfile file.

* Add `gem 'win32-open3-19', '0.0.1'`

Open the Gemfile.lock file.

* Change `eventmachine (>= 0.12.9)` to `eventmachine (>= 1.0.0.beta.2)`
* Change `eventmachine (0.12.10)` to `eventmachine (1.0.0.beta.2)`

### Libcurl

The Typhoeus gem depends on libcurl, and this provides a work-around approach of building the gem on Windows:

Download curl-7.19.4-devel-mingw32.zip from [libcurl](http://www.gknw.net/mirror/curl/win32/old_releases/) and save the file to your Desktop.

* Right click on the file and choose Extract All.
* In the Extraction Wizard window, click Next twice.
* Click Finish.

From the Command Prompt run:

    set CURL_VER=7.19.4

    mkdir C:\Ruby192\include\ruby-1.9.1\curl

    copy "%HOMEDRIVE%%HOMEPATH%\Desktop\curl-%CURL_VER%-devel-mingw32\curl-%CURL_VER%-devel-mingw32\bin\*.dll" C:\Ruby192\lib
    copy "%HOMEDRIVE%%HOMEPATH%\Desktop\curl-%CURL_VER%-devel-mingw32\curl-%CURL_VER%-devel-mingw32\include\curl\*.*" C:\Ruby192\include\ruby-1.9.1\curl
    
### Installing Diaspora

From the command line, type:  

    cd "C:\Progra~1\Diaspora"  
    C:\Progra~1\RubyDevKit\devkitvars.bat

    bundle config build.mysql2 '--with-mysql-include="C:\Progra~1\MySQL\MySQL Server 5.5\include"'  
    bundle install --path vendor

### Set up the database

Follow the Set up the database section on the main Installing and Running Diaspora page.  Remember to put the root MySQL password into the common section of database.yml.

### Windows Compatibility

Edit config/application.yml

Change `redis_url: ''` to `redis_url: 'localhost'`

### Sending E-mail (optional)

To enable password resets and notifications you will need to install a program capable of sending e-mail to a mail server.  Do not send your friends invites from your Windows installation as it is not configured to allow connections from the Internet.

This will send e-mails from  your home e-mail account or other e-mail server that you prefer.  You will need its connection information before proceeding.

Download [msmtp](http://msmtp.sourceforge.net/)

* Click on Download.
* Click on the SourceForge link under Releases.
* Click on the Download button.  Save the file to your Desktop.
* Right click on the saved file.
* Choose Extract All.
* Click Next twice.
* Uncheck show extracted files.
* Click Finish.

In a Command Prompt type

    mkdir "C:\Progra~1\Diaspora\software"
    copy "%HOMEDRIVE%\%HOMEPATH%\Desktop\msmtp-1.4.23-w32\msmtp-1.4.23-w32\msmtp.exe" C:\Progra~1\Diaspora\software

Edit `config/app.yml`.  If `config/app.yml` does not exist, save a copy of `config/app.yml.example` to `app.yml`.

* Change `mailer_on: false` to `mailer_on: true`
* Change sendmail_location from `sendmail_location: '/usr/sbin/sendmail'` to `sendmail_location: 'C:/Progra~1/Diaspora/software/msmtp'`
* Change the entries that begin with `smtp_` to match that of your e-mail server.

### Running Diaspora

Create a batch file named server.bat in C:\Progra~1\Diaspora\script.  Put this into the batch file.

    net start mysql

    set DIASPORA_PATH=C:\Progra~1\Diaspora
    set RAILS_ENV=development
    set REDIS_PATH=C:\Progra~1\Redis
    set QUEUE=*

    cd %REDIS_PATH%
    start redis-server redis.conf

    cd %DIASPORA_PATH%
    start bundle exec ruby .\script\websocket_server.rb
    start bundle exec rake resque:work
    bundle exec thin -e %RAILS_ENV% -a 127.0.0.1 -p 3000 start

Start the server and needed processes by typing.

    c:\Progra~1\Diaspora\script\server.bat

This will spawn three additional Command Prompt windows.
    
A Windows Firewall dialog will appear in the following situations:

*  When Redis is started.  If this happens close out the Windows Firewall window and edit C:\Progra~1\Redis\redis.conf.  Uncomment the line that says `bind 127.0.0.1` and start Redis again.
*  When the websocket loads a Windows Firewall dialog will appear.  Click the Unblock button.  The websocket will need to communicate with external servers/clients.

### Create an Account

Browse to http://localhost:3000/users/sign_up

## Notes from a developer
I had to copy `libmysql.dll` to the ruby bin directory, rather than the lib directory. Same for `libcurl.dll`.