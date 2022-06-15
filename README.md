# Born2beRoot
Born2beRoot
root pass : Gcomlan#42Born
user pass : Born2BeRoute#42
Passe ; PassePartout#42


# Comming soon
```
Install
Partitionn Guide screen
```
# Link for iso
```
*Small Netinstaller - https://www.debian.org/CD/netinst/ |<-amd64
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

# For password expiration every 30 days, go to line 160 and edit it.
```
PASS_MAX_DAYS   30
```
# To set minimum number of days between password changes to 2 days, go to line 161 and edit it.
```
PASS_MIN_DAYS   2
```
For the user to receive a warning message 7 days before their password expires, go to line 162 and edit it.
```
PASS_WARN_AGE   7
```
To set up policies in relation to password strength, install the libpam-pwquality package.
```
apt install libpam-pwquality
```
Open file **common-password** using vim.
```
vim /etc/pam.d/common-password
```
go to line 25 and modify it as in the example below.
```
password        requisite                       pam_pwquality.so retry=3 minlen=10 ucredit=-1 dcredit=-1 maxrepeat=3 reject_username enforce_for_root difok=7
```
**minlen** = minimum password length.

**ucredit** = maximum number of uppercase characters that will generate a credit.

**dcredit** = maximum number of digits that will generate a credit.

**maxrepeat** = the maximum number of times a single character may be repeated.

**reject_username** = The option rejects a password if it consists of the username either in its normal way or in reverse.

**enforce_for_root** = This ensures that the password policies are adhered to even if it’s the root user configuring the passwords.

**difok** = the minimum number of characters that must be different from the old password.
Configuring sudo
- After setting up your configuration files, you will have to change
all the passwords of the accounts present on the virtual machine,
including the root account.

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
 To set up a strong configuration for your sudo group, you have to comply with the
following requirements:
```
apt install sudo
```
Open file **sudoers** using vim.
```
vim /etc/sudoers
```
- Authentication using sudo has to be limited to 3 attempts in the event of an incorrect password.
go to line 12 and add the line below.
```
Defaults     passwd_tries=3
```
- A custom message of your choice has to be displayed if an error due to a wrong
password occurs when using sudo.
go to line 13 and add the line below.
```
Defaults     badpass_message="Do you think like you type ?"
```
- Each action using sudo has to be archived, both inputs and outputs. The log file
has to be saved in the /var/log/sudo/ folder.
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
At server startup, the script will display some information (listed below) on all terminals every 10 minutes (take a look at wall). The banner is optional. No error must
be visible.
Your script must always be able to display the following information:

• The architecture of your operating system and its kernel version.

• The number of physical processors.

• The number of virtual processors.

• The current available RAM on your server and its utilization rate as a percentage.

• The current available memory on your server and its utilization rate as a percentage.

• The current utilization rate of your processors as a percentage.

• The date and time of the last reboot.

• Whether LVM is active or not.

• The number of active connections.

• The number of users using the server.

• The IPv4 address of your server and its MAC (Media Access Control) address.

• The number of commands executed with the sudo program.

#
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
![This is an image](https://blogger.googleusercontent.com/img/a/AVvXsEh-wXgFfnGxCw5CfzsvusVR0vmUAyhzvYcV4xYGW1w2m667uYhDCRzg-ntYwtoovGdYB7KQyYKXfk-WA8Cj-qXoVXHlvb5rO1K9XjFCCQqpX5ncMpP2fyIr2IbmbmLtybLU48kcujcICCyifWD8b-h58MNUvwQR49hNwkOeaof9SRjYrqHc3gLkbYEIeg)
delete the rules number 1 and 2.
```
ufw delete 1
```
```
ufw delete 2
```
- Connecting SSH server.
![This is an image](https://blogger.googleusercontent.com/img/a/AVvXsEgRRTFZwRRR5jwgqLUpVHf1XPBmB0GPXzMZYdddmsCgAkgE5yIGQEuZDDSZCok8RBy3nF4Pb_0Og-rc2mdaT5dt7x1T-HsG6bavyxdqgC3PZ6lk2VIWWw2bo9nuiuJHehp73OgtjFNhFH66I1UBY3yH35kC4lzKHBppI6JwhkFjVR3mAh7SpKq5l3Og0A)


--------------------------------------------------------------------------------

You may not be able to connect to your VM via SSH with standard settings in
VirtualBox. Theres a way to wix it!

1) Turn off your VM ([sudo shutdown])
2) Go to your VM settings in VirtualBox
3) Network -> Adapter 1 -> Advanced -> Port forwarding
4)Add new rule (little green button on right top side) and next parameters:

**************************************************************************
* Protocol       Host IP       Host Port       Guest IP       Guest Port *
* TCP            127.0.0.1     4242            10.0.2.15      4242       *
**************************************************************************
6) In your host (physical) machine open Terminal and run
[ssh <vmusername>@localhost -p 4242]

Now you can control your virtual machine from the host terminal.




#### For Defense

```

```
 ```

```
 ```

```
#

#

#




# Link
https://unix.stackexchange.com/questions/502540/why-does-drmvmw-host-log-vmwgfx-error-failed-to-send-host-log-message-sh

https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=635473

https://bugs.debian.org/cgi-bin/bugreport.cgi?att=0;bug=635473;msg=70