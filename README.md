# WireGuard-setup-with-Ansible
### WireGuard automated installation using Ansible playbook. ###

For OS Red Hat 7 or CentOS 7 platfrom-python has been already installed with OS. Command to check its status:
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
Create wireguard folder and change to it:
```bash
mkdir wireguard
cd wireguard/
```
Run playbook wireguard_setup.yml to setup WireGuard and create config for the first client:
```bash
ansible-playbook wireguard_setup.yml
```
To connect the first VPN client use QR code printed on the screen. QR code additionally saved to wg-client1.utf8 can be read using cat or vim command.  
wg-client1.conf used for QR code generation also saved to the current directory. You can use this configuration for VPN client setup if QR is not suitable:

![image](https://github.com/RuslanSalyakhov/WireGuard-setup-with-Ansible/assets/45723128/51b86c34-94fc-40e2-a1a4-656db8047a5a)

### Add new VPN client. ###
Run playbook add_new_client.yml to add a new client to the VPN server and generate connection config:
```bash
ansible-playbook add_new_client.yml
```
Playbook will use the subsequent numbers 2,3,4... for conf file naming and assign next IP address (10.0.0.3, 10.0.0.4...) to the vpn client.<br />To connect VPN client use QR code printed on the screen. QR code additionally saved to wg-client#.utf8 and can be read using cat or vim command.  
wg-client#.conf used for QR code generation also saved to the current directory. You can use this configuration for VPN client setup if QR is not suitable:
![image](https://github.com/RuslanSalyakhov/WireGuard-setup-with-Ansible/assets/45723128/f6d60a5f-f17a-4274-a13c-13aa4a387145)


