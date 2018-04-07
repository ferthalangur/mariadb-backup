# maria_db_backup - A Cron Wrapper for the MariaDB Backup Program #

MariaDB provides a new backup program, called
mariabackup. Documentation lives here:
(https://mariadb.com/kb/en/library/mariadb-backup).

That program is well-documented, but does not include a cron wrapper to
execute automatically, similar to automysqlbackup. 

This is a relatively simple script to run mariabackup daily, as often
as once per hour, from /etc/cron.hourly

## Prerequisites ##

You need to install MariaDB and MariaDB Backup using your favorite
package manager or from source code. I've developed and tested on
CentOS 7, but it should be fairly portable. Everything is parametrized
in the configuration file and the script.

The base directory where your backups will go must be created before
running the script or it will fail. I use
/usr/local/backup/mariabackup/ as my base directory but you can change
that in the configuration file.

## Installation ##

Clone or download this repository somewhere.

You will need sudo privileges or a root shell to do this installation
correctly.

1. If you don't have a database user set up for backups, create one. I
use maria_db_backup. You will have to grant many, if not all
privileges to this user, especially if you will also be restoring with
it. However, for the purpose of doing automated backups, you will need
to store credentials in a file for this user, so I recommend against
using the root user and password.

2. Edit mariabackup.my.cnf with the user name and password that will
be used by mariabackup. Copy the file to /etc/my.cnf.d/mariabackup and
set permissions to protect the credentials.

```
   $EDITOR mariabackup.my.cnf
   cp mariabackup.my.cnf /etc/my.cnf.d/mariabackup.cnf
   chown root:root /etc/my.cnf.d/mariabackup.cnf
   chmod 600 /etc/my.cnf.d/mariabackup.cnf
```

3. Edit mariabackup.default for your system. Most important is
the MARIABACKUP_HOUR variable that sets the frequency of backups per
day. The default is to run at 6 AM, noon and 6 PM.
Also, any options that you want to pass to mariabackup get set in
MARIABACKUP_OPTIONS. Copy the file to /etc/default/mariabackup
```
   $EDITOR mariabackup.default
   cp mariabackup.default /etc/default/mariabackup
   chown root:root /etc/default/mariabackup
   chmod 644 /etc/default/mariabackup
```

4. Copy mariabackup.cron to /etc/cron.hourly . It should not require
any modification.
```
   cp mariabackup.cron /etc/cron.hourly/mariabackup
   chown root:root /etc/cron.hourly/mariabackup
   chmod 744 /etc/cron.hourly/mariabackup
```

That should do it. The backups will run every hour that you specified
in the defaults file. Output will be handled the way that all cron
output is handled (email to root).


## Support ##.

If you find bugs or make improvements and would like to send me a PR
with a note of what you changed and why, I would appreciate it. 

Report issues on the issues tracker on GitHub.

## Release History ##
v1.0.0 - 2018-04-06. Initial release.

## Author(s) ##

* **Rob Jenson** - *Initial author* -
    [ferthalangur](https://github.com/ferthalangur)

## License ##
    Copyright (C) 2018 Robert Bryan Jenson  rbj.(at).spotch.com

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
