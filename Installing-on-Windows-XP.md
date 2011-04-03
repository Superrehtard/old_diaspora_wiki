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
