*Switch to root*
1. Ansible is not in RHEL8 default repo, so we need to install the epel-release.
```
sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm -y
```
2. Install Ansible.
```
sudo dnf install ansible
```
3. Check the Ansible version.
```
sudo ansible --version
```
