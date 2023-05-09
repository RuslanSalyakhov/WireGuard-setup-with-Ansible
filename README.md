# WireGuard-setup-with-Ansible
WireGuard automated installation using Ansible playbook. 

For OS Red Hat 8 or CentOS 8 platfrom-python has been already installed with OS. Command to check its status:
```bash
yum list installed platform-python
```
Switch to root user: 
```bash
sudo -i
```
If the platform-python has not been installed earlier install python36 package:
```bash
yum install python36    
```
Install EPEL repository which has Ansible package:
```bash
yum -y install epel-release
```
Install Ansible package:
```bash
yum -y install ansible
```
```bash
1. mkdir wireguard
2. cd wireguard/
```
