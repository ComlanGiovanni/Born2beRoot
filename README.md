# Born2beRoot


#Mot de passe
```
Born2beRoot
root pass : Gcomlan#42Born
user pass : Born2BeRoute#42
Passe : PassePartout#42
```

# Comming soon
```
Install
Partitionn Guide screen
```
# Link for iso
```
https://www.debian.org/CD/netinst/
```
# Switch to root.
```
su -
```
# Upadate os
```
apt-get update -y
apt-get upgrade -y
```
# Installing git
```
apt-get install git -y
```

# Insatll wget
```
sudo apt-get install wget
```
# Installing Oh my zsh
```
sudo apt-get install zsh
zsh --version
sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```
# Insatall Vim
```
apt-get install vim
```
# Open file login.def using vim
```
vim /etc/login.defs
```

# Password expiration every 30 days ,line 160.
```
PASS_MAX_DAYS   30
```
# Password changes to 2 days, line 161t.
```
PASS_MIN_DAYS   2
```
warning message 7 days before their password expires,line 162
```
PASS_WARN_AGE   7
```
Password strength, install the libpam-pwquality package.
```
apt install libpam-pwquality
```
Open **common-password**.
```
vim /etc/pam.d/common-password
```
Line 25.
```
password    requisite   pam_pwquality.so retry=3 minlen=10 ucredit=-1 dcredit=-1 maxrepeat=3 reject_username enforce_for_root difok=7
```
**minlen** = minimum password length

**ucredit** = maximum number of uppercase characters

**dcredit** = maximum number of digits

**maxrepeat** = the maximum number of times a single character may be repeated

**reject_username** = rejects a password if it consists of the username either in its normal way reverse.

**enforce_for_root** = password policies are adhered to even if it’s the root user configuring the passwords.

**difok** = the minimum number of characters that must be different from the old password.
Configuring sudo

# Apply politique on root and 42login
```
sudo chage -M 30 intra_login42
sudo chage -m 2 intra_login42
sudo chage -W 7 intra_login42

sudo chage -M 30 root
sudo chage -m 2 root
sudo chage -W 7 root
```
 #### 3 : Configuring sudo.

```
apt install sudo
```
Open file **sudoers** using vim.
```
vim /etc/sudoers
```
```
Defaults     passwd_tries=3
```
- A custom message of error .
line 13.
```
Defaults     badpass_message="Do you think like you type ?"
```
- both inputs and outputs saved in the /var/log/sudo/ folder.
First, create a file, name it as you like with the extension .log
```
touch /var/log/sudo/arv.log
```
Then
```
vim /etc/sudoers
```
go to line 14 and add the line below.
```
Defaults	logfile="/var/log/sudo/arv.log"
Defaults	log_input,log_output
```
- The TTY mode has to be enabled for security reasons.
go to line 16 and add the line below.
```
Defaults        requiretty
```
- For security reasons too, the paths that can be used by sudo must be restricted.
go to line 11 and modify it as in the example below.
```
Defaults   secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
```
- In addition to the root user, a user with your login as username has to be present.
```
visudo
```
add the line below.
```
user_name ALL=(ALL) ALL
```
- This user has to belong to the user42 and sudo groups.
```
groupadd user42
```
add user to the user42 and sudo groups.
```
usermod -aG sudo user_name
```
```
usermod -aG user42 user_name
```
Finally, you have to create a simple script called monitoring.sh. It must be developed in bash.
```

cd /user/local/bin/

git clone https://gist.github.com/5ff6404e27520c2ac01a662debff2329.git

mv to monitoring.sh

chmod +x /usr/local/bin/monitoring.sh
```
To run script every 10 minutes
```
crontab -u root -e
```
Add at end as follows: (*/10 means every 10 mins the script will show)
```
*/10 * * * * /usr/local/bin/monitoring.sh
```
- SSH & UFW
A SSH service will be running on port 4242 only. For security reasons, it must not be
possible to connect using SSH as root
```
apt install openssh-server
```
Changing default port (22) to 4242
```
vim /etc/ssh/sshd_config
```
Edit the file, change the line #Port22 to Port 4242.
Go to line 15 and edit it.
```
Port 4242
```
It must not be possible to connect using SSH as root.
change line 34.
```
PermitRootLogin no
```
Restart the SSH service.
```
service ssh restart
```
Install UFW (Uncomplicated Firewall)
```
apt-get install ufw
```
```
ufw enable
```
Configure the rules
```
ufw allow ssh
```
```
ufw allow 4242
```
Deleting UFW Rules By rule number.
```
ufw status numbered
```
Delete the rules number 1 and 2.
```
ufw delete 1
```
```
ufw delete 2
```
- Connecting SSH server.

--------------------------------------------------------------------------------

# You may not be able to connect to your VM via SSH with standard settings in
VirtualBox. Theres a way to wix it!

```
1) Turn off your VM ([sudo shutdown])
2) Go to your VM settings in VirtualBox
3) Network -> Adapter 1 -> Advanced -> Port forwarding
4)Add new rule (little green button on right top side) and next parameters:
```
```
**************************************************************************
* Protocol       Host IP       Host Port       Guest IP       Guest Port *
* TCP            127.0.0.1     4242            10.0.2.15      4242       *
**************************************************************************
```
```
6) In your host (physical) machine open Terminal and run
[ssh <vmusername>@localhost -p 4242]

Now you can control your virtual machine from the host terminal.
```

#

#### Command For Defense

```
su -
```

```
sudo ufw status
```

```
sudo service sshd status
```

```
hostnamectl
```

```
head -n 2 /etc/os.relase
/usr/sbin/aa-status
```

```
getent group sudo
```

```
getent group user42
```

```
cut -d: -f1 /etc/passwd
```

```
sudo adduser killua
```

```
sudo groupadd evaluating
```

```
sudo usermod -aG evaluating killa
```

```
getent group evaluating
```

```
groups
```

```
chage -l killua
```


```
sed -n 22,27p /etc/pam.d/common-password
```


```
hostnamectl set-hostname
```

```
sudo nano /etc/hosts
```

```
sudo reboot
```

```
lsblk
```

```
usermod -aG root killua
```

```
cat /etc/sudoers
cat /var/log/sudo/arv.log
```

```
ufw status
ufw allow 8080
ufw status numbered
ufw status delete 2
ufw status delete 3

or

sudo ufw deny 8080
```

```
cat /etc/ssh/sshd_config
service sshd status
ssh gcomlan@localhost -p 4242
ssh killua@localhost -p 4242
ssh root@localhost -p 4242
```

```
cat /usr/local/bin/monitoring.sh
sudo crontab -u root -e

*/1 * * * * sleep 30s && /path/to/monitoring.sh
```

```

```

#

# Signature
```
• Pour Windows : %HOMEDRIVE%%HOMEPATH%\VirtualBox VMs\
• Pour Linux : ~/VirtualBox VMs/
• Pour Mac M1 : ~/Library/Containers/com.utmapp.UTM/Data/Documents/
• Pour MacOS : ~/VirtualBox VMs/
```

``
Pour Windows : certUtil -hashfile centos_serv.vdi sha1
• Pour Linux : sha1sum centos_serv.vdi
• Pour Mac M1 : shasum Centos.utm/Images/disk-0.qcow2
• Pour MacOS : shasum centos_serv.vdi
```

# Link
https://unix.stackexchange.com/questions/502540/why-does-drmvmw-host-log-vmwgfx-error-failed-to-send-host-log-message-sh

https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=635473

https://bugs.debian.org/cgi-bin/bugreport.cgi?att=0;bug=635473;msg=70