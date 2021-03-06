---
permalink: backing-up-your-files-with-rsync/
audit_date:
title: Back up your files with rsync
type: article
created_date: '2011-03-16'
created_by: Rackspace Support
last_modified_date: '2016-05-27'
last_modified_by: Stephanie Fillmon
product: Cloud Servers
product_url: cloud-servers
---

Backing up files on a regular basis is an integral part of administering
your server.

One way is to download every individual file when you want to save them.
However, rsync makes the task a lot easier as it only downloads files
that have changed - saving time and bandwidth.

### Install rsync

Rsync should be available on most Linux distributions by default, but if you need to install it manually, you can do so by using your distribution's package manager. For example, the following installation commands are for Ubuntu, Gentoo, and CentOS:

    sudo aptitude install rsync
    sudo emerge rsync
    sudo yum install rsync

**Note:** If you are downloading files to another system, both systems must have rsync installed.

### Use SSH with rsync

To keep your files and system secure, be sure to use encryption when uploading or downloading files. This example uses the SSH protocol with rsync to keep your data secure. If you prefer not to use SSH, you will be prompted for a password each time you run rsync.

If you use rysnc to automate your backups, ensure that the destination server (where the backup directory
is located) has access to the originating server.

To backup your files from one server to another, run the following command on the destination server:

    rsync -e 'ssh -p 30000' -avl --delete --stats --progress demo@123.45.67.890:/home/demo /backup

Following is an explanation of each part of the command:

-  `-e 'ssh -p 30000'` - Ensures rsync uses the SSH protocol and sets the
port.

-  `-avl` - A shortcut containing three options:

    -  `-a` - Archive. Saves the permission settings for the files.
    -  `-v` - Verbose. Returns more information about what rsync is doing. You can change how much information rsync returns by eliminating `-v` or request more information with `-vv` or `-vvv`.

       **Note:** Setting verbose mode with `-vvv` returns all information about the rsync process.

    -  `-l` - Links. Preserves any symlinks you created on the source server.

-  `--delete` - Deletes files from the destination folder that are no longer
required (i.e. they have been deleted from the folder being backed up).

-  `--stats` - Adds more output regarding the file transfer status.

-  `--progress` - Displays the progress of each file transfer.

-  `demo@123.45.67.890:/home/demo` - The originating folders to backup.

   **Note:** Rsync differentiates between `/home/demo` and `/home/demo/`. With the trailing slash, rsync will copy the contents of the directory, but won't recreate the directory. In this example, we're backing up a folder from one system to another, so leaving off the trailing slash will replicate the entire directory structure.

-  `/backup/` - Identifies the folder on the backup server to place the files.

Your output should look similar to the following:

    receiving file list ...
    31345 files to consider
    ./
    tuning-primer.sh
           42596 100%  533.30kB/s    0:00:00 (xfer#2, to-check=31331/31345)
    bin/
    bin/Backup
             618 100%    7.74kB/s    0:00:00 (xfer#3, to-check=31310/31345)
    bin/Search
             455 100%    5.70kB/s    0:00:00 (xfer#4, to-check=31309/31345)
    configs/
    configs/php.ini
             114 100%    1.43kB/s    0:00:00 (xfer#5, to-check=31307/31345)
    public_html/
    ...
    ...

Rsync receives a list of files (in this example, 31,345 of them) and, because this is the first time rsync has been run, downloads them all.

If you run rsync again, only files that have changed since the last backup will be synced to your backup folder.
