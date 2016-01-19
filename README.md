#Statusengine  - the missing event broker

Statusengine is a drop in replacement for NDOutils and it is able to use the same
database schema. Statusengine uses [gearmand](https://github.com/gearman) as Queueing engine,
so your MySQL database will not slow down the Nagios/Naemon core.

Additionally Statusengine is worker based. If your system grows and you need to process
more data, you can simply increase the number of worker processes.

To make your data visible Statusengine comes with a responsive web interface which
allows you to submit commands and provides a nice way to process the data with external
scripts by quering the HTTP API and append the url with **.json** or **.xml** extension.

Statusengine is modular, so you can use just the parts you need!

[Visit the project page](http://www.statusengine.org) for more information.

##Features
- Worker based Nagios/Naemon event data processor
- Based on MySQL
- Json based communication
- Automatic database schema updates
- Responsive Web Interface
- Processing of performance data
- Full UTF-8 support
- In memory engine
- Modular

##Requirements
- **Nagios 4** or **Naemon**
- MySQL server
- PHP 5.4 or greater
- Ubuntu 14.04 LTS

##Installation

1) Clone repository
```bash
chmod +x install.sh
./install.sh
```

2) Set your username and password of MySQL server in /opt/statusengine/cakephp/app/Config/database.php
```php
	public $legacy = array(
		'datasource' => 'Database/Mysql',
		'persistent' => false,
		'host' => 'localhost',
		'login' => 'naemon',
		'password' => '12345',
		'database' => 'naemon',
		'prefix' => 'naemon_',
		'encoding' => 'utf8',
	);
```

3) Create database (using CakePHP shell) MyISAM
```bash
/opt/statusengine/cakephp/app/Console/cake schema update --plugin Legacy --file legacy_schema.php --connection legacy
```
or:

3) Create database InnoDB **(Recommended!)**
```bash
/opt/statusengine/cakephp/app/Console/cake schema update --plugin Legacy --file legacy_schema_innodb.php --connection legacy
```

4) Change path to your nagios.cfg / naemon.cfg in /opt/statusengine/cakephp/app/Config/Statusengine.php if different on your system
```php
'coreconfig' => '/etc/naemon/naemon.cfg',
```

5) Start Statusengine in legacy mode (forground):
```bash
/opt/statusengine/cakephp/app/Console/cake statusengine_legacy -w
```
or

5) Start Statusengine in legacy mode (background):
```bash
service statusengine start
```
Check the documentation for the [migration guide](http://statusengine.org/getting_started.php#migration)

##Tested with
- Naemon 0.8.0 up to master
- Nagios 4.0.8
- mod_gearman
- NagVis
- MySQL
- MariaDB

##Changelog

**1.0.1**
- First stable version of Statusengine

**1.2.0**
- Add in memory engine

**1.3.0**
- Multithreading for Servicestatus

**1.4.0**
- Add native performance data processor
- Add mod_perfdata (performance data processor for mod_german)

**1.4.1**
- Add support for Naemon configuration process_performance_data for each service

**1.5.0**
- Add responsive web interface

**1.5.1**
- Fixed "MySQL has gone away" crashes of StatusengineLegacyShell

**1.5.2**
- Improved performance of StatusengineLegacyShell (GEARMAN_WORKER_NON_BLOCKING)

**1.5.3**
- Add Pull-To-Refresh to the web interface for mobile devices

**1.5.4**
- Resolve issue with orphaned child processes [Issue 14](https://github.com/nook24/statusengine/issues/14)
- Remove /var/log/statusengine.log [LogfileTask.php] and use syslog instead [Issue 15](https://github.com/nook24/statusengine/issues/15)

##Licence

Copyright (c) 2014 - present Daniel Ziegler <daniel@statusengine.org>

Statusengine is free software; you can redistribute it and/or
modify it under the terms of the GNU General Public License
as published by the Free Software Foundation in version 2

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program; if not, write to the Free Software
Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston, MA  02110-1301, USA.
