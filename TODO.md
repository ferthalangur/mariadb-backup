# TODO List for maria_db_backup #
-----
        (Updated 2018-12-28)

## Bug to Fix ##

## Features to Add ##

* Make a variable to source the configuration file from if you do not
  want to use /etc/default.

* Make the hour select more natural, such as an array of hours (6 12
  18) instead of "06 12 18".
  
* Encrypt and Compress to a Single File

* Support incremental backups

* Option to run without output unless an error occurs

## Associated Things to Do ##

* Create a Restore Script

## Other ##

* I *could* add some command-line switches for debugging and overriding
  the options in /etc/default/mariabackup ... but that seems to add a
  lot of cruft to put into a cron script.

