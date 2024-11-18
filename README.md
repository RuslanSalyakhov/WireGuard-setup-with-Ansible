# WireGuard setup with Ansible on CentOS/RedHat 7 

## WireGuard automated installation using Ansible playbook. ##

Update all installed packages :
```bash
yum update -y
```

For OS RedHat 8 or CentOS 8 platfrom-python has been already installed with OS. Command to check its status:
```bash
yum list installed platform-python
```
Switch to root user if your current user included in the group from /etc/sudoers file or switch to root using **su -** command with root password : 
```bash
sudo -i
```
OR
```bash
su -
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
Create wireguard folder and change to it:
```bash
mkdir wireguard
cd wireguard/
```
Install git and copy Ansible files from GitHub repository:
```bash
yum -y install git
git clone https://github.com/RuslanSalyakhov/WireGuard-setup-with-Ansible.git ~/wireguard
```
Run playbook wireguard_setup.yml to setup WireGuard and create config for the first client:
```bash
ansible-playbook wireguard_setup.yml
```
OR if you want to skip updating all installed packages task:
```bash
ansible-playbook wireguard_setup.yml --skip-tags updates
```
To connect the first VPN client use QR code printed on the screen. QR code additionally saved to wg-client1.utf8 can be read using cat or vim command.  
wg-client1.conf used for QR code generation also saved to the current directory. You can use this configuration for VPN client setup if QR is not suitable:

![playbook_1](https://github.com/RuslanSalyakhov/WireGuard-setup-with-Ansible/assets/45723128/caf01cc3-5e18-4308-b839-73f26db70bc8)

### Add new VPN client. ###
Run playbook add_new_client.yml to add a new client to the VPN server and generate connection config:
```bash
ansible-playbook add_new_client.yml
```
Playbook will use the subsequent numbers 2,3,4... for conf file naming and assign next IP address (10.0.0.3, 10.0.0.4...) to the vpn client.<br />To connect VPN client use QR code printed on the screen. QR code additionally saved to wg-client#.utf8 and can be read using cat or vim command.  
wg-client#.conf used for QR code generation also saved to the current directory. You can use this configuration for VPN client setup if QR is not suitable:
![playbook_2](https://github.com/RuslanSalyakhov/WireGuard-setup-with-Ansible/assets/45723128/b3a3b376-beab-41c8-9680-daa6ef642229)

## Post Installation activities to secure server access via ssh  ##
### Create wireguard user and ssh keys for authentication. ###
Create wireguard user on the WireGuard server and adding this user to the wheel group to get access to sudo:
```bash
useradd wireguard
usermod -aG wheel wireguard
```
Set wireguard user password:
```bash
echo my_new_password | passwd --stdin wireguard
```

Generate ssh keys on a **Remote machine**:
```bash
ssh-keygen -t rsa
```
Copy generated public key from **Remote machine** to the WireGurard server. To authenticate you need to enter wireguard user password:
```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub wireguard@<WIREGUARD_SERVER_IP>
```
Connect to WireGuard server via ssh using public key copied earlier for authentication:
```bash
ssh wireguard@<WIREGUARD_SERVER_IP>
```
### Secure WireGuard server access via ssh. ###
Edit /etc/ssh/sshd_config disabling password authentication and denying root user authentication to improve ssh security:
```bash
sudo vi /etc/ssh/sshd_config
```
Find PasswordAuthentication and set it to no:
```bash
PasswordAuthentication no
```
Find PermitRootLogin and set it to no:
```bash
PermitRootLogin no
```
Find ChallengeResponseAuthentication and check that it set to no:
```bash
ChallengeResponseAuthentication no
```
Find UsePAM and set it to no:
```bash
UsePAM no
```
Reload sshd service to apply configuration changes:
```bash
sudo systemctl reload sshd
```

### Two way ping disabling. ###
Turn off the ICMP requests:
```bash
sudo firewall-cmd --permanent --add-icmp-block=echo-request
```
Apply updated config:
```bash
sudo firewall-cmd --reload
```
