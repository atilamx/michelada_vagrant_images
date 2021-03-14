
# Overview

Vagrant images to run several security tests 

### Images included
freebsd-12-2
kali-3

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
```bash
cd firewall_tests && vagrant up 
vagrant ssh freebsd
vagrant ssh kali
```

### Stop kali & FreeBSD 
```bash
cd firewall_tests && vagrant halt
```

