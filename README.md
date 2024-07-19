# Interview RPI DUT Setup

The Idea of this is to provide a device for Interview candidates to test their knowledge. The Test device is a RPI3 with the following ports;

* 80 (Some basic page with XSS)
* 443 (A secure webpage)
* 21 FTP with anon
* 22 This is by default as this is build on a RPI being set up using SSH on port 22

# Setup

Put a fresh RPI install on the RPI SDCARD

Log in via SSH

```bash

sudo apt update
sudo apt upgrade -y




```

## FTP setup

```bash

sudo apt install vsftpd -y






```
