***此 Demo 需要至少兩台EC2主機(RHEL8)***

>主機1: Ansible Control Server(控制端)

>主機2: Ansible Controlled    (被控端)

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
4. Ansible 端點之間通常使用免密碼登入，以便在自動化佈署時需要手動一台一台輸入密碼，所以使用指令在 **控制端** 生成 ssh keygen。
```
ssh-keygen (出現選項時，使用預設(Enter)即可)
```
5. 將生成的檔案匯入至被控端，**但 EC2 主機是使用金鑰連線，所以並不知道預設(ec2-user)以及 root 的密碼**
>- 所以需要先登陸至**被控端(root)** ，並增加一名使用者。(此處使用 ansuser)
```
# 新增使用者
useradd ansuser
# 修改使用者密碼
passwd ansuser
```
>- 將該名使用者加入 sudoers 名單中。
```
echo "ansuser ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
```
>- 並將預設關閉的「密碼登入」開啟，並重啟 sshd 服務。
```
sed -ie 's/PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config

service sshd reload
```
* 再回到**控制端**，將生成的檔案匯入**被控端**
```
# 控制端與被控端在互為不同使用者名稱的情況下須指定被控端的使用者名稱
# Ex: 控制端使用 root 控制 被控端 ansuser
ssh-copy-id username@ip_address

# 如果在使用者名稱相同的情況下，可直接省略使用者名稱輸入IP即可
ssh-copy-id ip_address
```
6. 驗證測試
- 新增被控端 IP 至 `/etc/ansible/hosts` 內
```
IP_ADDRESS ansible_ssh_user=USERNAME ansible_ssh_pass=PASSWORD ansible_ssh_port=22
```
- 同時也可自訂群組名稱，以利辨識及管理。例:
```
[test_servers] #群組名稱(test_servers)
IP_ADDRESS ansible_ssh_user=USERNAME ansible_ssh_pass=PASSWORD ansible_ssh_port=22
...

[web_servers] #群組名稱(web_servers)
IP_ADDRESS ansible_ssh_user=USERNAME ansible_ssh_pass=PASSWORD ansible_ssh_port=22
...
```

- 並輸入指令以測試控制端是否可 Ping 通被控端，成功時會出現綠色「SUCCESS」字樣。
```
ansible all -m ping
```
- 也可使用其他指令去驗證，例:
```
# 檢查 test_servers 群組內所有主機的內核版本
ansible all -m command -a "uname -r"
```
```
# 僅列出 web_servers 群組內的主機
ansible web_servers -i /etc/ansible/hosts --list-hosts
```
- 傳送檔案至 test_servers 群組內的主機
>- 控制端內新增一個名為 TEST_COPY 的檔案 `vim TEST_COPY`
```
This is for ansible copy test!!!
```
>- 使用指令傳送:
```
ansible test_servers -m copy -a "src=TEST_COPY dest=/home/ansuser/"
```
