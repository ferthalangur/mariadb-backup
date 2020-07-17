# TODO List for mariadb_backup #
-----
        (Updated 2020-07-16)

## Bugs to Fix ##

* The pruning logic needs to have some guard logic to prevent pruning
  if earlier backups have failed. For example, the database server
  did not start after a reboot, but mariadb_backup happily failed to
  back up and then pruned the good backup directories.

## Features to Add ##

* Set the ownership and permissions of the files and directories that
  are created. The mariabackup has hardcoded umask values for files
  and directories that override umask values. This leads to problems
  with group access permissions set up to allow later transfer of the
  files elsewhere.

* Either read a variable with the location of a configuration file to source
  that overrides /etc/default/$PROGNAME or add a command-line switch
  for the location of an alternate configuration file. It should
  be the last source of setting parameters (although an argument could
  made to use actual environment variables).

* Make the hour select more natural, such as an array of hours (6 12
  18) instead of "06 12 18".
  
* Encrypt and Compress to a Single File

* Support incremental backups

* Option to run without output unless an error occurs

## Associated Things to Do ##

* Create a Restore Script

## Other ##
