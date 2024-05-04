# วิธีใช้งาน Vagrant เบื้องต้น
การสร้าง VM ( virtual machine ) เพื่อทดสอบเป็นแต่ละโปรเจคไปโดยไม่ต้องไปลง OS อื่นๆ 

## ขั้นตอนการสร้าง Vagrant 
### 1. Download Programs
- [vagrant program](https://www.vagrantup.com/)
- [virtualbox](https://www.virtualbox.org/wiki/Download_Old_Builds_6_1) (ใช้เป็นโปรแกรมสำหรับเอาไว้รัน VM)
### 2. Create Vagrant File
```bash
vagrant init ubuntu/bionic64
```
### 3. Configuring  (for python )
``` bash
# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
 # The most common configuration options are documented and commented below.
 # For a complete reference, please see the online documentation at
 # https://docs.vagrantup.com.

 # Every Vagrant development environment requires a box. You can search for
 # boxes at https://vagrantcloud.com/search.
 config.vm.box = "ubuntu/bionic64"
 config.vm.box_version = "~> 20200304.0.0"

 config.vm.network "forwarded_port", guest: 8000, host: 8000

 config.vm.provision "shell", inline: <<-SHELL
   systemctl disable apt-daily.service
   systemctl disable apt-daily.timer
 
   sudo apt-get update
   sudo apt-get install -y python3-venv zip
   touch /home/vagrant/.bash_aliases
   if ! grep -q PYTHON_ALIAS_ADDED /home/vagrant/.bash_aliases; then
     echo "# PYTHON_ALIAS_ADDED" >> /home/vagrant/.bash_aliases
     echo "alias python='python3'" >> /home/vagrant/.bash_aliases
   fi
 SHELL
end
```
### 4. running and connecting 
- download image that we've specified in our vagrant file
```bash
vagrant up
```
- Connect Machine
```bash
vagrant ssh
```
- Disconnect Machine
```bash
exit
```