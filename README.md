# mariadb-backup - A cron-appropriate wrapper for mariabackup #

MariaDB provides a backup program, called
[mariabackup](https://mariadb.com/kb/en/library/mariabackup).

That program is well-documented, but does not include a cron wrapper to
execute automatically, similar to how automysqlbackup worked.

This is a driver script for **cron(8)** that will run mariabackup daily or as frequently as
once per hour. It can be run from */etc/cron.hourly*,
*/etc/crontab*, or root's crontab ... whichever idiom you prefer.



## Prerequisites ##

You need to install MariaDB and MariaDB Backup using your favorite
package manager or from source code. I test on the latest CentOS and
Ubuntu Linux distributions, but it should be fairly
portable. Everything is parametrized script and the configuration file.

The base directory where your backups will go must be created before
running the script or it will fail. I use
*/usr/local/backup/mariadb-backup/* as my base directory but you can
change that in the configuration file.

To follow or copy/paste my examples, set the environment variable
$EDITOR to your favorite text editor (e.g., export EDITOR='emacs -nw')
and then type in my example commands verbatim.


## Installation ##

Clone or download this repository.

You will need sudo privileges or a root shell to install.


1. Create a database user specifically for doing backups. This user
   will need to be granted many, but not all global privileges in
   order to create database backups and restore from them. For doing
   automated backups, you will need to store credentials in a file for
   this user, so I recommend against using the root user and
   password.

2. Edit mariadb-backup.my.cnf with the user name and password that was
   created in Step 1. Copy this file to */etc/my.cnf.d/mariadb-backup*
   set set permissions to protect the credentials.

```
   $EDITOR mariadb-backup.my.cnf
   cp mariadb-backup.my.cnf /etc/my.cnf.d/mariadb-backup.cnf
   chown root:root /etc/my.cnf.d/mariadb-backup.cnf
   chmod 00600 /etc/my.cnf.d/mariadb-backup.cnf
```

3. Edit mariadb-backup.default for your system. 

    This is a Bash snippet that contains environment variables that
    are sourced by the running script. The expected location for this
    file is in */etc/default/mariadb-backup* . If you want your
    configuration file elsewhere, change the variable
    **MADBB\_CONFIG_FILE** in the script itself.

    Variables that you may want to change in the default file:

		MADBB_HOURs sets the frequency that backups are run per
		day. This is a string that lists hours, separated by spaces,
		zero-padded, of the hours to make a backup. The default is "06 12
		18" which runs mariabackup at 6 AM, noon and 6 PM in the default
		timezone of your server. The script expects to be executed by one
		of the cron facilities every hour, and then it chooses which hours
		to actually do a backup based on this variable.

		MADBB_OPTIONS sets any options that you want to pass to the
		mariabackup executable.

		MADBB\_BACKUP_DIR sets where your backups will go. This
		directory must be created and the permissions set before running
		mariabackup for the first time.

		MADBB\_PRUNE_DAILY set to 0 will prevent daily pruning of older
		backups.

		MADBB\_SAVE_DAYS is an integer of the number of days of backups to
		be kept. Default is 10.

```
   $EDITOR mariadb-backup.default
   cp mariadb-backup.default /etc/default/mariadb-backup
   chown root:root /etc/default/mariadb-backup
   chmod 00644 /etc/default/mariadb-backup
```

4. You have two choices for installation. You can install the script
   to a location on your server, such as
   */usr/local/sbin/mariadb-backup* and then call it from an hourly **cron**
   entry, or you can drop the script into the */etc/cron.hourly*
   directory. It all depends on how you are most comfortable.

   The script should be run every hour, but backups will only be
   created in the hours you specify in **MADBB_HOURS**.

   (a) Copy mariadb-backup to /etc/cron.hourly.
	
```
   cp mariadb-backup /etc/cron.hourly/mariadb-backup
   chown root:root /etc/cron.hourly/mariadb-backup
   chmod 00744 /etc/cron.hourly/mariadb-backup
```

   (b) Copy mariadb-backup to somewhere on your server and create a
   crontab entry in root's crontab or in /etc/crontab

```
   cp mariadb-backup /usr/local/sbin/mariadb-backup
   chown root:root /usr/local/sbin/mariadb-backup
   chmod 00744 /usr/local/sbin/mariadb-backup
```

   That should do it. The backups will run every hour that you specified
   in the defaults file. Output will be handled the way that all cron
   output is handled (see TODO.md).



## Support ##

If you find bugs or make improvements and would like to send me a PR
with a note of what you changed and why, I will review it and if
appropriate, merge it into the base code, with attribution.

Report issues on the issues tracker on GitHub, or email me : rbj(atsymbol)spotch.com


## Release History ##
v1.0.1 - 2019-01-01
* Added pruning of old backups.
* Renamed the program and repository to mariadb-backup
* Shortened the configuration variable prefix to MADBB_

v1.0.0 - 2018-04-06. Initial release.

## Author(s) ##

* **Rob Jenson** - *Initial author* -
    [ferthalangur](https://github.com/ferthalangur)

## License ##
    Copyright (C) 2018-2020 Robert Bryan Jenson  rbj(atsymbol)spotch.com

    This program is free software: you can redistribute it and/or modify
    it under the terms of the GNU General Public License as published by
    the Free Software Foundation, either version 3 of the License, or
    (at your option) any later version.

    This program is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.

    You should have received a copy of the GNU General Public License
    along with this program.  If not, see <http://www.gnu.org/licenses/>.
