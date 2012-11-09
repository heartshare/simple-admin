admin-scripts
=============
Scripts for simplified Ubuntu server administration and
maintenance.


Requirements
------------
* Ubuntu 12.04
* lzma
* Perl
* Perl - Geo::IPfree
* Perl - JSON::XS


Installation
------------
1. ```apt-get install libgeo-ipfree-perl```
2. ```apt-get install libjson-xs-perl```
3. ```cp bin/admin-* /usr/local/bin/```
4. Copy & modify config files in ```etc```


admin-backup-files
------------------
Performs a filesystem backup for configured directories.

**Syntax:** admin-backup-files [-v]

Option | Description
-------|-------------------------------------------------
-v     | Print verbose output.

Configured in ```/etc/admin-backup-files.conf``` with one line
per directory to backup:

```
/etc
/home
/root
```

All the files in the directories specified will be recursively
copied to ```/backup/<hostname>/current/```, which serves as a
snapshot backup directory. Previous versions of files are also
stored, under the ```/backup/<hostname>/history``` directory.

Separate directories with changed files per hour, day and month
are created. The historic files are hard linked to the snapshot
directory to preserve space. The hourly history is preserved
for 24 hours, daily history for 30 days and thereafter only the
monthly history is kept.

The file backup can be run as frequently as every minute, but
should at least be run once per day.


admin-backup-mysql
------------------
Performs a MySQL dump of configured databases.

**Syntax:** admin-backup-mysql [-v]

Option | Description
-------|-------------------------------------------------
-v     | Print verbose output.

Configured in ```/etc/admin-backup-mysql.conf``` with one line
per database to backup:

```
database1 user password
database2 user password --ignore-table=table1
```

The ```/var/local/admin-backup-mysql``` directory is used to
store the output MySQL dump files (compressed).


admin-freemem
-------------
Cleans filesystem cache (if possible) to recover memory.

**Syntax:** admin-freemem

Forces a filesystem sync followed by a write
to ```/proc/sys/vm/drop_caches```.


admin-status
------------
Checks the status of a number of services on the machine.

**Syntax:** ```admin-status```

Configured in ```/etc/admin-status.conf``` with one line
per service (and PID file):

```
cron    /var/run/crond.pid
sshd    /var/run/sshd.pid
```


admin-uptodate
--------------
Runs a check to determine if the system requires updates.

**Syntax:** ```admin-uptodate [-s] [-m]```  

Option | Description
-------|-------------------------------------------------
-s     | Check only for security updates.
-m     | Mail root instead of printing results.


admin-utf8
----------
Converts a file or directory to UTF-8 (from ISO-8859-1). Only if needed.

**Syntax:** ```admin-utf8 (check|convert) <file or dir>```

Use either ```check``` to detect file encoding, or ```convert``` to
convert all ISO-8859-1 file(s).

