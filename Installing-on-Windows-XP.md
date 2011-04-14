### Versions

These instructions are for creating a local development instance on Windows XP Professional Service Pack 3.

### Known Problems

* SSL support is not tested.

### Ruby

Download [RubyInstaller 1.8.7](http://rubyinstaller.org/downloads/).  This also includes RubyGems.

When running the installer check "Add Ruby executables to your PATH."

### Ruby Development Kit

This provides compliation programs for building RubyGems.

Download the [Development Kit](http://rubyinstaller.org/downloads/).

When installing the Development Kit, type `C:\Progra~1\RubyDevKit` in the Extract To field.
Click the Extract button.

Open the command-line by clicking on Start -> All Programs -> Accessories -> Command Prompt

Enter these commands.

        cd "C:\Progra~1\RubyDevKit"
        ruby dk.rb init
        ruby dk.rb install

Close the command prompt window.

### MySQL

Download the Windows (x86, 32-bit), MSI Installer v5.5.10 from [MySQL.com](http://www.mysql.com/downloads/mysql/).  Click on the "No thanks, just take me to the downloads!" link at the bottom of the next web page.

Run the Installer.
Choose the Typical Setup Install Type.

In the Server Instance Configuration Wizard choose the following options:

* Maintance Option: Reconfigure Instace
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

Edit C:\\Program Files\\MySQL\\MySQL Server 5.5\\my.ini

In the mysqld section add

        bind-address=127.0.0.1

Restart MySQL.  From the command line run:

        net stop mysql
        net start mysql

Note:  If you do run the Server Instance Configuration Wizard again you will need to add bind-address=127.0.0.1 to my.ini file.

Open a Command Prompt window and type.

    copy "C:\PROGRA~1\MySQL\MySQL Server 5.5\lib\libmysql.dll" C:\Ruby187\bin

### ImageMagick

Download the Win32 dynamic at 16 bits-per-pixel binary from [ImageMagick.org](http://www.imagemagick.org/script/binary-releases.php#windows).

### Git

Download the full installer from [Google Code](http://code.google.com/p/msysgit/downloads/list).

When installing uncheck:

* Additional icons
* Windows Explorer integration
* "Associate .git*" configuration files

### Redis

Download version 2.2 from [Github](https://github.com/downloads/dmajkic/redis/redis-2.2.2-wi
n32-win64.zip). Save it to the Desktop.

* Right click on the Redis zip file choose Extract All.
* In the Extraction Wizard, click Next twice.
* Uncheck Show extracted files.
* Click Finish.

Open up a Command Prompt and type the following:

    cd %HOMEDRIVE%
    mkdir "C:\Program Files\Redis"
    copy "%HOMEDRIVE%%HOMEPATH%\Desktop\redis-2.2.2-win32-win64\32bit\*" "C:\Program Files\Redis"

Open C:\\Profile Files\\Redis\\redis.conf in a text editor and:

* Change, or un-comment, `#bind 127.0.0.1` to `bind 127.0.0.1`.
* Change `loglevel verbose` to `loglevel notice`.

### Java JRE (Optional)

Browse to the java [website](http://java.com) and click on the Downloads link.  Click on Agree and Start Free Download.  Save the file to your desktop and run it.

* Uncheck Install the Yahoo! Toolbar.

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

* Change `'SystemTimer', '1.2.1'` to `'ghazel-SystemTimer', '1.2.1.1'`
* Add `gem 'win32-open3', '0.3.2'`

Open the Gemfile.lock file.

* Change `SystemTimer (1.2.1)` to `ghazel-SystemTimer (1.2.1.1)`
* Change `SystemTimer (= 1.2.1.1)` to `ghazel-SystemTimer (= 1.2.1.1)`

### Installing Diaspora

The bundle install command will fail while installing the Typhoeus gem due to missing libcurl files.  While in the C:\\Progra~1\\Diaspora directory from the Command Prompt, type:

    C:\Progra~1\RubyDevKit\devkitvars.bat
    bundle install --path vendor

You will see an error message while compiling the Typhoeus gem.  In the example below Typhoeus 0.2.4 dependens on curl 7.19.4.

    checking for curl/curl.h in C:/PROGRA~1/Diaspora/vendor/ruby/1.8/gems/typhoeus-0.2.4/cross/curl-7.19.4.win32/include... no
    need libcurl
    *** extconf.rb failed ***

### Libcurl

Download [libcurl](http://www.gknw.net/mirror/curl/win32/old_releases/).  Find the version the Typhoeus is looking for in the format of curl-&lt;version number&gt;-devel-mingw32.zip.  For Typhoeus 0.2.4, download curl-7.19.4-devel-mingw32.zip.  Save the file to your Desktop.

* Right click on the file and choose Extract All.
* In the Extraction Wizard window, click Next twice.
* Click Finish.

From the Command Prompt run:

    set TYPHOEUS_VER=0.2.4
    set CURL_VER=7.19.4

    mkdir C:\PROGRA~1\Diaspora\vendor\ruby\1.8\gems\typhoeus-%TYPHOEUS_VER%\cross\curl-%CURL_VER%.win32\include
    cd %HOMEDRIVE%

    copy "%HOMEDRIVE%%HOMEPATH%\Desktop\curl-%CURL_VER%-devel-mingw32\curl-%CURL_VER%-devel-mingw32\include\curl" C:\PROGRA~1\Diaspora\vendor\ruby\1.8\gems\typhoeus-%TYPHOEUS_VER%\cross\curl-%CURL_VER%.win32\include
    copy "%HOMEDRIVE%%HOMEPATH%\Desktop\curl-%CURL_VER%-devel-mingw32\curl-%CURL_VER%-devel-mingw32\bin" C:\PROGRA~1\Diaspora\vendor\ruby\1.8\gems\typhoeus-%TYPHOEUS_VER%\cross\curl-%CURL_VER%.win32

    copy "%HOMEDRIVE%%HOMEPATH%\Desktop\curl-%CURL_VER%-devel-mingw32\curl-%CURL_VER%-devel-mingw32\bin\*.dll" C:\Ruby187\bin
    copy "%HOMEDRIVE%%HOMEPATH%\Desktop\curl-%CURL_VER%-devel-mingw32\curl-%CURL_VER%-devel-mingw32\bin\*.dll" C:\Ruby187\bin

Should the Typhoeus and Curl versions change, adjust the Typhoeus and Curl version environment variables used above.

### Installing Diaspora (Continued)

From the command line while in the Diaspora installation directory type:

    C:\Progra~1\RubyDevKit\devkitvars.bat
    bundle install

### Set up the database

Follow the Set up the database section on the main Installing and Running Diaspora page.  Remember to put the root MySQL password into the test and development password sections of database.yml.

### Sending E-mail (optional)

To enable password resets and notifications you will need to install a program capabile of sending e-mail to a mail server.  Do not send your friends invites from your Windows installation as it is not configured to allow connections from the Internet.

This will use your send e-mails from  your home e-mail account or other e-mail server that you prefer.  You will need its connection information before proceeding.

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

    mkdir "C:\\Progra~1\\Diaspora\\software"
    copy "%HOMEDRIVE%\\%HOMEPATH%\\Desktop\\msmtp-1.4.23-w32\\msmtp-1.4.23-w32\\msmtp.exe" C:\\Progra~1\\Diaspora\\software

Edit `config/app_config.yml`.  If `config/app_config.yml` does not exist, save a copy of `config/app_config.yml.example` to `app_config.yml`.

* Change `mailer_on: false` to `mailer_on: true`
* Change sendmail_location from `sendmail_location: '/usr/sbin/sendmail'` to `sendmail_location: 'C:/Progra~1/Diaspora/software/msmtp'`
* Change the entries that begin with `smtp_` to match that of your e-mail server.

### Windows Compatibility Modifications

#### Resque

The resque workers rely on the Unix `ps` command to determine which programs are running.  We will need to modify the source code to use the Windows `tasklist` command instead.  Note: If the version of the resque gem changes you will need to reapply these modifications.

Open vendor\\ruby\\1.8\\gems\\resque-1.10.0\\lib\\resque\\worker.rb

Look for def `kill_child` in that file.  Locate the line that reads

    if system("ps -o pid,state -p #{@child}")

and replace it with

    if system('tasklist /fi "PID eq #{@child}"')

Look for `def worker_pids`

Replace this code block

    def worker_pids
      `ps -A -o pid,command | grep [r]esque`.split("\n").map do |line|
        line.split(' ')[0]
      end
    end

with this one

    def worker_pids
      `tasklist /fi "Services eq resque"`.split("\n").map do |line|
        line.split(' ')[1]
      end
    end

### Running Diaspora

Create a batch file named server.bat in C:\\Progra~1\\Diaspora\\script.  Put this into the batch file.

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

*  When Redis is started.  If this happens close out the Windows Firewall window and edit C:\\Progra~1\\Redis\\redis.conf.  Uncomment the line that says `bind 127.0.0.1` and start Redis again.
*  When the websocket loads a Windows Firewall dialog will appear.  Click the Unblock button.  The websocket will need to communicate with external servers/clients.

### Create an Account

Browse to http://localhost:3000/users/sign_up