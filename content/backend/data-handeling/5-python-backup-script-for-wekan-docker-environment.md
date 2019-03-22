---
title: Introduction
pageTitle: Learn GraphQL Fundamentals with Fullstack Tutorial
description: "Learn all about GraphQL and why it is an API technology that's superior to REST. It is not only for React & Javascript developers but can be used for any API."
question: Which of these statements is true?
answers: ["GraphQL is a database technology", "GraphQL can only be used together with SQL", "GraphQL was invented by Facebook", "GraphQL was developed by Netflix and Coursera"]
correctAnswer: 2
videoId: oCT4HOJsUZQ
duration: 5
---

# Features
* reads values from config file (db-name, container-name, retention of backups, target-path)
* executes mongodump and copies it to the host system
* checks the target backup directory for existing dumps and deletes them if they reached a certain age

This backup script is meant to be executed via cronjob. 
Example crontab (Backup daily at 18:30):
```sh
30 18 * * * /usr/local/sbin/wekandump/wekandump.py /usr/local/sbin/wekandump/wekandump.yml > /dev/null 2>&1
```

Adjust the retention value in the yaml-config file to suit your needs (see example .yml file at the bottom of the page)


```py
#!/usr/bin/env python3


# vim: set fileencoding=utf-8 :
#various imports
import os
import sys
import subprocess
import configparser
import time
import datetime
import smtplib
import traceback
import logging
import gzip
import yaml
import abc
import shutil

#define config class with all required config-parameters
class Config:
  __conf = {
    "db_name" : '',
    "retention" : '',
    "dump_path" : '',
    "container" : '',
    "start_date" : time.strftime('%Y%m%d-%H%M%S'),
    "curdate" : time.strftime('%Y-%m-%d %X')
  }

  #define the parameters that can be set through config file
  __setters = ["db_name", "retention", "dump_path", "container"]

  @staticmethod
  def config(name):
    return Config.__conf[name]

  @staticmethod
  def set(name, value):
    if name in Config.__setters:
      Config.__conf[name] = value
    else:
      raise NameError("Name not accepted in set() method")

#define db class and assign vars
class Dbms(metaclass=abc.ABCMeta):
  def __init__(self, db_name, container, dump_path):
    self._database = db_name
    self._dumpfile = os.path.join(dump_path, self.getdumpfilename())
    self._container = container
    self._compression = CompressionGzip()
    self._dump_path = dump_path

  @abc.abstractmethod
  def dump(self):
    pass

  #function to define filename of the backup-archive
  def getdumpfilename(self):
    return 'dump-{}-{}'.format(self._database, Config.config('start_date'))

#class for the mongodb backup
class DbmsMongodb(Dbms):
  def dump(self):
    #command for creating the backup
    call = 'docker exec {} bash -c "mongodump -d {} -o /dump/"'.format(self._container, self._database)
    try:
      output = subprocess.check_output(call, universal_newlines=True, shell=True)
    except subprocess.CalledProcessError as e:
      raise Exception('Mongodump failed due to the following Error: {}'.format(e))
    #command for copying the backup to the host system
    call = 'docker cp {}:/dump {}'.format(self._container, self._dump_path)
    try:
      output = subprocess.check_output(call, universal_newlines=True, shell=True)
    except subprocess.CalledProcessError as e:
      raise Exception('Pulling dump from container failed due to the following Error: {}'.format(e))
    #tar the backup-folder
    call = 'tar -C {}/dump -cf {} .'.format(self._dump_path, self._dumpfile + '.tar')
    try:
      output = subprocess.check_output(call, universal_newlines=True, shell=True)
    except subprocess.CalledProcessError as e:
      raise Exception('Creating .tar-ball failed due to the following Error: {}'.format(e))
    self._compression.setFilename(self._dumpfile + '.tar')
    self._compression.compress()
    shutil.rmtree(self._dump_path + '/dump')


class Compression(metaclass=abc.ABCMeta):
  def __init__(self):
    self._filename = ''

  def setFilename(self, filename):
    self._filename = filename

  @abc.abstractmethod
  def compress(self):
    pass

#class for compressing the backup with gzip (this can be interchanged with xz, bzip etc.)
class CompressionGzip(Compression):
  def compress(self):
    call = 'gzip {}'.format(self._filename)
    try:
      output = subprocess.check_output(call, universal_newlines=True, shell=True)
    except subprocess.CalledProcessError as e:
      raise Exception('Compression failed due to the following Error: {}'.format(e))
    else:
      #print('Successfully created dump: {}'.format(self._filename + '.gz'))
      pass

#DB-Config-File-Checker: checks if the file passed in the function call is accessible, if not, raise exception
def checkcfg(conf):
  if(os.path.isfile(conf)):
    config_file = conf
  else:
    raise Exception("Specified Config File doesn't exist or insufficient access rights")
  checkpermission(config_file)
  return config_file

def checkpath(path):
  if not os.path.exists(path):
    os.makedirs(path)

#this checks the permissions of the config file (you can leave this part out, just required because of corporate environment)
def checkpermission(cfg):
  if (os.stat(cfg).st_uid != 0):
    raise Exception("Config file must be owned by user root!")
  elif (os.stat(cfg).st_gid != 0):
    raise Exception("Config file must be owned by group root!")
  else:
    accessmask = oct(os.stat(cfg).st_mode)[-3:]
    if accessmask == '600' or accessmask == '700':
      pass
    else:
      raise Exception("Root must have read and write access to config file, all other users mustn't be allowed. Current Access Mask: {} but it should be 600 or 700".format(accessmask))
    pass



def parseInput():
  sys.tracebacklimit = None
  #check if the script has been called with one argument --> The db-specific config file
  if len(sys.argv) != 2:
    raise Exception("usage: wekandump.py <path_to_configfile> \n Please specify the path to a configfile")

  #Send the specified db-config file to the Configuration-Checker
  config_file = checkcfg(sys.argv[1])

  #Now that the config-file have been checked, finally open it
  with open(sys.argv[1], 'r') as cfgfile:
    cfg = yaml.safe_load(cfgfile)


  #Set some vars using data from the config-file
  Config.set('db_name', cfg['dumps']['database'])
  Config.set('retention', cfg['dumps']['retention'])
  Config.set('dump_path', cfg['dumps']['path'])
  Config.set('container', cfg['dumps']['container'])

  checkpath(Config.config('dump_path'))

  cfgfile.close

def dumpcompress():
    dbms = DbmsMongodb(Config.config('db_name'), Config.config('container'), Config.config('dump_path'))
    dbms.dump()


def getcrtime(item):
  call = 'stat -c %y {}'.format(item)
  output = subprocess.check_output(call, universal_newlines=True, shell=True)
  output = output.rstrip()
  crtime = datetime.datetime.fromtimestamp(os.stat(item).st_mtime)
  return crtime

def housekeep():
  #get all filenames beginning with "dump-" located in the dump-directory
  call = 'ls {}'.format(os.path.join(Config.config('dump_path'), "dump-*"))
  output = subprocess.check_output(call, universal_newlines=True, shell=True)
  output = output.rstrip()
  dumps = output.split('\n')
  #now that we have a list with the filenames of the files in the dump-folder, every filename is handled seperately
  for item in dumps:
    item = os.path.join(Config.config('dump_path'), item)
    crtime = getcrtime(item)
    curtime = datetime.datetime.strptime(Config.config('curdate'), '%Y-%m-%d %X')
    if (curtime-crtime).days >= Config.config('retention'):
      try:
        os.remove(item)
      except:
        try:
          shutil.rmtree(item)
        except:
          raise Exception('Housekeep: failed to delete the dump {}'.format(item))
        else:
          #print("Housekeep: Deleted dump: {}, it has reached the age of {} days. (Retention is {} days.)".format(item, curtime-crtime, Config.config('retention')))
          pass
      else:
        #print("Housekeep: Deleted dump: {}, it has reached the age of {} days. (Retention is {} days.)".format(item, curtime-crtime, Config.config('retention')))
        pass
    else:
      #print("Housekeep: Dump {} was kept since it is only {} hours old. (Retention is {} days.)".format(item, curtime-crtime, Config.config('retention')))
      pass


def main():
  parseInput()
  dumpcompress()
  housekeep()

if __name__ == "__main__":
  main()


#created by DrGraypFroot
```

Yaml Config file (Specify the database name, retention in days, backup target path and name of your mongodb-docker-container:

```yml
dumps:
    database: wekan #name of the database
    retention: 14 #number of days of retention
    path: /var/lib/wekandump/ #name of the target directory for dumps
    container: wekan-db #name of the docker-container
```

IMPORTANT:
* the names of the values in the yml-file shouldn't be changed. If you really need to change them, keep in mind that you also have to alter the script accordingly
* You need to have PyYAML and Python installed
* feel free to comment if you have any issues
* Disclaimer: I don't take any responsibility for lost data