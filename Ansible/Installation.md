> 此 Demo 需要至少兩台EC2主機(RHEL8)

  主機1: Ansible Control Server(控制端)
  主機2: Ansible Controlled    (被控端)

*控制端(root)*
1. 因為 Ansible 不在 RHEL8 預設軟體庫內，所以需要安裝 epel-release 增加軟體源。
```
dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm -y
```
2. 安裝 Ansible。
```
dnf install ansible
```
3. 使用指令確認 Ansible 版本
```
ansible --version
```
4. Ansible 端點之間通常使用免密碼登入，以便在自動化佈署時需要手動一台一台輸入密碼，所以使用指令生成 ssh 連線的 keygen。
```
ssh-keygen (出現選項時，使用預設(Enter)即可)
```
5. 將生成的檔案匯入至被控端
```
ssh-copy-id username@ip_address
```
**但 EC2 主機是使用金鑰連線，所以並不知道預設(ec2-user)以及 root 的密碼**
>- 登陸至被控端(root)，並增加一名使用者。(此處使用 ansuser)
`useradd ansuser`
>- 將該名使用者加入 sudoers 名單中。
`echo "ansuser ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers`
>- 並且把預設關閉的「密碼登入」開啟
```
sed -ie 's/PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config
sudo service sshd reload
```
