1. Ansible is not in RHEL8 default repo, so we need to install the epel-release.
```
dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm -y
```
2. Install Ansible.
```
dnf install ansible
```
