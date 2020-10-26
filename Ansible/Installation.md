- 「Ansible Server」 加入「監控主機」 `/etc/ansible/hosts`
```
192.168.8.50 ansible_ssh_user=test ansible_ssh_pass=test ansible_ssh_port=22
192.168.8.51 ansible_ssh_user=test ansible_ssh_pass=test ansible_ssh_port=22
```
- ssh-copy-id: Permission Denied
>至Host端
```
sed -ie 's/PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config
sudo service sshd reload
```
