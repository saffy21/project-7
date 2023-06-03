# CREATING A DEVOPS TOOLING WEBSITE SOLUTION

### PREPARING NFS SERVER

opening up three EC2 instances with RHEL Linux 9 Operating System

![opening instances](./images_7/opening_of_instances.png)

`lsblk` checking blocks where disks are (nfs server)

![using lsblk to inspect blocks](./images_7/runing_lsblk_to_check_block.png)

`sudo gdisk /dev/xvdf`	using gdisk to partition first disk (nfs server)

![partitioning first disk](./images_7/using_gdisk_for_first_partition.png)

![continuation of first partition](./images_7/continuation_of_using_gdisk_for_first_partition.png)

`sudo gdisk /dev/xvdg` using gdisk to partition second disk (nfs server)

![partitioning second disk](./images_7/using_gdisk_for_second_partition.png)

![continuation of second partition](./images_7/continuation_of%20_using_gdisk_for_second_partition.png)

`sudo gdisk /dev/xvdh`	using gdisk to partition third disk (nfs server)

![partitioning third disk](./images_7/using_gdisk_for_third_partition.png)

`lsblk` (nfs server)

![checking partitioned disks](./images_7/running_lsblk_to_check_partitioned_disks.png)

` sudo yum install lvm2 -y` installing lvm2
![lvm2 install](./images_7/lvm2_installation.png)

![continuation of lvm2 install image](./images_7/continuation_of_lvm2_installation.png)

`sudo lvmdiskscan` (nfs server)

![lvm disk scan](./images_7/lvm_disk_scan.png)

`lsblk` (nfs server)

![inspecting block](./images_7/inspecting_block.png)

`sudo pvcreate /dev/xvdf1 /dev/xvdg1 /dev/xvdh1` creating physical volumes (nfs server)

![using pvcreate to create physical volume ](./images_7/using_pvcreate_to_create_physical_volume.png)

`sudo pvs` checking physical volumes created (nfs server)

![checking physical volumes created](./images_7/using_sudo_pvs.png)

`sudo vgcreate webdata-vg  /dev/xvdf1 /dev/xvdg1 /dev/xvdh1` creating a group for my physical volumes so it can be seen as one (cocantinate) (nfs server)

![creating a volume group](./images_7/creating_my_volume_group.png)

`sudo vgs`  confirming volume group created (nfs server)

![checking volume group created](./images_7/checking_the_volume_group_created.png)

`sudo lvcreate -n apps-lv -L 9G webdata-vg` creating logical volume for apps-lv (nfs server)

`sudo lvcreate -n logs-lv -L 9G webdata-vg` creating logical volume for logs-lv (nfs server)

`sudo lvcreate -n opt-lv -L 9G webdata-vg` creating logical volume for opt-lv (nfs server)

![creating logical volumes](./images_7/creating_logical_volumes_for_apps-lv_logs-lv_opt-lv.png)

`sudo lvs` (nfs server)

![checking logical volumes](./images_7/sudo_lvs.png)

`lsblk` checking logical volumes in block (nfs server)

![checking logical volumes in block](./images_7/checking_block_to_confirm_volume_groups_created.png)

`sudo vgdisplay -v #view complete setup - VG, PV, and LV` (nfs server)

![verifying the whole setup](./images_7/confirming_the_whole_setup.png)

![verifying the whole setup2](./images_7/confirming_the_whole_setup_2.png)

![verifying the whole setup3](./images_7/confirming_the_whole_setup_3.png)

`sudo mkdir /mnt/apps` (nfs server)

`sudo mkdir /mnt/logs` (nfs server)

`sudo mkdir /mnt/opt`  (nfs server)

![making directory](./images_7/making_directory.png)

`sudo mount /dev/webdata-vg/apps-lv /mnt/apps` (nfs server)

`sudo mount /dev/webdata-vg/logs-lv /mnt/logs` (nfs server)

`sudo mount /dev/webdata-vg/opt-lv /mnt/opt` (nfs server)

![mounting devices](./images_7/mounting_devicess.png)

`sudo yum -y update` (nfs server)

