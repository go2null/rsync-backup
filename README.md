rsync-backup
============

rsync backup script
  
Usage
-----
        rsync-backup -d DIRECTORY -k NUMBER -K NUMBER -s NUMBER -S NUMBER -o -f FILE
        
Options
-------
      -f FILE      - config file. Can be specified multiple times in order of increasing priority.
      -d DIRECTORY - directory to backup to.
      -k NUMBER    - minimum backups to keep. Default is 1.
      -K NUMBER    - maximum backups to keep.
      -s NUMBER    - minimum free space to reserve, in MB.
      -S NUMBER    - minimum free space to reserve, as %.
      -o           - by default, -k is overridden when -s or -S is met.
                     This option disables this behaviour. Impact is that the backup is not done if -s or S is met.
      -h           - Display this help.

Description
-----------
* Creates `YYYY-MM-DD-HHMMSS` directories in `BAK_DIR`
* Creates a `latest` symlink, pointing to the backup made
* Creates a `previous` symlink, allow other backup scripts to use a `rsync --link-dest`
* Example `.conf` file.

        BAK_DIR='/var/backups'
        ## Num backups to keep
        BAK_KEEP_MIN=7
        BAK_KEEP_MAX=31
        BAK_SPACE_MB_MIN=1000
        BAK_SPACE_PC_MIN=5
        ## Override KEEP_MIN if free space falls below MIN_KB or MIN_%
        BAK_MIN_OVERRIDE=1
        ## path of directories and files to backup
        ##   directories must end with '/'
        BAK_1="$HOME/"
        BAK_2='/etc/apache2/ports.conf'
        BAK_3='/etc/apache2/ssl'
        PTN_3="--exclude='*.key' --include'*/' --exclude='*'"

Release Notes
-------------
* **2013-04-22 : 0.0.0 : go2null**
  * initial release
* **2013-04-24 : 0.1.0 : go2null**
  * Changed shell from bash to sh, as is POSIX compliant
* **2013-04-25 : 0.2.0 : go2null**
  * NEW : Added options to autoremove oldest backups if free space (KB or %) falls below either minimum
  * NEW : Added MAX # backups to keep
* **2013-05-06 : 0.3.0 : go2null**
  * NEW : Creates a 'previous' symlink, allow other backup scripts to use a rsync --link-dest
* **2013-05-07 : 0.4.0 : go2null**
  * FIX : BAK_SPACE_MB was calculated incorrectly
  * NEW : Added output INFO messages
  * NEW : checks free space after backup
* **2013-05-28 : 0.5.0 : go2null**
  * FIX : bug #31746#note-3 in directory listing (no code change)
* **2013-06-11 : 0.6.0 : go2null**
  * NEW : option for MIN_KB/MIN_% to override KEEP_MIN to allow backups even with critically low free space
  * FIX : directory to backup - leaf directory repeated. Need to append '/'.
  * FIX : # backups check always return 0
  * FIX : If PTN not defined, rsync copies PWD as well as source.
* **2013-09-16 : 0.7.0 : go2null**
  * NEW : split out user settings into .conf file
  * NEW : all options can be specified as input parameters
* **2013-09-23 : 0.8.0 : go2null**
  * FIX : 'latest' symlink is invalid for relative paths
  * NEW : Updated description to add example conf file.
  * NEW : Add -h option to diplay help
* **2013-10-02 : 0.8.1 : go2null**
  * Minor help text cleanup.
  * Moved Release Notes to README.
