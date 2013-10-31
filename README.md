vzpbackup
=========

OpenVZ Container Backup - for containers using ploop storage

The scripts are meant to provide a backup solution to backup
containers that use ploop storage. Traditional storage is
not supported by the scripts. The scripts are based on the
OpenVZ wiki page regarding image backup:
[http://openvz.org/Ploop/Backup](http://openvz.org/Ploop/Backup)

## BACKUP: vzpbackup.sh

Backup a container with container ID 200:

vzpbackup.sh --suspend=yes --compress=gz 200

Options:

--suspend=\<yes/no> (Default: no)

	This will result in calling vzctl snapshot without the
	parameter "--skip-suspend" resulting in a snapshot of
	the running container being included in the backup.
	If you restore that backup using vzprestore.sh, you can
	resume the container after the restore has finished.

--backup-dir=\<Directory> (Default: /store/vzpbackup)

	Parameter to change the default backup directory. Either
	use the parameter or change the script.

--compress=\<Compression> (Default: no(ne))

    Allows you to compress the resulting archive file using either
    bzip2, pigz, gzip or xz to save some disk space.

--all

	This will backup _all_ containers that are currently
	configured on the host (Every container returned by
	"vzlist -Hoctid"). Currently there is no option to
	exclude specific CTIDs from the backup.

## RESTORE: vzprestore.sh

Restore a backup to a new container 250:

vzprestore.sh --archive=/store/vzpbackup/vzpbackup_200_test.host.tar --container=250

Options:

--archive=\<PathToBackupArchive> (Default: None)

	Specify the backup archive to be restored.

--container=\<ContainerID> (Default: None)

	Specify the container that the backup shall be restored to.

--confirm=\<yes/no>

	Confirm what is being done prior execution.

--delete-backup-snapshot=\<yes/no>

	Delete (switch to it and delete it) the snapshot created during
        backup creation. Note: Any memory dumps will be lost if you do it
        at this stage after restoring the container.

--vzdir=\<VZ Directory for VE_PRIVATE and VE_ROOT>

        Instead of using the default VE_PRIVATE and VE_ROOT from the
        default OpenVZ configuration (/etc/vz/vz.conf), create the
        VE_ROOT and VE_PRIVATE in the directory given by the vzdir
        option


Author: Andreas Faerber, af@maeh.org
