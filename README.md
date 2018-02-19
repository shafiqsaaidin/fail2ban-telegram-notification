# fail2ban-telegram-notification
Send notification to telegram when fail2ban ban an IP address and unband an IP address

### Requirement
- openssh
- vsftpd 
- fail2ban
- curl
- telegram bot api

### Installation
`$ sudo apt install vsftpd fail2ban ssh-server`

### Configuration
#### Fail2ban
- Create a copy of jail.conf `cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local`.
- Create ban rules in jail.local <br>
   ignoreip = 127.0.0.1/8 192.168.1.101   
   bantime = 3600  
   findtime = 120   
   maxretry = 3  
  
- Protect vsftpd with fail2ban. Make chage to [vsftpd] section in jail.local
   enabled = true  
   filter  = vsftpd  
   action  =  
   iptables[name=VSFTPD, port=21, protocol=tcp]  
   telegram
   
 - If you want to protect SSH with fail2ban add this to [sshd]  
   enabled = true  
   filter  = sshd   
   maxretry = 3  
   logpath = /var/log/auth.log  
   action  =  
   iptables[name=SSH, port=22, protocol=tcp]  
   telegram 
   
 - Make script directory to place our shell script  
 `sudo mkdir /etc/fail2ban/scripts/`  
 in the following directory add `fail2ban-telegram.sh`  
 
 - Copy telegram.conf to `/etc/fail2ban/action.d/` directory  
 `cp telegram.conf /etc/fail2ban/action.d/`
 
 - Edit fail2ban-telegram.sh and replace the `apiToken` and `chatId` with your api. You must create telegram bot first and get the api key [here](https://www.sohamkamani.com/blog/2016/09/21/making-a-telegram-bot/)
 
### Start the service  
systemctl start ssh-server  
systemctl start vsftpd  
systemctl start fail2ban

### Demo video
[![Fail2ban with telegram notification](http://img.youtube.com/vi/LYLiCK5XGXk/0.jpg)](https://youtu.be/LYLiCK5XGXk)
