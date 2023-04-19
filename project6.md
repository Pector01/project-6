# PECTOR DOCUMENTION FOR PROJECT 6
- Here, I was asked to prepare storage infrastructure on two Linux servers and implement a basic web solution using WordPress. As shown in the documentation, we have a Three-tier Architecture which is a client-server software architecture pattern that comprise of 3 separate layers (Presentation Layer (PL)), (Business Layer (BL)) and (Data Access or Management Layer (DAL)) which we also have in 3-Tier Setup
- A Laptop or PC to serve as a client
- An EC2 Linux Server as a web server (This is where you will install WordPress)
- An EC2 Linux server as a database (DB) server.
Volumes for 2 servers were created
![Volumes](./images/Volumes%20for%20the%20two%20servers.png), I then created a linux terminal for instance
![Database](./images/for%20database.png), to create my partition, I ran `lsblk` ![Partition](./images/first%20partition.png).
I used  pvcreate utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM `sudo pvcreate /dev/xvdf1` `sudo pvcreate /dev/xvdg1` `sudo pvcreate /dev/xvdh1` ![pvcreate](./images/physical%20volume%20verification.png) thereafter create the volume group with the command `sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1` ![volume](./images/physical%20volume%20creation.png), logical volume was created and running ![Dependenccreatedy](./images/logical%20volume.png), 
![running](./images/logical%20volumes%20running.png), I ran `sudo mkfs -t ext4 /dev/webdata-vg/app-lv and sudo mkfs -t ext4 /dev/webdata-vg/logs-l` ![file system](./images/logical%20volumes%20with%20ex4%20filesystem.png)
 I used `sudo mkdir -p /var/www/html` to create a directory in other to store the websites files and `sudo mkdir -p /home/recovery/logs` to store backup of log `/var/www/html/` was mounted on `app-lv` with `sudo mount /dev/webdata-vg/apps-lv /var/www/html/` and `sudo rsync -av /var/log/. /home/recovery/logs/` to back up all the files in the log directory on `/home/recoverylogs/` After mounting, `sudo rsync -av /home/recovery/logs/. /var/log` was used to resore all the lost data. We needed to update the `/etc/fstab`, i ran `sudo blkid` ![verify](./images/verification%20of%20webser%20setup.png) and the output gave me this ![Output](./images/Output.PNG) also sudo into mysql to check if it is running ![Dependency](./images/Configuration.png)
 # INSTALLATION OF WORDPRESS
 - To update and upgrage in linux terminal is quite diffent from Ubuntu Terminal here you use yum `sudo yum update` `sudo yum upgrade` and to get the dependencies running I run `sudo systemctl enable httpd` `sudo systemctl start httpd` `sudo systemctl start httpd` ![Dependencies run](./images/httpd%20status.png) 
 Now to install PHP, I ran some codes For PHP Installation;
`sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm`
`sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm`
`sudo yum module list php`
`sudo yum module reset php`
`sudo yum module enable php:remi-8.0` Please note, version 8.0 was the most recent version at the time of implementation and that was what i used not 7.4 as shown in the documentaion
`sudo yum install php php-opcache php-gd php-curl php-mysqlnd`
`sudo systemctl start php-fpm`
`sudo systemctl enable php-fpm` to check status, `sudo systemctl status php-fpm` ![run stat](./images/status.PNG) 
`setsebool -P httpd_execmem 1` After that, the follwing commands to install, create a directory and copy contents to wordpress: `mkdir wordpress` `cd wordpress` `sudo wget http://wordpress.org/latest.tar.gz` NOTE: If your terminal does not support wget or it is showing command not found,  wget installation `sudo tar xzvf latest.tar.gz` `sudo rm -rf latest.tar.gz cp wordpress/wp-config-sample.php wordpress/wp-config.php cp -R wordpress /var/www/html/` ![mysql](./images/mysqlstatus.PNG), ![running?](./images/mysqlstatus%20connect.PNG), atferwards I access my server on my web and this was the output ![Redhat](./images/Redhat.PNG) proceding through to the documentation and the output resulted into this, ![WordPress](./images/wordpress.PNG)
I think I have funny deep in love with Devops. Thank You darey.io