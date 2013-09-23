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
