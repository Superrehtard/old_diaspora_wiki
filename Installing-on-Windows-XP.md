### Versions

These instructions are for creating a local development instance on Windows XP Professional Service Pack 3.

(This guide is currently a work in progress and is not complete.)

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

Edit C:\Program Files\MySQL\MySQL Server 5.5\my.ini

In the mysqld section add

        bind-address=127.0.0.1

Restart MySQL.  From the command line run:

        net stop mysql
        net start mysql

Note:  If you do run the Server Instance Configuration Wizard again you will need to add bind-address=127.0.0.1 to my.ini file.

Open a Command Prompt window and type.

    copy "C:\PROGRA~1\MySQL\MySQL Server 5.5\lib\libmysql.dll" C:\Ruby187\bin

### ImageMagicK

Download the Win32 dynamic at 16 bits-per-pixel binary from [ImageMagicK.org](http://www.imagemagick.org/script/binary-releases.php#windows).

### Git

Download the full installer from [Google Code](http://code.google.com/p/msysgit/downloads/list).

When installing uncheck:

    Additional icons
    Windows Explorer integration
    "Associate .git*" configuration files

### Redis

Download version 2.2 from [Github](https://github.com/downloads/dmajkic/redis/redis-2.2.2-wi
n32-win64.zip). Save it to the Desktop.

    Right click on the Redis zip file choose Extract All.
    In the Extraction Wizard, click Next twice.
    Uncheck Show extracted files.
    Click Finish.

Open up a Command Prompt and type the following:

    cd %HOMEDRIVE%
    mkdir "C:\Program Files\Redis"
    copy "%HOMEDRIVE%%HOMEPATH%\Desktop\redis-2.2.2-win32-win64\32bit\*" "C:\Program Files\Redis"

Open C:\Profile Files\Redis\redis.conf in a text editor and:

    Un-comment the bind 127.0.0.1 line.
    Change the loglevel notice.

### Bundler

Install the bundler gem.

    gem install bundler

### Getting Diaspora

Create a directory for Diaspora.

    mkdir "C:\Program Files\Diaspora"

Some programs do not like spaces in the current working directory.

    cd "C:\Progra~1\Diaspora"

### Get the source code.

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

The bundle install command will fail while installing the Typhoeus gem due to missing libcurl files.  While in the C:\Progra~1\Diaspora directory from the Command Prompt, type:

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

From the Command Prompt and in the Diaspora installation directory run:

    C:\Progra~1\RubyDevKit\devkitvars.bat
    set TYPHOEUS_VER=0.2.4
    set CURL_VER=7.19.4

    mkdir C:\PROGRA~1\Diaspora\vendor\ruby\1.8\gems\typhoeus-%TYPHOEUS_VER%\cross\curl-%CURL_VER%.win32\include
    cd %HOMEDRIVE%

    cp -R "%HOMEDRIVE%%HOMEPATH%\Desktop\curl-%CURL_VER%-devel-mingw32\curl-%CURL_VER%-devel-mingw32\include\curl" C:\PROGRA~1\Diaspora\vendor\ruby\1.8\gems\typhoeus-%TYPHOEUS_VER%\cross\curl-%CURL_VER%.win32\include
    cp -R "%HOMEDRIVE%%HOMEPATH%\Desktop\curl-%CURL_VER%-devel-mingw32\curl-%CURL_VER%-devel-mingw32\bin" C:\PROGRA~1\Diaspora\vendor\ruby\1.8\gems\typhoeus-%TYPHOEUS_VER%\cross\curl-%CURL_VER%.win32

    copy "%HOMEDRIVE%%HOMEPATH%\Desktop\curl-%CURL_VER%-devel-mingw32\curl-%CURL_VER%-devel-mingw32\bin\*.dll" C:\Ruby187\bin
    copy "%HOMEDRIVE%%HOMEPATH%\Desktop\curl-%CURL_VER%-devel-mingw32\curl-%CURL_VER%-devel-mingw32\bin\*.dll" C:\Ruby187\bin

Should the Typhoeus and Curl versions change, adjust the Typhoeus and Curl version environment variables used above.

### Installing Diaspora (Continued)

From the command line type:

    C:\Progra~1\RubyDevKit\devkitvars.bat
    bundle install
