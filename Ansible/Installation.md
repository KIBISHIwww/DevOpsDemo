- 「Ansible Server」 加入「監控主機」 `/etc/ansible/hosts`
```
192.168.8.50 ansible_ssh_user=test ansible_ssh_pass=test ansible_ssh_port=22
192.168.8.51 ansible_ssh_user=test ansible_ssh_pass=test ansible_ssh_port=22
```
- ssh-copy-id: Permission Denied
>To host
1. Add the ansuser to sudoers file. 
```echo "ansadmin ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers```
2. Turn PasswordAuthentication to yes
```
sed -ie 's/PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config
sudo service sshd reload
```
