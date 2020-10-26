*Switch to root or maybe you can add `sudo` to each command*
1. Ansible is not in RHEL8 default repo, so we need to install the epel-release.
```
dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm -y
```
2. Install Ansible.
```
dnf install ansible
```
3. Check the Ansible version.
```
ansible --version
```