![installing update](./images_7/installing_update.png)

![installing update](./images_7/installing_update_continuation.png)

`sudo yum install nfs-utils -y` (nfs server)

![install nfs-utils](./images_7/installation_nfs-utils.png)

![install nfs-utils continuation](./images_7/nfs-utils_installation_configuration.png)

`sudo systemctl start nfs-server.service` (nfs server)

`sudo systemctl enable nfs-server.service` (nfs server)

`sudo systemctl status nfs-server.service` (nfs server)

![starting enabling status check nfs server](./images_7/starting_enabling_checking_status_of_nfs_server.png)

`sudo chown -R nobody: /mnt/apps` (nfs server)

`sudo chown -R nobody: /mnt/logs` (nfs server)

`sudo chown -R nobody: /mnt/opt` (nfs server)

`sudo chmod -R 777 /mnt/apps` (nfs server)

`sudo chmod -R 777 /mnt/logs` (nfs server)

`sudo chmod -R 777 /mnt/opt` (nfs server)

`sudo systemctl restart nfs-server.service` (nfs server)

![chown chmond restart service](./images_7/chown_chmond_restartin_service.png)

`sudo vi /etc/exports` (nfs server)

![configuring access to NFS for clients within the same subnet](./images_7/configuring_nfs_to_clients.png)

`sudo exportfs -arv` (nfs server)

![exportfs](./images_7/exportfs.png)

`rpcinfo -p | grep nfs` to check which port is used by nfs so it can be opened (nfs server)

![checking nfs ports](./images_7/checking_nfs_ports.png)

opening ports in EC2 instance

![opening ports](./images_7/opening_ports_2049_111.png)

`vi /etc/yum/pluginconf.d/product-id.conf` setting enabled=0 (db server)

`/etc/yum/pluginconf.d/subscription-manager.conf` setting enabled=0 (db server)

`sudo yum update` (db server)

`sudo yum install mysql-server -y` (db server)

`sudo systemctl start mysqld` (db server)

`sudo systemctl enable mysqld` (db server)

`sudo systemctl status mysqld` (db server)

![mysql status check](./images_7/mysql_status.png)

`sudo mysql` (db server)

`mysql> create database tooling;` (db server)

`mysql> create user 'webaccess'@'172.31.0.0/20' identified by 'password';` (db server)

`mysql> grant all privileges on tooling.* to 'webaccess'@'172.31.0.0/20';` (db server)

`mysql> flush privileges;` (db server)

`mysql> show databases;` (db server)

`mysql> use tooling;` (db server)

`mysql> exit` (db server)

![creating database 'tooling' and user 'webaccess'](./images_7/creating_database_tooling_user_webaccess.png)

`sudo yum install nfs-utils nfs4-acl-tools -y` (nfs server)

`sudo mkdir /var/www` (nfs server)

`sudo mount -t nfs -o rw,nosuid <NFS-Server-Private-IP-Address>:/mnt/apps /var/www` (nfs server)

![confirming mounted devices](./images_7/using_df%20-h_to%20_confirm_mounted_devices.png)

`sudo vi /etc/fstab` (nfs server)

![making sure the mount persist after reboot](./images_7/ensuring_mount_persists.png)

`sudo yum install httpd -y` (webserver 1)

`sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm`  (webserver 1)

`sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm`    (webserver 1)

`sudo dnf module reset php`(webserver 1)

`sudo dnf module enable php:remi-7.4`  (webserver 1)

`sudo dnf install php php-opcache php-gd php-curl php-mysqlnd`(webserver 1)

`sudo systemctl start php-fpm`  (webserver 1)

`sudo systemctl enable php-fpm` (webserver 1)

`setsebool -P httpd_execmem 1` (webserver 1)

repeat steps (installation) above for webserver 2, 3, 4

`sudo touch /var/www/test.md` (webserver 1)

`ls /mnt/apps` (nfs server)

![confiming if nfs was successfully mounted](./images_7/confirming_if_nfs_was_mounted_successfully1.png)

