# RaspberryPi-Automatic-data-backup
I wrote the following python script because my Raspberry Pi failed once and I couldn't get to my apps and files anymore. This script creates a backup of relevant folders and uploads it automatically to your Dropbox



---
## Python Script which should run on your raspberry pi

Important: You need to install dropbox on your raspberry pi (cf. https://pimylifeup.com/raspberry-pi-dropbox/)

Copy this skript (20_12_31_Backup.py) on your raspberry pi and chance the folders you want to backup into your dropbox cloud.


````python
#!/usr/bin/python
import re
import time
import datetime
import shutil
import glob
import os
import subprocess

#TIME

UnixTime = int(time.time())
print (UnixTime)
currentdate = datetime.datetime.fromtimestamp(UnixTime).strftime('%Y-%m-%d') # creates a string with current date 
print(currentdate)


output_filename = "PiBackup_" + currentdate
dir_name = "/home/pi/Dokumente/Programme" # insert your folder which you want to upload into your dropbox cloud
shutil.make_archive(output_filename, 'zip', dir_name)

time.sleep(10)


#Dropbox Upload

targetPattern = r"/home/pi/*.zip" #search for the current backupfile
a = glob.glob(targetPattern)[0] #search for the current backupfile
os.system("/home/pi/Dropbox-Uploader/dropbox_uploader.sh upload " + a + " /Backup/") #upload the current backup file to the dropboxcloud

time.sleep(30)

#Delete zip file on raspberry 

os.remove(a)

````

---
## crontab 


open crontab by typing crontab -e in your terminal


<img src= "images/00 rasp crontab.jpg" width="600">

insert the file path to your python script. You can adjust the upload-frequency by changing the crontab syntax (cf. https://crontab.guru/between-certain-hours)


<img src= "images/01 rasp crontab.jpg" width="600">


---
## Backup on your dropbox cloud
after uploading your backup you can have access on all your connected devices.

<img src= "images/02 uploads.jpg" width="600">



