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

wget https://github.com/jrd3n/InterviewTest/archive/refs/heads/main.zip
unzip main.zip 

```

## FTP setup

```bash

sudo apt install vsftpd -y
sudo cp ./InterviewTest-main/vsftppd.conf /etc/vsftpd.conf

sudo mkdir -p /home/pi/ftp
sudo mkdir -p /home/pi/ftp/upload

sudo chown -R ftp:ftp /home/pi/ftp
sudo chmod -R 555 /home/pi/ftp

sudo cp ./InterviewTest-main/ftpFlag.txt /home/pi/ftp/ftpFlag.txt

sudo chmod -R 777 /home/pi/ftp/upload

sudo systemctl restart vsftpd

sudo systemctl enable vsftpd

```

## SSH User

```bash

sudo cp ./InterviewTest-main/sshd_config /etc/ssh/sshd_config

#!/bin/bash

create_ssh_user() {
    local username=$1
    local password=$2
    local message=$3

    # Encrypt the password using openssl
    local encrypted_password=$(openssl passwd -6 "$password")

    # Create the user with the encrypted password
    sudo useradd -m -p "$encrypted_password" "$username"

    # Set up the .ssh directory for the new user
    sudo mkdir -p /home/"$username"/.ssh
    sudo chmod 700 /home/"$username"/.ssh
    sudo chown "$username":"$username" /home/"$username"/.ssh

    # Create the flag.txt file with the message
    echo -e "$message" | sudo tee /home/"$username"/flag.txt
    sudo chown "$username":"$username" /home/"$username"/flag.txt
}


# Example usage
create_ssh_user "lowuser" "password1" "Great Job you are at layer one! \nSSH creds lowuser624:anotherpassword"
create_ssh_user "lowuser624" "anotherpassword" "Fantastic! You've reached layer two! \nSSH creds miduser:midpassword123"
create_ssh_user "miduser" "midpassword123" "Impressive! You're halfway there! \nSSH creds almostthereuser:nearlydone456"
create_ssh_user "almostthereuser" "nearlydone456" "How deep is this thing! \nSSH creds admin:finalpassword789"
create_ssh_user "admin" "finalpassword789" "Congratulations! \nssh pi:raspberry"

echo -e "flag password 'I got root ! '" > /home/$USER/flag.txt

sudo systemctl restart ssh

```