![confiming if nfs was successfully mounted](./images_7/confirming_if_nfs_mounted_successfully.png)

`sudo mount -t nfs -o rw,nosuid 172.31.12.105:/mnt/logs /var/log/httpd` (webservers 1,2,3)

`sudo vi /etc/fstab` (webservers 1,2,3)

`172.31.12.105:/mnt/apps /var/www nfs defaults 0 0` (webservers 1,2,3)

![making sure mount persists](./images_7/making_sure_changes_persists_after_reboot.png)

`sudo yum install git -y`   (webservers 1,2,3)

`git init` initializing an empty git repository for webservers 1,2,3

`git clone https://github.com/darey-io/tooling.git` (webservers 1,2,3)

`ls` webserver 1
![checking clone file](./images_7/checking_cloned_file_tooling_from_github_in%20_webserver.png)

`cd tooling/` (webservers 1,2,3)

`ls` checking files in tooling directory  (webservers 1,2,3)

![checking files in tooling directory](./images_7/checking_files_in_tooling_directory.png)


`ls /var/www`  (webservers 1,2,3)

![checking www directory](./images_7/checking_www_directory.png)

`sudo cp -R html/. /var/www/html` copying contents html in the tooling directory to /var/www  (webservers 1,2,3)

`ls /var/www/html`  (webservers 1,2,3)

`ls html`  (webservers 1,2,3)

![checking contents of /var/www/html](./images_7/checking_contents_of_var_www_html_after_copied.png)

open port 80 on my EC2 instance to connect from any where

`sudo systemctl status httpd` checking status of apache server

`cd ..`  (webservers 1,2,3)

`sudo setenforce 0`  (webservers 1,2,3)

`sudo vi /etc/sysconfig/selinux`  (webservers 1,2,3)

![making changes permanent](./images_7/making_changes_permanent.png)

`sudo systemctl start httpd`  (webservers 1,2,3)

`sudo systemctl status httpd`  (webservers 1,2,3)

![checking status of httpd](./images_7/checking_httpd_status.png)

`sudo vi /var/www/html/functions.php`

![updating functions.php file](./images_7/updating_functions.php_hile.png)

`sudo vi /etc/my.cnf` (database server)

![changing bind address](./images_7/changing_bind_address_database_server.png)

`cd tooling` (webservers 1,2,3)

`sudo yum mysql -y`  (webservers 1,2,3)

`mysql -h 172.31.12.13 -u webaccess -p tooling < tooling-db.sql`  (webservers 1,2,3)

![applying script to database](./images_7/applying_script_to_database.png)

`sudo mysql`

![opening_mysql](./images_7/opening_msql_in%20dbserver.png)

![mysql](./images_7/opening_mysql_in_dbserver.png)

`sudo vi tooling-db.sql` in tooling directory (webservers 1,2,3)

![checking_credentials](./images_7/checking_values.png)

`sudo mysql`   (db server)

`create user 'myuser'@'%' identified by 'password';` (db server)

` grant all privileges on tooling.* to 'myuser'@'%';` (db server)

`show databases;` (db server)

`use users;` (db server)

` create table users(` (db server)

`ID VARCHAR(60) NOT NULL,` (db server)

` USERNAME VARCHAR(60) NOT NULL,` (db server)

` PASSWORD VARCHAR(60) NOT NULL,` (db server)

`EMAIL VARCHAR(60) NOT NULL,` (db server)

`USER_TYPE VARCHAR(60) NOT NULL,` (db server)

`STATUS VARCHAR(60) NOT NULL,` (db server)

`PRIMARY KEY(ID));` (db server)

` INSERT INTO users VALUE ("1", "myuser", "5f4dcc3b5aa765d61d8327deb882cf99", "user@mail.com", "admin", "1");` (db server)

![creating myuser in mysql](./images_7/setting_new_values_for_database.png)

![creating myuser in mysql](./images_7/setting_new_values_for_database_.png)

![creating myuser in mysql](./images_7/setting_new_values_for_database___.png)

[logging in website with my user](http://18.116.27.32/index.php)


![website on browser](./images_7/propitix_tooling_website.png)