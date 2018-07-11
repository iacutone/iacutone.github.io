---
layout: post
title: My Photo Storage Solution
date: 2017-04-15 10:58:55 -0400
comments: true
categories: shell linux
---

I have been searching for a photo storage solution. My solution only involves two devices, a raspberry pi and an external hard drive. This post will explain how to set up your pi in order to automatically transfer RAW and JPG photos from an SD card to the hard drive connected to the pi. The scripts I used are available [here](http://github.com/iacutone/scripts).

I like this solution because the entire process is automatic. You are notified when the photo transfer has finished with an email. Also, for redundancy, I created a cron job that syncs all of the photos on my hard drive to Amazon photos. I use [this service](https://rclone.org/) to sync the photos in the cloud.

### Steps

#####  Mount the external hard drive to a folder on your pi.
* `sudo fdisk -l` take note of the Device, something like `/dev/sdb1`
* I like to mount the drive under the home dir, `sudo mount /dev/sdb1 ~/external_hd/`
* Run `lsblk -f`  to know here files are mounted

##### Create a `udev` rule
* `udev` rules live here: `/etc/udev/rules.d`
* `cd /etc/udev/rules.d`
* `touch 50-sdcard.rules`
* `chmod +x 50-sdcard.rules`
* `sudo fdisk -l` again to find the sd card device name, in this instance 'sda1'
* `echo 'KERNEL=="sda1", ACTION=="add", RUN+="/home/pi/sdcard/sdcard_added.sh"' < 50-sdcard.rules`
* `tail -f /var/log/syslog` is extremely useful for debugging udev rules

##### Create an fstab privilege
* `vi /etc/fstab`
* Find device uuid with `blkid`
* Add `UUID=<your-uuid> /home/pi/SD_CARD auto rw,users,noauto      0       0`
* Uncomment out 'user_allow_other' in `/etc/fuse.conf`

##### `rsync` when an sd card is added
* `touch /home/pi/sdcard/sdcard_added.sh`
* `chmod +x /home/pi/sdcard/sdcard_added.sh`
* create the script
```bash
#!/bin/bash

echo "`date +%Y-%m-%d:%H:%M:%S` SD card inserted" >> /home/pi/sdcard/output.log
mount /dev/sda1 /home/pi/SD_CARD
rsync -av /home/pi/SD_CARD/DCIM/100MSDCF/ /home/pi/external_hd/raw_photos
```
##### Sync photos to cloud provider
* Set up cron job
* `sudo crontab -e`
* `00 4 * * * /home/pi/sd_card/rclone_cron.sh`, this runs the cron job at 4 am, daily
* create the bast script
```bash
#!/bin/sh
/usr/sbin/rclone sync /home/pi/external_hd/raw_photos/ amazon:raw_photos --config /home/pi/.rclone.conf -v
```
##### Send Email Notification
* I used [this post](https://rianjs.net/2013/08/send-email-from-linux-server-using-gmail-and-ubuntu-two-factor-authentication) to easily add email notifications when the rsync process completes

##### Tips Since Creating My Photo Process
* I could not get the above code to work with Debian due to permission issues
* It is better to identify from UUID instead of KERNEL for setting up udev rules, run `ls -l /dev/disk/by-uuid` to discover device UUID's

##### RClone

* Amazon has blocked all thrid party applications from pushing to Amazon Drive. For more information see [this](https://forum.rclone.org/t/rclone-has-been-banned-from-amazon-drive/2314)
* I am going to implement [Duplicati](https://github.com/duplicati/duplicati) as a solution to sync my photos to Amazon Drive
* Another potential solution is to continute to use rclone, but host the files on Google Drive
