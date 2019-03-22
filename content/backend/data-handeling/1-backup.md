---
title: Backup
pageTitle: Backup
description: "Learn all about GraphQL and why it is an API technology that's superior to REST. It is not only for React & Javascript developers but can be used for any API."
question: Which of these statements is true?
answers: ["GraphQL is a database technology", "GraphQL can only be used together with SQL", "GraphQL was invented by Facebook", "GraphQL was developed by Netflix and Coursera"]
correctAnswer: 2
videoId: oCT4HOJsUZQ
duration: 5
---

## MongoDB shell on Wekan Snap

mongoshell.sh
```bash
#/bin/bash
version=$(snap list | grep wekan | awk -F ' ' '{print $3}')
mongo=$"/snap/wekan/$version/bin/mongo"
$mongo --port 27019
```

## Backup script for MongoDB Data, if running Snap MongoDB at port 27019

```sh
#!/bin/bash

makeDump()
{
    # Gets the version of the snap.
    version=$(snap list | grep wekan | awk -F ' ' '{print $3}')

    # Prepares.
    now=$(date +'%Y-%m-%d_%H.%M.%S')
    mkdir -p /var/backups/wekan/$version-$now

    # Targets the dump file.
    dump=$"/snap/wekan/$version/bin/mongodump"

    # Makes the backup.
    cd /var/backups/wekan/$version-$now
    printf "\nThe database backup is in progress.\n\n"
    $dump --port 27019

    # Makes the tar.gz file.
    cd ..
    printf "\nMakes the tar.gz file.\n"
    tar -zcvf $version-$now.tar.gz $version-$now

    # Cleanups
    rm -rf $version-$now

    # End.
    printf "\nBackup done.\n"
    echo "Backup is archived to .tar.gz file at /var/backups/wekan/${version}-${now}.tar.gz"
}

# Checks is the user is sudo/root
if [ "$UID" -ne "0" ]
then
    echo "This program must be launched with sudo/root."
    exit 1
fi


# Starts
makeDump

```

## Restore script for MongoDB Data, if running Snap MongoDB at port 27019 with a tar.gz archive.

```sh
#!/bin/bash

makesRestore()
{
    # Prepares the folder used for the backup.
    file=$1
    if [[ "$file" != *tar.gz* ]]
    then
        echo "The backup archive must be a tar.gz."
        exit -1
    fi

    # Goes into the parent directory.
    ext=$"$(basename $file)"
    parentDir=$"${file:0:${#file}-${#ext}}"
    cd $parentDir

    # Untar the archive.
    printf "\nMakes the untar of the archive.\n"
    tar -zxvf $file
    file="${file:0:${#file}-7}"
    
    # Gets the version of the snap.
    version=$(snap list | grep wekan | awk -F ' ' '{print $3}')

    # Targets the dump file.
    restore=$"/snap/wekan/$version/bin/mongorestore"

    # Restores.
    printf "\nThe database restore is in progress.\n\n"
    $restore -d wekan --port 27019 $file/dump/wekan
    printf "\nRestore done.\n"

    # Cleanups
    rm -rf $file
}

# Checks is the user is sudo/root.
if [ "$UID" -ne "0" ]
then
    echo "This program must be launched with sudo/root."
    exit 1
fi


# Start.
makesRestore $1

```

## Docker Backup and Restore

[Docker Backup and Restore](https://github.com/wekan/wekan/wiki/Export-Docker-Mongo-Data)

[Wekan Docker Upgrade](https://github.com/wekan/wekan-mongodb#backup-before-upgrading)

## Snap Backup

[Snap Backup and Restore](https://github.com/wekan/wekan-snap/wiki/Backup-and-restore)

[Wekan Snap upgrade](https://github.com/wekan/wekan-snap/wiki/Install#5-install-all-snap-updates-automatically-between-0200am-and-0400am)

## Sandstorm Backup

Download Wekan grain with arrow down download button to .zip file. You can restore it later.

[Export data from Wekan Sandstorm grain .zip file](https://github.com/wekan/wekan/wiki/Export-from-Wekan-Sandstorm-grain-.zip-file)

## Python Backup Script for Wekan Docker environment

[Use Python to backup your Mongo Data, with automatic cleanup](https://github.com/wekan/wekan/wiki/Python-Backup-Script-for-Wekan-Docker-environment)